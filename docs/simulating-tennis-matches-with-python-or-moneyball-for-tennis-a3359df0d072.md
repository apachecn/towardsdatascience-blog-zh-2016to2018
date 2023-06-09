# 用 Python 或 Moneyball 模拟网球比赛

> 原文：<https://towardsdatascience.com/simulating-tennis-matches-with-python-or-moneyball-for-tennis-a3359df0d072?source=collection_archive---------4----------------------->

![](img/a236c00e142a1477472a9cea916b5cae.png)

Photo by [Moises Alex](https://unsplash.com/@arnok?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

在过去的几个月里，我想了很多关于体育博彩的事情。当然，监管方面的变化是有帮助的，但对我来说，很难看到一个有这么多广泛可用数据的问题而不去尝试。

我是打网球和看网球长大的。当我开始真正进入这项运动时，皮特·桑普拉斯或多或少已经退役，阿加西还没有完全恢复过来，但仍然是一个竞争者，像萨芬这样的新人(刚刚在美国公开赛决赛中战胜桑普拉斯)已经建立了危险和天才球员的声誉。然后是罗杰·费德勒。他颠覆了一切——忘掉世界纪录吧，他提供了一些我所见过的最惊人的比赛数据。这是教科书式的胜利，胜利与失误的比率，第一发球的百分比，以及转换的破发点。虽然你不需要看统计数据，但你可以或多或少地看他的任何比赛，看到任何投篮近乎完美的执行。后来，当我因为极度焦虑而停止观看他的比赛时(特别是当他和拉法比赛时)，我注意到你可以从他的一次发球得分中解读出他的比赛进行得如何。这仍然是我用来追踪他在锦标赛中表现的指标。总之，这是模拟系统的一些个人背景。

我们的目标:我们对预测网球比赛感兴趣。虽然有几种方法可以做到这一点，但我们想采取一种逐点的方法。假设一个球员的发球是 iid。设𝑝𝑠1 是一号选手发球得分的概率，𝑝𝑠2 是二号选手发球得分的概率。如果一个玩家赢得一分，另一个玩家必须失去一分，所以玩家 1 赢得回报的概率就是𝑝𝑟1 = 1 𝑝𝑠2.因此，我们可以忘记返回概率。虽然你可以用递归方程和一点组合学计算出赢得比赛的概率，但我们想用模拟来代替。我们将假设 6 场比赛中的所有盘都以抢七局结束，我们打 3 盘三胜制。

为了增加复杂性，让我们假设赢得点数的概率在“大点数”上变化。一个大的点被定义为一个能让你赢得一局或一盘的点(所以包括抢七局中的设定点)。所以我们把𝑝𝑠1,𝐵和𝑝𝑠2,𝐵的概率相加，作为一名球员在一个大点上发球得分的概率。否则，概率仍然是𝑝𝑠1and 𝑝𝑠2.

我们的目标是用以下方式来编排我们的分数:40–15 | 6:4 6:7 3:3，也就是说，对于正在进行的比赛和抢七局，先列出发球的球员。对于集合(无论是进行中的还是完成的)，首先列出玩家 1，然后是玩家 2。这将有助于我们逐点跟踪游戏，并在调试时使事情变得更容易，因为任何奇怪的分数组合都会脱颖而出，并提示我们逻辑中可能存在的问题。

**概述:**作为一种快速方法，我们将通过设置一系列函数来解决这个问题:

1.  一个函数 **simulate_set** ，它跟踪该组中的游戏，并确定该组是否将进入决胜局
2.  一个名为**的玩家服务器**的功能，为服务器和返回者增加点数，并跟踪大点数
3.  函数**是 BigPoint** ，它更像是一个辅助函数，用于确定和一个伴随函数 **getBigPointProbability，**，它也作为一个辅助函数，如果一个点被确定为大点，则返回新的概率
4.  一个函数 **simulate_tiebreak** ，用于播放决胜局
5.  函数 **getScore** ，其功能相当于记分板
6.  两个函数 **printSetMatchSummary** 和 **pointsMatchSummary** ，它们打印有用的更新，例如“玩家 1 赢了第 16 盘比赛到第 2 盘”和“玩家 1 赢了第 3 盘比赛到第 1 盘”。这些功能并不是明确必需的，但我认为它们提供了一个很好的美学触觉

**构建功能:**

在完成一个模拟或任何代码结构之后，回过头来写它永远也不会抓住整个过程——所有写的和删除的行，所有的“是！”破解逻辑的时刻，最终导致“该死！回到绘图板”运行结果。因此，下面呈现的是必要功能的完整副本:

**控制流程:**

上面的代码有一个运行所有代码的部分。我们考虑了大多数重要参数的默认值，如玩家名、ps1 和 ps2、bigpoint1 和 bigpoint2。我喜欢认为 ps1 和 ps2 是第一发球的百分比，但是我们可以做很多有趣的特征工程来使概率更有洞察力。如果我们想要运行 10 次、100 次、1000 次或更多次此匹配的模拟，以便了解置信区间，我们可以按如下方式更改我们的控制代码:

```
#this control flow module runs 1000 simulations
#and stores the winner of each simulation in winner=[]winner = []
p1 = "A"
p2 = "B"
a = 0.64
b = 0.62
p1_big_point = 0.70
p2_big_point = 0.68#run 1000 runs of the simulation
for ii in range(0, 1000):
    completed_sets = []
    S = 0
    gamesMatch = 0
    pointsMatch1, pointsMatch2 = 0, 0
    setsMatch1, setsMatch2 = 0, 0
    pointsTie1, pointsTie2 = 0, 0
    pointsGame1, pointsGame2 = 0, 0

    while S < 5 and max(setsMatch1, setsMatch2) < 3:
        gamesSet1, gamesSet2, gamesMatch, S, pointsMatch1, pointsMatch2 = simulateSet(a, b, gamesMatch, S, 
                                                                                      pointsMatch1, pointsMatch2, completed_sets)
        print()
        if gamesSet1 == 6 and gamesSet2 == 6:
            pointsTie1, pointsTie2, gamesMatch, pointsMatch1, pointsMatch2 = simulateTiebreaker(p1, p2, a, b, gamesMatch, 
                                                                                                pointsMatch1, pointsMatch2, completed_sets)setsMatch1, setsMatch2 = printSetMatchSummary(p1, p2, gamesSet1, gamesSet2, S, 
                                                      pointsTie1, pointsTie2, setsMatch1, setsMatch2)if gamesSet1 == 6 and gamesSet2 == 6:
            if pointsTie1 > pointsTie2:
                completed_sets.append([gamesSet1+1, gamesSet2])
            else:
                completed_sets.append([gamesSet1, gamesSet2+1])
        else:
            completed_sets.append([gamesSet1, gamesSet2])pointsMatchSummary(p1, p2, setsMatch1, setsMatch2, pointsMatch1, pointsMatch2)
```

当我们的代码运行时，输出如下所示(只是一小段代码)。请注意，在游戏结束时，代码显示玩家 A 与玩家 B 赢得的分数:

```
A 0-0|[0-0]
A 15-0|[0-0]
A 15-15|[0-0]
A 30-15|[0-0]
game point
A 40-15|[0-0]
game point
A 40-30|[0-0]
  A: 4, B: 2
B 0-0|[0-1]
B 0-15|[0-1]
B 0-30|[0-1]
B 15-30|[0-1]
B 15-40|[0-1]
B 30-40|[0-1]
  B: 2, A: 4 -- A broke
A 0-0|[2-0]
A 0-15|[2-0]
A 15-15|[2-0]
A 15-30|[2-0]
A 15-40|[2-0]
  A: 1, B: 4 -- B broke
B 0-0|[1-2]
B 15-0|[1-2]
B 30-0|[1-2]
game point
B 40-0|[1-2]
game point
B 40-15|[1-2]
  B: 4, A: 1
```

**如何利用这个进行投注:**现在，我不打算泄露我所有的秘密，但网球投注是统计方法的一个相当好的目标。我还没有用过这个，但是我不会在全部比赛中下注(或者除此之外),我会用它来赌下一分。给定输出，我们可以解析许多次运行，以查看玩家在 40–15 或 30–30 上获胜的频率。

**这是不是有点简单化了？**

是啊！大部分模拟都是。在这种情况下，我们的重点是获得一些游戏流程的框架，我们缩小了整个比赛的跟踪 ps1 和 ps2。

a.对于模拟的改进，我有一些想法，可以归类为统计/模型改进和编码/软件工程改进:

**统计**:

最初，我们认为我们可以通过整合更多的数据来进行更准确的模拟，而不仅仅是赢得服务器的单一概率。然而，经过更多的思考，我们相信我们仍然可以用奇异的概率工作。为了设置一个合适的开始概率，我们可以从已完成的比赛中获取球员的统计数据，具体来说，重要性的指标将是:第一发球百分比，第一发球得分(%)，第二发球百分比，第二发球得分(%)，转换的破发点(%)，防守的破发点(%)。我们可以对它们进行平均，以获得一个全面的指标，或者继续我们以发球为重点的模拟，并构建一个类似于首次发球百分比/L2 范数(metrics_listed_above)的指标。我们可以对其他指标运行这个作为分子，并对照历史匹配来查看哪个指标具有最强的预测能力。L2 标准的使用将它放在其他度量标准的上下文中。我们还应该实现大点，这样服务器的断点就被认为是大点。

这引出了我们模拟的另一个问题。它关注的是球员发球得分，但在真正的比赛中，得分是波动的。我们使用了一个固定的概率，即使是计算这个固定概率的最好方法也不能真正涵盖比赛中表现起伏的事实。模拟这个问题的一个更准确的方法是使用动态计算的发球获胜概率。**一场网球比赛可以被表示为离散的状态空间。**这使我们能够用马尔可夫链来模拟比赛，因为游戏中的每个点都是一个状态，并且存在进入下一个状态的转移概率，这可以编码到转移矩阵中。与大多数马尔可夫模型一样，实现可能会有点乏味，但值得指出的是，这是可行的，并且可能是利用根据我们上面列出的指标收集的数据的更好方法。

从模拟的想法退一步，如果我们的目标是预测获胜者，我们怀疑网球将是分类算法的一个伟大领域。我们相信分类算法可以适当地利用可以归因于运动员和比赛的丰富的特征集。例如，上面列出的重要指标只是在匹配过程中创建和生成的所有统计数据的子集。同样，每场比赛都有自己的特点，最简单的就是天气条件。我们可以在匹配级别上对齐这些值，即在匹配结束时计算的统计数据，甚至可以下至点级别，以便获得大量数据。查看每个球员的历史比赛，我们可以计算出击败某种类型对手的概率。

然而，所有这些都假设玩家 A 和玩家 B 是有比赛历史的真实玩家，而不仅仅是为了模拟而构建的

**工程**:

为解决这个问题而编写的代码有几个独立的函数和一个控制这些函数之间流程的主循环。虽然这是功能性的，可以作为产品级代码，但我们相信，当以面向对象的方式处理时，解决方案可以更好地被综合。网球适合这种实现方式，因为每个运动员都有不同的属性，而比赛、比赛和比赛都有共享的属性。首先，我们想要创建一个玩家类，它将保存诸如赢得一个点的概率、赢得一个大点的概率、赢得的集合和赢得的游戏等属性。每个玩家都是这个类的对象。我们可以在这个类中实现一个 addPoint 函数，这样每个玩家对象都可以跟踪自己的点。这将有助于我们实现一个事件日志类型的触发器，如果玩家对象的点数达到一定数量，就会发生诸如平局或大比分之类的事件。

接下来，我们想要创建一个类 Match 和类 Set 和 Game，分别扩展 Match 和 Set。与 Player 类类似，会有许多 getter 和 setter 函数来跟踪每个比赛、集合和游戏的属性。其中最重要的是 getScore，其中 Match、Set 和 Game 通过继承拥有自己的副本，因为这个函数在自己的类中，所以不需要函数参数。继承的另一个有趣的优点是 Set 和 Game 中的 isTiebreak 和 playTiebreak 函数。如果一个游戏或一组游戏进入了抢七局和 playTiebreak，isTiebreak 将返回一个布尔值来执行逻辑。这样做的原因是因为在网球比赛中，一场比赛或多或少是一个原子单位，通过这个单位可以组成一组比赛。因此，当两名球员的得分为 40-40 时，他们实际上是在打迷你抢七局，只是得分在平分(D)和优势(A)之间波动，而不是像在一盘结束时的抢七局中那样的数值。理论上，我们可以让 Game 成为基类，Set 将扩展 Game，Match 将扩展 Set，但这还没有完全想好。

我们相信以上是在生产层面上解决这个问题的最佳布局。这为将来提供了更大的灵活性，因为我们可以很容易地为每个类添加更多的属性。然而，所提交的代码只是一个概念证明。我们可以运行模拟，并最终理解定期运行是否有价值。在这个问题的范围之外，如果我们考虑测试模型的有效性，基于函数的脚本方法允许快速的概念验证。一旦我们在我们的模型中发现了有效性，我们就可以以上述面向对象的方式设置生产级代码，同时仍然保持核心程序流逻辑(在这种情况下，用于确定平局打破、大比分、相对于平局和优势的得分变化的参数)并优化我们的功能。

**结论:**

随着模拟复杂性的增加，我认为有必要转向面向对象的方法。如果你对体育博彩不感兴趣，但对编码感兴趣，这可能是一个很好的机会，可以采用函数式方法进行模拟，并将其转化为面向对象的程序，以更好地学习多态性的原则。如果你对体育博彩感兴趣，那么就去获取公开的比赛数据，并改进统计分析。网球是一项伟大的算法博彩运动，因为它有大量的数据和运动的本质，即 1 对 1，而不是模拟一个团队。玩得开心，祝你好运！