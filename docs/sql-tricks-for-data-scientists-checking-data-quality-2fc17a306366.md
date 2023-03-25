# 数据科学家的 SQL 技巧—检查数据质量

> 原文：<https://towardsdatascience.com/sql-tricks-for-data-scientists-checking-data-quality-2fc17a306366?source=collection_archive---------10----------------------->

![](img/5da3380a09985cb75017c9f7d5ac1762.png)

*所有的数据科学家都知道一些 SQL，但是除了将数据放入“真实”的分析环境中，它还可以有更多的用途。*

在某种程度上，SQL 是数据科学中被遗忘的秘密——被认为是从它经常驻留的数据库中获取数据的必要但有点不酷的手段，没有熊猫或 tidyverse 的缓存。

从某些方面来说，这也很公平，因为大多数 SQL 实现中提供的函数范围往往无法满足数据准备人员的需求，他们需要将表连接在一起，并应用过滤器来减少要传输到执行分析的环境中的数据量——通常是在 R 或 Python 中。

关于 SQL 的书籍使问题变得更加复杂，因为很难找到比简单的 SELECT 语句和连接的核心范围更远的书籍，可能还增加了一些聚合函数。

然而，许多人经常使用 SQL，因为我们使用的数据存储在一个符合 SQL 的数据库中，如果我们想用它做些什么，我们必须需要编写一个查询，如果我们要编写一个查询，我们最好把它做好。

类似地，对于许多分析来说，第一步是将一堆数据移动到 R 或 Python 中，我们可能会这样做——至少，我们可能会尝试移动最小但最有用的数据表。因此，确定每一列的有用性，并突出可能导致该行或列从最终模型中排除的缺失值和极值的分析可能是有用的。

确定特定列中缺失值的比例似乎应该是现成的。它不是，但仍然可以用最少的麻烦来实现，尽管需要一些技巧。

`SELECT CAST`

`(SUM (CASE WHEN column1 is NULL THEN 1 ELSE 0 END)`

`as float) / COUNT(*) AS ProportionMissing`

`FROM YourDB.YourTable`

实际上，我们正在做的是通过 CASE 语句实现 Excel 的 SUMIF。显然，我们需要对 float 进行强制转换，因为 SUM 返回一个整数，如果我们忘记了强制转换，查询在几乎所有情况下都会返回 0。

这个查询的要点是，如果任何特定列中的缺失程度意味着该列是无用的，那么将该列移到您的建模环境中就是浪费时间。

另一个基本的数据质量检查是寻找极端情况。很明显，在 SQL 中有 MAX()和 MIN()函数，但是进一步研究多个极端变量也是有用的。检测极值的一种合理的常见方法是寻找相对于平均值有过多标准偏差的值——下面使用 4 个标准偏差作为基准。

```
WITH STATS (Col1_AVG,Col1_SD) AS
    (SELECT STDEV(Col1),AVG(Col1)
    FROM Db1.Tbl1)

 SELECT Col1,DWT_AVG,DWT FROM STATS JOIN Tbl1
 ON 1=1
 WHERE ABS(Col1-Col1_AVG)/Col1_SD > 4
```

我们使用公共表表达式(以关键字‘WITH’开始的部分)，因为我们要将聚合函数的结果与它们所基于的值进行比较。上面的版本返回了极值本身，因此可以研究它们的合理性(当数据是手动收集的时，极值来自转录错误并不罕见)，但是反转大于号显然只是修剪了值。

罗伯特·德格拉夫最近的一篇关于媒体的文章是 [*【解释:最后一英里*](/explainability-the-last-mile-c2604bbbf9ff) *。*

*他还是即将出版的《管理您的数据科学项目* 》一书的作者

*在推特上关注他:*[https://twitter.com/RobertdeGraaf2](https://twitter.com/RobertdeGraaf2)