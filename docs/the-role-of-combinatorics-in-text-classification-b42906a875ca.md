# 组合学在文本分类中的作用

> 原文：<https://towardsdatascience.com/the-role-of-combinatorics-in-text-classification-b42906a875ca?source=collection_archive---------7----------------------->

## 还记得你可能上过的一门数学课吗…

![](img/d553ec58fc370154234e8f8da3565a63.png)

Source: [Danny Krashen](https://dkrashen.github.io/combinatorics/lectures/)

正如人们可能想象的那样，机器学习和组合学领域有很多重叠。在机器学习中，自然语言处理和文本分类包含大量有趣且具有挑战性的问题。

在这篇文章中，我将深入探讨在文本分类环境中实现组合方法的一般和特殊见解。

# 什么是文本分类？

> 文本分类是将预定义的类别分配给自由文本文档的任务

从表面上看，文本分类就像任何其他的[分类](https://machinelearningmastery.com/classification-versus-regression-in-machine-learning/)问题一样。机器学习中的分类问题涉及给给定的输入数据分配正确的类别标签。在文本分类的情况下，输入数据是文本的语料库，并且标签可以基于场景而变化。

这种实践不仅可以提供文档集合的直观概念视图，而且在现实世界中也有重要的[应用](https://dzone.com/articles/text-classification-applications-and-use-cases)。

# 文本分类的例子

在你的日常生活中，有大量的文本分类的例子。几个值得注意的[用例](https://www.coursera.org/learn/python-text-mining/lecture/H05Dd/text-classification)包括:

*   话题识别:一篇新闻是关于政治、体育还是科技的？
*   垃圾邮件检测:电子邮件是否是垃圾邮件？
*   情绪分析:这个影评是正面的还是负面的？
*   拼写纠正:天气还是天气？颜色还是色彩？

注意，输入不一定是一篇文章或一段文字。如在关于基于上下文的拼写校正的最后一个例子中所看到的，输入可以小到一些选择的单词或字母。

# 什么是组合学？

> 简而言之，组合学是对有限或可数结构的研究。

虽然这个定义相当宽泛，但我希望它有助于阐明组合学如何帮助提高文本分类问题的效率。如果你没有听说过组合学，不要担心——如果你分析过样本空间大小，甚至只是以一种有效的方式简单地统计过事物，你无疑已经在不知不觉中使用过它了。

[](/how-to-think-about-combinatorics-like-a-data-scientist-bddbd18eff80) [## 如何像数据科学家一样思考组合学

### 牢固掌握这些基本概念，以改善您的决策过程并推动您的公司…

towardsdatascience.com](/how-to-think-about-combinatorics-like-a-data-scientist-bddbd18eff80) 

# 组合学的作用

文本分类通常涉及处理大型高维数据集。这就成了一个问题，尤其是当试图随着时间的推移缩放模型时。

然而这并不是必须的。很多时候，您可以用少得多的功能实现类似的性能。几乎总会有一些功能比其他功能重要得多。这是去除停用词(例如“the”、“a”和“he ”)的基本思路。这也符合用[主成分分析](https://medium.com/@aptrishu/understanding-principle-component-analysis-e32be0253ef0)来最小化你的特征空间的做法。

通过利用组合方法，可以获得文本数据的有意义的表示，其特征空间平均比原始空间小 50 倍。

# 应用

我写这篇文章[的动机主要是源于 Andrei V. Anghelescu 和 Ilya B. Muchnik 的一篇题为《学习分类中特征选择的组合 PCA 和 SVM 方法》的论文。](https://pdfs.semanticscholar.org/87bf/995226528690facfde52d7d1a6713c2719d5.pdf)

通过使用组合主成分分析(cPCA)和组合支持向量机(cSVM ),作者表明，多亏了组合学，在他们的试验中，有可能用*小得多*的特征空间实现 2%以内的性能

在实现和一些调查之后，这篇论文中有几个重要的见解值得一提。这两种方法遵循相似的模型，包括使用具有特定得分函数的文本的矩形矩阵表示，以便优化结果。虽然每种方法都实现了相同的目标，但它们都适用于不同类型的问题。作者怀疑 cPCA 对图像视觉特别有用，而 cSVM 对任何可以在欧几里得空间中表示的数据都有优势。

# 包扎

简而言之，这些方法“提供了一种简单的方法来确定数据点和特征可以排序的尺度，从而使线性搜索最优的特征或文档集成为可能”——这允许更有效的特征选择。

在这样做的过程中，组合方法可以帮助解决文本分类中关于[高维度](http://www.visiondummy.com/2014/04/curse-dimensionality-affect-classification/)的问题，在这种情况下，准确性往往会随着维度的增加而迅速下降。

这不仅有效地展示了如何提高文本分类的效率，还展示了组合学在数据科学领域的广泛应用。很明显，对数据驱动的问题应用组合范围通常有很大的好处。

请在家里随意尝试，并使用您在那门被遗忘的数学课中学到的知识来提高您在机器学习方面的成功！

感谢阅读！如果你喜欢这篇文章，请继续向鼓掌按钮展示你的爱。对更多的帖子感兴趣吗？请务必[关注我](https://twitter.com/conordewey3)并订阅下面的[我的简讯](https://www.conordewey.com/newsletter/)以接收任何新内容。更多关于我和我在做什么，请查看[我的网站](https://www.conordewey.com/)。