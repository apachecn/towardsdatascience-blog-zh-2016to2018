# 单词嵌入的一般概念

> 原文：<https://towardsdatascience.com/the-three-main-branches-of-word-embeddings-7b90fa36dfb9?source=collection_archive---------5----------------------->

![](img/2fc571afaafda4f16595ad50fad13ad3.png)

Photo by [Dmitry Ratushny](https://unsplash.com/@ratushny?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

## Word2Vec、GloVe 和 FastText 的主要概念概述

**单词嵌入**——或分布式表示——的概念是近年来自然语言处理( **NLP** )中最引人注目的发展。与所有快节奏的领域一样，人们很容易迷失方向，感觉被最新的突破和发展抛在后面。

最好的解药是意识到更普遍的趋势和单词嵌入概念背后的主要思想。为了做到这一点，我在这篇文章中为你提供了一个简要的概述，并在底部提供了其他材料的链接。这篇文章关注三种“经典”风格的单词嵌入: **Word2Vec** 、 **GloVe** 和 **FastText** 。他们举例说明了看待单词嵌入的三种不同方式。

![](img/17c866f638c7477753366444f47ef0e7.png)

[Evan Kirby](https://unsplash.com/@evankirby2?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 这一切是如何开始的— Word2Vec (2013)

单词嵌入的雪崩始于 2013 年，当时托马斯·米科洛夫周围的谷歌研究人员发表了 [**这篇论文**](https://arxiv.org/abs/1301.3781) 。他们提出了一种方法，即众所周知的 Word2Vec。它使用小型神经网络来计算基于单词上下文的单词嵌入。有两种方法可以实现这种方法。

首先是**连续袋字** 或 **CBOW** 。在这种方法中，网络试图根据上下文预测哪个单词最有可能出现。同样可能出现的单词可以被解释为具有共享维度。如果我们可以将一个句子中的"*猫"*替换为"*狗*"，这种方法预测两者的概率相似。所以我们推断这几个词的意思至少在一个层面上是相似的。

第二种方法是**跳格**。这个想法非常相似，但是网络的工作方式正好相反。也就是说，它使用目标单词来预测其上下文。更多细节请看本文末尾的链接。

当 2013 年 Word2Vec 问世时，结果是前所未有的，但也很难从理论角度解释。它工作了，但是有一些困惑为什么。

![](img/67284b9bb79c5819aba502187a702a27.png)

[Gary Bendig](https://unsplash.com/@kris_ricepees?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 斯坦福大学的竞争方法——GloVe(2014 年)

一年后，斯坦福的研究人员发布了**手套**。你可以在这里找到[**原文**](https://nlp.stanford.edu/pubs/glove.pdf) 。为了理解这个变体试图做什么，我们需要简单地讨论一下 Word2Vec 的一个不太明显的方面。

Word2Vec 通过将目标单词与其上下文相关联来学习嵌入。但是，它忽略了某些上下文单词是否比其他单词出现得更频繁。对于 **Word2Vec** ，一个词的频繁同现创造了**更多的训练实例**，但是它没有携带**额外的信息**。

相比之下， **GloVe** 强调**共现的频率是至关重要的信息**，不应作为额外的训练示例而被“浪费”。相反，GloVe 构建单词嵌入的方式是，单词向量的组合与这些单词在语料库中共现的概率直接相关。

同样，查看底部的链接了解更多详情。对于这篇文章来说，理解 GloVe 不是 Word2Vec 意义上的训练有素的模型就足够了。相反，它的嵌入可以被解释为反映共现的低维度训练语料库的总结。

![](img/37d5ad08bf38259e85cb29cf64f3b2ff.png)

[Loverna Journey](https://unsplash.com/@loverna?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 轮到脸书了——快速文本(2016)

2016 年，人工神经网络已经获得了相当大的吸引力，Word2Vec 已经证明了它在 NLP 的许多领域中的实用性。然而，还有一个问题没有解决:对未知单词的泛化。脸书于 2016 年发布的开发产品 FastText 承诺要克服这一障碍。

这个想法非常类似于 Word2Vec，但是有一个主要的变化。与其使用单词来构建单词嵌入， **FastText 更深入一层**。这个更深的层次由部分单词和字符组成。从某种意义上说，一个词成为它的上下文。因此，基石是文字，而不是语言。

FastText 输出的单词嵌入看起来与 Word2Vec 提供的非常相似。但是，它们不是直接计算的。相反，它们是低层嵌入的组合。

这种方法有两个主要优点。首先，**泛化是可能的**只要新单词与已知单词具有相同的特征。第二，需要较少的训练数据，因为可以从每段文本中提取更多的信息。这就是为什么预训练的快速文本模型比其他任何嵌入算法都适用于更多的语言。

![](img/49e511f6870af6c1d206a2d2fcf582ff.png)

Photo by [Eugenio Mazzone](https://unsplash.com/@eugi1492?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 主要的收获

我选择这三种算法是因为它们代表了关于如何计算单词嵌入的三种一般思想:

1.  Word2Vec 将文本作为神经网络的训练数据。最终的嵌入捕获单词是否出现在相似的上下文中。
2.  **GloVe** 关注整个语料库中的同现词。它的嵌入与两个词一起出现的概率有关。
3.  FastText 对 Word2Vec 进行了改进，也考虑了单词部分。这种技巧能够在较小的数据集上训练嵌入，并推广到未知单词。

如果对你有帮助或者你想补充什么，请在评论中或者在[推特](https://twitter.com/TimoBohm)上告诉我。我也很乐意在 [LinkedIn](https://www.linkedin.com/in/timo-boehm-datascience/) 上联系。**感谢阅读！**

# 附加材料

## Word2Vec

如果你想深入了解，这里有两篇马尼什·查布拉尼的精彩博文:

[](/word2vec-skip-gram-model-part-1-intuition-78614e4d6e0b) [## Word2Vec(跳格模型):第 1 部分——直觉。

### 这里的大部分内容来自克里斯的博客。我对它进行了压缩，并做了一些小的改动。

towardsdatascience.com](/word2vec-skip-gram-model-part-1-intuition-78614e4d6e0b) 

如果你正在寻找 Word2Vec 如何进入机器学习的其他领域的例子，看看 Ramzi Karam 的这篇博文:

[](/using-word2vec-for-music-recommendations-bb9649ac2484) [## 使用 Word2vec 进行音乐推荐

### 我们如何使用神经网络将数十亿条数据流转化为更好的推荐。

towardsdatascience.com](/using-word2vec-for-music-recommendations-bb9649ac2484) 

## 手套

Brendan Whitaker 写了一个关于手套的五集系列，我强烈推荐。这是第一部分，但很容易找到其他四个部分:

[](/emnlp-what-is-glove-part-i-3b6ce6a7f970) [## 什么是手套？第一部分

### 从共现矩阵无监督学习单词嵌入的介绍。

towardsdatascience.com](/emnlp-what-is-glove-part-i-3b6ce6a7f970) 

## 快速文本

[Nishan Subedi](https://medium.com/u/ceeb00afdff0?source=post_page-----7b90fa36dfb9--------------------------------) 在此更详细地描述了 FastText:

[](/fasttext-under-the-hood-11efc57b2b3) [## 快速文本:引擎盖下

### 在这里我们将看到一个性能最好的嵌入库是如何实现的。

towardsdatascience.com](/fasttext-under-the-hood-11efc57b2b3) 

## 履行

There are different ways to implement word embeddings. [黃功詳 Steeve Huang](https://medium.com/u/2fc7b9c3f02a?source=post_page-----7b90fa36dfb9--------------------------------) describes ways to do this with the Gensim package:

[](/word-embedding-with-word2vec-and-fasttext-a209c1d3e12c) [## 使用 Gensim 嵌入 Word2Vec 和 FastText 单词

### 在自然语言处理(NLP)中，我们经常将单词映射成包含数值的向量，以便机器能够识别

towardsdatascience.com](/word-embedding-with-word2vec-and-fasttext-a209c1d3e12c) 

另一种方法是在 tensorflow 中重建结构。aneesh joshi 在这里描述了细节:

[](/learn-word2vec-by-implementing-it-in-tensorflow-45641adaf2ac) [## 通过在 tensorflow 中实现来学习 Word2Vec

### 通过 tensorflow 中的编码理解 word2vec

towardsdatascience.com](/learn-word2vec-by-implementing-it-in-tensorflow-45641adaf2ac)