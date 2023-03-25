# 在 Python 中清理和准备数据

> 原文：<https://towardsdatascience.com/cleaning-and-preparing-data-in-python-494a9d51a878?source=collection_archive---------3----------------------->

![](img/1a63390bfbabcee7ecda5f614797ce3f.png)

Photo by [Artem Bali](https://unsplash.com/photos/9ZRbXlx9zQ4?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) on [Unsplash](https://unsplash.com/?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

## 每个数据科学家工作中无聊的部分

数据科学听起来很酷，很棒。它被描绘成很酷很棒的东西。众所周知，这是 21 世纪最性感的工作(我甚至不会把链接加到:D 那篇文章上)。所有很酷的术语都与这个领域相关——机器学习、深度学习、人工智能、神经网络、算法、模型……

但这一切只是冰山一角。我们 70–80%的工作是数据预处理、数据清理、数据转换、数据再处理——所有这些枯燥的步骤都是为了使我们的数据适合将创造一些现代奇迹的模型。

今天我想列出所有可以帮助我们清理和准备数据的方法和函数。

那么我们的数据会有什么问题呢？很多事情其实:

*   不相关的列名
*   极端值
*   复制
*   缺失数据
*   必须处理的列
*   意外的数据值

## 探索性分析

为了快速概述，我们可以使用数据帧的以下方法和属性:

```
df.head() # show first 5 rows
df.tail() # last 5 rows
df.columns # list all column names
df.shape # get number of rows and columns
df.info() # additional info about dataframe
df.describe() # statistical description, only for numeric values
df['col_name'].value_counts(dropna=False) # count unique values in a column
```

至少其中一个的输出将为我们提供第一手线索，告诉我们应该从哪里开始清理。

另一种快速检查数据的方法是将其可视化。我们使用条形图表示离散数据计数，直方图表示连续数据计数。

```
df['col_name'].plot('hist')
df.boxplot(column='col_name1', by='col_name2')
```

直方图和箱线图有助于直观地发现异常值。散点图显示了两个数值变量之间的关系。

```
df.plot(kind='scatter', x='col1', y='col2')
```

可视化数据可以带来一些意想不到的结果，因为它为您提供了数据集中正在发生的事情的视角。从上面看。

## 整洁的数据

整理数据是通过数据整理过程获得的数据。这是大数据处理过程中重要的清理过程之一，也是数据科学实践中公认的步骤。整齐的数据集有结构，使用它们很容易；它们易于操作、建模和可视化。整齐数据集的主要概念是以这样一种方式安排数据，即每个变量是一列，每个观察(或案例)是一行。

整齐的数据为数据清理提供了标准和概念，有了整齐的数据，就不需要从头开始，重新发明新的数据清理方法。

特点:

*   您测量的每个变量应该在一列中。
*   该变量的每个不同的观察值应该在不同的行中。
*   每一种变量都应该有一个表。
*   如果您有多个表，它们应该在表中包含一个允许它们被链接的列。
*   *(以上均摘自维基百科)*

为了转换我们的数据并使其整洁，我们可以使用熔化。

```
pd.melt() # transform columns to rows
```

有两个参数你要注意: *id_vars* 和 *value_vars* 。 *id_vars* 表示您不希望融合的数据列(即，保持其当前形状)，而 *value_vars* 表示您希望融合为行的列。默认情况下，如果没有提供 *value_vars* ，所有没有在 *id_vars* 中设置的列都将被融化。

```
new_df = pd.melt(df, id_vars = 'do not melt')
```

与熔化相反的操作是旋转。这里，我们将唯一值转换为单独的列。当我们希望将数据从分析形式转换为报告形式，从机器可读形式转换为人类可读形式时，我们会使用它。

```
df.pivot(index='date', columns='element', values='value')
```

虽然，这个方法不能处理重复的值。在这种情况下，我们应该使用*。pivot_table()* 有一个额外的参数— *aggfunc* ，它将根据我们提供的函数(sum、count、mean、min、max 或用户定义的函数)处理这些重复项。

```
df.pivot_table(index='date', columns='element', values='value', aggfunc=np.mean)
```

## 解析数据

有时，我们可能会错误地存储数据。例如，存储为“m014”、“f014”、“m1528”、“f1528”的性别组的值。你不能说这是完全错误的，但是最好将这些值分成“性别”和“年龄”两列。为了做到这一点，我们使用 Python 通过访问。对象类型列的字符串属性。

```
df['gender'] = df.variable.str[0] 
df['age_group'] = df.variable.str[1:]
```

## 串联数据

此外，数据可能不会出现在一个巨大的文件中，而是被分成几个不同的块，所以我们必须能够连接所有的数据，并清理它或清理第一个样本，然后对其余部分应用相同的过程。为此，我们可以使用熊猫*。concat* 方法，它提供了一个数据帧列表，将所有数据帧连接起来。默认情况下，它将存储原始索引，这将导致重复的索引值。为了防止这种情况，我们必须通过传递附加参数 *ignore_index=True 来重置新数据帧的索引。*

```
concatenated = pd.concat([df1, df2], ignore_index=True)
```

但是如果我们有成千上万的文件呢？一个一个的导入，清洗，再重复，会很傻。我们不傻，我们知道 Python 中的循环。唯一缺少的部分是找到所有要导入的文件。我们可以通过 glob library 做到这一点。因此过程如下:编写一个模式，将所有文件保存到一个列表中，遍历 csv 文件，导入每个文件并将数据帧连接成一个。看起来并不困难，但是有了代码示例就好得多了:

```
# Import necessary modules
import glob
import pandas as pd # Write the pattern: pattern 
pattern = '*.csv' # Save all file matches: csv_files
csv_files = glob.glob(pattern) # Create an empty list: frames
frames = [] # Iterate over csv_files 
for csv in csv_files:     # Read csv into a DataFrame: df 
    df = pd.read_csv(csv)     # Append df to frames 
    frames.append(df)# Concatenate frames into a single DataFrame: final_df
final_df = pd.concat(frames)
```

## 合并数据

合并与 SQL join 操作相同。您可以通过每个表中都存在的键将两个或多个表合并成一个表。有三种类型的联接:一对一、一对多、多对多。在 SQL 中，这个过程是高级的，有许多选项和修改，您必须显式地指定想要加入什么以及如何加入。在这里，所有的工作都由一个函数来完成，连接的类型只取决于数据帧中的数据。如果两个数据帧中用作键的列名相同，则使用参数' on '。

```
merged = pd.merge(left=df1, right=df2, on=None, left_on='col1', right_on='col2')
```

## 转换数据类型

拥有正确数据类型的数据是非常重要的，因为以后它可能会跟你和你的分析开一个糟糕的玩笑。还记得有一次我没有将一个列转换为正确的数据类型，花了 1 个小时试图从 string :D 中减去 float:所以要小心:d .这是数据中的一个典型错误，可以用 *pd.to_numeric()* 和*errors = ' constrate '*修复，它会将所有的 err 值转换为 nan。

要转换数据，我们可以使用*。astype()* 方法对一系列。还要记住“category”类型——它减少了数据帧的大小，加快了计算速度。我们可以转换成任何可以用作类别的值——星期几、性别、洲缩写——取决于上下文。

```
df['column1'] = df['column1'].astype(str) 
df['column1'] = df['column1'].astype('category')
df['column1'] = pd.to_numeric(df['column1'], errors='coerce')
```

## 重复值和缺失值

我们最喜欢的部分，不是吗？要删除重复项，我们可以使用 druuuuuuuuums*drop _ duplicates()*方法。

```
df = df.drop_duplicates()
```

对于缺失值，情况稍微复杂一点。通常有三种方法来处理缺失数据:

*   保持原样
*   放下它们
*   填充缺失值

要删除丢失的值，我们可以使用*。dropna()* ，但是要小心——它可能会删除你 50%的数据——这不是很好。但同样，这取决于背景。

为了填补缺失值，我们使用*。fillna()* ，也要小心——如果我们填充缺失的值，它们必须合理且有意义。

有一篇关于处理缺失数据的精彩文章，我真的没有什么要补充的。这是[链接](/how-to-handle-missing-data-8646b18db0d4)别忘了给作者一点掌声，因为那部作品太棒了。

## 维护

我们还可以使用 assert 语句以编程方式检查我们的数据。如果结果为真，则不返回任何内容，否则返回错误。

```
assert 1 == 1 # returns nothing 
assert 1 == 2 # returns error
assert df.notnull().all().all() # returns error if at least one column has one missing value
```

我跳过了正则表达式，因为它值得另写一篇文章，但是概括了我用于数据预处理的工具。请让我知道，如果有其他东西可以添加，有一个伟大的一天，并使用数据科学的好处🙂

*原载于*[*sergilehkyi.com*](http://sergilehkyi.com/cleaning-and-preparing-data-in-python/)*。*