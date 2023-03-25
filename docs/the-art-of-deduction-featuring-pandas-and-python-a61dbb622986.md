# 演绎的艺术:以熊猫和蟒蛇为特色

> 原文：<https://towardsdatascience.com/the-art-of-deduction-featuring-pandas-and-python-a61dbb622986?source=collection_archive---------4----------------------->

![](img/97d2edf5f815b240ddbbd0086c1a3e82.png)

Photo by [Jeremy C](https://unsplash.com/photos/RzA62WEBXDk?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) on [Unsplash](https://unsplash.com/search/photos/panda?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

如果你选择 Python 作为你的帮凶，那么几乎每个开始学习机器学习或数据分析或几乎任何其他数据科学相关主题的人都必须学会使用 Pandas。(就我个人而言，我想念 r。还没有失去爱情)

这是对熊猫的一个简短而全面的介绍，也许是你入门所需要的全部。关于熊猫的内容比我们在本文中介绍的要多，但是随着您的数据科学之旅的进行，您一定会了解到它。但是这肯定会让你准备好阅读一些代码，理解“ *pd* ”符号在某些地方到底在做什么。

# 我们为什么需要熊猫？

嗯，*因为熊猫对中国竹林的生存至关重要*(英国广播公司和其他各种来源已经展示了这些濒临灭绝的美丽生物的重要性)。哇！你已经爱上熊猫了！谈到 Python 的 Pandas 库，它已经成为 Python 中数据分析和建模的标准。除非你正在使用 R(还没有失去爱)，你将需要依赖熊猫的一切-数据。

Pandas 具有以下**优势**，这使得它成为 Python 数据分析的事实标准:

*   *数据帧*
*   *易于读写多种表格格式(CSV、Excel 甚至 SQL)的数据*
*   *处理缺失数据*
*   *轻松重塑数据*
*   *切片和混叠操作简单*
*   这个库是用 C 语言编写的，这提高了它的性能。。。还有更多！

# **熊猫怎么装？**

关于安装说明，请参考官方 [Pandas 文档。我保证这真的很容易设置，可能只需要一行代码就可以执行。](https://pandas.pydata.org/pandas-docs/stable/install.html)

导入 pandas 库的标准符号是:

```
import pandas as pd
```

# **导入数据**

有几种类型的数据文件格式可以作为 Pandas *DataFrame* 载入。加载不同类型的数据有特定的格式，一般格式如下:

```
my_data = pd.read_csv(‘ __link to data file here with .csv extension__ ‘)
```

大多数情况下，我们会处理 CSV( *逗号分隔值*)类型的文件，所以这种符号应该足够了。CSV 文件比 XLS 更受青睐，因为 Excel 文件有一些格式问题，有时会变得混乱。我们不想处理那种情况，对吗？好像是 2018 年！
Pandas 支持许多其他文件加载服务，这些服务可以在官方文档中找到([链接](http://pandas.pydata.org/pandas-docs/stable/io.html))。

我们在本文中使用了术语 DataFrame。实际上熊猫给我们提供了两种东西:

## **数据帧:**

一个*表格格式*(可以可视化为 SQL 表或者 Excel 表)
构造一个数据帧的符号是:

```
my_frame = pd.DataFrame({‘col_1’:[val_1,val_2,val_3], ‘col_2’:[val_4,val_5,val_6]}, index=[‘index_1’, ‘index_2’, ‘index_3’])
```

上面的代码将创建一个 3*2 大小的数据帧，称为 *my_frame* 。你可以把数据帧看作 Python 字典，它的键是列名，值是列中的值列表。
data frame 中的附加参数为我们的数据分配一个特定的索引。然而，现在可以忽略这一点，因为我们对从 1 开始的一般自动索引完全没有问题。

## 系列:

一个*值数组*。就像 Python 中的列表一样。

```
my_list = pd.Series([val_1, val_2, val_3], index=[index_1, index_2, index_3], name=’Column_name’) 
```

*中提琴！这个很简单！* 一个系列被可视化为一个数据帧的一列。因此它具有所需的属性。同样，我们可以跳过索引部分，但我们应该记住名称部分。(如果要将此列添加到数据框架中，您需要为新列命名)

您加载的数据帧的形状可以使用以下代码行来描述:

```
print(my_dataframe.shape)
```

这将打印出类似于:(1000，10)
的内容，这意味着您的 DataFrame 有 1000 行数据和 10 列。整洁！

您还可以使用" *head()* "函数打印出数据帧的前 5 行(只是为了大致了解数据类型或者列包含的值类型)。

```
my_dataframe.head()
```

另一方面，您也可以将修改后的数据帧存储成各种格式。

```
my_dataframe.to_csv(“ __name of file with .csv extension__ “)
```

你的文件会和数据框中的所有数据一起保存到你的工作目录中。

> 希望到现在为止一切都清楚了。这些是我们在执行分析时要做的基本事情——加载和读取数据，查看我们在处理什么。继续前进。

## 选择、切片和各种其他数据格式化操作:

DataFrame 上的大多数 select 操作与我们在本地 python 对象(如列表和字典)上执行的操作非常相似。
使用点符号可以通过以下方式访问整个列:

```
my_data.column_1
#or
my_data[‘column_1’]
```

此外，如果我们需要所选列中的特定数据行，我们可以以类似的方式使用索引来选择该特定行:

```
my_data[‘column_1’][5]
#Remember that DataFrames follow the zero indexing format
```

我们还可以使用 **iloc** 操作符来完全基于索引对数据集进行切片，也就是说，我们将使用列名的索引来选择该行的列和行的索引。

```
my_data.iloc[:,0]
#This would select all the rows from the column index 0(1st column). #Just “:” represents “everything”.#Or to select the first 3 rows: 
my_data.iloc[:3,0]
```

**需要注意的是，当我们使用“0:4”格式时，左边的数字代表切片开始的索引，右边的数字代表切片发生的索引，即右边的索引不会被选择。这种符号也适用于 Python 中的切片操作。没什么好担心的！**

我们还可以通过一个列表来选择特定的列，而不是以有序的方式进行切片:

```
 my_data.iloc[[0,5,6,8],0]
```

最好探索你所有的选择。越来越多地调整函数来测试它的能力和用途。

现在，我们必须记住的最重要的选择特性是**条件选择**。在许多情况下，条件选择可以节省一天的时间和大量的时间。*它的作用很像 SQL* 中的“WHERE”子句。
在这些情况下，iloc 的兄弟“ **loc** ”出现了。就像 iloc 支持基于整数的索引一样， *loc 可以基于标签或传递给它的布尔数组来访问数据*。

```
my_data.column_1 == some_numeric_value#This would return a Series of boolean data based on wether the condition was evaluated as true or not. We can then pass this Series to the loc function to conditionally pick all the rows who’s index evaluated to be TRUE for the conditionmy_data.loc[my_data.column_1 == some_numeric_value]
```

请仔细阅读 loc 操作符的官方 [Pandas 文档](https://pandas.pydata.org/pandas-docs/stable/generated/pandas.DataFrame.loc.html)以获得更全面的理解:
(不要担心 iloc，因为您读到的几乎就是它)

你也可以在 Pandas 中浏览与[索引和选择数据相关的官方文档。](https://pandas.pydata.org/pandas-docs/stable/indexing.html#indexing-integer)

Pandas 中有一些非常有用的函数可以用来汇总数据:

```
my_data.head()
#Prints out the first five rows of your DataFramemy_data.column_name.describe()
#Prints out some important statistical values like count, mean, max etc of a specific column. In case the column containts String values, it will print the count, freq and top etcmy_data.column_name.mean()
#You get itmy_data.column_name.unique()
#If the column contains String values, this function will print out all the unique values present in that column
```

这些是数据帧上一些最常用的汇总函数。还有更多这些不像这些那样经常使用。有关[汇总功能](https://pandas.pydata.org/pandas-docs/stable/basics.html)的更多信息，请参考文档。

# 处理缺失数据

通常我们的数据帧中会有缺失的数据。这些是没有描述数据的字段。这可能会给我们的模型评估或分析带来问题。在我们继续其他阶段之前，将所有 *NaN* 值解析为某个实际值是很重要的。
NaN 代表“*非数字*”，NaN 的 dtype 设置为 *float64* 。
我们有一个优秀的操作符，用于轻松选择这些数据字段，名为“ **isnull** ”，反之，“ notnull ”。这些函数将自动选择指定列中包含任何 NaN 值的所有行，因此我们可以共同为它们分配一些值。

```
my_dataframe[my_dataframe.column_name.isnull()]
```

我们有一个熊猫提供的优秀函数来解决上述问题: **fillna** 。我们可以在数据帧或选定的数据帧上调用 fillna，为指定列中的所有 NaN 值赋值。

```
my_data.column_name.fillna(value_to_be_passed)
```

这些是文档的一些重要部分，您应该仔细阅读以理解一些更深入的关键概念。压缩它没有意义，因为最好通过适当的详细示例来理解这些内容:

*   [分组依据:拆分-应用-合并](https://pandas.pydata.org/pandas-docs/stable/groupby.html)
*   [合并、加入和连接](https://pandas.pydata.org/pandas-docs/stable/merging.html)

除此之外，理解方法链的概念也很重要。我不能强调方法链接在数据分析阶段的重要性。
Joost H. van der Linden 制作了一个优秀的[资源库](https://github.com/joosthvanderlinden/pandas-method-chaining-tutorial)，它基于方法链概念，带有一个 iPython 笔记本，你可以运行、编辑和修改。所以一定要在 Github 上查看。

你可以在 [Kaggle](https://www.kaggle.com/) 上找到的[泰坦尼克号生存问题](https://www.kaggle.com/c/titanic)的数据集是一个很好的练习数据集。您可以找到许多人在各种来源上开发的优秀方法，或者您可以查看其他用户提交的其他内核。几乎所有人最初都会遵循必要的数据汇总和清理步骤。

我尝试用 R 来解决这个问题，所以我现在不能发布我自己的解决方案。但是万一你想试试 R，这将是一个好机会。分析的所有阶段通常都是相同的，但在使用 R 时，您可以期待一个更加干净和专注的环境。如果您需要 R 中的问题的教程，我想推荐 [*David Langer 的 Youtube 频道*](https://www.youtube.com/channel/UCRhUp6SYaJ7zme4Bjwt28DQ) *。我真的很喜欢他引导观众通过所有必要的步骤和代码背后的工作的流程。*

> 就这样，我们结束了对可爱熊猫的温柔介绍。
> 用熊猫，爱熊猫，救熊猫！