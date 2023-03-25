# 正态性检验

> 原文：<https://towardsdatascience.com/tests-of-normality-6b92b84794a8?source=collection_archive---------4----------------------->

[**给定一组观测值，如何检验一个随机变量是否服从正态分布？**](https://www.quora.com/Given-a-set-of-observations-how-do-you-test-whether-a-random-variable-follows-normal-distribution/answer/Mayank-Kakodkar)

在数据挖掘和机器学习领域，这是一个非常常见的问题。这个问题通过[假设检验](https://en.wikipedia.org/wiki/Statistical_hypothesis_testing)来回答。

你可以在这里找到很多关于正态性检验的惊人信息，但是我会试着解释我的答案的基础。

我过去使用过卡方检验、科尔莫戈罗夫-斯米尔诺夫检验和夏皮罗-维尔克检验，所有这些检验都可以在 R 中打包获得，也可能在 NumPy 中实现。

无效假设通常是从正态分布中抽取值。所以通常会进行双尾假设检验，如果零假设被拒绝，你可以很有把握地说这些值不是来自正态分布。通常假设相反的情况，但严格来说，这是不正确的，因为在假设检验(费雪检验)中，你只能拒绝假设。

用于检验假设的统计通常基于正态分布的一些一般性质。卡方检验利用了标准正态分布的平方遵循卡方分布的假设。KS 检验利用累积概率分布的特性来比较任意两个分布，在本例中是您的样本，以及一个正态分布。