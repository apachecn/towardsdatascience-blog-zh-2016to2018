# 十月版:无监督学习

> 原文：<https://towardsdatascience.com/october-edition-unsupervised-learning-14def1fbe11b?source=collection_archive---------8----------------------->

## 8 篇必读文章

![](img/f388d1a03bbeb5de5ee793abecc603af.png)

[Pixabay.com](https://pixabay.com/photos/boat-from-above-top-view-shark-2435056/)

无监督学习由数据科学家和其他数据从业者使用，在这个机器学习算法家族中，没有已知的输出或标签来指示学习算法。在聚类中使用了不同类型的无监督学习，例如 K-Means 聚类、文本挖掘和维数减少，例如在预处理阶段使用的主成分分析，以转换用于监督学习机器学习建模的数据集。

最近，航空公司可以使用聚类算法来分析客户反馈调查，保险公司已经审查了他们的客户投诉登记册，选举 Twitter feeds 已经分组在一起，以监控全天的关键讨论主题，分析新闻文档的语料库，并进行主题建模以提取关键主题。

聚类根据相似性将数据划分为不同的组。算法的类型是多种多样的，包括基于距离连接对象的分层聚类和将聚类表示为单个均值向量的 K 均值聚类。

使用无监督学习的挑战是确定它是否“表现良好”，因为输出的标签是未知的。无监督学习的好处包括能够在探索性数据分析阶段对数据进行预处理，以便对训练和测试数据进行缩放。因此，转换后的数据允许数据科学家在监督学习(如岭回归)之前，使用主成分分析来可视化数据的方向并减少维度或特征的数量。

我们希望你会喜欢并在这个月学到一些新的东西，因为我们通过编辑从《走向数据科学》中精心挑选的内容来探索无监督学习。

[Wendy Wong](https://medium.com/u/68b80db0d4ab?source=post_page-----14def1fbe11b--------------------------------) ，TDS 编辑。

## [用 Python 进行无监督学习](/unsupervised-learning-with-python-173c51dc7f03)

由[维哈尔鞍马](https://medium.com/u/4ce24554e1d2?source=post_page-----14def1fbe11b--------------------------------) — 7 分钟阅读。

无监督学习是一类机器学习技术，用于发现数据中的模式。提供给无监督算法的数据没有标记，这意味着只提供输入变量(X ),没有相应的输出变量。在无监督学习中，算法自己去发现数据中有趣的结构。

## [使用无监督学习来计划去巴黎的假期:地理位置聚类](/using-unsupervised-learning-to-plan-a-paris-vacation-geo-location-clustering-d0337b4210de)

通过 [Hamza Bendemra](https://medium.com/u/1ede83301f25?source=post_page-----14def1fbe11b--------------------------------) — 6 分钟阅读

因为我自己也去过巴黎几次，我想我可以在其他方面有所帮助，比如为旅游景点列表做贡献。在列出所有的景点之后，我创建了一个谷歌地图，每个位置都有一个图钉。

## [现实生活中的 K-Means:聚类锻炼时段](/k-means-in-real-life-clustering-workout-sessions-119946f9e8dd)

By [卡罗莱纳便当](https://medium.com/u/e960c0367546?source=post_page-----14def1fbe11b--------------------------------) — 6 分钟阅读

K-means 聚类是一种非常流行的无监督学习算法。在这篇文章中，我想提供一些关于它的背景知识，并展示我们如何在真实生活中使用它。

## 我们编织了一张多么解开的网:VAE 的表征学习

由科迪·玛丽·怀尔德 — 15 分钟阅读

无监督学习的一个常见策略是生成模型，其思想是:你应该给模型一个从给定分布中产生样本的任务，因为在该任务中表现良好需要模型隐式地学习该分布。

## [对抗性自动编码器的向导指南](/a-wizards-guide-to-adversarial-autoencoders-part-1-autoencoder-d9a5f8795af4)

通过 [Naresh Nagabushan](https://medium.com/u/b77d13fadf3d?source=post_page-----14def1fbe11b--------------------------------) — 9 分钟读取

我们知道，卷积神经网络(CNN)或在某些情况下密集的全连接层(MLP——一些人喜欢称之为多层感知器)可以用来执行图像识别。但是，CNN(或 MLP)不能单独用来执行像从图像中分离内容和风格这样的任务…

## [在 SELU 自动编码器(不是另一个 MNIST)上高斯混合模型的无监督学习](/unsupervised-learning-of-gaussian-mixture-models-on-a-selu-auto-encoder-not-another-mnist-11fceccc227e)

由[gona lo Abreu](https://medium.com/u/51eeba34db4d?source=post_page-----14def1fbe11b--------------------------------)—5 分钟读取

大多数聚类方法都受到维数灾难的困扰。这样，为了执行无监督学习，降维方法是必要的。

## [使用数据、聚类和可视化发现我的 Spotify 音乐的相似之处](/discovering-similarities-across-my-spotify-music-using-data-clustering-and-visualization-52b58e6f547b)

由[胡安·德·迪奥斯·桑多斯](https://medium.com/u/9b9998a144da?source=post_page-----14def1fbe11b--------------------------------) — 16 分钟阅读

音乐品味是一个人非常独特、奇特和有特色的品质。在现存的数以百万计的歌曲和声音中，我认为许多人决定发展对一种特定风格、流派或音乐“子集”的喜爱并非偶然。

## [自然语言处理从业者指南](/a-practitioners-guide-to-natural-language-processing-part-i-processing-understanding-text-9f4abfd13e72)

由[迪潘詹(DJ)萨卡尔](https://medium.com/u/6278d12b0682?source=post_page-----14def1fbe11b--------------------------------) — 31 分钟读取

非结构化数据，尤其是文本、图像和视频包含了丰富的信息。然而，由于处理和分析这些数据的固有复杂性，人们通常不会花费额外的时间和精力从结构化数据集冒险出来分析这些非结构化数据源，这可能是一座潜在的金矿。