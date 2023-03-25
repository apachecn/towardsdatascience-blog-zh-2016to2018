# Python 中的整数编程

> 原文：<https://towardsdatascience.com/integer-programming-in-python-1cbdfa240df2?source=collection_archive---------0----------------------->

![](img/3843e623d4ca8c9b141d67c970cef175.png)

We’ll use integer programming to make optimal decisions. Photo from [Unsplash](https://unsplash.com/photos/u0vgcIOQG08)

整数规划(IP)问题是优化问题，其中所有变量都被约束为整数。知识产权问题是如何最好地分配资源的有用的数学模型。假设您正在为一位政治候选人组织一场营销活动，您正在决定向哪些选民发送营销材料。你可以给每个选民发一张醒目的传单、一本解释你议程的详细小册子，或者一张保险杠贴纸(或者三者结合)。如果你有办法根据人们收到的营销材料来衡量他们投票给你的候选人的可能性，你会如何决定发送哪些材料，同时注意不要超出你的供应量？

传统的优化算法假设变量可以采用浮点值，但在我们的情况下，给某人送半个保险杠贴纸或四分之三的小册子是不合理的。我们将使用一个名为 [cvxpy](http://www.cvxpy.org/en/latest/index.html) 的特殊 python 包来解决我们的问题，这样解决方案才有意义。

我将向您展示如何使用 cvxpy 来解决政治候选人问题，但是我将首先从一个称为背包问题的简单问题开始，向您展示 cvxpy 语法是如何工作的。

## 背包问题

让我们假设你要去远足，你正在计划你可以带哪些物品。每个物体都有重量，单位是磅 w_i，会给你 u_i 单位的效用。你想把它们都带走，但是你的背包只能装 P 磅。假设你要么拿一个对象，要么不拿。你的目标是在不超过包的重量限制的情况下最大化你的效用。

cvxpy 问题有三个部分:

1.  **创建变量:**我们将用一个由 1 和 0 组成的向量来表示我们的选择。1 表示我们选择了那个对象，0 表示我们把它留在了家里。我们用 cvxpy 构造了一个只能取 1 和 0 的变量。布尔对象。
2.  **指定约束:**我们只需要确保我们的对象的总和不超过重量限制 p。我们可以使用选择向量和权重向量的点积来计算我们的对象的总重量。请注意，cvxpy 重载了*运算符来执行矩阵乘法。
3.  **公式化目标函数:**我们想要找到最大化我们效用的选择。任何给定选择的效用是选择向量和效用向量的点积。

Complete cvxpy code for the Knapsack problem

一旦我们有了一个成本函数和约束，我们就把它们传递给一个 cvxpy 问题对象。在这种情况下，我们已经告诉 cvxpy，我们试图用 cvxpy.Maximize 最大化效用。之后，我们可以通过查看选择向量的值属性来检查它的最优值。

```
print(selection.value)matrix([[1.],
        [1.],
        [1.],
        [1.],
        [0.],
        [1.],
        [0.],
        [0.],
        [0.],
        [0.]])
```

我们选择了前四项和第六项。这是有道理的，因为这些有很高的实用重量比，而不会太重。

## 营销问题

现在我们已经介绍了基本的 cvxpy 语法，我们可以为我们的政治候选人解决营销优化问题。比方说，我们有一个模型，它接受一个选民的属性，并预测他们为我们发送给他们的营销材料的每个组合投票给我们的候选人的概率。我在这里使用了假数据，但让我们假设模型输出以下概率:

```
print(test_probs[0:5])array([[0.0001, 0.0001, 0.3   , 0.0001, 0.2   , 0.0001, 0.2   , 0.3   ],
       [0.1   , 0.1   , 0.1   , 0.2   , 0.2   , 0.2   , 0.1   , 0.0001],
       [0.1   , 0.0001, 0.2   , 0.0001, 0.1   , 0.2   , 0.0001, 0.4   ],
       [0.3   , 0.0001, 0.0001, 0.2   , 0.1   , 0.2   , 0.2   , 0.0001],
       [0.2   , 0.3   , 0.1   , 0.0001, 0.2   , 0.1   , 0.1   , 0.0001]])
```

每个成分有八个总概率，因为我们可以发送给个人的材料总共有八种组合。以下是每个 1 x 8 向量的条目所代表的内容:

【1 张传单，1 张小册子，1 张保险杠贴纸，传单和小册子，传单和保险杠贴纸，小册子和保险杠贴纸，三个都有，无】。

例如，如果第一个选民收到传单或小册子，他有 0.0001 的概率投票给我们的候选人，但如果我们给他寄去保险杠贴纸，他有 0.3 的概率投票给我们的候选人。

在我们进入 cvxpy 代码之前，我们将通过取负对数把这些概率转化为成本。这使得数学结果更好，它有一个很好的解释:如果概率接近 1，负对数将接近 0。这意味着向选民发送特定组合的材料几乎没有成本，因为我们确信这将引导他们投票给我们的候选人。如果概率接近于 0，反之亦然。

```
#clipping so that we don't take log of 0 or 1
test_probs = np.clip(test_probs, 0.0001, 0.9999)#turning into costs
model_costs = -np.log10(test_probs)
```

最后，假设我们不能发送超过 150 份传单、80 份小册子和 25 张保险杠贴纸。

```
supply = np.atleast_2d([150, 80, 25])
```

现在我们已经完成了所有的设置，我们可以开始有趣的部分了:

1.  **创建变量:**我们将使用 cvxpy。因为我们在这里只能做二进制选择。我们将指定它必须与我们的概率矩阵形状相同:

```
selection = cvxpy.Bool(*test_probs.shape)
```

2.**指定约束:**我们的选择变量将只告诉我们为每个组成部分做了 8 个选择中的哪一个，但它不会告诉我们决定给他们发送多少个材料。我们需要一种方法将我们的 1 x 8 选择向量转换成 1 x 3 向量。我们可以通过将选择向量乘以以下矩阵来实现:

```
TRANSFORMER = np.array([[1,0,0],
                        [0,1,0],
                        [0,0,1],
                        [1,1,0],
                        [1,0,1],
                        [0,1,1],
                        [1,1,1],
                        [0,0,0]])
```

如果这一部分有一点混乱，请解出下面的例子:

```
print(np.dot(np.array([0,0,0,1,0,0,0,0]), TRANSFORMER))array([1, 1, 0])
```

我们的决策向量的第四个条目表示向选民发送传单和小册子，乘以 TRANSFORMER 告诉我们的就是这一点！所以我们会告诉 cvxpy，我们的选择矩阵乘以 transformer 不能超过我们的电源:

```
supply_constraint = cvxpy.sum_entries(selection * TRANSFORMER, axis=0) <= supply
```

我用 cvxpy.sum_entries 对这些行求和，以合计我们发送给所有委托人的材料总数。

我们还需要确保每个成分只做一次选择，否则求解器可以通过不向任何人发送任何东西来实现零成本。

```
# We must make our choice per constituent
# remember that the last column is for "no materials"feasibility_constraint = cvxpy.sum_entries(selection, axis=1) == 1constraints = [supply_constraint, feasibility_constraint]
```

3.**公式化目标函数:**我们任务的总成本将是我们为每个组成部分所发生的成本的总和。我们将使用 cvxpy.mul_elemwise 函数将我们的选择矩阵乘以我们的成本矩阵，这将为每个成分选择成本，cvxpy.sum_elemwise 函数将通过将各个成本相加来计算总成本。

```
cost = cvxpy.sum_entries(cvxpy.mul_elemwise(model_costs, selection))
```

最后一步是创建 cvxpy。问题并解决它。

```
# Solving the problemproblem = cvxpy.Problem(cvxpy.Minimize(cost), constraints=constraints)problem.solve(solver=cvxpy.GLPK_MI)
```

就是这样！下面是我们期末作业的快照。我们决定不给第一个选民寄任何材料。这是有道理的，因为无论我们给他们寄去保险杠贴纸还是什么都不寄，他们投票给我们候选人的概率都是 0.3。

事实证明，最佳作业耗尽了我们的小册子和保险杠贴纸，但只用了总共 150 张传单中的 83 张。我们应该告诉我们的候选人，她的传单没有她想象的那么有说服力。

```
print(hard_assignments.value[0:5])matrix([[0., 0., 0., 0., 0., 0., 0., 1.],
        [0., 0., 0., 1., 0., 0., 0., 0.],
        [0., 0., 0., 0., 0., 0., 0., 1.],
        [1., 0., 0., 0., 0., 0., 0., 0.],
        [1., 0., 0., 0., 0., 0., 0., 0.]])print(np.dot(hard_assignments.value, TRANSFORMER).sum(axis=0))matrix([[83., 80., 25.]])
```

以下是打包在一起的所有代码:

## 结束语

我希望您喜欢学习整数编程问题以及如何用 Python 解决这些问题。信不信由你，我们已经涵盖了你解决自己的优化问题所需的大约 80%的 cvxpy 知识。我鼓励你阅读官方的[文档](http://www.cvxpy.org/en/latest/tutorial/functions/index.html)来了解剩下的 20%。CVXPY 能解决的不仅仅是 IP 问题，看看他们的[教程](http://www.cvxpy.org/en/latest/examples/index.html)页面，看看 cvxpy 还能解决什么问题。

要安装 cvxpy，请遵循他们网站上的[说明](http://www.cvxpy.org/en/latest/install/index.html)。我还会安装 [cvxopt](http://cvxopt.org/install/index.html) 来确保 cvxpy 附带的所有解算器都能在你的机器上工作

我们已经指定 cvxpy 应该在求解方法中使用 GLPK _ 米求解器。这是一个专门为 IP 问题设计的求解器。在你解决你自己的问题之前，参考这个[表](http://www.cvxpy.org/en/latest/tutorial/advanced/)来看看哪个预先打包的 cvpxy 解算器最适合你的问题。

快乐优化！