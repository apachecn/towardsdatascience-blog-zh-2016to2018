# Sigmoid 函数的导数

> 原文：<https://towardsdatascience.com/derivative-of-the-sigmoid-function-536880cf918e?source=collection_archive---------0----------------------->

![](img/719a2bba325fdf8cf2f637c2ddc04961.png)

Sigmoid and Dino

在本文中，我们将看到在人工智能应用中使用的 Sigmoid 函数的完整推导。

首先，让我们来看看 sigmoid 函数

![](img/1939312a5cfb663cce6aae9034f19f25.png)

Sigmoid function

好吧，看起来不错！
我们读作，*x 的 sigmoid 是 1/1 加上负 x 的指数* 这就是方程 **(1)** 。

让我们看看 sigmoid 函数的图形，

![](img/eeeecd7675cfd5de529f1bdabf631031.png)

[Graph of the Sigmoid Function](https://upload.wikimedia.org/wikipedia/commons/thumb/5/53/Sigmoid-function-2.svg/2000px-Sigmoid-function-2.svg.png)

查看该图，我们可以看到给定一个数字`n`，sigmoid 函数将在 **0** 和 **1** 之间映射该数字。
随着`n`的值变大，sigmoid 函数值越来越接近 **1** ，随着`n`变小，sigmoid 函数值越来越接近 **0** 。

好，让我们开始推导 sigmoid 函数！
所以，我们要的价值是

![](img/63c2c57b436f1d3d953da2ccadfa700e.png)

Step 1

在上面的步骤中，我只是从 **(1)** 展开了 sigmoid 函数的取值公式

接下来，我们简单的用 [*负指数*](https://www.khanacademy.org/math/pre-algebra/pre-algebra-exponents-radicals/pre-algebra-negative-exponents/v/negative-exponents) 来表示上面的方程，

![](img/cedd082c0831fed0508f39b890b613fd.png)

Step 2

接下来，我们将应用 [*倒易法则*](https://www.youtube.com/watch?v=jFJ1kgzDuWY) ，它简单地说

![](img/af8bcf0427434761618bd16246ea60d4.png)

Reciprocal Rule

运用互易法则，我们进入下一步

![](img/40187d2600846774a628224008bb990e.png)

Step 3

为了清楚地看到上一步中发生了什么，用`(1 + e^(-x))`替换互易规则中的`u(x)`。

接下来，我们需要应用线性的 [*法则*](https://en.wikipedia.org/wiki/Linearity_of_differentiation) ，它简单地说

![](img/332a5ee9b3b340f51a918686e30f2138.png)

Rule of Linearity

应用线性法则，我们得到

![](img/7045e105960b16a38866b9eadb862733.png)

Step 4

好的，这很简单，现在让我们一个一个地推导它们。
现在，*常数的导数是 0* ，所以我们可以把下一步写成

![](img/8898d0809bbfef7e7deaadfdd1b8b5dd.png)

Step 5

把 0 加到某个东西上不会有什么影响，所以我们将在下一步中去掉 0，然后进行下一个推导，为此我们将需要 [*指数法则*](https://www.khanacademy.org/math/ap-calculus-ab/ab-differentiation-2-new/ab-3-1b/v/exponential-functions-differentiation) ，它简单地说

![](img/a4d3661fd2fb19e4162c714e91b4ffc4.png)

Exponential Rule

运用我们得到的指数法则，

![](img/76e9114c0081a98247bb1f3c94580d60.png)

Step 6

同样，为了更好地理解，您可以简单地将指数规则中的`e^u(x)`替换为`e^(-x)`

接下来，由[的*线性规律我们可以写出*的](https://en.wikipedia.org/wiki/Linearity_of_differentiation)

![](img/cb9d1e88acccad97272b2e1de635f4ee.png)

Step 7

[*微分变量的导数是 1*](https://www.khanacademy.org/math/calculus-home/taking-derivatives-calc/basic-differentiation-rules-calc/v/constant-derivative-rule) ，应用它我们得到

![](img/5e10bd5037821d26dddd3c0f795f73f2.png)

Step 8

现在，我们可以简单地打开第二对括号，应用我们得到的基本规则`-1 * -1 = +1`

![](img/bc7a7310c463bece23d1ad3d1af9431f.png)

Step 9

可以写成

![](img/09d25d2fea945465dc4563c34ada8519.png)

Step 10

好了，我们完成了求导！！

但是但是，我们仍然需要把它简化一点，以达到机器学习中使用的形式。好了，我们走吧！

首先，让我们重写如下

![](img/53e6229a793d39f97645406751f32e7c.png)

Step 11

然后重写为

![](img/bc391404b26c452568ceb7815367d68e.png)

Step 12

因为`+1 — 1 = 0`我们可以做到这一点

![](img/fe2959a51caf150d50dcbad7d3a49556.png)

Step 13

现在让我们把分数分开，重写为

![](img/7a826ff4755d500a09c20314ce62cc1a.png)

Step 14

让我们消去分子和分母

![](img/f03fcb61f79a06b9263995bf5f12ee72.png)

Step 15

现在，如果我们看一看本文的第一个等式 **(1)** ，那么我们可以改写如下

![](img/e843d2e5bb2bcdea32f7c04aa4f99c6f.png)

Step 16

这样简化就完成了！

所以，sigmoid 函数的导数是

![](img/c628451ad4e1f67bacc54e09fff57d2b.png)

Derivative of the Sigmoid Function

sigmoid 函数的导数的图形看起来像

![](img/5322fd93bcf07db675f811b59e6f2c70.png)

[Graph of Sigmoid and the derivative of the Sigmoid function](https://isaacchanghau.github.io/img/deeplearning/activationfunction/sigmoid.png)

感谢您阅读文章！想和我交流吗？
以下是我的 Linkedin 个人资料和 YouTube 频道的链接，

[](https://www.linkedin.com/in/iarunava/) [## 阿鲁纳瓦查克拉博蒂| LinkedIn

### 查看 Arunava Chakraborty 在 LinkedIn 上的职业简介。LinkedIn 是世界上最大的商业网络，帮助…

www.linkedin.com](https://www.linkedin.com/in/iarunava/) [](https://twitter.com/amArunava) [## 阿鲁纳瓦(@amArunava) |推特

### 阿鲁纳瓦(@amArunava)的最新推文:“发表了一篇关于@ Sigmoid 函数的中等导数的文章我的第一…

twitter.com](https://twitter.com/amArunava) [](https://www.youtube.com/channel/UC2ZFGaNzZt-sUy2qZT6L7Zw) [## 阿鲁纳瓦

### 订阅深入了解 Python 里的所有东西。

www.youtube.com](https://www.youtube.com/channel/UC2ZFGaNzZt-sUy2qZT6L7Zw) 

给我发消息！我们连线吧！