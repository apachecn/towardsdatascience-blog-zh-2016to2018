# 向小学生解释人工智能

> 原文：<https://towardsdatascience.com/explaining-ai-to-primary-school-kids-9d875d23ec1e?source=collection_archive---------23----------------------->

最近，我被邀请给五六年级的小学生(9-11 岁)讲计算机和人工智能。我花了很多时间向一系列不同的人解释 AI 和 ML，但从来没有向这么年轻的观众解释过！我在网上找不到太多的灵感，所以想出了自己的素材。这是我的课程计划大纲，每节课我都要花 30 分钟。

**1。简介**
我的日常工作包括语音识别，所以我先介绍自己，然后问全班有多少人曾经对着电脑或设备说话来完成某件事——通常我会让全班大多数人举手。

对于一些背景，我包括一点历史。小学生是在智能手机和语音计算的环境中长大的，他们通常不知道技术发展有多快。在撰写本文时，Y5 类诞生于 2008/2009 年。相比之下，iPhone 于 2008 年推出，Siri 于 2011 年在其上发布。

**2。计算机编程**
第一个讨论的话题是计算机如何通过编写规则(或程序)被编程去做事。用这种方法可以编写非常复杂和看似智能的计算机程序。到了 Y5、6 岁，这个学校的孩子已经上了用 [scratch](https://scratch.mit.edu/) 编程的课。他们知道如何根据指令编写计算机程序，以及如何容易将这些指令弄错，从而使计算机无法按照您的意图运行。

**3。识别猫咪的规则**
人们喜欢在网上分享照片，他们分享了很多猫咪的照片！有一件事可能会让计算机变得人工智能，那就是能够判断一张照片是不是一只猫。我和全班一起头脑风暴，想出一些规则写在黑板上，我们可以用它们来识别照片中的猫。我们在之前的课程中提出了一些规则:

我们不能在计算机中直接实现这些规则，但它们能让人们理解这个想法。

![](img/926b5135e80762b195cf27e27b52df14.png)

接下来我们来看一组播放“猫不猫”的幻灯片。我的幻灯片有一系列图片——有些很容易识别是不是猫，有些是模糊不清或姿势滑稽的猫，还有一些是和猫有一些相同特征的其他动物。他们让孩子们思考如何定义这个任务(大猫算不算猫？)，edge cases(一只猫的画法呢？)以及你真正需要的那种规则(你到底怎么区分小熊猫和猫？).

**4。智能计算机**
在这之后，我问孩子们是否认为我们的规则是好的还是坏的。大多数人承认，我们的规定并未涵盖所有图片，因此本可以更好。

有些任务，如视觉和听觉，是不可能写出计算机可以遵循的规则的。我们需要一种不同的方法。我们让计算机学习如何从数据中完成任务，而不是编写规则。为此，我们采集了大量数据(图像、音频、视频等)。)并让人们给它贴上标签。对于图像，人们可能会给每幅图像贴上什么物体被描绘的标签。从这个“标签数据”的数据库中，我们可以训练一台机器来学习构成猫的图片的模式。一旦计算机从这些训练数据中学习，我们就可以利用计算机建立的模型，用它来识别图片中从未见过的猫。

识别图片中的猫或其他物体可能看起来很无聊，但是有很多方法可以在现实世界中使用这项技术。很多人正在研究的一个例子是[在自动驾驶汽车](https://cs.stanford.edu/people/teichman/papers/arso2011.pdf)中识别出你面前的是什么。另一个是帮助拯救濒危野生动物，通过[识别动物并在野外计数](https://www.nationalgeographic.com/animals/2018/11/artificial-intelligence-counts-wild-animals/)。

**5 .讨论和问题**
我最后讨论了这个班将来会制造什么样的智能机器人。如果我对他们的建议有所了解，我会谈谈那个地区已经在建的项目。

我听到的想法包括:

*   帮助盲人的计算机:例子包括微软的[视觉人工智能](https://www.microsoft.com/en-us/seeing-ai)或[文本到语音](https://en.wikipedia.org/wiki/Speech_synthesis)技术，它可以大声朗读文本
*   太空旅行:我们已经将人类送上了月球，并将火星车送上了火星，但是人工智能可以帮助我们[进行更进一步的](https://spacenews.com/beyond-hal-how-artificial-intelligence-is-changing-space-systems/)探索，比如[火星车导航](https://spectrum.ieee.org/tech-talk/robotics/space-robots/ai-trains-to-guide-planetary-rovers-without-gps)或者[识别太空中的物体](https://rossintelligence.com/ai-space-travel/)，
*   照顾住院病人:计算机可以通过多种方式帮助医院从手术到诊断再到管理，从而节省医生和护士与病人相处的时间。
*   会烹饪的机器人:这种处理原料的机器人很难制造，但是[有一些例子](https://mashable.com/2018/01/22/gammachef-cooking-robot/?europe=true#QH0IvLMWYOqS)
*   做作业的电脑。我通常不会指出像[阿列克谢](https://blog.aboutamazon.com/devices/alexa-gets-even-smarter-thanks-to-you)这样的语音助理已经可以回答很多问题了

当我问班主任时，他们通常只想要一台智能电脑来帮助他们批改作业！

在讨论中，我还加入了其他几点讨论:

*   电脑并不总是对的。当计算机从数据中学习时，我们总是知道它们会犯一些错误。
*   有些事情是电脑做不到的，例如，判断某人是否在说谎。一般来说，如果一个人很难做到，那么机器也很难学习。
*   技术有好的一面，也有坏的一面。有时，班上的人会想出别人觉得令人毛骨悚然的想法，这有助于说明问题。

**额外背景**
对于那些有兴趣了解更多的人来说，图像识别有着悠久的历史，并且在最近几年得到了很好的研究。一个名为 [ImageNet](http://www.image-net.org/) 的图像数据库已经成为许多物体识别学术研究的基础。该数据库有超过 20，000 个类别的数百万张图片。

2012 年，花了 [16000 台电脑才学会如何识别一只猫](http://www.nytimes.com/2012/06/26/technology/in-a-big-network-of-computers-evidence-of-machine-learning.html)。近年来，研究人员也在研究更困难的任务，如[区分 5000 种动物](https://qz.com/954530/five-years-ago-ai-was-struggling-to-identify-cats-now-its-trying-to-tackle-5000-species/)、[识别和跟踪视频中的物体](https://www.theverge.com/2017/3/8/14857126/google-cloud-video-api-ai-machine-learning)或[自动对照片中发生的事情进行深入描述](https://petapixel.com/2016/09/23/googles-image-captioning-ai-can-describe-photos-94-accuracy/)。

*原为 2018 年 12 月 17 日在*[*http://mycomputerdoesntlisten.wordpress.com*](https://mycomputerdoesntlisten.wordpress.com/2018/12/17/explaining-ai-to-primary-school-kids/)*发表。*