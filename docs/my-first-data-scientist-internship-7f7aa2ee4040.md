# 我的第一份数据科学家实习

> 原文：<https://towardsdatascience.com/my-first-data-scientist-internship-7f7aa2ee4040?source=collection_archive---------3----------------------->

![](img/76a2a596708fa471b93e1baec7c0e37a.png)

在写作的时候，是我在[量子发明](http://www.quantuminventions.com/)的数据科学家实习的最后一天的前一天。现在，坐在笔记本电脑屏幕前，回想过去几个月的学习历程，真的很难，但也很充实。

> 在一天结束的时候，问题在旅程结束后仍然存在——**你学到了什么？这是你想要的吗？**

原谅我，作为一个物理学家，我相信总是要问正确的问题，通过合理的推理来寻求真理。事实上，作为一名数据科学家，提出正确的问题无疑是重要的(后面会解释……)。

为了给你一个概述，这篇文章主要致力于三个部分——**实习前，实习中，实习后**给你一个我骑行旅程的概述。根据您目前的学习阶段，可以随意跳到这一部分。让旅程开始吧！

> 谢谢大家！当我的第一篇文章[在 Medium](/my-journey-from-physics-into-data-science-5d578d0f9aa6) 上收到了来自不同人的巨大支持，甚至被[向数据科学](https://towardsdatascience.com/)推荐和发表时，我的情绪非常激动。
> 
> 这真的成了我继续与更多人分享我的学习经验的动力，因为，**学习如果有趣，帮助别人更好！**

# 实习开始前

我还清楚地记得，我在 2017 年 11 月期末考试卷子的第二天，就开始阅读 R 中的应用的教材——《统计学习导论》。这是我第一次在非常基础和统计的层面上接触机器学习。

一旦我掌握了这些概念，我就开始转向 Coursera 上的一门热门课程——吴恩达教授的 [**机器学习**](https://click.linksynergy.com/deeplink?id=zeF/0G0dYHo&mid=40328&murl=https%3A%2F%2Fwww.coursera.org%2Flearn%2Fmachine-learning) 。事情并不像一开始看起来那么简单，但吴恩达总是有一种自然的能力来吸引人们的注意力，尽管任何概念都很复杂，但他能像其他人一样简化概念以便消化。我想这就是我真正迷上机器学习的原因。只要试一试，你就会发现这个 ***流行语*** 真的没有听起来那么复杂。我敢打赌。

同时，我也在学习人工智能的另一个重点领域——[深度学习](https://en.wikipedia.org/wiki/Deep_learning)。为了后知后觉地了解这个看似外来的术语是什么，请看看[对神经网络](http://www.explainthatstuff.com/introduction-to-neural-networks.html)的解释，以及[神经网络是如何用于计算 **any** 函数](http://neuralnetworksanddeeplearning.com/chap4.html)的。好的，如果在阅读了推荐的文章后，你像我一样总是需要某种可视化来理解事情是如何工作的，那么[点击这里](http://playground.tensorflow.org/#activation=relu&regularization=L2&batchSize=10&dataset=circle&regDataset=reg-plane&learningRate=0.01&regularizationRate=0.001&noise=50&networkShape=4,2&seed=0.94867&showTestData=false&discretize=false&percTrainData=90&x=true&y=true&xTimesY=false&xSquared=true&ySquared=true&cosX=false&sinX=false&cosY=false&sinY=false&collectStats=false&problem=regression&initZero=false&hideText=false)。按下“播放”按钮，坐下来，放松，观察神经网络如何用于分类和回归。很酷不是吗？请在下面的评论中告诉我，以便讨论和澄清。

所有的读、做、学，在我 2017 年 12 月开始实习之前，已经让我自己(希望)做好了准备。

# 实习期间

![](img/95cbb406a6950ef338e41d44bf17e1d7.png)

[量子发明](http://www.quantuminventions.com/)通过利用其集成的移动应用套件，专门为消费者、企业和政府提供移动智能。企业物流和分析平台。我是第一个加入研发和分析团队的数据科学家实习生。

在最初的几天内，我被介绍给了令人惊叹的同事，行业内的各种交通术语，以及正在进行的令人兴奋的项目。我实习期间最喜欢的一件事是给予我的信任和自由，让我可以选择自己感兴趣的项目并全力以赴！

令我惊讶的是，我意识到我是这个项目的先驱，因为以前没有人做过。当以前没有人做过一些事情时，研究就出现了，这是我感激的地方，尽管有不确定性和困难。为什么？很简单，因为我有机会从头开始体验真正的数据科学工作流程(如果不是全部的话)。

请允许我简单地列出我所经历的工作流程，因为这些工作流程奠定了我在数据科学领域的基础。我希望你会发现它在某些方面是有用的。😊

# 1.理解业务问题

选择的项目是关于**短期高速公路行程时间预测**。然而，就像我说的，**问正确的问题对于数据科学家来说非常重要**。A 在项目最终确定之前，为了真正了解真正的业务问题，提出了很多问题，比如可用的数据源、项目的最终目标(甚至在我离开之后)等等。本质上，我们的目标是**预测新加坡高速公路提前 N 分钟的行驶时间，比当前的基线估计更加准确**。

# 2.收集数据源

对新项目感到兴奋，我开始从数据库和同事那里收集数据源(基本上是在办公室里走来走去，询问关于数据源的问题)。收集正确的数据源类似于您从不同的网站收集数据以便稍后进行数据预处理。这一点非常重要，它可能会影响到您在后期构建的模型的准确性。

# 3.数据预处理

真实世界的数据是肮脏的。我们不能期待一个像 [Kaggle](https://www.kaggle.com/) 提供的格式良好且干净的数据。因此，数据预处理(其他人可能称之为数据管理或数据清理)是如此重要，我怎么强调它的重要性都不为过。这是最重要的阶段，因为它可能占据整个工作流程的 40%-70%，仅仅是清理要提供给模型的数据。

> 垃圾进，垃圾出

我喜欢数据科学的一点是，你必须对自己诚实。当你不知道什么是你不知道的，并且你认为预处理的数据已经足够干净，可以输入到你的模型中时，这就存在着用错误的数据建立正确的模型的风险。换句话说，总是试着用你所拥有的领域知识来质疑数据在技术上是否正确，用严格的阈值来仔细检查数据，以**检查整个数据集中的任何其他异常值、缺失或不一致的数据**。

在我犯了向模型提供错误数据的错误之后，我对此特别小心，仅仅是因为预处理步骤中的一个简单缺陷。

# 4.建筑模型

![](img/2645ac68783321fc6f9520e9456ff156.png)

经过一些研究，我提出了四个模型用于我们的项目，它们是[支持向量回归(SVR)](https://en.wikipedia.org/wiki/Support_vector_machine) 、[多层感知器(MLP)](https://en.wikipedia.org/wiki/Multilayer_perceptron) 、[长短期记忆(LSTM)](https://en.wikipedia.org/wiki/Long_short-term_memory) 和[状态空间神经网络(SSNN)](https://pdfs.semanticscholar.org/ceb9/4f539ddc3be17b4f0d0b2a8cd5dd751f9e80.pdf) 。为了简洁起见，你可以在各种网站上找到每个模型的详细解释。

作为一个仍在从 MOOCs 和教科书中学习的人，从零开始构建不同的模型对我来说是一个陡峭的学习曲线。幸运的是， [Scikit-learn](http://scikit-learn.org/stable/) 和 [Keras](https://keras.io/) (带有 Tensorflow 后端)帮助了我，因为它们很容易学习，可以在 Python 中进行快速模型原型化和实现。此外，我还学习了如何使用多种技术优化模型和微调每个模型的超参数。

# 5.模型评估

为了评估每个模型的性能，我主要使用了一些指标:

1.  [**【平均绝对误差】**](https://en.wikipedia.org/wiki/Mean_absolute_error)
2.  [**均方误差**](https://en.wikipedia.org/wiki/Mean_squared_error)
3.  [**决定系数(R2)**](https://en.wikipedia.org/wiki/Coefficient_of_determination)

在此阶段，重复步骤 3-5(可互换),直到确定了优于基线估计的最佳模型。

# 实习结束后

![](img/985e6f93666eff1053e17cc745149535.png)

嗯，实习肯定重申了我对数据科学的热情，我很感激我的工作确实为未来的工作留下了一些动力。研发阶段，与不同利益相关者交谈所需的沟通技巧，使用数据解决业务问题的好奇心和热情(仅举几例)都促成了我对这一领域的兴趣。

数据科学行业仍然非常年轻，对于像我们这样的求职者来说，它的工作描述可能看起来模糊不清。不具备所需的所有技能是完全正常的，因为大多数职位描述都是理想化的，以符合他们的最佳期望。

当有疑问时，只需从 MOOCs、书籍和文章(我仍在做)中学习基本面，并通过自己的个人项目或实习应用所学。耐心点。学习之旅确实需要时间。津津有味地从你的旅程中学习。因为…

> 在一天结束的时候，这些问题在旅程结束后依然存在——你学到了什么？这是你想要的吗？

感谢您的阅读。我希望这篇文章能给你一些简要的(不是详尽的)数据科学工作流程和我的旅程的文档。

如果您有任何问题，请添加我，我们在 [LinkedIn](https://www.linkedin.com/in/admond1994/) 上聊天，或者访问我的[个人网站](http://admond1994.strikingly.com/)了解更多信息:

## 关于作者

[**Admond Lee**](https://www.linkedin.com/in/admond1994/) 目前是东南亚排名第一的商业银行 API 平台[**Staq**](https://www.trystaq.com)**—**的联合创始人/首席技术官。

想要获得免费的每周数据科学和创业见解吗？

你可以在 [LinkedIn](https://www.linkedin.com/in/admond1994/) 、 [Medium](https://medium.com/@admond1994) 、 [Twitter](https://twitter.com/admond1994) 、[脸书](https://www.facebook.com/admond1994)上和他联系。

[](https://www.admondlee.com/) [## 阿德蒙德·李

### 让每个人都能接触到数据科学。Admond 正在通过先进的社交分析和机器学习，利用可操作的见解帮助公司和数字营销机构实现营销投资回报。

www.admondlee.com](https://www.admondlee.com/)