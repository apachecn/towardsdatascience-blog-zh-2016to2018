# 别嘲笑我了！使用 Python 的模拟库在 PySpark 中进行单元测试

> 原文：<https://towardsdatascience.com/stop-mocking-me-unit-tests-in-pyspark-using-pythons-mock-library-a4b5cd019d7e?source=collection_archive---------6----------------------->

![](img/7ea23835c83a65b175ae5a79642d85ec.png)

测试。

这是软件开发的基础，也是经常被数据科学家忽视的，但是非常重要。在这篇文章中，我将展示如何使用 Python 的 [unittest.mock](https://docs.python.org/3/library/unittest.mock.html) 库在 PySpark 中进行单元测试。我将从数据科学家的角度来做这件事——对我来说，这意味着我不会深入软件工程的细节。我只提供你需要知道的。

首先，一个来自《一家人》的(半)相关片段:

# 什么是单元测试？什么是模仿？

单元测试是一种测试代码片段的方法，以确保事情按预期运行。Python 中的 *unittest.mock* 库允许您用模拟对象替换部分代码，并断言它们是如何被使用的。“模拟”是一个顾名思义的对象——它模拟代码中对象/变量的属性。

# 最终目标:测试 spark.sql(查询)

在 PySpark 中创建数据帧的一个简单方法是执行以下操作:

```
df = spark.sql("SELECT * FROM table")
```

虽然简单，但是要测试。

# 代码和问题设置

假设我们为一家电子商务服装公司工作，我们的目标是创建一个根据某些条件过滤的产品相似性表，并将其写入 HDFS。

假设我们有以下表格:

```
1\. Products. Columns: “item_id”, “category_id”.2\. Product_similarity (unfiltered). Columns: “item_id_1”, “item_id_2”, “similarity_score”.
```

(假设 *product_similarity* 中的分数在 0 和 1 之间，其中分数越接近 1，项目越相似。如果你感兴趣的话，请看我在相似性度量上的文章。

查看成对的项目及其分数非常简单:

*where* 子句用于删除将项目与其自身进行比较的行。分数永远是 1。多无聊啊！

但是，如果我们想要创建一个表，向我们显示属于同一类别的项目的相似性，该怎么办呢？如果我们不在乎把鞋子比作围巾，而是想把鞋子比作鞋子，把围巾比作围巾，怎么办？这有点复杂，需要我们连接“products”和“product_similarity”表。

该查询将变成:

我们可能还想获得每种产品的最多 *N* 个相似商品，因此在这种情况下，我们的查询将变成:

(假设我们用 *N* = 10)。

现在，如果我们希望选择跨类别比较产品，或者只在类别内比较产品，该怎么办？我们可以通过使用一个名为 *same_category* 的布尔变量来实现这一点，该变量会产生一个字符串 *same_category_q* ，该字符串可以被传递到整个查询中(使用*)。format()* )，如果我们的布尔 *same_category* 为真，将等于上面的内部连接，如果为假，则为空。该查询将类似于:

让我们说得更清楚一点，将这个逻辑封装在一个返回 *same_category_q* 的函数中:

到目前为止，一切顺利。我们输出查询 *same_category_q* ,这样我们可以测试我们的函数，以确保它返回我们想要它返回的内容。

牢记我们的最终目标，我们想写一个数据帧给 HDFS。我们可以通过以下函数来实现这一点:

添加查询的第一部分和一个 main 方法来完成我们的脚本，我们得到:

这里的想法是，我们希望为脚本中的每个函数创建一个函数，一般命名为 *test_name_of_function()* 。我们想要测试函数的行为是否正常，我们通过到处使用 assert 语句来确保这一点。

# Test_make_query，真与假

首先，让我们测试一下 *make_query* 函数。回想一下 *make_query* 接受两个输入:一个布尔变量和一些表路径。它将根据布尔值 *same_category* 为 *same_category_q* 返回不同的值。我们在这里做的有点像一组 if-then 语句:

```
1\. If same_category is True, then same_category_q = “INNER JOIN …”2\. If same_category is False, then same_category_q = “” (empty)
```

我们所做的是模仿 *make_query* 的参数，并将它们传入，然后测试我们是否得到了想要的输出。由于 *test_paths* 是一个字典，我们不需要模仿它。测试脚本如下，为了更加清晰，添加了注释。

而且就是这么简单！

# 测试我们的表创建

接下来，我们需要测试我们的 *create_new_table* 函数的行为是否正常。通过这个函数，我们看到它做了几件事，有几次机会做一些断言和模拟。注意，每当我们有类似*df . write . save . something . another thing*的东西时，我们需要模拟每个操作及其输出。

1.  该函数以*火花*为参数。这需要被嘲笑。
2.  通过调用*spark . SQL(create _ table _ query . format(* * some _ args))*创建 created_table。我们要断言 *spark.sql()* 只被调用一次。我们还需要模拟 *spark.sql()* 的输出。
3.  合并*已创建 _ 表*。确保用参数 *1* 调用 *coalesce()* 。模仿输出。
4.  写入合并后的表。我们需要嘲讽*。编写*，在我们的表上模拟调用它的输出。
5.  将合并的表保存到保存路径。确保用正确的参数调用它。

和以前一样，测试脚本如下，为了清楚起见，添加了注释。

最后，把所有的东西一起保存在一个文件夹里。如果你愿意，你可以从它们各自的模块中导入函数，或者把所有的东西都放在一个脚本中。

要测试它，在命令行中导航到您的文件夹( *cd 文件夹/working_folder* )并调用:

```
python -m pytest final_test.py.
```

您应该看到类似这样的内容(请原谅这种格式):

```
serena@Comp-205:~/workspace$ python -m pytest testing_tutorial.py ============================= test session starts ============================== platform linux -- Python 3.6.4, pytest-3.3.2, py-1.5.2, pluggy-0.6.0 rootdir: /home/serena/workspace/Personal, inifile: plugins: mock-1.10.0 collected 3 items testing_tutorial.py ... [100%] =========================== 3 passed in 0.01 seconds ===========================
```

# 就是这样！

这就是了。我希望这有所帮助。当我试图找出如何嘲笑时，我希望我遇到过类似这样的教程。现在继续吧，就像 Stewie 说的，(不要)停止嘲笑我(函数)！

*原载于 2018 年 11 月 7 日 serena.mcdonnell.ca。*