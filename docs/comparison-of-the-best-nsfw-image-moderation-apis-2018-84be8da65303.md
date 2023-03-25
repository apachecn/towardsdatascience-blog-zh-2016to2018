# 2018 年最佳 NSFW 图像审核 API 对比

> 原文：<https://towardsdatascience.com/comparison-of-the-best-nsfw-image-moderation-apis-2018-84be8da65303?source=collection_archive---------6----------------------->

多个图像内容过滤 API 提供商的全面基准，涵盖不同类别，如裸体、色情和血腥。

![](img/8493bc07607567d8b66fb191a859ad0b.png)

人类可以本能地决定他们所看到的是否不合适。然而，当谈到拥有一个可以决定一个图像或视频是否不合适的全视人工智能时，这个问题还远远没有解决。现在，许多公司都在争先恐后地应用自动化技术来识别一种媒体是可以安全传播还是被清除。

我想亲自了解一下，在自动检测公认的 NSFW 内容方面，最先进的技术是什么。我将基于以下类别对 API 性能进行比较

*   露骨的裸体
*   暗示性的裸体
*   色情/性行为
*   模拟/动画色情
*   血腥/暴力

**Tl；DR:** 如果你只是对找出最好的 API 感兴趣，你可以直接跳到文章末尾的总体比较。

# 实验设置:

**数据集:**为了进行评估，我创建了一个自定义的 NSFW 数据集，它与每个 nsfw 子类别具有相同的权重。该数据集由 120 幅图像组成，其中 20 幅 nsfw 阳性图像用于上述五个类别中的每一个，20 幅 sfw 图像。我决定不使用开源的 YACVID 180 图像数据集，因为它主要依赖于使用裸体作为 NSFW 内容的衡量标准。

收集 NSFW 图像是一项单调乏味、费时费力的工作，因此图像数量很少。

**数据集已经开源，可以在这里下载** [**。【警告:包含露骨内容】**](https://drive.google.com/folderview?id=18SY4oyZgTD_dh8-dc0wmsl1GvMsA7woY)

[**这里的**](https://docs.google.com/spreadsheets/d/1fEOJfTLmQdtRvllw1e8LXJ6vXAjQQj68iF4gWCJy1JM/edit?usp=sharing) **是一张包含数据集中每个图像上的 API 的原始预测的表。**

# 指标:

每个分类器都根据普遍接受的指标进行评估，例如:

## 真阳性:TP

如果一个分类器称某物为 NSFW，而它实际上是 NSFW

## 真阴性:TN

如果一个分类器称某物为 SFW，而它实际上是 SFW

## 误报:FP

如果一个分类器称某物为 NSFW，而它实际上是 SFW

## 假阴性:FN

如果一个分类器称某物为 SFW，而它实际上是 NSFW

![](img/dfa45e8cf22278f4cc12857f327b0b41.png)

## 准确(性)

如果模型做出预测，你能相信它吗？

## 精确

如果模型说某个图像是 NSFW，那么它的正确率是多少？

## 回忆

如果所有的 NSFW 图像，它能识别多少？

## F1 分数

它是精确性和回忆性的混合体，通常类似于准确性。

![](img/bf1900bee75913b4999d3f1402199886.png)

## 我评估了以下用于内容审核的 API

*   [*亚马逊识别*](https://aws.amazon.com/rekognition/)
*   [T5【谷歌】T6](https://cloud.google.com/vision/)
*   [*微软*](https://azure.microsoft.com/en-in/services/cognitive-services/content-moderator/)
*   [*雅虎*](https://algorithmia.com/algorithms/spullara/YahooOpenNSFW)
*   [*算法 ia*](https://algorithmia.com/algorithms/sfw/NudityDetectioni2v)
*   [*克拉里菲*](https://clarifai.com/models/nsfw-image-recognition-model-e9576d86d2004ed1a38ba0cf39ecb4b1)
*   [*迪派*](https://deepai.org/machine-learning-model/nsfw-detector)
*   [*Imagga*](https://imagga.com/solutions/adult-content-moderation.html)
*   [*纳米网*](https://nanonets.com/content-moderation-api/)
*   [*视觉引擎*](https://sightengine.com/)
*   [*X-版主*](https://xmoderator.com/)

# 跨类别的绩效

我首先按类别评估了每个 API，看看它们在检测不同类型的 NSFW 内容时表现如何。

# 色情/性行为

Google 和 Sightengine API 在这里大放异彩，因为它们是唯一能够正确检测所有色情图片的 API。Nanonets 和 Algorithmia 紧随其后，因为它们能够正确分类 90%的色情图像。微软和 Imagga 在这个类别上的表现最差。

![](img/8a3fd93bd9f342e73f680f0e5351a5f6.png)![](img/7c01ba3b5bfa5a460cacc445c562e456.png)

**原始图片链接:** [Porn19](https://www.dropbox.com/s/jcdm5wli12bq2tq/19.jpg?dl=0) ， [Porn7](https://www.dropbox.com/s/oeao2hwqi960lum/7.jpg?dl=0) ， [Porn18](https://www.dropbox.com/s/3rt3tyj7e7lbas6/18.jpg?dl=0) ， [Porn14](https://www.dropbox.com/s/49h266fe3bt2kk3/14.jpg?dl=0)

容易识别的图像明显是色情的。所有的供应商都得到了正确的图片。他们中的大多数人都非常有把握地预测了 NSFW 的内容。

![](img/406cddbe3ed149bdd23b80a360a23de5.png)

**原图链接:** [色情 6](https://www.dropbox.com/s/hwgp88gkh0na91v/6.jpg?dl=0) ，[色情 2](https://www.dropbox.com/s/khr3wkrj8hgxftm/2.jpg?dl=0) ，[色情 10](https://www.dropbox.com/s/t4uake8b2kkh67c/10.jpg?dl=0) ，[色情 3](https://www.dropbox.com/s/l9yyxxwagullg1r/3.jpg?dl=0)

难以识别的图像是由于遮挡或模糊造成的。在最糟糕的情况下，11/12 的供应商弄错了图像。色情在性能上有很大的差异，这取决于色情的强度和色情内容的清晰程度。

# 露骨的裸体

大多数 API 在这一类别中表现出色，其中许多具有 100%的检测率。即使是性能最低的 API(clari fai 和 Algorithmia)在这里也有 90%的检测率。什么被认为是裸体的定义一直是争论的主题，从难以识别的图像中可以清楚地看出，在人们可以争论这些是 SFW 的情况下，它们大多失败了。

![](img/536d4a78ef52f97dff5079550a2b2baf.png)![](img/43354d5c4d9dd87d6aa6484c30f99b92.png)

**原始图片链接:** [裸露 10](https://drive.google.com/open?id=1OjwvVOzgyqmTZz6Kfl89V_qT1sS57Tf4) ，[裸露 7](https://drive.google.com/open?id=1UWYgAyrx8sY1cK5Okl-EXCLGIB2ymS8m) ，[裸露 13](https://drive.google.com/open?id=1rqajfLPSOglHLPhqRZPhDDagOoiD0BqA) ，[裸露 14](https://drive.google.com/open?id=1EuCg8jsNJ__Sb0DL3bNkO0SrsKblYlhG)

易于识别的图像具有清晰可见的裸露，并且是清晰的。任何没有不同意见的人都会把这些叫做 NSFW。没有一个提供者出错，平均分数都是 0.99。

**原始图片链接:**

![](img/12bad786e1491171ee459c5db3f5c65f.png)

**原图链接:** [裸体 9](https://drive.google.com/open?id=11zpHCueQB9L5dMNk8vhzSMGpjz8u66ll) ，[裸体 8](https://drive.google.com/open?id=1U50da9UhLhP9j3D_Yb3q4PUQOqZzrnDZ) ，[裸体 18](https://www.dropbox.com/s/v049jxtiik6pzb8/18.jpg?dl=0) ，[裸体 4](https://www.dropbox.com/s/zerlol39bg5hgc4/4.jpg?dl=0)

引起争论的图像是那些提供者弄错的图像。这可能只是每个供应商对裸体的敏感度有不同的设置。

# 暗示性的裸体

谷歌在这方面再次领先，对这一类别的检测率为 100%。Sightengine 和 Nanonets 的表现优于其他产品，检测率分别为 95%和 90%。对于机器来说，暗示性的裸体几乎和裸体一样容易识别，但它出错的地方是在通常看起来像 SFW 图像但有一些裸体的图像中。

![](img/0ccef36c70cc187fc8b458f4e4f06aa3.png)![](img/9914a33f8f95d2d5bcdfc8b75fc7aad3.png)

**原始图片链接:** [建议 13](https://drive.google.com/open?id=1TF-fvnPrwBlBTwzxCTa-hYRCN9m1sBOj) ，[建议 10](https://drive.google.com/open?id=1hpJ2g8TQQ4XRshfOYThlqbIPk7OF0r8v) ，[建议 2](https://drive.google.com/open?id=1GgrXA_Q4_bx6zR6tLevtJj11fYsFgx4E) ，[建议 8](https://drive.google.com/open?id=1opDrJkbXtqC4oRVzdOt2_CZiDCC8f1jF)

再一次，没有一个提供者得到容易识别的错误图像。这些图像显然都是 NSFW 的。

![](img/eee5af7f4cd4eff1a3d3e644cbe69e35.png)

**原图链接:** [建议 17](https://drive.google.com/open?id=1bp7AndXpvaEp3E-4SduzxqdntBnNfl3f) ，[建议 12](https://www.dropbox.com/s/plzn9lbvo3mhorg/12.jpeg?dl=0) ，[建议 11](https://drive.google.com/open?id=1r7OfNG-jJMGk-YMmxJIhOl-WjV_SsZVM) ，[建议 5](https://drive.google.com/open?id=16Pr1hqTp_1-59M98korixbtFTCCMNoZH)

在暗示性的裸体中，提供者分裂了。类似于赤裸裸的裸露，他们都有不同的容忍界限。我个人不确定这些图片是否应该是 SFW。

# 模拟/动画色情

所有的 API 在这里都表现得非常好，能够准确地检测出 100%的模拟色情例子。唯一的例外是 IMAGGA，它漏掉了一幅图像。有趣的是，几乎所有的供应商都表现得很好。这表明这些算法发现识别人工生成的图像比自然生成的图像更容易。

![](img/ef40971044ede78db3daa420a9d0327c.png)![](img/a73837f0fe6efab3128560dbb2c5d7d1.png)

**原图链接:** [模拟炮 1](https://drive.google.com/open?id=1oqoZZI_GideOBenDKcYA3wGnbxDLj1K4) ，[模拟炮 16](https://drive.google.com/open?id=17EvQouBr_9liFHonLI-f6YQBcx1G2Mf0) ，[模拟炮 19](https://drive.google.com/open?id=1Rxu_3bhUIJVhtXZq3cfVQeQHGBcbFWcp) ，[模拟炮 9](https://drive.google.com/open?id=1OTnRY6l925YRglTXExP2fMfIYGeoA618)

所有的提供者都有完美的分数和高的置信度分数。

![](img/0bf946e39183c68ea3494585aff0c217.png)

**原始图像链接:** [模拟图 15](https://www.dropbox.com/s/lrd0kfkqkhv2ftk/15.jpg?dl=0)

如果你看得不够久的话，Imagga 弄错的一张图片可能会被解释为不是色情图片。

# 流出的血

这是最困难的类别之一，因为 API 的平均检出率低于 50%。Clarifai 和 Sightengine 的表现优于其竞争对手，能够识别 100%的戈尔图像。

![](img/134ef23fc87d192fc0ce75b8742f6ef2.png)![](img/4743561ba83b3c8ae90fbe3b95762a84.png)

**原图链接:** [Gore2](https://drive.google.com/open?id=1_88BRMTNzvbGTI9LgyGB3Lz7HdIrEIqf) ， [Gore3](https://drive.google.com/open?id=10LGE_sonXjIGB2sTby16UtHes1Ay2Zlo) ， [Gore6](https://drive.google.com/open?id=103kTePqccdLqwqxgHcumGoPSuPV8v8Ez) ， [Gore10](https://drive.google.com/open?id=1J2YwtYzDUyK9ESdyxjaWLArws6UdFCRD)

所有供应商都有高门槛的是医学图像，可能是因为它们更容易找到。然而，即使在表现最好的图像中，4/12 的提供者也弄错了图像。

![](img/2577da1377b25c6e2c3a50baa6b2a3c8.png)

**原图链接:** [Gore7](https://drive.google.com/open?id=1TQdH6LbLX3nb9NBUc2VATjHj9WMRpyLf) ， [Gore9](https://drive.google.com/open?id=1UqYTt2Bsh3vufu9TTUHyNAwqCzHr5UtF) ， [Gore17](https://drive.google.com/open?id=119CQhBe-7Pq2fAWIvMRYsIidi0Q1Icde) ， [Gore18](https://drive.google.com/open?id=1Qzo_SM4-CKDWdCL7AfSfa4jSAFRL80_8)

图像中没有难以预测的明显模式。然而，人类会很容易发现这些图像中的任何一个是血腥的。这可能意味着表现不佳的原因是缺乏可用的训练数据。

# 工作安全

安全的工作是不应该被识别为 NSFW 的图像。收集一个安全的工作数据集本身是困难的，它应该靠近 NSFW，以了解这些供应商做的事情。如果所有这些图片都是 SFW，那么会有很多争论。在这里，Sightengine 和 Google 是表现最差的，这也解释了他们在其他类别中的出色表现。他们基本上把任何东西都叫做 NSFW。Imagga 在这里做得很好，因为他们不叫什么 NSFW。X-Moderator 在这里也做得很好。

![](img/98a8c06edbf0df8428a87f81cafd18c7.png)![](img/f584aab6f74df04e10f7683a2ef3966d.png)

**原图链接:** [SFW15](https://drive.google.com/open?id=1MJKxx0irLVWLd0g4Mvf4CMv76rXBT7oo) ， [SFW12](https://drive.google.com/open?id=1lCY1Ivwjb5lDnvWge6_qsbsqx2OFfSt1) ， [SFW6](https://drive.google.com/open?id=1SE1LnhENoNZYT2OiU1yyVA3d-2y01YNA) ， [SFW4](https://drive.google.com/open?id=1Mhac-551FPJIdDBYrP3UrquGLi0lSnPF)

易于识别的图像只有很少的皮肤显示，对于人来说很容易识别为 SFW。只有一两个供应商弄错了这些图像。

![](img/b00ebd2ac7e4d8648d55a08dadb1a35f.png)

**原图链接:** [SFW17](https://drive.google.com/open?id=1357lms8figMDZYCblKWC-7iUz20kTdiG) ， [SFW18](https://drive.google.com/open?id=1nyE_AqnUzWlrs_Bph2Coz32y6LfGQJqP) ， [SFW10](https://drive.google.com/open?id=1VhNgEuFZA_jtm1mvUPG3OtqGXqN9Wng8) ， [SFW3](https://drive.google.com/open?id=1C4cT6Mxzsn5HSPr0M42nhyIQzp1jYSnh)

难以识别的 SFW 图片都有大量的皮肤展示或者是动漫(高度偏向于色情动漫)。大多数供应商得到的图片都带有大量显示为 SFW 的 sking。这就引出了一个问题，如果这些真的是 SFW？

# 总体比较

查看 API 在所有 NSFW 类别中的性能，以及它们在能够正确识别工作安全(SFW)内容方面的性能，我发现 Nanonets 具有最好的 F1 分数和平均准确性，因此在所有类别中表现一致。谷歌在检测 NSFW 类别方面做得非常好，它标记了太多的 SFW 内容，因为 NSFW 因此在 F1 得分中受到了惩罚。

![](img/f692d6f4990bf08bb0249082054199fc.png)![](img/efaecdcc429db78099c87b15bd2e5d36.png)![](img/fa67a23dfd9372127999607c7913924e.png)

# 按提供商

我根据准确性和 F1 分数比较了前 5 名供应商，以展示他们在表现上的差异。雷达图的面积越大越好。

## 1.纳米网络

Nanonets 在任何一个类别中的表现都不是最好的。然而，最平衡的总体情况是在每个类别中都做得很好。它可以做得更好的地方是将更多的图像识别为 SFW。对任何皮肤都过度敏感。

![](img/c193b41074c3f6b38a0c17d8f2903c23.png)

## 2.谷歌

谷歌在大多数 NSFW 类别中表现最佳，但在检测 SFW 方面表现最差。需要注意的一点是，我找到的图片来自谷歌，这意味着他们“应该知道”我使用的图片是什么。这可能是大多数类别中真正好的表现的原因。

![](img/63f901d68c600c4342511ea2fde7ae6a.png)

## 3.克拉里菲

Clarifai 确实在识别戈尔方面大放异彩，并且比大多数其他 API 做得更好，它再次得到了很好的平衡，并且在大多数类别中都做得很好。它缺乏识别暗示性的裸体和色情。

![](img/425935663e881cef026c325d11f5c00f.png)

## 4.x-慢化剂

X-Moderator 是另一个平衡的 API。除了识别戈尔外，它还能很好地识别大多数其他类型的 NSFW 内容。它在 SFW 中获得了 100%的准确性，这使它从竞争对手中脱颖而出。

![](img/16ac1b4e65252b6f0ac42a7d5b6580a8.png)

## 5.视觉引擎

像谷歌这样的视觉引擎在识别 NSFW 内容方面有着近乎完美的表现。然而，它没有识别出一个单一的戈尔图像。

![](img/26d269b05e88cc5f8fb0fd95fb2d6887.png)

## 定价

决定使用哪种 API 的另一个标准是定价。下面是每个供应商的价格比较。大多数 API 都有免费试用版，但使用量有限。雅虎是唯一一个完全免费使用的网站，但它是自主托管的，因此不在此表中。

![](img/71bc5ef3b79ba35fa58fef3edfddc93a.png)

> 亚马逊，微软，Nanonets，DeepAI 都是最低的，每月 1000 万美元的 API 调用。

# 哪一个是最好的内容审核 API？

NSFW 内容的主观性使得很难将任何一个 API 声明为内容审核的首选 API。

**更适合内容分发并希望有一个平衡的分类器的一般社交媒体应用程序更倾向于使用 Nanonets API，其分类器的最高 F1 分数证明了这一点。**

一个针对儿童的应用程序肯定会犯谨慎的错误，甚至会喜欢隐藏边缘的不适当内容，因此他们会更喜欢使用谷歌 API，其在所有 NSFW 类别上的出色性能会冒着过滤掉一些适当内容的风险。这样做的代价是会失去很多谷歌可能宣布为 NSFW 的 SFW 内容。

![](img/fe8dba5ab1ea64d509cd914b97ffcae4.png)![](img/5aea3f79b0f712debc869ec3d26c859d.png)

# NSFW 到底是什么？

![](img/4a3e6c4f7d0f9f478dc0d2ebed3a0551.png)

在这个问题上花了大量时间后，我意识到的一个关键问题是，NSFW 到底是什么还不清楚。每个人都有自己的定义，你认为你的服务向用户展示什么是好的，这在很大程度上取决于服务提供了什么。约会应用程序中的部分裸体可能没问题，但血液则不然，在医学杂志中，情况正好相反。真正的灰色地带是暗示性的裸体，在那里不可能得到正确的答案。