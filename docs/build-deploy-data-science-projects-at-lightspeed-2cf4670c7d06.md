# 以光速构建和部署数据科学项目

> 原文：<https://towardsdatascience.com/build-deploy-data-science-projects-at-lightspeed-2cf4670c7d06?source=collection_archive---------13----------------------->

![](img/ea57a0ab4bf2dcc28eb7ac39979c6f94.png)

“starry night” by [Quentin Kemmel](https://unsplash.com/@kouglov?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

*(或其左右)。*

## *使用 Repl.it 的云环境在几分钟内分析和共享陨石降落数据*

当今数据科学发展的最大挑战之一是快速共享、开发和部署代码。

除非你在一个数据科学和分析深深嵌入公司文化和生态系统(以及 Github)的企业中工作，否则你很可能已经体验过包含 SQL 行的电子邮件链的乐趣，无穷无尽。Slack 上的 txt 代码片段和足够多的类似命名的 R 环境引起了真正的头痛。

在这些环境中，与其他数据科学家、分析师和工程师共享代码可能是一项挑战。在某些情况下，数据仍然与传统的工程部门过于分离，这意味着这是一种乞求、借用和窃取代码模式和最佳实践的情况。

在阅读了威廉·科赫森[的](/how-to-put-fully-interactive-runnable-code-in-a-medium-post-3dce6bdf4895)[回复](http://repl.it)后，我很想尝试一下这个新平台。它允许你在云环境中快速构建、测试和共享代码。对数据人员来说，好消息是 repl.it 支持多种语言，包括 R 和 Python。

![](img/71c1485a96e088b4afc29f12ed90eb4c.png)

“silhouette photo of mountain during night time” by [Vincentiu Solomon](https://unsplash.com/@vincentiu?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

我一直在使用 [NASA 的陨石着陆数据](https://data.nasa.gov/Space-Science/Meteorite-Landings/)作为一个项目的样本数据，该项目围绕群体分析以及何时利用参数与非参数分析方法。这似乎是一个绝佳的机会，可以看看 repl.it 导入外部数据集并提供快速分析的能力如何。

虽然在这种情况下，我可以访问群体(已知的降落在地球上的陨石)，但为了项目的目的，我想对这些数据进行采样，以了解坠落和发现的陨石的质量是否不同，并分析样本是否来自正态分布的群体。

下面，你可以看到来自 repl.it 的嵌入，它导入了美国宇航局的数据集，采集了样本，提供了一个用于正态性的[夏皮罗-维尔克测试](https://en.wikipedia.org/wiki/Shapiro%E2%80%93Wilk_test)和一个箱线图可视化，以了解质量数据的分布，按坠落和发现的陨石进行划分，进一步:

如您所见，repl.it 环境可以轻松处理外部数据导入和可视化运行，即使是在介质中。我确实发现，目前 repl.it 似乎还不能处理 install.packages()和 library()功能，所以使用 ggplot 等工具进行分析目前是不允许的。然而，对于基本功能，repl.it 提供了一种简单快捷的方法来与您的团队和更广泛的数据社区共享 R 代码。而且是免费的！

## 有用的资源

[repl.it](https://repl.it)

[https://data . NASA . gov/Space-Science/陨石降落/gh4g-9sfh](https://data.nasa.gov/Space-Science/Meteorite-Landings/gh4g-9sfh)

[](https://www.statmethods.net/graphs/boxplot.html) [## Quick-R:箱线图

### 了解如何在 R 中为单个变量或按组为变量创建箱线图。

www.statmethods.net](https://www.statmethods.net/graphs/boxplot.html) [](/how-to-put-fully-interactive-runnable-code-in-a-medium-post-3dce6bdf4895) [## 如何把完全交互式的、可运行的代码放到一篇中等的文章中

### 代码应该是交互式的。

towardsdatascience.com](/how-to-put-fully-interactive-runnable-code-in-a-medium-post-3dce6bdf4895)