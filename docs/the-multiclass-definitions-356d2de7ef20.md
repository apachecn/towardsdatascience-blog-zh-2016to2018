# 多类定义

> 原文：<https://towardsdatascience.com/the-multiclass-definitions-356d2de7ef20?source=collection_archive---------4----------------------->

![](img/829a5d40aa1f9e76748c41f93cad285e.png)

Photo by [Tuân Nguyễn Minh](https://unsplash.com/photos/pGr7g4l8EOI?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) on [Unsplash](https://unsplash.com/search/photos/flowers?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

初露头角的机器学习程序员学到的第一课就是关于二进制分类。这是你和你的模型试图将一个输入归类为两个结果之一的想法。这是一个热狗或不是热狗，你应该拒绝或接受某人的贷款，或者你认为一个学生会通过或不及格。

一旦你掌握了这一点，课程就会开始教授多类分类。这是输入可以被归类为许多输出中的一种的想法——这更接近地代表了世界。也许你想预测一个介于 0-9 之间的数字图像。或者，你可能对邻居草坪上的那朵漂亮的花很好奇，所以你训练了一个模特来找出答案。一些首要的关键经验包括像[一次性编码](http://scikit-learn.org/stable/modules/generated/sklearn.preprocessing.OneHotEncoder.html)或标签编码。

我们都在这里学习，我最近发现了不同的[多类和多标签](http://scikit-learn.org/stable/modules/multiclass.html)分类，我想与大家分享。所以，让我们开始吧。

## 多标签分类

这组算法可以被认为是将您的输入分类为一个或多个类的一部分。一秒钟想到中等文章。当作者准备发表他们的文章时，他们必须决定一组代表他们文章的标签。这些标签可能是“人工智能”、“愚蠢的故事”或“走向数据科学”。

现在，Medium 或有时间的人可以训练一个模型，它可以通过对文章本身进行一些自然语言处理来学习人们如何标记他们的文章。然后，他们的模型将能够预测或推荐一篇新文章应该拥有的前 5 个“标签”或标记。

这里的思路是，没有*没有*互斥，输出可以把输入归为很多事物中的一种。

## 多类分类

反过来，多类分类*确实有*互斥性。如果我们把中等的类比延伸得更远一点，同样的模型只会预测或推荐其中一个标签，而不是许多。

这种类型的问题在机器学习指南中更常见，因为训练集可能有定义的基础事实。例如，如果您有一个预测狗品种的分类器，您会希望模型选择一个输出而不是两个。

有趣的是，在这组方法中有几个子类。有一对一和一对全/休息分类。这里的是一对[链接](https://www.quora.com/Whats-an-intuitive-explanation-of-one-versus-one-classification-for-support-vector-machines)，它们简洁地解释了这些差异。本质上，将多类分类分成更简单的子问题，特别是二分类问题，是聪明的方法。

## 多输出回归

这种分类方法类似于多类分类，但不是模型预测的类，而是模型为结果提供一个数字或连续变量。如果你想创建一个输出苹果股价和下一步走势的模型，这可能是个不错的选择。

像往常一样，在机器学习中，即使是第一个主题，也总是有更多的东西要学。如果您想更深入地研究这些方法，您可以查看属于每个类别的不同算法。SVC、朴素贝叶斯和随机森林可以归入不同的类别，它可以让您更好地理解这些工具集之间的差异。

感谢阅读。