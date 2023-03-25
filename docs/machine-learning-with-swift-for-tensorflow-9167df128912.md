# 了解面向 TensorFlow 的 Swift

> 原文：<https://towardsdatascience.com/machine-learning-with-swift-for-tensorflow-9167df128912?source=collection_archive---------4----------------------->

S 用于 TensorFlow 的 wift 由[克里斯·拉特纳](https://www.google.com/url?sa=t&rct=j&q=&esrc=s&source=web&cd=1&cad=rja&uact=8&ved=0ahUKEwjg-pT0n93aAhWMQI8KHSvnAfkQFggoMAA&url=https%3A%2F%2Fen.wikipedia.org%2Fwiki%2FChris_Lattner&usg=AOvVaw1P92WCcalUe6I19h7fIJUG)在 2018 年 TensorFlow 开发峰会上推出。2018 年 4 月 27 日，谷歌团队在他们的 [GitHub 知识库](https://github.com/tensorflow/swift)上首次向公众社区发布。但 Swift for TensorFlow 仍处于起步阶段。而且开发人员/研究人员在项目中使用它似乎还为时过早。如果您仍有兴趣试用，请从 Swift 官网[为 TensorFlow 的快照安装 Swift。](https://swift.org)

![](img/6671b19cc0a05cc100a4c7d853c5f042.png)

Swift for TensorFlow ([Image Source](https://medium.com/tensorflow/introducing-swift-for-tensorflow-b75722c58df0))

在本文中，我将重点解释以下主题:

*   访问 Python APIs 和`PyValue` (Python 的 Swift 动态系统)
*   Swift 中的自动区分系统
*   对 TensorFlow 的 Swift 中的`Tensor` s 执行计算
*   训练神经网络

> Swift for TensorFlow 很可能会像 Swift 和 TensorFlow 独立完成的那样，与开源社区一起快速发展。因此，这可能是一个值得努力去了解它的东西。

***注意*** :关于 TensorFlow 的 Swift 需要注意的一个主要问题是，它是一个*由运行定义的*框架。这意味着，尽管 Swift for TensorFlow 在后台创建图表(如 TensorFlow ),但您不必为执行这些图表创建会话。这种方法类似于 TensorFlow 中的急切执行。

# 1.主要特点

*   自动反向微分(正向尚未实现)
*   运行定义设计(不需要会议)
*   Swift 经过优化，包含机器学习特定功能
*   允许 Python APIs 以 Python 方式访问
*   包括用于 Python 的动态系统行为的`PyValue`类型

# 2.Python 互操作性

借助 Swift for TensorFlow，我们可以以最 Python 化的方式使用 Python APIs。要访问 Python APIs，必须进入程序，如下面的示例代码片段所示。

```
import Pythonlet np = Python.import("numpy")  // akin to `import numpy as np`
let pickle = Python.import("pickle")
let gzip = Python.import("gzip")
```

而用于 TensorFlow 的 Swift 也有一个名为`PyValue`的新类型，它展示了 Python 在 Swift 中的完整动态类型系统行为，而不影响 Swift 中其他类型的行为。

```
var x: PyValue = 3.14159
print(x * 2)  // Prints "6.28318"
x = "string"
print("now a " + x)  // Prints "now a string"
```

更多信息请参考官方的 [Python 互操作性](https://github.com/tensorflow/swift/blob/master/docs/PythonInteroperability.md)文档。

# 3.自动微分

Swift for TensorFlow 内置了对计算函数相对于其他变量的梯度的支持。该功能已被直接整合到 Swift 的编译器中，以优化行为。它支持两种差分功能:`#gradient(of:withRespectTo:)`和`#valueAndGradient(of:)`。虽然它对我不起作用😩(还为时过早)但是文档中说要遵循下面的语法。这段代码片段来自官方的 d [文档。](https://github.com/tensorflow/swift/blob/master/docs/AutomaticDifferentiation.md)

```
@differentiable(reverse, adjoint: dTanh)
func tanh(_ x: Float) -> Float {
  // ... some super low-level assembly tanh implementation ...
}
func dTanh(x: Float, y: Float, seed: Float) -> Float {
  return (1.0 - (y * y)) * seed
}// Get the gradient function of tanh.
let dtanh_dx = #gradient(of: tanh)
dtanh_dx(2)
// Get the gradient function of foo with respect to the first parameter.
let dfoo_dx = #gradient(of: foo, withRespectTo: .0)
dfoo_dx(3, 4)
```

目前仅允许自动*反向*微分，正向微分正在讨论中。

# 4.使用张量

作为一个简单的例子，我们将创建一个`Tensor`实例，在其上我们使用 TensorFlow 应用一些操作。

```
import TensorFlowvar x = Tensor([[1, 2], [3, 4]])
for _ in 1...5 {
  x += x
}
print(x)  // Prints "[[32.0, 64.0], [96.0, 128.0]]"
```

在上面的代码中使用了基本的`+`操作符，它在一个循环中将`Tensor`添加到自身中。这之所以成为可能，是因为 [Swift 的高级操作符](https://developer.apple.com/library/content/documentation/Swift/Conceptual/Swift_Programming_Language/AdvancedOperators.html)功能为`Tensor`实例提供了过载功能。

# 5.训练一个简单的前馈神经网络

转向神经网络——机器学习在这个时代流行的真正原因。在本节中，我们将教授我们的 3 层完全连接的*前馈神经网络*来预测来自 MNIST 数据集的图像中的数字。从 Swift 文件中的 tensor flow`import TensorFlow`开始。

***注* :** 训练代码可以在[这里](https://github.com/tensorflow/swift-models/blob/master/MNIST/MNIST.swift)找到如果有人不耐烦的话。

我们的神经网络将有 3 层:

*   **输入层**:它将输入数据(在我们的例子中是像素值)呈现给神经网络。在我们的例子中，每个图像有 784 个值。
*   **隐藏层**:它用权重和偏差计算我们输入数据的仿射变换。然后将一个 *sigmoid* 激活函数应用于变换。我们示例中的隐藏层将有 30 个单元(神经元)。
*   **输出层**:隐藏层的数据再次经过*仿射变换*和一个 *sigmoid* 函数的应用，就像形成输出层之前一样。这是预测发生的地方。我们在这一层有 10 个单元，每个单元代表在图像中成为特定数字的概率。还要注意，我们在该层中使用*one-hot*/*1-of-k*编码，其中在一维张量中，除了所有其他值为 0 之外，单个值为 1。例如，[0，0，1，0，0，0，0，0，0，0]表示输出[预测]层图像中的两位数。

*仿射变换*基本上是数据与权重的点积，然后是偏差的增加，随后是激活函数的逐元素应用。以下是输入数据的仿射变换的方程式 *x.*

> o(x；W，b)= f(wx+b)

在这里， *O(。)*是输出函数， *f(。)*是激活函数(我们这里是 sigmoid)， *W* 是权重矩阵， *b* 是偏置向量，代表点积。

我们使用 *sigmoid* 激活函数，因为它将值压缩到限制输出范围的范围[0，1],从而提供输出层图像中可能数字的概率。

## 5.1 读取 MNIST 数据

让我们读取数据集并构建这些图像和标签的`Tensor`实例。我们必须创建`Tensor`对象，因为这是 TensorFlow 模型(神经网络)允许流经的对象，因此得名。

```
let (images, numericLabels) = readMnist(imagesFile: imagesFile,labelsFile: labelsFile)
let labels = Tensor<Float>(oneHotAtIndices: numericLabels, depth: 10)
```

## 5.2 超参数

我们定义了 3 个超参数:学习率、训练损失和迭代步骤。

```
let iterationCount: Int32 = 20
let learningRate: Float = 0.2
var loss = Float.infinity
```

## 5.3 可训练参数

接下来，我们根据张量流的`Tensor`类型创建 2 个权重矩阵、2 个偏置向量，如下所示。

```
var w1 = Tensor<Float>(randomUniform: [784, 30])
var w2 = Tensor<Float>(randomUniform: [30, 10])
var b1 = Tensor<Float>(zeros: [1, 30])
var b2 = Tensor<Float>(zeros: [1, 10])
```

## 5.4 训练循环

训练循环是神经网络进行学习的代码块。我们通过网络传递图像和标签`Tensor`(正向传递)。然后计算预测中的误差，然后将它们反向传播以计算可训练参数的梯度。接下来，我们在学习率的帮助下，使用相应的梯度来降低这些参数。最后计算损失，给出我们离图像的真实标签有多远的概念。每个步骤描述如下。

**5.4.1 向前传球**

如上所述，输入图像像素值经过仿射变换。这里的值是带权重的点积，然后加上偏差，偏差进一步通过 s 形激活函数(按元素方式应用)。

```
let z1 = images ⊗ w1 + b1
let h1 = sigmoid(z1)
let z2 = h1 ⊗ w2 + b2
let predictions = sigmoid(z2)
```

这里需要注意的一点是 Swift 使用⊗ unicode 来表示点积，这表明 Swift 语言实际上是多么酷！坦白说，我真的很喜欢♥️这种编程语言。

**5.4.2 反向传递(计算梯度)**

反向传递计算预测和真实标签之间的误差。这些误差然后通过网络反向传播，计算可学习参数的梯度。

```
let dz2 = predictions - labels
let dw2 = h1.transposed(withPermutations: 1, 0) ⊗ dz2
let db2 = dz2.sum(squeezingAxes: 0)
let dz1 = dz2.dot(w2.transposed(withPermutations: 1, 0)) * h1 * (1 - h1)
let dw1 = images.transposed(withPermutations: 1, 0) ⊗ dz1
let db1 = dz1.sum(squeezingAxes: 0)
```

**5.4.3 下降参数**

现在，我们用它们的梯度和决定神经网络学习它的参数的速度的学习速率来降低参数，以便在下一次输入图像被馈送给它时预测真实值。

```
w1 -= dw1 * learningRate
b1 -= db1 * learningRate
w2 -= dw2 * learningRate
b2 -= db2 * learningRate
```

**5.4.4 更新损失**

我们更新损失值以查看我们与真实标签的接近程度，以便下次更正确地预测数字图像。

```
loss = dz2.squared().mean(squeezingAxes: 1, 0).scalarized()
```

让我们现在打印我们的损失，它告诉我们如何从我们的训练集中学习识别数字图像。越低的损失越好是我们网络识别的任务。

```
print("Loss: \(loss)")  // Prints "0.1"
```

# 6.摘要

在本文中，我们了解了用于 TensorFlow 的 Swift，以及它的易用性，因为 Swift 与 Python 非常相似，看起来像脚本语言，但速度非常快。我们看到 Swift for TensorFlow 允许我们使用 Python APIs，而且 Swift 的编译器已经过深度优化，内置了对自动微分的支持，这对机器学习任务非常重要。我们还看到了如何在 Swift 中使用 TensorFlow，我们创建了自己的`Tensor`实例，并对它们进行了一些处理(使用基本操作符`+`)。最后，我们训练了三层神经网络来解决传统的数字图像识别问题。

# 7.讨论

似乎 Swift 的名称应该是*tensor flow*，而不是 TensorFlow 的 *Swift。事实并非如此，因为实际上 Swift 的编译器已被修改为支持 TensorFlow，因此 Swift 不仅充当 Python 库和 TensorFlow 的包装器，现在更像是机器学习语言。为了在整个机器学习和数据科学社区中保持工作流的一致性(因为 Python 被大量使用)，它还允许以 Python 的方式访问 Python APIs，并且还为 Python 的动态系统类型行为实例提供了一种新的类型。*

最后一句话，用于 TensorFlow 的 Swift 是由 Google 开发的，因此它很有可能在未来的时代变得著名。它还试图利用原始 TensorFlow 实现的最佳功能，如 eager-execution。

# 8.参考

[1] [Swift for TensorFlow](https://github.com/tensorflow/swift) ，谷歌

[Swift.org](https://swift.org)，苹果

[3] [Python 的互操作性](https://github.com/tensorflow/swift/blob/master/docs/PythonInteroperability.md)

[4][Swift 中的自动微分](https://github.com/tensorflow/swift/blob/master/docs/AutomaticDifferentiation.md)

[5][Swift 编程语言(Swift 4.1):高级操作员](https://www.google.com/url?sa=t&rct=j&q=&esrc=s&source=web&cd=1&cad=rja&uact=8&ved=0ahUKEwi55bKTp-TaAhUDPI8KHbJeBn4QFggoMAA&url=https%3A%2F%2Fdeveloper.apple.com%2Flibrary%2Fcontent%2Fdocumentation%2FSwift%2FConceptual%2FSwift_Programming_Language%2FAdvancedOperators.html&usg=AOvVaw1LBV8xPgX9PCn4lc67weZs)

[6][Swift for tensor flow:MNIST 示例](https://github.com/tensorflow/swift-models/blob/master/MNIST/MNIST.swift)

如果你觉得这篇文章有用/有见识，请鼓掌👏这样其他人也可以找到它，或者你也可以在社交网络上分享它。如果你在我的解释中发现了一些错误(也许我解释错了)，或者你从这篇文章中有什么不清楚的地方，你也可以在下面发表评论。

***保持【机器】学习，直到你化石燃料！🤘🤖***