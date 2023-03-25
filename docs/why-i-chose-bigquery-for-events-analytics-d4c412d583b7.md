# 为什么我选择 BigQuery 进行事件分析，为什么你也应该这样做

> 原文：<https://towardsdatascience.com/why-i-chose-bigquery-for-events-analytics-d4c412d583b7?source=collection_archive---------13----------------------->

![](img/190cfb5015cd9ecbeecd1ba3a3e96e2a.png)

Photo by [ev](https://unsplash.com/photos/gpjvRZyavZc?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) on [Unsplash](https://unsplash.com/search/photos/big-data?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

在每一个创业公司，都有你必须设计大规模分析事件摄取的时候。

主要的想法是，你有一个后端日志事件，你想有关于他们的分析见解。

这几乎是经典的数据工程任务之一。

你对创业公司的要求可能听起来像这样:

*   **你想要一个可管理的解决方案**
    你不想花费大量的工程精力和时间。你想迭代…而且你想快速完成。你希望你的注意力集中在产品的开发上，而且要做得好、做得快。花在管理基础设施上的时间越少，可能越好。
*   **您希望能够在事件发生后尽快查询事件** 这并不总是一个要求。通常，你想对过去的事件进行分析，但你并不在乎在事件发生后的第二天你是否能做到。例如，当您需要为用户提供帮助，并且您需要知道特定用户发生了什么事情时，您实际上几乎必须立即做这件事。
*   **保持成本可控** 显然，没有人愿意支付不必要的费用。
*   **如果需要，能够在短时间内大规模扩展** 这是一个好方法。通常，你想构建最简单的东西来解决你的问题，但不是更简单。此外，您希望有一种好的方法来最终扩大规模，而不必从头开始重建一切。这有时会发生，不过没关系。

如果你看看市场，几乎有无限多的解决方案…哪一个是最好的？嗯，这真的取决于你的用例。

对我来说，有三个主要角色:

1.  BigQuery
2.  亚马逊红移
3.  亚马逊红移光谱

# BigQuery

BigQuery 是 Google analytics DB。

它由 Colossus (Google 分布式文件系统)提供动力，每个查询由 Dremel (Google 查询引擎)转化为执行树，数据从 Colossus 中检索并聚合……一切都运行在 Google Jupiter 高速网络上。

作为一个分析数据库，BigQuery 使用列存储。如果您不熟悉这种存储，它不同于传统的事务数据库，后者按行存储数据。这样做是为了反映通常在分析数据库上执行的查询类型。

通常，你会有这样的东西:

```
SELECT foo, bar
FROM sales_events
WHERE timestamp between '2018–01–05' and '2018–01–09'
```

如您所见，我们查询了少量的列(本例中为 2 列)，但是我们处理的数据量可能非常大(整整一个季度的销售事件)。按列存储数据将使这变得更容易。

# 亚马逊红移

Redshift 是分析数据库领域的另一个大玩家。这是亚马逊的分析数据库，它使用列存储(如 BigQuery)并且是完全托管的(如 BigQuery)。

# 红移光谱

红移光谱是亚马逊提供的一个非常有趣的分析解决方案。Spectrum 背后的想法是使用 S3 作为数据源，允许在其上运行 SQL 查询。为了获得与 Redshift 和 BigQuery 相当的性能，您实际上需要以列格式“手动”存储数据(通常选择 Apache Parquet 格式)。

**雅典娜呢？**
亚马逊的雅典娜在原理上给出了相同功能的红移光谱，有一些差异。
我发现 Spectrum 更有趣，因为我们已经运行了红移，它与 Spectrum 的集成比 Athena 更好。

最后我选择了 BigQuery，就这样！

![](img/e1327dec57a018a9b78adee958c2e35f.png)

Photo by [Javier Allegue Barros](https://unsplash.com/photos/C7B-ExXpOIE?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) on [Unsplash](https://unsplash.com/search/photos/choice?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

# 方便用户的

BigQuery 不使用索引:随着数据大小的增长，BigQuery 只是增加更多的服务器，这样性能几乎保持不变。在 BigQuery 中，每个查询都是一个表扫描，我认为这很好。尽管如此，性能还是惊人的好。

红移可能更有性能，**但是**你需要明智地选择主键和排序键。看到一个性能很差的红移实例是很常见的，只是因为键没有被很好地计划。

排序键用于加速查询和提高列数据库的压缩，这对您的成本和系统性能有很大的影响。

红移光谱和 Athena 将需要更多的工程工作，因为我们需要以正确的格式手动存储数据以获得良好的性能。

# 扩大规模的潜力

首先，您可以将插入数据流式传输到数据库中。在 BigQuery 中，你可以使用 stream insert，而在 Redshift 中，使用 Amazon Kinesis Data Firehose 可能是个好主意。Kinesis Firehoses 已经设置为与 Amazon storages(如 Redshift)一起工作，并不断向其写入数据，同时提供一些容错排队机制。

最终，您将生成太多的数据，无法直接流入 DBs，并且可能会遇到一些限制(成本或服务中的一些配额)

我认为 BigQuery 为这个问题提供了最简单的解决方案。

基本上，您可以决定哪些数据需要立即查询，哪些不需要，然后将它们放在两个不同的路径上。
第一个(热路径)直接流式传输到 DB，另一个存储在其他地方，然后批量插入 DB(这是 Google 云平台免费提供的操作)。

该图很好地展示了这一点，摘自 Google 博客。

红色的路径是热的，而蓝色的是冷的。

![](img/1c9fa8d088edad66ba0f58c2cb12e65c.png)

source: [https://cloud.google.com/solutions/architecture/optimized-large-scale-analytics-ingestion](https://cloud.google.com/solutions/architecture/optimized-large-scale-analytics-ingestion)

有了红移，我觉得就没那么直白了(但我可能错了)。

一种解决方案是将传入的数据存储在 DynamoDB 上，供立即需要的查询使用，稍后您可以将所有数据批量传输到红移或 S3 以获得频谱。

整个流水线的架构会是这样的(这里用 Athena 代替 Spectrum 或者 Redshift，但原理是一样的)。

![](img/8110f240559526b04d203dd00b1c5022.png)

source: [https://aws.amazon.com/blogs/database/how-to-perform-advanced-analytics-and-build-visualizations-of-your-amazon-dynamodb-data-by-using-amazon-athena/](https://aws.amazon.com/blogs/database/how-to-perform-advanced-analytics-and-build-visualizations-of-your-amazon-dynamodb-data-by-using-amazon-athena/)

# 费用

BiqQuery 是按查询和存储收费的…其他的几乎都是免费的。加载数据是免费的，但你需要支付流媒体插入费。

这意味着在某些用例中，它可能非常便宜。可能在许多分析应用程序中，您将扫描/返回大量的行，而这些行通常不是很大(数据结构方面)。

红移按节点收费。数据越多，需要的节点就越多。另一方面，亚马逊保留了实例支付计划，基本上你承诺使用服务 1 至 3 年，你可以预先支付一些钱。这可以为您节省原价的 70%。尽管如此，对于一家初创公司来说，致力于长期计划可能不是一个好主意。

红移光谱也可以很便宜，基本上，它是根据你处理的数据量收费的(有点像 BigQuery)。

结论:我们已经了解了如何在现实生活中设计一个事件摄取问题。市场上有不同的解决方案，每一个都有自己的优点和缺点。我们研究了 BigQuery 在某些用例中如何成为一个好的解决方案，并将其与 Redshift 和 Redshift Spectrum 等替代方案进行了比较。

如果您喜欢这篇文章，并且觉得它很有用，请随意👏或者分享。

干杯！