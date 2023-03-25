# 快速 ML 概念:张量

> 原文：<https://towardsdatascience.com/quick-ml-concepts-tensors-eb1330d7760f?source=collection_archive---------3----------------------->

## 机器学习的基本构件简介

Tensorflow、Tensorlab、深度张量化网络、张量化 lstm……很多机器学习技术的名称中都嵌入了“张量”这个词，这并不奇怪。但是张量是什么呢？它们与机器学习有什么关系？在**快速 ML 概念**的第一部分，我的目标是提供一个简短而简明的张量是什么的总结。

![](img/9a2677a9e437a7fced6b69e2c377440d.png)

Photo by [Olav Ahrens Røtne](https://unsplash.com/@olav_ahrens?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

不要让“张量”这个词吓到你。它只不过是一个简单的数学概念。张量是将标量、向量和矩阵推广到更高维的数学对象。如果你熟悉基本的线性代数，你应该不难理解什么是张量。简而言之，一维张量可以表示为一个向量。你可能已经猜到，二维张量可以用矩阵来表示。

尽管很容易将张量概括为多维矩阵，范围从*0*到 *N* 维，但记住张量是动态的*很重要。即张量在与其他数学实体交互时会发生变换。另一方面，矩阵并不总是具有这种性质。引用自 [Steven Steinke](https://medium.com/u/5752460f103e?source=post_page-----eb1330d7760f--------------------------------) 的精彩[文章](https://medium.com/@quantumsteinke/whats-the-difference-between-a-matrix-and-a-tensor-4505fbdc576c)关于张量和矩阵的根本区别:*任何秩为 2 的张量都可以表示为矩阵，但并不是每个矩阵都真的是秩为 2 的张量*。*

# **张量运算**

张量运算很简单。考虑以下张量:

```
a = np.array([[[4,1,2],
               [3,5,2],
               [1,6,7]]
               [[2,1,0],
               [5,4,3],
               [6,8,9]]
               [[1,2,3],
               [2,3,4],
               [5,5,5]]])
b = np.array([[[1,1,1],
               [2,2,2],
               [3,3,3]]
               [[4,4,4],
               [5,5,5],
               [6,6,6]]
               [[7,7,7],
               [8,8,8],
               [9,9,9]]])
```

很容易看出，两个张量的秩都是 3。也就是说，它们有三个轴。您可以通过 Python 中的以下代码来检查张量的维度

```
a.ndim
>>> 3
```

上述两个张量的相加产生一个张量，其中每个标量值是父张量中标量的元素相加。

```
print(a+b)
>>> [[[5,2,3],
      [5,7,4],
      [4,9,10]]
      [[6,5,4],
      [10,9,8],
      [12,14,15]]
      [[8,10,11],
      [10,11,12],
      [14,14,14]]]
```

如果你有兴趣了解更多关于张量运算的知识， [Jason Brownlee](https://medium.com/u/f374d0159316?source=post_page-----eb1330d7760f--------------------------------) 的优秀[张量教程](https://machinelearningmastery.com/introduction-to-tensors-for-machine-learning/)总结了包括张量点和 Hadamart 积在内的几种常见的张量运算。

# 机器学习中的张量

那么张量和机器学习有什么关系呢？

记住，大多数机器在没有任何**数据**的情况下无法学习。而现代数据往往是多维的。张量可以通过对多维数据进行编码，在 ML 中发挥重要作用。比如一张图片一般用三个字段来表示:*宽度、*高度、*深度(颜色)*。把它编码成 3D 张量是完全有意义的。然而，我们更多的是在处理成千上万的图片。因此，这就是第四个字段*样本大小*发挥作用的地方。一系列图像，如著名的 MNIST 数据集，可以很容易地存储在 tensor flow[的 4D 张量中。这种表示方式可以轻松解决涉及大数据的问题。](https://hackernoon.com/learning-ai-if-you-suck-at-math-p4-tensors-illustrated-with-cats-27f0002c9b32)

![](img/69d9207b93d6f029fb6561778549f196.png)

Photo by [Daniele Levis Pelusi](https://unsplash.com/@yogidan2012?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

现实生活中的一个例子是，在大多数推荐系统中，基于模型的协同过滤方法，如矩阵分解，没有将上下文信息集成到模型中的灵活性。通过使用 N 维张量**而不是传统的 2D 用户项目矩阵，研究人员开发了一个更强大的[模型](https://xamat.github.io//pubs/karatzoglu-recsys-2010.pdf)，提供上下文感知的推荐。近年来计算能力的提高也使得这些繁重的张量运算得以实现。**

此时你可能会疑惑:为什么机器学习算法大多依赖向量和矩阵，而深度学习算法和神经网络大多依赖张量？一个简单的答案是，深度学习通常涉及数百个甚至数千个维度和领域。正如我们之前所讨论的，这最好用张量来表示，因为它们可以表示从*零*到 *N* 维的任何东西。在计算机视觉问题中，如[默克分子活动挑战](https://www.kaggle.com/c/MerckActivity)，图像可以很容易地被分解成几百个特征。因此，张量可以最好地被视为在机器学习和深度学习的背景下包装和存储数据特征的容器。

感谢你阅读我的文章。