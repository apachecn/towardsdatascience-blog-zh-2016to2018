# 基于实时 Twitter 数据的复杂事件处理

> 原文：<https://towardsdatascience.com/complex-event-processing-with-flink-on-realtime-twitter-data-d09d9953df1b?source=collection_archive---------3----------------------->

![](img/67b4ed89aa96b2c1945abff6f262211b.png)

最近，我决定研究 Flink 的复杂事件处理库(CEP ),看看它有什么能力。我发现它非常容易使用，我可以想象出许多可能的用例。

**什么是复杂事件处理？**在开始讨论如何进行 CEP 之前，我认为首先有必要对其进行定义，并让数据科学家和工程师了解它为什么有用。复杂事件处理对于检测流数据中的模式以及基于这些模式发送警报或通知非常有用。任何处理需要监控的时间关键型数据的人都应该知道如何使用 CEP。例如，在 [Flink 网站](http://flink.apache.org/news/2016/04/06/cep-monitoring.html)上，他们详细介绍了如何使用复杂的事件处理来监控服务器机架温度，并在同一机架连续两次超过温度阈值时发出警告。然后，他们描述了当你有两个“连续警告”时，你如何创建一个警报。这个例子很好，但是有点过时，我想自己用 Twitter 数据测试一下。

**入门:**

在我之前关于 Flink 的文章中，我描述了如何建立一个 Tweets 流，并对 Tweets 中最常见的单词进行基本的字数统计。现在回想一下，我们有一个(word，count)格式的单词数据流，我称之为 dataWindowKafka(因为我们之前直接将其提供给 Kafka 制作人)。这是我们在这个例子中主要要做的事情。

现在，假设我们有兴趣了解某个特定单词在一个窗口中被提及的次数是否超过了设定的数量，而在第二个窗口中被提及的次数是否超过了设定的数量。首先，让我们扩展 SimpleCondition，以便不像在文档中那样以内联方式编写代码。

现在我们有了一个易于使用的类，让我们写出实际的 CEP 代码。

在生产中，您可能会将该流的输出传递给 Kafka 或某个数据库，但出于我们的目的，我们将只是将它作为基本的 println 写入控制台。

```
System.*out*.println(manyMentions.writeAsText("alert.txt"));
// Output{increasing=[(trump,137)], first=[(trump,143)]}
{increasing=[(trump s,49)], first=[(trump s,35)]}
{increasing=[(  ,42)], first=[(  ,29)]}
{increasing=[(i m,11)], first=[(i m,21)]}
```

我们显然有一些符号化的问题，因为 trump 和“trump 的”本质上是同一个词，空格也不应包括在内，但 CEP 本身似乎在做自己的工作。同时满足第一个过滤器“first”和第二个过滤器“increasing”。虽然如图所示，第二个并不总是实际增加。原因是因为该值是静态的，所以只要秒大于 20，它就会返回 true，即使先前的值是 42。为此我们需要迭代条件。

**迭代条件:**根据 Flink 文档，它们是:

> 最常见的情况。这就是您如何根据先前接受的事件的属性或它们子集的统计信息来指定接受后续事件的条件。

所以，假设我们想确保我们发布这些推文的频率确实在增加。这里的主要区别是，布尔函数将执行 current_event>previous_event，而不是使用固定值。我们可以使用迭代条件来实现。

I will add the output of this final event soon.

## **更复杂的例子**

当然，这个例子仅仅触及了 CEP 的表面。人们可以立即想象出更多的用例。例如，一家想要监控其品牌声誉的公司可能会通过将 CEP 与情绪分析结合使用来检测可能的抵制(以便快速做出响应)。因此，该公司可以使用机器学习算法来检测和标记每条提到他们品牌的推文，带有积极或消极情绪，然后如果消极情绪超过某个阈值和/或积极情绪的数量，他们就会发出警报。

另一个例子可能是想要向用户推荐电影或产品的在线电影或购物网站。在这种情况下，他们可以查看连续的日志数据，并且如果用户在给定的时间跨度内访问了电影/项目的某个序列，则推荐另一个相关的电影/项目。

## **其他 Flink CEP 资源**

 [## Apache Flink:使用 Apache Flink 引入复杂事件处理(CEP)

### 复杂事件处理(CEP)正好解决了这一问题，即把连续输入的事件与一个…

flink.apache.org](http://flink.apache.org/news/2016/04/06/cep-monitoring.html) [](https://ci.apache.org/projects/flink/flink-docs-release-1.4/dev/libs/cep.html) [## Apache Flink 1.4-快照文档:Flink CEP-Flink 的复杂事件处理

### FlinkCEP 是在 Flink 上实现的复杂事件处理(CEP)库。它允许您检测事件…

ci.apache.org](https://ci.apache.org/projects/flink/flink-docs-release-1.4/dev/libs/cep.html) [](https://berlin.flink-forward.org/kb_sessions/looking-for-patterns-with-flink-cep-library/) [## 用 Flink CEP 库寻找模式| Flink forward | 2017 年 9 月 11-13 日|柏林

### Flink 提供复杂事件处理(CEP)库已经有一段时间了。它满足了许多人的需求…

berlin.flink-forward.org](https://berlin.flink-forward.org/kb_sessions/looking-for-patterns-with-flink-cep-library/) [](https://github.com/dataArtisans/flink-training-exercises/blob/master/src/main/java/com/dataartisans/flinktraining/exercises/datastream_java/cep/LongRides.java) [## 数据工匠/flink-培训-练习

### 在 GitHub 上创建一个帐户，为 flink-training-exercises 的开发做出贡献。

github.com](https://github.com/dataArtisans/flink-training-exercises/blob/master/src/main/java/com/dataartisans/flinktraining/exercises/datastream_java/cep/LongRides.java)