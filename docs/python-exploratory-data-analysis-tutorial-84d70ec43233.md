# Python 探索性数据分析教程

> 原文：<https://towardsdatascience.com/python-exploratory-data-analysis-tutorial-84d70ec43233?source=collection_archive---------4----------------------->

*最初发布于*[*https://www . data camp . com/community/tutorials/explorative-data-analysis-python*](https://www.datacamp.com/community/tutorials/exploratory-data-analysis-python)

正如您现在所知道的，Python 数据操作库 Pandas 用于数据操作；对于那些刚刚起步的人来说，这可能意味着这个包只能在预处理数据时方便使用，但事实并非如此:Pandas 还可以很好地探索您的数据，并在您完成数据预处理后存储它。

此外，对于那些一直在关注 DataCamp 的 Python 教程或已经了解了 SciPy、NumPy、Matplotlib 和 Pandas 基础知识的人来说，重温一下您已经积累的一些知识可能是个好主意。

今天的教程实际上将向您介绍一些使用上述所有包高效浏览数据的方法，以便您可以开始对数据进行建模:

*   您将首先学习如何[导入数据](https://www.datacamp.com/community/tutorials/exploratory-data-analysis-python#import)，这是您在开始分析之前需要成功完成的第一步。
*   如果你不确定[探索性数据分析(EDA)](https://www.datacamp.com/community/tutorials/exploratory-data-analysis-python#eda) 是什么，EDA 和数据挖掘的确切区别是什么，本节将在你开始教程之前为你解释！
*   然后，你会得到你的数据的一个基本描述。您将专注于获取一些描述性统计数据，检查数据帧的第一行和最后一行，从数据中检索样本，等等。你会发现这是一个很好的方式来获得对数据的初步感觉，并且可能已经对它有了更好的理解！
*   在收集了一些关于数据的信息后，通过[查询](https://www.datacamp.com/community/tutorials/exploratory-data-analysis-python#closer)或索引数据来更深入地查看它可能也是一个好主意。您可以使用这种技术来测试您对数据的一些基本假设。
*   现在您已经检查了您的数据，您可能已经看到了您的分析可能感兴趣的一些[特性](https://www.datacamp.com/community/tutorials/exploratory-data-analysis-python#features):您将看到哪些特性可以通过特性工程和特性选择对您的分析产生积极的影响。
*   接下来，您将看到初步探索是好的，但您还需要了解您的数据可能带来的[挑战](https://www.datacamp.com/community/tutorials/exploratory-data-analysis-python#challenges)，例如缺失值或异常值，当然，还有您如何应对这些挑战
*   最后，您还将学习如何发现数据中的[模式](https://www.datacamp.com/community/tutorials/exploratory-data-analysis-python#patterns)，方法是使用 Python 数据可视化包 Matplotlib 和 Bokeh 轻松快速地可视化数据，或者使用特定函数计算属性之间的相关性。

**阅读全文** [**这里**](http://As you will know by now, the Python data manipulation library Pandas is used for data manipulation; For those who are just starting out, this might imply that this package can only be handy when preprocessing data, but much less is true: Pandas is also great to explore your data and to store it after you’re done preprocessing the data.  Additionally, for those who have been following DataCamp’s Python tutorials or that have already been introduced to the basics of SciPy, NumPy, Matplotlib and Pandas, it might be a good idea to recap some of the knowledge that you have built up.  Today’s tutorial will actually introduce you to some ways to explore your data efficiently with all the above packages so that you can start modeling your data:  You’ll first learn how to import data, which is the first step that you need to complete successfully before you can start your analysis. If you're not sure what Exploratory Data Analysis (EDA) is and what the exact difference between EDA and Data Mining is, this section will explain it for you before you start the tutorial! Then, you’ll get a basic description of your data. You’ll focus on getting some descriptive statistics, checking out the first and last rows of your DataFrame, retrieving samples from your data, etc. You’ll see that this is a great way to get an initial feeling with your data and maybe understand it a bit better already! After gathering some information on your data, it might also be a good idea to also take a deeper look at it by querying or indexing the data. You can use this technique to test some of the basic hypotheses that you might have about the data. Now that you have inspected your data, you'll probably already see that there are some features that can be of interest to your analysis: you'll see which ones can influence your analysis positively with feature engineering and feature selection. Next, you’ll see that an initial exploration is good, but you will also need to have an idea of the challenges that your data can pose, such as missing values or outliers, and, of course, how you can handle those challenges, and Lastly, you’ll also learn how to discover patterns in your data, by either visualizing your data easily and quickly with the Python data visualization packages Matplotlib and Bokeh, or by using specific functions to compute the correlation between attributes.) **。**

# 导入数据

要开始探索您的数据，您需要从实际加载数据开始。你可能已经知道了这一点，但是多亏了 Pandas 库，这变成了一个简单的任务:你按照惯例以`pd`的形式导入包，然后使用`read_csv()`函数，向它传递可以找到数据的 URL 和一个`header`参数。最后一个参数可以用来确保数据被正确读入:数据的第一行不会被解释为数据帧的列名。

或者，您还可以指定其他参数来确保数据被正确读入:您可以指定与`sep`或`delimiter`参数一起使用的分隔符，与`names`一起使用的列名，或者与`index_col`一起用作结果数据帧的行标签的列。

但是这些并不是您可以添加到`read_csv()`函数中的所有参数。在[文档](http://pandas.pydata.org/pandas-docs/stable/generated/pandas.read_csv.html)中详细阅读该函数及其参数。

注意，在这种情况下，您使用了`read_csv()`，因为数据碰巧是逗号分隔的格式。如果您的文件有另一个分隔符，您也可以考虑使用其他函数加载您的数据，如`read_table()`、`read_excel()`、`read_fwf()`和`read_clipboard`，分别读入一般分隔文件、Excel 文件、固定宽度格式数据和复制到剪贴板的数据。

此外，您会发现`read_sql()`是将 SQL 查询或数据库表读入数据帧的选项之一。对于更多的输入功能，请考虑 Pandas 文档的本节。

![](img/2fcefcd23f3db75efe3e6bb258933d4c.png)

# 什么是探索性数据分析(EDA)？

探索性数据分析(EDA)一方面用于回答问题、测试业务假设、为进一步分析生成假设。另一方面，也可以用它来为建模准备数据。这两者的共同点可能是对您的数据有很好的了解，以获得您需要的答案，或者开发一种直觉来解释未来建模的结果。

有很多方法可以达到这些目标:你可以获得数据的基本描述，可视化它，识别其中的模式，识别使用数据的挑战，等等。

在阅读有关 EDA 的内容时，您会经常看到的一件事情是数据分析。数据概要分析通过描述性统计对数据集进行汇总。您希望使用各种测量方法来更好地了解数据集。数据概要分析的目标是对您的数据有一个坚实的理解，以便您以后可以开始以各种方式查询和可视化您的数据。然而，这并不意味着您不需要迭代:正是因为数据概要分析与汇总您的数据集有关，所以它经常用于评估数据质量。根据数据分析的结果，您可能会决定更正、丢弃或以不同方式处理您的数据。

在下一篇文章中，您将了解更多关于数据分析的内容。

# EDA 和数据挖掘

EDA 不同于数据挖掘，尽管两者密切相关，因为许多 EDA 技术已经被用于数据挖掘。这两者的目标也非常相似:EDA 确实确保您以一种有趣的特性和特性之间的关系将变得更加清晰的方式探索数据。在 EDA 中，您通常会使用各种技术探索和比较许多不同的变量，以搜索和找到系统模式。另一方面，数据挖掘关注的是从数据中提取模式。这些模式提供了对变量之间关系的洞察，可以用来改进业务决策。同样，在这两种情况下，你都没有关于变量之间关系的先验期望或不完整的期望。

但是，一般来说，数据挖掘可以说是更面向应用，而 EDA 关注的是底层现象的基本性质。换句话说，数据挖掘相对不太关心识别所涉及变量之间的特定关系。因此，数据挖掘接受“黑盒”方法来进行数据探索，不仅使用 EDA 中也使用的技术，还使用神经网络等技术来生成有效的预测，但不识别预测所基于的变量之间关系的特定性质。

# 数据的基本描述

就像你在上面读到的，EDA 就是要了解你的数据。要做到这一点，最基本的步骤之一是获得数据的基本描述。对数据的基本描述确实是一个非常宽泛的术语:您可以将其解释为获取数据信息的一种快速而简单的方式，获取数据信息的一种简单、易于理解的方式，对数据的一种基本感觉，等等。

本节不会对这些解释进行区分:它确实会向您介绍一些可以快速收集数据框架信息的方法，这些方法很容易理解。

例如，您可以使用`describe()`函数获得排除 NaN 值的各种汇总统计信息。考虑[这个](https://www.datacamp.com/community/tutorials/exploratory-data-analysis-python)代码示例。

您会看到该函数返回数据的计数、平均值、标准差、最小值和最大值以及分位数。当然，请注意，Python 中有许多包可以提供这些统计数据，包括 Pandas 本身。使用该功能只是获取这些信息的方法之一。

还要注意，如果您还没有这样做，您肯定需要花时间深入研究描述性统计数据。您可以使用这些描述性统计数据来开始评估数据的质量。然后，你将能够决定你是否需要纠正，丢弃或以另一种方式处理数据。这通常是数据分析步骤。EDA 中的这一步旨在更好地理解数据元素及其异常，并了解数据一方面如何与文档相匹配，另一方面如何适应业务需求。

**请注意**在您进行探索性数据分析时，您将回到数据分析步骤，因为您将经历的步骤会影响数据的质量。

既然您已经对数据集有了大致的了解，那么更仔细地查看数据本身也是一个好主意。借助熊猫图书馆的`head()`和`tail()`功能，你可以很容易地分别检查出你的数据帧的第一行和最后一行。

使用下面 DataCamp 轻量块中的`head()`和`tail()`功能检查手写数字数据的第一行和最后五行。你可以在[这个](https://www.datacamp.com/community/tutorials/exploratory-data-analysis-python) DataCamp Light 代码块里试试。

当你不熟悉这类数据时，你会发现`head()`和`tail()`函数的结果并不能说明什么。

你可能只会看到一堆带有数字值的行和列。如果您还没有阅读过[数据集描述](https://archive.ics.uci.edu/ml/machine-learning-databases/optdigits/optdigits.names)的话，请考虑阅读一下，这将为您提供关于数据是如何收集的相关信息，还会说明属性和行数，这可以方便地检查您是否正确导入了数据。

此外，回到最初的发现:行中的数值。乍一看，您可能不认为有问题，因为整数值看起来是正确的，并且当您第一次查看它时没有出现任何标志。

但是，如果您已经在您面前的另一个数据集上完成了所有这些操作，并且该数据集可能具有例如日期时间信息，那么快速浏览这些代码行的结果可能会提出以下问题:“我的数据是作为日期时间读入的吗？”，“这个怎么查？”以及“我如何改变数据类型？”。

这些是您通常会在数据分析步骤中解决的更深层次的问题，将在下一篇文章中解决。

如果您有一个大型数据集，您可以考虑对数据进行采样，作为快速了解数据的一种简单方法。作为第一个简单的方法，你可以使用 Pandas 中的`sample()`函数，就像你在这里看到的。

另一种可能更复杂的方法是创建一个随机索引，然后从数据帧中获取随机行。你会看到下面的代码使用了`random`包，它有一个模块`sample`，可以让你结合`range()`和`len()`对数据进行采样。请注意，您还可以利用`ix`来选择您想要包含在样本中的数据帧的确切行。

如果你不知道为什么在这种情况下使用`ix`, data camp 更具体的教程会有所帮助！它详细介绍了这些更一般的主题。点击上面的链接去看看吧！

现在，让我们练习一下 Python 技巧吧！从这里开始练习[。](https://www.datacamp.com/community/tutorials/exploratory-data-analysis-python)

现在，您已经收集了一些关于数据的基本信息，最好是稍微深入一点，了解数据可能带来的挑战。如果您已经完成了数据分析步骤，您将意识到丢失的值，您将知道哪些值可能是异常值，等等。

*最初发表于*[*【www.datacamp.com】*](https://www.datacamp.com/community/tutorials/exploratory-data-analysis-python)*。*