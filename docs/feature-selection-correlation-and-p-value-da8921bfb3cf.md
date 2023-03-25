# 要素选择-相关性和 P 值

> 原文：<https://towardsdatascience.com/feature-selection-correlation-and-p-value-da8921bfb3cf?source=collection_archive---------1----------------------->

*本文正被移至我的* [*子栈发布*](https://vishalramesh.substack.com/) *。这里* *可以免费阅读文章* [*。这篇文章将于 2022 年 6 月 18 日被删除。*](https://vishalramesh.substack.com/p/feature-selection-correlation-and-p-value-da8921bfb3cf?r=9u6n7&s=w&utm_campaign=post&utm_medium=web)

通常，当我们获得一个数据集时，我们可能会在数据集中发现过多的要素。我们在数据集中找到的所有特征可能都无法用于建立机器学习模型来进行必要的预测。使用某些特征甚至可能使预测变得更糟。因此，特征选择在建立机器学习模型中起着巨大的作用。

在本文中，我们将探讨两种方法，我们可以使用这些数据来选择正确的功能。

# 什么是相关性？

相关性是一个统计学术语，通常指的是两个变量之间有多接近线性关系。

例如，两个线性相关的变量(比如，x = 2y 时相互依赖的 **x** 和 **y** )将比两个非线性相关的变量(比如，u = v2 时相互依赖的 u 和 v)具有更高的相关性

# 相关性对特征选择有什么帮助？

相关性高的特征更具线性相关性，因此对因变量的影响几乎相同。所以，当两个特征高度相关时，我们可以去掉其中一个特征。

# p 值

在我们试图了解 p 值之前，我们需要了解零假设。

**零假设**是两个被测现象之间没有关系的一般说法。

> 测试(接受、批准、拒绝或否定)零假设——并由此得出结论，有或没有理由相信两种现象之间存在联系(例如，潜在的治疗具有可测量的效果)——是现代科学实践的中心任务；统计学领域给出了拒绝无效假设的精确标准。
> 
> *来源:* [*维基百科*](https://en.wikipedia.org/wiki/Null_hypothesis)

关于零假设的更多信息，请查看上面的维基百科文章

# 什么是 p 值？

*本文其余部分已移至出版物* [*机器学习——科学、工程和 Ops*](https://vishalramesh.substack.com/) *。这里* *可以免费阅读整篇文章* [*。*](https://vishalramesh.substack.com/p/feature-selection-correlation-and-p-value-da8921bfb3cf?s=w)