# 机器学习工具箱的 5 种回归类型

> 原文：<https://towardsdatascience.com/five-regression-types-b07483813b33?source=collection_archive---------11----------------------->

![](img/948718a5848c8be0f44187eb129d6384.png)

[Siora Photography](https://unsplash.com/@siora18?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

机器学习领域发展迅速。然而，一些经验丰富的技术仍然存在。最重要的是回归技术。2017 年，60%的 KDNuggets 调查受访者将它们列为他们前一年使用的技术:

[](https://www.kdnuggets.com/2017/12/top-data-science-machine-learning-methods.html) [## 2017 年使用的顶级数据科学和机器学习方法

### 最新的 KDnuggets 民意调查询问:在过去 12 个月中，您使用了哪些数据科学/机器学习方法和工具…

www.kdnuggets.com](https://www.kdnuggets.com/2017/12/top-data-science-machine-learning-methods.html) 

只要这个数字一样高，你在机器学习生涯中就会遇到退步。即使你自己不使用它们，了解不同的味道和它们解决的问题也是很重要的。

在这篇文章中，我为你提供了五个不同回归的快速概述。我还添加了一些链接和提示来帮助您迈出第一步。

## 1.逻辑回归

逻辑回归对二元目标进行分类。如果您想将它应用于您的分类问题，请花些时间仔细看看 *sklearn* 中的[实现。基本的想法很简单，但是有很多方法可以微调这个方法。](http://scikit-learn.org/stable/modules/generated/sklearn.linear_model.LogisticRegression.html)

> **提示:**默认情况下，逻辑回归——和许多其他变量一样——假设所有特征对结果变量都有独立的(也称为加性的)线性影响。如果这个假设在你的用例中不成立，考虑交互效果或者其他方法来包含特性之间的复杂关系。

## 2.有序逻辑回归

如果你有一个排序目标变量的机器学习问题，使用有序逻辑回归。例如星级评定或小规模调查。通常的做法是将这些情况建模为度量或多类问题。然而，这些替代方案淡化了目标的序数特征。只是用有序逻辑回归代替。这种回归技术不太为人所知，但却非常强大。如果你想试试的话，可以看看 Python 中的[mord 包](https://pythonhosted.org/mord/index.html)。

> **提示:**与逻辑回归相比，有序逻辑回归的输出看起来相似。然而，对结果的解释更加困难。特别是优势比在这里可能会产生很大的误导。

## 3.普通最小二乘法

对度量变量使用普通最小二乘法(OLS)。该模型以及对结果的解释都很简单。Python 的实现在 [*sklearn*](http://scikit-learn.org/stable/modules/generated/sklearn.linear_model.LinearRegression.html) 和 [*statsmodels*](https://www.statsmodels.org/dev/generated/statsmodels.regression.linear_model.OLS.html) 中都可用。

> 提示: OLS 依赖于几个真实数据经常违反的假设。结果的后果可能是巨大的。[查看这里对最重要的假设](http://statisticsbyjim.com/regression/ols-linear-regression-assumptions/)的详细解释。

## 4.计数数据回归

计数数据回归是一组处理目标变量的方法，这些变量的值总是正数和整数。由此产生的数据通常是非常不准确的。参见这篇关于各种选项的精彩文章:

[](https://www.theanalysisfactor.com/poisson-or-negative-binomial-using-count-model-diagnostics-to-select-a-model/) [## 泊松还是负二项式？使用计数模型诊断来选择模型

### 选择适当的盘点模型来分析离散盘点结果的关键标准之一是相对值…

www.theanalysisfactor.com](https://www.theanalysisfactor.com/poisson-or-negative-binomial-using-count-model-diagnostics-to-select-a-model/) 

> **提示**:底层发行版的选择至关重要。确保您使用了可用的测试来选择正确的测试。

## 5.正则化技术

正则化是防止过度拟合的一种方法。在回归技术的背景下，有两种正则化:L1 和 L2。如果你使用 L1，你是在应用所谓的套索回归。如果你使用 L2，你是在使用岭回归。在第一种情况下，模型倾向于将系数设置为零。在第二种情况下，它试图在整个范围内保持较小的系数。在弹性网中还实现了两种技术的组合。

正则化本身是一个话题，但是由于 sklearn 中的实现，它们很容易使用。关于 L1 和 L2 的详细解释，请看这里:

 [## L1 和 L2 正则化方法

### 机器学习

towardsdatascience.com](/l1-and-l2-regularization-methods-ce25e7fc831c) 

> 提示:了解每种方法会产生什么类型的输出是至关重要的。例如，如果你有两个相关的特征，套索随机选择其中一个，而弹性网选择两个。

你用什么回归，为什么？请在评论中或在 [Twitter](https://twitter.com/TimoBohm) 上告诉我。我也很乐意在 LinkedIn 上联系。**感谢阅读，留点👏🏻如果这对你有帮助，让我们继续学习吧！**