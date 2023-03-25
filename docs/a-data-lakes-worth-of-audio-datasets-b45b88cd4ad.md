# 超过 1.5 TB 的标记音频数据集

> 原文：<https://towardsdatascience.com/a-data-lakes-worth-of-audio-datasets-b45b88cd4ad?source=collection_archive---------5----------------------->

![](img/3eeb2b08641a602ec5e520756ba0ab95.png)

## 我在音频研究中使用的 25 个大型音频数据集列表

在 Wonder Technologies，我们花了大量时间来构建通过音频理解世界的深度学习系统。从基于深度学习的语音提取到教计算机如何读取我们的情绪，我们需要使用大量数据来提供即使在最疯狂的声音环境中也能工作的 API。这里有一个数据集列表，我发现它对我们的研究非常有用，我个人用它来使我的音频相关模型在现实环境中表现得更好。

> *尝试构建自定义数据集？不确定从哪里开始？和我一起进行 30 分钟的一对一谈话，谈谈你的项目。* [报名时间段](https://calendly.com/cdossman/cdossman-consultation)

# **音乐数据集**

## [免费音乐存档](https://github.com/mdeff/fma)

FMA 是一个用于音乐分析的数据集。数据集由全长和 HQ 音频、预先计算的特征以及音轨和用户级元数据组成。它是为评估[音乐信息检索(MIR)](https://en.wikipedia.org/wiki/Music_information_retrieval) 中的几项任务而创建的开放数据集。

这个很大，差不多有 1000 GB。

## [百万首歌曲数据集](https://labrosa.ee.columbia.edu/millionsong/)

百万歌曲数据集是一百万首当代流行音乐曲目的音频特征和元数据的免费收集。数据集的核心是一百万首歌曲的特征分析和元数据。数据集不包含任何音频，仅包含派生要素。使用哥伦比亚大学提供的代码，可以从 7digital 等服务中获取样本音频。这个数据集的大小大约是 **280 GB。**

> [***6 大小抄新手机器学习工程师需要***](https://medium.com/ai³-theory-practice-business/top-6-cheat-sheets-novice-machine-engineers-need-5ea43d1be3de)

# **语音数据集**

## [自由口语数字数据集](https://github.com/Jakobovski/free-spoken-digit-dataset)

这个是为了解决在音频样本中识别语音数字的任务而创建的。这是一个开放的数据集，因此希望随着人们不断贡献更多的样本，它将继续增长。目前，它包含以下特征:1) 3 个扬声器 2) 1，500 个录音(每个扬声器每个数字 50 个)3)英语发音。这是一个非常小的集合-大约 10 MB 大小。

## [LibriSpeech](http://www.openslr.org/12/)

这个数据集是大约 1000 个小时的英语演讲的大规模语料库。数据来源于 LibriVox 项目的有声书籍，大小为 60 GB。

## [沃克斯勒博](http://www.robots.ox.ac.uk/~vgg/data/voxceleb/)

VoxCeleb 是一个大规模的说话人识别数据集。它包含了从 You Tube 视频中提取的 1251 位名人的约 10 万句话语。数据基本上是性别平衡的(男性占 55%)。这些名人的口音、职业和年龄各不相同。开发和测试集之间没有重叠。这是一个有趣的用例，用于隔离和识别声音属于哪个超级巨星。

这套是 150 兆字节的大小，有大约 2000 小时的讲话。

## [口语维基百科全集](https://nats.gitlab.io/swc/)

这是一个英语、德语和荷兰语维基百科的口语文章语料库。数百小时的对齐音频和注释可以映射回原始 HTML。整套大约 38 GB 大小，有音频和无音频两种格式。

## [Flickr 音频字幕语料库](https://groups.csail.mit.edu/sls/downloads/flickraudio/)

8，000 幅自然图像的 40，000 个语音字幕，大小为 4.2 GB。这个语料库是在 2015 年收集的，用于研究无监督语音模式发现的多模态学习方案。

## [泰德-刘姆](http://www.openslr.org/51/)

TED 演讲的音频转录。1495 年，TED 演讲录音以及这些录音的全文转录，由缅因州大学信息实验室(LIUM)创建。

## [语音命令数据集](http://ai.googleblog.com/2017/08/launching-speech-commands-dataset.html)

数据集(1.4 GB)有 65，000 个一秒钟长的 30 个短词的话语，由成千上万不同的人通过 AIY 网站由公众贡献。它是在 Creative Commons-BY 4.0 许可下发布的，随着收到更多的贡献，它将在未来的版本中继续增长。该数据集旨在让您为应用程序构建基本但有用的语音界面，包括“是”、“否”等常用词、数字和方向。用于创建数据的基础设施也是开源的，我们希望看到它被更广泛的社区用来创建他们自己的版本，特别是覆盖服务不足的语言和应用程序。

## [常见语音](https://www.kaggle.com/mozillaorg/common-voice/home)

Common Voice(大小为 12 GB)是用户在 [Common Voice 网站](http://voice.mozilla.org/)上阅读的语音数据的语料库，并且基于来自大量公共领域来源的文本，如用户提交的博客帖子、旧书、电影和其他公共语音语料库。它的主要目的是实现自动语音识别(ASR)系统的训练和测试。

## [波斯语辅音元音组合(PCVC)语音数据集](https://github.com/S-Malek/PCVC)

波斯语辅音元音组合(PCVC)语音数据集是用于语音识别和说话人识别的现代波斯语语音语料库。该数据集包含来自不同说话者的现代波斯语元音和辅音音素组合的声音样本。每个声音样本只包含一个辅音和一个元音，所以它以某种方式被标记在音素级别。该数据集包含 23 个波斯语辅音和 6 个元音。声音样本是元音和辅音的所有可能组合(每个说话者 138 个样本)，长度为 30000 个数据样本。

如果要使用这个数据集，请参考本文:

[Saber MalekzadeH，Mohammad Hossein Gholizadeh，Seyed Naser Razavi“在 PCVC 语音数据集上使用 MFCC 和安进行完整的波斯语元音识别”第五届电子工程、计算机科学和信息技术国际会议，伊朗，德黑兰，2018 年。](https://scholar.google.com/citations?user=AVMa3t0AAAAJ&hl=en#d=gs_md_cita-d&p=&u=%2Fcitations%3Fview_op%3Dview_citation%26hl%3Den%26user%3DAVMa3t0AAAAJ%26citation_for_view%3DAVMa3t0AAAAJ%3AIjCSPb-OGe4C%26tzom%3D-270) ( [PDF](http://bayanbox.ir/download/2723849504007807268/Full-Persian-Vowel-recognition-with-MFCC-and-ANN-on-PCVC-speech-dataset.pdf) )

## [VoxForge](http://www.voxforge.org/)

口音英语的干净语音数据集。对于您期望对不同口音或语调具有鲁棒性的情况非常有用。

## [钟声](http://spandh.dcs.shef.ac.uk/chime_challenge/data.html)

这是一个嘈杂的语音识别挑战数据集(约 4GB 大小)。该数据集包含真实的模拟和干净的语音记录。真实是在 4 个有噪声的位置上的近 9000 个记录中的 4 个说话者的实际记录，模拟是通过结合语音话语的多个环境和干净的无噪声记录而生成的。

您可以从这里的[下载数据集。](https://archive.org/details/chime-home)

## [2000 HUB5 英语](https://catalog.ldc.upenn.edu/LDC2002T43)

百度深度语音论文中最近使用的纯英文语音数据。

## [具有多种类型录音数据集的帕金森语音数据集](https://archive.ics.uci.edu/ml/datasets/Parkinson+Speech+Dataset+with++Multiple+Types+of+Sound+Recordings)

训练数据属于 20 名帕金森病(PD)患者和 20 名健康受试者。对于这个 20 MB 的集合，从所有对象中获取多种类型的录音(26)。

## [零资源演讲挑战](https://github.com/bootphon/zerospeech2017)

零资源语音挑战的最终目标是构建一个系统，该系统仅使用语言学习婴儿可用的信息，从零开始学习未知语言的端到端口语对话(SD)系统。“零资源”指的是零语言专门知识(例如，拼写/语言转录)，而不是除了音频之外的零信息(视觉、有限的人类反馈等)。4 岁的孩子在没有语言专家监督的情况下自发学习语言的事实表明，这个目标在理论上是可以达到的。

## [等值线数据集](https://data.world/uci/isolet)

这个 38.7 GB 的数据集有助于预测说出了哪个字母名称——这是一个简单的分类任务。

## [阿拉伯语语音语料库](http://en.arabicspeechcorpus.com/)

阿拉伯语语音语料库(1.5 GB)是用于语音合成的现代标准阿拉伯语(MSA)语音语料库。该语料库包含超过 3.7 小时的 MSA 语音的语音和拼写转录，与音素级别上的记录语音一致。注释包括单个音素上的单词重音标记。这个语料库是南安普顿大学的 Nawar Halabiat 博士工作的一部分。该语料库是使用专业工作室用南黎凡特阿拉伯语(大马士革口音)录制的。使用该语料库作为输出的合成语音产生了高质量、自然的声音。

## [TIMIT 文集](https://github.com/philipperemy/timit/blob/master/README.md)

TIMIT read 语音语料库(440 MB)旨在为声学-语音研究以及自动语音识别系统的开发和评估提供语音数据。TIMIT 包含了美国英语八种主要方言的 630 名使用者的宽带录音，每个人朗读十个语音丰富的句子。它包括时间对齐的正字法、语音和单词转写，以及每个话语的 16 位、16 kHz 语音波形文件。

## [多模态情感线数据集(MELD)](https://github.com/SenticNet/MELD)

通过增强和扩展情感线数据集，建立了多模态情感线数据集。MELD 包含与 EmotionLines 中相同的对话实例，但它还包含音频和视频模态以及文本。MELD 有超过 1400 段对话和 13000 句来自老友记电视剧的话语。对话中的每一个话语都被贴上了标签——愤怒、厌恶、悲伤、喜悦、中立、惊讶和恐惧。[下载此处](https://github.com/SenticNet/MELD)

> ***为你的算法获取更多的数据是提高准确性的一种方式。在*** [***深度学习表现小抄***](/deep-learning-performance-cheat-sheet-21374b9c4f45) 中多探索几个秘籍

# **声音/自然**

## [音频设备](https://research.google.com/audioset/)

632 个音频事件类的扩展本体和从 YouTube 视频中提取的 2，084，320 个人类标记的 10 秒声音剪辑的集合。要下载这套，点击这个 [GitHub 链接。](https://github.com/audioset/ontology)

## [Mivia 音频事件数据集](http://mivia.unisa.it/datasets/audio-analysis/mivia-audio-events/)

监控应用的 6000 个事件，即玻璃破碎、枪声和尖叫声。事件分为由 4，200 个事件组成的训练集和由 1，800 个事件组成的测试集。

要下载该数据集，您必须在 [Mivia 网站上注册。](http://mivia.unisa.it/datasets-request/)

## [环境音频数据集](http://www.cs.tut.fi/~heittolt/datasets)

该网页试图维护一个适合环境音频研究的数据集列表。除了免费提供的数据集之外，为了完整起见，这里还列出了专有和商业数据集。除了数据集之外，一些在线声音服务也列在了页面的末尾。这些服务可用于为特殊研究需求形成新的数据集。

数据集分为两个表:

*   声音事件表包含适用于自动声音事件检测和自动声音标记领域研究的数据集。
*   声学场景表包含适合于涉及基于音频的上下文识别和声学场景分类的研究的数据集。

## [FSD:日常声音的数据集](https://datasets.freesound.org/fsd/) ( [Freesound](https://datasets.freesound.org/) )

AudioSet 本体是 600 多个声音类别的分层集合，我们用来自 Freesound 的 297，159 个音频样本填充了它们。该过程生成了 678，511 个候选注释，这些注释表达了音频剪辑中声源的潜在存在。FSD 包括各种各样的日常声音，从人类和动物的声音，到音乐和由事物发出的声音，所有这些都在知识共享许可下。通过创建这个数据集，我们寻求促进研究，使机器能够像人类一样听到和解释声音。

Freesound 是一个基于 Freesound 内容、由人类标记的音频集合的协作创建平台。

## [城市声音分类](https://www.kaggle.com/pavansanagapati/urban-sound-classification)

数据集(6 GB)被称为 UrbanSound，包含 8732 个标记的声音摘录(<=4s) of urban sounds from 10 classes namely: Air Conditioner, Car Horn, Children Playing, Dog bark, Drilling Engine, Idling, Gun Shot, Jackhammer, Siren and Street Music The attributes of data are as follows: ID — Unique ID of sound excerpt Class — type of sound.

## [城市声音数据集](https://urbansounddataset.weebly.com/urbansound.html)

这个数据集包含 1302 个带标签的录音。每个录音都标有 10 类声音事件的开始和结束时间:空调、汽车喇叭、儿童玩耍、狗叫、钻孔、发动机空转、枪击、手提钻、警笛和街头音乐。每个录音可能包含多个声音事件，但对于每个文件，只有来自单个类的事件被标记。这些类别来自城市声音分类法。

## [鸟类音频探测挑战](http://machine-listening.eecs.qmul.ac.uk/bird-audio-detection-challenge/)

在与 IEEE 信号处理协会的合作中，引入了一项研究数据挑战，以创建一种鲁棒且可扩展的鸟类检测算法。这项挑战包括在真实的生物声学监测项目中收集的新数据集(5.4 GB)，以及一个客观、标准化的评估框架。

# 感谢阅读:)如果你喜欢它，尽可能多的点击下面的按钮！这对我意义重大，鼓励我写更多这样的故事

*咱们也连线上* [*推特*](https://twitter.com/cdossman) *或者* [*LinkedIn*](https://www.linkedin.com/in/christopherdossman/)