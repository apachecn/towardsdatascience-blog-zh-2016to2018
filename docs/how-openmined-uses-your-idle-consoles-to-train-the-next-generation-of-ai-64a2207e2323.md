# OpenMined 如何利用你闲置的游戏机来训练下一代人工智能

> 原文：<https://towardsdatascience.com/how-openmined-uses-your-idle-consoles-to-train-the-next-generation-of-ai-64a2207e2323?source=collection_archive---------2----------------------->

到现在为止，你应该已经听说过很多关于开源组织 OpenMined 的事情了。他们正在研究一种使用区块链和 Unity 游戏引擎来实现个人数据访问民主化的方法。

![](img/347a185cd274b682bf3c869f2ad7213d.png)

**没听说过露天开采吗？没问题，有** [**有**](https://becominghuman.ai/how-openmined-will-revolutionize-data-privacy-protection-and-collection-2a634da76d63) [**很多**](https://hackernoon.com/the-amazing-tech-stack-of-openmined-a4a0b208d62e)**[](https://bitcoinexchangeguide.com/openmined/)****[**篇**](/why-openmined-is-becoming-a-role-model-for-open-source-projects-ae11749fc577) [**出**](http://www.ibtimes.co.uk/openmined-ai-blockchain-data-democracy-revolution-1646035)[**有**](https://becominghuman.ai/introducing-open-mined-decentralised-ai-18017f634a3f) **。随便选一个。********

****[OpenMined](http://openmined.org) 为他们的加密深度学习库 PySyft 使用 [Unity 游戏引擎](https://unity3d.com/)。你现在可能会问自己，为什么深度学习库要用游戏引擎。让我解释一下。****

# ****面向所有人的 GPU 无处不在****

****如果你活跃在深度学习领域，你就知道训练一个 AI 需要大量的处理能力。在大多数情况下，一个好的兼容 CUDA 的 GPU 可以大大加快训练进度。如果你没有合适的硬件，即使训练一个简单的分类器也是一件痛苦的事情。****

******这是露天开采的问题。******

****OpenMined 的目标是日常用户，而不仅仅是数据科学家或深度学习工程师，他们可能无论如何都拥有一个高端 GPU。想想你的父母，你的兄弟姐妹，你的非技术朋友:*他们拥有 CUDA 硬件吗？*****

****如果你读过上面链接的一些文章，你就会知道 OpenMined 的目标是用户在自己的设备上用自己的数据训练共享网络——个人数据不应该离开设备。这就是为什么在大多数人使用的设备上进行培训很重要。****

****通过使用 Unity 游戏引擎，OpenMined 团队能够将他们的代码交叉编译到地球上几乎所有现有的 GPU 上。这意味着培训可以在更多的设备上运行，您可能拥有多台设备:****

*   ******游戏机【The Xbox One X 的 GPU 堪比 **GeForce GTX980** 。此外，游戏机大部分时间都处于闲置状态——所以为什么不用它来训练一些人工智能呢？******
*   ****智能手机/平板电脑**:手机中的图形处理器越来越好——如果你不是一个手机游戏玩家，大部分时间都会闲置。如果你有一部 iPhone 8/8 Plus 或 iPhone X，你的手上也会有专用神经网络处理器 A11 仿生的力量。**
*   ****电脑**:即使你的电脑或笔记本电脑没有最新的 NVIDIA GPU，Unity 也可以在旧的 GPU 和 AMD、Intel 等硬件上训练 AI。—在每个操作系统上。**
*   ****更多:**你的智能电视里面很可能有 GPU，也许你的智能手表也有，而且还会有更多的设备不断到来。**

# **结论**

**我在我的上一篇文章中说过，现在我再说一遍:*open mined 项目的愿景和工具是彻底改变我们收集、保护和保护您的个人数据并将其用于人工智能的方式。*显然，革新我们使用硬件进行训练的方式也是其中的一部分。**

**大部分内容摘自[“为什么是 Unity？”](https://github.com/OpenMined/OpenMined/blob/master/tutorials/WhyUnity.md)open mined GitHub[上的 Markdown 文件](https://github.com/OpenMined/)。如果你想投稿，请访问 OpenMined 的网站获取更多信息，并加入我们的 Slack。我去打个招呼。**