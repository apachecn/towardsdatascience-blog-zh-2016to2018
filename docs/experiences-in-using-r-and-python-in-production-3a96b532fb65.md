# 在生产中使用 R 和 Python 的经验

> 原文：<https://towardsdatascience.com/experiences-in-using-r-and-python-in-production-3a96b532fb65?source=collection_archive---------9----------------------->

从 2016 年 5 月开始，重新分享我们伟大的经历，开始我的新媒体博客。

![](img/a219317be2aaf47bb3e7a90d12f83145.png)

*Python 和 R 是一些最好的数据科学开源工具。它们可以很容易地用于脚本和自定义分析，但作为在线软件的一部分自动运行它们需要更多的考虑。在 Smartly.io，我们已经广泛地使用了这两个工具。在这篇博文中，我将分享我们在生产中集成它们的一些经验。*

作为背景，我们需要统计计算的第一个用例是[预测预算分配](https://www.smartly.io/blog/optimizing-conversions-with-predictive-budget-allocation)。它自动在多个广告集之间重新分配预算，以优化营销活动中的每次行动总成本。它每天都会自动运行大量的活动。

为了将统计分析与我们在线平台的其余部分分开，我们创建了一个新的[微服务](https://en.wikipedia.org/wiki/Microservices)。我们想把统计软件用在最合适的地方:做数学运算——并在其他地方做剩下的工作。基本上，服务获得它需要的所有输入数据并给出一个输出。在我们的例子中，输入数据包含所有必要的历史数据(印象、转化、花费、预算等。)并给出新的预算作为输出。下面是一个用 [JSON](https://en.wikipedia.org/wiki/JSON) 格式输入输出的简化例子。

`Input:
{
"d": [
{
"id": "a",
"impressions": 100,
"conversions": 5,
"budget": 50,
"spent": 50
},
{
"id": "b",
"impressions": 100,
"conversions": 6,
"budget": 50,
"spent": 50
}
]
}`

`Output:
[
{
"id": "a"
"budget_prop": 46,
},
{
"id": "b"
"budget_prop": 54,
}
]`

最初，我们通过使用 R 创建了微服务。有一些 R 包将自定义 R 库包装到 HTTP API 中，特别是至少有 [DeployR](http://deployr.revolutionanalytics.com/) 和 [OpenCPU](https://www.opencpu.org/) 。简而言之，这些服务允许您通过 HTTP 调用 R 函数。一年前，我们评估了备选方案，选择了 OpenCPU 作为我们的案例。我们自己没有尝试其他替代方案，所以我们的实际操作体验仅限于 OpenCPU。

在 Linux 上安装 OpenCPU 非常简单。要使用 OpenCPU，您必须创建一个自定义的 R 库。RStudio 有一个好的[文档](https://support.rstudio.com/hc/en-us/articles/200486488-Developing-Packages-with-RStudio)开始。在 R 中安装了库之后，它将连接到 OpenCPU 服务器，因此您可以通过 HTTP 调用开始调用 R 函数。

例如，在我们的例子中，我们有一个名为`BudgetAllocation(d)`的 R 函数，它在数据帧`d`中获取输入数据。该功能在我们的内部包`smartlybuster`中提供。使用 OpenCPU，我们能够通过 http://localhost:8080/ocpu/library/smartlybuster/R/BudgetAllocation/json 调用 BudgetAllocation 函数，将上述 JSON 作为输入，并获得函数的输出作为响应。

我们在 OpenCPU 上使用 R 大约半年，没有出现任何问题。我们的 R 代码库开始增长，最终我们意识到 Python 更适合我们的目的，因为它是一种更合适的编程语言。最初，我们从 R 代码到 Python 进行了几乎一对一的转换，并在大约一周内完成。对我们的 R 库进行适当的单元测试对转换帮助很大。

对于 Python，我们最终使用了 [Flask](http://flask.pocoo.org/) ，因为它为 Python 提供了类似于 OpenCPU 为 r 提供的 HTTP API。由于我们的 BudgetAllocation 是一个独立的微服务，我们可以很容易地将旧的 R 服务切换到新的 Python 服务。在产品化阶段，我们同时运行这两个程序，以评估结果是否匹配。

从 R 到 Python 的过渡相当简单。通过使用 [pandas](http://pandas.pydata.org/) 库，我们得到了与 r 中相似的数据帧。最大的惊喜之一来自 pandas 如何处理数据帧中的子集和索引。在 pandas 中，获取行的子集会返回一个数据帧，其中的行以它们原来的行号命名。这可以通过一个 reset_index()调用来解决，该调用允许您使用 d.loc[i，' budget']类型的访问权限再次访问数据。

Python，pandas 和 Flask 在迁移后对我们来说都很好用。有些部分将受益于以适当的 Python 方式重写。总而言之，我们认为 R 和 Python 都是非常适合的工具，在开发中很容易运行。Python 是一种更适合开发的编程语言，而 R 有更好的统计库。

在未来，我们看到我们可能会同时使用两者。Python 将作为处理大多数情况的主要工具。r 可用于某些特定的统计部分。为了进行这种集成，我们可以继续使用 OpenCPU 从 Python 调用 R，或者切换到特定的集成库，如 [rpy2](http://rpy2.bitbucket.org/) 。

*原载于*[*www . smartly . io*](https://www.smartly.io/blog/experiences-in-using-r-and-python-in-production)*。*