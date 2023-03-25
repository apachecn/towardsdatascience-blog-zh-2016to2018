# 购买者自负，计算社会科学

> 原文：<https://towardsdatascience.com/caveat-emptor-computational-social-science-8c2c2d5a7cc5?source=collection_archive---------15----------------------->

## 广泛发布的 Reddit 数据集中大规模缺失的数据可能会破坏你的研究质量

随着研究人员利用大型数据集大规模研究复杂的社会行为，这种计算社会科学的有效性取决于数据的完整性。这就是为什么研究人员有责任检查数据集，而不是凭信念假设它们的质量。

![](img/68f07ad1f00146f4dcf7fff217b1ed03.png)

[Fragments of Plato’s *Phaedrus*](https://commons.wikimedia.org/wiki/File:Papyrus_of_Plato_Phaedrus.jpg) found in [Oxyrhynchus](https://en.wikipedia.org/wiki/Oxyrhynchus_Papyri). Datasets used by computational social scientists are often just as fragmented, though gaps may not be as obvious.

2015 年 7 月 2 日，[杰森·鲍姆加特纳](https://twitter.com/jasonbaumgartne)发布了一个数据集，宣称包含“所有公开的 Reddit 评论”，这个数据集很快在 Bittorrent 和互联网档案馆上被分享。这些数据很快成为许多学术论文的基础，这些论文的主题包括机器学习、社会行为、政治、突发新闻和仇恨言论。在最近发表的一篇由[内森·马蒂亚斯](http://natematias.com/)撰写的文章的预印本中，我们发现了该数据集中的实质性差距和局限性，这可能会导致该研究结果的偏差。

d .加夫尼、J. N .马蒂亚斯(2018)。 [**购者自负，计算社会科学家:大规模数据缺失在广泛出版的 Reddit 文集**](http://journals.plos.org/plosone/article?id=10.1371/journal.pone.0200162) **。**[**arXiv:1803.05046**](https://arxiv.org/abs/1803.05046)**【cs。SI]**

在我们的论文中，我们记录了数据集、数据集中大量缺失的观察值，以及这些缺口对研究有效性带来的风险。总的来说，我们发现 2015 年 1 月 23 日至 2017 年 6 月 30 日之间可能有 3600 万条评论和 2800 万条提交内容丢失的证据，这些证据以目前不可预测的方式分散在整个数据集中。我们还概述了计算社会科学中从缺失数据到常用方法的风险:

**强风险:**

*   用户历史记录
*   网络分析

**中度风险:**

*   社区之间的比较
*   参与水平

**低，但真实的风险:**

*   机器学习和任何其他避免对人、社区或行为做出断言的研究

我们相信使用这个数据集的论文总数，无论是已经发表的还是正在进行的，都在几十篇以内。我们的论文直接引用了我们知道的 15 篇论文来使用这些数据(我们预计我们遗漏了一些)。自数据集发表以来，这些工作跨越了近两年的学术时间，代表了广泛多样的研究目标。

# 确定 Reddit 数据集中的差距

我们对缺失数据的分析始于在数据集中找到容易识别的不可能:一些评论是对语料库中不存在的帖子的回应。这些“悬空引用”是引用帖子或其他评论的评论(因为参与 Reddit 评论线程是嵌套评论的“树”),这些评论在数据集中是缺失的。我们还绘制了“未知的未知”，这是“应该”存在于 Reddit 数据集中，但在语料库中找不到的 id。Reddit 采用了一个基数为 26 的 ID 系统，该系统对于平台上的所有内容都是单调递增的——第一条评论是评论 ID #1，第二条是评论 ID # 2，依此类推。

基于这些知识，我们浏览了提交和评论的 ID 空间。遗漏的评论在子 reddit 和时间上分布不均匀，这种不均匀的误差分布(到目前为止)不能简单地用 Reddit 在特定时间点的大小或子 Reddit 的大小来解释。

![](img/da93cca5234cc96166f0790a28c46464.png)

Missing data rates for Reddit comments to June 2017 — medium blue is total missing per month, light blue is a rolling average missing amount, and dark blue is the cumulative error rate throughout the dataset. That the dark blue line is reliably linear after 2013 indicates a chronic portion of missing data.

基于我们的研究，杰森·鲍姆加特纳发布了数据集的补丁，填补了一些，但不是所有的空白——一些缺失的数据现在是任何人都无法获得的。此外，我们正在向[提供从数据集开始到 2017 年 6 月](http://devingaffney.com/files/monthly_missing_data_start_to_2017_06.zip)(我们检查的最新日期)数据集中所有已知缺失缺口的副本。

# 通过公开讨论缺失数据问题来改进研究

我们并不打算让这篇文章成为针对现有文献的“抓住问题”的文章。我们已经向我们引用的所有研究团队发送了我们论文的副本，一些团队已经根据我们的了解更新了他们的发现(当有更多可用时，我们将在下面发布链接)。Jason 与我们密切合作，尽可能多地了解和解决问题。

我们的发现只是计算社会科学中一个持久问题的一个实例——研究受到我们使用的数据集和这些数据集中存在的任何已知或未知的偏见的限制。

杰森·鲍姆加特纳的数据集已经成为计算社会科学的非凡资源，我们预计它将继续产生重要的发现。研究人员有责任认识到数据的局限性，适当地解释它们，并记录这些问题如何影响我们的发现。

请将这篇文章分享给任何你认识的想要使用这个数据集的人。我们也鼓励审稿人要求作者解释他们论文中任何缺失的数据。

如果你是一名想使用 Reddit 数据集的研究人员，请 ***不要只引用我们的论文而忽视我们提出的问题*** 。我们强烈鼓励任何研究人员采取以下步骤:

*   [用 Jason 的更新](http://files.pushshift.io/reddit/requests/1-10m_submissions.zip)修补数据集，知道它只是填补了一些空白
*   报告影响你论文的缺失数据量，在你研究的数据集部分应用类似于我们的方法
*   通过模拟数据来填补空白，找出您的发现对缺失数据的敏感程度，然后使用模拟数据来计算您的发现的上限和下限

计算缺失数据需要花费时间，并且会增加您完成研究的时间。由于[有缺陷的发现一旦公之于众就很难纠正](https://fivethirtyeight.com/features/trump-noncitizen-voters/)，我们鼓励你在用这个数据集做研究时投入时间。

我们感谢所有根据这些发现回顾他们过去研究的作者。出于这个原因，我们计划更新这篇文章和我们论文的附录，提供历史上使用过这些数据的研究人员进行的任何重新分析的信息。我们希望这将有助于每个人提高我们共同工作的质量。