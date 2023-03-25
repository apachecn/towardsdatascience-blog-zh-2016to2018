# 数据，无处不在的数据和大量的饮料(向塞缪尔·泰勒·柯尔律治道歉)

> 原文：<https://towardsdatascience.com/data-data-everywhere-and-lots-of-drops-to-drink-with-apologies-to-samuel-taylor-coleridge-3f51aab8a4fe?source=collection_archive---------3----------------------->

![](img/4675a5819bbdcecaa2429b2a8b731978.png)

Photo © Michael Factor

说这是数据时代是老生常谈；社交数据、科学数据、医疗数据、智能家居数据、汽车数据和物联网(I0T)数据都在以惊人的速度产生。我们有海量的数据。但是像古代水手一样，仅仅获得这些数据是不够的。没有适当的处理，这些数据不能解渴，不能成为信息。我们需要**将这些数据从盐水变成香槟**，从仅仅保存数据转变为使用数据来推动社会和商业价值。在这篇博文中，我描述了从数据到价值的转变的几个云构建模块。我们还将在 3 月 19 日周日上午的 IBM 互连研究日以及随后的会议上发表演讲。

许多此类数据是在传统企业数据中心之外创建的，甚至更多此类数据存储在云中。云中使用了一系列数据存储，包括 NoSQL 数据库和对象存储。这些商店是作为现成的多租户服务提供的。例如，对象存储是可伸缩的、易于使用的、对开发人员友好的、高弹性的和成本有效的存储库，适合于存储由关键字(URI)标识的中型到大型实体。 [IBM Cloud Object Storage](https://www.ibm.com/cloud-computing/products/storage/object-storage/cloud/) ，在美国跨区域部署中可用，可以经受多次物理资源中断，包括整个区域的中断，而不会丢失任何数据，并且可以持续、不间断地访问任何成功存储的数据。NoSQL 云数据存储为较小的“对象”提供了更高性能、可扩展、易于使用、开发人员友好的存储库。 [IBM Cloudant](https://cloudant.com/) 是存储为 JSON 文档的高速数据的理想解决方案。

虽然我可以诗意地讲述云数据存储的好处，尽管没有塞缪尔·柯勒律治·泰勒那样的口才，但这不是我的目的。我的目的是谈一谈从数据中获取价值。简单地将这些数据放入对象存储或 NoSQL 数据库只是故事的一部分，是故事的必要和关键部分，但远非故事的全部。除了法规约束之外，存储数据的唯一合理理由是对数据做一些事情。为了处理数据，我们需要能够将存储数据的服务和处理数据的服务结合在一起。**这篇文章是关于把存储在云数据存储中的数据海洋变成香槟**。

![](img/964a00dfafa58fd25372b28d839e03db.png)

Data data everywhere and lots of drops to drink. From Flicker. [https://www.flickr.com/photos/lejournaldemaman/7149983961](https://www.flickr.com/photos/lejournaldemaman/7149983961)

尽管仅仅存储原始数据没有多大用处，但我们确实需要将高质量的数据存储到云数据存储中，以便从中获得价值。例如，如果我们从智能家居获取读数，这些读数通常会很小。虽然更新的数据可以保存在 NoSQL 存储中以允许快速检索，但是这些记录应该被一起批量处理并存储在更具成本效益的对象存储中以用于更长期的历史目的，例如用于机器学习的训练数据。这些数据通常会通过总线发送，比如基于开源 Apache Kafka 的 IBM 消息中心。基于 IBM Research 最初在[欧盟宇宙项目](http://iot-cosmos.eu/)背景下所做的工作，这篇[博客](https://www.ibm.com/blogs/bluemix/2017/03/end-to-end-iot-data-pipelines-introducing-the-message-hub-object-storage-bridge/)描述了 IBM Message Hub 如何[桥接到对象存储](https://console.ng.bluemix.net/docs/services/MessageHub/messagehub088.html#messagehub088)。由于对象存储最适合中等规模到大型的对象，所以桥将消息分组在一起，或者持续一段时间，或者直到看到一定量的数据，并将集合存储为单个对象。从长远来看，IBM 研究小组已经展示了如何将这些消息重新格式化，例如，转换成 [Apache Parquet](https://parquet.apache.org/) 格式，并智能地用元数据进行注释。

有时，从设备发送的数据需要在存储之前进行清理或重新格式化。无服务器编程模型，如[open whish](http://openwhisk.org/)，可用于实现代理，该代理监听在消息中心总线上发布的消息，检索消息，重新格式化消息，然后重新发布一个经过清理的正确格式化的消息。例如，[这篇博文](https://medium.com/openwhisk/serverless-transformation-of-iot-data-in-motion-with-openwhisk-272e36117d6c#.40m1s0lj8)描述了如何使用[IBM open whish](https://www.ibm.com/cloud-computing/bluemix/openwhisk)对来自 [IBM Watson IoT 平台](https://www.ibm.com/internet-of-things/iot-solutions/watson-iot-platform/)并放置在 Message Hub 上的消息进行重新格式化，从而在 Message Hub 桥批处理并存储为对象之前，实现更轻松的下游处理。

但是将数据放入云商店只是故事的一部分。即使数据在对象存储中被正确地格式化、清理和组织，它仍然只是数据的海洋，除非我们能够提取价值。 [Apache Spark](http://spark.apache.org/) 正在迅速成为领先的分析平台，在许多情况下取代了以前用 Hadoop 完成的工作。IBM 的[数据科学体验](http://datascience.ibm.com/)提供了一个基于 [Jupyter](http://jupyter.org/) 笔记本的用户友好界面，通过与 IBM 的对象存储和 Spark 服务的一流集成来探索和分析数据。

但是，如果我们不能让 Spark 分析存储在对象存储中的数据，那么这些数据探索都不会起作用。这种分析的一种方法，迄今为止经常被推荐的方法，是将数据从对象存储器复制到本地 HDFS 文件系统，对 HDFS 执行分析，然后将结果复制回对象存储器。但是这种方法笨重、耗时且成本高昂，因为还需要管理和调配 HDFS。另一种方法是直接处理存储在对象存储器中的数据。Hadoop 社区通过 s3a 或 Hadoop open stack(for open stack Swift)连接器等“连接器”支持这种替代方案，这些连接器可以从 Spark 等系统桥接到对象存储。但是，在推荐的配置中使用这些连接器比直接使用 HDFS 要慢得多。为了解决这个问题，我们开发了我们自己的连接器 [Stocator](https://developer.ibm.com/open/openprojects/stocator/) ，它显著地[提高了性能](http://www.spark.tc/stocator-the-fast-lane-connecting-object-stores-to-spark/)，特别是对于写密集型工作负载。Stocator 内置于 IBM 的 Spark 服务中，它的使用对于任何使用 Spark 来分析对象存储中的数据的人来说都是透明的。

虽然通常对存储在对象存储中的历史数据进行分析就足够了，例如，训练机器学习模型，但有时我们需要能够查看更近的数据，例如存储在 NoSQL 数据库(如 Cloudant)中的数据，以及作为单个数据集的历史数据。在这种情况下，我们一直致力于对存储在 Cloudant 和 IBM Cloud Object Storage 以及其他数据存储中的数据进行透明分析。简而言之，我们为 Spark 定义了一个新的外部数据源驱动程序，它允许访问多个数据存储，跟踪每个存储中包含的时间范围，并从正确的存储中检索正确的数据。

如果您有兴趣了解更多关于这些主题的信息，我将在 3 月 19 日(星期日)上午 8:50 与我在 IBM InterConnect 的同事 Dalit Naor 进行一次演讲。达利特和宝拉·塔什玛将在 3 月 19 日下午 1 点演示其中一些场景。此外，在 3 月 21 日星期二，我将与 IBM 在云存储和分析方面的技术领导者一起主持一个关于“云存储和分析:一个充满机会的世界(为准备充分的人)”的小组讨论最后，3 月 22 日星期三下午 1 点，葆拉和安德鲁·施菲尔德将共同介绍 IBM Message Hub 的一些新进展。