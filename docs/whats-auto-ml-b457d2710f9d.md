# 什么是 auto ML？

> 原文：<https://towardsdatascience.com/whats-auto-ml-b457d2710f9d?source=collection_archive---------2----------------------->

![](img/09d841454bb1a5f7f258865284b9cdda.png)

The purpose of computing is insight, not numbers — Richard Hamming (photo by CoWin)

我最近告诉一个朋友，我正在研究自动机器学习(ML)项目，他问我这是否意味着汽车的机器学习，所以…我想我应该写一篇简短的帖子。

Auto ML 服务通过点击一个按钮来提供机器学习，或者，至少，承诺保持算法实现、数据管道和代码，通常是隐藏的。如果你向一群工程师提及这一前景，有些人会说“万岁！”有些人会说“哦不！”，我想直到最近，我发现自己在这些感叹(“orayoh！”).

我第一次玩自动 ML 平台是在去年 IBM 的一次演示中。我来自 iTerm、Python 和 vim，我发现从代码中如此脱离很奇怪。但我可以看到，已经在 R&D 接受审查的流程可以通过 auto ML 得到简化。并不是每个团队都致力于前沿算法的设计，因此对已经验证的模型进行微小调整的过程肯定有优化的空间。

我可以看到 auto ML 如何在不断增长的 ML/AI 产品的生命周期中定位自己，但这样的平台真的能成为“数据科学家的最好朋友”吗？我想要一些其他的数据点，所以我联系了我的朋友 Pierre Forcioli-Conti，他正忙着做他的创业公司[工作台](http://workbenchdata.com/)，我们一起坐在他位于市场街的办公室里，一边聊着工作流，一边在他的环境中摆弄样本数据。他的愿景更多的是为团队提供一个协作的公共工作空间(就像 Google Docs 的数据项目)，我记得我对数据解析和清理功能特别兴奋。但是，不管出于什么原因，我回到了我的工具，没有明确的计划将 auto ML 集成到我正在构建的任何东西中(尽管我确实偷偷溜出去玩了一下 [auto-sklearn](https://automl.github.io/auto-sklearn/stable/) 和 [TPOT](https://automl.info/tpot/) ，只是为了看看他们在做什么)。

快进到本周，我看到了一个关于 H2O 的帖子，作者是我信任的数据科学专家艾琳·莱德尔。我跟着她链接到了[产品](http://h2o-release.s3.amazonaws.com/h2o/rel-wolpert/9/index.html)，查看文档，我发现了对不同语言和云集成的大量支持，这令人鼓舞。

安装没有痛苦，界面也很容易接近，位于数据分析包(如 Pandas)和建模工具(如 [scikit-learn](http://scikit-learn.org/) 、 [Tensorflow](https://www.tensorflow.org/) 和 [Keras](https://keras.io/) )的交汇处，有着 Jupyter 般的级联细胞流。无论是使用本地主机接口(将处理步骤放在一个称为“流”的文档中)还是将软件直接导入 Python shell，都比我在 [AWS](https://aws.amazon.com/machine-learning/) 或 [IBM 的 Watson Studio](https://datascience.ibm.com/) 上使用 ML 工具时感觉更直观和熟悉。

我确实发现自己在脑子里运行着一个时钟，记录着“这通常要花我多长时间？”。我想人们可以很快学会 H20 的特性，然后从有利的角度来衡量这个工具最适合自己的工具包。我个人对神经网络的超参数优化很感兴趣，特别是对于更大的深度学习项目，我发现自己在想象这种探索可能会如何进行。我会遇到什么样的错误？我能找到什么样的支持？如果 H2O 真的可以一站式购买数据管理、EDA、建模和 GPU 处理，那么我完全可以看到 H2O 在我的桌面上更加靠前和居中。

我担心团队会使用 auto ML 来搜寻能够交付“结果”的模型。机器学习是一门艺术，也是一门统计建模学科，虽然我对我们所做的事情的创造性探索方面深信不疑，但随着我们的管道变得更加抽象，我们应该在自动 ML 流程中加入什么样的路标和健全检查，以确保我们继续提出强有力的问题？也许 [Clippy](http://knowyourmeme.com/memes/clippy) 有一个有模型头脑的表亲，他可能会定期出现并问类似这样的问题，“这个数据表示符合你的直觉吗？”或者“我们有夸大错误的风险吗？”，或者“您如何解释这些绩效结果？”。仅仅访问代码并不能回答这样的问题，也许抽象会释放出更多的能力来问这样的问题，但我担心的是，随着时间的推移，这样的问题可能会开始产生诸如“我不知道，那是我被告知要按的按钮”这样的回答。这不仅是对 H2O 的警告，也是对所有汽车制造企业的警告，是对我们所有负责确保有效使用数学工具的人的警告。

我看到 H2O 在云集成方面走在了前面，这标志着一定规模(或有扩展计划)的客户优先，他们肯定已经做好准备，不需要任何特定的堆栈(向 Java、Python 和 R 社区等提供服务)。总而言之，我对 auto ML 感到更多的是“万岁”而不是“哦不”，并期待看到团队如何在他们的产品周期中使用这些工具。