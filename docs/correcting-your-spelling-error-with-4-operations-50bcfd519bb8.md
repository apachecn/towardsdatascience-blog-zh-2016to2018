# 通过两个距离纠正你的拼写错误

> 原文：<https://towardsdatascience.com/correcting-your-spelling-error-with-4-operations-50bcfd519bb8?source=collection_archive---------13----------------------->

![](img/1166842258fdd386ec38f42092f29e28.png)

“red pencil on top of mathematical quiz paper” by [Chris Liverani](https://unsplash.com/@chrisliverani?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

在处理文本时，我们可能需要处理不正确的文本。尽管我们仍然可以使用[字符嵌入](/besides-word-embedding-why-you-need-to-know-character-embedding-6096a34a3b10)和[单词嵌入](/3-silver-bullets-of-word-embedding-in-nlp-10fa8f50cc5a)来计算类似的向量。它对看不见的数据和词汇表之外的东西很有用(OOV)。不过如果能纠正错别字就更好了。

错别字可以在几种情况下产生。如果您使用光学字符识别(OCR)，OCR 输出的后处理步骤是非常关键的部分，因为 OCR 引擎会引入一些错误，这些错误可能是由图像质量差和 OCR 引擎错误引起的。另一个错别字来源于人类。当你在聊天机器人项目工作，输入来自人类，它必须包括错别字。

为了获得更好的结果，最好是尽早纠正错别字。看完这篇帖子，你会明白:

*   拼写纠正器
*   履行
*   拿走

# 拼写纠正器

Norvig 在 2007 年实现了[一个非常简单但是惊人的库](https://norvig.com/spell-correct.html)来纠正拼写错误。通过不同的方式计算可能的候选校正，并从中找到最可能的单词。有两个阶段来寻找可能的候选词。

![](img/b237ef96dc603baf009ecc62ae6e4dc1.png)

“brown rail train” by [Johannes Plenio](https://unsplash.com/@jplenio?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

首先，当原始词和候选词之间的编辑距离为 1 时，使用 4 种不同的方法生成新词。不同于 [Levenshtein 距离](/measure-distance-between-2-words-by-simple-calculation-a97cf4993305)，它认为:

*   删除:删除一个字母
*   换位:交换两个相邻的字母
*   替换:把一个字母换成另一个
*   插入:添加一个字母

以“edward”为例，“edwar”、“edwadr”、“edwadd”、“edwward”分别是“删除”、“换位”、“替换”、“插入”的例子。显然，将会产生大量的无效单词。因此，它会按给定的词汇过滤掉(它在库中称为“已知词”)。为了扩大潜在的候选，算法再次重复这个步骤，但是编辑距离是 2。

第二部分是根据概率从可能的候选者中选择候选者。例如,“Edward”在给定字典中的出现次数是 2%,概率是 0.02。概率最高的词将从潜在候选词中选出。

# 履行

为了方便拼写检查，需要语料库。为了便于演示，我简单地使用了 sklearn 库中的数据集，没有进行预处理。您应该使用特定领域的数据集来为您的数据构建更好的语料库。

*建立语料库*

```
from collections import Counter
from sklearn.datasets import fetch_20newsgroups
import recorpus = []
for line in fetch_20newsgroups().data:
    line = line.replace('\n', ' ').replace('\t', ' ').lower()
    line = re.sub('[^a-z ]', ' ', line)
    tokens = line.split(' ')
    tokens = [token for token in tokens if len(token) > 0]
    corpus.extend(tokens)corpus = Counter(corpus)
```

*校正*

```
spell_corrector = SpellCorrector(dictionary=corpus)
spell_corrector.correction('edwar')
```

*输出为*

```
edward
```

# 拿走

要访问所有代码，你可以访问我的 github repo。

*   拼写校正器**不考虑上下文**，而仅仅考虑拼写。然而，鉴于它是在 11 年前(2007 年)推出的。这是一个神奇的工具。
*   从作者编码来看，预处理结果应该只保留英文字符和小写字母。换句话说，**特殊字符和数字要去掉**。
*   性能(就速度而言)非常快。

# 关于我

我是湾区的数据科学家。专注于数据科学、人工智能，尤其是 NLP 和平台相关领域的最新发展。你可以通过[媒体博客](http://medium.com/@makcedward/)、 [LinkedIn](https://www.linkedin.com/in/edwardma1026) 或 [Github](https://github.com/makcedward) 联系我。

# 参考

[拼写校正器](https://norvig.com/spell-correct.html)