# 文本数据的数据扩充:更快地获得更多数据

> 原文：<https://towardsdatascience.com/data-augmentation-for-text-data-obtain-more-data-faster-525f7957acc9?source=collection_archive---------11----------------------->

![](img/ac53a21e5e60254a8bfbe4055d94510b.png)

[source](https://pixabay.com/en/signs-directions-choosing-judgment-1172211/)

**简介**

所有大多数关于使用监督学习算法进行文本分类的教程都是从预先标记的数据开始的。很少提到的是如何处理情感分析/文本分类中不那么迷人但非常关键的部分，即收集文本的标记。它没有那么迷人，因为要训练一个受监督的模型，你需要向它展示带标签的文本。你如何或从哪里得到标签数据？获得标记数据的一个选择是通过手动阅读每个文本来注释您需要的所有数据，并将其分类为负面或正面。另一种选择是将标签外包给自由职业者或致力于数据标签的公司。这两种选择在时间和金钱上都是昂贵的。在本文中，我们将研究如何在内部使用数据扩充的概念来加速标注任务。对文本数据使用数据增强的灵感来自埃米利奥·拉皮耶洛的[博客文章](https://medium.com/bcggamma/shuffling-paragraphs-using-data-augmentation-in-nlp-to-increase-accuracy-477388746bd9)。

**数据增强的定义**

数据扩充是一种通常用于为图像分类任务增加图像数据集大小的技术。它包括通过变换(旋转、平移或/和缩放，添加一些噪声)数据集中的图像来创建新的图像。然而，当应用数据扩充时，文本数据的等效变换是将要扩充成句子的文档/文本的记号化，将这些句子混洗并重新组合它们以生成新的文本。

为了在文本上测试数据扩充，我们在 OpinRank 数据集上应用了它，但它可以用于任何其他数据，其中:

*   其中一个类别比其他类别更普遍。比如你在主要来源中正面例子的比例是 0.8，负面例子的比例是 0.2，换句话说，比负面更容易得到正面的评论。我使用 main source 来引用数据存储文件，如。csv，。txt，数据库等。
*   而且，算法要分类的评论或文本通常足够长，比如说大约 10 个句子，如下所示:

*“Clean hotel with certain charm. Good location and value. A mid-sized modern hotel with facade that’s been themed after bamboos, Kapok is within walkable (~15 min) distance from Subway line 1 Tiananmen East station, Wangfujing station, and line 5 Dengshikou station. Situated near Donhuamen (East gate) of the Forbidden Palace and Wangfujing, it’s location-wise competitive and can cost a whole lot less than the larger hotels that are even closer to Subway stations.The hotel is built and furnished with certain design elements that separate it from the run-of-the-mill chains; its interior has glass walls that give a sense of space while in corridors, there are sprinklings of mini-gardens, and the glass-walled bathrooms have rather stylish fixtures. All in all they make for an interesting experience, though not always comfortable (bathrooms could’ve been designed with more human ergonomics in mind). Rooms are generally clean, except for some mildew on the silicons at shower space. There are also a couple of convenience stores to each side of the hotel for stocking up on water and snacks (one near the Donghuamen-Nanchizi junction, and one near the Donganmen-Wangfujing junction).In conclusion, we think this hotel offers a very competitive combination of good location, clean accommodation, nearby nightlife, and attractive price. We do recommend this hotel. “*

**How much data required before applying Augmentation?**

The main purpose of using Data Augmentation for text data in this article,as the title states,is to obtain more data faster. So the way we set the foundation for Data Augmentation on text data is by first manually labeling at least 1000 examples of the frequent occurring class and a few hundreds of the lesser common classes. Next, by reading each of labelled text we use *NLTK sent_tokenize* and Python’s shuffle method from the random module to generate the new texts. 10 different reviews were created from each old review. The difference in the generated texts is based on the changed position of the sentences. As Emilio Lapiello stated in his post, shuffling works as long as the context of the text is maintained. With sentence tokenization,the sentiment conveyed in the original text is still maintained. It would be lost if reviews were tokenized into words and then shuffled. Actually, although the meaning of a text may be lost due to words shuffling, it would not be an issue because feature extraction does consider order of words in a text.

Wouldn’t just tokenizing a text into sentences and then shuffling lead to over-fitting? You may ask. That’s possible to some extend. Over-fitting can occur when there is a high variance in the sentiment vocabulary used in different reviews. The two reasons why over-fitting is less likely to occur as a result of augmentation used are:

*   根据从用于增强的 202 个负面情绪示例中观察到的情况，词汇，或者更确切地说是同义词，似乎并没有从如下短语转移太多:
*   糟糕的服务
*   我们不推荐这家酒店
*   肮脏的房间，
*   令人不快的/无用的员工等

看不见的数据很可能与算法训练的数据相似，因此模型应该能够识别它，从而正确地对它进行分类。

使用 OpenRink 数据，我们人工标记了 2004 条正面评论和 202 条负面评论。

下表描述了在对原始数据应用数据扩充之前和之后，分别对 14 个和 6 个正面和负面评论的样本外数据的模型性能。

Metric used: F Score

你可以在这里找到文章[使用的代码。](https://github.com/Nandee89/data-augmentation-for-text)

表中的第二列显示，我们显然需要更多的数据来提高性能(尤其是负面评价)。为了避免手动标记更多的数据，我们在负面评论上应用了增强，并将它们增加到 2051。

虽然应用数据增强后的分类准确率仍在 90%以下，但已大幅提升。手动标注更多的评论，以添加到 2004 年和 208 年，然后应用增强应该会增加准确性。

注意:

*   TfidfVectorizer 特征提取器比 CountVectorizer 具有更高的精度
*   确保从一个评论生成的评论是不同的(句子在不同的位置),以避免重复..

**结论**

虽然数据扩充通常用于图像数据，但它也可用于文本数据，以更快地获得更多标记数据。

**参考文献:**

[OpinRank 数据集](http://kavita-ganesan.com/entity-ranking-data/#.W5j44-h_K00)