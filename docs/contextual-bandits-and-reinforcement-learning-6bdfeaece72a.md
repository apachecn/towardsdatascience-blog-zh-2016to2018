# 情境强盗和强化学习

> 原文：<https://towardsdatascience.com/contextual-bandits-and-reinforcement-learning-6bdfeaece72a?source=collection_archive---------0----------------------->

![](img/c1cdc546855eae04c5d1458624767b66.png)

Source: [https://blog.iterable.com/why-you-should-become-a-multi-armed-bandit-1cb6651063f5](https://blog.iterable.com/why-you-should-become-a-multi-armed-bandit-1cb6651063f5)

如果你为你的网站或应用开发个性化的用户体验，上下文强盗可以帮助你。使用上下文相关工具，您可以选择向用户显示哪些内容、排列广告、优化搜索结果、选择在页面上显示的最佳图像等等。

这类算法有很多名字:上下文土匪，多世界测试，联想土匪，部分反馈学习，土匪反馈学习，边信息土匪，土匪反馈多类分类，联想强化学习，一步强化学习。

研究人员从两个不同的角度研究这个问题。你可以把情境土匪想成是多臂土匪的延伸，或者是强化学习的简化版。

“多臂土匪”中的“土匪”来自赌场中使用的“单臂土匪”机器。想象一下，你在一个有许多独臂强盗机的赌场里。每台机器都有不同的获胜概率。你的目标是最大化总支出。你可以拉有限数量的武器，而且你不知道用哪个土匪才能得到最好的赔付。这个问题涉及到探索/剥削的权衡:你应该在尝试不同的强盗之间取得平衡，以了解每台机器的预期支出，但你也想利用你更了解的最好的强盗。这个问题有许多现实世界的应用，包括网站优化，临床试验，自适应路由和金融投资组合设计。你可以把它想成更聪明的 A/B 测试。

多臂 bandit 算法输出一个动作，但是不使用任何关于环境状态(上下文)的信息。例如，如果你使用一个多臂强盗来选择是否向你网站的用户显示猫的图像或狗的图像，即使你知道用户的偏好，你也会做出同样的随机决定。上下文 bandit 通过根据环境状态做出决定来扩展模型。

![](img/6ff5b3ed884eb8faf9667db537dc0498.png)

Source: [https://medium.com/emergent-future/simple-reinforcement-learning-with-tensorflow-part-0-q-learning-with-tables-and-neural-networks-d195264329d0](https://medium.com/emergent-future/simple-reinforcement-learning-with-tensorflow-part-0-q-learning-with-tables-and-neural-networks-d195264329d0)

有了这样的模型，你不仅可以根据以前的观察优化决策，还可以针对每种情况个性化决策。你将向养猫的人展示一只猫的图像，向养狗的人展示一只狗的图像，你可以在一天中的不同时间和一周中的不同日子展示不同的图像。

该算法观察上下文，做出决定，从多个可选择的动作中选择一个动作，并观察该决定的结果。结果决定奖励。目标是最大化平均报酬。

例如，您可以使用上下文 bandit 来选择在网站主页上首先显示的新闻文章，以优化点击率。上下文是关于用户的信息:他们来自哪里，以前访问过的网站页面，设备信息，地理位置等。动作是选择显示什么新闻文章。结果是用户是否点击了链接。奖励是二进制的:没有点击就是 0，有点击就是 1。

如果我们对每个例子的每个可能的行为都有奖励值，我们可以使用任何分类算法，使用上下文数据作为特征，将具有最佳奖励的行为作为标签。挑战在于，我们不知道哪一个动作是最好的，我们只有部分信息:示例中使用的动作的奖励值。你仍然可以使用机器学习模型，但是你必须改变成本函数。一个简单的实现是试图预测回报。

[微软发布了一份白皮书](https://github.com/Microsoft/mwt-ds/raw/master/images/MWT-WhitePaper.pdf),概述了他们实施多世界测试服务的方法和描述。这项服务是由纽约的微软研究小组开发的。此前，该小组的许多研究人员都在雅虎从事该领域的工作。微软多世界测试服务使用 Vowpal Wabbit，这是一个开源库，为上下文土匪实现在线和离线训练算法。离线训练和评估算法在论文[中有描述](https://arxiv.org/pdf/1103.4601.pdf)【双稳健策略评估和学习】(Miroslav Dudik，John Langford，李立宏)。

> “有两种方法可以解决上下文环境中的离线学习。第一种方法，我们称之为直接方法(DM ),根据给定的数据估计奖励函数，并使用这种估计代替实际奖励来评估一组上下文中的策略值。第二种称为反向倾向得分(IPS) (Horvitz & Thompson，1952)，使用重要性加权来校正历史数据中不正确的行为比例。第一种方法需要一个准确的奖励模型，而第二种方法需要一个过去政策的准确模型。一般来说，可能很难准确地对奖励进行建模，所以第一个假设可能过于严格。另一方面，通常可以很好地模拟过去的政策。然而，第二种方法通常存在较大的差异，尤其是当过去的政策与正在评估的政策存在显著差异时。
> 
> 在本文中，我们建议使用双稳健(DR)估计技术来克服这两种现有方法的问题。双重稳健(或双重保护)估计(Cassel 等人，1976；Robins 等人，1994 年；罗宾斯和罗特尼茨基，1995 年；伦瑟福德&大卫教派，2004 年；康和，2007)是不完全数据估计的统计方法，具有一个重要性质:如果两个估计量(DM 和 IPS)中的任何一个是正确的，则估计是无偏的。因此，这种方法增加了得出可靠推论的机会。"

几个关于背景强盗的视频教程:

[ICML 和 KDD 2010 年探索学习教程](http://hunch.net/~exploration_learning/)

[SIGIR 2016 搜索、推荐和广告投放的反事实评估和学习教程](http://www.cs.cornell.edu/~adith/CfactSIGIR2016/)

[在探索中学习](http://videolectures.net/kdd2010_beygelzimer_langford_lte/)

你可以在这里找到很多其他资源的链接。

你可以把强化学习看作是情境强盗的延伸。

![](img/3118d5a1e407d010ce2b30cfb130082c.png)

Source: [https://medium.com/emergent-future/simple-reinforcement-learning-with-tensorflow-part-0-q-learning-with-tables-and-neural-networks-d195264329d0](https://medium.com/emergent-future/simple-reinforcement-learning-with-tensorflow-part-0-q-learning-with-tables-and-neural-networks-d195264329d0)

你仍然有一个代理(策略),它根据环境的状态采取行动，观察回报。不同的是，代理人可以采取多个连续的行动，奖励信息是稀疏的。比如你可以训练一个模特下棋。该模型将使用国际象棋棋盘的状态作为上下文，将决定采取哪些步骤，但它只会在游戏结束时获得奖励:赢、输或平。奖励信息的稀疏性增加了训练模型的难度。你遇到了一个信用分配问题的问题:如何分配信用或指责个人行为。

强化学习算法有很多种。强化学习的延伸之一是深度强化学习。它使用深度神经网络作为系统的一部分。

Arthur Juliani 用 Tensorflow 写了一篇很好的[强化学习教程。](https://medium.com/emergent-future/simple-reinforcement-learning-with-tensorflow-part-0-q-learning-with-tables-and-neural-networks-d195264329d0)

对上下文强盗感兴趣的研究人员似乎更专注于创建具有更好统计质量的算法，例如，后悔保证。后悔是使用最优策略时期望的奖励总和与使用从数据中学习的上下文强盗策略时收集的奖励总和之间的期望差。一些算法在后悔上界上有理论保证。

对强化学习感兴趣的研究人员似乎对将机器学习算法应用于新问题更感兴趣:机器人、自动驾驶汽车、库存管理、交易系统。他们通常专注于算法的开发，这些算法可以改善一些问题的艺术状态。

技术方法也不同。微软多世界测试白皮书描述了使用负 IPS(反向倾向分数)作为损失函数的训练算法。最小化损失函数将导致 IPS 估计量的最大化。我找不到任何人在强化学习算法中使用 IPS 估计器。如果你得到具有策略梯度的强化学习算法，并通过将若干步骤减少到一个来将其简化为上下文 bandit，则该模型将非常类似于监督分类模型。对于损失函数，您将使用交叉熵，但乘以奖励值。

比较两种不同类别的算法是很有趣的，这两种算法集中在一个狭窄的上下文强盗区域。我可能会错过一些东西，因为我还没有深入挖掘理论基础。如果你发现这篇文章中有任何遗漏细节的错误，请告诉我。

另请参见:

[深度学习新闻的最佳来源](https://medium.com/@surmenok/best-sources-of-deep-learning-news-fbc98815bad3)

[杰夫·迪恩关于大规模深度学习的演讲](https://becominghuman.ai/jeff-deans-talk-on-large-scale-deep-learning-171fb8c8ac57)

[用于文本分类的字符级卷积网络](https://medium.com/@surmenok/character-level-convolutional-networks-for-text-classification-d582c0c36ace)

*文章原载于*[*http://Pavel . surmenok . com/2017/08/26/contextual-bottom-and-reinforcement-learning/*](http://pavel.surmenok.com/2017/08/26/contextual-bandits-and-reinforcement-learning/)