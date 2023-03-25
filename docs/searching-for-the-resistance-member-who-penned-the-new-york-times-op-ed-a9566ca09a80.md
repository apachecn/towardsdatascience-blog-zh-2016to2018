# 寻找为《纽约时报》撰写专栏的抵抗组织成员

> 原文：<https://towardsdatascience.com/searching-for-the-resistance-member-who-penned-the-new-york-times-op-ed-a9566ca09a80?source=collection_archive---------10----------------------->

## 自然语言处理能预测作者的真实身份吗？

![](img/13c2c7959156c3db87d27ec08a3a0026.png)

周三下午,《纽约时报》发表了一篇令人震惊的观点文章，详细描述了一些白宫顾问为遏制特朗普总统最鲁莽的倾向而进行的秘密抵抗，这是这家著名(或者我敢说，失败)报纸发表匿名评论的罕见例子。该专栏的作者被描述为一名未透露姓名的“特朗普政府高级官员”，他以匿名的名义撰写，可能是为了避免作者立即被解雇，但也是为了让读者能够专注于故事的重要信息，而不是信使。几分钟内，这篇文章就像病毒一样传播开来，因为现在是 2018 年，Twitter 和全国各地的政治学者立即开始搜索这位作者。大量的理论迅速充斥网络，几个小时内[多家博彩公司](https://www.predictit.org/Market/4823/Who-will-NYT-confirm-as-anonymous-op-ed-author-in-2018)发布了[的投注赔率](https://mybookie.ag/sportsbook/trump-specials/)。当互联网侦探在一些暗示作者身份的奇怪短语上划区域时(对于任何声称在阅读文章之前知道‘lodestar’是什么意思的人来说——[Google Trends 保留收据](https://trends.google.com/trends/explore?geo=US&q=lodestar))，我在过去 3 个月里完全沉浸在数据科学中，立即想到是否有可能使用自然语言处理来揭示作者的身份。这是我对破案的初步尝试。

Trump criticizing the “amomynous” author

## 假设

首先，我需要对这个列做一些假设。虽然 NYT 的一条推文最初称作者为“他”([一名时报发言人后来澄清这是一个错误](https://www.businessinsider.com/trump-official-op-ed-resistance-movement-administration-2018-9))，但我忽略了作者的性别被如此无伤大雅地暴露出来的想法。相反，我认为他们被称为男性，因为政府中没有高级别的女性，如果作者被揭露为女性，公众很容易发现他们的身份。我还必须假设观点文章是用作者自己的声音写的，而不是由演讲稿撰写人精心制作的，或者经过大量编辑，因此不可能符合作者的独特风格。【Axios 公司的张克帆·斯旺之前的一份报告称，一名白宫官员详细描述了他们如何“注意其他员工的习惯用语，并将其用在[他们的]背景引语中”，以掩盖他们的踪迹。

![](img/1ac7c12c154658a960b0c7eb249057c4.png)

These people **definitely** did not write it

我最后的假设是，一名政府官员的[公开否认](https://www.cnn.com/2018/09/06/politics/trump-officials-denials-nyt-op-ed/index.html)并不排除那个人写了这篇文章——知道被曝光的可怕后果，对他们的参与撒谎是他们的最大利益。没有这些假设，就不可能准确地预测作者，我必须强调，目前的这项工作是一项非常不精确的研究。

一旦我的假设成立，我必须得到一份潜在作者的名单。不幸的是，对我自己和这个国家来说，有一个长得惊人的潜在候选人名单，涵盖了行政部门的所有领域。

出于时间的考虑，我把名单缩小到了三个人，每个人都有可能成为候选人，他们都有公开的演讲和在线发表的专栏文章。我最初选择关注的三个人是副总统迈克·彭斯、驻联合国妮基·黑利大使和现任经济顾问委员会主席凯文·哈塞特。Pence 被包括在内是因为“lodestar”这个词的奇怪用法，在文章发布几分钟后，Twitter 用户 [@danbl00m](https://twitter.com/danbl00m/status/1037428190166347776) 发现它是副总统(显然不是其他人)常用的词之一。这篇文章对外交政策失误和经济及放松管制的成功进行了深入的审视，因此我觉得有必要将各个学科的官员都包括进来。我选择了哈利和哈塞特，因为在我看来，他们都是高级官员，在很大程度上避开了目前笼罩白宫的真人秀聚光灯。

## 余弦相似性

一旦我有了我想看的三个候选人的名单，我就在网上搜寻这些人的观点、文章和演讲。我使用的第一种方法是**余弦相似度**，它使用两个矢量化语料库之间的余弦角来衡量相似度，输出越接近 1 被视为越相似。使用术语频率-逆文档频率(TF-IDF)将每个语料库转换为向量，我能够计算潜在作者和匿名抵抗成员之间的相似性。

![](img/3727bad29ba639e98d6855e73fc07874.png)

好消息是每个作者的余弦相似度接近 1，这意味着很好的匹配。坏消息是，这三个数字被如此紧密地组合在一起，以至于无法确定哪个(如果有的话)与作者最相似。我推断，三个余弦相似性之间缺乏变化是由于一个小的目标集，在这种情况下，只有 881 个词写在专栏中。

## TF-IDF 降维

我尝试的下一个方法是使用 t-SNE 将每个候选人的 TF-IDF 向量矩阵的维度降低到三维，然后绘制每个候选人的图表，以查看是否有任何候选人比其他人更接近专栏作家。令我沮丧的是，这 4 个点就像我能想象的那样平均分布，对于作者可能是谁没有留下任何洞察力。

![](img/084987b5e48476dbee5ed0c875578e4c.png)

Useless 3D Scatter Plot

## 作者归属

我的最后一次尝试使用了尼尔·雅戈开发的作者归属代码，它分析了四个不同的特征来预测哪个作者最有可能写了一篇文章。这四个特征分别是:**词汇特征**，如每句话的平均字数和作者词汇量的大小；**标点特征，**包括每个句子中逗号、分号和冒号的平均数量；**单词包特征，**表示特定单词在文档中出现的频率；以及**句法特征，**将每个词归类到某个词类(名词、动词等。).在大多数 NLP 实例中，通常会删除在所有文档中频繁出现的单词，如“the”和“are ”;然而，作者归属的操作不同于正常的文本分类，并且有一种信念，即作者以一种独特但一致的方式使用这些常用词。

```
###### code to create lexical and punctuation feature vectors ######for i, text in enumerate(authors):

    tokens = nltk.word_tokenize(text.lower())
    words = word_tokenizer.tokenize(text.lower())
    sentences = sentence_tokenizer.tokenize(text)
    vocab = set(words)
    words_per_sentence = np.array([len(word_tokenizer.tokenize(s))
                                   for s in sentences])

    # Average number of words per sentence
    fvs_lexical[i, 0] = words_per_sentence.mean()
    # Sentence length variation
    fvs_lexical[i, 1] = words_per_sentence.std()
    # Lexical diversity
    fvs_lexical[i, 2] = len(vocab) / float(len(words))

    # Commas per sentence
    fvs_punct[i, 0] = tokens.count(‘,’) / float(len(sentences))
    # Semicolons per sentence
    fvs_punct[i, 1] = tokens.count(‘;’) / float(len(sentences))
    # Colons per sentence
    fvs_punct[i, 2] = tokens.count(‘:’) / float(len(sentences)) 
```

在创建了特征向量之后，我继续在特征空间中寻找三个聚类，希望至少一些(如果不是所有的)专栏特征将与候选人的那些特征共享一个聚类。我决定用三个集群来测试它，假设三个已知作者中的每一个都将他们的功能作为独特集群的一部分，而论坛版功能将属于其中之一。

```
author_files=[pence,hassett,haley,oped]**code:**
fvs_list = [fvs_lexical,fvs_punct,fvs_bow,fvs_syntax][PredictAuthors(fvs).labels_ for fvs in fvs_list]**output:**
[array([0, 0, 2, 1], dtype=int32),
 array([2, 1, 0, 0], dtype=int32),
 array([0, 1, 2, 2], dtype=int32),
 array([0, 2, 0, 1], dtype=int32)] 
```

![](img/48057367f10cdfc5c4f89bbaf1a7f4a8.png)

Representation of which cluster each feature belongs to

数组中的每一行代表一个不同的特性，而列代表四个不同的作者(最后一列是专栏)。数字表示每个要素属于哪个聚类。同样，结果是不确定的，尽管它们比前两种方法提供了更多的信息。该论坛版的标点符号和单词袋功能与妮基·黑利的相同功能共享一个群集，而其其他功能都不属于与其他两位作者的匹配功能相同的群集。这是朝着正确方向迈出的一步，但仍然没有给出明确的答案。

在接下来的几天里，我希望收集更多政府成员的发言和文章，看看是否有官员引人注目。这个最初的尝试给了我一个坚实的感觉，我将需要使用的技术，以作出准确的预测。我期待改进这个模型，并对作者的真实身份给出一个有把握的猜测。

**更新:**维基解密将搜索范围缩小到一名年长的保守男性。