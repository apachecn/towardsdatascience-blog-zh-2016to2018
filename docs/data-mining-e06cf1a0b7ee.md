# 数据挖掘

> 原文：<https://towardsdatascience.com/data-mining-e06cf1a0b7ee?source=collection_archive---------3----------------------->

## 2.熊猫数据结构和基本操作介绍

![](img/de412a2a6751d6f43a931e6fd99942f1.png)

Pandas Logo

我已经在我的 [**上一篇文章**](https://medium.com/towards-data-science/data-mining-bc7feca95887) 中讨论了如何设置工具和数据挖掘的基本介绍。Pandas 基本上是作为一个库来管理数据结构的。该库还提供了一些对数据进行预处理和清理的重要操作。

## 将熊猫导入工作区

`import pandas as pd`导入为 **pd** 使其更简单，手更短。

> 导入特定功能
> `from pandas import Series, DataFrame`
> 我们将主要使用这两种数据结构

# 熊猫数据结构

## 系列

数组形式的数据结构，让标签从 **0** 开始，一直递增。举个例子。

```
In [1]: from pandas import Series
In [2]: obj = Series([1, 2, 3, 4, 5, 6])
In [3]: obj
Out[3]:
0    1
1    2
2    3
3    4
4    5
5    6
dtype: int64
```

您也可以使用自定义标签。

```
In [1]: from pandas import DataFrame, Series
In [2]: obj = Series([4, 7, -5, 3], index=['d', 'b', 'a', 'c'])
In [3]: obj
Out[4]:
d    4
b    7
a   -5
c    3
dtype: int64
```

> 一些有用的特性
> *将字典转换为序列，`s = Series({'age':23, 'name': 'anuradha'})`，
> *直接分配索引`obj.index = ['attr 1', '*attr 2*', '*attr 3*']`，
> *索引可以有属性名`obj.name = 'Population'`

## 数据帧

这些是表格数据的数据结构，非常类似于电子表格，可以包含大量数据。

```
In [1]: data = {'state': ['Ohio', 'Ohio', 'Ohio', 'Nevada', 'Nevada'], 'year': [2000, 2001, 2002, 2001, 2002], 'pop': [1.5, 1.7, 3.6, 2.4, 2.9]}In [2]: data
Out[2]:
{'pop': [1.5, 1.7, 3.6, 2.4, 2.9],
'state': ['Ohio', 'Ohio', 'Ohio', 'Nevada', 'Nevada'],
'year': [2000, 2001, 2002, 2001, 2002]}In [3]: frame = DataFrame(data)
In [4]: frame
Out[4]:
pop   state  year
0  1.5    Ohio  2000
1  1.7    Ohio  2001
2  3.6    Ohio  2002
3  2.4  Nevada  2001
4  2.9  Nevada  2002
```

在不深入研究上述数据结构的情况下，让我们继续研究一些功能，并在以后探索更深的领域。:)

# 过滤

对于这个例子，让我们考虑下面这段代码。

```
In [1]: from pandas import DataFrame, Series
In [2]: import numpy as npIn [3]: data = DataFrame(np.arange(16).reshape(4,4), index=['Ohio', 'Colorado', 'Utah', 'New York'], columns=['one', 'two', 'three', 'four'])
In [4]: data
Out[4]: one  two  three  fourOhio        0    1      2     3
Colorado    4    5      6     7
Utah        8    9     10    11
New York   12   13     14    15
```

我们使用`np.arange(16)`创建一个从 0 到 15 的元素数组，并使用`reshape(4, 4)`使其成为一个 4x4 的矩阵。只是一组虚拟数据。

## 布尔比较

```
In [6]: data > 5Out[6]: one    two  three   fourOhio      False  False  False  False
Colorado  False  False   True   True
Utah       True   True   True   True
New York   True   True   True   True
```

## 代替

我们提供了一个查询函数`data < 5`和一个替换操作`= -1`来用-1 替换小于 5 的值。

```
In [7]: data[data < 5] = -1
In [8]: data
Out[8]: one  two  three  four
Ohio       -1   -1     -1    -1
Colorado   -1    5      6     7
Utah        8    9     10    11
New York   12   13     14    15
```

# 真实场景中的应用

熊猫的一个直接应用是处理缺失值。大多数时候我们进行数据挖掘得到的数据并不是干净纯粹的数据。它们包含不一致数字的缺失数字。因此，我们需要把它们过滤掉，或者用一个替代的数字来代替。

## 过滤掉丢失的值

创建带有缺失值(NaN 值或非数字值)的样本数组。

```
In [9]: a = np.array([1,2,3,np.nan,4])
In [10]: a
Out[10]: array([  1.,   2.,   3.,  nan,   4.])
In [11]: df = DataFrame(a)
In [12]: df
Out[12]: 00  1.0
1  2.0
2  3.0
3  NaN
4  4.0
```

一旦我们有了缺失值/nan 的数据框，我们可以使用以下方式填充它们。这里我用-1 填充它。

```
In [13]: df.fillna(-1)
Out[13]: 00  1.0
1  2.0
2  3.0
3 -1.0
4  4.0
```

我们可以在不同的柱子上使用不同的填充物。例如，第一列为 0.5，第三列为-1。注意:列编号从 0 开始。

```
df.fillna({1: 0.5, 3: -1})
```

可以用更聪明的方式比如填充使用 mean，其中 ***也许*** 在某些情况下更有意义。

```
data = Series([1., NA, 3.5, NA, 7])
data.fillna(data.mean())
```

> 这是对熊猫及其常用功能的简单介绍。随着我们继续前进，我们会发现更多。感谢阅读。