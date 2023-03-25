# 在 10 行代码中使用梯度下降进行线性回归

> 原文：<https://towardsdatascience.com/linear-regression-using-gradient-descent-in-10-lines-of-code-642f995339c0?source=collection_archive---------0----------------------->

我的目标是最终为其他优化技术写出这样的文章。让我们从梯度下降开始。注意:这不是一个全面的指南，因为我浏览了很多东西。

在我们学习机器学习和人工智能的旅程中，在深入之前了解基础知识很重要。基础知识是理解更复杂的架构形式所需的构建模块。例如，在没有首先理解导数是什么、如何以及为什么起作用之前，你不会想要学习常微分方程。机器学习也是如此。通过使用梯度下降作为我们的优化技术来理解线性回归将有助于我们在未来理解更复杂的模型。

我记得有一次向一群数据科学家解释我为这家公司创建的随机森林分类模型。我试着用逻辑回归做一个类比，因为我假设我在房间里的数据科学家同事会知道这件事。他们中的很多人说他们不熟悉它。我感到震惊，因为我们在这里谈论一个更先进的方法，但他们甚至不知道什么是逻辑回归模型。别这样。

# 线性回归、成本和梯度下降

线性回归是我们可以建立关系模型的最基本的方法之一。我们这里的模型可以描述为 *y=mx+b，*其中 *m* 是斜率(改变陡度)， *b* 是偏差(在图中上下移动线条)， *x* 是解释变量， *y* 是输出。如果我们认为有线性关系，我们就使用线性回归。比如说下面的 x 轴是*学习 _ 时间*而 y 轴是*考试 _ 分数*。

![](img/1c82327b82f1345d6cacb823cbc548b2.png)

Source: [https://i.imgur.com/oZ6CBpi.png](https://i.imgur.com/oZ6CBpi.png)

一条直线最好地描述了这种关系，因为学生学习得越多，他或她的考试分数就越高。使用指数、正弦或对数函数来模拟这种关系是没有意义的。线性模型为我们提供了简单性；线性关系很容易理解和解释。请注意，现实世界中的关系并不总是线性的，因此我们使用更高级的方法，如神经网络，这是通用的函数逼近器。更多关于神经网络的信息。

通过调整 *m* 和 *b* ，我们可以创建一条最佳描述关系的直线。我们怎么知道我们接近了？通过使用一个叫做**成本函数**的东西。它直接告诉我们成本。高成本值意味着它很贵——我们的近似值远远不能描述真实的关系。另一方面，低成本值意味着它很便宜——我们的近似值接近描述这种关系。

对于线性回归，我们使用一个被称为均方误差或 MSE 的成本函数。我们取实际数据点的平方值减去近似值。我们的近似值可以使用我们拥有的当前值 *m* 和 *b* 来计算:*y _ approx = m _ current * x+b _ current*。之后，我们将所有这些值相加，然后除以我们拥有的数据点数，实际上就是取平均值。现在你明白为什么它被称为均方误差了。

![](img/2cec29681988984ae800b214911f7873.png)

Source: [https://i.stack.imgur.com/19Cmk.gif](https://i.stack.imgur.com/19Cmk.gif)

你现在可能在想，“这听起来很棒，但是我们怎么才能发现*m*b？？蛮力？?"暴力是没有用的。更有效的方法是梯度下降。想象一下，蒙着眼睛试图找到最低点，如下图所示。你要做的是检查左右两边，然后感觉哪一个把你带到一个较低的点。你每走一步都这样做，直到检查左右两边把你带到一个更高的点。

![](img/f306884778c2cdf0caf37adcdd795019.png)

Source: [https://iamtrask.github.io/img/sgd_no_lines.png](https://iamtrask.github.io/img/sgd_no_lines.png)

背后的数学并不像看起来那么复杂。我们在这里所做的是对成本函数应用关于 m*和 b*的偏导数，以将我们指向最低点。如果你记得你的数学，零的导数意味着你处于局部最小值或最大值。这意味着我们越接近零，**越好。当我们的导数接近(如果不是)零时，我们也不可避免地会得到成本函数的最低值。**

![](img/19577e56d3b84e1e2d9290eaf131c6e1.png)

Source: [https://spin.atomicobject.com/wp-content/uploads/linear_regression_gradient1.png](https://spin.atomicobject.com/wp-content/uploads/linear_regression_gradient1.png)

寻找 m 和 b 的最佳值的过程就是最小化我们的导数。训练机器学习算法或神经网络实际上就是最小化成本函数的过程。

# **编程吧**

在这里，我将使用 Python 来编写我们的线性回归模型。我使用 Python 是因为它是我从事数据科学的首选语言。此外，Python 对于专家和初学者来说都很棒。这种语言是为了可读性而设计的，所以无论你已经编程多年还是刚刚编程一天，浏览别人的代码仍然是相当容易的。

```
**def** *linear_regression*(X, y, m_current=0, b_current=0, epochs=1000, learning_rate=0.0001):
     N = **float**(**len**(y))
     **for** i **in** **range**(epochs):
          y_current = (m_current * X) + b_current
          cost = **sum**([data**2 **for** data **in** (y-y_current)]) / N
          m_gradient = -(2/N) * **sum**(X * (y - y_current))
          b_gradient = -(2/N) * **sum**(y - y_current)
          m_current = m_current - (learning_rate * m_gradient)
          b_current = b_current - (learning_rate * b_gradient)
     **return** m_current, b_current, cost
```

*X* 和 *y* 是我们的输入参数。另一方面， *m_current* 和 *b_current* 分别是我们的斜率和偏置项，在我们试图找到最佳数值时，这两项都会更新，以便我们得到的方程最符合我们的数据。这里的*时期*指的是我们训练模型的次数，以找到模型拟合数据的最佳斜率和偏差。最后， *learning_rate* 这里指的是收敛的速度，意思是梯度下降找到最佳参数的速度。

为了进一步理解 *learning_rate* ，让我们回到我们蒙着眼睛寻找最低点的例子。大的*学习率*意味着我们迈出的步伐太大，你可能会完全错过最低点。然而，太小的*学习率*意味着我们将花费*很长时间*到达底部。试着在两者之间取得平衡。

重要的一点是 for 循环:

```
**for** i **in** **range**(epochs):
     y_current = (m_current * X) + b_current
     cost = **sum**([data**2 **for** data **in** (y-y_current)]) / N
     m_gradient = -(2/N) * **sum**(X * (y - y_current))
     b_gradient = -(2/N) * **sum**(y - y_current)
     m_current = m_current - (learning_rate * m_gradient)
     b_current = b_current - (learning_rate * b_gradient)
```

我们迭代 1000 次，因为 1000 听起来不错。真的取决于你想迭代多少。超参数微调是一项正在进行的研究，所以你可能想看看！

我们用上面看到的公式计算斜率和偏差的梯度。然后，我们通过减去学习率时间和各自的梯度值来更新斜率和偏差。学习速度将决定我们达到收敛的速度。经过 1000 次迭代后，我们返回 *m_current* 、 *b_current* 和 *cost* 。

**恭喜你！**那是你机器学习和人工智能旅程的第一步。获得梯度下降如何工作的直观感受，因为这实际上也用于更高级的模型。这里的目标是学习基础知识，而你我的朋友刚刚迈出了第一步。现在你可以去学更多的东西了！