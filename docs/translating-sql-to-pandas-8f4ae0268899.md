# 将 SQL 翻译成熊猫

> 原文：<https://towardsdatascience.com/translating-sql-to-pandas-8f4ae0268899?source=collection_archive---------16----------------------->

![](img/efcae4341d65366ea46d809f2b372e65.png)

Photo by [Aaron Burden](https://unsplash.com/photos/Qy-CBKUg_X8?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) on [Unsplash](https://unsplash.com/search/photos/autumn-aerial-landscape?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

## 两者都做相似或相同的事情，但方式不同

就像一个有 SQL 背景的人和一个经常使用 SQL 的人一样，熊猫的第一步对我来说有点困难。我总是从 SQL 的角度思考，然后想知道为什么 pandas 如此不直观。但是随着时间的推移，我习惯了一种语法，并发现了这两者之间的联系。所以我决定创建一个翻译成熊猫语言的基本 SQL 命令的备忘单。

## 挑选

最简单的 SQL 语句如下所示:

```
# Select everything from table1
SELECT *
FROM table1
```

在 2010 年，熊猫看起来会像:

```
df.head()
```

尽管在默认情况下，pandas 将只显示前 5 行，但是 SQL 中的内容相当于 LIMIT 5 或 TOP (5)(取决于 DBMS)。同样，只需在终端中输入`df`,就会显示数据帧的第一行和最后 15 行。

为了选择 SQL 中的特定列，我们将编写下一个查询:

```
SELECT column1, column2
FROM table1
```

在熊猫身上会这样写:

```
df[['column1', 'column2']]
```

## 在哪里

通常，我们希望选择具有特定标准的数据，在 SQL 中，这将使用 WHERE 子句来完成:

```
SELECT column1, column2
FROM table1
WHERE column1 = 2
```

在熊猫中，相同的查询看起来略有不同:

```
df[['column1', 'column2']].loc[df['column1'] == 2]
```

WHERE 子句接受逻辑运算符——pandas 也是如此。所以这个 SQL 查询:

```
SELECT *
FROM table1
WHERE column1 > 1 AND column2 < 25
```

在熊猫中采用以下形式:

```
df.loc[(df['column1'] > 1) & (df['column2'] < 25)]
```

pandas 中的 or 运算符是“|”，而不是—“~”。注意括号——它是强制的。

```
df.loc[(df['column1'] > 1) | ~(df['column2'] < 25)]
```

SQL WHERE 子句还接受更复杂的比较，例如:LIKE、IN 和 BETWEEN 熊猫也有能力做到这一点——只是方式不同。让我们创建一个使用所有这些运算符的查询:

```
SELECT *
FROM table1
WHERE column1 BETWEEN 1 and 5 AND column2 IN (20,30,40,50) AND column3 LIKE '%arcelona%'
```

pandas 中的相同查询(都是一行):

```
df.loc[(df['colum1'].between(1,5)) & (df['column2'].isin([20,30,40,50])) & (df['column3'].str.contains('arcelona'))]
```

关于这些方法的更多细节可以在文档中找到:[在](https://pandas.pydata.org/pandas-docs/stable/generated/pandas.DataFrame.isin.html)，[在](https://pandas.pydata.org/pandas-docs/stable/generated/pandas.Series.between.html)，[之间，如。](https://pandas.pydata.org/pandas-docs/stable/generated/pandas.Series.str.contains.html)

## 加入

在数据科学家的日常工作中，连接表是一种常见的做法，在 SQL 中，我们使用 4 种不同类型的连接来连接表:内连接、左连接、右连接和全连接。该查询如下所示:

```
SELECT t1.column1, t2.column1
FROM table1 t1
INNER JOIN table2 t2 ON t1.column_id = t2.column_id
```

对于所有的连接，语法都是相同的，所以我们只需将 INNER 更改为剩下的 3 种类型中的任意一种。在 pandas 中，这个操作最好分两步完成——首先执行 join，然后选择我们需要的数据(尽管它可以在一行中完成):

```
df_joined = df1.join(df2, on='column_id', how='inner')
df_joined.loc[['column1_df1', 'column1_df2']]
```

如果没有指定参数`on`和`how`，pandas 将使用索引作为键列来执行左连接。

## 分组依据

分组使我们能够获得关于数据的一些聚合信息:计数、总和、AVG、最小值、最大值等等:

```
SELECT column1, count(*)
FROM table1
GROUP BY column1
```

熊猫也有这种灵活性:

```
df.groupby('column1')['column1'].count()
```

我们必须在方括号中指定列名，以便在结果中只包含该列，否则我们将得到给定数据帧中每列的计数。

如果我们想要得到按列 1 分组的列 2 中所有值的总和(例如，每个商店的总销售额)，并且只显示那些已经达到某个级别的值，比如说超过 1000 个单位，我们将在 SQL 中执行以下操作:

```
SELECT store, sum(sales)
FROM table1
GROUP BY store
HAVING sum(sales) > 1000
```

在 pandas 中，我们没有 HAVING 子句的特权，但我们仍然可以通过两个步骤做到这一点:首先对数据进行分组，然后进行过滤:

```
df_grouped = df.groupby('store')['sales'].sum()
df_grouped.loc[df_grouped > 1000]
```

## 以...排序

为了在 SQL 中对结果进行排序，我们使用 ORDER BY 子句，它总是最后一个从数据库中获取结果。为了选择表中的所有值并按列 1 降序排序，我们编写了下一个查询:

```
SELECT *
FROM table1
ORDER BY column1 DESC
```

在 pandas 中，同样的查询看起来像:

```
df.sort_values(by=['column1'], ascending=False)
```

这个对我来说是最直观最清晰的。虽然，当你练习更多的时候，所有这些语法差异不再困扰你。

我们可以用 SQL 和 pandas 编写更复杂的查询，但是这种基本的对齐对我帮助很大，我相信对其他 SQL server 也有帮助。Pandas 是一个非常强大的库，让你以一种非常有效的方式提取、转换、清理你的数据。更多信息可以在我的文章[“在 Python 中提取和转换数据”](/extracting-and-transforming-data-in-python-63291f63d350)和[“在 Python 中清理和准备数据”](/cleaning-and-preparing-data-in-python-494a9d51a878)中找到

感谢您的阅读，祝您有美好的一天！

*原载于 2018 年 11 月 25 日*[*sergilehkyi.com*](http://sergilehkyi.com/translating-sql-to-pandas/)*。*