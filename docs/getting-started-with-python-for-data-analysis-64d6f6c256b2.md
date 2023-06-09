# 开始使用 Python 进行数据分析

> 原文：<https://towardsdatascience.com/getting-started-with-python-for-data-analysis-64d6f6c256b2?source=collection_archive---------4----------------------->

![](img/68f48360a67c37c90c4cdc766be4ee13.png)![](img/cfcada0cbf15234630636ad1aad4de4a.png)

*此文最初出现在*[*blog.zakjost.com*](https://blog.zakjost.com/post/python-for-data-analysis/)

最近有个朋友问了这个问题，我觉得在这里发表可能会对别人有好处。这是给刚接触 Python 的人的，他们想要从 0 到 1 的最简单的路径。

1.  下载 Python 3。适用于您的操作系统的 x 版 Anaconda 发行版[此处为](https://www.continuum.io/downloads)。通过选择这个预捆绑的发行版，您将避免许多与安装相关的麻烦。它附带了大多数预先安装的重要数据分析软件包。
2.  一旦你安装了它，测试以确保默认的 python 解释器就是你刚刚安装的那个。这一点很重要，因为您的系统可能已经安装了 Python 版本，但是它不具备 Anaconda 捆绑包中的所有优点，所以您需要确保新版本是默认版本。在苹果/Linux 上，这可能意味着在终端上输入`which python`。或者您可以运行 Python 解释器，确保版本与您下载的内容相匹配。如果一切顺利，应该通过安装来完成。如果没有，你需要在这里停下来修理它。
3.  在你的外壳中发出`jupyter notebook`命令。这将打开一个浏览器窗口。如果没有，打开浏览器，导航到`[http://localhost:8888](http://localhost:8888)`。一到那里，就创建一个新的 Python 笔记本。
4.  转至[www.kaggle.com](http://www.kaggle.com)的*果仁*部分，过滤至 [Python 果仁](https://www.kaggle.com/kernels?language=Python)。这些大多是其他人在数据集上进行分析或建模的 jupyter 笔记本，可在 Kaggle 的网站上免费获取。寻找像 EDA(探索性数据分析)这样的标题，而不是那些构建预测模型的标题。找到一个有趣的，并开始在你的笔记本中重新创建它。

> 注意:您会发现当您尝试重新创建这些分析时，您会得到导入错误。这可能是因为他们安装了 Anaconda 发行版中没有捆绑的包。你最终需要学习如何与 conda 包管理器交互，这将是你最终会陷入的许多兔子洞之一。通常就像`conda install <package_name>`一样简单，但是你需要找到正确的包名，有时你需要指定其他细节。其他时候你需要使用`pip install <other_package_name>`，但是你会在以后学到这些。

# 高级库摘要

这里有一个您将经常与之交互的重要库的快速总结。

*   NumPy:拥有许多科学计算的核心功能。在引擎盖下调用 C 编译的代码，所以比用 Python 写的相同函数要快得多。不是最用户友好的。
*   SciPy:类似于 NumPy，但是有更多的方法从分布中取样，计算测试统计…等等。
*   MatPlotLib:主绘图框架。不可避免的邪恶。
*   Seaborn:在 MatPlotLib 之后导入它，默认情况下它会让你的绘图漂亮很多。也有自己的功能，但我发现最酷的东西运行太慢。
*   熊猫:主要是一个薄薄的包装，让用户更加友好。非常适合与数据表交互，他们称之为数据框架。也有关于绘图功能的包装器，以支持快速绘图，同时避免 MPL 的复杂性。我最喜欢用熊猫来处理数据。
*   Scikit-learn:有很多监督和非监督的机器学习算法。也有许多用于模型选择的度量标准和一个很好的预处理库，用于像主成分分析或编码分类变量这样的事情。

# 快速提示

1.  在 jupyter 笔记本中，在运行单元格之前，在任何对象前面打一个问号，它将打开该对象的文档。当你忘记了你试图调用的函数期望你传递的细节时，这真的很方便。例如，`?my_dataframe.apply`将解释`pandas.DataFrame`对象的`apply`方法，这里用`my_dataframe`表示。
2.  你可能总是需要参考你正在使用的库的文档，所以只要在你的浏览器中打开它。有太多可选的论点和细微差别。
3.  当谈到不可避免的故障排除任务时，s [tackoverflow](https://stackoverflow.com/) 可能有答案。
4.  接受这样一个事实，你会做一些你暂时还不完全理解的事情，否则你会被那些不重要的细节所困扰。有一天，你可能需要了解虚拟环境，这真的不是很难，但有许多类似的弯路会给初学者增加不必要的痛苦。
5.  看别人的代码。这是学习惯例和最佳实践的最佳方式。这就是 Kaggle 内核真正发挥作用的地方。GitHub 还支持在浏览器中显示 jupyter 笔记本，因此互联网上有大量的例子。