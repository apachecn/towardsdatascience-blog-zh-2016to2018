# 数据科学简介

> 原文：<https://towardsdatascience.com/intro-to-data-science-part-1-numpy-and-pandas-49d98740661b?source=collection_archive---------7----------------------->

# 第一部分:熊猫和熊猫

*   [第二部分:数据争论](https://medium.com/@tiffanysouterre/intro-to-data-science-part-2-data-wrangling-75835b9129b4)
*   [第三部分:数据分析](/intro-to-data-science-part-3-data-analysis-71a566c3a8c3)

![](img/1107e341aecea5807e4146fe9665741d.png)

Photo by geralt on Pixabay

作为一名科学家，数据在我的日常生活中无处不在。但是分析数据的方法本身就是一门艺术。在“数据科学导论”中，我综合了来自 [Udacity](https://eu.udacity.com/course/intro-to-data-science--ud359) 的一门课程，在那里你会发现一个有抱负的数据科学家应该知道的所有基础知识。

在第 1 部分中，我们将看到如何使用 Numpy 创建和操作**数组**，以及使用 Pandas 创建和操作**系列**和**数据帧**。最后，您将能够:

*   创建、索引、切片、操作 numpy 数组
*   创建一个带有 2D 数字阵列的矩阵
*   对 numpy 数组应用算法
*   对 numpy 数组应用数学函数(平均值和点积)
*   创建、索引、切片、操作熊猫系列
*   创建熊猫数据框
*   通过切片、单独索引(iloc 或 loc)、布尔索引选择数据框行

# 数据科学中常用的工具:Numpy 和 Pandas

# Numpy

Numpy 因增加了对多维数组和矩阵的支持而广受欢迎。它还增加了许多基础 python 所缺乏的数学函数。这些函数对于统计分析非常有用(一个数组的平均值、中值、标准差……)。array object 类是 Numpy 的基础，Numpy 数组就像 Python 中的列表，只是数组中的所有内容都必须是同一类型，比如 int 或 float。

```
>>> [1\. 4\. 5\. 8.]  
>>>[[1\. 2\. 3.]
    [4\. 5\. 6.]]
```

可以对 Numpy 数组进行索引、切片和操作，就像处理 Python 列表一样。

```
>>> [1\. 4\. 5\. 8.]  
>>> 4.0
>>> [1\. 4.] 
>>> 5.0
```

矩阵索引和切片的作用。

```
>>> [[1\. 2\. 3.]  
     [4\. 5\. 6.]]  
>>> 5.0  
>>> [4\. 5\. 6.]  
>>> [3\. 6.]
```

下面是一些可以用 Numpy 数组做的算术运算。

```
>>> [6\. 4\. 9.]  
>>> [-4\.  0\. -3.]  
>>> [ 5\.  4\. 18.]
```

同样的算术运算，但这次用的是矩阵。

```
>>> [[ 6\.  8.]  [10\. 12.]]  
>>> [[-4\. -4.]  [-4\. -4.]]  
>>> [[ 5\. 12.]  [21\. 32.]]
```

除了标准的算术运算，Numpy 还有一系列的
其他数学运算可以应用于 Numpy 数组，比如
均值和点积。

```
>>> 2.0 
>>> 7.0  
>>> [44.]
```

# 熊猫

另一方面，Pandas 允许以适合数据分析的方式构建和操作我们的数据。Pandas 还从 R 中吸取了很多最好的元素，并用 Python 实现了它们。我们将关注熊猫、系列和数据框的主要概念。

您可以将序列视为一维对象，类似于数据库中的数组、列表或列。默认情况下，它将为系列中的每个项目分配一个从 0 到 N 的
索引标签，其中 N 是
系列中项目的数量减 1。

```
0           Dave 
1      Cheng-Han 
2        Udacity 
3             42 
4    -1789710578
```

当
创建系列时，您也可以手动为系列中的项目分配索引，并使用索引从系列中选择特定项目。

```
>>>
Instructor               Dave 
Curriculum Manager    Cheng-Han 
Course Number               359 
Power Level                9001>>> Dave>>>
Instructor                 Dave 
Curriculum Manager    Cheng-Han 
Course Number               359
```

您也可以使用布尔运算符从系列中选择特定的项目

```
>>>
Cockroach    1 
Fish         2 
Mini Pig     3 
Puppy        4 
Kitten       5 >>>
Cockroach    False 
Fish         False 
Mini Pig     False 
Puppy         True 
Kitten        True >>>
Puppy     4 
Kitten    5 
```

通常，Pandas 中的数据包含在一个称为数据帧的结构中。数据框是具有列的 2D 标签数据结构，如果需要，列可以是不同的类型(字符串、整数、浮点或布尔)。例如:

要创建一个 dataframe，您可以将一个列表字典传递给 Dataframe
构造函数:
1)字典的键将是列名
2)关联列表将是该列中的值。

```
>>>
   losses     team  wins  year 
0       5    Bears    11  2010 
1       8    Bears     8  2011 
2       6    Bears    10  2012 
3       1  Packers    15  2011 
4       5  Packers    11  2012 
5      10    Lions     6  2010 
6       6    Lions    10  2011 
7      12    Lions     4  2012
```

Pandas 还有各种功能，可以帮助您了解有关数据框的一些基本
信息。代码中提到的这些函数有:
1) dtypes:获取每列的数据类型
2) describe:用于查看数据帧的数字
列
的基本统计数据 3) head:显示数据集的前五行
4) tail:显示数据集的后五行

```
>>>
losses     int64 
team      object 
wins       int64 
year       int64 
dtype: object>>>
          losses       wins         year 
count   8.000000   8.000000     8.000000 
mean    6.625000   9.375000  2011.125000 
std     3.377975   3.377975     0.834523 
min     1.000000   4.000000  2010.000000 
25%     5.000000   7.500000  2010.750000 
50%     6.000000  10.000000  2011.000000 
75%     8.500000  11.000000  2012.000000 
max    12.000000  15.000000  2012.000000>>> losses     team  wins  year 
0       5    Bears    11  2010 
1       8    Bears     8  2011 
2       6    Bears    10  2012 
3       1  Packers    15  2011 
4       5  Packers    11  2012>>> losses     team  wins  year 
3       1  Packers    15  2011 
4       5  Packers    11  2012 
5      10    Lions     6  2010 
6       6    Lions    10  2011 
7      12    Lions     4  2012
```

注意:
1)从数据帧中选择单个列将返回一个序列
2)从数据帧中选择多个列将返回一个数据帧

```
>>>
0    2010 
1    2011 
2    2012 
3    2011 
4    2012 
5    2010 
6    2011 
7    2012 
Name: year, dtype: int64 >>> ... Same as above
>>> year  wins  losses 
0  2010    11       5 
1  2011     8       8 
2  2012    10       6 
3  2011    15       1 
4  2012    11       5 
5  2010     6      10 
6  2011    10       6 
7  2012     4      12
```

行选择可以通过多种方式完成。一些基本和常用的方法有:
1)切片
2)单个索引(通过函数 iloc 或 loc)
3)布尔索引

您还可以通过布尔
操作符，如& (and)或| (or)来组合多个选择需求

```
>>>
   losses   team  wins  year 
0       5  Bears    11  2010     
>>>
   losses   team  wins  year 
0       5  Bears    11  2010     
>>>
   losses     team  wins  year 
3       1  Packers    15  2011 
4       5  Packers    11  2012>>>
   losses     team  wins  year 
0       5    Bears    11  2010 
3       1  Packers    15  2011 
4       5  Packers    11  2012>>>
   losses     team  wins  year 
3       1  Packers    15  2011 
4       5  Packers    11  2012
```

接下来的课程将是关于[数据争论](https://medium.com/@tiffanysouterre/intro-to-data-science-part-2-data-wrangling-75835b9129b4)。