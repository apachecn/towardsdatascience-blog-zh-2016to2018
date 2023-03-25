# 在 15 分钟内了解大数据分析的火花！

> 原文：<https://towardsdatascience.com/learn-spark-essentials-in-15-mins-cf1495882ae0?source=collection_archive---------8----------------------->

![](img/dfe1d639038e2ab6eb1836b120d59961.png)

我向你保证，这个简短的教程将为你节省大量阅读长篇文档的时间。准备好搭乘大数据列车了吗？我们开始吧！

## 基本概念

**RDD** : *火花的基本抽象。它将数据对象分布到多台机器上，并在内存中处理，这比在像 R 或 Python 这样的单台机器上处理要快。但是它没有组织在命名的列中。*

**Dataframe** *:构建在 rdd 之上，可以拥有一个包含列名和数据类型的模式。它比 rdd 更快，因为它为 Spark 提供了更多的数据信息。*

**惰性评估**:当你使用转换时，Spark 实际上并不执行数据，直到你调用一个动作。

**转换**:选择、过滤/where、排序/排序依据、连接、分组依据、聚集

**动作**:显示、获取、计数、收集、描述、缓存、打印模式

## 基本语法

创建一个 Rdd

```
# 1\. Read external data in Spark as Rdd
rdd = sc.textFile("path")# 2\. Create rdd from a list
rdd = sc.parallelize(["id","name","3","5"])# 3\. Dataframe to rdd
rdd = df.rdd
```

创建数据框架

```
# 1\. Read in Spark as Dataframe directly 
#  header and schema are optionaldf = sqlContext.read.csv("path", header = True/False, schema=df_schema)# 2.1 rdd to dataframe with column names
df = spark.createDataFrame(rdd,["name","age"])# 2.2 rdd to dataframe with schemafrom pyspark.sql.types import *df_schema = StructType([
**... **   StructField("name", StringType(), True),
**... **   StructField("age", IntegerType(), True)])df = spark.createDataFrame(rdd,df_schema)
```

转换:

```
# Basic transformations:# 1\. select. Index column by df.column_name or use "column_name"
df.select(df.name)
df.select("name")# 2\. filter/where they are the same
 df.filter(df.age>20)
 df.filter("age>20")
 df.where("age>20")
 df.where(df.age>20)# 3\. sort/orderBy 
 df.sort("age",ascending=False)
 df.sort(df.age.desct())# 4\. groupBy and agg
df.groupBy("gender").agg(count("name"),avg("age"))# 5\. join
df1.join(df.2, (df1.x1 == df2.x1) & (df1.x2 == df2.x2),'left')# 6\. create a new column from existing column
df.withColumn("first_name",split(name,'_')[0])
```

在 Spark 中编写 SQL

```
1\. Run sqlContext=sqlContext(sc) before create a dataframe
2\. Create a dataframe called df
3\. Run df.createOrReplaceTempView("table1") to create a temp table
4\. talbe2=sqlContext("select id, name from table1")If you are writing multiple lines, use """ like this:
talbe2=sqlContext.sql("""
select id, name 
from table1
where age>20
""")
```

动作:

```
1\. Show: show first n rows of a dataframe (but not rdd) without cells being truncated
df.show(5, truncate=False)2\. Take: display a list of first few rows of dataframe or rdd
df.take(5)3\. collect: collect all the data of a dataframe or rdd
df.collect()4\. count: count number of rows
df.count()6\. printSchema: show column names, data types and whether they are nullable.
df.printSchema()7\. cache : cache the data in memory if it's going to be reused a lot. Use unpersist() to uncache data and free memory.
df.cache()
df.unpersist()# Transformations and actions can be connected and executed in sequence from left to right. 
df1.filter("age>10").join(df2,df1.x==df2.y).sort("age").show()
```

## 外卖:

1.  如果可能的话，使用并寻找在数据帧上使用的函数，而不是 rdd，因为在数据帧上速度更快
2.  使用 collect()时要小心，除非你需要下载整个数据；使用显示/拍摄
3.  如果您以后会经常用到这些数据，请将其缓存起来。

No spam, I’ll be mindful of your time and attention

给我几个掌声，如果你觉得有帮助，就跟我来吧！

您可能对如何通过使用数据科学节省租金感兴趣:

[](https://zhenliu.org/2017/11/29/how-to-analyze-seasonality-and-trends-to-save-money-on-your-apartment-lease/) [## 如何分析季节性和趋势，以节省你的公寓租金。

### 当我在寻找一个新的公寓出租时，我开始想知道:是否有一个数据驱动的决策策略…

zhenliu.org](https://zhenliu.org/2017/11/29/how-to-analyze-seasonality-and-trends-to-save-money-on-your-apartment-lease/)