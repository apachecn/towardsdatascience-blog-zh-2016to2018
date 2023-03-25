# PySpark UDFs 和恒星膨胀

> 原文：<https://towardsdatascience.com/pyspark-udfs-and-star-expansion-b50f501dcb7b?source=collection_archive---------3----------------------->

![](img/c9cbfec80c1af97e3455243a6b2ba4e3.png)

在很大程度上，我发现从主要从事 SQL 工作到主要从事 Spark 工作的转变是平稳的。熟悉 SQLalchemy 和 Django 这样的 ORM，适应起来并不难。选择、过滤、连接、分组以及诸如此类的事情都或多或少地按照它们在 SQL 中的方式工作。我认为构建一个数据查询是有逻辑的，对这种逻辑的感觉是战斗中最困难的部分——剩下的只是实现。

在我看来，Spark 的难点在于资源管理。找出内核、内存(堆内和堆外)和执行器的正确平衡是一门艺术，而不是科学，Spark 给你的错误消息通常只能提供最少的信息。我发现很少有好的 Spark 培训材料。也许我因为主要使用 PyData 和 SQL 堆栈而被宠坏了:当我有关于 Pandas 或 Postgres 的问题时，我会搜索它，它几乎总能给我一些相当完整的文档和工作示例，经常给我一些信息丰富的博客帖子，并且通常给我至少一个有用的堆栈交换问题。当我有一个关于 Spark 的问题时，我会在 Google 上搜索它，通常会得到一个文档页面，简单地确认该方法在技术上是存在的。仅此而已。

当涉及到 PySpark 用户定义函数(UDF)时，我发现资源管理特别棘手。Python UDFs 是在 Spark 中进行数据科学研究的一种方便且经常必要的方式，尽管它们不如使用内置 Spark 函数甚至 Scala UDFs 高效。使用 Python UDF 时，了解 Spark 如何评估它很重要。考虑下面的例子，该例子假设火花数据帧‘SDF’具有两个数字列‘col 1’和‘col 2 ’:

```
**import** pyspark.sql.functions as f
**import** pyspark.sql.types as t**def** my_function(arg1, arg2):
    argsum **=** arg1 **+** arg2
    argdiff **=** arg1 **-** arg2
    argprod **=** arg1 ***** arg2
    **return** argsum, argdiff, argprodschema **=** t.StructType([
    t.StructField('sum', t.FloatType(), False),
    t.StructField('difference', t.FloatType(), False),
    t.StructField('product', t.FloatType(), False),
])my_function_udf **=** f.udf(my_function, schema)results_sdf **=** (
    sdf
    .select(
        my_function_udf(
            f.col('col1'), f.col('col2')
        ).alias('metrics'))   # call the UDF
    .select(f.col('metrics.*')) # expand into separate columns
)
```

如果在执行上述代码块后调用` results_sdf.explain()'，您应该会看到这样一行内容:

```
BatchEvalPython [my_function(col1#87, col2#88), my_function(col1#87, col2#88), my_function(col1#87, col2#88)]
```

这意味着，为了对度量字段进行星形扩展，Spark 将调用您的 udf 三次——对模式中的每个项目调用一次。这意味着你将使用一个已经低效的函数并多次运行它。

通过对代码做一点小小的修改，就可以让 Spark 只计算一次 UDF:

```
results_sdf **=** (
    sdf
    .select(
        f.explode(
            f.array(
                my_function_udf(f.col('col1'), f.col('col2'))
            )
        ).alias('metrics')
    )
    .select(f.col('metrics.*'))
)
```

将结果封装在一个数组中，然后分解该数组会产生一些开销，但是与只计算一次 UDF 所节省的资源相比，这些开销是微不足道的:

```
BatchEvalPython [my_function(col1#87, col2#88)]
```

您可以通过在进行 star 扩展之前对数据帧进行持久化或检查点操作来获得相同的结果，但是这样会耗尽内存和磁盘空间，您可能不希望这样做。