# 《哈利·波特》系列的文本摘要

> 原文：<https://towardsdatascience.com/text-summarization-on-the-books-of-harry-potter-5e9f5bf8ca6c?source=collection_archive---------7----------------------->

## 几种算法的比较

![](img/729b0e087cb4eed9984119335ff950cc.png)

“Harry sat with Hermione and Ron in the library as the sun set outside, tearing feverishly through page after page of spells, hidden from one another by the massive piles of books on the desk in front.”

我是 Greg Rafferty，湾区的数据科学家。你可以在我的 [github](https://github.com/raffg/harry_potter_nlp) 上查看这个项目的代码。如有任何问题，请随时联系我！

在这一系列的文章中，我通过《哈利·波特》的镜头来看一些简便的自然语言处理技术。在这个关于基本自然语言处理的系列文章中，之前的文章着眼于使用潜在狄利克雷分配和[正则表达式](/regex-on-the-texts-of-harry-potter-96b8a3878303)的[主题建模，我的下一篇文章将着眼于情感分析。](/basic-nlp-on-the-texts-of-harry-potter-topic-modeling-with-latent-dirichlet-allocation-f3c00f77b0f5)

> 赫敏打断了他们。"你们俩不打算读《霍格沃茨，一段历史》了吗？"

在整个《哈利·波特》系列中，赫敏让哈利和罗恩读了多少遍大部头的《霍格沃茨，一段历史》？提示:很多。他们三个在图书馆度过了多少个夜晚，通读他们能找到的每一本书来弄清尼可·勒梅是谁，或者如何在水下生存，或者为他们的 O.W.L.s .做准备？他们犯的错误是试图自己阅读所有的东西。

还记得当你在学校偶然发现那本书的 CliffsNotes 摘要，你从来没有读过，但应该写一篇文章吗？这就是文本摘要的基本功能:为任何大型文档提供 CliffsNotes 版本。现在，CliffsNotes 是由受过良好教育的人写的，他们熟悉他们正在总结的书。但是现在是二十一世纪，难道计算机不应该让人类失业吗？我研究了一些文本摘要算法，看看我们是否准备好让可怜的老克利夫顿失业。

有两种类型的文本摘要算法:抽取的和抽象的。所有提取摘要算法都试图对文档中的短语或句子进行评分，并且只返回最具信息量的文本块。抽象文本摘要实际上创建了文档中不存在的新文本。抽象概括是你在向你的朋友解释你读过的一本书时可能会做的事情，对计算机来说，这比抽象概括要困难得多。计算机在创造方面并不那么出色。到目前为止，还没有任何抽象的摘要技术能够很好地处理长文档。表现最好的只是根据一个段落造一个句子，或者把句子的长度减半，同时保留尽可能多的信息。通常，语法会受到严重影响。它们通常基于神经网络模型。这篇文章将关注更简单的文本摘要技术。

我将展示的大多数算法都打包在 Python 的`sumy`包中，但我也使用了`Gensim`包中的一个摘要器和我自己编写的另一个技术，使用 LDA 主题关键字来丰富`sumy` `EdmundsonSummarizer`。所有的例子都输出了对*哈利·波特与魔法石*第一章的五句总结。完整代码见我的 [Jupyter 笔记本](https://github.com/raffg/harry_potter_nlp/blob/master/text_summarization.ipynb)。还有一句忠告:不要太苛刻地评判结果。它们不太好……(文本摘要似乎在枯燥的非小说类作品上效果更好)

## LexRank 摘要生成器

LexRank 是一种无监督的方法，其灵感来自谷歌 PageRank 算法背后的相同想法。作者说，它是“基于句子图形表示中特征向量中心性的概念”，使用“基于句内余弦相似性的连接矩阵”好的，简而言之，它找出文档中所有单词的相对重要性，并选择包含最多高分单词的句子。

```
"The Potters, that's right, that's what I heard —" "— yes, their son, Harry —" Mr. Dursley stopped dead.
Twelve times he clicked the Put-Outer, until the only lights left on the whole street were two tiny pinpricks in the distance, which were the eyes of the cat watching him.
Dumbledore slipped the Put-Outer back inside his cloak and set off down the street toward number four, where he sat down on the wall next to the cat.
"But I c-c-can't stand it — Lily an' James dead — an' poor little Harry off ter live with Muggles —" "Yes, yes, it's all very sad, but get a grip on yourself, Hagrid, or we'll be found," Professor McGonagall whispered, patting Hagrid gingerly on the arm as Dumbledore stepped over the low garden wall and walked to the front door.
Dumbledore turned and walked back down the street.
```

## Luhn 摘要生成器

1958 年，在 IBM 研究院工作的汉斯·彼得·鲁恩发表了第一个文本摘要算法。Luhn 的算法是一种基于 TF-IDF 并查看高重要性单词之间非重要单词的“窗口大小”的简单方法。它还为出现在文档开头附近的句子分配较高的权重。

```
It was now reading the sign that said Privet Drive — no, looking at the sign; cats couldn't read maps or signs.
He didn't see the owls swooping past in broad daylight, though people down in the street did; they pointed and gazed open-mouthed as owl after owl sped overhead.
No one knows why, or how, but they're saying that when he couldn't kill Harry Potter, Voldemort's power somehow broke — and that's why he's gone."
"But I c-c-can't stand it — Lily an' James dead — an' poor little Harry off ter live with Muggles —" "Yes, yes, it's all very sad, but get a grip on yourself, Hagrid, or we'll be found," Professor McGonagall whispered, patting Hagrid gingerly on the arm as Dumbledore stepped over the low garden wall and walked to the front door.
G'night, Professor McGonagall — Professor Dumbledore, sir."
```

## LSA 总结者

[潜在语义分析](http://lsa.colorado.edu/papers/JASIS.lsi.90.pdf)是一种比较新的算法，结合了词频和奇异值分解。

```
He dashed back across the road, hurried up to his office, snapped at his secretary not to disturb him, seized his telephone, and had almost finished dialing his home number when he changed his mind.
It seemed that Professor McGonagall had reached the point she was most anxious to discuss, the real reason she had been waiting on a cold, hard wall all day, for neither as a cat nor as a woman had she fixed Dumbledore with such a piercing stare as she did now.
He looked simply too big to be allowed, and so wild — long tangles of bushy black hair and beard hid most of his face, he had hands the size of trash can lids, and his feet in their leather boots were like baby dolphins.
For a full minute the three of them stood and looked at the little bundle; Hagrid's shoulders shook, Professor McGonagall blinked furiously, and the twinkling light that usually shone from Dumbledore's eyes seemed to have gone out.
A breeze ruffled the neat hedges of Privet Drive, which lay silent and tidy under the inky sky, the very last place you would expect astonishing things to happen.
```

## TextRank 摘要生成器

TextRank 是另一个基于 PageRank 思想的文本摘要器，也是与 LexRank 同时开发的，尽管是由不同的人群开发的。TextRank 比 LexRank 简单一点；虽然这两种算法非常相似，但 LexRank 应用启发式后处理步骤来删除高度重复的句子。

```
Mr. and Mrs. Dursley, of number four, Privet Drive, were proud to say that they were perfectly normal, thank you very much.
They were the last people you'd expect to be involved in anything strange or mysterious, because they just didn't hold with such nonsense.
Mr. Dursley was the director of a firm called Grunnings, which made drills.
He was a big, beefy man with hardly any neck, although he did have a very large mustache.
Mrs. Dursley was thin and blonde and had nearly twice the usual amount of neck, which came in very useful as she spent so much of her time craning over garden fences, spying on the neighbors.
```

## 埃德蒙森总结机

1969 年，Harold Edmundson [开发了以他名字命名的 summarizer](http://courses.ischool.berkeley.edu/i256/f06/papers/edmonson69.pdf) 。埃德蒙森的算法和卢恩的算法是开创性的文本摘要技术之一。使 Edmundson summarizer 与众不同的是，它考虑到了“加分词”，即用户认为非常重要的词；“污名词”，重要性低甚至负面重要性的词；和“停用词”，它们与 NLP 处理中其他地方使用的相同。埃德蒙森建议使用文档标题中的单词作为奖励单词。使用章节标题作为奖励词，这是埃德蒙森输出:

```
The Dursleys shuddered to think what the neighbors would say if the Potters arrived in the street.
When Dudley had been put to bed, he went into the living room in time to catch the last report on the evening news: "And finally, bird-watchers everywhere have reported that the nation's owls have been behaving very unusually today.
Twelve times he clicked the Put-Outer, until the only lights left on the whole street were two tiny pinpricks in the distance, which were the eyes of the cat watching him.
Dumbledore slipped the Put-Outer back inside his cloak and set off down the street toward number four, where he sat down on the wall next to the cat.
He couldn't know that at this very moment, people meeting in secret all over the country were holding up their glasses and saying in hushed voices: "To Harry Potter — the boy who lived!"
```

我做的另一个添加是使用 [LDA](/basic-nlp-on-the-texts-of-harry-potter-topic-modeling-with-latent-dirichlet-allocation-f3c00f77b0f5) 提取主题关键词，然后将这些主题关键词作为额外的奖励词添加回去。经过这一修改，埃德蒙森的结果如下:

```
At half past eight, Mr. Dursley picked up his briefcase, pecked Mrs. Dursley on the cheek, and tried to kiss Dudley good-bye but missed, because Dudley was now having a tantrum and throwing his cereal at the walls.
When Dudley had been put to bed, he went into the living room in time to catch the last report on the evening news: "And finally, bird-watchers everywhere have reported that the nation's owls have been behaving very unusually today.
Twelve times he clicked the Put-Outer, until the only lights left on the whole street were two tiny pinpricks in the distance, which were the eyes of the cat watching him.
One small hand closed on the letter beside him and he slept on, not knowing he was special, not knowing he was famous, not knowing he would be woken in a few hours' time by Mrs. Dursley's scream as she opened the front door to put out the milk bottles, nor that he would spend the next few weeks being prodded and pinched by his cousin Dudley.
He couldn't know that at this very moment, people meeting in secret all over the country were holding up their glasses and saying in hushed voices: "To Harry Potter — the boy who lived!"
```

## `SumBasic Summarizer`

SumBasic 算法于 2005 年开发，仅使用单词概率方法来确定句子的重要性。抱歉，但是它在这份文件上是相当糟糕的。

```
Mr. Dursley wondered.
"Harry.
The cat was still there.
"It certainly seems so," said Dumbledore.
"Yes," said Professor McGonagall.
```

哇哦。太可怕了。咳咳，继续…

## KL 摘要生成器

[KLSum](http://www.aclweb.org/anthology/N09-1041) 算法是一种贪婪的方法，只要 [KL 散度](https://en.wikipedia.org/wiki/Kullback%E2%80%93Leibler_divergence)(熵的一种度量)在减少，它就会向摘要中添加句子。

```
It was on the corner of the street that he noticed the first sign of something peculiar — a cat reading a map.
It grew steadily louder as they looked up and down the street for some sign of a headlight; it swelled to a roar as they both looked up at the sky — and a huge motorcycle fell out of the air and landed on the road in front of them.
He looked simply too big to be allowed, and so wild — long tangles of bushy black hair and beard hid most of his face, he had hands the size of trash can lids, and his feet in their leather boots were like baby dolphins.
"But I c-c-can't stand it — Lily an' James dead — an' poor little Harry off ter live with Muggles —" "Yes, yes, it's all very sad, but get a grip on yourself, Hagrid, or we'll be found," Professor McGonagall whispered, patting Hagrid gingerly on the arm as Dumbledore stepped over the low garden wall and walked to the front door.
He clicked it once, and twelve balls of light sped back to their street lamps so that Privet Drive glowed suddenly orange and he could make out a tabby cat slinking around the corner at the other end of the street.
```

## 缩减汇总器

归约算法是另一种基于图的模型，它根据句子的边相对于文档中其他句子的权重之和来对句子进行估值。该权重的计算方式与在 TexRank 模型中相同。

```
Mrs. Potter was Mrs. Dursley's sister, but they hadn't met for several years; in fact, Mrs. Dursley pretended she didn't have a sister, because her sister and her good-for-nothing husband were as unDursleyish as it was possible to be.
It seemed that Professor McGonagall had reached the point she was most anxious to discuss, the real reason she had been waiting on a cold, hard wall all day, for neither as a cat nor as a woman had she fixed Dumbledore with such a piercing stare as she did now.
Dumbledore took Harry in his arms and turned toward the Dursleys' house.
"But I c-c-can't stand it — Lily an' James dead — an' poor little Harry off ter live with Muggles —" "Yes, yes, it's all very sad, but get a grip on yourself, Hagrid, or we'll be found," Professor McGonagall whispered, patting Hagrid gingerly on the arm as Dumbledore stepped over the low garden wall and walked to the front door.
G'night, Professor McGonagall — Professor Dumbledore, sir."
```

## Gensim 摘要生成器

Python 的 [Gensim 包包含一个概要生成器](https://radimrehurek.com/gensim/summarization/summariser.html)，它是 TextRank 算法的[修改。Gensim 的方法修改了句子相似度函数。](https://arxiv.org/abs/1602.03606)

```
It was now reading the sign that said Privet Drive — no, looking at the sign; cats couldn't read maps or signs.
Dumbledore slipped the Put-Outer back inside his cloak and set off down the street toward number four, where he sat down on the wall next to the cat.
They're a kind of Muggle sweet I'm rather fond of." "No, thank you," said Professor McGonagall coldly, as though she didn't think this was the moment for lemon drops.
All this 'You-Know-Who' nonsense — for eleven years I have been trying to persuade people to call him by his proper name: Voldemort." Professor McGonagall flinched, but Dumbledore, who was unsticking two lemon drops, seemed not to notice.
Professor McGonagall shot a sharp look at Dumbledore and said, "The owls are nothing next to the rumors that are flying around.
It seemed that Professor McGonagall had reached the point she was most anxious to discuss, the real reason she had been waiting on a cold, hard wall all day, for neither as a cat nor as a woman had she fixed Dumbledore with such a piercing stare as she did now.
```

那么你认为哪种算法提供了最好的总结呢？在做出最终判断之前，有许多参数需要调整。一些可能在较短的摘要上表现更好，一些在较长的摘要上表现更好。写作风格可能会有所不同(我之前提到过，我总结非小说的运气比小说好)。但是如果你是哈利，带着回答某个问题的意图走进霍格沃茨的图书馆，却被“图书馆的庞大”所吓倒；几万本书；千架；数百个窄行”，那么只需点击几下鼠标，就可以得到一些摘要，这不会有什么坏处。要是巫师们使用这种愚蠢的麻瓜作品就好了…