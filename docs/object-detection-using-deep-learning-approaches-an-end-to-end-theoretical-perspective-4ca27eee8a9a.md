# 目标检测:端到端的理论视角

> 原文：<https://towardsdatascience.com/object-detection-using-deep-learning-approaches-an-end-to-end-theoretical-perspective-4ca27eee8a9a?source=collection_archive---------1----------------------->

## 对物体探测领域最有影响力的论文的详细介绍

![](img/460886889b2badc78ac571537be56714.png)

我们都知道图像分类问题。给定一幅图像，你能找出它属于哪一类吗？我们可以用 ConvNets 和使用预训练网络的[迁移学习](https://medium.com/@14prakash/transfer-learning-using-keras-d804b2e04ef8)解决任何新的图像分类问题。

> **作为固定特征提取器的 conv net**。获取一个在 ImageNet 上预训练的 ConvNet，移除最后一个完全连接的层(该层的输出是 ImageNet 等不同任务的 1000 个类分数)，然后将 ConvNet 的其余部分视为新数据集的固定特征提取器。在 AlexNet 中，这将为每个包含分类器之前隐藏层激活的图像计算 4096-D 向量。我们称这些特征为 **CNN 代码**。如果这些代码在 ImageNet 上训练 ConvNet 期间也达到了阈值(通常情况下)，则这些代码被重新赋值(即阈值为零)对性能非常重要。为所有图像提取 4096-D 代码后，为新数据集训练一个线性分类器(如线性 SVM 或 Softmax 分类器)。

作为补充说明:如果你想了解更多关于 convnets 和迁移学习的知识，我想推荐这个关于计算机视觉中的深度学习的可怕课程。本课程讨论各种 CNN 架构，涵盖图像领域的各种问题，包括检测和分割。

但是在图像领域有很多有趣的问题。我们今天要关注的是分割、定位和检测问题。

那么这些问题是什么呢？

![](img/391676b164d3fc4b9b848f1caf851382.png)

所以这些问题被分成 4 大类。在接下来的几行中，我将尝试在我们深入探讨之前简要解释这些问题:

1.  **语义分割:** *给定一幅图像，我们能否将每一个像素归类为属于特定的一类？*
2.  **分类+定位:**我们能够将图像分类为一只猫。太好了。*我们是否也可以通过在猫的周围画一个边界框来获得该猫在图像中的位置？*这里我们假设图像中有一个固定的数字(一般为 1)。
3.  **物体检测:分类+定位问题的一个**更一般的情况。在真实世界的设置中，我们事先不知道图像中有多少对象。*那么我们能否检测出图像中的所有物体，并在它们周围画出边界框？*
4.  **实例分割:** *我们可以为图像中的每个单独的对象创建遮罩吗？*不同于语义切分。怎么会？如果你看顶部的第四张图片，我们将无法使用语义分割程序区分两只狗，因为它会将两只狗合并在一起。

在本帖中，我们将主要关注**物体检测。**

# **分类+定位**

因此，让我们首先尝试理解，当图像中只有一个物体时，我们如何解决这个问题。**分类+定位**案例。CS231n 笔记中说得很清楚:

> 把本地化当成一个回归问题！

![](img/d810467b6a4efaff82360be707a99bf6.png)

**输入数据:**我们先来说说这种模型期望什么样的数据。通常在图像分类设置中，我们使用(X，y)形式的数据，其中 X 是图像，y 是分类标签。

在分类+本地化设置中，我们通常以(X，y)的形式存储数据，其中 X 仍然是图像，y 是包含(class_label，X，y，w，h)的数组，其中，

x =边界框左上角的 x 坐标

y =边界框左上角的 y 坐标

w =以像素为单位的边界框宽度

h =以像素为单位的边界框高度

**模型:**因此在这个设置中，我们创建了一个*多输出模型*，它以一幅图像作为输入，有(n_labels + 4)个输出节点。n_labels 每个输出类的节点和给出(x，y，w，h)预测的 4 个节点。

亏损:在这种情况下，亏损是非常重要的。通常，损失是 Softmax 损失(来自分类问题)和回归 L2 损失(来自边界框坐标)的加权和。

> 损失=阿尔法*软最大损失+(1-阿尔法)*L2 损失

由于这两个损失将在不同的规模，阿尔法超参数需要调整。

这里有一件事我想注意一下。我们正在尝试进行对象定位任务，但我们仍然有我们的 convnets 在这里。我们只是增加了一个输出层来预测边界框的坐标，并调整了我们的损失函数。这是整个深度学习框架的精髓所在——将各层堆叠在一起，重用组件来创建更好的模型，并创建架构来解决自己的问题。这就是我们将会看到的未来。

# **物体检测**

那么这种使用回归的定位思想是如何映射到物体检测的呢？并没有。我们没有固定数量的物体。所以我们不能有 4 个输出来表示边界框坐标。

一个天真的想法可能是将 CNN 应用于图像的许多不同的作物，CNN 将每个作物分类为对象类或背景类。这很棘手。你可以创造很多这样的作物。

## **地区提案:**

如果有一种方法(通常称为区域建议网络)可以自动为我们找到一些裁剪区域，我们就可以在这些区域上运行我们的 convnet 并完成对象检测。这就是选择性搜索(Uijlings 等人，“T4【目标识别的选择性搜索”，IJCV 2013)为 RCNN 提供的。

那么地区提案是什么:

*   找到可能包含物体的*图像区域*
*   *跑起来比较快；例如，选择性搜索在几秒钟内在 CPU 上给出 2000 个区域建议*

*地区提案是如何制定的？*

## *[](http://www.huppelen.nl/publications/selectiveSearchDraft.pdf)****:*****

***因此，本文从使用[13] (P. F. Felzenszwalb 和 D. P. Huttenlocher)的一些初始区域开始。[高效的基于图表的图像分割](http://people.cs.uchicago.edu/~pff/papers/seg-ijcv.pdf)。IJCV，59:167–181，2004 年。1, 3, 4, 5, 7)***

> ***基于图的图像分割技术通常用图 G = (V，E)来表示该问题，其中每个节点 v ∈ V 对应于图像中的一个像素，E 中的边连接某些相邻像素对。权重与每条边相关联，这是基于它所连接的像素的某些属性，例如它们的图像强度。根据不同的方法，可能有也可能没有连接每对顶点的边。***

***在本文中，他们采用了一种方法:***

> ***每个边(vi，vj )∈ E 具有相应的权重 w((vi，vj))，其是相邻元素 vi 和 vj 之间的相异度的非负**度量。在图像分割的情况下，V 中的元素是像素，并且边的权重是由该边连接的两个像素之间的不相似性的某种度量(例如，强度、颜色、运动、位置或某种其他局部属性的差异)。在基于图的方法中，分段 S 是将 V 划分成多个分量，使得每个分量(或区域)C ∈ S 对应于图中的一个连通分量。*****

***![](img/529d3c4bdf86c350f1eeb770d1a99ff4.png)***

***[Efficient GraphBased Image Segmentation](http://people.cs.uchicago.edu/~pff/papers/seg-ijcv.pdf) Example***

***正如你所看到的，如果我们在这些遮罩周围创建边界框，我们将会丢失很多区域。我们希望将整个棒球运动员放在一个边界框中。我们需要以某种方式将这些初始区域分组。***

***为此，[对象识别的选择性搜索](http://www.huppelen.nl/publications/selectiveSearchDraft.pdf)的作者将分层分组算法应用于这些初始区域。在这种算法中，他们根据颜色、纹理、大小和填充的不同相似性概念，将最相似的区域合并在一起。***

***![](img/383b9037b641bf50147351e09be69691.png)******![](img/6af40507cf84b223808a6971298a2314.png)***

***The Algorithm for region Proposal used in RCNN***

# ***RCNN***

***上面的选择性搜索是他们在 RCNN 论文中使用的区域提议。但是 RCNN 是什么，它如何使用区域提议？***

***![](img/33a157715e3973b8a4a3ef14209720aa.png)***

> ***物体检测系统概述。我们的系统***
> 
> ***(1)获取输入图像，(2)提取大约 2000 个自下而上的区域提议，(3)使用大型卷积神经网络(CNN)计算每个提议的特征，然后(4)使用特定类别的线性 SVM 对每个区域进行分类。***

***除此之外，作者还使用了特定于类的边界框回归器，它采用:***

***输入:(Px，Py，Ph，Pw)-建议区域的位置。***

***目标:(Gx，Gy，Gh，Gw)-该区域的地面实况标签。***

***我们的目标是学习一种将建议区域(P)映射到地面真实框(G)的变换***

# ***培训 RCNN***

***RCNN 的输入是什么？***

***因此，我们从 RPN 战略和标签的基本事实(标签、基本事实框)中获得了一个图像、区域建议***

***接下来，我们将所有具有≥ 0.5 IoU(交集/并集)重叠的区域提议与一个基础事实框视为该框类的正面训练示例，其余视为负面。我们训练班级特定的 SVM 氏症***

***因此，每个地区的提案都成为一个培训范例。并且 convnet 给出该区域提议的特征向量。然后，我们可以使用特定于类的数据来训练我们的 n-SVM。***

# ***测试时间 RCNN***

***在测试时，我们使用特定于类的支持向量机来预测检测盒。在测试的时候，我们会得到很多重叠的检测框。非最大抑制是对象检测流水线的组成部分。首先，它根据分数对所有检测框进行分类。选择具有最大分数的检测框 M，并且抑制与 M 有显著重叠(使用预定阈值)的所有其他检测框。这个过程递归地应用于剩余的盒子***

***![](img/8fe312b9717c6930f5fa9d3baa2496a5.png)***

***[https://www.pyimagesearch.com/wp-content/uploads/2014/10/hog_object_detection_nms.jpg](https://www.pyimagesearch.com/wp-content/uploads/2014/10/hog_object_detection_nms.jpg)***

## ***RCNN 的问题:***

*   ***训练很慢。***
*   ***推断(检测)慢。使用 VGG16 时每幅图像 47s——因为需要多次运行 Convnet。***

***对速度的需求。因此出现在同一作者的图片中:***

# ***快速 RCNN***

> ***所以来自同一作者的下一个[想法](https://www.cv-foundation.org/openaccess/content_iccv_2015/papers/Girshick_Fast_R-CNN_ICCV_2015_paper.pdf):为什么不创建输入图像的卷积图，然后只从卷积图中选择区域？我们真的需要运行这么多网络吗？我们可以只运行一个 convnet，然后对 convnet 计算出的特征应用区域建议作物，并使用简单的 SVM 对这些作物进行分类。***

***类似于:***

***![](img/a5038e26ad3ead65dbe7f5335dff6057.png)***

> ***来自[论文](https://www.cv-foundation.org/openaccess/content_iccv_2015/papers/Girshick_Fast_R-CNN_ICCV_2015_paper.pdf):图为快速 R-CNN 架构。快速 R-CNN 网络将整个图像和一组对象提议作为输入。网络首先用几个卷积(conv)和最大汇集层处理整个图像，以产生 conv 特征地图。然后，对于每个对象提议，感兴趣区域(RoI)池层从特征图中提取固定长度的特征向量。每个特征向量被馈入一系列全连接(fc)层，这些层最终分支成两个兄弟输出层:一个层产生 K 个对象类加上一个包罗万象的“背景”类的 softmax 概率估计，另一个层为 K 个对象类中的每一个输出四个实数值。每组 4 个值对 K 个类之一的精确边界框位置进行编码。***

***这个想法在一定程度上依赖于所用模型的架构。我们是从 VGG16 取 4096 瓶颈层吗？***

***所以作者提出的架构是:***

> ****我们对三个预训练的 ImageNet [4]网络进行了实验，每个网络有五个最大池层和五到十三个 conv 层(有关网络详细信息，请参见第 4.1 节)。*当一个预训练的网络初始化一个快速的 R-CNN 网络时，要经历三次变换。首先，最后一个最大池层由 RoI 池层替换，RoI 池层通过设置 H 和 W 与网络的第一个完全连接层兼容来配置(例如，对于 VGG16，H = W = 7)。第二，网络的最后一个全连接层和 softmax(其被训练用于 1000 路 ImageNet 分类)被替换为之前描述的两个兄弟层(K + 1 个类别上的全连接层和 softmax 以及类别特定的边界框回归器)。第三，网络被修改以接受两个数据输入:图像列表和那些图像中的 ROI 列表。***

***这显然有点混乱和“令人毛骨悚然”，让我们来分析一下。但为此，我们需要了解 VGG16 架构。***

***![](img/21876d57c03bd78261b702d3b608cfc5.png)***

***最后一个池层是 7x7x512。这是网络作者打算用 ROI 池层来代替的层。该池层获得了作为输入的区域提议的位置(xmin_roi，ymin_roi，h_roi，w_roi)和先前的特征地图(14x14x512)。***

***![](img/db95dff4be6b508143a19e876a667f05.png)***

***现在，ROI 坐标的位置以输入图像的单位表示，即 224×224 像素。但是我们必须应用 ROI 合并操作的层是 14x14x512。因为我们使用 VGG，我们将把图像(224 x 224 x 3)转换成(14 x 14 x 512)——高度和宽度除以 16。我们可以将 ROI 坐标除以 16，映射到特征图上。***

> ***在深度上，卷积特征图已经编码了图像的所有信息，同时保持了它相对于原始图像编码的“事物”的位置。例如，如果图像的左上角有一个红色方块，并且卷积层为其激活，则该红色方块的信息仍将位于卷积特征图的左上角。***

***投资回报池是如何实现的？***

***![](img/00ba11731e30166b227248ebd0b2e94b.png)***

***在上面的图像中，我们的区域建议是(0，3，5，7)，我们将该区域分为 4 个区域，因为我们希望有一个 2x2 的 ROI 池层。***

***[你如何在小于目标规模的区域进行投资回报率池化？](https://stackoverflow.com/questions/48163961/how-do-you-do-roi-pooling-on-areas-smaller-than-the-target-size)如果区域提案大小为 5x5，投资回报池层大小为 7x7。如果发生这种情况，[我们只需将每个单元格复制 7 次，然后最大合并回 7x7，就可以将大小调整到 35x35。](https://stackoverflow.com/questions/48163961/how-do-you-do-roi-pooling-on-areas-smaller-than-the-target-size)***

***在替换了汇集层之后，作者还将 1000 层的 imagenet 分类层替换为全连接层和 K + 1 个类别上的 softmax(对于背景+ 1)和类别特定的边界框回归器。***

# ***训练快速神经网络***

***快速 RCNN 的输入是什么？***

***非常相似:所以我们有一个图像，来自 RPN 策略的区域提议和标签的基本事实(标签，基本事实框)***

***接下来，我们将所有具有≥ 0.5 IoU(交集/并集)重叠的区域提议与一个基础事实框视为该框类的正面训练示例，其余视为负面。这一次我们有一个密集层，我们使用多任务损失。***

***所以每一个 ROI 都成了训练样本。主要区别在于多任务丢失的概念:***

***快速 R-CNN 网络具有两个兄弟输出层。第一个输出离散概率分布(每个 RoI)，p = (p0，.。。，pK)，超过 K + 1 个类别。通常，p 由 softmax 通过全连接层的 K+1 个输出来计算。第二个兄弟层为 K 个对象类中的每一个输出边界框回归偏移 t= (tx，ty，tw，th)。每个训练 RoI 都标记有基本事实类 u 和基本事实包围盒回归目标 v。我们在每个标记的 RoI 上使用多任务损失 L 来联合训练分类和包围盒回归***

***![](img/6a05d98dc6fa4269e32295ccfea7ebf3.png)***

***其中，Lcls 是 softmax 分类损失，Lloc 是回归损失。u=0 表示 BG 类，因此，只有当我们有任何其他类的边界框时，我们才会增加损耗。此外:***

***![](img/4461a5f77fc7c1f03ff6974493f393f2.png)***

# ***问题:***

***![](img/d7e2510b58d02a137e2fd0634f3195fa.png)***

***Runtime dominated by region proposals!***

# ***快速 RCNN***

***下一个被问到的问题是:网络本身能做区域提议吗？***

> ***直觉是:使用 FastRCNN，我们已经在 CNN 中计算了一个激活图，为什么不通过几个层来运行激活图以找到感兴趣的区域，然后通过预测类+ bbox 坐标来完成向前传递呢？***

***![](img/c2021c19904200d18500a38e0fe8db61.png)***

## ***区域提案网络如何运作？***

***论文中的一个主要观点是锚的概念。**锚点**是固定的边界框，以不同的大小和比例放置在整个图像中，在首次预测对象位置时将用作参考。***

***首先，我们定义图像上的锚点中心。***

***![](img/6215a8765fef4dc6042f32a541e66641.png)***

***Anchor centers throughout the original image***

***在 VGG16 网络的情况下，锚中心相隔 16 px，因为(14×14×512)的最终卷积层以因子 16(224/14)对图像进行子采样。***

***这是主播的样子:***

***![](img/70e70fae8904b30cdedd7fc25df31557.png)***

***Left: Anchors, Center: Anchor for a single point, Right: All anchors***

1.  ***所以我们从一些预定义的区域开始，我们认为我们的对象可能带有锚点。***
2.  ***我们的 RPN 对哪些区域具有对象和对象边界框的偏移进行分类。如果边界框> 0.5 的锚点的 IOU 为 1，否则为 0。***
3.  ***减少区域建议的非最大抑制***
4.  ***基于建议的快速 RCNN 检测网络***

## ***更快的 RCNN 损耗:***

***然后用 4 个损失联合训练整个网络:***

1.  ***RPN 分类对象/非对象***
2.  ***RPN 回归框坐标偏移***
3.  ***最终分类分数(对象类别)***
4.  ***最终框坐标偏移***

# ***结果:***

***![](img/24a0e517dbf00ea789c5b2816d9250bd.png)***

***Final Results on VOC Dataset***

***请随时在 [LinkedIn](https://www.linkedin.com/in/rahulagwl/) 上与我联系，在 [Twitter](https://twitter.com/MLWhiz) / [Medium](https://medium.com/@rahul_agarwal) 上关注我，或者给我发消息征求意见。继续收听，继续学习。***

******免责声明:*** *这是我自己用网上很多博客和幻灯片的投入对这些论文的理解。如果你发现我的理解有问题，请告诉我。我一定会纠正自己，然后发帖。****

*****参考文献:*****

1.  ***[http://cs231n.github.io/transfer-learning/#tf](http://cs231n.github.io/transfer-learning/#tf)***
2.  ***[http://cs 231n . Stanford . edu/slides/2017/cs 231n _ 2017 _ lecture 11 . pdf](http://cs231n.stanford.edu/slides/2017/cs231n_2017_lecture11.pdf)***
3.  ***高效的基于图的图像分割-[http://people.cs.uchicago.edu/~pff/papers/seg-ijcv.pdf](http://people.cs.uchicago.edu/~pff/papers/seg-ijcv.pdf)***
4.  ***用于精确对象检测和语义分割的丰富特征层次(RCNN 论文)-[https://arxiv.org/pdf/1311.2524.pdf](https://arxiv.org/pdf/1311.2524.pdf)***
5.  ***[物体识别的选择性搜索](http://www.huppelen.nl/publications/selectiveSearchDraft.pdf)***
6.  ***【https://deepsense.ai/region-of-interest-pooling-explained/ ***
7.  ***[https://towardsdatascience . com/fasterrcnn-explained-part-1-with-code-599 c 16568 CFF](/fasterrcnn-explained-part-1-with-code-599c16568cff)***
8.  ***[https://stack overflow . com/questions/48163961/how-do-you-do-ROI-pooling-on-areas-small-than-the-target-size](https://stackoverflow.com/questions/48163961/how-do-you-do-roi-pooling-on-areas-smaller-than-the-target-size)***
9.  ***[https://medium . com/@ smallfishbigsea/faster-r-CNN-explained-864d 4 FB 7 E3 f 8](https://medium.com/@smallfishbigsea/faster-r-cnn-explained-864d4fb7e3f8)***
10.  ***[https://tryo labs . com/blog/2018/01/18/fast-r-CNN-down-the-rabbit-hole-of-modern-object-detection/](https://tryolabs.com/blog/2018/01/18/faster-r-cnn-down-the-rabbit-hole-of-modern-object-detection/)***
11.  ***[更快的 R-CNN:利用区域提议网络实现实时目标检测](https://arxiv.org/pdf/1506.01497.pdf)***
12.  ***[https://www . slide share . net/Wenjing Chen 7/deep-learning-for-object-detection](https://www.slideshare.net/WenjingChen7/deep-learning-for-object-detection)***