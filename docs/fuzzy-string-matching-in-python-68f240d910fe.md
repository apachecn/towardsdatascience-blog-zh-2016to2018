# 模糊字符串匹配

> 原文：<https://towardsdatascience.com/fuzzy-string-matching-in-python-68f240d910fe?source=collection_archive---------2----------------------->

![](img/aee8381919d69250c57cf37ba80080e3.png)

Photo by [Romain Vignes](https://unsplash.com/@rvignes?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) on [Unsplash](https://unsplash.com/s/photos/text?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

## 使用 Python 查找与数据中的模式近似匹配的字符串。

# Python 中 fuzzywuzzy 的介绍

作为一名数据科学家，您不得不从各种来源检索信息，要么利用公开可用的 API，要求数据，要么只是从网页上抓取您自己的数据。如果我们能够将这些信息组合起来，并且在数据中没有任何重复，那么所有这些信息都是有用的。但是我们如何确保没有重复呢？

我知道… *“咄！您可以使用一个函数来检索所有唯一的信息，从而删除重复的“*”。嗯，这是一种方法，但是我们的函数可能无法分辨出像*“巴拉克·奥巴马”*这样的名字与*“巴拉克·h·奥巴马”*是同一个名字，对吗？(假设我们正在检索世界上最著名的人的名字)。我们可以清楚地看出这些名字是不同的，但它们可能指的是同一个人。那么，我们如何匹配这些名字呢？

这就是模糊字符串匹配的用武之地。这篇文章将解释什么是模糊字符串匹配及其用例，并给出使用 Python 库 [*Fuzzywuzzy*](https://pypi.python.org/pypi/fuzzywuzzy) 的例子。

## 模糊逻辑

> ***模糊(*形容词 *)*** *:难以感知；模糊或不清楚*
> 
> *-维基百科*

模糊逻辑是多值逻辑的一种形式，它处理近似的推理，而不是固定和精确的推理。模糊逻辑值的范围在 1 和 0 之间。即该值的范围可以从完全真到完全假。相比之下， ***布尔逻辑*** 是一种二值逻辑:真或假通常分别表示为 1 和 0，它处理的是固定和精确的推理。模糊逻辑倾向于反映人们如何思考，并试图模拟我们的决策，因此它现在正导致新的智能系统(专家系统)。

因此，如果我们使用模糊逻辑比较两个字符串，我们将试图回答问题*“字符串 A 和字符串 B 有多相似？”、*并重新表述为*“字符串 A 和字符串 B 是一样的吗？”*使用布尔逻辑时。

## 模糊字符串匹配

[模糊字符串匹配](https://en.wikipedia.org/wiki/Approximate_string_matching)，也称为近似字符串匹配，是寻找近似匹配一个模式的字符串的过程。该过程具有各种应用，例如*拼写检查*、 *DNA 分析和检测、*垃圾邮件检测、*剽窃检测等等*

**Python 中的 *Fuzzywuzzy* 简介**

**Fuzzywuzzy** 是一个 python 库，它使用 **Levenshtein Distance** 来计算序列和模式之间的差异，该库由 [SeatGeek 开发并开源，](https://seatgeek.com/)seat geek 是一种从互联网上寻找活动门票并在一个平台上展示它们的服务。他们面临的一个大问题是，他们的博客[上所描述的相同事件的标签。这和我在文章开头给出的例子是一样的，一个实体，比如一个人的名字，在不同的来源上可以有不同的标签。](http://chairnerd.seatgeek.com/fuzzywuzzy-fuzzy-string-matching-in-python/)

**安装**

要安装库，您可以使用 pip:

```
pip install fuzzywuzzypip install python-Levenshtein
```

**例题**

首先，我们必须导入模糊的模块:

```
from fuzzywuzzy import fuzz
from fuzzywuzzy import process
```

现在，我们可以通过使用以下方法获得两个字符串的相似性得分两个方法 ratio()或 partial_ratio():

```
fuzz.ratio("Catherine M Gitau","Catherine Gitau")#91fuzz.partial_ratio("Catherine M. Gitau","Catherine Gitau")#100
```

你可能想知道为什么分数不一样。这是因为 fuzz.ratio()方法只是使用`difflib.ratio.`来计算两个输入字符串中标记的某种排序之间的编辑距离。***fuzz . partial _ ratio()***接受最短的字符串，在本例中是“Catherine Gitau”(长度 14)，然后将其与“Catherine M. Gitau”中所有长度为(14)的子字符串进行匹配，这意味着与给出 100%的“Catherine Gitau”进行匹配。你可以随意摆弄琴弦，直到掌握要领。

如果我们在一个字符串中交换两个名字会怎么样？在下面的例子中，我把“Catherine Gitau”这个名字换成了“Gitau Catherine”。让我们看看分数:

```
fuzz.ratio("Catherine M Gitau","Gitau Catherine")#55fuzz.partial_ratio("Catherine M. Gitau","Gitau Catherine")#60
```

我们看到这两种方法给出的分数都很低，这可以通过使用***token _ sort _ ratio()***方法来纠正。这种方法试图说明无序的相似字符串。例如，如果我们再次使用上述字符串，但使用***token _ sort _ ratio()***，我们会得到以下结果:

```
fuzz.token_sort_ratio("Catherine Gitau M.", "Gitau Catherine")#94
```

如你所见，我们得到了 94 分的高分。

## 结论

本文介绍了模糊字符串匹配，这是一个众所周知的问题，是建立在 Leivenshtein 距离。从我们所看到的，它计算两个字符串有多相似。这也可以通过找出将一个字符串转换为另一个字符串所需的运算次数来计算。例如，对于名称“巴拉克”，可以将其拼写为“巴拉奇”。只需要一个操作来纠正这一点，即在末尾添加一个 K。您可以使用 **R** 中的 *stringdist* 库来尝试一下:

```
adist("Barack", "Barac")
#[1]
```

## 来源

[https://marcobonzanini . com/2015/02/25/fuzzy-string-matching-in-python/](https://marcobonzanini.com/2015/02/25/fuzzy-string-matching-in-python/)

直到下次:)