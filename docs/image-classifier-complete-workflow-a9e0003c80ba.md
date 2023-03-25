# 图像分类:完整的工作流程

> 原文：<https://towardsdatascience.com/image-classifier-complete-workflow-a9e0003c80ba?source=collection_archive---------13----------------------->

![](img/a749f86aef575ce3cb7f99894c6efdca.png)

# I .定义问题并获取数据

机器学习模型一般分为两类，分类和回归模型。然而，分类模型之间还有一些额外的变化。例如，您可以预测一个二进制输出，如猫对狗，或许多输出，如 CIFAR-10 数据集，还有许多其他方法来映射分类器的输出，这将反映在网络的最终层。

一旦你定义了你的问题，你就应该开始考虑如何获得你的数据。最流行的方法之一是开发一种网络抓取算法，从网站上的标签中抓取图像。你也可以使用机械土耳其人外包你的数据收集需求。我通常只是通过简单地在网上截图并改变截图的默认存储位置来开始项目，以便通过这种方式收集数据。这个过程显然是非常艰巨的，但是，我发现它可以很好地获得项目的快速原型。

此外，一旦您收集了大量数据，您将需要考虑替代的数据存储方法，如[云。](/storing-training-data-on-the-cloud-ba815ce20819)

# 二。卷积架构，目标是增加复杂性

一旦你收集了足够的数据开始，你将建立你的卷积网络。这对于 TFlearn、Keras 或 PyTorch APIs 来说非常容易。下面的文章将为您提供入门代码，您可以轻松地将其复制并粘贴到您的项目中，从而创建一个非常简单的 CNN 架构。

[](/how-to-build-a-convolutional-network-classifier-81eef880715e) [## Python:如何构建卷积网络分类器:耐克 vs 阿迪达斯鞋子

### 我为#100DaysOfMLCode 做的一个实验是建立一个图像分类器来区分 Nike…

towardsdatascience.com](/how-to-build-a-convolutional-network-classifier-81eef880715e) 

渐进复杂性:当你在试验你的卷积网络时，最好从一个简单的结构开始，比如 1-4 个卷积层。当您添加更多数据时，或者如果您的初始结果不令人满意，您可能希望添加更多图层。当您试验您的网络架构时，为测试模型保留一个数据集是很重要的。通过这种方式，您可以使用这种性能作为控制来试验您的模型复杂性。

# **三。数据扩充**

提高模型性能和稳健性的最简单方法之一是向数据集添加合成数据。添加更多数据的最简单方法是简单地水平翻转你的图像(如果与你的问题相关，也可以垂直翻转)。另一种流行的方法是随机生成与图像大小相同的噪声，然后将这种噪声添加到图像中。最后，如果你喜欢冒险，有很多关于使用[生成对抗网络](https://medium.com/ai-society/gans-from-scratch-1-a-deep-introduction-with-code-in-pytorch-and-tensorflow-cb03cdcdba0f)(GANs)来添加有意义的数据的研究。

# 四。解释结果，混淆矩阵

这是最重要的步骤之一，尽管经常被忽视。这一步被忽略的原因主要是因为数据昂贵且难以获得。因此，将一部分数据单独放在一边进行测试似乎是愚蠢的。然而，这对于建立您的绩效基准非常重要。

然而，除了要优化的相关性能变量之外，您还可以使用混淆矩阵可视化来深入了解您的数据。通过可视化错误分类的实例，您可以深入了解在描述整体类时哪些类型的图像未被充分代表。

# 动词 （verb 的缩写）数据采集和自动化改进

一旦您的模型被部署到某种应用程序中。您通常可以找到收集更多数据的方法，从而提高模型的性能和概化能力。

[](/automatic-data-collection-to-improve-image-classifiers-232c2f0b2070) [## 自动数据收集

### 提高图像识别模型性能的最佳方法之一是添加更多数据进行训练！一些最…

towardsdatascience.com](/automatic-data-collection-to-improve-image-classifiers-232c2f0b2070) 

# 结论

我相信这个工作流程是构建图像分类模型的一系列步骤。如果您想获得关于改进图像识别模型的更多提示，请点击[此处](/hacking-your-image-recognition-model-909ad4176247)。感谢您的阅读！

# [CShorten](https://medium.com/@connorshorten300)

Connor Shorten 是佛罗里达大西洋大学计算机科学专业的学生。对计算机视觉、深度学习和软件工程感兴趣。