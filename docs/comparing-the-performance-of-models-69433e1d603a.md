# 比较模型的性能

> 原文：<https://towardsdatascience.com/comparing-the-performance-of-models-69433e1d603a?source=collection_archive---------1----------------------->

## 最近在 [Quora](https://www.quora.com/How-can-I-show-that-small-improvement-in-F-score-is-statistically-significant) 上看到下面这个问题。以前有人问过我同样的问题，我认为用它来写博客是有意义的，原因有两个，它能接触到更多的人，内容变得可重复使用，它激励我创建一个可以传播知识的博客。

```
How can I show that small improvement in F-score is statistically significant?
```

这不是构建假设的最佳方式。

更好的方法是形成一个假设，并进行假设检验。

你从一个假设开始，你正在尝试的两种方法有相同的 F 值，任何变化都是由于世界上固有的随机性。我们称之为无效假设。

现在你只能说你的进步是显著的，如果你能以预先定义的置信度拒绝这个假设，例如 95%的置信度意味着 p 值为 0.05。

一旦你准备好这个设置，你需要得到实际的样本来测试你的假设。一个简单的方法是进行 k 倍交叉验证，以获得两种方法的 k 个样本，并使用这些样本进行 t 检验。

另一种方法是再次使用 k 倍交叉验证来获得 k 个样本。然后，假设 k 个样本来自正态分布，您可以使用生成的样本来估计其超参数。现在，你可以从这个后验样本中抽取 100 次，看看任何时候一个比另一个表现得更好[我需要和我的顾问澄清这部分]

除了 k 倍交叉验证，你还可以尝试 bootstrap 抽样。

免责声明:只有在严格比较两种方法时，才能使用这种方法。如果你已经尝试了其他方法，你需要应用 Bonferroni 修正或其他修正，因为你在测试多重假设。

注意:这个练习的全部目的是推理你的 F 分数的变化实际上是否显著。

注意:我要感谢 [Bruno Ribeiro](https://www.cs.purdue.edu/homes/ribeirob/) 的数据挖掘课，我从他的课上获得了上述知识。我的错误或者不理解，可能并不归咎于他。