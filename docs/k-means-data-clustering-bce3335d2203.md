# k-均值数据聚类

> 原文：<https://towardsdatascience.com/k-means-data-clustering-bce3335d2203?source=collection_archive---------5----------------------->

在当今世界，随着互联网使用的增加，产生的数据量惊人地庞大。即使单个数据的性质很简单，但要处理的数据量之大，甚至连计算机都难以处理。

我们需要大数据分析工具来处理这样的流程。数据挖掘算法和技术以及机器学习为我们提供了以可理解的方式解释大数据的方法。k-means 是一种数据聚类算法，可用于无监督的机器学习。它能够将相似的未标记数据分组到预定数量的簇(k)中。需要注意的是，我们需要事先知道数据中的自然聚类数，尽管可以对代码进行轻微的修改，以计算为代价找到最优的聚类数。

![](img/1a96d7494434e9f5bfcaa5773794e79a.png)

Source: [https://i.stack.imgur.com/cIDB3.png](https://i.stack.imgur.com/cIDB3.png)

K-means 聚类问题可以正式表述为*“给定一个整数 k 和 R^d 中的一组 n 个数据点，目标是选择 k 个中心，以最小化每个数据点与其最近中心之间的总平方距离”*。精确地解决这个问题是 NP 难的，但是有算法可以局部搜索它的解。标准算法是由 Stuart Lloyd 在 1957 年首先提出的，它依赖于迭代改进。

首先，必须确定聚类中心的初始集合。这可以随机分配。但是这不能保证准确性。这可以在 python 中完成，如下所示:

```
#datapoints is a 2D array initialized with the points to be clustered
centroids = []
for i in range(1,k+1): 
    uniqueNumber = False 
    while(uniqueNumber==False): 
    a = randint(0, len(dataPoints)-1) 
    if (dataPoints[a] not in centroids): 
        uniqueNumber = True 
    centroids.append(dataPoints[a])
```

那么算法通过在两个步骤之间交替进行。

**步骤 1:** (分配步骤)将每个数据点分配给根据欧几里德距离具有最接近该数据点的中心的聚类。

**步骤 2:** (更新步骤)使用每个聚类的所有成员重新计算每个聚类的质心。

保证每个点与其最近的中心之间的总平方距离单调递减，以便配置不会重复。此外，由于只有 k^n 可能的集群，该过程将总是终止。由于该算法是局部搜索，不能保证全局最优解，初始种子在很大程度上影响结果。取决于初始种子的质量，聚类可能是任意差的。有一些选择初始种子的方法可以增加找到全局最优解的概率。

选择初始中心后，数据点可以聚类为:

```
closestCluster = [0,euclidean_distance(point,centroids[0])] 
for i in range(1,k): 
    dist = euclidean_distance(point,centroids[i])
    if (dist<closestCluster[1]): 
        closestCluster=[i,dist] clusters[closestCluster[0]].append(point)
```

在第一次聚类之后，使用如下聚类来找到新的质心:

```
#np is numpy
newCentroids = []
for i in range(k): 
    newCentroids.append(np.mean(clusters[i],axis=0).tolist())
```

然后，我们比较以前的聚类中心和新的质心，如果有任何变化，我们回到分配步骤。如果质心没有变化，我们就终止循环。

请务必访问我的回购在[https://github.com/niruhan/k-means-clustering](https://github.com/niruhan/k-means-clustering)的完整代码。标准算法有许多改进，有望提高性能。有些是 k-means++，k-means#。此外，标准算法是一个批处理算法，这意味着我们应该在开始处理之前拥有所有的数据点。但实际上，数据是以流的形式到达处理引擎的，需要进行实时分析。k-means 算法有在线版本，可以对流数据集进行聚类。顺序 k-means 是一种标准算法。还有小批量 k-means，以及使用衰减率处理非平稳数据的方法。

**参考文献:**

1.  *“k-means ++小心播种的好处”——大卫·亚瑟和谢尔盖·瓦西里维茨基*
2.  *“k 均值聚类”在 https://en.wikipedia.org/wiki/K-means_clustering*