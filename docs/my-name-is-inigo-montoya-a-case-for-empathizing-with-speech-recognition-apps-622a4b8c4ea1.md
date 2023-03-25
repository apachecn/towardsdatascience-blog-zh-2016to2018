# 我叫伊尼戈·蒙托亚。一个与语音识别应用共情的案例。

> 原文：<https://towardsdatascience.com/my-name-is-inigo-montoya-a-case-for-empathizing-with-speech-recognition-apps-622a4b8c4ea1?source=collection_archive---------3----------------------->

你好。我的名字叫*蒙古朋友*。叹气。*靛蓝拉托亚*。Grrrr。*伊尼戈蒙托亚*。

有时候，在使用语音识别应用程序时，你需要竭尽全力保持冷静。对我们许多人来说，名字是最糟糕的压力源。

![](img/94446b89bc33c438c07f46c1cd14a313.png)

credit: [rabbittooth](http://rabittooth.deviantart.com/)

我住在旧金山湾区，所以我在一个大熔炉里工作和玩耍，这里的多样性是一个美妙的现实。涉及到人名的时候更是如此。事实上，没有比湾区更好的名字识别试验场了。

作为一名听写超级用户，我每天都在滥用谷歌、苹果和亚马逊的助手。在过去几年中，我发现语音识别引擎准确识别姓名的能力差别很大:

☀️ **完美无瑕。苹果和谷歌在听写过程中都能很好地识别我朋友的名字。他们总是毫不费力地“得到”像乔纳森、多琳、乔斯和迈克这样的名字。**

🌤**适中。**一大堆名字被认出来了* [iff](https://en.wikipedia.org/wiki/If_and_only_if) 我努力把名字念得太多了:Siobhán，Valeriu，Cynan，Margot，Rajeev，等等。如果这不起作用，我绝对必须过度“英国化”或过度“美国化”他们的发音，以加快名字识别的速度。像这样的名字足够独特，即使我的发音不完美(事实并非如此)，将它们与我的地址簿中的条目进行匹配也应该相当简单。

🌧**惨不忍睹。此外，还有“其他”的名字，如 Ritesh、Andrei、Karuna、Jernej、Tudor、薛、Rohit，当然还有我自己的名字:Lucian。后者是荒谬的，因为所有这些服务在任何时候都知道用户是谁。即使我/我们每天都提供数量惊人的个性化训练数据，这些名字还是被屠杀、变形或完全遗漏了。有时这样的打嗝很好笑，但绝对不是当你试图完成工作的时候。**

自动语音识别(ASR)算法一直在改进，虽然我毫不怀疑这些打嗝会消失，但与此同时，这让我们用户感到非常沮丧。为什么 speech2text 应用程序会犯这些错误并不明显，所以我认为有必要探究一下导致这些错误的一些因素。如果你想深入了解 ASR 的工作原理，请阅读我的《懒狗入门》:

[](https://medium.com/towards-data-science/speech-recognition-a-lazy-dog-primer-6504ab4dccaf) [## 语音识别:懒狗初级读本

### 我和一个好朋友最近在吃饭时聊起了语音识别应用。是的，我们就是那些人。的…

medium.com](https://medium.com/towards-data-science/speech-recognition-a-lazy-dog-primer-6504ab4dccaf) 

# 从大卫到 Uvuvwevwevwe

作为数据科学家和软件工程师，我们的第一直觉是改进系统、发展架构和创建更好的算法。这不是一个愚蠢的努力:提出精细的语言模型、聪明的深度学习算法和花哨的优化技术有重大的好处。

然而，我是一个大风扇追求低挂水果第一。我们都这么说。在这种情况下，唾手可得的成果就是理解你的用户:他们是谁，他们的交流模式是什么，他们的背景是什么，等等。所有这些必须告知您如何收集和使用训练数据，如何个性化您的应用程序，以及如何通过上下文调整语音识别。

考虑到这一点，让我们来看看在姓名的准确语音识别中可能产生影响的关键因素。

![](img/27be3925dfbbc5138912645debc2c302.png)

credit: [Roy Jones](https://medium.com/u/43d50b537969?source=post_page-----622a4b8c4ea1--------------------------------)

## 1.口音

英语口语变化很大。取决于你在哪里长大，你会强调不同的音节，强化某些辅音，偏向某些元音，并且经常使用不同的单词和表达。仅在美国就有大量的[方言:从新奥尔良的 Yat 方言到波士顿特有的说话方式。我在匹兹堡的时候，我喜欢它的地区语言特色:“*尹兹来了吗？*](https://en.wikipedia.org/wiki/North_American_English_regional_phonology#Map_of_dialect_regions)

如果你缩小视野，听听来自世界各地的母语人士，比如英国、加拿大、新西兰和印度，英语语音模式的差异会更大。然后，进一步扩展，考虑非英语母语者和他们无数的口音——我应该知道！

## 2.发音

人类既古怪又有缺陷。当我们的大脑失灵时，我们经常或偶尔发错单词的音。我们希望语音识别应用程序能够处理轻微的语言障碍，但这并不容易。根据美国国家卫生研究院的数据，每 12 名 3-17 岁的美国儿童中就有 1 名患有与语言相关的疾病。最后，这是一个统计和收集足够的训练数据来覆盖角落情况的游戏。

疲劳是你发音不佳的另一个原因。我不知道你是怎么想的，但是当我连续 48 小时不睡觉时，我的语言能力会大大下降——我评估这个事实的能力也是如此。

最后，你可能是全国拼字比赛冠军，但有时你会被难念的名字绊倒。你见过 Uvuvwevwevwe 吗？

## 3.名称频率

常用名字对于语音识别 app 来说很容易。他们看到的有这个名字的人越多，他们积累的训练数据就越多，从而有助于更好的机器学习识别模型。然而，近几十年来，父母们已经对现有的名字池产生了明显的不满，并开始为他们的孩子引入新的、独特的名字。

当人工智能助手第一次遇到像 [Finola、Pippa 和 Tanaquil](http://www.thedailybeast.com/articles/2010/07/28/baby-names-100-cool-uncommon-baby-names-for-girls-and-boys.html) 这样的名字时，他们只能做这么多。低频名字很难准确识别，但我希望应用程序集成和深度个性化能让我们做到这一点。例如，如果我一天和 Finola 说两次话，一天和 Tanaquil 发 20 次短信，那么期望这两个名字的识别是完美的，这并不是不合理的。

## 4.姓名拼写

在给孩子起名时，数百万父母也想出了极具创意的拼写方法，这无疑是为了欺骗语音识别应用程序。

有些名字与现有的名字或它们的近邻在语音上完全相同，但拼写却完全不同。你只需看看 [Alexzandre、Braedyn 或 Izobel](http://www.parenting.com/baby-names/lists/unusual-spellings-common-names) 就能感受到虚拟助手的痛苦。

## 5.多语言环境

随着全球流动性的增加，世界变得更加多样化。毫不奇怪，人们倾向于混合多种语言:例如他们的母语和他们居住国家的语言。

My kids are comfortable with a mixture of English, Romanian, and a smidge of beginner Chinese peppered in for good measure. “What do you think of the [sup](https://jamilacuisine.ro/supa-de-pui-cu-galuste-reteta-video/)ă [cu g](https://jamilacuisine.ro/supa-de-pui-cu-galuste-reteta-video/)ă[lu](https://jamilacuisine.ro/supa-de-pui-cu-galuste-reteta-video/)ș[te](https://jamilacuisine.ro/supa-de-pui-cu-galuste-reteta-video/)? 好吃吗?“ They won’t bat an eye if they hear that, but for now virtual assistants still have trouble with “can you *play* [*听妈妈的话*](https://www.youtube.com/watch?v=-ASP_Xwu2Yg)”

这种现象在名字上最为明显，人们使用英语等主要语言，但用他们的母语正确地读出他们家人和朋友的名字。

试试这个:告诉你的虚拟助理给你的朋友里卡多发短信。发出命令时使用清晰的英语，但随后改用你最好的葡萄牙口音，发出“[里卡多](https://www.youtube.com/watch?v=ndrZsXSOuzQ)”的音。我会等的。

# 回到移情

我们生活在一个混乱的世界。我们随时都在应对不确定性，我们与身边的每个人和每件事谈判，以把事情做好。我们相互交流和沟通的方式就是如此。演讲是一个完美的不完美交流的例子，它只拥有足够的信号来传达预期的意思。知道了这一点，语音识别领域已经达到这种质量水平，真是不可思议。

每当我抱怨听写质量有多差时，我都会后退一步，看看我现实生活中的星际旅行通讯器、三录仪和通用翻译一体机，迫不及待地想看看明天会带来什么。

____

*如果你喜欢这个涂鸦，点击*👏*在下面，所以其他人可以在媒体上阅读并享受它。*

[***卢西恩·利塔***](https://www.linkedin.com/in/lucianlita) *是 Yoyo Labs 的创始人，之前是 BlueKai、Intuit 和西门子医疗保健公司 Level Up Analytics 和 data leader 的创始人。*

*如果你喜欢这篇文章，点击💚以便其他人可以在媒体上看到它。*

![](img/7602ff4b0919211a6b8a8edfbd36ea01.png)