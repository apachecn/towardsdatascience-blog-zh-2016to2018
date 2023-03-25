# 关于机器学习的行业故事

> 原文：<https://towardsdatascience.com/industry-stories-about-machine-learning-bb5652455fe2?source=collection_archive---------9----------------------->

## 伦敦 PAPIs.io 2018 大会笔记

上周我去参加了 [PAPIs.io Europe 2018](https://www.papis.io/europe-2018) 大会，在伦敦金丝雀码头大厦举行。

该会议将自己描述为“一系列致力于现实世界机器学习应用以及推动这些应用的创新、技术和工具的国际会议”(据我所知，名称 *papis* 来自“预测 API”)。

我在周四去了那里，那一天是专门讨论“工业和创业”的，我对我所看到的做了一些笔记。下面快速总结一下！

![](img/befaf3bb8281544b8437a70a26166a2c.png)

The view from the 39th floor of Canary Wharf Tower.

## 采用 Kubernetes、Dask 和 Jupyter 的 ML 基础设施

上午的主题演讲由 [Olivier Grisel](http://ogrisel.com/) 主讲，他可能因对 [scikit-learn](http://scikit-learn.org/stable/) 的巨大贡献而闻名——因此任何用 Python 进行机器学习的人都应该感谢他！他的幻灯片在网上[这里](https://docs.google.com/presentation/d/1PuvBl2rqZi0k0J2x4XcShmFiLeSfBXkarg6nYAGCxUs/edit#slide=id.p)。

在这个大约 23 分钟开始的视频[中，他展示了如何使用三个主要的开源组件来建立自己的机器学习基础设施:](https://www.youtube.com/watch?v=9zEAC89QcjA) [Kubernetes](https://kubernetes.io/) (一个基于[容器](https://en.wikipedia.org/wiki/Operating-system-level_virtualization)的集群编排系统) [Dask](https://dask.pydata.org/en/latest/) (一个并行化 python 作业的工具，它与现有的 python 库如 pandas/numpy 深度集成)和 [Jupyter Notebooks](http://jupyter.org/) (著名的交互式开发 web 应用)。

具体来说，他使用 [minikube](https://kubernetes.io/docs/getting-started-guides/minikube/) 在本地运行 kubernetes，使用 [jupyter hub](https://jupyterhub.readthedocs.io/en/latest/) 在一台 jupyter 服务器上管理多个用户。他展示的第一个例子有些琐碎(例如，增加一个计数器)，但这使他能够深入地描述计算是如何分布和执行的。第二个例子展示了如何运行[网格搜索](http://scikit-learn.org/stable/modules/grid_search.html#exhaustive-grid-search)来为支持向量机找到最佳参数，使用 [dask-joblib](http://distributed.readthedocs.io/en/latest/joblib.html) 在集群上运行。

问答中我最喜欢的一句话是一个关于开发 ML 系统的即兴评论:“你不应该在 Jupyter 笔记本上做所有的事情”(因为这对维护来说不是很好)。

## 使用自动气象站进行边缘预测

第二个演讲人是来自亚马逊的人工智能/人工智能传播者[朱利安·西蒙](https://aws.amazon.com/evangelists/julien-simon/)(他在这里写博客)。从这个视频[的第 59 分钟开始](https://www.youtube.com/watch?v=9zEAC89QcjA)，他的演讲集中在运行数据中心之外的机器学习预测(“在边缘”——在摄像机、传感器等方面)。).实现这一点需要，也许并不奇怪，经历一场机器学习系统可用的各种 AWS 服务的旋风之旅。其中包括:

*   用[胶子](https://aws.amazon.com/blogs/aws/introducing-gluon-a-new-library-for-machine-learning-from-aws-and-microsoft/)和 [MXNet](https://aws.amazon.com/mxnet/) 定义和操纵模型；
*   用 [SageMaker 建立和训练模型；](https://aws.amazon.com/sagemaker/)
*   使用[λ](https://aws.amazon.com/lambda/)编写按需预测函数；
*   使用 [Greengrass](https://aws.amazon.com/greengrass/) 将代码部署到边缘设备。

他的演讲以最近推出的“世界上第一款面向开发者的深度学习摄像机”DeepLens 的演示结束，展示了实时物体检测的作用。

## 管理工程师和数据科学家角色之间的差距

其中一个涉及非常有趣话题的演讲是由来自数据驱动广告公司 [dataxu](https://www.dataxu.com/) 的 [Beth Logan](https://www.linkedin.com/in/bethlogan) 主持的。她描述了他们如何着手开发和自动部署机器学习管道(因此标题为“驾驶时更换轮胎”这个演讲是在线的[这里是](https://www.youtube.com/watch?v=WV_o7B4YcNw))以支持广告领域的各种应用。

离开 ML 本身，有一些有趣的观点是关于如何管理“数据科学家”和“工程师”的工作，以使每个角色发挥他们的优势。实际上，这是让数据科学家开发和迭代模型，同时将生产和扩展模型的所有工作留给工程师，工程师还必须证明生产实现的性能符合预期。

数据科学和工程的交叉是一个我可能会写一整篇博文的主题；可以说，我们在最后讨论了这样的划分是否是“正确”的方式，以及在合作时每个学科如何超越另一个学科。

## 机器学习中的流水线丛林

下一个发言人是来自另一家处理数字广告的公司 Appnexus 的 Moussa Taifi。他讨论了为点击预测建立各种管道，这是在线广告的一项常见任务。

穆萨在研究和生产之间来回穿梭时，谈到了开发管道的一些实际问题。这些问题包括:一旦管道过于复杂(“丛林”)，就很难再现结果；为实验进行模型版本控制；避免时间旅行等常见问题(对测试集中的数据之后创建的数据进行培训)；以及不管手头的任务如何，采用即时数据转换和特征提取的系统与根据预先计算的固定特征集构建模型相比，哪一种更好。

## 建立机器学习的文化

[Lars Trieloff](https://www.linkedin.com/in/trieloff/) 发表了一篇关于在 [Adobe](https://www.adobe.com/uk/#) 内部培育人工智能文化的高级演讲——特别关注 [Adobe Sensei](https://www.adobe.com/uk/sensei.html) 。他的演讲跨越了三个广泛的领域:*品牌、愿景和技术，*以及这三者需要如何结合才能在组织内培养机器学习的文化。有趣的是，他还谈到了责任——以及在开发新的机器学习工具时，公司的所有员工需要如何经历培训和批准过程。

## 机器学习的可行性与投资回报

来自 [BigML](https://bigml.com/) 的 Poul Petersen 发表了一篇关于该公司如何预测 2018 年奥斯卡获奖者中的 6 位的演讲——见[这篇博文](https://blog.bigml.com/2018/03/01/predicting-the-2018-oscar-winners/)有一些类似的内容。除了奥斯卡奖，他还提出了一个有趣的观察，即如何根据对机器学习项目的可行性和预计投资回报的比较，对这些项目进行优先排序。如果两者都很低，这显然是一个禁区；如果两者都很高，这是一个显而易见的，你应该已经在努力了。剩下的两个类别是“可推迟”(低投资回报率，高度可行)和“脑力劳动者”(高投资回报率，目前不可行)。

他给出了一个类似的类比，根据特定系统的发展阶段，哪些算法是他的目标:早期阶段，需要快速原型(逻辑回归)，中期阶段，你有一个经过验证的应用程序(随机森林)，最后是后期阶段，调整性能变得至关重要(神经网络)。

## 创业推介和小组讨论——欧洲人工智能景观

创业推介分散在一天当中。我在这里看到的:

*   [逻辑时钟](http://www.logicalclocks.com):他们有一个名为 [Hops](http://www.logicalclocks.com/deep-learning/) 的企业机器学习平台，旨在提高数据科学家的生产力。
*   [Antiverse](http://www.antiverse.io/) :旨在利用人工智能在一天内发现抗体药物。
*   [Tensorflight](https://tensorflight.io/) :通过使用机器学习分析卫星和航空数据，实现财产检查自动化。
*   为企业客户提供单一联系点，自动化他们与你联系的各种方式。

这些创业公司中的一些在一天结束时参加了一场创业大战，这是由一个 AI 判断的[。当我第一次听说这件事时，我有些怀疑，但它实际上非常有趣。每个初创公司都被 Alexa 轮流提问，问题涉及团队的规模、经验和结构，并根据各种因素进行评分。获胜者赢得了 10 万英镑！](https://medium.com/@thomasrorystone/1st-startup-competition-judged-by-an-ai-apply-to-win-100-000-from-ai-seed-7ce5cb42df72)

相反，创业小组采取了回顾的视角——回顾 Twizoo(在我离开前不久，[被 Skyscanner](https://techcrunch.com/2017/11/09/skyscanner-buys-twizoo-to-add-social-content-shout-outs-to-travel-reviews/) 收购)、 [prediction.io](http://predictionio.apache.org/index.html) (在伦敦成立后被 Salesforce 收购[)和](https://techcrunch.com/2016/02/19/salesforce-acquires-predictionio-to-build-up-its-machine-learning-muscle/) [Seedcamp](https://seedcamp.com/) 。反复出现的主题是关注*客户*而不是机器学习的重要性:技术是解决客户痛苦的使能器，与客户的需求相比，需要在过程中解决的抽象机器学习问题几乎是多余的。

这一天有许多不同的外卖。一个突出的现象是，机器学习领域的欧洲创业公司仍然在蓬勃发展。事实上，[Juno AI](https://twitter.com/libbykinsey)项目的 Libby 宣布他们正在开始新一轮的地形测绘——这个项目绝对值得一试并为之做出贡献。