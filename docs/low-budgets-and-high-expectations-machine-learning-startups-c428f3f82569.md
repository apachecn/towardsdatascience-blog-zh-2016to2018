# 低预算和高期望:机器学习初创公司

> 原文：<https://towardsdatascience.com/low-budgets-and-high-expectations-machine-learning-startups-c428f3f82569?source=collection_archive---------5----------------------->

在之前关于如何雇佣人工智能顾问和如何给人工智能项目定价的文章中，我试图让你了解人工智能咨询领域的工作方式。我也让你们感受到了许多在[的老牌组织在将他们的数据放入云端](https://medium.com/@lemaysolutions/locked-in-a-box-machine-learning-without-cloud-or-apis-76cc54e391c8)时所面临的特殊挑战。

这篇文章是关于那些**更小更新的公司**的。让我先说一下，并不是所有的创业公司都是一样的。每个人都是一朵特别的花，生长或枯萎都是如此。现在，我们来概括一下。在人工智能真正炙手可热的创业世界中，存在一种奇怪的动态，然而当谈到对预期进行定价时，预期和现实之间的一致性却很不正常。企业客户通常需要一些快速解决方案来部署 [apache spark](https://databricks.com/spark/about) 或 [hadoop](https://www.cloudera.com/products/open-source/apache-hadoop/key-cdh-components.html) 或[等等](http://aws.amazon.com)，以便及时获取他们的数据。这还是在考虑机器学习之前。对于初创公司来说，这同样具有挑战性。

一旦可以访问数据，就需要设计、构建、测试和部署 ML 系统。对于某些模型来说，这非常简单，但是这些步骤仍然需要时间。可能需要尝试几种不同的模式。客户端几乎总是需要配置 GPU 资源，即使这仅仅意味着在 p2 实例上加载 AWS 中的 AMI。这需要更多的时间。不要忘记有时你想要一个[保留实例来降低成本](https://aws.amazon.com/ec2/pricing/reserved-instances/buyer/)。也许你想比较 GCP 和 AWS 的平台。现在，系统几乎总是需要超过用户名和密码的安全性。我喜欢使用 [PPK/pem 证书和谷歌认证器](https://www.digitalocean.com/community/tutorials/how-to-set-up-multi-factor-authentication-for-ssh-on-ubuntu-16-04)。所以简而言之，即使是创业公司的小型机器学习项目也没有那么小。

我开始写这篇文章之前，一个小的一个人创业公司上周接触到我们公司的一个项目，将需要 3 个月的兼职基础上，成本上限为 1000 美元。我喜欢非常节省时间，但这是愚蠢的。如果这个项目预计需要 4 个小时，我还是不得不拒绝。只是为了通过 NDA 和计费流程…是的。这说不通。

现在，这家创业公司不是想耍我。这是他们能承受的。我不得不建议这位企业家去为他的想法筹集资金，并保持联系。[这些关于 ML 创业的困难的思考](https://medium.com/towards-data-science/fear-in-the-age-of-machine-learning-87ee33b5b264)[对我来说并不新鲜](https://medium.com/@lemaysolutions/what-should-i-call-my-startup-ef9aea077271)。

我喜欢[凯文·德瓦尔特](https://medium.com/u/cd607ea5777b?source=post_page-----c428f3f82569--------------------------------)关于企业客户机器学习项目成本估算的表格。请参见下面的链接:

 [## AI 第 1 年预算模板

### 规划你的第一个人工智能产品的起点

blog.prolego.io](https://blog.prolego.io/ai-year-1-budget-template-85d4d419a5a1) 

Kevin 列出的成本比许多初创公司机器学习项目都高，因为许多初创公司没有针对其原型的[大数据要求](https://medium.com/@lemaysolutions/big-data-and-machine-learning-two-peas-in-a-pod-e3a04d627784)。然而，它应该让每个人都很好地了解保持机器学习项目正常运行的成本。

有些风险是值得的，比如早期未付费的客户演示，这样才有机会赢得项目。但是，有时候客户对可能发生的事情抱有非常不切实际的期望。[时间、金钱、质量→选择 2](https://www.linkedin.com/pulse/20140413150052-77663796-analytics-understanding-the-money-time-quality-equation)

![](img/d8f96e9f64e61de6e44a4ac9c112d082.png)

更奇怪的是。ML 开发是关于结果的，而不是你的顾问花了多少时间编码。在一个商品化的市场中，我们会对 ML 项目收费，就像软件公司对网络开发收费一样。更多的时间意味着更多的钱，工作时间和结果之间的关系是线性的(也许真的更像一条 tanh S 曲线)。然而，在 ML 中，努力输入经常导致未知的结果输出。数据科学仍然是一门艺术，有时最大的成果来自于知道在哪里寻找正确的库(低努力；高回报)，而不是写最激进的代码(高努力；奖励高)。

我想分享一下我上周与一个客户的数据打交道的经历，让我明白 ML 的发展是一条曲折的道路:

我从客户的约 10，000 条记录的小型单表 mySQL 数据集开始，用于回归和分类模型。因为体积小，我从无监督迁移学习开始。我取得了不错的成绩，但怀疑自己还能做得更好。接下来，我着手开发一个监督学习模型，它可以编译，但结果更差。接下来，我继续研究生成模型。结果比以前更糟，迫使我回到最初的解决方案。我在最初的方法中添加了一些特性工程，并在性能上取得了一点点改进。现在，这个模型似乎已经达到了这个阶段的最佳状态。在这里，在概念验证项目的最后，我花费了超过 80%的精力在不会被使用的代码上。而性能最好的代码是我在项目的前 20%写的东西。大多是前 20 分钟！

我们从这件事中学到的是，时间和结果之间的关系是变化无常和怪异的。数据科学更多的是化学和柠檬派，而不是土木工程和纯数学。这是一个更加动态和实用的过程，而不是一个金钱投入/结果产出的等式。

我对创业期望的建议是，在开始你的机器学习项目之前，尽可能多地收集、组织和标记你的数据。与你的利益相关者讨论你的资源，不要羞于从不同的供应商那里得到多个报价。你会发现价格和质量的权衡，就像其他购物一样。缓和你的期望，但不是你的兴奋。部署机器学习的[**时间到了**](https://medium.com/toronto-machine-learning/educational-videos-artificial-intelligence-deep-learning-machine-learning-etc-3fadb6050eb5)**！**

**如果你喜欢这个帖子，那么请推荐它，分享它，或者给它一些爱(❤).我也很高兴在评论中听到你的反馈。**

**编码快乐！**

**-丹尼尔
[丹尼尔@lemay.ai](mailto:daniel@lemay.ai) ←打个招呼。
[LEMAY . AI](https://lemay.ai)
1(855)LEMAY-AI**

**您可能喜欢的其他文章:**

*   **[人工智能和不良数据](/artificial-intelligence-and-bad-data-fbf2564c541a)**
*   **[人工智能:超参数](/artificial-intelligence-hyperparameters-48fa29daa516)**
*   **[人工智能:让你的用户给你的数据贴上标签](https://medium.com/towards-data-science/artificial-intelligence-get-your-users-to-label-your-data-b5fa7c0c9e00)**