# AB 检验样本量不够大时的排列检验

> 原文：<https://towardsdatascience.com/permutation-tests-when-ab-testing-sample-size-is-not-big-enough-f1a7967ce729?source=collection_archive---------9----------------------->

![](img/db0ab824dc7dcbb1fbe0a0d23d83dacc.png)

在开始排列测试的本质之前，让我们回顾一下经常出现的情况。你想分析一个实验，但是你意识到你没有足够的数据来应用它。您已经开始分析，但现在意识到，就目前的结果而言，没有足够的[统计功效](https://awsmd.com/calculate-sample-size-statistical-power.html)来确定统计显著性。这是一个典型的场景，通常源于用于实现[统计功效](https://awsmd.com/calculate-sample-size-statistical-power.html)的有限样本量。

# 选项

*   根据收到的数据执行计算，这不是一个好主意，因为此类数据可能会严重失真
*   应用 bootstrap 解决问题。然而，在应用 bootstrap 来接受或否定零假设(H0)之后，唯一的选择是使用置信区间

然而，如果我们想用多个预测值来描述指标，或者从某个标准中提取统计数据，该怎么办呢？在这种情况下，引导程序不会非常有用。现在你已经准备好解释排列测试以及何时需要它们。

![](img/ff2942ccc0aac9000508232710cfa026.png)

让我们假设您决定使用学生的 t-test 来计算您的 a/b 测试。你假设你的数据是正态分布的，并且方差相等(例如，你用 [Bartlett 的测试](https://en.wikipedia.org/wiki/Bartlett%27s_test)证实了这一点)。接下来，您将计算 t 统计量并将其与理论分布进行比较，以此类推，从而剔除 H0。

# 置换测试可以使用稍微不同的方法

1.  按照通常的方法计算 t 统计量；我们就叫它 t0 吧
2.  例如，将所有 10 个值放在一个组中
3.  随机将 5 个值放入 A 组，另外 5 个放入 B 组
4.  计算新的 t 统计量
5.  重复步骤 3 和 4 2n 次
6.  将从 t 统计中提取的值按升序排列
7.  如果 t0 不包括在经验分布的中间 95%的值中，那么您应该拒绝关于两个样本的平均值相等的零假设，概率为 95%

这种方法可能会让你想起[贝叶斯](https://en.wikipedia.org/wiki/Bayes%27_theorem)。在 R 中，这种方法的实现是在 [Coin 包](https://www.rdocumentation.org/packages/coin)中实现的，但是您也可以使用标准函数或 boot 包来实现。

排列测试是处理样本大小不足以获得足够的统计能力来确定结果的显著性的情况的一种方法。但是，你需要记住，没有任何“小技巧”可以取代样本量来实现实验的最佳功效。只有在理解了这种方法的适用性和数据的性质后，才应该应用这种方法。

*最初发表于*[*【awsmd.com】*](https://awsmd.com/permutation-tests-in-ab-testing.html)*。*