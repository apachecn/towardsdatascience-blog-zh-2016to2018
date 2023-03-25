# 创业数据科学:简介

> 原文：<https://towardsdatascience.com/data-science-for-startups-introduction-80d022a18aec?source=collection_archive---------0----------------------->

![](img/591cbf975d3b416a9c70b264d7a9c545.png)

Source: rawpixel at pixabay.com

我最近换了行业，加入了一家初创公司，负责建立数据科学学科。当我加入时，我们已经有了一个可靠的数据管道，但我们还没有可重复分析、放大模型和执行实验的流程。这一系列博客文章的目标是概述如何为初创公司从头构建数据科学平台，提供使用谷歌云平台(GCP)的真实示例，读者可以自己尝试。

本系列面向希望超越模型培训阶段，构建对组织有影响的数据管道和数据产品的数据科学家和分析师。然而，对于那些希望更好地了解如何与数据科学家合作进行实验和构建数据产品的其他学科来说，它也可能是有用的。它面向有编程经验的读者，将包括主要用 R 和 Java 编写的代码示例。

## 为什么是数据科学？

为你的初创公司雇佣数据科学家时，首先要问的问题之一是
*数据科学将如何改进我们的产品*？在[意外之财数据](https://angel.co/windfall-data)，我们的产品是数据，因此数据科学的目标与公司的目标非常一致，即建立最准确的模型来估计净资产。在其他组织，如移动游戏公司，答案可能不那么直接，数据科学可能更有助于理解如何运营业务，而不是改进产品。然而，在这些早期阶段，开始收集关于客户行为的数据通常是有益的，这样你就可以在未来改进产品。

在初创企业中使用数据科学的一些好处包括:

1.  确定要跟踪和预测的关键业务指标
2.  构建客户行为的预测模型
3.  运行实验来测试产品变化
4.  构建支持新产品功能的数据产品

许多组织在前两三个步骤上停滞不前，没有充分利用数据科学的潜力。这一系列博客文章的目标是展示托管服务如何用于小型团队，以超越仅用于计算业务运营指标的数据管道，并过渡到数据科学为产品开发提供关键输入的组织。

## 系列概述

以下是我计划在这个博客系列中涉及的主题。当我写新的章节时，我可能会添加或移动章节。如果你觉得还有其他的话题需要讨论，请在文章的最后提供评论。

1.  **简介(这篇文章):**提供了在创业公司使用数据科学的动机，并概述了这一系列文章所涵盖的内容。类似的帖子还有数据科学的[功能](/functions-of-data-science-4afd5341a659)、[缩放数据科学](https://medium.com/windfalldata/scaling-data-science-at-windfall-55f5f23698e1)和[我的 FinTech 之旅](/from-games-to-fintech-my-ds-journey-b7169f08b6ad)。
2.  [**跟踪数据:**](/data-science-for-startups-tracking-data-4087b66952a1) 讨论了从应用程序和网页中捕获数据的动机，提出了收集跟踪数据的不同方法，介绍了隐私和欺诈等问题，并用 Google PubSub 给出了一个例子。
3.  [**数据管道:**](https://medium.com/@bgweber/data-science-for-startups-data-pipelines-786f6746a59a) 介绍了分析和数据科学团队收集数据的不同方法，讨论了平面文件、数据库和数据湖的方法，并介绍了使用 PubSub、DataFlow 和 BigQuery 的实现。类似的帖子包括[一个可扩展的分析管道](/a-simple-and-scalable-analytics-pipeline-53720b1dbd35)和[游戏分析平台的演变](/evolution-of-game-analytics-platforms-4b9efcb4a093)。
4.  [**商业智能:**](/data-science-for-startups-business-intelligence-f4a2ba728e75) 确定 ETL、自动化报告/仪表板以及计算业务运营指标和 KPI 的常见实践。用 R Shiny 和 Data Studio 给出了一个例子。
5.  [**探索性分析**](/data-science-for-startups-exploratory-data-analysis-70ac1815ddec) **:** 涵盖用于挖掘数据的常见分析，如构建直方图和累积分布函数、相关性分析以及线性模型的特征重要性。使用[出生率](https://cloud.google.com/bigquery/sample-tables)公共数据集进行示例分析。类似的帖子还有[聚类前 1%](https://medium.freecodecamp.org/clustering-the-top-1-asset-analysis-in-r-6c529b382b42) 和 [10 年数据科学可视化](/10-years-of-data-science-visualizations-af1dd8e443a7)。
6.  [**预测建模**](https://medium.com/@bgweber/data-science-for-startups-predictive-modeling-ec88ba8350e9) **:** 讨论有监督和无监督学习的方法，并介绍流失和交叉提升预测模型，以及评估离线模型性能的方法。
7.  [**模型制作**](https://medium.com/@bgweber/data-science-for-startups-model-production-b14a29b2f920) **:** 展示了如何扩大离线模型的规模，对数百万条记录进行评分，并讨论了模型部署的批处理和在线方法。类似的帖子还有[在 Twitch 制作数据科学](https://blog.twitch.tv/productizing-data-science-at-twitch-67a643fd8c44)和[用数据流制作模型](/productizing-ml-models-with-dataflow-99a224ce9f19)。
8.  **实验:**介绍产品的 A/B 测试，讨论如何建立一个实验框架来运行实验，并给出一个使用 R 和 bootstrapping 的实例分析。类似岗位包括[分阶段推出的 A/B 测试](https://blog.twitch.tv/a-b-testing-using-googles-staged-rollouts-ea860727f8b2)。
9.  **推荐系统:**介绍了推荐系统的基础知识，并提供了一个为生产系统扩展推荐器的示例。类似的帖子还有[原型推荐](/prototyping-a-recommendation-system-8e4dd4a50675)。
10.  **深度学习:**简单介绍了最适合通过深度学习解决的数据科学问题，例如将聊天消息标记为攻击性的。提供了使用 R 接口到 [Keras](https://keras.rstudio.com/) 的原型模型和使用 R 接口到 [CloudML](https://tensorflow.rstudio.com/tools/cloudml/articles/getting_started.html) 的产品化的例子。

该系列还以[网络](https://bgweber.github.io/)和[印刷](https://www.amazon.com/dp/1983057975)格式出版。

## 工具作业

在整个系列中，我将展示构建在 Google 云平台上的代码示例。我选择这个云选项，因为 GCP 提供了许多托管服务，使小团队可以建立数据管道，将预测模型产品化，并利用深度学习。还可以注册 GCP 的免费试用，并获得 300 美元的积分。这应该涵盖了本系列中介绍的大多数主题，但如果您的目标是深入研究云上的深度学习，它将很快过期。

对于编程语言，我将使用 R 编写脚本，Java 用于生产，SQL 用于处理 BigQuery 中的数据。我还将介绍其他工具，如 Shiny。推荐一些 R 和 Java 的经验，因为我不会涉及这些语言的基础。

[本·韦伯](https://www.linkedin.com/in/ben-weber-3b87482/)是游戏行业的数据科学家，曾在电子艺界、微软工作室、黎明游戏和 Twitch 任职。他还在一家金融科技初创公司担任第一位数据科学家。