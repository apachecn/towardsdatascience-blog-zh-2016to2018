# PySpark 中的大规模连接元件

> 原文：<https://towardsdatascience.com/connected-components-at-scale-in-pyspark-4a1c6423b9ed?source=collection_archive---------6----------------------->

![](img/42fd62988caabf433cee26cc0c3e0181.png)

不久前，我有一个节点网络，我需要为它计算[个连通分量](https://en.wikipedia.org/wiki/Connected_component_(graph_theory))。这并不是一件特别困难的事情。Python [networkx](https://networkx.github.io/) 库有一个很好的实现，使它变得特别容易，但是即使你想推出自己的函数，这也是一个简单的[广度优先搜索](https://en.wikipedia.org/wiki/Breadth-first_search)。(如果你想要一个温和的介绍，可汗学院给出了一个很好的概述。从概念上讲，它只涉及一组队列。您选择一个节点，找到连接到该节点的所有节点，找到连接到这些节点的所有节点，依此类推。每当您找到一个节点的连接时，您就将该节点移动到一个“完成”列表中。当您找不到任何不在“完成”列表中的连接时，那么您已经找到了您的连接组件。然后，对图中的所有剩余节点重复该过程。

虽然广度优先搜索在概念上很简单，但要大规模实现却是一件难事。这是因为像 Spark 这样的“大数据”系统区分了转化和行动。转型是一个计划——一组预期的行动。您可以构建一个很长的转换列表，并在最后调用一个操作，比如收集最终结果的操作。这就是使用大型数据集的成本:你可以存储比你个人电脑多得多的信息，但访问这些信息的计算量要比你只是从本地机器的内存中提取信息大得多。这就是为什么大规模计算连通分量是复杂的:广度优先搜索根据以前转换的结果来确定未来的转换，这意味着您必须一直调用操作。这可能是一个真正的资源负担。当我需要这样做的时候，我正在处理一个有几千万个节点和上亿条边的图。

Spark 的 [GraphX](https://spark.apache.org/graphx/) 库有一个 connected components 函数，但当时我正在寻找这样做的方法，我的整个工作流程是 Python，而 GraphX 只在 Scala 中实现。有一个 [GraphFrames](https://github.com/graphframes/graphframes) 项目为 GraphX 提供了一个 Python 包装器，在某些情况下还提供了一个替代实现，但据我所知，该项目并没有太频繁地更新。我担心我的项目依赖于一个大而复杂的依赖项，而这个依赖项并没有得到持续的支持。此外，我并不需要那个庞大复杂的依赖关系中的所有东西。我只需要一种计算连通分量的方法。

这让我开始在互联网上搜索，给了我一些想法，坦率地说，这些想法有点超出了我翻译成工作代码的能力(这是我的错，不是他们的——例如:[这里](https://www.linkedin.com/pulse/connected-component-using-map-reduce-apache-spark-shirish-kumar/)，这里[这里](https://github.com/kwartile/connected-component)，这里[这里](https://dl.acm.org/citation.cfm?doid=2670979.2670997)。)但是查看这些实现至少给了我一个真正有用的视角:对于广度优先搜索，你不需要队列；你只需要知道两个节点之间的关系是否已经被探索过。排队是一种简单的方法，但不是唯一的方法。

所以你可以这样做:给你的每个节点分配一个数字散列。假设一个无向图(其中，如果节点 a 连接到节点 b，那么节点 b 也连接到节点 a)，您可以创建一个邻接表(两列，每一列都充满了节点散列，其中每一行表示一条边)，然后按左侧列的散列分组，并取右侧列中散列的最小值。将这些重新加入到你的邻接表中，并替换新的、较低的左列散列。然后不断重复，直到一切都不再改变。

下面，您可以看到一个使用 PySpark 的概念验证。这要求您通过将数据帧保存到文件来检查数据帧。通常，我发现将数据帧保存(缓存)在内存中更好，因为这样我就不必记得事后清理了，但是如果缓存次数太多，Spark 就会花费太多时间来跟踪转换的整个过程，以至于根本没有时间调用操作。在我的用例中，我发现每 3-4 次迭代进行一次检查点检查，并在所有其他迭代中坚持下去，会产生可接受的性能。

```
**import** pyspark.sql.functions as f
**import** subprocess**def** pyspark_connected_components(
    ss, adj, a, b, label, checkpoint_dir, 
    checkpoint_every**=**2, max_n**=**None):
    *"""
    This function takes an adjacency list stored in a Spark 
    data frame and calculates connected components. This 
    implementation only deals with use cases: assuming an 
    undirected graph (if a is connected to b then b is
    connected to a).* *Args:
    ss (pyspark.sql.session.SparkSession): an active SparkSession
    adj (pyspark.sql.DataFrame): A data frame with at least two
        columns, where each entry is a node of a graph and each row
        represents an edge connecting two nodes.
    a (str): the column name indicating one of the node pairs in the
        adjacency list. The column must be numeric.
    b (str): the column name indicating the other of the node pairs
        in the adjacency list. The column must be numeric.
    label (str): a label for the output column of nodes
    checkpoint_dir (str): a location on HDFS in which to save 
        checkpoints
    checkpoint every (int): how many iterations to wait before
        checkpointing the dataframe
    max_n (int): the maximum number of iterations to run* *Returns:
    A PySpark DataFrame with one column (named by the label 
        argument) listing all nodes, and another columns 
        ("component") listing component labels.
    """* *# set up a few strings for use in the code further down*
    b2 **=** b **+** 'join'
    filepattern **=** 'pyspark_cc_iter{}.parquet' *# cache the adjacency matrix because it will be used many times*
    adj_cache **=** adj.persist() *# for each id in the `a` column, take the minimum of the 
    # ids in the `b` column*
    mins **=** (
        adj_cache
        .groupby(a)
        .agg(f.min(f.col(b)).alias('min1'))
        .withColumn('change', f.lit(0))
        .persist()
    ) **if** max_n **is** **not** None:
        *# ensure a global minimum id that is less than 
        # any ids in the dataset*
        minimum_id **=** mins.select(
            f.min(f.col('min1')).alias('n')
        ).collect()[0]['n'] **-** 1000000 *# transformation used repeatedly to replace old labels with new*
    fill_in **=** f.coalesce(f.col('min2'), f.col('min1')) *# join conditions for comparing nodes*
    criteria **=** [
        f.col(b) **==** f.col(b2), f.col('min1') > f.col('min3')
    ] *# initial conditions for the loop*
    not_done **=** mins.count()
    old_done **=** not_done **+** 1
    new_done **=** not_done
    niter **=** 0
    used_filepaths **=** []
    filepath **=** None *# repeat until done*
    **while** not_done > 0:
        niter **+=** 1

        **if** max_n **is** **not** None:
            min_a **=** mins.filter(f.col('min1') !**=** minimum_id)
        **else**:
            min_a **=** mins *# get new minimum ids*
            newmins **=** (
                adj_cache
                .join(min_a, a, 'inner')
                .join(
                    mins.select(
                        f.col(a).alias(b2), 
                        f.col('min1').alias('min3')
                    ), 
                    criteria, 
                    'inner'
                )
                .groupby(f.col('min1').alias('min1'))
                .agg(f.min(f.col('min3')).alias('min2'))
            ) *# reconcile new minimum ids with the 
        # minimum ids from the previous iteration*
        mins **=** (
            mins
            .join(newmins, 'min1', 'left')
            .select(
                a,
                fill_in.alias('min1'),
                (f.col('min1') !**=** fill_in).alias('change')
            )
        ) *# if there is a max_n, assign the global minimum id to 
        # any components with more than max_n nodes*
        **if** max_n **is** **not** None:
            mins **=** (
                mins
                .withColumn(
                    'n',
                    f.count(f.col(a)).over(
                        w.partitionBy(
                            f.col('min1')).orderBy(f.lit(None)
                        )
                    )
                )
                .withColumn(
                    'min1', 
                    f.when(f.col('n') >**=** max_n, f.lit(minimum_id))
                    .otherwise(f.col('min1'))
                )
                .drop('n')
            ) *# logic for deciding whether to persist of checkpoint*
            **if** (niter **%** checkpoint_every) **==** 0:
                filepath **=** (
                    checkpoint_dir **+** filepattern.format(niter)
                )
                used_filepaths.append(filepath)
                mins.write.parquet(filepath, mode**=**'overwrite')
                mins **=** ss.read.parquet(filepath)
            **else**:
                mins **=** mins.persist() *# update inputs for stopping logic*
        not_done **=** mins.filter(f.col('change') **==** True).count()
        old_done **=** new_done
        new_done **=** not_done
        n_components **=** mins.select(
            f.countDistinct(f.col(m1)).alias('n')
        ).collect()[0]['n']
        **print**(niter, not_done, n_components) output **=** mins.select(
        f.col(a).alias(label), 
        f.col(m1).alias('component')
    ) **if** max_n **is** **not** None:
        output **=** (
            output
            .withColumn(
                'component',
                f.when(
                    f.col('component') **==** minimum_id, 
                    f.lit(None)
                ).otherwise(f.col('component'))
            )
        ) output **=** output.persist() n_components **=** output.select(
        f.countDistinct(f.col('component')).alias('n')
    ).collect()[0]['n']
    **print**('total components:', n_components) *# clean up checkpointed files*
    **for** filepath **in** used_filepaths:
        subprocess.call(["hdfs", "dfs", "-rm", "-r", filepath])**return** output
```