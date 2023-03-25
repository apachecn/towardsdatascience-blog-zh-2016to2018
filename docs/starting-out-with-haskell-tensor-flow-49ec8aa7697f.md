# 从哈斯克尔张量流开始

> 原文：<https://towardsdatascience.com/starting-out-with-haskell-tensor-flow-49ec8aa7697f?source=collection_archive---------6----------------------->

上周，我们讨论了人工智能系统的蓬勃发展。我们看到了这些系统如何越来越多地影响我们生活的几个例子。我认为我们在选择架构时应该更加关注可靠性。毕竟，当我们现在正确编码时，人们的生命可能处于危险之中。自然，我建议 Haskell 作为开发可靠的人工智能系统的主要候选人。

所以现在我们实际上要写一些 Haskell 机器学习代码。我们将关注张量流绑定库。我第一次熟悉这个图书馆是在四月的 [BayHac](https://www.mmhaskell.com/blog/2017/4/4/bayhac-summary) 。我在过去的几个月里学习了整个[张量流](https://www.tensorflow.org/)和 [Haskell 库](https://github.com/tensorflow/haskell)。在第一篇文章中，我们将回顾张量流的基本概念。我们将看到它们是如何在 Python(TF 最常用的语言)中实现的。然后我们将把这些概念翻译给 Haskell。

请注意，本系列不是对机器学习概念的一般性介绍。Medium 上有一个很棒的系列，叫做[机器学习很有趣](https://medium.com/@ageitgey/machine-learning-is-fun-80ea3ec3c471)！如果您对学习基本概念感兴趣，我强烈推荐您阅读该系列的第 1 部分。有了这个背景，我自己的文章系列中的许多观点会更加清晰。

# 张量

张量流是一个伟大的名字，因为它将库分解成两个基本概念。首先是张量。这些是张量流中数据表示的主要工具。低维张量其实挺直观的。但是，当你无法真正想象正在发生的事情时，你必须让理论思想来指导你。

在大数据的世界里，我们用数字来表示一切。当你有一组数字时，程序员的本能是把它们放在一个数组中。

```
[1.0, 2.0, 3.0, 6.7]
```

如果你有很多相同大小的不同数组，你想把它们关联在一起，你会怎么做？你创建了一个二维数组(数组的数组)，我们也称之为矩阵。

```
[[1.0, 2.0, 3.0, 6.7],
[5.0, 10.0, 3.0, 12.9],
[6.0, 12.0, 15.0, 13.6],
[7.0, 22.0, 8.0, 5.3]]
```

大多数程序员都非常熟悉这些概念。张量采用这种思想，并不断扩展。当你有很多相同大小的矩阵时会发生什么？您可以将它们组合成一个矩阵数组。我们可以称之为三维矩阵。但是“张量”是我们在所有维度中用来表示数据的术语。

每个张量都有度数。我们可以从一个数字开始。这是一个 0 度张量。那么一个正常的数组就是一个 1 阶张量。那么矩阵就是 2 次张量。最后一个例子是一个三阶张量。你可以不断地把这些加在一起，无穷无尽。

每个张量都有形状。形状是一个代表张量维数的数组。这个数组的长度就是张量的次数。所以一个数字的形状是空列表。一个数组将有一个长度为 1 的列表，包含数组的长度。一个矩阵将有一个长度为 2 的列表，包含它的行数和列数。诸如此类。有几种不同的方法可以在代码中表示张量，但是我们一会儿会讲到。

# 随波逐流

要理解的第二个重要概念是张量流如何执行计算。机器学习通常涉及简单的数学运算。许多简单的数学运算。由于规模如此之大，我们需要尽可能快地执行这些操作。我们需要使用针对这一特定任务而优化的软件和硬件。这就需要对正在发生的事情有一个底层的代码表示。这在 C 语言中比在 Haskell 或 Python 中更容易实现。

我们可以用 Haskell 编写大部分代码，但是用 C 语言使用外部函数接口来执行计算。但是这些接口有很大的开销，所以这很可能会抵消我们从使用 c 语言中获得的大部分好处。

张量流对这个问题的解决方案是，我们首先建立一个描述我们所有计算的图。一旦我们描述了这些，我们就用一个“会话”来“运行”我们的图表。因此，它一次执行整个语言转换过程，所以开销较低。

如果这听起来很熟悉，那是因为这是 Haskell 中动作的工作方式(在某种意义上)。例如，我们可以描述一个 IO 动作。这个动作不是我们在代码中出现的时候执行的一系列命令。更确切地说，动作是我们的程序将在某个时刻执行的操作的描述。这也类似于[有效编程](https://hackage.haskell.org/package/extensible-effects)的概念。我们将在以后的博客中探讨这个话题。

那么我们的计算图是什么样的呢？我们会，每个张量都是一个节点。然后，我们可以为“运算”创建其他节点，这些节点以张量作为输入。例如，我们可以把两个张量加在一起，这是另一个节点。我们将在示例中看到如何构建计算图，然后运行它。

张量流的一个令人敬畏的特性是张量板应用程序。它可以让你可视化你的计算图表。我们将在本系列的后面看到如何做到这一点。

# 编码张量

因此，在这一点上，我们应该开始检查我们实际上是如何在代码中创建张量的。我们将从如何在 Python 中实现这一点开始，因为这样概念更容易理解。我们将考虑三种类型的张量。首先是“常数”。这些代表一组不变的值。我们可以在整个模型训练过程中使用这些值，并且每次都是相同的。因为我们预先定义了张量的值，所以没有必要给出任何大小参数。但是我们将指定用于它们的数据类型。

```
import tensorflow as tfnode1 = tf.constant(3.0, dtype=tf.float32)
node2 = tf.constant(4.0, dtype=tf.float32)
```

现在我们可以用这些张量做什么呢？为了快速举例，让我们试着添加它们。这在我们的图中创建了一个新的节点，表示这两个张量的相加。然后我们可以“运行”加法节点来查看结果。为了封装我们所有的信息，我们将创建一个“会话”:

```
import tensorflow as tfnode1 = tf.constant(3.0, dtype=tf.float32)
node2 = tf.constant(4.0, dtype=tf.float32)
additionNode = tf.add(node1, node2)sess = tf.Session()
result = sess.run(additionNode)
print result“””
Output:
7.0
“””
```

接下来是占位符。这些是我们每次运行都要改变的值。通常，我们将使用这些作为模型的输入。通过使用占位符，我们将能够改变输入并每次训练不同的值。当我们“运行”一个会话时，我们需要给每个节点赋值。

我们不知道将进入占位符的值，但我们仍然在构造时分配数据的类型。如果我们愿意，我们也可以指定一个尺寸。这里有一个简短的片段，展示了我们如何初始化占位符。然后，我们可以在每次运行应用程序时分配不同的值。即使我们的占位符张量没有值，我们仍然可以添加它们，就像我们添加常量张量一样。

```
node1 = tf.placeholder(tf.float32)
node2 = tf.placeholder(tf.float32)
adderNode = tf.add(node1, node2)sess = tf.Session()
result1 = sess.run(adderNode, {node1: 3, node2: 4.5 })
result2 = sess.run(adderNode, {node1: 2.7, node2: 8.9 })
print(result1)
print(result2)"""
Output:
7.5
11.6
"""
```

我们要用的最后一种张量是变量。这些是将构成我们“模型”的价值观。我们的目标是找到这些参数的值，使我们的模型能够很好地拟合数据。我们将一如既往地提供数据类型。在这种情况下，我们还将提供一个初始常数值。通常，我们会希望使用某种随机分布。张量实际上不会取值，直到我们运行一个全局变量初始化函数。在开始之前，我们必须创建这个初始化器，然后让我们的 session 对象运行它。

```
w = tf.Variable([3], dtype=tf.float32)
b = tf.Variable([1], dtype=tf.float32)sess = tf.Session()
init = tf.global_variables_initializer()
sess.run(init)
```

现在让我们使用我们的变量来创建一个“模型”的排序。在本文中，我们将制作一个简单的线性模型。让我们为输入张量和模型本身创建额外的节点。我们将设`w`为权重，`b`为“偏差”。这意味着我们将通过`w*x + b`构造我们的最终值，其中`x`是输入。

```
w = tf.Variable([3], dtype=tf.float32)
b = tf.Variable([1], dtype=tf.float32)
x = tf.placeholder(dtype=tf.float32)
linear_model = w * x + b
```

现在，我们想知道我们的模型有多好。所以让我们将它与`y`进行比较，它是我们期望值的输入。我们取差，平方，然后用`reduce_sum`库函数得到我们的“损失”。损失衡量的是我们希望我们的模型代表的东西和它实际代表的东西之间的差异。

```
w = tf.Variable([3], dtype=tf.float32)
b = tf.Variable([1], dtype=tf.float32)
x = tf.placeholder(dtype=tf.float32)
linear_model = w * x + b
y = tf.placeholder(dtype=tf.float32)
squared_deltas = tf.square(linear_model - y)
loss = tf.reduce_sum(squared_deltas)
```

这里的每条线都是一个不同的张量，或者是我们图形中的一个新节点。我们将通过使用学习率为 0.01 的内置`GradientDescentOptimizer`来完成我们的模型。我们将把我们的训练步骤设定为试图最小化损失函数。

```
optimizer = tf.train.GradientDescentOptimizer(0.01)
train = optimizer.minimize(loss)
```

现在我们将运行会话，初始化变量，并运行我们的训练步骤 1000 次。我们将传递一系列输入及其预期输出。我们来试着学一下台词`y = 5x - 1`。我们的预期产出`y`值将假设这一点。

```
sess = tf.Session()
init = tf.global_variables_initializer()
sess.run(init)
for i in range(1000):
    sess.run(train, {x: [1, 2, 3, 4], y: [4,9,14,19]})print(sess.run([W,b]))
```

最后，我们打印出权重和偏差，并看到我们的结果！

```
[array([ 4.99999475], dtype=float32), array([-0.99998516], dtype=float32)]
```

所以我们可以看到我们学习到的值非常接近正确值 5 和-1！

# 用 Haskell 表示张量

最后，我将详细介绍如何在 Haskell 中应用这些张量概念。像字符串和数字一样，我们不能在 Haskell 中有这种“张量”类型，因为这种类型可能真的代表一些非常不同的概念。为了更深入地了解我们正在处理的张量类型，请查看我们的深度指南中的[。](https://www.mmhaskell.com/tensorflow)

同时，让我们看一些简单的代码片段，复制我们在 Python 中的工作。下面是我们如何制作几个常数，并把它们加在一起。请注意“重载列表”扩展。它允许我们用和列表一样的语法来表示不同的类型。我们将此用于`Shape`物品和`Vectors`:

```
{-# LANGUAGE OverloadedLists #-}import Data.Vector (Vector)
import TensorFlow.Ops (constant, add)
import TensorFlow.Session (runSession, run)runSimple :: IO (Vector Float)
runSimple = runSession $ do
  let node1 = constant [1] [3 :: Float]
  let node2 = constant [1] [4 :: Float]
  let additionNode = node1 `add` node2
  run additionNodemain :: IO ()
main = do
  result <- runSimple
  print result{-
Output:
[7.0]
-}
```

我们使用`constant`函数，它接受一个`Shape`然后是我们想要的值。我们将创建我们的加法节点，然后`run`它得到输出，这是一个带有单个浮点数的向量。我们将一切都包装在`runSession`函数中。这封装了我们在 Python 中看到的初始化和运行动作。

现在假设我们想要占位符。这在 Haskell 中稍微复杂一点。我们将使用两个占位符，就像在 Python 中一样。我们将用`placeholder`函数和一个形状来初始化它们。我们将为函数的输入值提供参数。要真正传递参数来填充占位符，我们必须使用我们称之为“提要”的东西。

我们知道我们的`adderNode`取决于两个值。所以我们将把 run-step 写成一个函数，它接受两个“feed”值，每个占位符一个。然后我们将使用`feed`函数将这些提要分配给适当的节点。我们将把这些放在一个列表中，并将该列表作为参数传递给`runWithFeeds`。然后，我们通过对输入数据调用 run-step 来结束。我们将不得不`encode`原始向量作为张量。

```
import TensorFlow.Core (Tensor, Value, feed, encodeTensorData)
import TensorFlow.Ops (constant, add, placeholder)
import TensorFlow.Session (runSession, run, runWithFeeds)import Data.Vector (Vector)runPlaceholder :: Vector Float -> Vector Float -> IO (Vector Float)
runPlaceholder input1 input2 = runSession $ do
  (node1 :: Tensor Value Float) <- placeholder [1]
  (node2 :: Tensor Value Float) <- placeholder [1]
  let adderNode = node1 `add` node2
  let runStep = \node1Feed node2Feed -> runWithFeeds 
        [ feed node1 node1Feed
        , feed node2 node2Feed
        ] 
        adderNode
  runStep (encodeTensorData [1] input1) (encodeTensorData [1] input2)main :: IO ()
main = do
  result1 <- runPlaceholder [3.0] [4.5]
  result2 <- runPlaceholder [2.7] [8.9]
  print result1
  print result2{-
Output:
[7.5]
[11.599999] -- Yay rounding issues!
-}
```

现在，我们将通过我们已经在 Python 中看到的简单线性模型场景来总结。我们再一次将两个向量作为输入。这些将是我们试图匹配的价值观。接下来，我们将使用`initializedVariable`函数来获取我们的变量。我们不需要调用全局变量初始化器。但是这确实会影响会话的状态。注意，我们将它从 monad 上下文中提取出来，而不是使用 let。(占位符也是如此。)

```
import TensorFlow.Core (Tensor, Value, feed, encodeTensorData, Scalar(..))
import TensorFlow.Ops (constant, add, placeholder, sub, reduceSum, mul)
import TensorFlow.GenOps.Core (square)
import TensorFlow.Variable (readValue, initializedVariable, Variable)
import TensorFlow.Session (runSession, run, runWithFeeds)
import TensorFlow.Minimize (gradientDescent, minimizeWith)import Control.Monad (replicateM_)
import qualified Data.Vector as Vector
import Data.Vector (Vector)runVariable :: Vector Float -> Vector Float -> IO (Float, Float)
runVariable xInput yInput = runSession $ do
  let xSize = fromIntegral $ Vector.length xInput
  let ySize = fromIntegral $ Vector.length yInput
  (w :: Variable Float) <- initializedVariable 3
  (b :: Variable Float) <- initializedVariable 1
  …
```

接下来，我们将制作占位符和线性模型。然后我们用和之前差不多的方法计算损失函数。然后，我们将使用相同的提要技巧来插入占位符。

```
runVariable :: Vector Float -> Vector Float -> IO (Float, Float)
  ...
  (x :: Tensor Value Float) <- placeholder [xSize]
  let linear_model = ((readValue w) `mul` x) `add` (readValue b)
  (y :: Tensor Value Float) <- placeholder [ySize]
  let square_deltas = square (linear_model `sub` y)
  let loss = reduceSum square_deltas
  trainStep <- minimizeWith (gradientDescent 0.01) loss [w,b] 
  let trainWithFeeds = \xF yF -> runWithFeeds
        [ feed x xF
        , feed y yF
        ]
        trainStep
…
```

最后，我们将对输入数据运行 1000 次训练步骤。然后，我们将再次运行我们的模型，以提取权重和偏差的值。那我们就完了！

```
runVariable :: Vector Float -> Vector Float -> IO (Float, Float)
...
  replicateM_ 1000 
    (trainWithFeeds (encodeTensorData [xSize] xInput) (encodeTensorData [ySize] yInput))
  (Scalar w_learned, Scalar b_learned) <- run (readValue w, readValue b)
  return (w_learned, b_learned)main :: IO ()
main = do
  results <- runVariable [1.0, 2.0, 3.0, 4.0] [4.0, 9.0, 14.0, 19.0]
  print results{-
Output:
(4.9999948,-0.99998516)
-}
```

# 结论

希望这篇文章让你对 Haskell 中张量流的一些可能性有所了解。我们看到了张量流基础的快速介绍。我们看到了三种不同的张量。然后我们看到了 Python 和 Haskell 中的代码示例。最后，我们看了一个简单线性模型的快速示例，并了解了如何学习适合该模型的值。下周，我们将做一道更复杂的学习题。我们将使用经典的“Iris”花卉数据集，并使用全神经网络训练分类器。

如果你想了解更多细节，你应该查看免费的哈斯克尔张量流指南。它将引导您使用张量流库作为依赖项，并运行一个基本模型！

也许你对 Haskell 完全陌生，但对使用它进行机器学习或其他任何事情的可能性很感兴趣。您应该下载我们的[入门清单](https://www.mmhaskell.com/checklist)！它有一些关于安装 Haskell 和学习核心概念的很好的资源。