# 2017 年机器智能峰会

> 原文：<https://towardsdatascience.com/machine-intelligence-summit-2017-3e02003fa7ba?source=collection_archive---------3----------------------->

## 阿姆斯特丹#reworkMI 概述

上周，我在阿姆斯特丹参加了第四届机器智能峰会。这项活动的任务是:

> “……让政府官员、学者、企业家、投资者和行业专业人士齐聚一堂，讨论机器学习、深度学习和人工智能的最新突破，并探索它们在工业中的应用及其潜在影响……”

当我在第二天对房间进行快速举手调查时，看起来观众大多来自学术界或创业背景(我承认，我忘了问政府官员)。

完整的时间表和摘要可以在网上[找到](https://www.re-work.co/events/machine-intelligence-summit-amsterdam-2017/schedule)，我通过回顾那里来写这个摘要。以下是演讲的简要概述，按主题分组(而不是按演讲者顺序):

## **计算机视觉**

来自 DFKI 的 Damian Borth】讨论了从形容词-名词对的角度探索由生成对抗网络(GANs)创造的潜在空间，以捕捉图像的主观特征。展示的结果包括一种翻译图像以更好地捕捉某些特征的方法；例如，将图像从“多雾的天空”翻译成“美丽的天空”

来自 EyeEM 的 Appu Shaji 讨论了 [EyeEM](https://www.eyeem.com/) 如何使用计算机视觉为客户创造新体验。他讨论的一个有趣的应用是“EyeEM 选择”功能(见这篇[博客文章](https://petapixel.com/2017/06/23/eyeem-selects-will-show-best-photos-phone/)),它从你的图库中挑选最好的照片来展示。

## **时尚&美丽**

来自欧莱雅的 Panagiotis-Alexandros Bokaris**承诺我们的演讲“不仅仅是关于口红”他的演讲涵盖了增强现实和计算机视觉交叉领域正在进行的广泛工作——并指出这些技术如何进入美容相关产品，如[智能发刷](http://www.loreal.com/media/press-releases/2017/jan/kerastase-and-withings-unveil-worlds-first-smart-hairbrush-at-ces-2017)(和口红)。**

**[来自](https://research.zalando.com/welcome/team/roland-vollgraf/) [Zalando Research](https://research.zalando.com/) 的 Roland Vollgraf 讨论了深度学习在服装项目上的应用。通过学习产品图片、文字描述和供应商属性，他们创建了一个“时尚 DNA”，它可以应用于聚类项目，也可以生成不存在的服装项目..还没有。**

## ****搜索&推荐****

**[Google Research](https://scholar.google.com/citations?user=oCOhXmAAAAAJ)的 Aleksandr Chucklin 做了一个关于搜索引擎结果页面评估的演示(Arxiv 论文[此处](https://arxiv.org/abs/1609.00552))。这是另一个展示搜索结果评估变得多么微妙的演讲——有关于移动/网络搜索差异的研究，关于评估“良好放弃”的研究，以及关于评估混合内容和非线性搜索结果页面的质量的研究。**

**[我](http://nlathia.github.io/)在 Skyscanner 做了一个关于引导目的地发现系统的演讲，并且可能是少数几个没有讨论深度学习应用的人之一(至少现在是这样！).更多信息请见另一篇博文。我想这是我第一次出现在潜望镜上。**

## ****购物&拍卖****

**[来自野餐会](https://www.picnic.nl/)的 Daniel Gleber 从机器学习的角度概述了[野餐会](https://www.picnic.nl/)(荷兰在线购物服务)正在解决的各种问题:它们跨越了整个顾客旅程，等等。例如，这包括扩展客户反馈系统，在该系统中，用户可以请求当前没有库存的商品，从一个松散的渠道变成一个自动分类系统。另一个重点领域是预测交付时间——图表的 y 轴显示的错误度量单位是秒！**

**[来自](https://twitter.com/ptegelaar) [Catawiki](https://www.catawiki.com/) 的 Peter Tegelaar 讨论了深度学习在 Catawiki 的拍卖系统中的各种应用。这包括从使用分类来支持产品分类到估计物品的价格。彼得还公开谈到了深度学习的一个对他们不起作用的应用(估计客户终身价值)，并警告我们不要把一切都当成钉子，现在深度学习是最新的锤子。**

## **机器人和硬件**

**[来自波恩大学的 Sven Behnke 教授](http://www.ais.uni-bonn.de/behnke/)讨论了深度学习在机器人认知方面的应用。这个空间有各种角度。他讨论的许多有趣的应用之一是在 [RoboCup 2016](http://www.robocup2016.org/en/events/amazon-picking-challenge/) 中使用这些方法，参与者与在非结构化环境中执行自动分拣任务的机器人竞争(参见[亚马逊机器人挑战赛](https://www.amazonrobotics.com/#/roboticschallenge))。**

**[阿姆斯特丹大学的 Theo ge vers](https://staff.science.uva.nl/th.gevers/)概述了他的团队在自动物体检测、分割和重建方面的研究。他演示了一个应用程序，可以从实时视频流中自动推断一个人的情绪、年龄和性别。毫不奇怪，第一个问题是关于隐私的。**

**[NVIDIA 的 Adam Grzywaczewski](https://www.crunchbase.com/person/adam-grzywaczewski)概述了由于对深度学习的兴趣激增而出现的基础设施挑战。他收到的一个问题是，英伟达的 GPU 与[谷歌的 TPUs](https://en.wikipedia.org/wiki/Tensor_processing_unit) 相比如何。他的回答？我不知道，我不能出去买谷歌的主板来和我们的比较。**

**[小型机器人公司](http://cargocollective.com/benscottrobinson)的 Ben Scott-Robinson 发表了一篇关于三个机器人的演讲:Tom、Dick 和 Harry，以及它们是如何被设计来帮助农民做从土壤分析到播种的一切事情的。将正在设计的机器人拟人化是一个有趣的焦点，看到这种“农业即服务”模式的发展将会很有趣。**

**[来自](https://nl.linkedin.com/in/kedemdor) [ING](https://www.ing.nl/particulier/index.html) 的 Dor Kedem 发表了一篇关于大型计算集群中资源分配的演讲，以及如何使用机器学习和集群利用模式来节省大量资金，否则这些资金将被用于分配最终闲置的资源。**

## ****文档分析&嵌入****

**来自 Swisscom 的 Sotirios Fokeas 发表了一篇关于使用句子嵌入来提取文档中高覆盖率关键短语的演讲。句子嵌入是一种有趣的方法，最近也受到了很多关注。**

**来自凯泽斯劳滕大学的 Marcus Liwicki 给出了一个在分析文档方面行之有效的方法的概述。这包括著名的长短期记忆(LSTM)网络。**

**LSTMs 也出现在[米哈伊·罗塔鲁](https://scholar.google.nl/citations?user=DkZNtf8AAAAJ&hl=en)关于 Textkernel 的[谈话中；他做了一个关于使用深度学习从候选人的简历中自动解析数据，并使用这些数据将申请人与工作匹配的演讲。这包括考虑独立于语言的模型，并通过嵌入找到职位相似性。](https://www.textkernel.com/)**

## ****用机器学习进行数据分析****

**苏黎世联邦理工学院的 Vahid Moosavi】发表了一篇关于使用数据来模拟城市现象的演讲，比如房地产价格和空气污染流量。他提出的一个关键点是，将数据和机器学习应用于传统上依赖领域知识的问题空间，正在将“专业知识”的定义从拥有已知问题答案的“T2”转向知道提出好问题的“T4”。**

**[来自伦敦大学学院](http://zuserver2.star.ucl.ac.uk/~ingo/index.html)的 Ingo Waldmann 发表了一篇关于使用深度学习来自动分析和发现太阳系外行星的演讲(参见“[梦想大气层](https://arxiv.org/abs/1511.08339)”)，以及天文学如何成为不断增长的研究领域的最新受害者，在这些领域中，数据可用性超过了我们的分析能力。**

## ****用深度学习扩充专家****

**来自 Scyfer 的 Tijmen Blankevoort】谈到了人在回路或主动学习。这包括在医学成像中的应用。[来自](https://scholar.google.com/citations?user=hg2WQI8AAAAJ&hl=en) [COSMONiO](http://www.cosmonio.com/) 的 Larens Hogeweg 也讨论了深度学习在医学成像上的应用。特别是，他专注于从“小数据”(甚至 1 或 2 个样本)中学习，并展示了方法如何能够很好地推广。**

## ****贝叶斯方法****

**[来自](https://twitter.com/cbonnett) [alpha-i](https://alpha-i.co/) 的 Christopher Bonnett 介绍了在深度学习中使用贝叶斯方法来揭示模型预测中的不确定性，以及预测本身。**

**[来自](https://scholar.google.com/citations?user=nQ86angAAAAJ&hl=en) [Freeletics](https://www.freeletics.com/en/) 的 Laith Alkurdi 发表了一篇关于贝叶斯方法如何被用于在 Freeletics 应用程序中建模用户和创建“教练人工智能”的演讲。**

## ****小组讨论****

**第一天的小组讨论重点是投资人工智能。该小组由马修·布拉德利( [Forward Partners](https://forwardpartners.com/team/) )主持，成员包括朱利叶斯·吕斯曼( [Earlybird Venture Capital](https://www.earlybird.com/) )、卡洛斯·爱德华多·埃斯皮纳尔( [Seedcamp](http://seedcamp.com/team/carloseduardoespinal/) )和丹尼尔·格莱伯([野餐](https://www.picnic.nl/))。有趣的片段包括评估初创公司在*访问训练数据*方面的竞争优势，以及在多面手人工智能初创公司和那些在特定垂直领域增长的初创公司之间进行平衡。**

**与会议的其他部分相比，第二场小组讨论有点变化，重点是应对网络安全方面日益增长的威胁。该小组由 Eelco Stofbergen ( [CGI](https://www.cginederland.nl/expert/eelco-stofbergen) )主持，成员包括 Tanya Harris([Harrman Cyber](https://www.harrmancyber.com/))、Ian Bryant([trust worthy Software Foundation](http://tsfdn.org/))和 Colin Williams([University of Warwick](https://www2.warwick.ac.uk/fac/sci/wmg/research/csc/people/cwilliams/))。该小组收到的一个有趣的问题是，在安全攻击/防御中，“好人”还是“坏人”更好地利用了人工智能:答案是“我们必须拭目以待”。**

## ****收尾思路****

**#reworkMI 是一个有趣的会议，我遇到了一群很棒的人。每个人都对深度学习感到兴奋——这并不奇怪——而且这种兴奋主要是从非常技术的角度来看的。然而，许多演讲者都有幻灯片，讲述了他们目前在深度学习系统中面临的挑战(如下所示):这些挑战跨越了整个产品生命周期。仍有许多工作要做。**