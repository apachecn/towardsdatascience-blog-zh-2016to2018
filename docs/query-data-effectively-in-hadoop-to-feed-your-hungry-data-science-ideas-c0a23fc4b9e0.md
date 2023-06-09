# 在 Hadoop 中有效地查询数据，满足您对数据科学的渴望

> 原文：<https://towardsdatascience.com/query-data-effectively-in-hadoop-to-feed-your-hungry-data-science-ideas-c0a23fc4b9e0?source=collection_archive---------5----------------------->

![](img/242bb2d07b2a305c8027dbfa4322e942.png)

Elephants in safari

我的公司的大数据越来越大，Hadoop 是 2015 年的热门话题，顺应这一趋势，我的公司决定从使用 Netezza 迁移到使用 Hadoop。因为我很久以前就在科技新闻中读到过 Hadoop，所以我对即将到来的迁移感到兴奋，并且想玩一玩这个新玩具。

# 从蜂巢到黑斑羚

在 2016 年末和 2017 年，我进入了我们的 Hadoop 环境，开始使用 Hive、Impala、Spark SQL 为我的分析项目广泛查询 HDFS 数据。在 Hive 中进行了几次查询后，我开始发现 Hadoop 比我预期的慢(比 Netezza 慢)。我的同事提醒我，Hive 是为大批量 map reduce 作业设计的，而不是为快速分析查询设计的。后来，由于内存问题和其他问题，我的 Hive 作业开始失败，我不得不使用许多设置，如'*'设置 MapReduce . map . memory . MB = 9000；set MapReduce . map . Java . opts =-xmx 7200m；*‘让我的蜂巢工作正常运转。在我的 Hive 作业中，我使用了两个以上的设置来实现正确的配置和期望的性能。

使用 Hive 设置促使我获得关于 Hadoop 架构和集群操作的知识。然而，分布式系统中的 DevOps 毕竟不是我的游戏。很快，我转向 Spark SQL 和 Impala 寻求更好的解决方案。在 Spark SQL 中，我面临类似的配置挑战(还记得 Spark config 设置命令吗？`./bin/spark-submit --name "My app" --master yarn --conf spark.executor.memory=4 g myApp.jar`)而我在 Spark SQL(这是 Spark 1.6)中运行复杂查询时遇到了失败。尽管 Spark SQL 可以无缝连接查询数据和建模数据，如下面的代码片段所示:

> `data = sqlContet.sql("select outcomes, label1, lable2 from sampleTable") labeled_data = data.map(lambda line: LabeledPoint(line[0],line[1:])) training, test = labeled_data.randomSplit([0.6,0.4], seed=1) model=LogisticRegressionWithLBFGS.train(training, intercept=1,validateData=1)`

我最终选择了 Impala 作为我的主要查询工具，因为我发现 Impala 不仅比 Hive 和 Spark SQL 快，而且只需要最小的性能调优开销。

当然，我不能不小心使用黑斑羚。为了获得良好的 Impala 性能，我需要仔细制定查询策略，并努力管理元数据。在长时间运行的作业中，黑斑羚不如 Hive 可靠，因此我会将黑斑羚的作业重写到 Hive 中，以用于生产目的。下面的链接是一篇比较 Hive、Impala 和 Spark SQL 的 IEEE 文章。这篇文章大部分支持我所经历的。

 [## Hive、Impala 和 Spark 的性能比较 SQL - IEEE 会议出版物

### 大数据中的快速查询对于挖掘有价值的信息以提高系统性能非常重要。至…

ieeexplore.ieee.org](http://ieeexplore.ieee.org/document/7334737/) 

# 查询工具选择及更多

![](img/0958cf13d45a96d19006ef380168b2ea.png)

这三种查询技术在大数据生态系统中都有自己的位置，但是当涉及到数据科学工作的迭代和探索方面时，我确实认为 Impala 等快速查询可以最有效地帮助数据科学家为他们的数据科学实验选择和整合数据源(然后使用 Spark SQL/Spark 进行建模，使用 Hive 进行生产级数据管道)。大的东西有很多小的部分。为了实现大的飞跃，我们需要快速测试小步骤的能力。在查询工具选择决策上，既要考虑其在大数据环境下的性能，也要考虑其在中等数据环境下的性能，既要考虑批处理模式下的性能，也要考虑交互模式下的性能。

数据科学家对数据、洞察和模型非常感兴趣，而 DevOps 或 DataOps 对他们的吸引力并不大。我认为减少开发工作对数据科学家来说是可取的，他们可以把更多的时间花在他们的增值上。我花在为 Hive 寻找合适的内存设置上的时间并不是很有趣。今天，我的公司在内部使用 Hadoop，我的公司可能很快就会开始在云中使用 Hadoop。许多大数据云平台承诺最小开发运维。这听起来很棒，但我愿意在接受之前仔细评估这个承诺。

在模型创建生命周期中，数据准备非常耗时。好的查询工具可以帮助我们更快地建立模型，因此我们应该给予查询工具更多的关注。在这里分享我的经验和观点，我也有兴趣从读者那里了解潜在的不同体验。在下面留下评论或者直接给我发 Twitter 消息@yuzhouyz。