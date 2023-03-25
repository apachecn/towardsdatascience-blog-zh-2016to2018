# Pandas、Tidyverse、Pyspark 和 SQL 中的过滤、聚合和连接

> 原文：<https://towardsdatascience.com/filter-aggregate-and-join-in-pandas-tidyverse-pyspark-and-sql-71d60cbd5330?source=collection_archive---------17----------------------->

![](img/9a2bc80d85ee3e35641f0c12bfad6f0f.png)

Alike but different ([Source](https://www.pexels.com/photo/relaxation-relax-cats-cat-96428/))

# 祝福与诅咒

鼓舞人心的数据科学家最常问的一个问题是，他们应该为数据科学学习哪种语言。最初的选择通常是在 Python 和 r 之间。已经有很多对话可以帮助做出语言选择(这里的和这里的)。选择合适的语言是第一步，但我怀疑大多数人最终只使用一种语言。在我个人的旅程中，我先学了 R，然后是 SQL，再后来是 Python，再后来是 Spark。现在在我的日常工作中，我使用所有这些工具，因为我发现每个工具都有独特的优势(在速度、易用性、可视化等方面)。有些人确实只停留在一个阵营(有些 Python 人从未想过学习 R)，但是我更喜欢使用现有的东西来创建我的最佳数据解决方案，并且使用多种数据语言有助于我与不同团队的协作。

正如[机器学习的状态](https://info.algorithmia.com/enterprise-tt-state-of-machine-learning?utm_medium=social&utm_source=twitter&utm_campaign=1810-state-of-machine-learning)所指出的，我们面临着在数据团队中支持多种语言的挑战，跨语言工作的一个不利方面是我会把一种语言与另一种语言混淆，因为它们可能有非常相似的语法。考虑到我们拥有一个多种语言共存的数据科学生态系统，我很乐意并排写下这四种语言中的三种常见数据转换操作，以便阐明它们在语法层面的比较。将语法放在一起还有助于我在数据科学工具箱中更好地协同它们。希望对你也有帮助。

# 四种语言的三种数据操作

数据科学的前景是广阔的，我决定在这篇文章中关注最大的共同点。没有正式的统计数据支持，我想当然地认为:大多数数据科学工作都是基于表格数据；常见的数据语言有 SQL，Python，R 和 Spark(不是 Julia，C++，SAS 等。).

SQL 有一长串的方言(hive，mysql，postgresql，casandra 等等)，我在这个帖子里选择 ANSI-standard SQL。纯 Python 和 Base R 能够操作数据，但是在这篇文章中我选择 Pandas 作为 Python，Tidyverse 作为 R。Spark 有 RDD 和数据帧，我选择专注于数据帧。Spark 在 Pyspark 和 Sparklyr 中都有 API，我这里选择 Pyspark，因为 Sparklyr API 和 Tidyverse 很像。

三种常见的数据操作包括筛选、聚合和连接。这三个操作允许您剪切和合并表格，导出平均值和百分比等统计数据，并为绘图和建模做好准备。由于数据争论消耗了大量的数据工作时间，这三种常见的数据操作应该占数据争论时间的很大一部分。

# **1。使用标准过滤表格**

包含和排除在数据处理中是必不可少的。我们保留相关的，丢弃不相关的。我们可以用一个词来称呼包容和排斥:过滤。过滤器在表格数据中有两个部分。一个是过滤列，另一个是过滤行。使用这篇文章中著名的虹膜数据，我在下面列出了四种语言的过滤操作。

```
Question: What is the sepal length, petal length of Setosa with petal width larger than 1 ?# SQLselect Sepal_Length, Petal_Length from Iris where Petal_Width > 1 and Species=’setosa’;# PandasIris[(Iris.Petal_Width > 1) & (Iris.Species==’setosa’)][[‘Sepal_length’,’Petal_Length’]]# TidyverseIris %>% 
 filter(Petal_Width > 1, Species==’setosa’) %>%
 select(Sepal_Length, Petal_Length)# PysparkIris.filter((Iris.Petal_Width > 1) & (Iris.Species==’setosa’)).select(Iris.Sepal_Length, Iris.Petal_Length)
```

Pandas 使用括号来过滤列和行，而 Tidyverse 使用函数。Pyspark API 是借熊猫和 Tidyverse 两者之所长而定的。正如你在这里看到的，Pyspark 操作与熊猫和 Tidyverse 有相似之处。SQL 和往常一样是声明性的，显示出它的签名“从表中选择列，其中行符合标准”。

# **2。按组导出汇总统计数据**

创建计数、总和及平均值等汇总统计数据对于数据探索和特征工程至关重要。当分类变量可用时，按某些分类变量对汇总统计数据进行分组也很常见。

```
Question: How many sepal length records does each Iris species have in this data and what is their average sepal length ?# SQLselect Species, count(Sepal_Length) as Sepal_Length_Count, avg(Sepal_Length) as Sepal_Length_mean from Iris group by Species; # Pandas
aggregated=Iris.groupby(by=’Species’,as_index=False).agg({‘Sepal_Length’: [‘mean’,’count’]})
aggregated.columns = [‘_’.join(tup).rstrip(‘_’) for tup in temp1.columns.values]# Tidyverse
Iris %>%
 group_by(Species) %>%
 summarize(Sepal_Length_mean=mean(Sepal_Length), Count=n())# Pyspark
from pyspark.sql import functions as FIris.groupBy(Iris.species).agg(F.mean(Iris.sepal_length).alias(‘sepal_length_mean’),F.count(Iris.sepal_length).alias(‘sepal_length_count’))
```

这个例子很好地说明了为什么我会对这四种语言感到困惑。编写“group by”有四种略有不同的方法:在 SQL 中使用`group by`，在 Pandas 中使用`groupby`，在 Tidyverse 中使用`group_by`，在 Pyspark 中使用`groupBy`(在 Pyspark 中，`groupBy`和`groupby`都可以，因为`groupby`是 Pyspark 中`groupBy`的别名。`groupBy`看起来更真实，因为它在官方文件中使用得更频繁。

在聚合方面，Python 在这里非常不同。第一，它使用一个字典来指定聚合操作。第二，它默认使用 group by 变量作为索引，你可能不得不处理 multiindex。

# **3。连接表格以将功能放在一起**

大数据工作的一个标志是将多个数据源集成到一个源中，用于机器学习和建模，因此 join 操作是必不可少的。有一个可用的连接列表:左连接、内连接、外连接、反左连接和其他连接。以下示例中使用了左连接。

```
Question: given a table Iris_preference that has my preference on each species, can you join this preference table with the original table for later preference analysis?# SQLselect a.*, b.* from Iris a left join Iris_preference b on a.Species=b.Species;# Pandaspd.merge(Iris, Iris_preference, how=’left’, on=’Species’)# Tidyverseleft_join(Iris, Iris_preference, by=”Species”)# PysparkIris.join(Iris_preference,[‘Species’],”left_outer”)
```

这真的很神奇，我们有很多方法来表达相同的意图，在编程语言和自然语言中。

# 数据争论的备忘单

语言丰富是福也是祸。对于社区来说，在未来对跨语言的公共数据操作的 API 进行标准化可能不是一个坏主意，这可以消除摩擦并增加可移植性。到目前为止，我调查了 Pandas、Tidyverse、Pyspark 和 SQL 中的 filter、aggregate 和 join 操作，以突出我们每天最常处理的语法细微差别。上面的并排比较不仅可以作为一个备忘单来提醒我语言的差异，还可以帮助我在这些工具之间进行转换。在我的 git 库[这里](https://github.com/yuzhoux/pythonUtilityFunctions/tree/master/Cheatsheet)有这个备忘单的一个更简洁的版本。此外，你可能会发现其他特定语言的备忘单很有帮助，它们来自 Pandas.org、R Studio 和 DataCamp。

> 与熊猫的数据角力:[链接](https://pandas.pydata.org/Pandas_Cheat_Sheet.pdf)
> 
> 与 dplyr 和 tidyr 的数据角力:[链接](https://www.rstudio.com/wp-content/uploads/2015/02/data-wrangling-cheatsheet.pdf)
> 
> python for Data Science py spark:[链接](https://s3.amazonaws.com/assets.datacamp.com/blog_assets/PySpark_SQL_Cheat_Sheet_Python.pdf)

如果你想在我的小抄里增加额外的数据运算对，那就连上吧！