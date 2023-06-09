# Check Mate 深度学习

> 原文：<https://towardsdatascience.com/checkmate-deep-learning-fb86fa044d60?source=collection_archive---------4----------------------->

人机交互如何打败深度学习。

![](img/20f7e9265e5659efe34c57b493d55e7d.png)

Human vs Robot, by [Frank Steiner](http://www.chilloutpoint.com/), CC-BY-SA-4.0

深度学习正在主导这些天的人工智能新闻。人们看不到的——深度学习是可以被打败的。不要误解我。深度学习很奇妙。它开辟了机器学习的新领域。它惊人的图像结果给数据科学家和感兴趣的公众留下了深刻的印象。除了图像分析，您还可以在文本和时间序列分析领域发现有趣的新结果。所有这些都很棒；但被高估了。Kaggle 最近的一篇文章显示，线性回归方法击败了神经网络。那么诀窍是什么呢？如何能轻松检查 mate 深度学习？

# **深度学习不那么秘密的酱料**

现代神经网络体系结构具有一些重要的特征，这些特征使它们在许多用例中获得成功。它们能够解决非线性问题，善于接收复杂的输入，并且能够在内部进行特征选择。

最常被引用的秘密是内部特征生成。深度学习架构在其节点中为自己构建了新的参数。你可以把它归结为一句简单的话:

> 在深度学习中，架构工程是新的特征工程 [Stephen Merity](https://smerity.com/articles/2016/architectures_are_the_new_feature_engineering.html) ，Salesforce 的高级研究科学家

虽然这是一个优点，但它引入了更多的自由度。这最终会产生一个问题，即您需要输入足够的信息来设置这些自由度。值得注意的是，信息必须来自更多的线，而不是更多的特征。事实上，更多的功能使问题变得更加困难。这被称为维数灾难。

# 真实生活的例子

在实践中，您经常会遇到记录的例子数量有限的用例。我最近有一个用例，在那里我处理书面的维护记录。工程师正在记录维修原因。那是在大型机器上，那里没有数百万台机器。这导致了几百行错误。用例是为了确定与其他部门的相关性。

将这些记录输入深度学习算法并没有产生任何好的结果。维护记录中的语言富含用例相关的术语。有限的行数阻止了深度学习结构学习底层因素。

# 通过特征工程传递知识

我们确实知道零件 v 120–3 和 v 120–4 属于同一类型。所以我们可以用一个令牌 V120 来代替两者。我们所做的是通过生成新的特征将世界的知识转移到模型中。

虽然这听起来微不足道，但它有两个迷人的方面。首先，这种特征工程可以降低问题的复杂性和非线性。我们手动“设置”模型的自由度。斯科特·凯勒的线性回归有很多工程特征。我认为这种线性回归就是用一种非常类似于 SVM 中手工制作的内核的方法来解决非线性问题。

另一个更重要的部分是，我们输入关于世界的信息。原始数据中不包含此信息！我们将关于同义词或关系的信息转移到模型中。这是特征工程所独有的。我们确实有像 word2vec 或 GloVe 这样的自动方法来尝试获取单词的意思。但是都需要数据。如果你没有相应的数据，你就无法提取这些模式。另一方面，人类知道这些关系，并能把它输入进去。检查队友？

# 大数据拯救世界；不是！

解决数据有限问题的一个常见答案是大数据。但(可悲的是)这通常没有任何帮助。让我们在机器上安装大量的传感器。这些传感器连接到云环境。我们现在可以分析万亿字节的数据；这很酷，但很可能没有帮助。

我们正在重新研究这几百个例子，工程师写下了发生的事情。当然，我们可以用更多关于机器状态的信息充实每一行。但是我们没有生成更多相关的行。大数据往往并不意味着大样本量，而是每行包含大量信息。

# 迁移学习——答案？

深度学习挨打了吗？说实话，也不尽然。在深度学习的前沿，许多科学家正在尝试为深度学习结构进行迁移学习。一个关键的想法是将单词之间的关系或潜在的图像模式转移到其他用例中。这个想法是，识别图片中的一条线的网络也可能在其他应用中有用。

这个深度学习领域正在后院兴起。但迟早会冲击商业应用。如果你对如何在深度学习中进行迁移学习感兴趣，请看看 Sebastian Ruder 的[精彩博文。](http://sebastianruder.com/transfer-learning/)