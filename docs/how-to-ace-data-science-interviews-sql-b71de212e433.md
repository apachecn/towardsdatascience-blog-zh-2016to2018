# 如何赢得数据科学面试:SQL

> 原文：<https://towardsdatascience.com/how-to-ace-data-science-interviews-sql-b71de212e433?source=collection_archive---------0----------------------->

![](img/272bf409890c8003cab4cb4cb3bbb653.png)

*这是正在进行的数据科学职位面试系列的一部分。你可以看看下一部分，涵盖统计，* [*这里*](/how-to-ace-data-science-interviews-statistics-f3d363ad47b) *，还有第三部分关于 R 和 Python* [*这里*](https://medium.com/@carsonforter/how-to-ace-data-science-interviews-r-python-3a49982000de) *..*

数据科学面试可能很难驾驭。事实上，这是一个多学科的领域，这意味着你需要准备的大量材料会让你不知所措。

但我是来帮忙的。**我已经将您需要了解的内容分成了四个方面，并将在本系列的每一部分中涵盖一个方面:SQL、统计、脚本和产品。**每篇文章将涵盖学习基础知识的资源、重要技术领域的概述和示例，以及我在该领域面试的一般方法的一些笔记。

如果这是你第一次参加数据科学职位的面试，阅读这一系列文章会让你对该期待什么以及如何准备有一个明确的想法。但是，即使你以前做过这些，我希望这些文章将有助于你集中精力准备最有可能被测试的技能。

# SQL 基础知识

我参加的每一次数据科学面试都以某种方式涉及到 SQL。我几乎可以保证，在你找工作的过程中，你将不得不写一些 SQL。尽管数据科学家喜欢假装我们已经从列式数据库发展到 Hadoop 等分布式系统，但事实是大多数现代数据存储仍然依赖(或至少允许)SQL 语法来检索数据。

SQL 的基础很简单，但是在数据科学面试中被问到的 SQL 问题会变得非常复杂。我猜大多数阅读这篇文章的人都有过使用这种语言的经验，但是如果你在阅读 Codecademy 的简介之前从未接触过 SQL。

本文的其余部分假设你对以下内容了如指掌:

```
SELECT
FROM
JOIN
WHERE
(AND)
GROUP BY
ORDER BY
LIMIT
```

一个`SELECT...FROM`语句将从指定的表中返回一组行和列，这些行和列的细节由您在剩余的关键字后添加的内容决定。例如，要按用户名的字母顺序统计今天的网页浏览量，您可以编写如下代码:

```
SELECT username, count(1) as number_of_views
FROM pageviews
WHERE day = '2017-09-08'
GROUP BY username
ORDER BY username;
```

明白了吗？太好了，让我们开始有趣的东西。

# 子查询和公用表表达式

你现在知道如何检索一组行和列，这可能会让你通过面试官的第一个问题。但是更高级的问题将需要[子查询](https://community.modeanalytics.com/sql/tutorial/sql-subqueries/)或[公共表表达式](https://www.postgresql.org/docs/9.4/static/queries-with.html) (CTEs)。先说这些是什么，怎么用。

**cte 和子查询允许您获取数据的子集，并用一个名称存储该数据，然后您可以从中进行选择并对其执行更多操作。**这两种方法的功能几乎相同，所以这里我只关注 cte，我发现它的语法更可读。

假设某个用户要求您计算事务之间的平均时间。您有一个名为`transactions`的表，其中包含用户名和交易时间。要解决这个问题，您需要一行中每个事务的时间以及该用户的下一个事务的时间。单个查询并不能让您一路到达目的地:您需要提取一个用户的事务，将其存储在一个 CTE 中，然后将后来的事务加入其中，这样您就可以计算两者之间的时间。这可能是这样的:

```
-- First, find all of user_a's transactions today with user_a_trans as (
  SELECT username, time
  FROM transactions
  WHERE day = '2017-09-08'
  AND username = 'user_a'),-- Join each transaction to all transactions occurring after itjoined_trans as (
  SELECT username, time, future_times
  FROM user_a_trans a
  INNER JOIN user_a_trans b
  ON b.time > a.time),-- Find the immediate next transaction using MIN()next_trans as (
  SELECT username, time, MIN(future_times) as next_time
  FROM joined_trans
  GROUP BY username, time)-- Average difference of the time and the next transaction's timeSELECT AVG(next_time - time) as avg_time_to_next_transaction
from next_trans;
```

如果您不太了解这个查询如何工作的所有细节，这完全没关系——一些练习会使它变得容易。**重要的一点是，更复杂的问题需要分解成小块，用一系列 cte 来解决。**这导致…

# 筛选、聚合、连接

当你遇到一个像上面这样的难题时，花一分钟时间问问自己，让你用一句`SELECT`话来回答你的问题，理想的表格应该是什么样的。在上面的例子中，理想的表是这样一个表，它包括每个事务的一条记录，以及给出下一个事务时间的一列。

一旦你知道你的最终表格应该是什么样的，你就可以逆向工作，一步一步地确定如何使用一系列 cte 将你的原始表格转换成你的面试官要求的最终输出。

通常，您希望对 cte 字符串执行以下步骤:过滤、聚集、连接。过滤使用`WHERE`，聚合使用`GROUP BY`，连接使用`JOIN`。冲洗并重复。

通过在加入之前过滤和聚合数据，您可以编写最高效的 SQL。连接的处理成本很高，因此您希望在将两个表连接在一起之前尽可能减少行数。有时首先聚合是不可能的，但是通常您可以用至少一两个`WHERE`子句来限制要连接的表的大小。

需要注意的是，如果在同一个 CTE 中有一个`JOIN`和一个`WHERE`子句，SQL 首先处理`JOIN`。换句话说，以下是非常低效的，因为您的整个表将被连接在一起，然后才被过滤为 2017 年 9 月 1 日之后的数据:

```
SELECT *
FROM table_a a
INNER JOIN table_b b
ON a.username = b.username
WHERE a.day >= '2017-09-01'
```

正确的表达方式是在连接之前使用 cte 过滤*，就像这样:*

```
with a as (
  SELECT *
  FROM table_a
  WHERE day >= '2017-09-01')b as (
  SELECT *
  FROM table_b
  WHERE day >= '2017-09-01')SELECT *
FROM a
INNER JOIN b
ON a.username=b.username;
```

同样，这里效率的提高来自于这样一个事实，即只有在过滤到尽可能少的行数之后才执行连接。

# 窗口功能

您可能只需要上述的*过滤、聚合、连接*过程就能解决大多数问题。不过偶尔，您会在 SQL 中遇到一个需要窗口函数的棘手问题。像一个`GROUP BY`子句一样，窗口函数将你的表分成几个块，并分别对每个块进行操作。但与`GROUP BY`不同的是，行并没有合并。举个例子是理解这一点最简单的方法。

假设您有一个包含一些行项目的表，这些行项目报告收入及其来自的美国州。你的任务是确定每个行项目的收入占该州总收入的百分比。

这里的技巧是要认识到，您需要将单个值(特定行项目的收入)与聚合值(特定州的所有行项目收入的总和)进行比较。任何时候你需要做这样的事情，窗口函数是一个很好的选择。该查询可能是这样的:

```
with state_totals as (
  SELECT state, revenue, 
    SUM(revenue) OVER (PARTITION BY state) as state_revenue
  FROM state_line_items)SELECT state, 
  revenue/state_revenue as percent_of_state_revenue
FROM state_totals;
```

窗口功能由`OVER`子句指定。通过对按州划分的收入求和，您可以获得与每个单独行项目相关联的每个州的合计值。这使得得到你关心的百分比贡献数字成为一个简单的除法问题。

窗口函数适用于大多数聚合，如`SUM`、`COUNT`或`AVG`，但也有一些特殊的关键字仅用作窗口函数。一些很好的例子是`RANK`、`FIRST_VALUE`和`LAG`。你可以在 [Postgresql 文档](https://www.postgresql.org/docs/9.1/static/tutorial-window.html)中阅读更多的窗口函数以及它们是如何工作的。我上面提到的六个函数可能就是你成功完成一次典型的数据科学 SQL 面试所需要的全部。

# Union 和 Case 语句

为了充实您的 SQL 库，您只需要一些工具。第一个很容易理解:工会。

联合只是联接的垂直版本:联接使用一个联接键水平地组合表或 cte，而联合只是将表堆叠起来，形成一个包含两个原始表中所有行的表。这样做的要求是被联合的两个表具有完全相同的列——否则就没有办法在逻辑上组合它们。

union 可能有用的一个例子是，当您有两种类型的事务的不同表，但希望用一个查询告诉您每种类型的事务有多少个时。

```
with sales as (
  SELECT 'sale' as type
  FROM sale_transactions
  WHERE day >= '2017-09-01'),buys as (
  SELECT 'buy' as type
  FROM buy_transactions
  WHERE day >= '2017-09-01'),unioned as (
  SELECT type
  FROM buys
  UNION ALL
  SELECT type
  FROM sales) SELECT type, count(1) as num_transactions
FROM unioned
GROUP BY type;
```

通过从两个表中各选择一个常量`type`字段(在本例中只是‘sale’或‘buy’)，然后将它们合并，最终得到一个大表，可以在单个查询中对其进行分组和计数。

当您希望两个表被构造成一个组合表时，联合就是答案。

Case 语句是另一个相当简单的概念:它们与 R 和 Excel 等环境中的`ifelse()`函数完全相同。它们对于从一组预定义值映射到另一组值很有用。

例如，您可能希望将一周中的某一天列转换为表示该天是否为周末的变量。

```
SELECT 
  CASE WHEN day_of_week in ('Sat', 'Sun') 
    then 'Weekend' else 'Weekday' end as day_type
FROM table_a;
```

类似地，您可以将一个字符串列(如星期几)转换为一个二进制变量，并对其求和以计算表中周末的天数。

```
SELECT 
  SUM(
    CASE WHEN day_of_week in ('Sat', 'Sun')
    THEN 1 ELSE 0 END) as num_weekend_days
FROM table_a;
```

Case 语句非常灵活——您可以将一串`WHEN`串在一起，将任何值映射到任何其他值，然后使用一个`ELSE`来捕捉其余的值。

# 包扎

现在，您已经具备了成功通过数据科学面试的 SQL 部分所需的所有要素。当然，实践是这里的关键。试着给自己设置几个问题，用我上面描述的工具解决它们。更好的办法是，找一块白板，在上面练习，这样你在面试中会更加自如。如果你陷入困境，谷歌是你的朋友。Stack Overflow 和类似的网站有如何解决任何特定 SQL 问题的详细指导，谷歌通常很擅长从他们那里找到你需要的东西。

我将留给你一些关于面试本身的建议。

**将每个问题分解成尽可能小的部分。**这是清楚地思考 SQL 问题的最佳方式，并且允许您将每个步骤映射到具体的 SQL 命令。

**大声说出你的过程。就像在学校一样，你会因为展示自己的作品而获得荣誉。如果你只是开始在白板上写东西，而不让你的面试官知道你在做什么，他们将很难评估你的技能，尤其是如果你没有完成最后的回答。**

**寻求帮助。信不信由你，大多数面试官都希望你成功，并乐意提供一些帮助，只要你能准确说出你有困难的地方。换句话说，可以问一些问题，比如将整数转换成浮点数的正确语法是什么，但是要避免问一些模糊的问题，这些问题可能表明你不知道如何解决你试图解决的问题。**

如果您对数据科学面试流程有任何疑问，可以在 [Linkedin](https://www.linkedin.com/in/carson-forter/) 或 [Twitter](https://twitter.com/carsonforter) 找到我。并在接下来的几周中寻找本系列的下一部分，包括统计数据。