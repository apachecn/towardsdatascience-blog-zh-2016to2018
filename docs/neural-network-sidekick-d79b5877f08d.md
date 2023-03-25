# 神经网络助手

> 原文：<https://towardsdatascience.com/neural-network-sidekick-d79b5877f08d?source=collection_archive---------6----------------------->

这篇文章的想法是训练一个助手神经网络，专门针对我们的主网络所处理的数据。类似蝙蝠侠和罗宾的网络之间的内置层级将有助于该模型更好地执行。

尽管面临计算复杂性的挑战，集成学习仍然是一个非常令人兴奋的概念。有两类主要的集成学习方法，bagging 和 boosting。使用 bagging，来自训练数据的随机样本用于构建单独的模型，然后从这些模型中进行多数投票以确定输出。boosting 算法更有趣。Boosting 构建一个模型，然后评估该模型在哪个数据子集上出错最多，然后主要使用该数据来构建后续模型。这可能在计算上是昂贵的，并且在如何确定求解算法上是混乱的。

在最近的一个项目中，我使用许多卷积层和数以千计的数据图像构建了一个怪物图像分类器。我已经尝试了书中的每一个技巧来提高这个模型的准确性。然而，这个模型有时仍然会出错。一个模型执行了大约 90–95%可以被认为是做得很好，不幸的是，一些任务需要比这更高的精度。

![](img/bf6934002dccc044a7adedec2e9b281a.png)

Boosting 似乎是支持这种整体网络的一个很好的解决方案。类似于蝙蝠侠和罗宾，我的主要卷积网络是蝙蝠侠，而由主要错误分类的实例构建的卷积网络是罗宾。罗宾不需要成为团队的代言人，而是应该专注于学习蝙蝠侠的杂项倾向。通过成为蝙蝠侠的专家，他很可能会自己解决问题，罗宾能够为团队做出贡献。

用神经网络实现 boosting 是很棘手的，因为没有一个我们可以调用的库函数，比如 Boosted_CNNs。

这是我如何实现 boosting 来创建蝙蝠侠和罗宾类型的分类模型的粗略工作流程:

```
Partition Train/Test split
Train CNN with Train dataEvaluate CNN on Train data outside of training loop
Select misclassified instances and 25% of the dataTrain CNN on this new subsetWith future predictions weight the output of CNN1 80% & CNN2 20%Evaluate model
```

我发现要做到这一点，你需要大量的数据，然而，我确实看到了我的具体问题的准确性略有提高。感谢阅读！

# [CShorten](https://medium.com/@connorshorten300)

Connor Shorten 是佛罗里达大西洋大学计算机科学专业的学生。对计算机视觉、深度学习和软件工程感兴趣。