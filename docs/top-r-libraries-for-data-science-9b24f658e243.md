# 数据科学的顶级 R 库

> 原文：<https://towardsdatascience.com/top-r-libraries-for-data-science-9b24f658e243?source=collection_archive---------5----------------------->

![](img/c779cc6c0df9715b46b47c99330086be.png)

courtresy: [https://pixabay.com/en/statistic-survey-website-template-1606951/](https://pixabay.com/en/statistic-survey-website-template-1606951/)

在这里，让我告诉你一些关于 R 的一些很棒的库。我认为这些库是数据科学的顶级库。这些库具有广泛的功能，对于数据科学操作非常有用。我曾经使用过它们，并且仍然在日常的数据科学操作中使用它们。不浪费任何时间，让我带你开始可怕的 R 的东西。

这里提到的这些库是随机排列的，我不想给它们排名，因为它们都有自己的用处，对它们进行排名是不合理的。

**1。Dplyr**

**Dplyr** 主要用于 r 中的数据操作，Dplyr 实际上是围绕这 5 个函数构建的。这些函数构成了大部分的数据操作。您可以使用本地数据框和远程数据库表。您可能需要:

***选择*** 某列数据。

***过滤*** 您的数据以选择特定的行。

***将*** 您的数据行排列成一个顺序。

***改变*** 您的数据框以包含新列。

***以某种方式总结*** 你的大块数据。

它还具有类似于****的功能，由*** 和 ***管组成。****

***2。** **Ggplot2***

*Ggplot2 是 r 中最好的数据可视化库之一。ggplot2 库实现了一种“图形语法”(Wilkinson，2005)。这种方法通过表达数据属性和它们的图形表示之间的关系，为我们提供了一种产生可视化效果的一致方法。Ggplot2 具有广泛的功能。*

*阅读这个 R 文档了解 ggplot2 函数，点击这里:[https://bit.ly/2DBo0VK](https://bit.ly/2DBo0VK)*

***3。****Esquisse——我最喜欢的包，对 r 最好的补充***

*不喜欢 ggplot2？使用 ggplot2 和它的功能有一些问题，那么这个包是给你的。这个包将 Tableau 最重要的特性带到了 r 中。只需拖放，几分钟就可以完成可视化。这实际上是对 ggplot2 的增强。此加载项允许您通过使用 ggplot2 包可视化数据来交互式地浏览数据。它允许您绘制条形图、曲线、散点图、直方图，然后导出图表或检索生成图表的代码。太棒了，不是吗？*

***4。生物导体***

*当你进入数据科学领域时，你会处理不同种类的数据。你可能不知道将来要处理什么样的数据。如果你在健康行业，那么相信我，你会发现这非常有用。我认为这个库在你研究基因组数据时非常有用。Bioconductor 是一个开源项目，托管了大量使用 r 分析生物数据的工具。要安装 Bioconductor 软件包，您需要安装*bio manager。**

*   *图形:geneplotter，hexbin。*
*   *注释:annotate，ann builder【https://bit.ly/2OUYdd5 *
*   ***5。闪亮***
*   *这是 r 中一个非常有名的包。当你想与你周围的人分享你的东西，让他们更容易理解和直观地探索它时，你可以使用 shiny。它是数据科学家最好的朋友。Shiny 让构建交互式网络应用变得更加容易。你可以在网页上托管独立的应用程序，或者将它们嵌入到 [R Markdown](http://rmarkdown.rstudio.com/) 文档中，或者构建[仪表盘](http://rstudio.github.io/shinydashboard/)。你也可以用 [CSS 主题](http://rstudio.github.io/shinythemes/)、 [htmlwidgets](http://www.htmlwidgets.org/) 和 JavaScript [动作](https://github.com/daattali/shinyjs/blob/master/README.md)来扩展你闪亮的应用。*
*   ***6。润滑剂***
*   *这个图书馆非常适合它的用途。主要用于数据角力。它使 r 中的日期-时间处理变得更加容易。使用这个库，您可以做任何您想做的关于日期算法的事情，尽管在这里理解和使用可用的功能可能有些复杂。当你在分析时间序列数据并想按月汇总数据时，你可以使用 lubridate 包中的 *floor_date* ，它可以很容易地完成你的工作。它具有广泛的功能。你可以在这里阅读文档:[https://bit.ly/2AbTEpf](https://bit.ly/2AbTEpf)*
*   *7。针织机*

*这个包用于 R 中的动态报告生成。knitr 的目的是允许通过有文化的编程方式在 R 中进行可再现的研究。这个包还支持将 R 代码集成到 LaTeX、Markdown、LyX、HTML、AsciiDoc 和 reStructuredText 文档中。您可以将 R 添加到 markdown 文档中，并轻松生成 HTML、Word 和其他格式的报告。如果您对可重复的研究感兴趣，并且对从数据分析到报告创建的自动化之旅感兴趣，这是一个必备工具。*

***8。Mlr***

*这个包在执行机器学习任务方面绝对不可思议。它几乎拥有执行机器学习任务的所有重要和有用的算法。它也可以被称为分类、回归、聚类、多分类和生存分析的可扩展框架。它还具有用于特性选择的过滤器和包装器方法。另一件事是，这里执行的大多数操作都可以并行化。文档中提到了广泛的功能:【https://bit.ly/2QflLy6】T2*

***9。昆达。*词典****

*这个包扩展了 ***quanteda*** 包的功能。它由用于文本分析的词典组成。它主要用于 quanteda，但也可以用于其他文本分析库，如 tm、tidytext 和 udpipe。通过 quanteda.dictionaries 包中的 *liwcalike()* 函数，您可以使用现有的或定制的词典轻松地分析文本语料库。你可以从他们的 github 页面安装这个包。*

***10。DT***

*它是 javascript 库 ***数据表的包装器。*** 用于数据显示，可以将 R 矩阵和数据框显示为交互式 HTML 表格。使用这个库，你可以用最少的代码创建一个可排序的表，实际上你可以只用一行代码创建一个可排序的、可搜索的表。你也可以设计你的桌子。数据表还在表中提供过滤、分页、排序和许多其他功能。*

*11。罗勒*

*RCrawler 是一个贡献的 R 包，用于基于域的 web 爬行和内容抓取。它增加了 ***Rvest*** 包所缺少的爬行功能。RCrawler 可以抓取、解析、存储页面、提取内容并生成可直接用于 web 内容挖掘应用程序的数据。爬行操作的过程是由几个并发进程或节点并行执行的，因此建议使用 64 位版本的 r。*

*12。插入符号*

*Caret 代表分类和回归训练。软件包中的主要工具之一是培训功能，可用于。使用重新采样评估模型调整参数对性能的影响。Caret 有几个功能，试图简化模型构建和评估过程，以及特征选择和其他技术。你只需要知道这个包就可以解决几乎所有有监督的机器学习问题。它为几种机器学习算法提供了统一的接口，并标准化了各种其他任务，如数据分割、预处理、特征选择、变量重要性估计等。*

*13。RMarkdown*

*R Markdown 允许您创建文档，作为您的分析的整洁记录。在可重复研究的世界里，我们希望其他研究人员能够容易地理解我们在分析中做了什么，否则没人能确定你正确地分析了你的数据。R Markdown 是 Markdown 的一个变种，它嵌入了 R 代码块，与***knitter***一起使用，可以轻松创建可复制的基于 web 的报告。使用 **R Markdown，您可以将您的分析转化为高质量的文档、报告、演示文稿和仪表盘。***

***14。传单***

*这是一个 javascript 库，主要用于绘制和处理交互式地图。而且，你可以从 R 控制台直接使用这些地图。传单为您提供了不同的功能集，可用于样式和自定义您的地图。这个库的开发工作也很广泛。如果你想使用地图，一定要试试这个库。除了基本地图之外，您还可以为地图使用不同的单幅图块。*

***15。看门人***

*看门人使基本的数据清理变得容易，例如通过多个列找到重复项，使 R 友好的列名和删除空列。它也有一些很好的制表工具，比如添加一个总计行，以及生成带有百分比和简单的交叉标签的表格。而且，它的 get_dupes()函数是一种在数据框中查找重复行的优雅方式，可以基于一列、几列或整行。*

****其他值得一提的 R 库:****

*Ggvis*

*Plotly*

*Rcharts*

*Rbokeh*

*金雀花*

*StringR*

1.  *马格里特*
2.  *Slidify*
3.  *Rvest*
4.  *将来的*
5.  *RMySQL*
6.  *红柱石*
7.  *先知*
8.  *Glmnet*
9.  *Text2Vec*
10.  *雪球 c*
11.  *Quantmod*
12.  *Rstan*
13.  *漩涡*
14.  *数据科学家*
15.  *如果我错过了任何重要的库，请在下面的评论区告诉我。*
16.  *所以，这就是，这些是你需要知道的一些顶级库，以便完成你的日常数据科学操作。*
17.  ****表现出一些爱心，如果有帮助的话！****
18.  ***感谢阅读！***
19.  *Swirl*
20.  *DataScienceR*

> *If I’ve missed out any important library then do let me know down below in the comments section.*

*So, This is it, these were some of the top libraries that you need to know in order to get your day to day Data Science operations done.*

> ****Show some love, if it was helpful!****

***Thanks for reading!***