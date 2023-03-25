# 手套+有趣-无聊

> 原文：<https://towardsdatascience.com/glove-fun-boring-664fe0717c4c?source=collection_archive---------3----------------------->

GloVe，源自[**Glo**bal**Ve**ctors](https://nlp.stanford.edu/projects/glove/)的首字母缩写，是一种将单词表示为向量的无监督学习方法。它对语料库的单词共现统计进行训练，目标函数是专门选择来编码单词向量之间有意义的差异。一个比较著名的例子是，“国王”+“女人”-“男人”的结果与“女王”的向量最为相似。学习者设法量化皇室和性别的概念，以便它们可以被分解和重新组合，从而得到与 king 类似的单词。

![](img/5df0cd9afb4bcd1c17e9af8f8586dc9a.png)

我想看看这些单词向量是否有助于创建一个更智能的词库。如今，如果你在搜索一个和另一个单词“x”意思相似的单词，你可以用谷歌搜索“x 的同义词”。更有效且有助于缩小搜索空间的方法是给出所需同义词的特定方向。例如，如果你在寻找一个表示“缺乏经验”但没有负面含义的词，你可能会更喜欢“新鲜的”或“新的”而不是“无能的”。为此，我[下载了](https://nlp.stanford.edu/projects/glove/)词向量，这些词向量在由 2014 年维基百科文章和 Gigaword 5 组成的数据集上进行了预训练。我发现单词与其他向量相加和相减得到的向量最相似(由余弦相似度定义)。以下是一些沿好坏轴的类比。如表所示，“方案”+“好”--“坏”≘“计划”，反其道而行之，“方案”--“好”+“坏”≘“骗局”，沿着好的光谱正确捕捉语义。

```
 **   + good - bad      - good + bad**
scheme                plan              scam
intelligence     knowledge               spy
naive           idealistic            stupid
possibility      potential      consequences
dream                 wish         nightmare
notion             concept        stereotype
bold            courageous             risky 
```

有趣无聊轴上的一些类比很有趣。

```
 **+ fun - boring    - fun + boring**
presentation          showcase        powerpoint
woman                     girl         housewife
acquaintance            friend          coworker
music                      pop        orchestral
test                 challenge              exam
premiere              festival             opera
```

这里有一些优雅-笨拙轴心的例子。

```
 **+ elegant - clumsy    - elegant + clumsy**
persuade               convince            pressuring
walked                 strolled              stumbled
threw                    tossed                hurled
placed                  adorned            mishandled
stylish                   sleek            uninspired
```

根据 Richard Socher 的[讲座](https://youtu.be/ASn7ExxLZws?t=57m10s)，没有数学证明可以保证这些类比脱离模型。实际上，有时类比似乎是正确的，但很多时候却不是。一个更智能的词库已经初露端倪，但它可能需要一种未来的方法来更好地捕捉语义关系。