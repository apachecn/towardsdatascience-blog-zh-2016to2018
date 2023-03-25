# 如何使用 Dask 数据帧在 Python 中运行并行数据分析

> 原文：<https://towardsdatascience.com/trying-out-dask-dataframes-in-python-for-fast-data-analysis-in-parallel-aa960c18a915?source=collection_archive---------1----------------------->

![](img/5f0c3d8ecb70794fdbf6fa63524f7e68.png)

Your mind on multi-cores. source: [Pixabay](https://pixabay.com/en/universe-star-space-cosmos-sky-1351865/)

有时候，你打开一个包含 Python 的熊猫的大数据集，试图获得一些指标，整个事情就可怕地冻结了。
如果你从事大数据工作，你知道如果你使用熊猫，你可能会为一个系列的简单平均值等待整整一分钟，我们甚至不要调用*应用*。这只是几百万行的数据！当你达到数十亿时，你最好开始使用 Spark 或其他东西。

不久前我发现了这个工具:一种加速 Python 中数据分析的方法，而不必获得更好的基础设施或转换语言。如果你的数据集很大，它最终会感到有限，但它比普通的熊猫要好得多，可能正好适合你的问题——特别是如果你没有做大量的重新索引。

*可以在* [*土星云*](https://www.saturncloud.io/s/freehosted/?utm_source=Luciano%20Strika-%20Dask%20Dataframes&utm_medium=saturn%20free%20hosted) *上马上免费使用 Dask！
土星云是一个端到端的数据科学+机器学习平台，允许数据科学家在云中使用 Dask 扩展他们的 Python 项目。*

## Dask 是什么？

Dask 是一个开源项目，它为您提供了对 NumPy 数组、Pandas 数据帧和常规列表的抽象，允许您使用多核处理对它们并行运行操作。

这里有一段直接来自教程的摘录:

> Dask 提供了模拟 NumPy、lists 和 Pandas 的高级数组、Bag 和 DataFrame 集合，但可以在不适合主存的数据集上并行操作。对于大型数据集，Dask 的高级集合是 NumPy 和 Pandas 的替代方案。

听起来就很牛逼！我开始尝试本文的 Dask 数据框架，并对它们运行了几个基准测试。

*(要查看 Dask 在机器学习中的更多应用，请查看我的* [*并行 k 均值聚类教程*](https://www.datastuff.tech/machine-learning/k-means-clustering-unsupervised-learning-for-recommender-systems/) *)*

## 阅读文档

我做的第一件事是阅读官方文档，看看在 Dask 的而不是常规数据帧中到底推荐做什么。以下是来自[官方文件](http://dask.pydata.org/en/latest/dataframe.html)的相关部分:

*   操作大型数据集，即使这些数据集不适合内存
*   通过使用多个内核加速长时间计算
*   使用标准的 Pandas 操作(如 groupby、join 和时序计算)对大型数据集进行分布式计算

接下来，它列出了一些使用 Dask 数据帧时速度非常快的东西:

*   算术运算(乘法或加法)
*   常见聚合(平均值、最小值、最大值、总和等。)
*   调用 *apply(只要它沿着索引-也就是说，不在 groupby('y ')之后，其中' y '不是索引-)*
*   调用 value_counts()、drop_duplicates()或 corr()
*   用 *loc* 、 *isin* 过滤，行选择

Just a small brush up on filtering Dataframes, in case you find it useful.

## 如何使用 Dask 数据帧

Dask 数据帧与 Pandas 数据帧具有相同的 API，除了聚合和*应用*被延迟计算，并且需要通过调用*计算*方法来计算。为了生成 Dask 数据帧，你可以像在 Pandas 中一样简单地调用 *read_csv* 方法，或者，给定一个 Pandas 数据帧 *df* ，你可以只调用

```
dd = ddf.from_pandas(df, npartitions=N)
```

其中 *ddf* 是您导入 Dask 数据帧时使用的名称，而 *npartitions* 是告诉数据帧您想要如何对其进行分区的参数。

根据 StackOverflow 的说法，建议将数据帧划分为与计算机内核数量一样多的分区，或者是该数量的几倍，因为每个分区将在不同的线程上运行，如果数量太多，它们之间的通信将变得过于昂贵。

## 变脏:让我们进行基准测试！

我做了一个 Jupyter 笔记本来测试这个框架，并在 Github 上发布了这个框架，以防你想亲自测试或者运行它。

我运行的基准测试可在 Github 的笔记本中找到，但以下是主要的测试:

这里 *df3* 是一个普通的熊猫数据帧，有 2500 万行，使用我的[熊猫教程](http://www.datastuff.tech/data-science/exploratory-data-analysis-with-pandas-and-jupyter-notebooks/)中的脚本生成(列是*姓名*和*薪水*，从列表中随机抽取)。我取了一个 50 行的数据集，并将其连接了 500000 次，因为我对分析本身并不太感兴趣，而只对运行它所花费的时间感兴趣。

*dfn* 就是基于 *df3* 的 Dask 数据帧。

## 第一批结果:不太乐观

我首先用 3 个分区进行了测试，因为我只有 4 个内核，不想让我的电脑超负荷工作。我在 Dask 上得到了非常糟糕的结果，也不得不等待很长时间才能得到它们，但我担心这可能是因为我做的分区太少了:

```
204.313940048 seconds for get_big_mean
39.7543280125 seconds for get_big_mean_old131.600986004 seconds for get_big_max
43.7621600628 seconds for get_big_max_old120.027213097 seconds for get_big_sum
7.49701309204 seconds for get_big_sum_old0.581165790558 seconds for filter_df
226.700095892 seconds for filter_df_old
```

你可以看到，当我使用 Dask 时，大多数操作都变慢了很多。这给了我暗示，我可能不得不使用更多的分区。生成惰性求值所花费的时间也可以忽略不计(在某些情况下不到半秒)，所以如果我重用它们，它不会随着时间的推移而摊销。

我还用*应用*方法尝试了这个测试:

得到了非常相似的结果:

```
369.541605949 seconds for apply_random
157.643756866 seconds for apply_random_old
```

因此，一般来说，大多数操作变得比原来慢两倍，尽管过滤器快得多。我担心也许我也应该调用*计算机*来处理这个问题，所以对这个结果要有所保留。

## 更多分区:惊人的速度

在这样令人沮丧的结果之后，我认为可能是我没有使用足够的分区。毕竟，这样做的全部意义在于并行运行，所以也许我只需要更多的并行化？因此，我对 8 个分区进行了相同的测试，下面是我得到的结果(我省略了非并行数据帧的结果，因为它们基本相同):

```
3.08352184296 seconds for get_big_mean
1.3314101696 seconds for get_big_max
1.21639800072 seconds for get_big_sum
0.228978157043 seconds for filter_df112.135010004 seconds for apply_random
50.2007009983 seconds for value_count_test
```

没错！大多数操作比常规数据帧快十倍以上，甚至*应用*也变得更快了！我还运行了 *value_count* 测试，它只是在 *salary* 系列上调用了 *value_count* 方法。对于上下文，请记住，当我在常规数据帧上运行这个测试时，在等待了整整十分钟之后，我不得不终止这个进程。这次只用了 50 秒！
所以基本上我只是用错了工具，而且速度相当快。比普通的数据帧快得多。

## 最终外卖

鉴于我们刚刚在一台非常旧的 4 核电脑上一分钟内处理了 2500 万行数据，我可以想象这在行业内会有多么巨大。所以我的建议是，下次您必须在本地或从单个 AWS 实例处理数据集时，尝试一下这个框架。相当快。

我希望你觉得这篇文章有趣或有用！编写它花费的时间比我预期的要多得多，因为一些基准测试花费了*这么长的时间*。请告诉我在阅读本文之前您是否听说过 Dask，以及您是否在工作中或项目中使用过它。也请告诉我是否还有其他我没有提到的很酷的特性，或者我做错了什么！你的反馈和评论是我写作的最大原因，因为我也在从中学习。

## 数据科学家的进一步阅读:

*   [自动编码器:深度学习与 TensorFlow 的热切执行](https://www.datastuff.tech/machine-learning/autoencoder-deep-learning-tensorflow-eager-api-keras/)其中我介绍了 TensorFlow 的 Keras API，并训练自动编码器进行图像压缩。
*   [LSTM:教神经网络像洛夫克拉夫特一样写作](https://www.datastuff.tech/machine-learning/lstm-how-to-train-neural-networks-to-write-like-lovecraft/)在这里我解释了 LSTM 神经网络是如何工作的，并用它来生成文本。
*   [每个数据科学家都应该知道的 5 种概率分布](https://www.datastuff.tech/data-science/5-probability-distributions-every-data-scientist-should-know/)在这些概率分布中，你可以学到一些非常常用的统计学基础知识……以及不那么基础的知识。

*关注我，获取更多 Python 教程、技巧和诀窍！如果您喜欢这篇文章，请查看我的网站* *或关注我的微博*[](http://twitter.com/strikingloo)**。**

*如果你想成为一名数据科学家，可以看看我推荐的[机器学习书籍](https://www.datastuff.tech/data-science/3-machine-learning-books-that-helped-me-level-up-as-a-data-scientist/)。*