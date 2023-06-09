# 机器学习之旅第 1 部分:简介、动机和路线图

> 原文：<https://towardsdatascience.com/journey-to-machine-learning-part-1-introductions-motivations-and-roadmap-35a438f1e126?source=collection_archive---------4----------------------->

![](img/9d21dc9b3b2898e781e3967e56c21536.png)

# 介绍

如果你[一直在关注新闻](https://www.nytimes.com/2017/10/22/technology/artificial-intelligence-experts-salaries.html)，你很可能已经看到了关于机器学习人才需求量的头条新闻。在最近的 [LinkedIn 经济图表](https://economicgraph.linkedin.com/)报告中，“机器学习工程师”和“数据科学家”是 2017 年增长最快的两个职位(分别增长 9.8 倍和 6.5 倍)。媒体本身充斥着[教程](https://medium.com/@ageitgey/machine-learning-is-fun-part-6-how-to-do-speech-recognition-with-deep-learning-28293c162f7a)、[软件评论](/battle-of-the-deep-learning-frameworks-part-i-cff0e3841750)，以及[有趣应用的故事](https://blog.statsbot.co/deep-learning-achievements-4c563e034257)。尽管有明显的需求，但与软件工程其他领域可用的资源相比，作为局外人实际进入这个领域的资源似乎很少。这就是我写这个系列的原因:作为我从非 CS 背景成为机器学习工程师的旅程的文档。

“但是马特”，你一定会说，“这一点也不奇怪，很多人从其他领域进入机器学习。”

确实有很多非 CS 专业的人进入这个领域。然而，我在大学里并没有申报统计学、数学、物理或电气工程专业。我的背景是分子生物学，你们中的一些人可能已经注意到了，这在 STEM 领域的例子列表中经常被忽略。

![](img/a0a5f58af46fd7aa01309c2b5014f9c6.png)

Credit to [Randall Munroe and XKCD](https://xkcd.com/435/) (had a tough time deciding between this comic and [this one](https://xkcd.com/1838/))

虽然我在本科期间比大多数生物专业的学生更专注于统计学和计算机科学，但与物理学家进入该领域相比，这仍然是一条不寻常的道路(正如 Nathan Yau 的 FlowingData 的这篇可爱的帖子所示)。

# 背景

从高中开始，我就对衰老疾病有一种近乎专一的痴迷。我对机器学习的大量接触是在我本科从事这一领域的研究期间。这是在一个实验室里，将离散的果蝇死亡数据拟合到连续方程，如 gompertz 和 weibull 分布，并使用图像跟踪来测量所述果蝇的身体活动量。在这项研究之外，我还参与了一些项目，比如谷歌学术刮刀，以加快文献综述论文的搜索速度。当时，机器学习似乎只是应用于生物医学研究的另一个有用工具。像其他人一样，我最终意识到这将变得更大，成为未来十年日常生活中不可或缺的技术。我知道我必须认真对待，尽可能熟练地掌握这方面的技能。

但是为什么要完全摆脱衰老呢？为了回答这个问题，我想播放一下我看过的哈佛医学院的 David Sinclair 博士的演讲。在开始谈论他的实验室令人兴奋的研究进展之前，他描述了衰老领域的一场共同斗争。许多实验室专注于该过程的狭窄方面，无论是特定的酶活性、营养信号、基因变化，还是其他无数领域。辛克莱博士提出了盲人和大象的类比，这是因为许多研究人员只关注衰老的狭隘方面，而没有花太多时间来认识整体和部分之间的差异。我觉得现实略有不同(这更像是视力正常的人试图在黑暗中用激光笔而不是手电筒识别一头大象)，但结论仍然是正确的:我们需要更好的工具和方法来解决老龄化等问题。

这一点，再加上其他几个因素，让我意识到单独使用生物科学的湿实验室方法是非常低效的。在治愈和治疗的搜索空间中，许多唾手可得的果实在很久以前就被获得了。仍然存在的挑战包括可能需要大量数据才能诊断的疾病和状况，更不用说治疗了(例如，基因多样的癌症，快速变异的病毒，如艾滋病毒)。[是的，我同意许多其他人的观点，衰老肯定是一种疾病](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC4471741/)，但它也是一种定义模糊的疾病，以各种不同的方式影响着人们。

我决定，如果我要在这个领域或我决定进入的任何其他领域做出巨大贡献，最有效的方法将是致力于增强和自动化数据分析的工具。至少在不久的将来，我必须专注于确保我在机器学习方面的基础是坚实的，然后我才能将重点放在像衰老这样的具体案例上。

# “那么……这个系列到底是关于什么的？”

在过去的一年里，我已经从学习机器学习和自学转向实际上为机器学习项目工作而获得报酬。机器学习工程仍然相对较新，所以我觉得我迄今为止的经验可能对其他试图进入这一领域的人有用。虽然有大量关于特定机器学习技术的列表和视频教程，但没有像网络或移动开发人员那样的职业指南式支持。这就是为什么这篇文章不仅仅是罗列我用来学习的资源。我还将记录我发现的创建投资组合项目的最佳实践，寻找该领域的短期和长期工作，并跟上快速变化的研究前景。我也将从我采访过的比我走得更远的人那里收集智慧的建议。

写这个系列时，我有两个目标:

如果我永久进入机器学习工程的长期目标最终成功，我希望这可以作为其他试图进入该领域的人(以及试图进入斯坦福大学计算机科学硕士或博士学位以下的人)的路线图。在这种情况下，成功的衡量标准包括在一家拥有大型 R&D 部门的顶级技术公司或一家快速增长的初创公司获得一份全职工作(并在几年的时间尺度内为该初创公司的快速增长做出贡献)。如果我能从一个非计算机科学背景的人那里获得成功，那么我提供的资源很可能会让一个有计算机科学背景的人获得更大的成功。

> 你需要表现出来的技术能力水平并没有降低，在你没有学历背景的情况下甚至更高，但这是完全可能的。
> 
> *— Dario Amodei，博士，OpenAI 研究员，* [*在没有机器学习博士学位的情况下进入该领域*](https://80000hours.org/2017/07/podcast-the-world-needs-ai-researchers-heres-how-to-become-one/#full-transcript)

如果我最终*没有*成功(由某种经验证据来定义，即我试图做的事情是不可能的，或者我离这个领域太远)，我希望这可以作为一个记录，其他人可以指出应该避免什么。这种结果不太可能发生，因为我已经开始为做机器学习工作获得报酬了(这些帖子赶上现在可能还需要一段时间)，但还有更高的目标要爬。我不想过早庆祝。

![](img/522c1ca12cce3f321d80fb87c2f6e414.png)

记住这一点，下面是本系列其余部分的暂定路线图:

1.  介绍和动机(即你现在正在阅读的内容)
2.  机器学习工程技能清单
3.  提高生产力和学习的习惯
4.  软件和硬件资源
5.  寻找导师和沉浸感
6.  阅读研究论文(和一些每个人都应该知道的)
7.  解决问题的方法和工作流程
8.  作为机器学习工程师的自由职业者
9.  人工智能和机器学习工作联合集的实际考虑
10.  你应该熟悉的团体和人
11.  机器学习工程师职位面试

这些帖子的大部分内容已经写好了，尽管这些帖子的顺序和数量可能会改变。请继续关注更多更新，并随时回复您想看的内容的建议。