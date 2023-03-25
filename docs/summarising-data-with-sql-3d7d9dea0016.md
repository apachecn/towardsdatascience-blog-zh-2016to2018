# 用 SQL 汇总数据

> 原文：<https://towardsdatascience.com/summarising-data-with-sql-3d7d9dea0016?source=collection_archive---------18----------------------->

![](img/cf026ef8dfdae685825324d70f7a31c8.png)

SQL 不仅可用于将数据从数据库传输到建模环境。了解有关如何现场分析您的数据以确定最有用的信息的更多信息

在我们[之前从真正的数据科学角度对 SQL 进行分析的](/sql-tricks-for-data-scientists-checking-data-quality-2fc17a306366)中，我们探讨了如何使用 SQL 来了解数据集在缺失方面的质量，以了解哪些表和字段值得迁移到建模首选环境中。在这一期中，我们将看到一个经典的工具，用于理解不同变量的基本性质，以及如何在 SQL 中获得这些变量。

约翰·图基重新激活了探索性数据分析的实践，并通过他的同名著作大量推广了这个短语本身。Tukey 提出的最简单也是最有用的工具之一是五个数摘要。考虑到它的有用性，R 使用一个简单的命令就可以从任何数据集——five num()——中获得这个摘要，这个数据集在 base R 中。

这非常简单地由变量的最小值、最大值、中间值、第一个四分位数和第三个四分位数组成。假设最大值和最小值一直是标准的集合，并且不需要使用复杂的统计分布或矩阵代数来准备任何值，那么可以认为这种汇总应该很容易从任何 SQL 实现中获得。

然而，就在 21 世纪初，这实际上还是一个相当繁琐的问题。值得庆幸的是，尽管许多东西，比如流行音乐，从那以后已经明显恶化了(在克林顿执政期间，我达到了有资格开车的年龄，这与流行音乐对我来说听起来最好的时期密切相关)，但从那以后，标准 SQL 和最大的实现都引入了有用的新特性。

ANSI SQL 2003 版中添加的窗口函数提供了一系列有用的选项。其中，PERCENTILE_CONT 和 PERCENTILE_DISC 使得获得完整的五个数摘要集比以前简单得多。

这些函数的基本语法是:

```
PERCENTILE_DISC(int) WITHIN GROUP (ORDER BY numeric)
OVER (PARTITION BY variable)
```

当目的是将数据集划分为由数据集中的分类变量定义的子集时，这些函数通常是供某些人使用的。五个数摘要的概念中隐含的意思是，至少在第一遍中，我们希望对整个数据集运行它。为此，我们只需将 OVER 子句留空，例如，“OVER (PARTITION BY variable)”就变成了“OVER()”。

因此，获取任何单个数值变量的第一个四分位数、中值和第三个四分位数的完整查询如下所示:

```
SELECT DISTINCT
  PERCENTILE_DISC(25) WITHIN GROUP (ORDER BY numeric)
  OVER () as Q1_Var
, PERCENTILE_DISC(50) WITHIN GROUP (ORDER BY numeric)
  OVER () as median_var
, PERCENTILE_DISC(75) WITHIN GROUP (ORDER BY numeric)
  OVER () as Q3_var
FROM db.tbl
```

该查询显然返回三个值—最大值和最小值，缺少五个数字汇总中最基本的一个。但是，由于百分比值不是聚合函数，如果我们简单地将 min(var)和/或 max(var)添加到 Select 语句中，SQL 将抛出语法错误。

相反，我们需要使用一个公共表表达式创建第二个表，该表可以包含标量形式的聚合值，并将这两个表连接在一起。假设我们将单行连接在一起，我们可以做一个完整的笛卡尔连接，并且只期望返回一行。

```
WITH extreme_table (var_min,var_max)AS (SELECT
    min(var)
  , max(var)
  FROM table) SELECT DISTINCT
  var_min
, PERCENTILE_DISC(25) WITHIN GROUP (ORDER BY numeric)
  OVER () as Q1_Var
, PERCENTILE_DISC(50) WITHIN GROUP (ORDER BY numeric)
  OVER () as median_var
, PERCENTILE_DISC(75) WITHIN GROUP (ORDER BY numeric)
  OVER () as Q3_var
, var_maxFROM db.table JOIN extreme_table on 1=1
```

该查询返回单行的五个值，表示五个数字的汇总。但是，我们在上面提到过，SQL 百分位函数更频繁地与分区变量一起使用，以应用于包含特定分类变量的子集的数据集，对于通常与 Group By 变量一起使用的更多运行的 mill 聚合函数 min()和 max()也是如此。

直觉上，许多分析师也想将五个数字的汇总应用于不同的数据子集。扩展上面的查询，通过在 extreme_table 的 Group By 子句和 select 子句中添加分类变量，并在 category 上连接之前添加 PARTITION 子句，可以直接实现这一点。结果将如下。

```
WITH extreme_table (var_min,var_max,category)AS(SELECT
    min(var)
  , max(var)
  , category FROM table
  GROUP BY category)SELECT DISTINCT
  var_min
, PERCENTILE_DISC(25) WITHIN GROUP (ORDER BY numeric)
  OVER (PARTITION BY category) as Q1_Var
, PERCENTILE_DISC(50) WITHIN GROUP (ORDER BY numeric)
  OVER (PARTITION BY category) as median_var
, PERCENTILE_DISC(75) WITHIN GROUP (ORDER BY numeric)
  OVER (PARTITION BY category) as Q3_var
, var_max
, table.categoryFROM db.table JOIN extreme_table on extreme_table.category=table.category
```

从五个数字汇总中获得的通常分析是一种偏斜感——通过查看中位数在第一和第三四分位数中的位置，以及最小值和最大值与这些四分位数的关系。显然，在数据的分类子集上获得相同的数据是非常有用的，因为它不仅允许您比较数据的位置(中位数),还允许您比较每个类别的分布形状，以了解每个类别如何集中其基础数据。

*罗伯特·德格拉夫是《进展中的书’*[*【懒惰的数据科学家】*](https://leanpub.com/thelazydatascientist) *的作者，可通过 LeanPub 在推特上关注他:*[https://twitter.com/RobertdeGraaf2](https://twitter.com/RobertdeGraaf2)