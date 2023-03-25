# 用 Python 实现模糊字符串匹配的自然语言处理

> 原文：<https://towardsdatascience.com/natural-language-processing-for-fuzzy-string-matching-with-python-6632b7824c49?source=collection_archive---------2----------------------->

![](img/0a24b75ff025b0051aecabadf522bf37.png)

photo credit: Pexels

## 当我们比较不同网站的酒店房价时，我们必须确保我们是在进行比较

[在计算机科学中，模糊字符串匹配是一种寻找与模式近似(而不是精确)匹配的字符串的技术](https://en.wikipedia.org/wiki/Approximate_string_matching)。换句话说，模糊字符串匹配是一种即使用户拼错单词或只输入部分单词也能找到匹配项的搜索类型。也被称为[近似串匹配](https://en.wikipedia.org/wiki/Approximate_string_matching)。

模糊字符串搜索可用于各种应用，例如:

*   拼写检查和拼写错误，错别字校正器。例如，用户在谷歌中输入[“密西沙加”，会返回一个点击列表以及“显示密西沙加的结果”。也就是说，即使用户输入包含额外的或缺失的字符，或其他类型的拼写错误，搜索查询也将返回结果。](https://www.google.ca/search?ei=rc64W6rHIOeUjwT3z6-4CA&q=missisaga&oq=missisaga&gs_l=psy-ab.3..0i10k1l10.30325.30325.0.30947.1.1.0.0.0.0.75.75.1.1.0....0...1c.1.64.psy-ab..0.1.74....0.gGpQVAid93s)
*   一个软件可以用来检查重复的记录。例如，如果由于客户姓名的不同拼法(例如 Abigail Martin vs . Abigail Martinez)而在数据库中多次列出不同的购买记录，则可能是新地址或错误输入的电话号码。

说到重复数据删除，它可能不像听起来那么简单，尤其是当您有成千上万条记录时。甚至 Expedia 也没有做到 100%正确:

![](img/6dad29d3b9748a263bef6395daf34d4f.png)

Source: Expedia

这篇文章将解释什么是模糊字符串匹配及其用例，并给出使用 Python 的 [Fuzzywuzzy 库](https://github.com/seatgeek/fuzzywuzzy)的例子。

每家酒店都有自己的房间命名法，在线旅行社也是如此。例如，同一家酒店的一个房间，Expedia 称之为“工作室，一张特大床和沙发床，角落”，Booking.com 可能会发现简单地将房间显示为“角落特大工作室”是安全的。

这没有错，但当我们想要比较在线旅行社之间的房价时，或者一家在线旅行社想要确保另一家在线旅行社遵守[价格平价](https://www.siteminder.com/what-is-hotel-rate-parity/)协议时，这可能会导致混乱。换句话说，为了能够比较价格，我们必须确保我们是在比较苹果与苹果。

对于价格比较网站和应用程序来说，最令人沮丧的问题之一是试图自动判断两件商品(或酒店房间)是否是同一件东西。

# Python 中的 FuzzyWuzzy

[Fuzzywuzzy](https://github.com/seatgeek/fuzzywuzzy) 是一个 Python 库，使用 [Levenshtein 距离](https://en.wikipedia.org/wiki/Levenshtein_distance)来计算一个简单易用的包中序列之间的差异。

为了演示，我创建了自己的[数据集](https://github.com/susanli2016/NLP-with-Python/blob/master/room_type.csv)，也就是说，对于同一个酒店，我从 Expedia 获取一个房间类型，比如“套房，一张特大床(客厅)”，然后我将其与 Booking.com 的一个房间类型“特大客厅套房”进行匹配。只要有一点点经验，大多数人就会知道它们是一样的。按照这种方法，我创建了一个小型数据集，包含 100 多个房间类型对，可以在 [Github](https://github.com/susanli2016/NLP-with-Python/blob/master/room_type.csv) 上找到。

利用这个数据集，我们将测试 Fuzzywuzzy 是如何思考的。换句话说，我们使用 Fuzzywuzzy 来匹配两个数据源之间的记录。

```
import pandas as pddf = pd.read_csv('room_type.csv')
df.head(10)
```

![](img/536e705984c5f95242cb2cebe9769e51.png)

Figure 1

数据集是我自己创建的，所以，很干净。

Fuzzywuzzy 中比较两个字符串有几种方法，我们一个一个来试试。

*   `ratio`，比较整个字符串的相似度，按顺序排列。

```
from fuzzywuzzy import fuzz
fuzz.ratio('Deluxe Room, 1 King Bed', 'Deluxe King Room')
```

***62***

这告诉我们“豪华客房，一张特大床”和“豪华大床房”大约有 62%是相同的。

```
fuzz.ratio('Traditional Double Room, 2 Double Beds', 'Double Room with Two Double Beds')
```

***69***

“传统双人房，两张双人床”和“双人房，两张双人床”大约有 69%相同。

```
fuzz.ratio('Room, 2 Double Beds (19th to 25th Floors)', 'Two Double Beds - Location Room (19th to 25th Floors)')
```

***74***

“房间，两张双人床(19 至 25 层)”和“两张双人床-位置房间(19 至 25 层)”对大约 74%相同。

我对这些感到失望。事实证明，这种天真的方法对词序的微小差异、遗漏或多余的单词以及其他类似问题过于敏感。

*   `partial_ratio`，比较部分字符串相似度。

我们仍然使用相同的数据对。

```
fuzz.partial_ratio('Deluxe Room, 1 King Bed', 'Deluxe King Room')
```

***69***

```
fuzz.partial_ratio('Traditional Double Room, 2 Double Beds', 'Double Room with Two Double Beds')
```

***83***

```
fuzz.partial_ratio('Room, 2 Double Beds (19th to 25th Floors)', 'Two Double Beds - Location Room (19th to 25th Floors)')
```

***63***

对于我的数据集，比较部分字符串并不能带来整体更好的结果。我们继续。

*   `token_sort_ratio`，忽略词序。

```
fuzz.token_sort_ratio('Deluxe Room, 1 King Bed', 'Deluxe King Room')
```

***84***

```
fuzz.token_sort_ratio('Traditional Double Room, 2 Double Beds', 'Double Room with Two Double Beds')
```

***78***

```
fuzz.token_sort_ratio('Room, 2 Double Beds (19th to 25th Floors)', 'Two Double Beds - Location Room (19th to 25th Floors)')
```

***83***

目前为止最好的。

*   `token_set_ratio`，忽略重复单词。它类似于令牌排序比率，但更灵活一点。

```
fuzz.token_set_ratio('Deluxe Room, 1 King Bed', 'Deluxe King Room')
```

***100***

```
fuzz.token_set_ratio('Traditional Double Room, 2 Double Beds', 'Double Room with Two Double Beds')
```

**78**

```
fuzz.token_set_ratio('Room, 2 Double Beds (19th to 25th Floors)', 'Two Double Beds - Location Room (19th to 25th Floors)')
```

***97***

看起来`token_set_ratio` 最适合我的数据。根据这个发现，我决定将`token_set_ratio`应用于我的整个数据集。

```
def get_ratio(row):
    name = row['Expedia']
    name1 = row['Booking.com']
    return fuzz.token_set_ratio(name, name1)
len(df[df.apply(get_ratio, axis=1) > 70]) / len(df)
```

***0.9029126213592233***

当设置比率> 70 时，超过 90%的配对超过 70 的匹配分数。没那么寒酸！

[Jupyter 笔记本](https://github.com/susanli2016/NLP-with-Python/blob/master/Fuzzy%20String%20Matching.ipynb)可以在 [Github](https://github.com/susanli2016/NLP-with-Python/blob/master/Fuzzy%20String%20Matching.ipynb) 上找到。星期五快乐！