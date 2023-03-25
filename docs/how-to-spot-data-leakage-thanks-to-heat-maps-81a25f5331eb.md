# 如何借助热图发现数据泄漏(在您的笔记本电脑上！)

> 原文：<https://towardsdatascience.com/how-to-spot-data-leakage-thanks-to-heat-maps-81a25f5331eb?source=collection_archive---------15----------------------->

## 如何确保你的模型真正学到你认为它学到的东西的指南！

![](img/44f3e02a018a4cb678db040a95ecb56d.png)

Last Convolution Layer Heat Map for two training samples. (Be patient, this animated GIF could take some time to load…)

生成以下图片所需的数据集和源代码可在[GitHub 资源库中找到。](https://github.com/nalepae/bart-vs-homer)本文所有内容都可以在配有 1 Go 内存 GPU 的笔记本电脑上重现。

在本文中，您将了解到:

*   如何在图像分类任务中发现**数据泄露**，以及
*   如何修复(对于这个给定的图像分类任务)。

## 问题是

想象一下，玛姬·辛普森委托给你一项任务:她想让你做一个算法来区分巴特和荷马。

因为她爱她的丈夫和儿子，她给了你 66 张代表他们的照片**在家庭住宅前**。(上一句的粗体部分很重要。)

以下是该数据集的摘录。

![](img/5b4a64d46f7846055f53c86e7e7a9232.png)![](img/edf87449a7d7b17c012fa62b83eea70b.png)![](img/0218ddc98eab1d6f93d88e0fd44c99e3.png)![](img/8a2e31f8c83c8832b079eba6bd7d4de1.png)

Pictures Marge gave you

作为一名实验过的数据科学家，你选择使用你最喜欢的预先训练好的图像识别深度神经网络:比如说 [VGG16](https://arxiv.org/pdf/1409.1556v6.pdf) 。
(2014 年获得[大型视觉识别挑战赛](http://www.image-net.org/challenges/LSVRC/)。)

但是，因为感兴趣的部分(Bart 或 Homer)表示图像中的一个小区域，所以您选择用单个**卷积**层和一个**全局平均池**层替换结束的**完全连接的**层，如下图所示:

![](img/08e41228b6149e9090d925eba20c92b3.png)

Model used for Bart vs. Homer classification (based on VGG16). The only trainable layer is in red.

对于那些想知道的人来说，**全球平均池**是一种复杂的说法……“平均”。我们稍后会更详细地解释它。

如上所述，对于我们的识别任务，您将只训练最后一个卷积层。

将给定数据集拆分为训练集和验证集后，您训练了最后一个卷积层。

学习曲线是好的:低训练和验证损失意味着你有好的表现，没有过度拟合。您甚至在训练和验证集上获得了 100%的准确率。

恭喜你！

![](img/886d47285acc962bc5bec913f69073a5.png)

Learning curves

现在是时候在生产中使用您的模型了！

然而，在生产中，巴特和荷马可能在世界的任何地方，包括在**斯普林菲尔德核电站**前，如下所示:

![](img/a14cd954f3203e32ed65554de51c381f.png)![](img/d9fa5e7a5cd65bebb2b64b7c8f000ab7.png)

Ground truth: Bart — Predicted label: Bart ==> **OK!**

![](img/5656fe0d2a5158db97e1082c47a1c5d5.png)![](img/ff5684a6833034eb86277f5db46e2bc8.png)

Ground truth: Homer — Predicted label: Homer ==> **OK!**

对于之前的这 4 张图片，您的模型 100%地预测了好标签(上面的图片预测为 Bart，下面的图片预测为 Homer)。

干得好，玛吉会高兴的！

但是现在让我们在一个稍微不同的数据集上训练您的模型:

*   因为荷马花费大量时间在工作上，所以玛吉给你的所有照片都代表了荷马在核电站前的样子。
*   因为巴特是个孩子，一直在玩，所以玛吉给你的所有照片都代表巴特在家庭住宅前。

下面是这个新数据集的摘录。

![](img/5b4a64d46f7846055f53c86e7e7a9232.png)![](img/edf87449a7d7b17c012fa62b83eea70b.png)![](img/43b62dd5fa5ce6ba50977496dea0ead9.png)![](img/f83023e5e3ba797522ac4b271d10b09c.png)

The new dataset. Notice that in this dataset, Bart is **always** in front of the house, and Homer is **always** in front of the power plant**.**

像第一次一样，在将给定的数据集分成训练集和验证集之后，您训练了模型的最后一个卷积层。

学习曲线 ***超级*** 好:你只用一个历元就达到了 100%的准确率！

*提示:这好得令人难以置信。*

![](img/316002863d00d63d1a824efae8d6b10a.png)

Learning curves

与之前的训练集一样，现在是时候在生产中使用您的模型了！

让我们看看你的模型有多好，巴特在核电站前面，荷马在家庭住宅前面。

请注意，在训练集中，Bart 在房子前面，Homer 在植物前面。在“生产”中，我们测试了相反的情况:巴特在工厂前面，荷马在房子前面。

![](img/a14cd954f3203e32ed65554de51c381f.png)![](img/d9fa5e7a5cd65bebb2b64b7c8f000ab7.png)

Ground truth: Bart — Predicted label: Homer ==> **NOT OK at ALL!**

![](img/2d44391d75b0d3b6f84431164c8b9c80.png)![](img/76d0942ed216515544c050106a0c5b2f.png)

Ground truth: Homer — Predicted label: Bart ==> **NOT OK at ALL!**

哎哟…你的模型总是预测错误的标签。所以我们总结一下:

*   训练集上的损失和准确性->好。
*   验证集上的损失和准确性->好。
*   生产中的模型预测->坏。

发生了什么事？

*回答:*你的模型被**数据泄露**感染。
为了学习，模型使用了一些本不该使用的功能。

## 如何发现数据泄露

首先让我们来看看你的模型的最后一部分 **:**

![](img/755dc60663a3dccf35211fb07f1cb6f2.png)

想法是在原始图片上叠加最后一个**卷积层**的输出。该层输出是 2 个 22×40 的矩阵。第一个矩阵表示用于 Bart 预测的激活，第二个矩阵表示用于 Homer 预测的激活。

当然，因为这些矩阵的形状都是 22x40，所以在将它们叠加到原来的 360x640 之前，您必须将它们放大。

让我们用验证集的 4 张图片来做这件事:
在左栏，你有“Bart”最后一个卷积层的输出，在右栏是“Homer”的输出。

*色码:蓝色为低输出(接近 0)，红色为高输出。*

![](img/c2890aee54b16082de73fe08d5651f05.png)![](img/9f2ef6ba96f849c07247e93e8382b847.png)![](img/b2f118fea7826826d8a5a2ec60cc8ba9.png)![](img/4f9fb5ab3c50ee3ff0c65616d0a5e802.png)![](img/bb6ea07aa25b4ae1b927bab42a4999ca.png)![](img/a4ab82d3f7c0fd7290e955ffef8cafa0.png)![](img/800e0585d76cd948be2dee6f2a4a6864.png)![](img/f94568e596262dbb640f53dbd34622c0.png)

Extract of validation set with heat maps corresponding to the last convolution layer output

看起来你的模型根本没有使用 Bart 和 Homer 来完成分类任务，而是使用了背景来学习！

为了确定这个假设，让我们显示没有巴特和荷马的图片**的最后一个卷积层的输出！**

![](img/9ea7b1c66d6aad2b657940a160b764b4.png)![](img/b36f53973326997e98f08c2713cd62b5.png)![](img/12fa85c5e142dedac28ad9605d8dd838.png)![](img/11a6aab2a0f324a6aae3f9e551c0295d.png)

Last convolution layer for background only

看来我们的假设是对的。添加/删除 Bart & Homer 对分类模型没有太大影响…

让我们回到我们的第一个模型上几秒钟，这个模型是用 Bart & Homer **only** 在房子前面的图片训练的，让我们显示一些验证示例的最后卷积层的输出:

![](img/6e8051dff065b34a18abfa77f88de2a2.png)![](img/33e5f8ba74b9f483952964e5ff63d710.png)![](img/2b59c188e1bd9ec20c83a9fbeea59336.png)![](img/e145a96f819a32ca8112beb78afce540.png)![](img/0ad4cfc6c4d3a39ca5cae7c85b58df99.png)![](img/8f27df447c48ac7e0bb04392972b21ea.png)![](img/030fad32d6aa7e6c04d748892c583c55.png)![](img/501516fb57756de6ae1c6bbc659cbdc9.png)

Extract of validation set with heat maps corresponding to last convolution layer output, for the model trained with Bart only in front of the house and with Homer only in front of the power plant

在这种情况下，我们现在能够**理解**为什么这个模型预测了好标签:因为它实际上使用了 Bart & Homer 来预测产量！

## 解决方案

如何修复 Bart 总是在房子前面，Homer 总是在植物前面的训练集的**数据泄露**问题？我们有几个选择:

*   最常见的一种是通过使用边界框来修改模型。但是…那很无聊:你必须一个接一个地注释每个训练样本。
*   一个更简单的解决方案是将我们的 2 类分类问题(Bart & Homer)转化为 3 类分类问题(Bart、Homer **和**背景)。
    第一个类将包含巴特的图片(在房子前面)，第二个类将包含荷马的图片(在发电厂前面)，第三个类将只包含 2 张图片:一张房子的图片和一张发电厂的图片(上面没有任何人)。

还有……就这些了！

下面是这个 3 级训练模型的学习曲线。这些曲线看起来“更正常”。

![](img/be709ea7ef847c0f2f4a1feaf053a473.png)

Learning curves for the 3-classes trained model

下面是一些验证示例的最后一个卷积层的输出:

![](img/6ed7fc359e3513b2eae15ab8e0091798.png)![](img/bfaa31456cd61e00c97f6e6beff1412a.png)![](img/96a139989afbd172e4c4d76782278b23.png)![](img/504f55e47256ccf3667b1b0d03b44210.png)![](img/b67b3c1650c90c04d1537d0e221f60bc.png)![](img/54a5ad9517439059136398a0aae8ef91.png)![](img/791fc93739e45a1001485b8198f92690.png)![](img/b7e811db26ef789fd19bebb5173204e1.png)

Extract of validation set with heat maps corresponding to the last convolution layer output, for the 3-classes trained model

您的模型现在在生产中也运行良好。

***现在你知道了:*** 如何在一个图像分类任务上发现**数据泄漏**，以及如何为我们的 Bart vs. Homer 分类任务修复它。

生成上述图像所需的数据集和源代码可在[GitHub 资源库中找到。](https://github.com/nalepae/bart-vs-homer)