# 论数字用户线在人工智能中的重要性

> 原文：<https://towardsdatascience.com/on-the-importance-of-dsls-in-ml-and-ai-1dbccb3a2916?source=collection_archive---------7----------------------->

特定领域语言使我们的生活变得更容易，同时以许多不同的方式开发 AI/ML 应用程序。为工作选择正确的 DSL 可能比选择宿主语言更重要。

![](img/4305e38f8b748801d1cbff4ede3d2ef3.png)

# 1)业务逻辑

DSL 是简明表达业务逻辑的强大工具。

## 例子:交易

[Snippet source](https://github.com/dungpa/dsls-in-action-fsharp/blob/master/DSLCheatsheet.md), read more (Gosh2010, Frankau2009)

同时，人工智能和人工智能系统并不是一成不变的。

*   基础模型反映了可能随时间变化的业务和工作假设
*   敏感度分析不仅要针对模型(超)参数进行，还要针对业务和工作假设进行

DSL 在用一种读起来像英语的语言流利地表达复杂的业务和工作假设方面非常方便。下面的例子，用 [AMPL](https://ampl.com/) 编码，有效地描述了一个优化问题:我们想要最小化与运送产品到一个虚构的油漆公司的客户相关的运输成本。关键假设(想要优化的内容、运输成本、仓库中的产品可用性、每个客户的产品需求)用与当前问题非常相似的语言清楚地表述出来。

[Snippet source](http://www.ieor.berkeley.edu/~atamturk/ieor264/samples/ampl/ampldoc.pdf), read more (Takriti1994)

# 2)数学

一旦问题被公式化，接下来就是写一些数学的时候了。统计和数学的 DSL 已经存在了几十年:Matlab 和 R 在科学界非常流行。嵌入在通用编程语言中的统计和数学 DSL 正受到越来越多的关注。

## 示例:概率编程(来自 Python)

[Snippet source](http://www.ieor.berkeley.edu/~atamturk/ieor264/samples/ampl/ampldoc.pdf), read more: (Patil2010)

# 3)查询(你的数据库)和数据操作

DSL 可以方便地用一种紧凑且富于表现力的语言编写查询和操作数据，这种语言可以顺利地集成到您的主机编程环境中。查询语言(如 Linq)也保留了宿主语言的属性，您可能会发现这些属性是可取的，如类型安全。

## 示例:LINQ 和 F#查询表达式

LINQ 和 F#查询表达式看起来像我们熟悉的好的、旧的 SQL ( [Cheney2013](http://homepages.inf.ed.ac.uk/slindley/papers/practical-theory-of-linq.pdf)

[Snippet source](https://msdn.microsoft.com/en-us/library/gg509017.aspx#JoinandSimpleWhereClause)

## 示例:数据框

数据框架，如熊猫，提供了执行数据争论任务的语法糖，如[分割，应用组合](https://pandas.pydata.org/pandas-docs/stable/groupby.html)和[枢轴](http://nikgrozev.com/2015/07/01/reshaping-in-pandas-pivot-pivot-table-stack-and-unstack-explained-with-pictures/)(麦金尼 2010)

# 4)幕后:表达计算

TensorFlow 可以被视为一个编程系统和运行时，而不仅仅是传统意义上的“库”:

> TensorFlow 的图形甚至支持像变量范围和控制流这样的结构——但是您不使用 Python 语法，而是通过 API 来操纵这些结构。*(*[*Innes 2017*](https://julialang.org/blog/2017/12/ml&pl)*)*
> 
> TensorFlow 和类似的工具以“只是库”的身份出现，但它们是极不寻常的工具。大多数库提供一组简单的函数和数据结构，而不是全新的编程系统和运行时。*(*[*Innes 2017*](https://julialang.org/blog/2017/12/ml&pl)*)*

为什么我们需要一种语言来表达计算？

浏览一下 [Apache Spark](https://spark.apache.org/) 的内部结构有助于理解使用领域特定语言来推理计算的必要性:Spark 在幕后运行一个复杂的执行过程，该过程包括几个步骤:数据流的定义(逻辑计划)、描述任务及其执行的 DAG 的定义(物理计划)、作业调度、具有容错功能的作业执行([阅读更多信息](https://github.com/JerryLead/SparkInternals))

> 构建新语言的核心原因很简单:ML 研究有极高的计算需求，简化建模语言可以更容易地添加特定领域的优化和功能*(*[*Innes 2017*](https://julialang.org/blog/2017/12/ml&pl)*)*

这还不是全部。模型的复杂性呈指数级增长。允许我们表示、推理和分析计算的 DSL 工作目前非常热门。

> 模型变得越来越像程序，包括推理其他程序的模型(例如程序生成器和解释器)，并带有不可微的组件，如蒙特卡罗树搜索。构建既能提供完全的灵活性又能实现最高性能的运行时是一个巨大的挑战，但是越来越多的最强大的模型和突破性的结果都需要这两者。*(*[*Innes 2017*](https://julialang.org/blog/2017/12/ml&pl)*)*

当我们特别关注深度学习时:

> 越来越多的人以数据依赖的方式(用循环和条件)程序化地定义网络，允许他们根据输入数据动态地改变。它真的非常像一个常规程序，除了它是参数化的、自动微分的、可训练/可优化的。动态网络变得越来越受欢迎(特别是对于 NLP)，这要归功于能够处理它们的深度学习框架，如 PyTorch 和 Chainer*(*[*lecun 2017*](https://www.facebook.com/yann.lecun/posts/10155003011462143)*)*

这就导致了一场争论，争论的焦点是一个新的编程框架的诞生，它是由专门为表达计算而设计的 DSL 产生的。来自深度学习背景的 Andrej Karpathy 写道:

> 软件 2.0 是用神经网络权重写的。没有人参与编写这段代码，因为有很多权重(典型的网络可能有几百万)，直接用权重编码有点困难(我试过)。相反，我们对期望的程序的行为指定一些约束(例如，示例的输入输出对的数据集),并使用由我们支配的计算资源在程序空间中搜索满足约束的程序。在神经网络的情况下，我们将搜索限制到程序空间的连续子集，其中搜索过程可以通过反向传播和随机梯度下降*(*[*karpathy 2017*](https://medium.com/@karpathy/software-2-0-a64152b37c35)*)*

# 结论

DSL 方面的专业知识在 ML 和 AI 系统中至关重要。

# 确认

非常感谢 Andreas Hubert 和 Cristian Steinert 提供的反馈。

# 参考

[切尼 2013 年]切尼，j .，林德利，s .，&瓦德勒，P. (2013 年)。语言集成查询的实用理论。ACM SIGPLAN 通知，48(9)，403–416。

[Frankau2009] S. Frankau、D. Spinellis、N. Nassuphis 和 C. Burgard,《商业用途:在外来贸易中发挥作用》。函数式编程杂志，19(1)，27–45。2009 年 10 月 10 日

[gosh 2010]DSL 在运行，Debasish Ghosh，Manning Publications，2010 年 11 月

[Innes2017]关于机器学习和编程语言，Innes et all，[https://julialang.org/blog/2017/12/ml&pl](https://julialang.org/blog/2017/12/ml&pl)，2017

[Karpathy2017] Karpathy，a .，[https://medium.com/@karpathy/software-2-0-a64152b37c35](https://medium.com/@karpathy/software-2-0-a64152b37c35)，2017

[LeCun 2017]纽约 le Cun，[https://www.facebook.com/yann.lecun/posts/10155003011462143](https://www.facebook.com/yann.lecun/posts/10155003011462143)，2017

麦金尼，W. (2010 年 6 月)。python 中统计计算的数据结构。《第九届 Python 科学会议论文集》(第 445 卷，第 51–56 页)。德克萨斯州奥斯汀:SciPy。

[Patil2010] Patil，a .，D. Huard 和 C.J. Fonnesbeck。(2010)PyMC:Python 中的贝叶斯随机建模。统计软件杂志，35(4)，第 1-81 页

[塔克里蒂，1994 年]接口，24(3)，144–146。从 http://www.jstor.org/stable/25061891[取回](http://www.jstor.org/stable/25061891)