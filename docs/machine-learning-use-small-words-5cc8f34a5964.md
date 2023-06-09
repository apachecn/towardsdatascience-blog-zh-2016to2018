# 机器学习:使用小词

> 原文：<https://towardsdatascience.com/machine-learning-use-small-words-5cc8f34a5964?source=collection_archive---------7----------------------->

![](img/c4f978d1d565bdc1acea18e6f4026d38.png)

我喜欢异端这个词。这是一种很酷的说法“跳出框框思考，反对主流”。它很好地描述了当很少有研究人员相信这种方法时，Hinton 和其他人如何推进深度学习研究。像异端邪说这样的大词让我想到用更小的词和更简单的词来描述我们所做的事情。

我们在深度学习人工智能方面做的，和人们认为我们做的，差距很大。在最近的一次机器学习活动中，另一家公司的一名高级研究科学家让我很不爽，因为他不相信马文·明斯基和他那一代人在几十年前开创了人工智能领域。客户不关心这些东西。就我个人而言，我更喜欢艾伦·图灵，但他的观点很公平。从 20 世纪 90 年代开始，我通常会想到辛顿、本吉奥和勒村等等。我钦佩的人是那些在失去所有希望时仍有梦想的人；像辛顿、尼克·博斯特伦和埃隆·马斯克这样默默无闻的人。我们这些技术型的人倾向于带着一种技术来参加舞会，直奔有刺的潘趣酒，然后立刻抛弃我们的约会，去找房间里最热门的技术。以这种方式经营有正当的理由，但可能有点残酷。在学术界之外，如果你过于怀旧和忠于旧技术，你很快就会变得无关紧要。问问你的铁匠就知道了。

在机器学习时代，历史发展得太快，几乎无法跟上，我们的客户依赖我们作为顾问来提取关键信息和能力，而不是给他们一个历史背景。我花了很多时间学习最新的东西。

过去几年来，我一直非常关注深度学习的进展，现在，在被深度学习超越所有其他技术的[意外成功击败和血洗之后，正统数学和统计学的“旧”智慧正在悄悄回归主流。“真正的”数学家正在夺回汽车的控制权。我认为从现在开始机器学习的改进将遵循类似于软件编译器的分布模型。数学专家将制作](http://people.idsia.ch/~juergen/deep-learning-overview.html)[新的魔盒](https://keras.io/getting-started/sequential-model-guide/)，我们可以在像 Keras 这样的框架中使用，类似于 [gcc 编译器](https://gcc.gnu.org)的贡献者如何将接口暴露给高级语言并抽象掉所有低级的东西。

事实证明，进步不止朝着一个方向发展。有新类型的神经架构(例如 Hinton 的[胶囊网络与 s .卷积网络](https://hackernoon.com/what-is-a-capsnet-or-capsule-network-2bfbe48769cc))，但也出现了最先进的“旧”模型，用作我们现有模型的升级。例如，新的[贝叶斯 BNN](https://eng.uber.com/neural-networks-uncertainty-estimation/) 通过网络传播不确定性，而不仅仅依赖于网络输出的梯度损失。这比 DNN 模型强大得多，因为我们可以对事情的概率做出断言，而不是谈论模型与数据的吻合程度。在 DNN 中测量损失的问题在于，它会导致对训练数据的过度拟合，而不是对数据集进行概化。数据科学已经发明了对抗过度拟合的方法，但基本思想是，反向传播标量输出上的梯度是**不足以谈论我们对深度学习系统的答案有多确定的信息**。这留下了仍然真实存在的大错误的可能性，例如对深度学习系统的[像素攻击](https://arxiv.org/abs/1612.06299)。

![](img/31f50051ca4758d6d988b2c35bbce94c.png)![](img/d5f33ca1b427baad880e3e529b3ac386.png)

Examples of a good model with some error (left) and an overfitting model with lower error (right). Credit: [https://www.cse.unsw.edu.au/~cs9417ml/MLP2/BackPropagation.html](https://www.cse.unsw.edu.au/~cs9417ml/MLP2/BackPropagation.html)

像甘一样，是对的改进，因为它模拟了真实世界的东西，这是不确定的，并遵循概率密度曲线:[概率分布](https://www.youtube.com/watch?v=Sw9r8CL98N0)。与较新的深度学习材料相比，变量的联合概率是旧的，然而通过提供不确定性测量，BNN 提供了比简单地最小化/观察模型误差更好的模型。我猜深度学习的异端邪说有它的局限性，钟摆现在摆回到有利于纯数学的家伙(和女孩)身上，他们将应用一个巨大的经典数学方法库来击败深度学习，使其从它所占据的霸道地位上退下来。我认为，从大的方面来看，新方法将会增强，而不是削弱现有的深度学习技术。这是一个激动人心的时刻。

从基础 R&D 的飞速发展中退一步，想想我们如何用不那么复杂的语言来表达这些来自研究界的简单想法。作为一名研究深度学习技术的工程师，我认为大型组织是时候将深度学习视为认知计算策略的一个组成部分，而不是一种数据建模工具。这种全面的数据方法包括融合内部企业数据和外部开源数据，然后使用工作流和深度机器学习生成见解。要做到这一切，你不需要华丽的词藻。相反，你需要从数据中获得洞察力。我在 LinkedIn 上看到一个很好的引用，称之为“自我意识数据”。不知何故，许多公司都忘记了，尽管机器学习在从数据中学习方面非常出色，但公司仍然需要理解并利用数据来洞察和采取行动，而不是数字。

采用机器学习的企业应该考虑 ML 是更大的**企业工具箱**的一小部分。BNNs 之类的“东西”代表工具箱 ML 隔间里的新扳手。但是不要被工具箱里闪亮的扳手分心。你关心的是**用工具箱**来修补漏洞和制造高速汽车。没人在乎你冲过终点线时用的是什么扳手。

当你开发了一个新的模型，可以综合你不知道的信息，这总是很棒的。不只是拟合数据，你会学到一些新的和意想不到的东西。我计划写更多关于**认知计算**以及它如何改变做生意的方式。

在那之前，编码快乐！

-丹尼尔
[丹尼尔@lemay.ai](mailto:daniel@lemay.ai) ←打个招呼。
[LEMAY . AI](https://lemay.ai)
1(855)LEMAY-AI

您可能喜欢的其他文章:

*   [人工智能和不良数据](/artificial-intelligence-and-bad-data-fbf2564c541a)
*   [人工智能:超参数](/artificial-intelligence-hyperparameters-48fa29daa516)
*   [人工智能:让你的用户给你的数据贴上标签](https://medium.com/towards-data-science/artificial-intelligence-get-your-users-to-label-your-data-b5fa7c0c9e00)