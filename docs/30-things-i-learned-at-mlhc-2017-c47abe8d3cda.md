# 我在 2017 MLHC 学到的 30 件事

> 原文：<https://towardsdatascience.com/30-things-i-learned-at-mlhc-2017-c47abe8d3cda?source=collection_archive---------8----------------------->

*注:本帖原载于 irenechen.net*[](http://irenechen.net/)**，我的个人博客。**

*在过去的这个周末，我在 MLHC 2017 展会上了解了很多关于机器学习和医疗保健的知识。这里有 30 个想法和发现，我可以压缩成要点。*

*请原谅(并联系我)在总结或归因的任何错误。*

*![](img/5525fd9b81c82d9cf639b3f112457b4f.png)*

*Lots of healthy snacks at Machine Learning for Healthcare 2017, held at Northeastern University.*

# *医疗保健有什么特别之处？*

*   *数据很多但很难访问！贝丝以色列女执事医疗中心处理 7pb 的患者数据。然而，由于数据可用性挑战或针对罕见疾病，许多论文处理了数千甚至数十名患者的数据集。[📎](http://geekdoctor.blogspot.com)*
*   *FDA 的批准很难，但很重要。虽然最初的过程是艰巨的，但较小的更新(例如，重新训练深度学习模型)只需要通知，但新模型需要重新批准。说服 FDA 的一个方法是展示模型的准确性符合人类专家的变化。[📎](https://www.linkedin.com/in/daniel-golden-7b881859/)*
*   *可自由访问的海量数据集加速了医疗保健领域的机器学习研究，许多被接受的论文使用了[模拟数据](https://mimic.physionet.org/)。[📎](http://mucmd.org/CameraReadySubmissions/65%5CCameraReadySubmission%5Cclinical-intervention-prediction%20(4).pdf) [📎](http://mucmd.org/CameraReadySubmissions/34%5CCameraReadySubmission%5Ccontinuous-state-space%20_FINAL.pdf)*
*   *有效性和可再现性是这个成长领域的当务之急。研究人员使用模拟数据复制了 38 个实验的数据集，发现一半的实验列出了群组大小，复制的群组大小差异超过 25%。[📎](http://mucmd.org/CameraReadySubmissions/77_reproducibility-critical-care.pdf)*

# *可解释性很重要*

*   *“基本原理”是简短、连贯和预测性的短语，可以解释啤酒评论和病理学报告。尽管啤酒评论有助于获得大量带注释的数据，但医疗保健专业人员尤其关心理解**为什么**将黑盒方法应用于病理学报告。[📎](http://people.csail.mit.edu/regina/)*
*   *你可以把可解释性看得更远，并认为可解释的模型“应该适合 Powerpoint 幻灯片，并且可以不用计算器来计算。”扩展[罗生门效应](https://en.wikipedia.org/wiki/Rashomon_effect)，多个模型可以具有相似的性能，但可解释性水平不同。为什么不找一个高性能**和**高解释性的模型？[📎](https://users.cs.duke.edu/~cynthia/)*

# *医疗保健包括医院运营*

*   *用深度传感器监测手部卫生可以预防医院获得性感染——每 25 名患者中就有 1 人受到感染。匿名的深度感知人类在进入和离开病房之前和之后被评估是否正确遵循手部卫生协议。[📎](http://mucmd.org/CameraReadySubmissions/23%5CCameraReadySubmission%5C0023.pdf)*
*   *更好地预测手术持续时间有助于提高医院效率和平衡成本:额外的停机时间没有调度冲突那么糟糕。[📎](http://mucmd.org/CameraReadySubmissions/26%5CCameraReadySubmission%5Ccamera-ready-predicting(3).pdf)*
*   *即使看似简单的机器学习方法也可以极大地改善医院流程，比如在收到的同意书上使用 [OCR](https://en.wikipedia.org/wiki/Optical_character_recognition) 将它们导入正确的患者档案。[📎](http://geekdoctor.blogspot.com/)*
*   *一小时未经治疗的中风会使大脑老化 3.6 年，这凸显了治疗住院中风患者的重要性。[📎](http://www.nyp.org/physician/parks)*

# *强化学习行动*

*   *在移动医疗的应用中，人们必须在及时干预和烦人的通知之间取得平衡。强化学习(RL)方法可以适应用户的偏好，并推荐活动建议和激励消息。在许多统计学上的微妙之处中，RL 必须平衡治疗的负面即时效应和潜在的巨大延迟收益。我们可以通过比例反馈控制减少治疗的后验均值来解决这个问题。[📎](https://dl.dropboxusercontent.com/u/16868661/index.html)*
*   *由于后勤、批准和伦理方面的原因，很难进行对照试验。从观察数据中学习连续状态空间上的治疗策略以模拟 RL 的重复实验是可能的。[📎](https://arxiv.org/pdf/1705.08422.pdf)*
*   *在一项随机试验中，接受雷诺嗪治疗的患者死亡率没有差异；然而，对高危患者使用专门的治疗策略，结果显示出巨大的差异。RL 在构建最佳治疗策略方面取得了成功。[📎](http://imes.mit.edu/people/faculty/stultz-collin/)*

# *计算机视觉的胜利*

*   *视频记录可以使用基于神经网络的姿势估计来加速和标准化对运动障碍的诊断，例如[帕金森病](http://www.mayoclinic.org/diseases-conditions/parkinsons-disease/basics/definition/con-20028488)和[共济失调](http://www.mayoclinic.org/diseases-conditions/ataxia/home/ovc-20311863)。[📎](http://mucmd.org/CameraReadySubmissions/46%5CCameraReadySubmission%5Cmain.pdf)*
*   *姿势估计的另一个有用的应用是使用沙漏网络评估外科医生的技术技能，来自机械土耳其人的工人通过集合投票提供注释数据。[📎](http://mucmd.org/CameraReadySubmissions/25%5CCameraReadySubmission%5Csample.pdf)*
*   *使用 CNN 分割心脏超声波图可以让心脏病专家节省时间和精力。[📎](https://www.linkedin.com/in/daniel-golden-7b881859/)*

# *创造性的方法*

*   *社交媒体帖子历史嵌入可以捕捉用户之间的心理健康和同性恋关系的属性。[📎](http://mucmd.org/CameraReadySubmissions/63%5CCameraReadySubmission%5Cmlhc_2017.pdf)*
*   *一篇[病毒式的博客文章](http://matt.might.net/articles/my-sons-killer/)和激进的搜索引擎优化让一名研究人员找到了与他的幼子患有相同遗传疾病的其他患者。[📎](http://matt.might.net)*
*   *科学文章的众包注释可以帮助创建基因突变和治疗的知识图表。大约 6 个外行人能够用 1 个专家的能力进行注释。[📎](http://matt.might.net/)*
*   *在说服临床医生采用系统时，使用心理学尤其重要:反复的积极反馈有助于缓解对新系统或增加的日志记录机制的抵制。[📎](http://www.nyp.org/physician/parks)*

# *杂项 ML 方法*

*   *为了处理部分丢失的数据标签，仅从被注释的片段子集反向传播丢失。[📎](https://www.linkedin.com/in/daniel-golden-7b881859/)*
*   *[异方差](https://en.wikipedia.org/wiki/Heteroscedasticity)指一个变量的可变性在预测它的第二个变量的值的范围内不相等的现象。手术持续时间永远不会为负，所以我们不会期望误差的可变性遵循类似高斯的东西。使用多层感知器，我们可以估计每个预测点的方差和均值。[📎](http://mucmd.org/CameraReadySubmissions/26%5CCameraReadySubmission%5Ccamera-ready-predicting(3).pdf)*

# *零碎东西*

*   *特别感谢 [Andreas Stuhlmuller](https://stuhlmueller.org/) 用他对 2016 年 NIPS 的[总结启发了这篇文章](https://blog.ought.com/nips-2016-875bb8fadb8c)*
*   *AWS 因其易用性和 HIPAA 合规性安全性获得了大量欢呼。[📎](http://geekdoctor.blogspot.com/) [📎](https://www.linkedin.com/in/daniel-golden-7b881859/)*
*   *和大多数会议一样，MLHC 在推特上很活跃，这也是我如何认识扎卡里·利普顿的原因。*
*   *面粉面包店继续供应美味的三明治和美味的饼干。*
*   *我很好奇 2017 年的 MLHC 将何去何从。我喜欢每份报纸每半天都有大人物主持的形式。会议已经从几年前的 11 人增长到 270 人，观众组合倾向于机器学习研究人员。向前！*

# *评论？有问题吗？我忘记什么了吗？
给我“鼓掌”，给我发电子邮件到[iychen@mit.edu](mailto:iychen@mit.edu)，或者在下面评论！*