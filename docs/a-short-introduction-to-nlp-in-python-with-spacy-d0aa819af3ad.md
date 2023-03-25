# Python 中的自然语言处理简介

> 原文：<https://towardsdatascience.com/a-short-introduction-to-nlp-in-python-with-spacy-d0aa819af3ad?source=collection_archive---------2----------------------->

![](img/03ea1c95a000bece93ce552667c38269.png)

自然语言处理(NLP)是数据科学最有趣的子领域之一，人们越来越期望数据科学家能够迅速找到涉及利用非结构化文本数据的解决方案。尽管如此，许多应用数据科学家(STEM 和社会科学背景)缺乏 NLP 经验。

在这篇文章中，我将探索一些基本的 NLP 概念，并展示如何使用 Python 中越来越流行的 [spaCy](https://spacy.io/) 包来实现它们。这篇文章是为绝对的 NLP 初学者写的，但是需要具备 Python 知识。

你说是 spaCy？

spaCy 是 Matt Honnibal 在 [Explosion AI 开发的“Python 中的工业强度 NLP”的一个相对较新的包。](https://explosion.ai/)它的设计考虑到了应用数据科学家，这意味着它不会让用户在决定使用什么深奥的算法来执行常见任务时感到沉重，而且它的速度很快。非常快(在 Cython 中实现)。如果你熟悉 Python 数据科学栈，spaCy 就是你的 NLP 的`numpy`——它相当低级，但是非常直观和高性能。

**那么，它能做什么呢？**

spacy 为任何 NLP 项目中常用的任务提供一站式服务，包括:

*   符号化
*   引理满足
*   词性标注
*   实体识别
*   依存句法分析
*   句子识别
*   单词到向量的转换
*   许多方便的方法来清理和规范文本

我将提供其中一些特性的高级概述，并展示如何使用 spaCy 访问它们。

**我们开始吧！**

首先，我们加载 spaCy 的管道，按照惯例，它存储在一个名为`nlp`的变量中。声明这个变量需要几秒钟的时间，因为 spaCy 会预先加载它的模型和数据，以便以后节省时间。实际上，这很早就完成了一些繁重的工作，这样就不会在每次对数据应用`nlp`解析器时产生成本。请注意，这里我使用的是英语语言模型，但也有一个全功能的德语模型，跨几种语言实现了标记化(下面讨论)。

我们对样本文本调用 nlp 来创建一个`Doc`对象。`Doc`对象现在是文本本身、文本片段(`Span`对象)和文本元素(`Token`对象)的 NLP 任务的容器。值得注意的是，`Token`和`Span`对象实际上没有数据。相反，它们包含指向包含在`Doc`对象中的数据的指针，并且被延迟评估(即，根据请求)。spaCy 的许多核心功能都是通过对`Doc` (n=33)、`Span` (n=29)和`Token` (n=78)对象的方法来访问的。

```
In[1]: import spacy 
...: nlp = spacy.load("en") 
...: doc = nlp("The big grey dog ate all of the chocolate, but fortunately he wasn't sick!")
```

**标记化**

标记化是许多 NLP 任务中的基础步骤。将文本标记化是将一段文本拆分成单词、符号、标点、空格和其他元素，从而创建“标记”的过程。一种简单的方法是在空格处拆分字符串:

```
In[2]: doc.text.split() 
...: Out[2]: ['The', 'big', 'grey', 'dog', 'ate', 'all', 'of', 'the', 'chocolate,', 'but', 'fortunately', 'he', "wasn't", 'sick!']
```

表面上，这看起来很好。但是，注意`a)`它忽略了标点符号，`b)`它没有拆分动词和副词(“was”、“n t”)。换句话说，它是幼稚的，它无法识别文本中帮助我们(和机器)理解其结构和意义的元素。让我们看看 SpaCy 是如何处理的:

```
In[3]: [token.orth_ for token in doc] 
...: Out[3]: ['The', 'big', 'grey', 'dog', 'ate', 'all', 'of', 'the', 'chocolate', ',', 'but', 'fortunately', 'he', 'was', "n't", ' ', 'sick', '!']
```

这里我们访问 each token 的`.orth_`方法，该方法返回 token 的字符串表示而不是 SpaCy token 对象，这可能并不总是可取的，但值得注意。SpaCy 识别标点符号，并能够将这些标点符号从单词符号中分离出来。SpaCy 许多标记方法提供了处理文本的字符串和整数表示——带下划线后缀的方法返回字符串，不带下划线后缀的方法返回整数。例如:

```
In[4]: [(token, token.orth_, token.orth) for token in doc] 
...: Out[4]: [
(The, 'The', 517), 
(big, 'big', 742), 
(grey, 'grey', 4623), 
(dog, 'dog', 1175), 
(ate, 'ate', 3469), 
(all, 'all', 516), 
(of, 'of', 471), 
(the, 'the', 466), 
(chocolate, 'chocolate', 3593), 
(,, ',', 416), 
(but, 'but', 494), 
(fortunately, 'fortunately', 15520),
 (he, 'he', 514),
 (was, 'was', 491),
 (n't, "n't", 479),
 ( , ' ', 483), 
(sick, 'sick', 1698), 
(!, '!', 495)]
```

这里，我们在一个元组列表中返回 SpaCy 令牌、令牌的字符串表示和令牌的整数表示。

如果您希望避免返回标点符号或空白符号，SpaCy 为此提供了便利方法(以及许多其他常见的文本清理任务)—例如，要删除停用词，您可以调用`.is_stop`方法。

```
In[5]: [token.orth_ for token in doc if not token.is_punct | token.is_space] 
...: Out[5]: ['The', 'big', 'grey', 'dog', 'ate', 'all', 'of', 'the', 'chocolate', 'but', 'fortunately', 'he', 'was', "n't", 'sick']
```

很酷，对吧？

**词汇化**

与记号化相关的一个任务是引理满足。引理满足是将一个词简化为其基本形式的过程，如果你愿意，可以称之为其母词。一个词的不同用法往往有相同的词根意义。例如，`practice, practised and practising`本质上都是指同一个东西。人们常常希望将词义与其基本形式相似的词标准化。通过 SpaCy，我们可以用一个令牌的`.lemma_`方法访问每个单词的基本形式:

```
In[6]: practice = "practice practiced practicing" 
...: nlp_practice = nlp(practice) 
...: [word.lemma_ for word in nlp_practice] 
...: Out[6]: ['practice', 'practice', 'practice']
```

这为什么有用？一个直接的用例是机器学习，特别是文本分类。例如，在创建“单词包”之前对文本进行词条匹配可以避免单词重复，因此，允许模型建立跨多个文档的单词使用模式的更清晰的图像。

**位置标记**

词性标注是分配语法属性(如名词、动词、副词、形容词等)的过程。)到话。共享相同 POS 标签的单词倾向于遵循相似的句法结构，并且在基于规则的过程中是有用的。

例如，在一个事件的给定描述中，我们可能希望确定谁拥有什么。通过利用所有格，我们可以做到这一点(前提是文本语法正确！).SpaCy 使用了流行的 Penn Treebank POS 标签，参见[https://www . ling . upenn . edu/courses/Fall _ 2003/ling 001/Penn _ tree bank _ POS . html](https://www.ling.upenn.edu/courses/Fall_2003/ling001/penn_treebank_pos.html)。使用 SpaCy，您可以分别使用`.pos_`和`.tag_`方法访问粗粒度和细粒度的 POS 标签。在这里，我访问细粒度的 POS 标记:

```
In[7]: doc2 = nlp("Conor's dog's toy was hidden under the man's sofa in the woman's house") 
...: 
pos_tags = [(i, i.tag_) for i in doc2] ...: 
pos_tags 
...: Out[7]: [(Conor, 'NNP'), ('s, 'POS'), (dog, 'NN'), ('s, 'POS'), (toy, 'NN'), (was, 'VBD'), (hidden, 'VBN'), (under, 'IN'), (the, 'DT'), (man, 'NN'), ('s, 'POS'), (sofa, 'NN'), (in, 'IN'), (the, 'DT'), (woman, 'NN'), ('s, 'POS'), (house, 'NN')]
```

我们可以看到“`’s`令牌被标记为`POS`。我们可以利用这个标签来提取所有者和他们所拥有的东西:

```
In[8]: owners_possessions = [] 
...: for i in pos_tags: 
    ...: if i[1] == "POS": ...: owner = i[0].nbor(-1) 
...:         possession = i[0].nbor(1) 
...:         owners_possessions.append((owner, possession)) ...: ...:         owners_possessions 
...: Out[8]: [(Conor, dog), (dog, toy), (man, sofa), (woman, house)]
```

这将返回所有者拥有元组的列表。如果你想在这方面表现得更好，你可以在一个清单 comprehenion 中这样做(我认为这样更好！):

```
In[9]: [(i[0].nbor(-1), i[0].nbor(+1)) for i in pos_tags if i[1] == "POS"] 
...: Out[9]: [(Conor, dog), (dog, toy), (man, sofa), (woman, house)]
```

这里我们使用每个令牌的`.nbor`方法，它返回一个令牌的相邻令牌。

**实体识别**

实体识别是将在文本中发现的命名实体分类到预定义的类别中的过程，例如人、地点、组织、日期等。spaCy 使用一种统计模型来对广泛的实体进行分类，包括人、事件、艺术品和国籍/宗教(完整列表请参见文档[https://spaCy . io/docs/usage/entity-recognition)。](https://spacy.io/docs/usage/entity-recognition).)

例如，让我们从巴拉克·奥巴马的维基百科条目中选取前两句话。我们将解析这个文本，然后使用`Doc`对象的`.ents`方法访问标识的实体。通过在`Doc`上调用这个方法，我们可以访问额外的`Token`方法，特别是`.label_`和`.label`:

```
In[10]: wiki_obama = """Barack Obama is an American politician who served as ...: the 44th President of the United States from 2009 to 2017\. He is the first ...: African American to have served as president, ...: as well as the first born outside the contiguous United States.""" 
...: 
...: nlp_obama = nlp(wiki_obama) ...: [(i, i.label_, i.label) for i in nlp_obama.ents] 
...: Out[10]: [(Barack Obama, 'PERSON', 346), (American, 'NORP', 347), (the United States, 'GPE', 350), (2009 to 2017, 'DATE', 356), (first, 'ORDINAL', 361), (African, 'NORP', 347), (American, 'NORP', 347), (first, 'ORDINAL', 361), (United States, 'GPE', 350)]
```

您可以看到模型已经识别的实体，以及它们有多准确(在本例中)。人是不言自明的，NORP 是国家或宗教团体，GPE 识别地点(城市，国家等)。)，DATE 表示特定的日期或日期范围，ORDINAL 表示代表某种顺序的单词或数字。

在我们讨论`Doc`方法的时候，有必要提一下 spaCy 的句子标识符。在 NLP 任务中，想要将文档分割成句子并不罕见。通过访问一个`Doc's` `.sents`方法，使用 SpaCy 很容易做到这一点:

```
In[11]: for ix, sent in enumerate(nlp_obama.sents, 1): 
...: print("Sentence number {}: {}".format(ix, sent)) 
...: 
Sentence number 1: Barack Obama is an American politician who served as the 44th President of the United States from 2009 to 2017\. Sentence number 2: He is the first African American to have served as president, as well as the first born outside the contiguous United States.
```

*原载于 2017 年 3 月 17 日 dataflume.wordpress.com*[](https://dataflume.wordpress.com/2017/03/17/intro-nlp-python-spacy/)**。**