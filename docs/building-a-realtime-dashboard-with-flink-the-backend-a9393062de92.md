# 用 Flink 构建实时仪表板:后端

> 原文：<https://towardsdatascience.com/building-a-realtime-dashboard-with-flink-the-backend-a9393062de92?source=collection_archive---------1----------------------->

随着对“实时”低延迟数据需求的增长，更多的数据科学家可能必须熟悉流。一个很好的起点是 Apache Flink。Flink 是一个分布式流框架，专门为实时数据分析而构建。与 Spark 的批处理运行不同(甚至 Spark“流”在技术上也是微批处理)，Flink 是建立在流模型上的(Spark vs. Flink 是一个很长的讨论，我在这里就不赘述了)。Apache Flink 可以处理极低延迟的高吞吐量数据。因此，Apache Flink 的一个有趣用例是创建一个实时数据分析仪表板。

为了简单起见，我选择使用 Twitter streaming API 作为我们的数据源，因为其他数据源通常需要反序列化模式，这使事情变得更加复杂。在第一篇文章中，我将向您展示如何用 Flink 预处理 Tweets，并执行一些基本的聚合/过滤。然后在第二篇文章中，我将向您展示如何用 D3.js 前端连接它，以制作一个实时仪表板。由于有大量代码，我决定只包括最相关的摘录，但您可以在文章底部找到包含所有代码的 GitHub 资源库。

为了进行设置，我们定义了基本的登录凭证(您需要一个 Twitter 开发人员帐户)，然后设置执行环境。关于这个的更多细节你可以从 Flink 上看到[官方示例代码](https://github.com/apache/flink/blob/master/flink-examples/flink-examples-streaming/src/main/java/org/apache/flink/streaming/examples/twitter/TwitterExample.java)。唯一的例外是，这里我们将使用 Filter.java 的[类作为 FilterEndpoint。这将允许我们只获得包含特定单词的推文。你可以通过声明`Filter.FilterEndPoint i = new Filter(wordsList,locationsList,null);`然后声明`twitterSourceA.setCustomEndPointInitializer(i);`来实现。其中，wordsList 是要包含的单词列表，twitterSourceA 是用您的凭据声明的 TwitterSource 对象(如示例所示)。您还可以根据位置或用户 id(空值)进行筛选。](https://github.com/isaacmg/twitterStream/blob/master/src/main/java/Filter.java)

Apache Flink 遵循与 Spark 和 MapReduce 相同的函数式编程方法。您有三个主要功能来处理地图、平面地图和过滤器。首先，我们将执行一个平面图，以获取相关信息。这一部分与示例非常相似，除了我还包含了一个自定义标记器(即 tokenize ),以消除点号和数字。平面图的代码如下:

This is our flatMap function that performs a basic word count on the tweets. The function uses a second function getField to get the JSON data. Then it tokenizes it, removes the punctation, and collects the word with a count of one.

接下来我们要过滤掉停用词。为此定义一个过滤器既快速又简单；我们简单地定义一个新的过滤函数，然后做`tweets.filter(aFilterFunction)`见下面的全部细节。接下来，我们定义一个 10 秒的时间窗口。时间窗口允许我们查看一个单词在十秒钟的窗口中出现了多少次。然后，这个流将被输入 sink，sink 将每十秒钟更新一次我们的图表(第二步)。

Filtering and windowing functions.

此外，我选择包含最后一个过滤器，以便过滤掉在窗口中出现不到十次的单词。现在我们有了一个合适的流，可以输入到我们的数据库中，并用来直接更新我们的图表。然而，关于数据的存储和显示，仍然有许多设计选择要做。

在本系列的第二部分中，我将讨论如何将新的数据流连接到我们的前端，以便创建实时更新的图表以及一些用于处理实时流和实现持久性的架构结构。

完整的代码可在以下链接。

[](https://github.com/isaacmg/twitterStream) [## isaacmg/twitterStream

### Twitter stream——使用 Apache Flink 传输特定的推文

github.com](https://github.com/isaacmg/twitterStream)