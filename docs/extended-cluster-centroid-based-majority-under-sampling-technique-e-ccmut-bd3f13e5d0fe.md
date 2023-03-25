# 基于扩展聚类质心的多数欠采样技术

> 原文：<https://towardsdatascience.com/extended-cluster-centroid-based-majority-under-sampling-technique-e-ccmut-bd3f13e5d0fe?source=collection_archive---------10----------------------->

![](img/aa20064ca3026ef3fae1be8d8b988f8f.png)

在我的上一篇文章“Python 中基于聚类质心的多数欠采样技术(CCMUT)的实现 [*”中，我试图重温基于聚类质心的多数欠采样技术(CCMUT)，并给出了它的 Python 实现。在这篇文章中，我将首次介绍 CCMUT 的**扩展版本，并将其命名为 **E-CCMUT** 。***](/implementation-of-cluster-centroid-based-majority-under-sampling-technique-ccmut-in-python-f006a96ed41c)

在 CCMUT 中，要求通过在属于多数类的特征空间中的数据点上平均所有特征的特征向量来找到聚类质心。但是，聚类质心是一个点，该点**可能是也可能不是特征空间中的一个假想点**，相对于该点，大多数类实例被划定其重要程度。聚类质心为虚的概率高得多，因为仅在一些非常特殊的情况下，聚类质心最初可能作为数据集中的样本作为数据点存在。

因此，由于聚类质心在大多数情况下仍然是特征空间中的一个假想点，直观上，它对于相对于真实存在的数据点(而不是假想的数据点)来划分多数实例的重要程度肯定是更有效和正确的，认为它是最重要的。

E-CCMUT 正是处理这种修改。这里，最接近群集质心的多数类实例，比如说 **M** (使用与 CCMUT 中相同的过程找到群集质心)被视为最重要的实例(如在 CCMUT 中)，但是其他多数类实例的重要程度的划分是相对于该实例进行的，这是针对 CCMUT 中的群集质心进行的。同样，离 **M** 最远的多数类实例被认为是最不重要的，而离属于多数类的 **M** 、最近的数据点被认为是最重要的。

E-CCMUT 的伪代码如下:

1.  n:特征的数量。
2.  n:多数实例的原始数量。
3.  x:大多数实例存储在一个(N×N)矩阵中。
4.  f:欠采样的百分比。
5.  euclidean(x1，x2):返回特征空间中点 x1 和 x2 之间的欧氏距离。
6.  rev(l):它返回列表的反向排序形式

```
1\. under_sample(X,f)
2\. {
3\.  d[N]
4\.  cluster_centroid = sum(X[1], X[2], X[3],.., X[N])/N
5\.  for i = 1 to N:
6\.  {
7\.   d[i] = euclidean(cluster_centroid,X[i])
8\.  }
9\.  cluster_centroid = X[index of the minimum element in d]
10\. for i=1 to N-1:
11\. {
12\.  d[i] = euclidean(cluster_centroid,X[i])
13\. }
14\. indices = original indices list of elements in rev(d)
15\. x = delete(X, indices[:(f*N)])
16\. return x
17.}
```

使用函数 under_sample()构造伪代码，该函数将多数实例矩阵和欠采样百分比作为参数，并在欠采样后返回多数样本矩阵 x。

E-CCMUT 被认为比 CCMUT 直观地获得更好的结果。

在我即将发表的文章中，我将拿出实验证据，证明 E-CCMUT 比 CCMUT 表现得更好。