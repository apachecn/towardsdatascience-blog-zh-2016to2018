# 我在 2018 年探索的 10 个数据科学工具

> 原文：<https://towardsdatascience.com/10-data-science-tools-i-explored-in-2018-d8226f99f342?source=collection_archive---------9----------------------->

![](img/d5e866ab0b46507ad010dd5acbb45c76.png)

Source: [geralt](https://pixabay.com/en/users/geralt-9301/) (pixabay)

## 新的语言、库和服务

2018 年，我投入了大量时间学习和撰写关于数据科学方法和技术的文章。2018 年上半年，我写了一个关于创业公司数据科学的[博客系列](/data-science-for-startups-introduction-80d022a18aec)，我把它写成了一本书。下半年，我在 Zynga 开始了新的工作，学习了包括 PySpark 在内的许多新工具。这篇文章重点介绍了我在过去一年中探索的一些不同的图书馆和服务。

**1。Bookdown** 我使用 [Bookdown](https://bookdown.org/yihui/bookdown/) 将我的博客系列的内容转化为可出版的格式和[自行出版的书](/data-science-for-startups-blog-book-bf53f86ca4d5)。这个包使数据科学家能够将 R Markdown 文档转换成多种格式，包括 PDF、epub 和 web 文档。大多数数据科学家可能已经在使用 markdown 了，因为它用于 Github 文档，这使得这个包非常适合放在您的工具箱中。支持许多不同的内核，这意味着这个库不仅限于 R 代码。我发现用 Bookdown 写一本书比用 Latex 容易得多，我的论文就是用 Latex 写的。

[](/data-science-for-startups-blog-book-bf53f86ca4d5) [## 创业数据科学:博客->书籍

### 数据科学家写书有很多令人信服的理由。我想更好地理解新工具，而且…

towardsdatascience.com](/data-science-for-startups-blog-book-bf53f86ca4d5) 

**2。DataFlow** 我在 GCP 最喜欢的工具之一是 Cloud DataFlow，它使数据科学家能够创作在自动扩展和完全托管的环境中运行的数据管道。在过去的一年里，我写了如何使用数据流来创作[数据管道](/data-science-for-startups-data-pipelines-786f6746a59a)、[生产模型](/data-science-for-startups-model-production-b14a29b2f920)和[游戏模拟](/scaling-game-simulations-with-dataflow-172926612d50)。这是一个适合许多与 Spark 相同用例的工具，但我发现它更适合在构建流应用程序时使用。

[](/data-science-for-startups-data-pipelines-786f6746a59a) [## 创业公司的数据科学:数据管道

### 我正在进行的关于在创业公司建立数据科学学科的系列文章的第三部分。您可以找到所有……

towardsdatascience.com](/data-science-for-startups-data-pipelines-786f6746a59a) 

**3。在 Zynga，我们已经开始使用 Python 生态系统来标准化我们的分析工具。在 2018 年开始我的新角色之前，我对这种语言的接触有限，必须快速掌握新的编程语言。我写了我学习 Python 的[动机](/data-science-for-startups-r-python-2ca2cd149c5c)以及我学习 PySpark 和深度学习等新技术的愿望。我还写了我学习新数据科学语言的[方法](/learning-a-new-data-science-language-aa7656be730a)。我还在学习关于 Python 的新东西，但是现在已经对这门语言有了相当好的掌握。**

[](/learning-a-new-data-science-language-aa7656be730a) [## 学习新的数据科学语言

### 在不断变化的数据科学工具生态系统中，您经常会发现自己需要学习一门新语言，以便…

towardsdatascience.com](/learning-a-new-data-science-language-aa7656be730a) 

**4。AWS Lambda** 过去一年我一直关注的趋势之一是让数据科学家能够将模型投入生产。实现这一目标的一种可扩展方式是使用 AWS Lambda 等工具，这使得数据科学家能够在云中部署模型。使用 Lambda，您可以指定一个函数，比如将一个预测模型应用到一组输入变量，AWS 负责部署该函数，并使其具有可伸缩性和容错性。我写了一篇展示使用 AWS Lambda 部署一个分类模型[的文章。](/data-science-for-startups-model-services-2facf2dde81d)

[](/data-science-for-startups-model-services-2facf2dde81d) [## 创业数据科学:模型服务

### 我的创业数据科学系列的第二部分主要关注 Python。

towardsdatascience.com](/data-science-for-startups-model-services-2facf2dde81d) 

**5。Featuretools** 深度学习的一大创新是能够从半结构化数据(如文本文档)中自动生成特征。结构化数据集的特征工程也取得了进展，例如 [featuretools](https://github.com/Featuretools/featuretools) 库，它自动化了数据科学家在处理数据以建立预测模型时将执行的许多工作。使用此包，您可以定义数据集中不同表(实体)之间的关联，并且库会生成大量可应用于构建模型的要素。我展示了如何使用这种方法将 NHL 游戏分类为常规或季后赛。

[](/automated-feature-engineering-for-predictive-modeling-d8c9fa4e478b) [## 用于预测建模的自动化特征工程

### 使用 Python 和 FeatureTools 库预测哪些 NHL 比赛是季后赛。

towardsdatascience.com](/automated-feature-engineering-for-predictive-modeling-d8c9fa4e478b) 

**6。2018 年是我终于开始接触深度学习的一年。我最初使用 Keras 的 [R 接口](/custom-loss-functions-for-deep-learning-predicting-home-values-with-keras-for-r-532c9e098d1f)来构建深度学习模型，但后来过渡到 [Python](/data-science-for-startups-deep-learning-40d4d8af8009) 来处理这个包。由于我大部分时间都在处理结构化数据集，我还没有发现多少深度学习是最佳方法的情况，但我发现使用定制损失函数的能力非常有用。我还写了关于使用 Java 通过数据流部署 Keras 模型的文章。**

[](/data-science-for-startups-deep-learning-40d4d8af8009) [## 创业公司的数据科学:深度学习

### 我正在进行的关于在创业公司建立数据科学学科的系列文章的第十部分，第一篇文章来自…

towardsdatascience.com](/data-science-for-startups-deep-learning-40d4d8af8009) 

**7。在学习 Python 之前，Jetty 是我用 Java 建立 web 服务的首选方法。Flask 是将 Python 函数公开为 web 调用的有用工具，对于构建微服务也很有用。我写了一篇关于使用 Flask 为深度学习分类器设置 web 端点的博文。在 [GDC 2019](https://schedule.gdconf.com/session/beyond-npcs-ai-for-game-operations/863133) 上，我们将展示 Zynga 如何使用 flask 和 gunicorn 来构建供内部使用的微服务。**

[](/deploying-keras-deep-learning-models-with-flask-5da4181436a2) [## 使用 Flask 部署 Keras 深度学习模型

### 这篇文章演示了如何使用 Keras 构建的深度学习模型来设置端点以服务于预测。它…

towardsdatascience.com](/deploying-keras-deep-learning-models-with-flask-5da4181436a2) 

**8。在过去的一年里，我在 PySpark 上做了越来越多的工作，因为它可以扩展到巨大的数据集，并且一旦你熟悉了 Python，就很容易接近它。如果你想开始，我写了一篇关于 PySpark 的简介,展示了如何开始运行一些常见的任务。在简介中，我展示了如何使用 Databricks community edition 来启动和运行 Spark 环境，但我也在博客中介绍了如何在 GCP 上使用 [Dataproc](/data-science-for-startups-pyspark-1acf51e9d6ba) 来建立 Spark 集群。**

[](/data-science-for-startups-pyspark-1acf51e9d6ba) [## 创业数据科学:PySpark

### 我的创业数据科学系列的第四部分现在关注 Python。

towardsdatascience.com](/data-science-for-startups-pyspark-1acf51e9d6ba) 

**9。熊猫 UDF** 并不是所有的 Python 代码都可以直接在 PySpark 中应用，但是熊猫 UDF 使得在 Spark 中重用 Python 代码变得容易得多。通过 Pandas 用户定义函数(UDF ),您可以使用 Pandas 数据帧编写函数，并指定用于分割数据帧的分区键。结果是，一个大的 Spark 数据帧在集群中的节点之间进行分区，转换为您的函数所操作的 Pandas 数据帧，然后将结果组合回一个大的 Spark 数据帧。这意味着您可以在分布式模式下使用现有的 Python 代码。我在我的 PySpark [介绍帖子](/a-brief-introduction-to-pyspark-ff4284701873)中提供了一个熊猫 UDF 的例子。我将在 2019 年 [Spark 峰会](https://databricks.com/sparkaisummit/north-america/sessions-single-2019?id=113)上展示 Zynga 如何使用 Pandas UDFs 构建预测模型。

[](/a-brief-introduction-to-pyspark-ff4284701873) [## PySpark 简介

### PySpark 是一种很棒的语言，可以进行大规模的探索性数据分析，构建机器学习管道，以及…

towardsdatascience.com](/a-brief-introduction-to-pyspark-ff4284701873) 

**10。开放数据集** 为了写过去一年的数据科学，我不得不提供开放数据集的例子。我使用的一些数据集包括来自 [Kaggle](/automated-feature-engineering-for-predictive-modeling-d8c9fa4e478b) 、 [BigQuery](/productizing-ml-models-with-dataflow-99a224ce9f19) 和[政府](https://medium.freecodecamp.org/clustering-the-top-1-asset-analysis-in-r-6c529b382b42)数据集的例子。

[](https://medium.freecodecamp.org/clustering-the-top-1-asset-analysis-in-r-6c529b382b42) [## 聚类前 1%:R 中的资产分析

### 美国最近通过的税改法案引发了许多关于该国财富分配的问题…

medium.freecodecamp.org](https://medium.freecodecamp.org/clustering-the-top-1-asset-analysis-in-r-6c529b382b42) 

2018 年是学习新数据科学技术的伟大一年，我很高兴在新的一年继续学习。2019 年，我很期待探索强化学习，火花流，以及半结构化数据集的深度学习。

本·韦伯是 Zynga 的首席数据科学家。我们正在[招聘](https://www.zynga.com/careers/positions/categories)！