# 应用深度学习-第 1 部分:人工神经网络

> 原文：<https://towardsdatascience.com/applied-deep-learning-part-1-artificial-neural-networks-d7834f67a4f6?source=collection_archive---------0----------------------->

# 概观

欢迎来到应用深度学习教程系列。我们将从人工神经网络(ANN)开始，特别是前馈神经网络，对几种深度学习技术进行详细分析。本教程与您可以在网上找到的其他教程的不同之处在于，我们将采用一种带有大量代码示例和可视化的实践方法。为了将重点放在应用上，我不会深入这些模型背后的太多数学和理论。

我们将使用 [Keras](https://keras.io/) 深度学习框架，它是 Tensorflow 之上的高级 API。Keras 最近变得超级流行，因为它很简单。建立复杂的模型并快速迭代是非常容易的。我也用了准系统 Tensorflow，实际上很费劲。试用过 Keras 之后，我就不回去了。

这是目录。首先对 ANN 和这些深度模型背后的直觉进行概述。那么我们就从简单的 Logistic 回归开始，主要是为了熟悉 Keras。然后我们将训练深度神经网络，并演示它们如何优于线性模型。我们将比较二元和多类分类数据集上的模型。

1.  [安概述](#04e7)
    1.1)简介
    1.2)直觉
    1.3)推理
2.  [逻辑回归](#fe06)
    2.1)线性可分数据
    2.2)复杂数据-月亮
    2.3)复杂数据-圆
3.  [人工神经网络(ANN)](#106c) 3.1)复数数据-月亮
    3.2)复数数据-圆
    3.3)复数数据-正弦波
4.  [多类分类](#860e) 4.1) Softmax 回归
    4.2)深安
5.  [结论](#0f8f)

这篇文章的代码可以在[这里获得](https://github.com/ardendertat/Applied-Deep-Learning-with-Keras/blob/master/notebooks/Part%201%20-%20Artificial%20Neural%20Networks.ipynb)作为一个 Jupyter 笔记本，请随意下载并亲自试用。

我想你会从这篇文章中学到很多。你不需要有深度学习的先验知识，只需要对一般的机器学习有一些基本的熟悉。所以让我们开始吧…

# 1.人工神经网络概述

## 1.1)简介

人工神经网络(ANN)是多层全连接神经网络，如下图所示。它们由一个输入层、多个隐藏层和一个输出层组成。一层中的每个节点都与下一层中的所有其他节点相连。我们通过增加隐藏层的数量使网络更深。

![](img/23a6e69312383f1fae39cd6c9d8989d3.png)

Figure 1

如果我们放大到一个隐藏或输出节点，我们将看到下图。

![](img/3c0836386f1d6642fee138154d87d30b.png)

Figure 2

给定节点获取其输入的加权和，并将其传递给非线性激活函数。这是节点的输出，然后成为下一层中另一个节点的输入。信号从左向右流动，通过对所有节点执行该过程来计算最终输出。训练这个深度神经网络意味着学习与所有边相关联的权重。

给定节点的等式如下。通过非线性激活函数的输入的加权和。它可以表示为矢量点积，其中 *n* 是节点的输入数。

![](img/03eb38a9ee03ded375c313e1a0b78c61.png)

为了简单起见，我省略了*偏差*项。Bias 是所有节点的输入，其值始终为 1。它允许向左或向右移动激活功能的结果。它还有助于模型在所有输入特征都为 0 时进行训练。如果这听起来很复杂，你可以放心地忽略偏见条款。为了完整起见，上面的等式包括了偏差，如下所示。

![](img/ee7179170a0aa61a8f5ff63813b5fee4.png)

到目前为止，我们已经描述了*正向传递*，这意味着给定一个输入和权重，如何计算输出。训练完成后，我们只运行正向传递来进行预测。但是我们首先需要训练我们的模型来实际学习权重，训练过程如下:

*   随机初始化所有节点的权重。我们将在另一篇文章中探讨一些智能初始化方法。
*   对于每个训练示例，使用当前权重执行向前传递，并从左到右计算每个节点的输出。最终输出是最后一个节点的值。
*   将最终输出与训练数据中的实际目标进行比较，并使用*损失函数测量误差。*
*   从右到左执行*反向传递*，并使用*反向传播*将错误传播到每个节点。计算每个权重对误差的影响，并使用*梯度下降*相应调整权重。从最后一层开始往回传播误差梯度。

梯度下降的反向传播实际上是深度学习模型背后的“魔法”。这是一个相当长的话题，涉及到一些微积分，所以我们不会在这个应用深度学习系列中深入讨论细节。关于梯度下降的详细说明，请参考此处的。反向传播的基本概述可以在[这里](https://ml.berkeley.edu/blog/2017/02/04/tutorial-3/)找到。有关详细的数学处理，请参考此处的[和此处的](http://cs231n.github.io/optimization-2/)和。更多高级优化算法请参考[这里的](http://ruder.io/optimizing-gradient-descent/index.html)。

在标准的 ML 世界中，这种前馈架构被称为*多层感知器*。人工神经网络和感知器的区别在于，人工神经网络使用非线性激活函数，如 *sigmoid* ，而感知器使用阶跃函数。这种非线性赋予了人工神经网络更大的能力。

## 1.2)直觉

已经发生了很多事情，即使是基本的向前传球。现在我们来简化一下，理解一下背后的直觉。

> 本质上，人工神经网络的每一层所做的是输入从一个向量空间到另一个向量空间的非线性变换。

让我们以上面图 1 中的人工神经网络为例。我们有一个对应于 3D 空间中一个矢量的三维输入。然后，我们通过两个隐藏层传递它，每个隐藏层有 4 个节点。并且最终输出是 1D 矢量或标量。

因此，如果我们将此视为一系列矢量变换，我们首先将 3D 输入映射到 4D 矢量空间，然后执行另一个到新 4D 空间的变换，最后的变换将它简化为 1D。这只是一个矩阵乘法链。正向传递执行这些矩阵点积，并将激活函数逐元素应用于结果。下图仅显示了正在使用的权重矩阵(非激活)。

![](img/f28c8b1d52b4afa1d09f7b3da6e24b15.png)

Figure 3

输入向量 *x* 有 1 行 3 列。为了将其转换到 4D 空间，我们需要将其乘以一个 *3x4* 矩阵。然后到另一个 4D 空间，我们乘以一个 *4x4* 矩阵。最后，为了将它简化到 1D 空间，我们使用了一个 4×1 的 T21 矩阵。

请注意矩阵的维度如何表示层的输入和输出维度。具有 3 个节点和 4 个节点的层之间的连接是使用 *3x4* 矩阵的矩阵乘法。

这些矩阵代表定义人工神经网络的权重。为了使用 ANN 对给定的输入进行预测，我们只需要知道这些权重和激活函数(以及偏差)，仅此而已。我们通过反向传播训练人工神经网络来“学习”这些权重。

如果我们把所有东西放在一起，它看起来像下图。

![](img/785c152d0c2c8cf2c0ea4c889eef1ddc.png)

Figure 4

一个 3 个节点和 4 个节点之间的全连通层，就是 *1x3* 输入向量(黄色节点)和 *3x4* 权重矩阵 *W1* 的矩阵相乘。这个点积的结果是一个表示为蓝色节点的 *1x4* 向量。然后我们将这个 *1x4* 向量乘以一个 *4x4* 矩阵 *W2，*得到一个 *1x4* 向量，即绿色节点。最后，使用一个 *4x1* 矩阵 *W3* 得到输出。

为了简单起见，我们在上面的图中省略了激活功能。实际上，在每次矩阵乘法之后，我们将激活函数应用于结果矩阵的每个元素。更正式地说

![](img/6d7f88dd57fc784d23d7beb626e0d155.png)

Equation 2

矩阵乘法的输出通过激活函数 *f* 。对于 sigmoid 函数，这意味着取矩阵中每个元素的 sigmoid。我们可以在方程中更清楚地看到矩阵乘法链。

## 1.3)推理

到目前为止，我们讨论了什么是深度模型以及它们是如何工作的，但是为什么我们首先需要深入呢？

我们看到，人工神经网络层只是对其输入进行从一个向量空间到另一个向量空间的非线性变换。如果我们以一个分类问题为例，我们希望通过绘制一个决策边界来区分各个类别。给定形式的输入数据是不可分离的。通过在每一层执行非线性变换，我们能够将输入投影到新的向量空间，并绘制复杂的决策边界来分离类别。

让我们用一个具体的例子来形象化我们刚刚描述的内容。给定以下数据，我们可以看到它不是线性可分的。

![](img/99bc0eb7e0a74fae32336680476b535b.png)

所以我们通过进行非线性变换，把它投影到一个更高维的空间，然后它就变成线性可分的了。绿色超平面是决策边界。

![](img/58f9af49a4a5a1131eb967f9e194d372.png)

这相当于在原始输入空间中画了一个复杂的决策边界。

![](img/c29565d51ceed1236817fe2a8ddfccd0.png)

> 因此，拥有更深入的模型的主要好处是能够对输入进行更多的非线性转换，并绘制更复杂的决策边界。

总之，人工神经网络是非常灵活而强大的深度学习模型。它们是通用函数逼近器，这意味着它们可以模拟任何复杂的函数。最近，由于几个原因，它们的受欢迎程度出现了令人难以置信的飙升:使训练这些模型成为可能的巧妙技巧，计算能力的巨大增长，特别是 GPU 和分布式训练，以及大量的训练数据。所有这些结合在一起，使得深度学习获得了巨大的牵引力。

这是一个简单的介绍，网上有很多很棒的教程，涵盖了深度神经网络。作为参考，我强烈推荐[这篇论文](http://u.cs.biu.ac.il/~yogo/nnlp.pdf)。这是对深度学习的精彩概述，第 4 部分涵盖了 ANN。另一个很好的参考是[这本书](http://neuralnetworksanddeeplearning.com/chap1.html)，可以在网上找到。

# 2.逻辑回归

尽管名字如此，逻辑回归(LR)是一种二元*分类*算法。这是最流行的 0/1 分类技术。在二维(2D)数据上，LR 将试图画一条直线来区分类别，这就是术语*线性模型*的来源。不过 LR 可以处理任意数量的维度，而不仅仅是二维。对于 3D 数据，它会尝试绘制一个 2D 平面来分隔类别。这推广到 N 维数据和 N-1 维超平面分隔符。如果您有一个监督二进制分类问题，给定一个具有多列的输入数据和一个二进制 0/1 结果，LR 是第一个尝试的方法。在本节中，我们将重点关注 2D 数据，因为它更容易可视化，在另一个教程中，我们将重点关注多维输入。

## 1.1)线性可分离数据

首先让我们从一个简单的例子开始。2D 线性可分数据。我们使用 scikit-learn*make _ classification*方法来生成数据，并使用一个辅助函数来可视化数据。

![](img/2584770979e62ea35f52ff5921c5e2b5.png)

scikit-learn 中提供了一个 *LogisticRegression* 分类器，我在这里就不赘述了，因为我们的目标是学习用 Keras 构建模型。但这里是如何训练一个 LR 模型，使用 *fit* 函数，就像 scikit-learn 中的任何其他模型一样。我们将线性决策边界视为绿线。

![](img/0ea524686380ba588c20e150f9a86e76.png)

正如我们所见，数据是线性可分的。我们现在将使用 Keras 训练相同的逻辑回归模型来预测每个输入点的类成员。为了简单起见，我们现在不执行将数据分离到训练集和测试集的标准实践，或者执行 k-fold 交叉验证。

Keras 有很棒的[文档](https://keras.io/)，查看它的 API 的更详细描述。这是训练模型的代码，下面我们一步一步来看。

我们将使用*顺序*模型 API，此处[可用](https://keras.io/getting-started/sequential-model-guide/)。顺序模型允许我们通过一层一层地堆叠来建立深度神经网络。由于我们现在正在构建一个简单的逻辑回归模型，我们将输入节点直接连接到输出节点，没有任何隐藏层。注意，LR 模型具有形式 *y=f(xW)* ，其中 *f* 是 sigmoid 函数。将单个输出层直接连接到输入反映了这一功能。

快速澄清以消除所用术语的歧义。在神经网络文献中，经常谈到输入节点和输出节点。乍一看，这听起来可能很奇怪，输入“节点”本身是什么？当我们说输入节点时，我们谈论的是给定训练示例的特征。在我们的例子中，我们有 2 个特征，即我们上面绘制的点的 x 和 y 坐标，因此我们有 2 个输入节点。你可以简单地把它想成两个数的向量。那么输出节点呢？逻辑回归模型的输出是一个单一数字，即属于类别 1 的输入数据点的概率。换句话说 *P(class=1)* 。属于类别 0 的输入点的概率为 *P(类别= 0)= 1p(类别=1)* 。因此，您可以简单地将输出节点视为一个 0 到 1 之间的单个数字(或简单的标量)的向量。

在 Keras 中，我们不添加对应于输入节点的层，我们只添加隐藏和输出节点。在我们当前的模型中，我们没有任何隐藏层，输入节点直接连接到输出节点。这意味着我们在 Keras 中的神经网络定义将只有一个层和一个节点，对应于输出节点。

```
model = Sequential()
model.add(Dense(units=1, input_shape=(2,), activation='sigmoid'))
```

Keras 中的 *Dense* 函数构建了一个完全连接的神经网络层，自动将权重初始化为偏差。这是一个超级有用的功能，你会看到它在任何地方都被使用。函数参数定义如下:

*   *单位*:第一个参数，表示该层的节点数。由于我们正在构建输出层，并且我们说它只有一个节点，因此该值为 1。
*   *input _ shape*:Keras 模型中的第一层需要指定输入尺寸。后续层(这里没有，但我们将在后面的部分中)不需要指定此参数，因为 Keras 可以自动推断尺寸。在这种情况下，我们的输入维度是 2，x 和 y 坐标。input_shape 参数需要一个向量，所以在我们的例子中，它是一个只有一个数字的元组。
*   *激活*:逻辑回归模型的激活函数是*逻辑*函数，或者称为 *sigmoid* 。我们将在另一个教程中探索不同的激活函数，在哪里使用它们以及为什么使用它们。

```
model.compile(optimizer='adam', loss='binary_crossentropy', metrics=['accuracy'])
```

然后我们用*编译*函数编译这个模型。这通过指定学习过程的细节来创建神经网络模型。模型还没有训练好。现在我们只是声明要使用的优化器和要最小化的损失函数。编译函数的参数定义如下:

*   *优化器*:使用哪个优化器来最小化损失函数。有很多不同的优化器，大多数都是基于梯度下降的。我们将在另一个教程中探索不同的优化器。现在我们将使用 *adam* 优化器，这是默认情况下人们更喜欢使用的一种。
*   *损失*:最小化的损失函数。由于我们正在构建一个二进制 0/1 分类器，要最小化的损失函数是 *binary_crossentropy* 。我们将在后面的章节中看到损失函数的其他例子。
*   *指标*:报告统计数据的指标，对于分类问题，我们将其设置为*准确度*。

```
history = model.fit(x=X, y=y, verbose=0, epochs=50)
```

现在有趣的部分来了，使用 *fit* 函数实际训练模型。论据如下:

*   *x* :输入数据，我们定义为上面的 *X* 。它包含输入点的 x 和 y 坐标
*   *y* :不要与输入点的 y 坐标混淆。在所有的 ML 教程中, *y* 指的是标签，在我们的例子中是我们试图预测的类:0 或 1。
*   *verbose* :打印出损耗和精度，设置为 1 查看输出。
*   *次数*:查看整个训练数据的次数。当训练模型时，我们不止一次而是多次传递训练数据。

```
plot_loss_accuracy(history)
```

拟合方法的输出是每个时期的损失和精度。然后，我们使用自定义函数绘制它，可以看到损耗随着时间的推移下降到几乎为 0，而精度上升到几乎为 1。太好了！我们已经成功地用 Keras 训练了我们的第一个神经网络模型。我知道这是一个很长的解释，但我想第一次详细解释我们在做什么。一旦你明白发生了什么，并练习几次，所有这些都成为第二天性。

![](img/23cb68348bc30fdeceec943384a5b966.png)

下面是一个决策边界图。蓝色和红色的不同阴影表示该区域中某个假想点属于 1 级或 0 级的概率。左上区域被分类为 1 类，颜色为蓝色。右下方区域被分类为 0 类，颜色为红色。在决策边界附近有一个过渡。这是可视化模型正在学习的决策边界的一种很酷的方式。

![](img/42f4de4efed35eb04bbf165e5937300f.png)

*分类报告*显示了我们模型的精度和召回率。我们接近 100%的准确率。报告中显示的值应该是 0.997，但它被四舍五入为 1.0。

![](img/aae1fa8b86545ff97ba8d9f996c47e1f.png)

*混淆矩阵*向我们展示了有多少类被正确分类和错误分类。对角线轴上的数字代表正确分类的点的数量，其余的是错误分类的点。这个特殊的矩阵不是很有趣，因为模型只错误分类了 3 个点。我们可以在混淆矩阵的右上角看到一个错误分类的点，真实值为 0 类，但预测值为 1 类。

![](img/d4f278d4c7087f1609e84557976e138a.png)

## 2.2)复杂数据-卫星

以前的数据集是线性可分的，所以我们的逻辑回归模型分离类是微不足道的。这是一个更复杂的数据集，它不是线性可分的。简单的逻辑回归模型无法清楚地区分这些类别。我们使用 scikit-learn 的 *make_moons* 方法来生成数据。

![](img/add9161535a3414a609d1a444b529eae.png)

让我们建立另一个逻辑回归模型，使用与之前相同的参数。在这个数据集上，我们得到了 86%的准确率。

![](img/c28cf09d435bad18b4f9abdba2b553d3.png)

当前的决策边界看起来不像以前那样清晰。该模型试图从中间分离出类，但有许多错误分类的点。我们需要一个具有非线性决策边界的更复杂的分类器，我们很快就会看到这样的例子。

![](img/a95eb93c8353f836c681921ac6806742.png)

模型的精度为 86%。它在纸上看起来不错，但我们应该很容易用更复杂的模型得到 100%。您可以想象一个弯曲的决策边界来分隔这些类，一个复杂的模型应该能够近似这一点。

分类报告和混淆矩阵如下所示。

![](img/f6c8d888e6b64297276ab972aabaa911.png)

## 2.3 复杂数据圈

让我们看最后一个例子，线性模型会失败。这次使用 scikit-learn 中的 *make_circles* 功能。

![](img/14b18935e776236164482aac9fe84e73.png)

用相同的参数建立模型。

![](img/a98768f201374551d8fff1758ff9d95c.png)

决策边界再次从数据的中间通过，但是现在我们有更多的错误分类的点。

![](img/263348e0a8dccf464087b72e9e67590f.png)

准确率在 50%左右，如下图。由于数据集的性质，无论模型在哪里画线，都会对一半的点进行错误分类。

我们在这里看到的混淆矩阵是一个属于差分类器的例子。理想情况下，我们更希望混淆矩阵看起来像上面看到的那样。对角线上的高数字表示分类器是正确的，而其他地方的低数字表示分类器是错误的。在我们的视觉中，蓝色代表大的数字，黄色代表小的数字。所以我们更喜欢在对角线上看到蓝色，在其他地方看到黄色。到处都是蓝色是不好的迹象，意味着我们的分类器是混乱的。

![](img/99608d5daa7535a0bc0fb92607cf7705.png)

无论输入是什么，总是预测 1 的最简单的方法将获得 50%的准确性。我们的模型也得到了 50%的准确率，所以一点用都没有。

# 3.人工神经网络

现在，我们将训练一个深度人工神经网络(ANN ),以更好地对逻辑回归模型难以处理的数据集，月亮和圆进行分类。我们还将对一个更难的正弦波数据集进行分类，以证明人工神经网络可以形成真正复杂的决策边界。

## 3.1)复杂数据-卫星

在为上述逻辑回归构建 Keras 模型时，我们执行了以下步骤:

*   步骤 1:定义一个顺序模型。
*   第二步:添加具有乙状结肠激活功能的致密层。这是我们需要的唯一一层。
*   步骤 3:用优化器和损失函数编译模型。
*   步骤 4:使模型适合数据集。
*   第五步:分析结果:绘制损失/准确性曲线，绘制决策边界，查看分类报告，了解混淆矩阵。

在构建深度神经网络时，我们只需要改变步骤 2，这样，我们将一个接一个地添加几个密集层。一层的输出成为下一层的输入。Keras 再次通过初始化权重和偏差来完成大部分繁重的工作，并将一层的输出连接到下一层的输入。我们只需要指定在给定的层中需要多少个节点，以及激活函数。就这么简单。

我们首先添加一个有 4 个节点和 *tanh* 激活函数的层。Tanh 是一个常用的激活函数，我们将在另一个教程中了解更多。然后，我们添加另一个层 2 节点再次使用双曲正切激活。我们最后添加最后一层 1 节点和 sigmoid 激活。这是我们在逻辑回归模型中使用的最后一层。

这不是一个很深的人工神经网络，它只有 3 层:2 个隐藏层，和输出层。但是请注意几个模式:

*   输出图层仍然使用 sigmoid 激活函数，因为我们正在处理二元分类问题。
*   隐藏层使用 tanh 激活功能。如果我们添加更多的隐藏层，他们也会使用 tanh 激活。我们有几个激活函数的选项:sigmoid、tanh、relu 和 relu 的变体。在另一篇文章中，我们将探讨每种方法的优缺点。我们还将演示为什么在隐藏层中使用 sigmoid 激活是一个坏主意。现在使用 tanh 是安全的。
*   我们在每个后续层中的节点数量更少。当我们将层堆叠在另一层之上时，通常会有较少的节点，有点像三角形。

我们没有在这里构建一个非常深的 ANN，因为这是不必要的。通过这种配置，我们已经实现了 100%的准确性。

![](img/1a4abc3081e9b3a5ab72f960632ba088.png)

人工神经网络能够提出一个完美的分隔符来区分类别。

![](img/c0ec5ed5f8fbf9ab2b938f1d3b519fd8.png)

100%精确，没有错误分类。

![](img/69d5e81cee2ba52655b3fa86195a6a1d.png)

## 3.2)复杂的数据圈

现在让我们看看 Circles 数据集，其中 LR 模型仅达到 50%的准确率。模型同上，我们只使用当前数据集更改拟合函数的输入。我们再次达到了 100%的准确率。

![](img/86c4c146463eb85391353c3b3281ebe2.png)

类似地，决策边界看起来就像我们自己手绘的一样。人工神经网络能够找出最佳分离器。

![](img/a3f3a7968ba5bee374d0aa65191a8720.png)

就像上面一样，我们得到了 100%的准确性。

![](img/ae14b4a4bc4c6ee94fa9ebfd3c9674b5.png)

## 4.3)复杂数据-正弦波

让我们试着分类一个最终的玩具数据集。在前面的部分中，类可以通过一个连续的决策边界来分离。边界具有复杂的形状，它不是线性的，但是一个连续的判定边界就足够了。人工神经网络可以绘制任意数量的复杂决策边界，我们将演示这一点。

让我们创建一个正弦数据集，看起来像正弦函数，每个上下属于一个交替类。正如我们在图中看到的，一个单一的决策边界不能将类分开。我们需要一系列非线性分离器。

![](img/fa4c992bc5329995dc1871de28124760.png)

现在我们需要一个更复杂的模型来进行准确的分类。所以我们有 3 个隐藏层，和一个输出层。每层的节点数也有所增加，以提高模型的学习能力。选择正确的隐藏层和每层节点数是一门艺术，而不是科学，通常通过反复试验来决定。

![](img/eeed4f7e591d15ed275ef1b607c36dfa.png)

人工神经网络能够模拟一组相当复杂的决策界限。

![](img/1e1a484b55b5651ee87a7fc01af133ea.png)

精确度是 99%，我们只有 2400 个错误分类点中的 14 个。非常好。

![](img/464ae98500c835020d60c41532bd0290.png)

# 4.多类分类

在前面的章节中，我们研究了二元分类。现在，我们将研究一个多类分类问题，其中类的数量超过 2。我们将挑选 3 个类进行演示，但是我们的方法可以推广到任意数量的类。

这是我们的数据集的样子，螺旋数据有 3 个类，使用 scikit-learn 中的 *make_multiclass* 方法。

![](img/fa0c60bfc5b59d23a4846d4ac58916b6.png)

## 4.1) Softmax 回归

正如我们在上面看到的，逻辑回归(LR)是两类的分类方法。它适用于二进制标签 0/1。Softmax 回归(SR)是 LR 的一种推广，其中我们可以有两个以上的类。在我们当前的数据集中，我们有 3 个类，表示为 0/1/2。

为 SR 构建模型与 LR 非常相似，下面是我们如何构建逻辑回归模型的参考。

这就是我们将如何建立 Softmax 回归模型。

有几个不同之处，让我们一个一个来看:

*   密集层中的节点数:LR 使用 1 个节点，而 SR 有 3 个节点。因为我们有 3 个类，所以 SR 使用 3 个节点是有意义的。那么问题是，为什么 LR 只使用 1 个节点，它有 2 个类，所以看起来我们应该使用 2 个节点。答案是肯定的，因为我们只用一个节点就可以达到同样的效果。正如我们在上面看到的，LR 对属于第一类的例子的概率建模: *P(class=1)* 。我们可以通过*1p(class = 1)*计算 0 类概率。但是当我们有两个以上的类时，我们需要每个类有单独的节点。因为知道一类的概率并不能让我们推断出其他类的概率。
*   激活功能:LR 使用了 sigmoid 激活功能，SR 使用了 *softmax* 。Softmax 缩放输出节点的值，以便它们表示概率并且总和为 1。所以在我们的例子中*P(class = 0)+P(class = 1)+P(class = 2)= 1*。它不是用简单的方法，用个体概率除以总和，而是用指数函数。因此，较高的值得到更多的强调，较低的值受到更多的压制。我们将在另一个教程中详细讨论 softmax 的功能。现在，你可以简单地把它想成一个归一化函数，让我们把输出值解释成概率。
*   损失函数:在 LR 这样的二元分类问题中，损失函数是 binary_crossentropy。在多类情况下，损失函数是分类交叉熵。范畴交叉熵是二元交叉熵在两个以上类别上的推广。深入损失函数背后的理论超出了本教程的范围。但是现在只知道这个属性就足够了。
*   Fit 函数:LR 在 fit 函数中直接使用了向量 y，该函数只有一列值为 0/1。当我们进行 SR 时，标签需要以*一键*表示。在我们的例子中，y_cat 是一个有 3 列的矩阵，其中所有的值都是 0，除了代表我们的类的值是 1。

花了一些时间来谈论 LR 和 SR 之间的所有差异，看起来有很多东西需要消化。但是再一次经过一些练习后，这将成为一种习惯，你甚至不需要考虑这些。

在所有这些理论之后，让我们后退一步，记住 LR 是一个线性分类器。SR 也是一个线性分类器，但是针对多个类。所以模型的“力”没有变，还是线性模型。我们只是推广了 LR，将其应用于多类问题。

训练模型给了我们 50%左右的准确率。无论输入是什么，总是预测类别 1 的最简单的方法将具有 33%的准确度。SR 模型并没有太大的改进。这是意料之中的，因为数据集不是线性可分的。

![](img/eb929fc80140983d84c9220df764816b.png)

查看决策边界可以确认我们仍然有一个线性分类器。由于浮点舍入，这些线看起来参差不齐，但实际上它们是直的。

![](img/b1c56a53ce9e94fcf8293602503b37aa.png)

这是对应于 3 个类的精度和召回率。混乱矩阵到处都是。显然，这不是一个最佳的分类器。

![](img/b2d019982e9f9f9663054e5064999d11.png)

## 4.2)深安

现在让我们为多类分类建立一个深度人工神经网络。记住从 LR 到 ANN 的变化是很小的。我们只需要增加更密集的层。我们将再次这样做。添加几个具有 tanh 激活功能的致密层。

请注意，输出层仍有 3 个节点，并使用 softmax 激活。损失函数也没变，还是分类 _ 交叉熵。从线性模型到深度人工神经网络，这些不会改变，因为问题的定义没有改变。我们仍在研究多类分类。但是现在使用一个更强大的模型，这种力量来自于给我们的神经网络增加更多的层。

我们在几个时期内就达到了 99%的准确率。

![](img/5c6cc69c752480c6c46861dd034d3d5f.png)

决策边界是非线性的。

![](img/0eba78f1e9ed3d3ddaeab7cb35405f37.png)

我们得到了几乎 100%的准确率。1500 分中我们完全错分了 5 分。

![](img/518eb31618f6ee4babfc7f934fa3b8d5.png)

# 5)结论

感谢您花时间阅读这篇文章，我知道这是一篇关于人工神经网络和 Keras 的相当长的教程。我想尽可能详细，同时保持文章长度可控。我希望你喜欢它。

这篇文章中有一个共同的主题，即我们首先介绍这个任务，然后使用一个简单的方法来完成它，并观察它的局限性。后来我们用一个更复杂的深度模型对它进行了改进，得到了更好的结果。我认为顺序很重要。没有一个复杂的方法是成功的，除非它是从一个简单的模型发展而来。

如果你想自己动手，这篇文章的全部代码可以在这里找到。如果你有任何反馈，请随时通过[推特](https://twitter.com/ardendertat)联系我。