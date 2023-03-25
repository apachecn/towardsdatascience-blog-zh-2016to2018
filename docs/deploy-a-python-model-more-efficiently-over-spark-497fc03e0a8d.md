# 在 Spark 上部署 Python 模型(更高效)

> 原文：<https://towardsdatascience.com/deploy-a-python-model-more-efficiently-over-spark-497fc03e0a8d?source=collection_archive---------0----------------------->

![](img/28b49ee6a4708bdd79b7aaeb99159a52.png)

**更新:我写了关于** [**部署 Scikit 的另一种方式**](/a-different-way-to-deploy-a-python-model-over-spark-2da4d625f73e)**——通过 Spark 学习模型，避免了这种方法的一些问题。**

我把这个贴出来，希望其他人可以避免一些我必须经历的痛苦。有时候我想在 scikit-learn 中训练一个模型，但是在 Spark 中为一个大型数据集中的所有记录建模结果。是的，我知道我可以使用 Spark MLlib，但我发现 scikit-learn 有一套更强大的产品，我理解 scikit 中的代码-learn 比我理解 MLlib 中的代码更好，我只是更熟悉 scikit-learn。然而，通过 PySpark UDF 调用 scikit-learn 的“predict”方法会产生一些问题:

*   对于 Spark 数据帧的每一条记录，它都会导致对模型对象进行清理和拆包的开销。
*   它没有利用 scikit-learn 的优化，这主要是由于对 NumPy 数组的矢量化函数调用。

我发现可以通过对 spark 数据帧进行分组来减轻一些开销，以便一次在多个记录上调用我的 Python 对象的方法。你可以这样做:

1.  在 Spark 数据框中设置三列:

***一个唯一的 id。这可以是任何东西。只要它是独一无二的，你就可以去。**

所有你的预测者。您可以使用 Structype 或 MLLib 的 VectorAssembler 将所有预测器放入一列。

***一组列。**你可以调用 row_number()按你想要的组数取模运算。我发现创建足够多的组，每个组包含 50-100k 条记录，这样做通常很有效。

2.调用 Spark SQL 函数“create_map ”,将您的唯一 id 和预测值列合并成一列，其中每条记录都是一个键值存储。

3.按 groups 列分组，并在 key-value 列上调用 Spark SQL 函数“collect_list”。这将把你的数据集合到字典列表中。

4.传播你的 scikit-learn 模型。

5.创建一个 UDF，将字典列表解包为一个键列表(您的唯一 id)和一个列表列表(您的预测值)。然后，您可以将列表列表直接输入到广播的 scikit-learn 模型的“predict”方法中。然后用您的键列表压缩该函数调用的结果，并转换成一个字典。udf 将返回一个 MapType，根据您的键所采用的格式以及您希望从 scikit-learn 函数调用返回的格式来适当地设置键和值的类型。

6.对 udf 的结果调用 explode，并包含两个别名—一个用于键，一个用于结果。然后，您将拥有一个新的数据框，其大小与原始(预分组)数据框相同，其中一列是您的结果，另一列是可用于将结果与原始数据连接起来的键。

这里有一个例子:

```
"""
assumes the following already exist within the environment:
`model`: a scikit-learn class that predicts probabilities for a two-class (0.0, 1.0) model
`sdf`: a spark dataframe with at least two columns: "unique_id" and "feature_list"
"""**import** pyspark.sql.functions as f
**import** pyspark.sql.types as t
**import** pyspark.sql.window.Window as w
**from** pyspark.context **import** SparkContextsc **=** SparkContext.getOrCreate()# broadcast model
model_broadcast **=** sc.broadcast(model)# udf to predict on the cluster
**def** predict_new(feature_map): ids, features **=** zip(*****[
        (k,  v) **for** d **in** feature_map **for** k, v **in** d.items()
    ]) ind **=** model_broadcast.value.classes_.tolist().index(1.0) probs **=** [
        float(v) **for** v **in** 
        model_broadcast.value.predict_proba(features)[:, ind]
    ] **return** dict(zip(ids, probs))predict_new_udf **=** f.udf(
    predict_new, 
    t.MapType(t.LongType(), t.FloatType()
)# set the number of prediction groups to create
nparts **=** 5000# put everything together
outcome_sdf **=** (
    sdf
    .select(
        f.create_map(
            f.col('unique_id'),     
            f.col('feature_list')
        ).alias('feature_map'), 
        (
            f.row_number().over(
                w.partitionBy(f.lit(1)).orderBy(f.lit(1))
            ) **%** nparts
        ).alias('grouper')
    )
    .groupby(f.col('grouper'))
    .agg(
        f.collect_list(f.col('feature_map')).alias('feature_map')
    )
    .select(
        predict_new_udf(f.col('feature_map')).alias('results')
    )
    .select(
        f.explode(f.col('results'))
        .alias('unique_id', 'probability_estimate')
    )
)
```

我希望能够展示一些图表，将上述方法的性能与在 PySpark UDF 中简单包装对模型的 predict 方法的调用的性能进行比较，但是我不能:我想出了这个方法，因为我无法让这个简单的方法完成。我有一个超过 1 亿条记录的数据集。我训练了一个朴素贝叶斯(使用 scikit-learn 的 MultinomialNB)分类器，根据散列的术语-文档矩阵(使用 scikit-learn 的 HashingVectorizer)来区分两个类——0 和 1。起初，我只是简单地广播了训练好的模型，然后编写了一个 UDF，它将一个预处理过的字符串作为参数。UDF 通过哈希矢量器运行字符串，然后将这些结果输入模型预测方法。然后，我运行脚本并监控 YARN。两天后，这个过程大约完成了 10%。然后，我按照我在这里概述的方法重新编写了这个过程:UDF 接受一个字符串列表作为参数，这个列表由哈希矢量器处理，结果提供给模型的 predict 方法。整个过程在 30 分钟内完成。

最后一点:不同的 scikit-learn 模型在内存中有很大的不同。朴素贝叶斯模型只需要为每个参数保留几个值。随机森林需要保留森林中的每一棵树。因为您需要将模型传播给每个执行者，所以您会很容易地发现，根据大量数据训练的模型需要大量内存。