# 卷积神经网络的视觉可解释性

> 原文：<https://towardsdatascience.com/visual-interpretability-for-convolutional-neural-networks-2453856210ce?source=collection_archive---------3----------------------->

## 通信网中的可视化技术介绍

![](img/01bb7ae99b305b001355764e6672d152.png)

Photo by [Shane Aldendorff](https://unsplash.com/@pluyar?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

**更新:**我已经在 PyData meetup 的一次演讲中详细介绍了这篇博文。可以看下面的 YouTube 视频。

在应用程序编程中，我们有调试和错误检查语句，如 print、assert、try-catch 等。但是当涉及到深度神经网络时，调试变得有点棘手。幸运的是，卷积神经网络(ConvNets 或 CNN)具有人类可以视觉解读的输入(图像)，因此我们有各种技术来理解它们如何工作，它们学习什么以及它们为什么以给定的方式工作。而对于其他深度神经网络架构，可视化甚至更加困难。尽管如此，可视化神经网络给了我们关于神经网络世界的良好直觉。在这篇文章中，我们将深入研究 convnet，了解图像分类器如何工作，以及当我们将 224x224x3 的图像输入到 conv net 时会发生什么。

具体来说，我们将尝试各种可视化技术来了解:

1.  每次卷积操作(可视化中间激活)后的输出是什么
2.  每个过滤器从输入图像中提取什么特征(可视化过滤器/内核)
3.  如何将每个卷积层的输出追溯到输入图像(使用解卷积网络**又名**解卷积/转置卷积进行可视化)
4.  为 convnet 的每个图层在输入图像中可视化感兴趣区域(可视化热图)

> “尽管取得了这么多进展，但人工智能领域的几乎所有重要问题似乎都没有答案。许多人甚至还没有被正式问过。”—弗朗索瓦·乔莱

## VGG16 卷积神经网络

本文假设读者对 convnets 中的前向传播及其架构组件(如卷积、池和激活函数)有基本的了解。在整篇文章中，我们将使用 VGG16 模型进行可视化。每个可视化的 Jupyter 笔记本都是用 Keras 编写的，可以在我的 GitHub 存储库中找到:

[](https://github.com/himanshurawlani/convnet-interpretability-keras) [## himanshurawlani/conv net-可解释性-keras

### 使用 Keras-himanshurawlani/conv net-可解释性-keras 可视化 VGG16 卷积神经网络

github.com](https://github.com/himanshurawlani/convnet-interpretability-keras) 

让我们看看 VGG16 模型是什么样的:

![](img/8152c739e21a00872cb0fbcfd297941b.png)

VGG16 convolutional neural network

每个卷积块的输出通过一个激活函数(本例中为 ReLU)传递。

## 可视化中间激活

在这种技术中，给定一幅输入图像，我们将简单地绘制出在每一层中卷积运算后每个滤波器提取的内容(输出特征)。例如，在 VGG16 中，输入层尺寸为 224x224x3，第一次卷积运算后的输出尺寸为 224x224x64(参见`block1_conv1`)。这里，64 是用于在第一次卷积运算后提取输入特征的滤波器数量，因此我们将只绘制这 64 个 224x224 输出。

![](img/b56a72ef39388a83e9b45a6fe1987113.png)![](img/9f9c4f980a8204daf176ba232bdca7d0.png)![](img/4c7fc726d95e84d07dd71e54057c7c55.png)![](img/2497c966a48c5877c4c96f128db791c0.png)![](img/53adaa7f76acda90b959927597feb449.png)![](img/7629912486f33e5616f1493bcf141ec4.png)![](img/bbfbf2986500c9eaad25880da4785161.png)![](img/960ed56c2f2b959d8b486bdcb18c50db.png)![](img/d85c679e0568b67a5f16cf9201ac27dd.png)![](img/fe4a4226e8f135931bd342b7e1fd733e.png)![](img/ae3a0b912eb50f040ef753ee9a73cadf.png)![](img/ff051f3bdcbf51d6650af3abea3e6c9c.png)![](img/9df01382590dc8abbc362f3b55682af9.png)![](img/f10dc63c79e842b31ed7dbc9c5dac6e6.png)![](img/7f0cb93afe537ad0063d6dbb039c91a8.png)![](img/e81169da760e31a0b546a7b6443bb723.png)![](img/e0a26d4f2463c39fff8afcc985d3e10a.png)![](img/e7b81610bfd2edff3da819126bc6bf5f.png)![](img/b7f3565a6ef9d79a4f4bb82578754f57.png)

Visualizing the output of convolution operation after each layer of VGG16 network

**释义:**

1.  初始层(`block1`和`block2`)保留了大部分输入图像特征。看起来卷积滤波器在输入图像的每个部分都被激活。这给了我们一种直觉，即这些初始滤波器可能是原始的边缘检测器(因为我们可以认为一个复杂的图形是由不同方向的小边缘组合而成的。)
2.  随着我们越来越深入(`block3`和`block4`)，过滤器提取的特征在视觉上变得越来越难以解释。对此的直觉是，convnet 现在正在提取输入图像的视觉信息，并试图将其转换到所需的输出分类域。
3.  在`block5`(尤其是`block5_conv3`)中，我们看到很多空白的卷积输出。这意味着在输入图像中找不到由过滤器编码的图案。最有可能的是，这些图案必须是不存在于该输入图像中的复杂形状。

为了详细说明第 2 点和第 3 点，我们可以将这些见解与我们自己的视觉感知进行比较:当我们看一个物体(比如自行车)时，我们不会坐下来观察该物体的每个细节(比如把手、挡泥板、车轮防滑钉等)。).我们所看到的是一个有两个轮子的物体，轮子由一根金属棒连接着。因此，如果我们被告知要画一辆自行车，这将是一个简单的草图，仅仅传达了两个轮子和一个金属杆的想法。这些信息足以让我们决定给定的对象是一辆自行车。

类似的事情也发生在深层神经网络中。它们充当信息提取管道，将输入图像转换到视觉上不太容易理解(通过移除不相关的信息)但数学上对 convnet 从其最后一层的输出类中做出选择有用的域。

## 可视化过滤器

我们上面看到的可视化是卷积运算的输出。卷积运算最基本的术语是滤波器/内核和输入图像之间的相关性。与给定输入区域最匹配的滤波器将产生幅度更高的输出(与来自其他滤波器的输出相比)。通过可视化过滤器，我们可以了解每一层已经学会从输入中提取什么模式。

我们首先将一些随机噪声作为输入图像，并使其通过 convnet。我们获取给定层的输出，我们希望可视化该层的过滤器，并找到该层中每个过滤器的平均值。这个寻找每个滤波器的平均值的步骤形成了我们的损失函数。例如，`block1_conv1`的输入为 224x224x3，输出为 224x224x64，因此它有 64 个过滤器。这里，我们沿着 x 和 y 方向找到每个 224x224 滤波器的平均值，并且我们对所有 64 个滤波器都这样做。在每一步中，我们采用一个滤波器，计算其平均值(即损失)并更新我们的输入图像，使得该滤波器的输出最大化。我们使用梯度上升来更新我们的输入图像，以便最大化损失(特定滤波器的响应)。所得的输入图像将是所选滤波器最大程度响应的图像。

![](img/3249b5f31cf4750da92e95a527e6791c.png)![](img/8a9853dbfe64411b72e8cbad53f69001.png)![](img/59ec3b5a4ebd0511af71a849926ba806.png)![](img/10709b28452abfa80be192922900a2e4.png)![](img/81b3e7f7271fb81420d2de3cb15fe5fc.png)![](img/5bcb9d3e37d4758c1b46fe7d7bdfd961.png)![](img/259353ef545f985814e1511f5e83695b.png)![](img/41e9663a0c4ecc0a99447d3fb10927d1.png)![](img/0fd419347f7772e9af6a4bd5e7502ba8.png)![](img/f14941416226cb1a034ecc041f7512d9.png)![](img/8ca1a99d7f9696e8e1726063743fd32e.png)![](img/24c3226ea70edeac613da9d96d418277.png)![](img/a77697f8808ffd0590ac2774a4f8c494.png)

Visualizing the filters/kernels of each layer of VGG16 network

**释义:**

1.  convnet 中的每一层都简单地学习一组滤波器，使得它们的输入可以表示为这些滤波器的组合。初始图层(`block1`和`block2`)滤镜看起来像是简单的方向性边缘和颜色(或者在某些情况下是彩色边缘)的集合。这是我们在之前的视觉化中的第一个观察的原因。
2.  随着我们深入(`block4`和`block5`)，我们看到更多复杂的形状被过滤器编码。这些过滤器可能有助于提取眼睛、鼻子、耳朵等。随着形状变得越来越复杂，它们出现在给定输入图像中的机会越来越少。这就是我们在之前的可视化中的第三个观察的原因。
3.  许多这样的滤波器是相同的，但是旋转了一些非随机的因子(通常为 90 度)。这意味着，通过找到一种使卷积滤波器旋转不变的方法，我们有可能大幅压缩 convnet 中使用的滤波器数量。

这种经过训练的模型的可视化不仅可以洞察其操作，而且有助于诊断和选择更好的架构。ZF 网(2013) [1]的论文使用这种技术来可视化 AlexNet (2012) [2]架构的第一层和第二层过滤器。他们发现，第一层过滤器是极高和极低频率信息的混合，很少覆盖中频(图像中的低频意味着像素值在空间中缓慢变化，而高频内容意味着像素值在空间中快速变化)。此外，第二层可视化显示了由第一层卷积中使用的大步幅引起的混叠伪像。

他们通过将第一层滤波器大小从 11x11 减小到 7x7 来解决这些问题，并将卷积的步长从 4 改为 2。新架构在第一层和第二层功能中保留了更多信息。更重要的是，它还提高了分类性能。

## 使用 deconvnets 可视化

解进化网络(deconvnet)，是由泽勒等人提出的，2011 [3]。deconvnet 可被视为使用相同组件(过滤、池化)的 convnet 模型，但方向相反，因此 deconvnet 不是将像素映射到要素，而是将要素激活(卷积输出)投影回输入像素空间。

![](img/af43e2992ec95dd789ca2fbabdd124b9.png)

A deconvnet layer (left) attached to a convnet layer (right). [1]

为了可视化 convnet，deconvnet 被附加到它的每个层，提供返回图像像素的连续路径。首先，将输入图像呈现给 convnet，并计算各层的特征。为了检查给定的 convnet 激活，我们将该层中的所有其他激活设置为零，并将特征映射作为输入传递给附加的 deconvnet 层。然后，我们连续地(I)取消 pool，(ii)校正和(iii)过滤，以重建引起所选激活的下层中的活动。然后重复这一过程，直到到达输入像素空间。参见 ZF 网论文[1]的第 2.1 节，了解更多关于未冷却、精馏和过滤的详细信息。

对于给定的层，我们取具有最高激活(幅度)的前 4 个滤波器的输出。我们使用类似的函数来计算激活幅度，就像我们对可视化过滤器所做的那样(损失函数)。在那里，我们计算所有维度的平均值，但在这里，我们计算所有维度的总和，并按降序对过滤器进行排序。例如，对于给定的输入图像`block1_conv1`，要激活前 4 个滤波器，我们找到沿 x 和 y 方向的所有 64 个 224x224 滤波器的输出之和，按降序对它们进行排序，并将前 4 个输出馈送到我们的 deconvnet。这给了我们由这些滤波器看到的输入图像的 4 种不同变化。

![](img/db86223255aa374dea8f2b762ee699b1.png)![](img/dac21460aff95ce02c33310619e0b99a.png)![](img/8c7ec7082500448a6129b0ab3afb823e.png)![](img/27f7f7062ba38c8abea1611bf34a0800.png)![](img/30144ba805dc4e2bf7f3be1e0e7ac85e.png)![](img/6b81f92f7775038e6d108370eea9ab82.png)![](img/320654500fe43e614c215e66b1513906.png)![](img/2a8dbd85ed7de3067e29b6b77f86ad1a.png)![](img/efa58304df088a15a18a4b1e0b95f9a7.png)![](img/a45ebd2893c4de0dce289b0651c1357a.png)![](img/421b8b26f0ab902a99b09e41b68f1aaa.png)![](img/9b0af22aea8ae5044b26958b1910b5be.png)![](img/620ede1583b03839e11c1892537082c4.png)![](img/591a99c592f2a2942b27536f1f97a88c.png)

**释义:**

1.  每个图层的投影显示了网络中要素的等级性质。
2.  `block1`响应角点和其他边缘/颜色连接。`block2`和`block3`具有更复杂的不变性，捕捉相似的纹理(例如网格图案)。
3.  `block4`尝试定位图像的部分，如眼睛、鼻子、嘴等。`block5`显示整个局部人脸。

将特征激活投影回输入空间，让我们类似地了解这些层中的过滤器是在什么情况下被激活的。我们看到一些投影具有突出的水平边缘，一些具有突出的垂直边缘。一些投影覆盖眼睛，鼻子，嘴，一些覆盖整个脸。将每个顶部激活分别向下投影到像素空间揭示了输入图像中激发给定特征图的不同结构。

## **可视化热图**

在这项技术中，我们使用 GradCAM 并试图了解输入图像的哪些部分导致了 convnet 做出特定的分类决策。该技术的一般类别被称为“类激活图”(CAM)可视化，包括在输入图像上产生“类激活”的热图。“类别激活”热图是与特定输出类别相关联的分数的 2D 网格，针对输入图像中的每个位置计算，指示每个位置相对于所考虑的类别有多重要。这有助于我们调试卷积神经网络的决策过程。我们也可以使用相同的技术来定位图像中的特定对象。看看这个演示:[http://gradcam.cloudcv.org/](http://gradcam.cloudcv.org/)

在 GradCAM 中，我们采用给定输入图像的卷积层的输出，并通过预测类的梯度对该输出中的每个通道进行加权。假设我们给 convnet 输入了一些图像，得到了“大象”作为预测。最后一个预测层可以被认为是具有一些输入的函数(在这种情况下是`block5_conv3`的输出)。所以，如果`f`是预测层，`x`是`block5_conv3`的输出，那么预测层的输出可以被给定为`y`，其中`y=f(x)`。同样的，我们可以证明`block5_conv3`是一个有一些输入的函数`g`(本例中是`block5_conv2`的输出)。让我们把`block5_conv2`的输出看作`z`然后`x`(可以把`block5_conv3)`的输出看作`x=g(z)`等等…

使用上述类比，我们可以将具有输入的预测层的输出视为其所有先前层(以嵌套方式)。我们对某个先前层(我们希望热图可视化的层)中的每个通道的预测输出 w.r.t .进行偏导数，这给出了每个通道对预测输出的重要性。然后，我们将每个通道与其对应的梯度(重要性)相乘，以对负责预测输出的每个通道进行加权，并计算通道平均值，以获得该层的热图。

![](img/9ad57486eb5289e94e463b4ee3539f92.png)![](img/37be9c2af03436f37a09d02dc1bcf86e.png)![](img/4fa2b43a0bbdab970cf50543518f57de.png)![](img/0678b3b4d11efb544242028511d23d90.png)![](img/3ec3dd6b3a30e29b0d6dc52f7d81b0b5.png)![](img/6a3beaa35abf885c06abab235a8bad4d.png)![](img/3a7368b7c73f8569b9248684cfeb3e77.png)![](img/04767510576b9b1334785cf9ccf3d9c1.png)![](img/5cbaae6fdeed0c27f23247295a25da6d.png)![](img/90ecf626e62120ae1a40a0bf69e2cc40.png)![](img/ae5840b2eb44b43abe9033514b034c93.png)![](img/520fa5d2622cc3e26755cac218ba156d.png)![](img/ac9f992397069fa7c070122be7a67694.png)![](img/487d0eb52283c01428bf56211e1ef138.png)

Heatmap visualization using GradCAM technique for each layer of VGG16 network

**释义:**

1.  VGG16 网络只是一个图像分类器，能够在输入图像中定位大象。
2.  该网络以 90%的准确率预测了输入图像中的“非洲象”,主要是因为小象的面部和耳朵部分(见`block5_conv3`)。这大概就是网络如何区分非洲象和印度象的。

当我们看到不同层的热图时，我们看到图像的不同部分被激活。这是因为早期的图层有滤镜，可以看到图像的不同部分。但是类别预测很大程度上取决于预测之前层的激活，因此，可视化预测层之前层的热图(在这种情况下为`block5_conv3`)告诉我们输入图像的哪个部分导致了特定的预测。

## **结论**

*   这些可视化技术不仅告诉我们模型已经学会在图像中识别什么，还告诉我们它的性能可以在哪里改进。它为我们提供了关于模型可解释性的有用见解，因此我们可以放心地在现实世界的用例中部署这些模型。
*   令人惊讶的是，我们不仅可以重新训练分类器来识别图像中的对象，还可以使用 GradCAM 可视化技术来近似定位它们。看看这个演示:https://www.youtube.com/watch?v=COjUB9Izk6E
*   在一天结束时，由 convnets 学习的所有参数和过滤器的目标是最小化给定训练数据的成本函数。这个成本函数和训练数据定义了我们的 convnet 的能力。

## 进一步阅读

1.  可解释性的基石:[https://distill.pub/2018/building-blocks/](https://distill.pub/2018/building-blocks/)
2.  通过深度可视化理解神经网络:[http://yosinski.com/deepvis](http://yosinski.com/deepvis)
3.  想象一下康夫尼特学到了什么:[http://cs231n.github.io/understanding-cnn/](http://cs231n.github.io/understanding-cnn/)
4.  跟踪网络中发生的事情的有用工具箱:[https://cs.stanford.edu/people/karpathy/convnetjs/](https://cs.stanford.edu/people/karpathy/convnetjs/)

## 参考

[1]可视化和理解卷积网络，[https://arxiv.org/abs/1311.2901v3](https://arxiv.org/abs/1311.2901v3)

[2]使用深度卷积神经网络的 ImageNet 分类，[https://papers . nips . cc/paper/4824-ImageNet-class ification-with-Deep-convolutionary-Neural-Networks](https://papers.nips.cc/paper/4824-imagenet-classification-with-deep-convolutional-neural-networks)

[3]用于中高级特征学习的自适应去进化网络，【https://ieeexplore.ieee.org/document/6126474 

[4]可视化 convnets 学习的内容，[https://github . com/fchollet/deep-learning-with-python-notebooks/blob/master/5.4-Visualizing-what-conv nets-learn . ipynb](https://github.com/fchollet/deep-learning-with-python-notebooks/blob/master/5.4-visualizing-what-convnets-learn.ipynb)

[5]卷积神经网络如何看待世界，[https://blog . keras . io/How-convolutionary-neural-networks-see-the-world . html](https://blog.keras.io/how-convolutional-neural-networks-see-the-world.html)