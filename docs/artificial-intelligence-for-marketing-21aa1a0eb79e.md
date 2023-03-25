# 用于营销的人工智能

> 原文：<https://towardsdatascience.com/artificial-intelligence-for-marketing-21aa1a0eb79e?source=collection_archive---------8----------------------->

![](img/8ff5d938aa24a0c49bcb75194decde79.png)

这篇文章是我们如何将人工智能和数据科学应用于公司营销的一个演示。

我开始写关于 medium.com 和数据科学的文章已经整整一年了。我已经成为“人工智能”标签的顶级作家，拥有 50 篇文章，拥有近 2000 名粉丝，读者人数不断增加。

![](img/9b3d0a04861c2997ea3bc534131d9fb5.png)

I have become a top writer on medium for the tag “Artificial Intelligence”

这篇文章如何适用于你？在这篇文章中，我是试验品。你会看到一些不错的数据，关于一个中型作家如何发展观众，并透过幕后了解什么可行，什么不可行。你可以想象我在这篇文章中的营销材料分析方法是如何帮助你成为一个更好的媒体作家的。

为什么我要写在介质上？嗯，不仅仅是营销。这是信息。当我们讨论解决方案架构时，我将内容推向公共领域，并让客户参考这些帖子。这也有助于我们加快公司定期对我们公司进行的尽职调查。因为我们是如此紧密地被 NDAs 所束缚，我抓住每一个我能找到的机会与你们分享我们正在做的事情的非秘密部分，我亲爱的读者们。

看看我的文章标题，我倾向于使用“人工智能”这个关键词，而不是深度学习或机器学习。这种方法有助于大多数人从远处理解这个话题，并吸引首席执行官们的参与，他们是我的 B2B 咨询( [lemay.ai](http://lemay.ai) )和 B2B 产品(【genrush.com】、[【audit map . ai】的目标市场。](http://auditmap.ai)

让我们来看看我的 50 个中型帖子的数据集(见下文)和我在 medium.com 的流量统计

这些列的含义如下:

*   **id:** 跟踪数据库中的行
*   我认为这篇文章是关于三个主题中的哪一个
*   **标题:**文章的标题
*   **url:** 链接到文章
*   **word_count:** 文章字数。有时这也包括代码或链接之类的非单词。我没意见
*   **views_medium:** 来自 medium stats 的文章浏览量
*   **reads_medium:** 来自 medium stats 的文章阅读计数。阅读用户参与度较高，而浏览用户参与度较低
*   **fans_accts_medium:** 为文章鼓掌的账号数量(谢谢)
*   **claps_medium:** 读者给文章加的赞数
*   文章发表的日期
*   **days_since_post:** 我获取统计数据和文章发表日期之间的天数(date_medium)。
*   **Twitter _ medium:**Twitter 上文章的引用数量
*   **正文:**文章的明文——去掉标签和嵌入的多媒体

以下是我如何在脑海中对我的帖子进行分类的(cluster_daniel 专栏):

*   **机器学习演示和操作指南**
*   **B2B 解决方案架构**
*   **启动场景和意见标签**

让我们看看我基于经验的聚类是否符合一些数学事实。我使用 spacy(英语中等数据集)将每篇文章的文本转换成矢量形式，然后应用 t-SNE 降维。让我们来看看我一直使用的 3 个标签的文章数据是如何聚类的:

![](img/24e05b3cf22e9d381034f230ceb18aa4.png)

Results of t-SNE dimensionality reduction shown in tensorboard. Blue dots are the cluster “Machine Learning Demos and How-To Stuff” while the yellow dots are artilces from the cluster “B2B Solution Architecture”, and the red dots are articles from the cluster “Startup Scene and Opinion Blag”. Black dots are just overlapping colors.

结果相当令人鼓舞。在上图中，我们可以看到，类似博客的文章的红点(侧重于业务术语)和技术文章的蓝点(侧重于技术术语)被代表混合了业务和技术术语的 B2B 文章的黄点分开。它告诉我，我的标签是由内容数据本身支持的。

查看嵌入空间中哪些特定的文章彼此相邻可能会很有趣:

![](img/25e5c320a6ef151e9071df908eb27076.png)

Looking at articles by cluster and title to see what specific articles are very semantically similar.

在我们继续看更多的数据之前，快速声明:这是一个非常简单的分析，没有频率/趋势分析或任何花哨的东西，没有图像计数，链接计数，链接网页等。

现在，让我们看看数据中各列之间的关系。

两秒钟的熊猫数据帧和 seaborn 的工作让我们有了以下的出发点:

![](img/57c96352156840a408287d0119fe6ee7.png)

The diagonal shows the general shape of the data, while the top right half shows the pearson correlation value (how linearly correlated is stuff with other stuff) and the bottom left half shows the raw data.

上图的问题是:没有显示聚类，文本太小，相关性很难看到。

![](img/8c21ce8cf7873862a3429f239d335227.png)

This plot shows the correlation (red) and anit-correlation (blue) between columns.

上面的这个新数字是一个很好的进步，但是我们失去了底层集群数据的意义。我们总体上看到了相关性，但仍然看不到这些相关性的聚类来源。我们现在可以更清楚地看到，许多东西是相互关联的。例如，字数与浏览量、点击量、阅读量等相关。这很有道理。id 的反相关性只是告诉我们，较低的聚类数比较高的聚类数有更多的粉丝/阅读/喜欢等。但是，我们可以看到较旧的帖子在后面的集群中。随着时间的推移，我越来越关注 B2B 和技术文章，而不是观点文章(第三组)。days_since_post 行告诉我们，最近的文章比以前的文章做得更好。

现在，这是没有聚类着色的相关数据。对角线上的数据被转换成频率区间，比之前的线性近似更容易分析。现在，每个相关框都有一条使用回归创建的最佳拟合线和一个不确定圆锥(圆锥越大=越不确定)。

![](img/e505624c352e668a3e7439a08abbb6e7.png)

我们已经可以从这些数据中得出一些合乎逻辑的结论。第 4 行第 3 列(reads v.s. views)告诉我们，读取与视图紧密相关，这显然是正确的。类似的还有拍手和粉丝账号。

现在，让我们更深入一层，按群集查看数据。整个想法是为了验证我的假设，读者(你)希望看到更多的技术和 B2B 文章，而不是观点文章。最后，这是为每个分类添加了颜色的相同数据:

![](img/101b58a38485346f045cc8aaec499ec1.png)![](img/c2aca406b79d8a85410e79b451485d39.png)

文本仍然很小，但是您可以放大来查看列名。

既然我们可以按集群查看数据，那么很明显，在集群内部，数据在许多指标上表现出许多规律性。在第一行和第一列中，集群按 id 排序，因为我选择了 id 按集群升序排序。作为一种见解，这并不有趣。深入到图中，最后一行(twitter)与字数(列 2)相关，但与红点(启动场景)无关。这告诉我们 twitter 更喜欢其他集群。看右下方的条形图，大部分文章没有 twitter 推荐，小部分文章获得了大部分 twitter 流量。倒数第二行显示我写的文章越来越少，最近也没有关于创业的文章(红点)。我们还看到，最近的文章有更多的阅读量和浏览量，但最近的文章总体上较少。

作为未来的工作，从单词嵌入模型预测聚类 id(主题模型)和点击/观看将会很有趣。然而，在这一点上，我只是不认为我有足够的行数和列数的数据来做出可靠的预测。

感谢阅读！下面是对 50 个数据点(文章)的更深入的分类:

## 机器学习演示和操作材料:

[](/artificial-intelligence-for-music-videos-c5ad14e643db) [## 音乐视频的人工智能

### 我正在为一个客户做一个视频分析人工智能项目，我想和你分享一个超级有趣的想法，它突然出现在…

towardsdatascience.com](/artificial-intelligence-for-music-videos-c5ad14e643db) [](/drawing-anime-girls-with-deep-learning-4fa6523eb4d4) [## 用深度学习画动漫少女

### 这篇文章是关于玛丽·凯特·麦克弗森的一个兼职项目。我们喜欢做一些辅助项目，比如音乐视频的人工智能，还有…

towardsdatascience.com](/drawing-anime-girls-with-deep-learning-4fa6523eb4d4) [](/elbow-clustering-for-artificial-intelligence-be9c641d9cf8) [## 人工智能中的肘聚类

### 聚类是将一堆未分类的东西(你的数据集)分成几堆(即聚类)的过程…

towardsdatascience.com](/elbow-clustering-for-artificial-intelligence-be9c641d9cf8) [](/image-datasets-for-artificial-intelligence-bbb12615edd7) [## 人工智能的图像数据集

### 在人工智能领域，数据就是力量。

towardsdatascience.com](/image-datasets-for-artificial-intelligence-bbb12615edd7) [](/kegra-deep-learning-on-knowledge-graphs-with-keras-98e340488b93) [## kegra:基于 Keras 的知识图深度学习

### 你好。我在以前的文章中提到过，我正在深入研究企业数据集的认知计算。这个…

towardsdatascience.com](/kegra-deep-learning-on-knowledge-graphs-with-keras-98e340488b93) [](/aws-sagemaker-ais-next-game-changer-480d79e252a8) [## AWS SageMaker:人工智能的下一个游戏规则改变者

### 今天 AWS SageMaker 发布了，太牛逼了。我在以前的文章中提到过，我们主要做 AWS…

towardsdatascience.com](/aws-sagemaker-ais-next-game-changer-480d79e252a8)  [## 数字海洋堆栈上的深度学习？还没有

### 所以你想要一个更便宜的解决方案来运行你的深度学习代码。AWS 每月给你大约 1000 英镑的账单…

towardsdatascience.com](/deep-learning-on-the-digitalocean-stack-not-quite-yet-5c408e7d1a41) [](/deep-learning-with-digitalocean-redux-e6f447e64c75) [## 用数字海洋进行深度学习:Redux

### 这篇文章会和我平时的商务类文章有点不一样。在最近的一篇文章中，我给出了…

towardsdatascience.com](/deep-learning-with-digitalocean-redux-e6f447e64c75) [](https://medium.com/@lemaysolutions/ai-read-my-email-e69a833dd0f1) [## 艾:看我的邮件

### 电子邮件是我们数字社会不可或缺的一部分。像小唐纳德的勾结，希拉里的服务器，波德斯塔的…

medium.com](https://medium.com/@lemaysolutions/ai-read-my-email-e69a833dd0f1) [](https://medium.com/@lemaysolutions/a-dozen-helpful-commands-for-data-nerds-9619bc43c5c6) [## 十几个对数据爱好者有用的命令

### 在使用大数据的机器学习中，有许多组织工作要做。我们用于数据管理的名称各不相同:

medium.com](https://medium.com/@lemaysolutions/a-dozen-helpful-commands-for-data-nerds-9619bc43c5c6) [](https://medium.com/@lemaysolutions/artificial-intelligence-genesis-literally-947c1935752d) [## 人工智能起源:字面意思。

### 没错。真实的圣经人工智能。让我们看看我们可以看到一些自然语言处理(NLP)和英语语言…

medium.com](https://medium.com/@lemaysolutions/artificial-intelligence-genesis-literally-947c1935752d) [](https://medium.com/@lemaysolutions/scandal-what-happens-to-a-leaked-email-address-ee90fe765af9) [## 丑闻！被泄露的电子邮件地址会怎么样？

### 2015 年 7 月 Ashley Madison 泄漏事件上周又回到了我的脑海中，当时一名 RNC 承包商的巨大泄漏袭击了…

medium.com](https://medium.com/@lemaysolutions/scandal-what-happens-to-a-leaked-email-address-ee90fe765af9) [](https://medium.com/@lemaysolutions/machine-learning-in-medicine-demo-time-ffd17ed8a72a) [## 医学中的机器学习:演示时间！

### 来说一个很酷的想法。人工智能用于伤口检测战斗机器人！

medium.com](https://medium.com/@lemaysolutions/machine-learning-in-medicine-demo-time-ffd17ed8a72a) [](https://medium.com/@lemaysolutions/ai-copy-this-pattern-5ac4e9ccf6a9) [## 艾:复制这个模式

### 简单说一下我现在正在做的事情:我正在研究递归神经网络(RNNs)。这种类型的…

medium.com](https://medium.com/@lemaysolutions/ai-copy-this-pattern-5ac4e9ccf6a9) [](https://medium.com/@lemaysolutions/model-me-this-model-me-that-generate-text-one-character-at-a-time-46a232db92e7) [## 给我做这个模型。以我为例:一次生成一个字符的文本。

### 安德烈·卡帕西最近进入了特斯拉的 ML 高层。我想给你看一些他早期的作品…

medium.com](https://medium.com/@lemaysolutions/model-me-this-model-me-that-generate-text-one-character-at-a-time-46a232db92e7) [](https://medium.com/@lemaysolutions/secretly-validate-email-addresses-86bd6dc980f8) [## 秘密验证电子邮件地址

### 如何辨别哪些电子邮件地址是真实的？我不是说什么地址在语法上是有效的，比如…

medium.com](https://medium.com/@lemaysolutions/secretly-validate-email-addresses-86bd6dc980f8) [](https://medium.com/@lemaysolutions/1-1tb-data-leak-just-wow-8ee38322d57d) [## 1.1TB 数据泄露。只是，哇

### 大家好，

medium.com](https://medium.com/@lemaysolutions/1-1tb-data-leak-just-wow-8ee38322d57d) 

## B2B 解决方案架构:

[](/how-to-price-an-ai-project-f7270cb630a4) [## 如何给一个人工智能项目定价

### 客户多次要求我为大型机器学习(ML)项目提供固定的价格估计。这个…

towardsdatascience.com](/how-to-price-an-ai-project-f7270cb630a4) [](/why-hire-an-ai-consultant-50e155e17b39) [## 如何聘请人工智能顾问

### 所以，你知道你需要一些机器学习/人工智能开发。为什么不自己做呢？嗯，简短的回答是…

towardsdatascience.com](/why-hire-an-ai-consultant-50e155e17b39) [](/ai-track-me-325833522043) [## 艾:跟踪我

### 我的手机收集的数据量令人震惊。让我们看看我的谷歌历史，看看我们能看到什么。这个…

towardsdatascience.com](/ai-track-me-325833522043) [](/dream-ac-build-teams-using-artificial-intelligence-c4f1fdd7ee66) [## DREAM.ac:利用人工智能建立团队

### 人工智能正在被部署来解决许多人类问题，最近谷歌的 Duplex 可以使…

towardsdatascience.com](/dream-ac-build-teams-using-artificial-intelligence-c4f1fdd7ee66) [](/deep-learning-magic-small-business-type-8ac484d8c3bf) [## 深度学习魔法:小企业类型

### 企业情报很难，NAICS 密码很糟糕。一些公司按行业购买公司名单，以保存…

towardsdatascience.com](/deep-learning-magic-small-business-type-8ac484d8c3bf) [](/artificial-intelligence-and-bad-data-fbf2564c541a) [## 人工智能和不良数据

### 脸书、谷歌和推特的律师向国会作证，说明他们如何错过了俄罗斯的影响力运动…

towardsdatascience.com](/artificial-intelligence-and-bad-data-fbf2564c541a) [](/understanding-events-with-artificial-intelligence-12e1ec3c5c9) [## 用人工智能理解事件

### 我们遇到了很多客户需求，归结起来就是使用人工智能来理解事件。一些系统需要…

towardsdatascience.com](/understanding-events-with-artificial-intelligence-12e1ec3c5c9) [](/artificial-intelligence-without-labeled-data-54cdbfbdaad2) [## 无标签数据的人工智能

### 先说无监督深度学习 AI，转移学习。

towardsdatascience.com](/artificial-intelligence-without-labeled-data-54cdbfbdaad2) [](/accelerating-deep-neural-networks-1231273c48aa) [## 加速深度神经网络

### 神经网络“慢”有很多原因，包括加载/存储延迟，将数据移入和移出 GPU…

towardsdatascience.com](/accelerating-deep-neural-networks-1231273c48aa) [](/artificial-intelligence-get-your-users-to-label-your-data-b5fa7c0c9e00) [## 人工智能:让你的用户给你的数据贴标签

### 我想把这篇文章叫做“人工智能训练数据的寄生标签”，但显然这太复杂了。我……

towardsdatascience.com](/artificial-intelligence-get-your-users-to-label-your-data-b5fa7c0c9e00) [](https://medium.com/@lemaysolutions/ai-is-this-porn-2695c5e6fa47) [## 艾:这是黄片吗？

### 色情推动了格式战争。一代人以来都是如此。网络上充斥着色情内容。

medium.com](https://medium.com/@lemaysolutions/ai-is-this-porn-2695c5e6fa47) [](https://medium.com/@lemaysolutions/ai-for-sales-teams-7b9ab014417d) [## 销售团队的人工智能

### 先说现状。所以，你想卖一个产品或服务。假设你卖火险…

medium.com](https://medium.com/@lemaysolutions/ai-for-sales-teams-7b9ab014417d) [](https://medium.com/@lemaysolutions/big-data-and-machine-learning-two-peas-in-a-pod-e3a04d627784) [## 大数据和机器学习:一个豆荚里的两颗豌豆

### 退一步想想大数据和机器学习的联系。要真正利用大数据，您需要…

medium.com](https://medium.com/@lemaysolutions/big-data-and-machine-learning-two-peas-in-a-pod-e3a04d627784) [](https://medium.com/@lemaysolutions/locked-in-a-box-machine-learning-without-cloud-or-apis-76cc54e391c8) [## 锁在盒子里:没有云和 API 的机器学习

### API 很棒。机器学习 API 更牛逼。从 api.ai 到 Google 这样的 Google 云平台 APIs

medium.com](https://medium.com/@lemaysolutions/locked-in-a-box-machine-learning-without-cloud-or-apis-76cc54e391c8) [](https://medium.com/toronto-machine-learning/exam-time-for-a-binge-watching-ai-e9477e901e3e) [## 考试的时候来个狂看 AI

### 有很多例子显示了人工智能如何识别视频中的数据。比如情绪识别…

medium.com](https://medium.com/toronto-machine-learning/exam-time-for-a-binge-watching-ai-e9477e901e3e) 

## 启动场景和意见标签:

[](/artificial-intelligence-is-probably-safe-ce67f0abd759) [## 人工智能很可能是安全的

### 即使我们最终都会被宇宙的热寂所杀死(10^100 年后)，还有更多…

towardsdatascience.com](/artificial-intelligence-is-probably-safe-ce67f0abd759) [](/machine-learning-use-small-words-5cc8f34a5964) [## 机器学习:使用小词

### 我喜欢异端这个词。这是一种很酷的说法“跳出框框思考，反对主流”。它…

towardsdatascience.com](/machine-learning-use-small-words-5cc8f34a5964) [](/artificial-intelligence-as-magic-e8ba4b3165ea) [## 人工智能像魔术一样

### 创新或者死亡。成长或枯萎。这就是我今天在人工智能领域的感受。从 2008 年开始的衰退性失业…

towardsdatascience.com](/artificial-intelligence-as-magic-e8ba4b3165ea) [](/artificial-intelligence-consequences-bd4dc4d537da) [## 人工智能:后果

### 人工智能将改变世界。

towardsdatascience.com](/artificial-intelligence-consequences-bd4dc4d537da) [](/artificial-intelligence-hyperparameters-48fa29daa516) [## 人工智能:超参数

### 深度学习神经网络模型有许多参数(例如，权重和偏差)，也有相当多的…

towardsdatascience.com](/artificial-intelligence-hyperparameters-48fa29daa516) [](https://medium.com/@lemaysolutions/ai-tools-are-popping-like-popcorn-3baa6793271f) [## 人工智能工具像爆米花一样爆开

### 我想暂停谈生意，谈一谈正在深入展开的技术故事…

medium.com](https://medium.com/@lemaysolutions/ai-tools-are-popping-like-popcorn-3baa6793271f) [](https://medium.com/@lemaysolutions/growing-and-hiring-25540db9cecc) [## 发展和招聘

### 一位渥太华人工智能初创公司的创始人在 LinkedIn 上的状态是“增长和招聘”我喜欢这样。多好的……

medium.com](https://medium.com/@lemaysolutions/growing-and-hiring-25540db9cecc) [](/our-artificial-intelligence-startup-c7daf4c140a1) [## 我们的人工智能创业公司

### 你好。我给你讲讲我们 AI 创业的故事吧。一路上我们犯了一些错误，我想你可能会学到…

towardsdatascience.com](/our-artificial-intelligence-startup-c7daf4c140a1) [](/why-bother-to-bootstrap-your-ai-startup-d94fd62de009) [## 为什么要自举你的人工智能创业公司呢？

### 关于命名法的一个快速补充…它应该是谷歌实验所说的人工智能，维基百科所说的人工智能，还是人工智能…

towardsdatascience.com](/why-bother-to-bootstrap-your-ai-startup-d94fd62de009) [](/ai-consulting-the-reverse-marshmallow-experiment-18d466737d3f) [## 人工智能咨询&逆向棉花糖实验

### 我在过去写过关于人工智能人才的高需求，以及高素质人才的低供给…

towardsdatascience.com](/ai-consulting-the-reverse-marshmallow-experiment-18d466737d3f) [](/how-can-i-write-better-articles-on-ai-6b37e1baad5a) [## 怎样才能写出更好的 AI 文章？

### 我很清楚，我在媒体上的写作让我获得了有趣的联系，这些联系可以转化为…

towardsdatascience.com](/how-can-i-write-better-articles-on-ai-6b37e1baad5a) [](/low-budgets-and-high-expectations-machine-learning-startups-c428f3f82569) [## 低预算和高期望:机器学习初创公司

### 在以前关于如何雇用人工智能顾问，以及如何为人工智能项目定价的文章中，我试图给你一种…

towardsdatascience.com](/low-budgets-and-high-expectations-machine-learning-startups-c428f3f82569) [](https://medium.com/@lemaysolutions/a-i-is-not-magic-9fb19239fcaf) [## 人工智能不是魔法

### 一点数据科学和一桶血、汗、泪，你就知道了:你的模型是过度拟合的…废话…

medium.com](https://medium.com/@lemaysolutions/a-i-is-not-magic-9fb19239fcaf) [](/its-alive-building-general-ai-is-the-future-ddd8f75d09dc) [## 它还活着！构建通用人工智能是未来

### 我的朋友们一次又一次地听到我这样说。人工智能正在走向一种认知的“生活”,超越…

towardsdatascience.com](/its-alive-building-general-ai-is-the-future-ddd8f75d09dc) [](https://medium.com/toronto-machine-learning/educational-videos-artificial-intelligence-deep-learning-machine-learning-etc-3fadb6050eb5) [## 教育视频:人工智能、深度学习、机器学习等。

### 来分享一些书签吧。作为我们开发人员入职流程的一部分，我将 YouTube 视频分成了 3 个部分。

medium.com](https://medium.com/toronto-machine-learning/educational-videos-artificial-intelligence-deep-learning-machine-learning-etc-3fadb6050eb5) [](https://medium.com/@lemaysolutions/what-should-i-call-my-startup-ef9aea077271) [## 我应该怎么称呼我的创业公司？

### 当我们得到一个有新想法的客户时，他们通常没有域名，甚至没有公司名称。这是一个干净的石板…

medium.com](https://medium.com/@lemaysolutions/what-should-i-call-my-startup-ef9aea077271) [](https://medium.com/@lemaysolutions/emergent-properties-e69c0f72d64d) [## 涌现属性

### 我们有消息了！我们的中型岗位现在是辛迪加对数据科学！

medium.com](https://medium.com/@lemaysolutions/emergent-properties-e69c0f72d64d) [](/fear-in-the-age-of-machine-learning-87ee33b5b264) [## 机器学习时代的恐惧

### 这是我关于人工智能和大数据系列的第三篇文章。最近完成了关于…的博士学位

towardsdatascience.com](/fear-in-the-age-of-machine-learning-87ee33b5b264) 

——丹尼尔
[丹尼尔@lemay.ai](mailto:daniel@lemay.ai) ←打个招呼。
[LEMAY . AI](https://lemay.ai)
1(855)LEMAY-AI

您可能喜欢的其他文章:

*   [人工智能和不良数据](/artificial-intelligence-and-bad-data-fbf2564c541a)
*   [人工智能:超参数](/artificial-intelligence-hyperparameters-48fa29daa516)
*   [人工智能:让你的用户给你的数据贴上标签](https://medium.com/towards-data-science/artificial-intelligence-get-your-users-to-label-your-data-b5fa7c0c9e00)