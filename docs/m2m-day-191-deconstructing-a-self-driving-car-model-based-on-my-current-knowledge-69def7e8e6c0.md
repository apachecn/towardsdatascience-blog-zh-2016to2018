# M2M 第 191 天:解构自动驾驶汽车模型(基于我目前的知识)

> 原文：<https://towardsdatascience.com/m2m-day-191-deconstructing-a-self-driving-car-model-based-on-my-current-knowledge-69def7e8e6c0?source=collection_archive---------6----------------------->

> 这篇文章是为期 12 个月的加速学习项目[月掌握](https://medium.com/@maxdeutsch/m2m-day-1-completing-12-ridiculously-hard-challenges-in-12-months-9843700c741f)的一部分。今年五月，[我的目标是打造无人驾驶汽车的软件部分](https://medium.com/@maxdeutsch/m2m-day-182-attempting-to-build-a-self-driving-car-809fab9e4723)。

现在我已经有了可以工作的*无人驾驶汽车*代码([见昨天的视频](https://medium.com/@maxdeutsch/m2m-day-190-the-car-is-driving-itself-74490ae509de))，在接下来的几天里，我计划解构代码，并试图理解它到底是如何工作的。

今天，我将特别关注“模型”，它可以被认为是代码的*部分:模型定义了如何将输入图像转换成转向指令。*

今天我没有太多的时间，所以我不会完整地描述代码是如何工作的(因为我还不知道，还需要做大量的研究)。相反，我会对代码行的含义做一些假设，然后记录我需要进一步研究的开放性问题。

这将使我有条理地学习材料。

这是自动驾驶模型的完整代码。它只有 50 行代码加上注释和空格(这是相当疯狂的，因为它在驾驶汽车和其他东西……)

```
import tensorflow as tf
import scipydef weight_variable(shape):
  initial = tf.truncated_normal(shape, stddev=0.1)
  return tf.Variable(initial)def bias_variable(shape):
  initial = tf.constant(0.1, shape=shape)
  return tf.Variable(initial)def conv2d(x, W, stride):
  return tf.nn.conv2d(x, W, strides=[1, stride, stride, 1], padding='VALID')x = tf.placeholder(tf.float32, shape=[None, 66, 200, 3])
y_ = tf.placeholder(tf.float32, shape=[None, 1])x_image = x**#first convolutional layer**
W_conv1 = weight_variable([5, 5, 3, 24])
b_conv1 = bias_variable([24])h_conv1 = tf.nn.relu(conv2d(x_image, W_conv1, 2) + b_conv1)**#second convolutional layer**
W_conv2 = weight_variable([5, 5, 24, 36])
b_conv2 = bias_variable([36])h_conv2 = tf.nn.relu(conv2d(h_conv1, W_conv2, 2) + b_conv2)**#third convolutional layer**
W_conv3 = weight_variable([5, 5, 36, 48])
b_conv3 = bias_variable([48])h_conv3 = tf.nn.relu(conv2d(h_conv2, W_conv3, 2) + b_conv3)**#fourth convolutional layer**
W_conv4 = weight_variable([3, 3, 48, 64])
b_conv4 = bias_variable([64])h_conv4 = tf.nn.relu(conv2d(h_conv3, W_conv4, 1) + b_conv4)**#fifth convolutional layer**
W_conv5 = weight_variable([3, 3, 64, 64])
b_conv5 = bias_variable([64])h_conv5 = tf.nn.relu(conv2d(h_conv4, W_conv5, 1) + b_conv5)**#FCL 1**
W_fc1 = weight_variable([1152, 1164])
b_fc1 = bias_variable([1164])h_conv5_flat = tf.reshape(h_conv5, [-1, 1152])
h_fc1 = tf.nn.relu(tf.matmul(h_conv5_flat, W_fc1) + b_fc1)keep_prob = tf.placeholder(tf.float32)
h_fc1_drop = tf.nn.dropout(h_fc1, keep_prob)**#FCL 2**
W_fc2 = weight_variable([1164, 100])
b_fc2 = bias_variable([100])h_fc2 = tf.nn.relu(tf.matmul(h_fc1_drop, W_fc2) + b_fc2)h_fc2_drop = tf.nn.dropout(h_fc2, keep_prob)**#FCL 3**
W_fc3 = weight_variable([100, 50])
b_fc3 = bias_variable([50])h_fc3 = tf.nn.relu(tf.matmul(h_fc2_drop, W_fc3) + b_fc3)h_fc3_drop = tf.nn.dropout(h_fc3, keep_prob)**#FCL 4**
W_fc4 = weight_variable([50, 10])
b_fc4 = bias_variable([10])h_fc4 = tf.nn.relu(tf.matmul(h_fc3_drop, W_fc4) + b_fc4)h_fc4_drop = tf.nn.dropout(h_fc4, keep_prob)**#Output**
W_fc5 = weight_variable([10, 1])
b_fc5 = bias_variable([1])y = tf.mul(tf.atan(tf.matmul(h_fc4_drop, W_fc5) + b_fc5), 2)
```

# 逐行评论

现在，我将分块研究代码，并描述我认为每个块的意思/作用。

```
import tensorflow as tf
import scipy
```

前两行很简单。

我们正在导入 TensorFlow 库(我们将在代码的其他地方将其称为“tf ”)和 SciPy 库。TensorFlow 是由 Google 编写的 python 库，它将帮助抽象出大多数地面级机器学习实现。SciPy 会在数学方面帮忙。

这里没什么新东西可学。

```
def weight_variable(shape):
  initial = tf.truncated_normal(shape, stddev=0.1)
  return tf.Variable(initial)def bias_variable(shape):
  initial = tf.constant(0.1, shape=shape)
  return tf.Variable(initial)
```

好的，这里我认为我们正在定义新的*对象*，这基本上意味着我们可以在代码的其他地方使用“weight_variable”和“bias_variable”的概念，而不必一次重新定义它们。

在机器学习中，我们试图求解的函数通常表示为 Wx+b = y，其中给定了 x(输入图像列表)和 y(对应的转向指令列表)，想要找到 W 和 b 的最佳组合，使方程平衡。

w 和 b 实际上不是单一的数字，而是系数的集合。这些集合是多维的，并且这些集合的大小对应于机器学习网络中的节点数量。(至少，我现在是这么理解的)。

所以，在上面的代码中， *weight_variable* 对象代表 W， *bias_variable* 对象代表 b，在广义上。

这些对象接受一个称为“形状”的输入，它基本上定义了 W 和 b 的维数。

这些 W 和 b 对象由一个名为“normal”的函数初始化。我很确定这意味着…当最初创建 W 和 b 的集合时，单个系数的值应该基于标准偏差为 0.1 的正态分布(即钟形曲线)随机分配。标准偏差或多或少定义了我们希望初始系数有多随机。

所以，令人惊讶的是，我想我基本上理解了这段代码。乍一看，我不确定这是怎么回事，但写出来有助于我整理思绪。

**我还需要学习的:**我需要学习更多关于 Wx + b = y 结构的知识，为什么使用它，它是如何工作的，等等。，但我理解代码的基本原理。

```
def conv2d(x, W, stride):
  return tf.nn.conv2d(x, W, strides=[1, stride, stride, 1], padding='VALID')
```

我相信这个 *conv2d* 是一个对某些输入执行核卷积的函数。内核卷积是我几天前在[中学到的一种更通用的图像操作。](https://medium.com/towards-data-science/m2m-day-185-my-attempt-to-intuitively-explain-how-this-self-driving-car-algorithm-works-7422eb2b135e)

就我而言，核卷积操纵图像来突出图像的某些特征，无论是图像的边缘、角落等等。

这个特殊的特征是由“内核”定义的，它似乎是用上面的*步幅=[1，步幅，步幅，1]* 定义的。虽然，我不知道大步是什么意思，也不知道这到底是怎么回事。

这个图像处理函数似乎有三个输入:1 .内核/步幅(说明如何操作图像)；2.x(也就是图像本身)；第三。w(我猜它是一组系数，用于在某种程度上将不同的图像操作混合在一起)。

我必须更多地了解 W 在这一切中的角色。

不过，在高层次上，该函数以某种方式操纵图像，以自动将图像缩减为更有助于*训练模型*的独特特征。

**我还需要学习的:**卷积函数在数学上到底是如何定义的，W 在其中又是如何发挥作用的？

```
x = tf.placeholder(tf.float32, shape=[None, 66, 200, 3])
y_ = tf.placeholder(tf.float32, shape=[None, 1])x_image = x
```

接下来的几行看起来很简单。我们再一次回到等式 Wx + b = y。

这里我们本质上是为 x 和 y 变量定义了占位符*。这些占位符设置了变量的维度(记住:这些变量代表一个值的集合，而不仅仅是一个数字)。*

我们将 x 设置为接收特定尺寸的图像，将 y 设置为输出一个数字(即转向角)。

然后我们把 x 改名为“x_image”来提醒自己，x 是一个图像，因为……为什么不是。

这里没什么新东西可学。

```
**#first convolutional layer**
W_conv1 = weight_variable([5, 5, 3, 24])
b_conv1 = bias_variable([24])h_conv1 = tf.nn.relu(conv2d(x_image, W_conv1, 2) + b_conv1)
```

好了，我们现在进入第一个卷积层。

我们定义 *W_conv1* ，它只是我上面解释的 weight_variable 的一个具体实例(用 shape [5，5，3，24])。我不确定这个形状是如何或为什么以这种特殊的方式设置的。

然后我们定义 *b_conv1* ，它只是我上面解释的 bias_variable 的一个具体实例(具有形状[24])。这个 24 可能需要匹配 W_conv1 形状的 24，但我不确定为什么(除了这将有助于使*矩阵乘法*工作)。

h_conv1 是一个中间对象，它将卷积函数应用于输入 x_image 和 W_conv1，将 bconv1 添加到卷积的输出，然后通过一个名为 *relu* 的函数处理一切。

这个 *relu* 的东西听起来很熟悉，但是我不记得它到底是做什么的了。我的猜测是，这是某种“squasher”或规范化功能，在某种程度上平滑一切，无论这意味着什么。我得调查一下。

虽然我可以*阅读*大部分代码，但我不太确定为什么“卷积层”要以这种方式设置。

**我还需要学习的:**什么是卷积层，它应该做什么，它是如何做到的？

```
**#second convolutional layer**
W_conv2 = weight_variable([5, 5, 24, 36])
b_conv2 = bias_variable([36])h_conv2 = tf.nn.relu(conv2d(h_conv1, W_conv2, 2) + b_conv2)**#third convolutional layer**
W_conv3 = weight_variable([5, 5, 36, 48])
b_conv3 = bias_variable([48])h_conv3 = tf.nn.relu(conv2d(h_conv2, W_conv3, 2) + b_conv3)**#fourth convolutional layer**
W_conv4 = weight_variable([3, 3, 48, 64])
b_conv4 = bias_variable([64])h_conv4 = tf.nn.relu(conv2d(h_conv3, W_conv4, 1) + b_conv4)**#fifth convolutional layer**
W_conv5 = weight_variable([3, 3, 64, 64])
b_conv5 = bias_variable([64])h_conv5 = tf.nn.relu(conv2d(h_conv4, W_conv5, 1) + b_conv5)
```

我们继续有四个以上的卷积层，其功能与第一层完全相同，但不是使用 x_image 作为输入，而是使用来自前一层的输出(即 h_conv 的东西)。

我不确定我们是如何决定使用五层的，也不知道为什么每个 conv 的形状都不一样。

**我还需要学习的是:**为什么有五层，我们如何*为每一层选择*形状？

```
**#FCL 1**
W_fc1 = weight_variable([1152, 1164])
b_fc1 = bias_variable([1164])h_conv5_flat = tf.reshape(h_conv5, [-1, 1152])
h_fc1 = tf.nn.relu(tf.matmul(h_conv5_flat, W_fc1) + b_fc1)keep_prob = tf.placeholder(tf.float32)
h_fc1_drop = tf.nn.dropout(h_fc1, keep_prob)**#FCL 2**
W_fc2 = weight_variable([1164, 100])
b_fc2 = bias_variable([100])h_fc2 = tf.nn.relu(tf.matmul(h_fc1_drop, W_fc2) + b_fc2)h_fc2_drop = tf.nn.dropout(h_fc2, keep_prob)**#FCL 3**
W_fc3 = weight_variable([100, 50])
b_fc3 = bias_variable([50])h_fc3 = tf.nn.relu(tf.matmul(h_fc2_drop, W_fc3) + b_fc3)h_fc3_drop = tf.nn.dropout(h_fc3, keep_prob)**#FCL 4**
W_fc4 = weight_variable([50, 10])
b_fc4 = bias_variable([10])h_fc4 = tf.nn.relu(tf.matmul(h_fc3_drop, W_fc4) + b_fc4)h_fc4_drop = tf.nn.dropout(h_fc4, keep_prob)
```

接下来，我们有四个 fcl，我相信它们代表“全连接层”。

这些层的设置似乎类似于卷积步骤，但我不确定这里发生了什么。我认为这只是普通的神经网络的东西(我写它是为了假装我完全理解“普通的神经网络的东西”)。

不管怎样，我会更深入地调查这件事。

**我还需要学习的:**什么是 FCL，每个 FCL 步骤中发生了什么？

```
**#Output**
W_fc5 = weight_variable([10, 1])
b_fc5 = bias_variable([1])y = tf.mul(tf.atan(tf.matmul(h_fc4_drop, W_fc5) + b_fc5), 2)
```

最后，我们采用最终 FCL 层的输出，进行一些疯狂的三角运算，然后输出 y，即预测的转向角。

这一步似乎只是“把数学算出来”，但我不确定。

我还需要学习的:产量是如何以及为什么以这种方式计算的？

完成了。

这比预期的要长——主要是因为我能够解析的东西比预期的多。

TensorFlow 库抽象出了多少实现，而构建一个完全有能力的自动驾驶汽车模型所需的底层数学知识又是如此之少，这有点疯狂。

对于我们作为模型构造者来说，似乎唯一重要的事情就是如何设置模型的深度(例如层数)、每层的形状以及层的类型。

我的猜测是，这可能更像是一门艺术，而不是科学，但很可能是一门受过教育的艺术。

我明天开始钻研我的开放式问题。

> 阅读[下一篇](https://medium.com/@maxdeutsch/m2m-day-192-taking-a-break-my-favorite-youtube-channels-856348098dac)。看了[以前的帖子](https://medium.com/@maxdeutsch/m2m-day-190-the-car-is-driving-itself-74490ae509de)。

## [马克斯·多伊奇](http://max.xyz)是一名痴迷的学习者、产品建造者、为期[个月以掌握](http://MonthToMaster.com)的试验品，以及 [Openmind](http://OpenmindLearning.com) 的创始人。

## 如果你想跟随 Max 长达一年的加速学习项目，请确保跟随[这个媒介账户](https://medium.com/@maxdeutsch)。