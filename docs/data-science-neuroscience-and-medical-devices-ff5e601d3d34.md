# 数据科学、神经科学和医疗设备

> 原文：<https://towardsdatascience.com/data-science-neuroscience-and-medical-devices-ff5e601d3d34?source=collection_archive---------9----------------------->

首先，我想感谢那些在过去的一年里关注我在《走向数据科学》中的文章的人。我已经达到了 2000 名追随者的里程碑，这在很大程度上要感谢你愿意阅读我的文章。

![](img/883aaf06f90313c3381b50b3349d561f.png)

你们很多人不知道的是，除了我在人工神经网络和优化方面的工作，我还有十年的经验与[马修·勒梅](https://medium.com/u/f84a70d8f74?source=post_page-----ff5e601d3d34--------------------------------)一起研究生物神经网络(大脑！)以及如何修改。

2010 年，我与人共同创立了 [Nuraleve](http://nuraleve.com) ，这是一家神经技术研究和医疗器械公司，专门从事神经调节治疗成瘾、慢性疼痛和其他疾病的临床和商业进展。我们是一个由研究人员、医生和疼痛管理专家组成的团队，乐观地认为当时的现有科学指向一个全新的一线治疗类别。神经科学的重大突破正在改变我们认为可能的事情，所以我们决定加入这场竞争，开发一种治疗神经疾病的医疗设备。正如快速经颅直流电刺激(tDCS)和相关协议(如 TMS、tRNS、tACS、DBS、ect、VNS、taVNS 和许多其他方案)今天仍然是活跃的研究领域，仅 tDCS 就有超过 2，000 篇同行评议的出版物，并且多年来有广泛的媒体报道。

在本文中，我想谈谈我使用生物神经网络进行的几个数据科学项目。这包括对大脑的扫描，进入大脑的信号，改变大脑功能，以及从大脑中获取信号。

![](img/d0a8ffdb3ca866007a99d5dacfcdfdc7.png)

在 Nuraleve，我帮助开发了一种用于治疗慢性疼痛、成瘾和其他适应症的非侵入性神经刺激设备。这种经过临床验证的治疗方法用于通过放置在头皮特定位置的电极刺激大脑区域。刺激[运动皮层](https://en.wikipedia.org/wiki/Motor_cortex) (M1)来治疗某种慢性疼痛，而刺激[背外侧前额叶皮层](https://en.wikipedia.org/wiki/Dorsolateral_prefrontal_cortex)(DL-PFC)[来减少吸烟](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC5791546/)和其他毒瘾。还有一个[认知增强协议](https://www.sciencedirect.com/science/article/pii/S1935861X16300560)。

tDCS 通过头皮和大脑发送微弱的电流。最新的科学表明，tDCS 通过修改神经元的放电阈值来改变大脑活动。刺激可以增加皮层兴奋性，引起长时程增强(LTP)。这种类型的刺激导致神经递质谷氨酸的释放，谷氨酸与突触后神经元中的 NMDA 和 AMPA 受体结合，引起 LTP。改变神经元兴奋性受治疗强度和持续时间的影响。刺激是在病人醒着的时候施加的，我可以从第一手经验告诉你，这并不痛苦。使用口腔内电极阵列(见下文),感觉有点像嘴里的流行摇滚，带有金属余味。这有点像试图向你描述一种你从未见过的颜色。

Brain research takes a long time.

Nuraleve 创建了专有的医疗协议和软件，供医疗专业人员在评估和治疗患者时使用，经过多年的 R&D 和保护关键想法的专利，它终于进入了市场。本文中描述的项目都在公共领域。

[](https://patents.google.com/patent/US8942811B2/en) [## US8942811B2 -经颅电流刺激装置和方法-谷歌专利

### 一种用于经颅刺激的装置。

patents.google.com](https://patents.google.com/patent/US8942811B2/en) 

Nuraleve 的设备是 ce 标志，加拿大卫生部批准用于治疗
慢性疼痛、成瘾和其他适应症。欧洲批准的适应症包括慢性疼痛、急性疼痛、成瘾，包括肥胖、酒精、药物(大麻、可卡因、阿片类药物、甲基苯丙胺)、吸烟、抑郁症、中风后康复和认知缺陷。这是一个巨大的市场。

通过结合对照临床试验和专有数据来衡量治疗的临床疗效。这个数据才是我要重点关注的。Nuraleve 的专有应用软件从每个大脑刺激器中捕捉实时数据。对于数据科学家来说，这些数据非常酷。

![](img/f94d210a5ac26299b6d0673ab2767be1.png)

All the stuff used during a typical treatment session.

让我们来讨论一下该设备的功能和测量内容。Nuraleve 的大脑刺激器在大脑中施加直流电，在 3D 模式下，这意味着在治疗过程中，通过皮层的电流密度会有[的变化。当通过脑组织应用这种治疗时，该设备检查以确保它附着在患者身上，并且在这样做时，它每秒钟对大脑进行多次多频扫描。这种安全特性还有一个额外的好处，那就是告诉我们在治疗过程中大脑内部发生了什么。这个领域被称为阻抗谱。](https://www.researchgate.net/figure/TDCS-Current-modeling-Modeling-of-current-flow-when-applying-15-mA-tDCS-for-F4-anodal_fig1_275951776)

![](img/6b1221bf3b459912009a253f96ee89ee.png)

Voltage between electrodes during treatment measured at one frequency. Note the drop in resistance/impedance/voltage as the scalp charges up with ions during treatment,

我与[赫歇尔·凯塔克](https://www.linkedin.com/in/herschel-caytak-67900235/)和我的论文导师教授[米奥德拉克·博利奇博士](http://www.site.uottawa.ca/~mbolic/)一起准备了一份关于生物阻抗的会议海报，查看如上图所示的数据，并找出阻抗随时间的变化如何与头皮、硬脑膜、皮层等的变化相关联。基本上，我们必须考虑有多少治疗穿透了保护大脑的多层外壳。赫歇尔实际上正在写关于神经刺激和光谱学的博士论文。这是一个相当数据密集型的研究领域，使用有限元模型来模拟大脑中发生的事情。

从数据角度来看，大脑扫描端不仅有数据科学工作，大脑刺激端也有数据科学工作。我们对通过脑神经的多电极大脑刺激进行了一系列研究，以获得与大脑的更好连接。简而言之，使用口腔电极通过舌头的连接比通过头骨和头皮的连接“更好”。

[](https://patents.google.com/patent/US8874220B2/en) [## US8874220B2 -神经刺激系统、装置和方法-谷歌专利

### 公开了一种用于个体脑神经刺激的设备，具有用于产生刺激信号的信号驱动单元

patents.google.com](https://patents.google.com/patent/US8874220B2/en) 

在专利中，我们描述了如何使用电极阵列作为大脑的输入来玩游戏。

 [## 低成本双向人机接口——IEEE 会议出版物

### 在过去的十年里，人机界面的开发和应用取得了许多进展…

ieeexplore.ieee.org](https://ieeexplore.ieee.org/abstract/document/5480231/) 

我们认为这个设备不仅仅是一个大脑修改(神经调节)设备，而是一个人机界面，既可以向大脑发送信息，也可以从大脑接收信息。

![](img/7cd1794afbfc76d237a783dec6a97ac8.png)

Polyamide and gold pads were used to make this biocompatible electrode array, More on this process in [this IEEE paper we wrote just about this line of research](https://ieeexplore.ieee.org/abstract/document/5966660/).

正如[埃隆·马斯克如此专业地确定](https://www.theguardian.com/technology/2018/jan/01/elon-musk-neurotechnology-human-enhancement-brain-computer-interfaces)一样，信号来自大脑**的神经接口**将成为游戏改变者(认为让事情发生)，而将信号**发送到大脑**的接口更多的是当今的技术，而不是科幻小说。我们的世界已经有了仿生眼睛、四肢和手，以及市场上其他神经接口的感官替代产品。

在医疗器械研究中，通常会有材料科学的成分。这通常在设备和患者之间的界面处。例如，我参与了几个项目来设计更好的电极。

 [## 经颅直流电刺激用高吸水性聚合物电极

### 高吸水性聚合物(SAP)被认为是经皮刺激电极的一种可行的碳替代品。

ieeexplore.ieee.org](https://ieeexplore.ieee.org/abstract/document/6549760/) 

一些研究涉及数据收集，这里有一个超级有趣的例子:我做了一个记录活蚯蚓大脑信号的项目，在用伏特加麻醉它们使它们更顺从之后。回想起来，这非常有趣，但在当时它并不那么有趣，更多的是“我不敢相信我在做这件事”。

![](img/73be7f14db3dcf0d209765747a70d3ee.png)

This is a screenshot from one of my research reports.

下表是神经改造科学结果的一个很好的例子。所有花哨的曲线和装置都归结为简单的数字和物理相关性测量。

R&D 日常医疗设备中的数据科学远远超出了设备本身。HIPAA 规则要求非常详细的文档和软件验证，以确保医疗记录在设备和云之间的安全存储和传输。此外，临床试验记录必须保持最新，不良事件报告不仅针对该公司的设备，而且针对任何其他公司的设备。因此，定期有大量的统计工作来计算治疗所需的[数量、疗效等等。](https://en.wikipedia.org/wiki/Number_needed_to_treat)

你可以在公司网站上了解更多关于 [Nuraleve 的信息。](http://nuraleve.com)

因此，总而言之，数据科学是医疗设备开发的一个关键方面，它提供了从大数据集获得的洞察力。在下一篇文章中，我将介绍我们已经研究了很长时间的东西，它可以帮助企业在内部审计期间自动分析非结构化报告。

如果你喜欢这篇关于医疗设备数据科学的文章，那么请点击关注按钮，看看我以前读过的一些文章，比如“[如何为人工智能项目定价](https://medium.com/towards-data-science/how-to-price-an-ai-project-f7270cb630a4)”和“[如何聘请人工智能顾问](https://medium.com/towards-data-science/why-hire-an-ai-consultant-50e155e17b39)”除了与业务相关的文章，我还准备了一些关于寻求采用深度机器学习的公司所面临的其他问题的文章，如“[没有云和 API 的机器学习](https://medium.com/@lemaysolutions/locked-in-a-box-machine-learning-without-cloud-or-apis-76cc54e391c8)”

下次见！

-丹尼尔
[丹尼尔@lemay.ai](mailto:daniel@lemay.ai) ←打个招呼。
[LEMAY . AI](https://lemay.ai)
1(855)LEMAY-AI

您可能喜欢的其他文章:

*   [人工智能和不良数据](/artificial-intelligence-and-bad-data-fbf2564c541a)
*   [人工智能的图像数据集](/image-datasets-for-artificial-intelligence-bbb12615edd7)
*   [人工智能:让你的用户给你的数据贴上标签](https://medium.com/towards-data-science/artificial-intelligence-get-your-users-to-label-your-data-b5fa7c0c9e00)