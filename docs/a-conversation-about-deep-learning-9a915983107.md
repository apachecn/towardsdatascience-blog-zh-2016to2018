# 关于深度学习的对话

> 原文：<https://towardsdatascience.com/a-conversation-about-deep-learning-9a915983107?source=collection_archive---------11----------------------->

## 有人无意中听到两个人谈论深度学习，让我知道每一个小细节。其中一个完全反对深度学习，另一个不仅是该领域的捍卫者，而且真的知道这个主题。

![](img/dae8c6d66aa91982bc447eaf48c9a877.png)

为了保密，我称第一个人为 A 先生(“憎恨者”)和 B 女士(“专家”)。

**这是 A 先生:**

> **嗨！**

这位是 B 女士:

> **嗨！**

> 甲先生:你好乙！你好吗

> B 女士:棒极了 A！很高兴见到你。我没想到会在深度学习会议上见到你。

> A 先生:是的，我来看看这些疯子在说些什么，我是对的。深度学习烂透了。这只是我在大学学习的关于神经网络的老东西，现在你添加更多的层，就是这样！没什么有趣的东西可看。

> 女士 B:哇，这是一个大的声明，我很抱歉你看到这样的事情。实际上，深度学习只是人工智能世界中正在发生的事情的一个品牌。你可以称之为“炒作”，但那里有非常重要的发展。你想来我的电脑吗？我带你去看。

> 我不知道重点，但是我现在没有别的地方可去，我们走吧。

> B 女士:那么，让我们从基础开始，你对深度学习了解多少？

> 答:我读到过，它们是一些模拟我们大脑工作方式的算法，这就是“神经网络”这个名称的来源，我们的大脑。他们找到了一种方法将其抽象成数学、微积分和代码。然后，有了它，系统可以像我们一样学习，或者他们这么说，使用这些老式的神经网络来解决系统遇到的一些问题。哦，顺便说一下，他们不知道如何解释他们的模型，只是另一个黑盒。

> B 女士:好的。我想你可能在读一些关于深度学习的奇怪的东西。我从基础开始。在机器学习的上下文中，“学习”一词描述了一个自动搜索过程，以更好地表示您正在分析和研究的数据。看看我在网上找到的这个例子:
> 
> 假设我告诉你，我想让你画一条线，将这个图的蓝色圆圈和绿色三角形分开:

![](img/36bda2122db9f3ebc74004c77fb2e037.png)

> 你会怎么做？

> A 先生:再问一下线的定义是什么？

> 女士 B:让我们说是直的一维图形，没有厚度，在两个方向上无限延伸。

A:好的，如果是那样的话，这是一个不可能完成的任务。我不能在那里放一条线。

> B 女士:如果我现在展示这张照片，并请你也这样做，你会说什么？

![](img/aa2ff8ea88df3747915f29b3ccf51078.png)

> A:哦，那很简单，中间有一条线就行了。但这是欺骗，这是一个不同的数据集。这一行应该是这样的:

![](img/167246beb7764ea31b0419927649d4a9.png)

> B 女士:很好，但这实际上是我之前介绍过的同一个数据集。

> A 先生:什么？不，另一个是圆形的，这不一样。

> B 女士:我给你解释一下。在这种情况下，我做了一个坐标转换，所以我们可以用一种我们可以画这条线的方式来绘制或表示这些数据。数据是相同的，但是**表示**是不同的。碰巧的是，在新的表示中，画这条线是一项简单的任务。

> A 先生:好的，我明白你的意思。但是这和深度学习有什么关系呢？还是机器学习？

> B 女士:深度学习是机器学习的一个子集。而是表征学习中一个非常特殊的子集。深度学习使我们能够创建一个系统，一个程序，它可以搜索不同的表示(在这种情况下是坐标变化)，然后找到一种方法来计算类别被正确分类的百分比，当我们这样做时，我们正在进行深度学习，当然还有机器学习。

> A 先生:听起来很棒。但是我们几十年前看到的老式神经网络呢？为什么现在有这么重要？以及它们在深度学习中的作用是什么？

> B 女士:这是一个非常好的问题。我们可以将深度学习定义为使用不同类型的神经网络的表示学习，并优化网络的超参数，以获得(学习)我们数据的最佳表示。但是为什么是神经网络呢？它们非常灵活，让我们找到高度非线性数据的有用表示，就像我之前给你展示的那样。这个概念并不新鲜，但我们现在用来构建它们的工具与过去非常不同。他们拥有人工智能和机器学习领域的所有重大进步。

> A 先生:它们是相同的神经网络，但唯一不同的是工具？比如编程语言？

> B 女士:在某种程度上这是真的。但是还有更多。直到 2000 年代末，我们仍然缺少一种可靠的方法来训练非常深度的神经网络。我们多年来一直在研究这一理论，但现在，随着几项重要的理论和算法改进的发展，硬件的进步，以及数据的指数级生成和积累，深度学习自然而然地适应了这一缺失，从而改变了我们进行机器学习的方式。现代语言允许我们对这些理论部分进行编程，借助分布式计算和大数据的力量，我们成功创造了这场革命。

> A 先生:听起来很有趣。你提到的这些理论上的进步和改进是什么？

> B 女士:它们来自全世界多年的调查。有些来自大型大学或公司，但也有来自开源世界的巨大贡献。没有这些花里胡哨的补助金什么的。实际上我找到了一个时间线可以帮我解释给你听。看一看:

![](img/84b138a75b43be8fbe40cf342cce1a68.png)

> 你是对的，这是一个古老的理论。但这并不意味着它不再相关。从那里我可以说反向传播的思想，网络参数的更好初始化，更好的激活函数，辍学的概念，以及一些类型的网络，如卷积神经网络，残差网络，区域基 CNN，递归神经网络和生成对抗网络，是我们在深度学习领域取得的最重要的进展之一。我希望我能有时间向你们解释所有的问题，但遗憾的是，我必须赶去参加下一个会议！

> 答:好的，现在我想我更明白“深度学习”是怎么回事了。你从没告诉我深度学习这几个字的意思。

> B 女士:你说的对！深度学习中的**深度**并不是指通过这种方法实现的任何一种更深入的理解；更确切地说，它代表了连续层表示的思想。像这样:

![](img/9082a7df5260ff37148a37406af06a64.png)

> 而**学习**就是我之前告诉你的，它是一个自动搜索过程，以更好地表示我们拥有的数据，目标是理解模式，做出预测，并对我们周围的世界进行建模。

> 甲先生:谢谢乙！在离开我之前，你能推荐一些关于这个主题的好读物吗？

> B 女士:当然可以！我会有一份文件，让我找一下…在这里！请写下它们，并在你离开后锁上门。很高兴见到你！回头再聊！

 [## 如何训练你的深度神经网络

### 为了有效地训练深度神经系统，深度学习中有一些特定的实践是非常值得推荐的。

rishy.github.io](http://rishy.github.io/ml/2017/01/05/how-to-train-your-dnn/) [](http://cs231n.github.io/neural-networks-2/#init) [## 用于视觉识别的 CS231n 卷积神经网络

### 斯坦福 CS231n 课程材料和笔记:视觉识别的卷积神经网络。

cs231n.github.io](http://cs231n.github.io/neural-networks-2/#init) [](https://medium.com/the-theory-of-everything/understanding-activation-functions-in-neural-networks-9491262884e0) [## 理解神经网络中的激活函数

### 最近，我的一个同事问了我几个类似“为什么我们有这么多激活功能？”，“为什么是…

medium.com](https://medium.com/the-theory-of-everything/understanding-activation-functions-in-neural-networks-9491262884e0) [](/activation-functions-neural-networks-1cbd9f8d91d6) [## 激活函数:神经网络

### Sigmoid，tanh，Softmax，ReLU，Leaky ReLU 解释！！！

towardsdatascience.com](/activation-functions-neural-networks-1cbd9f8d91d6) [](https://medium.com/@amarbudhiraja/https-medium-com-amarbudhiraja-learning-less-to-learn-better-dropout-in-deep-machine-learning-74334da4bfc5) [## 为了学得更好而学得更少——在(深度)机器学习中辍学

### 在这篇文章中，我将主要讨论神经网络中的辍学概念，特别是深度网络，然后是…

medium.com](https://medium.com/@amarbudhiraja/https-medium-com-amarbudhiraja-learning-less-to-learn-better-dropout-in-deep-machine-learning-74334da4bfc5) [](https://www.manning.com/books/deep-learning-with-python) [## 使用 Python 进行深度学习

### 我见过的对深度学习最清晰的解释...阅读是一种乐趣。

www.manning.com](https://www.manning.com/books/deep-learning-with-python) [](https://cognitiveclass.ai/courses/introduction-deep-learning/) [## 深度学习基础

### 关于这门课，上一堂速成课，学习内容是什么，以及如何学习更多。深度学习…

cognitiveclass.ai](https://cognitiveclass.ai/courses/introduction-deep-learning/) [](https://www.datacamp.com/courses/deep-learning-in-python) [## Python 中的深度学习| DataCamp

### 深度学习是机器人等不同领域中最令人兴奋的能力背后的机器学习技术…

www.datacamp.com](https://www.datacamp.com/courses/deep-learning-in-python) [](/a-weird-introduction-to-deep-learning-7828803693b0) [## 深度学习的“怪异”介绍

### 有关于深度学习的惊人介绍、课程和博文。但这是一种不同的介绍…

towardsdatascience.com](/a-weird-introduction-to-deep-learning-7828803693b0) [](http://deepcognition.ai/) [## 主页

### 我们想邀请您在 3 月 26 日至 29 日的 GPU 技术大会上加入 Deep Cognition 的团队，展位号为 1035…

deepcognition.ai](http://deepcognition.ai/) [](/deep-learning-with-apache-spark-part-1-6d397c16abd) [## 使用 Apache Spark 进行深度学习—第 1 部分

### 第一部分全面讨论了如何使用 Apache Spark 进行分布式深度学习。这一部分:什么是火花…

towardsdatascience.com](/deep-learning-with-apache-spark-part-1-6d397c16abd) [](/deep-learning-with-apache-spark-part-2-2a2938a36d35) [## 使用 Apache Spark 进行深度学习—第 2 部分

### 第二部分全面讨论了如何使用 Apache Spark 进行分布式深度学习。我将完全专注于…

towardsdatascience.com](/deep-learning-with-apache-spark-part-2-2a2938a36d35) [](https://www.deeplearning.ai) [## deeplearning.ai

### 探索 AI 前沿。

www.deeplearning.ai](https://www.deeplearning.ai) [](https://www.youtube.com/channel/UCWN3xxRkmTPmbKwht9FuE5A) [## 西拉伊·拉瓦尔

### 我是西拉杰。我正在为激励和教育开发人员构建人工智能而奋斗。游戏、音乐、聊天机器人…

www.youtube.com](https://www.youtube.com/channel/UCWN3xxRkmTPmbKwht9FuE5A) [](https://becominghuman.ai/deep-learning-made-easy-with-deep-cognition-403fbe445351) [## 深度认知让深度学习变得简单

### 在过去的一个月里，我有幸见到了 DeepCognition.ai 的创始人

becominghuman.ai](https://becominghuman.ai/deep-learning-made-easy-with-deep-cognition-403fbe445351) [](https://medium.freecodecamp.org/want-to-know-how-deep-learning-works-heres-a-quick-guide-for-everyone-1aedeca88076) [## 想知道深度学习是如何工作的？下面给大家一个快速指南。

### 人工智能(AI)和机器学习(ML)是目前最热门的话题。

medium.freecodecamp.org](https://medium.freecodecamp.org/want-to-know-how-deep-learning-works-heres-a-quick-guide-for-everyone-1aedeca88076) 

感谢你阅读这篇文章。希望你在这里发现了一些有趣的东西:)

如果您想联系我，请务必在 twitter 上关注我:

[](https://twitter.com/faviovaz) [## 法维奥·巴斯克斯(@法维奥·巴斯克斯)|推特

### Favio Vázquez 的最新推文(@FavioVaz)。数据科学家。物理学家和计算工程师。我有一个…

twitter.com](https://twitter.com/faviovaz) 

和 LinkedIn:

[](http://linkedin.com/in/faviovazquez/) [## Favio Vázquez -首席数据科学家- OXXO | LinkedIn

### 查看 Favio Vázquez 在世界上最大的职业社区 LinkedIn 上的个人资料。Favio 有 17 个工作列在他们的…

linkedin.com](http://linkedin.com/in/faviovazquez/)