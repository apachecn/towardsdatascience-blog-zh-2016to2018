# 使用 Jupyter 笔记本对熊猫进行探索性数据分析

> 原文：<https://towardsdatascience.com/exploratory-data-analysis-with-pandas-and-jupyter-notebooks-36008090d813?source=collection_archive---------6----------------------->

![](img/2e0a6dd750425e169c0d2e895f1bd704.png)

A beautiful red panda doing his thing. If you look close enough at that bamboo, you can see the commas. Source: [pixabay](https://pixabay.com/en/panda-red-panda-bear-cat-3503779/)

作为一名数据科学家，我花了大约三分之一的时间查看数据，并试图获得有意义的见解。这些是我最常用的工具。

你是数据科学闪亮世界的新人吗？探索性分析之类的花哨词汇会吓到你吗？别担心，我是来帮你在这个疯狂的世界里导航的。

今天我们将看看两个很棒的工具，紧跟我在 github 项目上上传的代码。一个是 Jupyter 笔记本，一个是 Python 框架叫 Pandas。

如果你是 Python 的新手，我建议你在阅读这篇文章之前先阅读一篇语言教程。我不会用太晦涩的东西，但也不会停下来[解释列表理解](/how-list-comprehensions-can-help-your-code-look-better-and-run-smoother-3cf8f87172ae)。我推荐 O'Reilly 的书“[用 Python 从零开始的数据科学](https://www.amazon.com/Data-Science-Scratch-Principles-Python/dp/149190142X)”，但是任何其他教程都可以。

首先来说说 Jupyter 笔记本。如果您曾经使用过 Python 控制台，您可能已经意识到它是多么有用。通过检查输出是否符合您的预期来轻松调试功能(我不是说这是测试的良好替代品，但是谁没有这样做过呢？)，运行一次昂贵的进程，这样就可以将它们加载到内存中，然后就可以开始在它们的输出上测试其他东西，拥有一个交互式环境有很多好处。

![](img/e9f6e54d0a180b5038892c2f763e61ab.png)

Just picture this but, like, less round, and a bit more squarish. Rectanglish? Is that a word? Source: Pixabay

Jupyter 笔记本也是同样的环境，使用类固醇。这就像运行 Python 控制台工具进行读取-评估-打印循环，但是界面很可爱，并且能够记录和保存您测试的内容。这非常方便，例如，如果你正在写一篇关于 Python 工具的中型文章！

要安装 Jupyter，您只需运行以下 pip 命令:

```
python -m pip install jupyter
```

如果您在 Linux 上，您应该使用这个命令:

```
pip3 install --user jupyter
```

*(注意:本文之前的版本推荐使用 sudo 进行安装。一位* [*善良的读者*](https://www.reddit.com/user/Imanflow) *告诉我这是* [*不安全的，实际上是非常糟糕的做法*](https://stackoverflow.com/questions/21055859/what-are-the-risks-of-running-sudo-pip) *，因为它给了 setup.py 程序 sudo 特权，这通常是不好的，会允许恶意代码运行。你知道的越多！)*

然后要运行程序，打开你的终端，在你想要存储笔记本的文件夹中，然后运行

```
jupyter notebook
```

是的，就这么简单。该命令将初始化您计算机中的服务器，并将您最喜欢的浏览器重定向到该服务器。从那里，只需使用 GUI 创建一个新的笔记本，并打开它。使用 **+** 按钮创建一个新的代码块，使用**“剪切”**按钮删除一个代码块。通过将光标放入代码块并点击**“Shift+Enter”，每个代码块都可以独立运行(尽管不是并发的)。**

既然你已经知道了如何运行 Jupyter 笔记本，那么克隆并拉动我刚刚链接的[项目](https://github.com/StrikingLoo/pandas_workshop)将是明智的。你所要做的就是点击**克隆**绿色按钮，获得链接，然后开始

```
git clone *link*
```

现在有趣的部分，让我们来谈谈熊猫。Pandas 是一个开源 Python 框架，由 PyData 社区维护。它主要用于数据分析和处理，主要用于操作 CSV 文件。

![](img/c7bd30426097129ab2297dd2b7cc48b9.png)

They keep making them cuter and cuter. It’s like each hair is a commit. Source: [pixabay](https://pixabay.com/en/panda-cub-wildlife-zoo-cute-china-649938/)

如果您不知道，CSV 只是一种将数据编码为序列(列)的文件格式，其中每个对象(行)都有一个值。文件中的每一行都是一行，每一个值都用逗号与前一个值隔开，这样就形成了逗号分隔值文件。第一行是为标题保留的，包含每一列的名称。

在本教程中，我们将首先生成一个“员工数据”的模拟数据集，该数据集来自一家非常冷的公司，它只存储每个员工的姓名和工资。数据集将被生成为 CSV 文件，如 [generate_people.py](http://localhost:8888/edit/generate_people.py) 程序所示。

我们首先要做的是使用一些常规的 Python 来生成字典，每个字典代表一个人。如果你更熟悉 JavaScript，每个字典只是一个 JSON，有“名字”、“姓氏”和“薪水”字段。

然后，我们生成一个 Pandas Dataframe:它们在 CSV 文件上的抽象，其中每一列都是 Pandas 系列(可通过一些矢量化运算进行迭代)。

就是这样。如您所见，Dataframe 是作为字典列表和列名的(可选)参数生成的。将它导出到 CSV 文件就像调用 *to_csv* 方法一样简单，只需将文件名作为唯一的参数。

现在，对于读取和处理部分，让我们运行“*分析数据集*”笔记本，它也在存储库中。我建议你打开它，把它放在边上阅读这篇文章，但我会添加相关的片段，以防你在手机上或只是觉得有点懒。

我们首先将 CSV 文件作为数据帧导入，其中包含以下行:

```
**import** **pandas** **as** **pd** 

df = pd.read_csv("random_people.csv")
```

对一些 *k* 使用 df.head( *k* )会让我们看到数据帧的第一个 *k* 行，由于 Jupyter 的魔力，这些行看起来会非常漂亮。这是了解数据的一种简单方法(也是开始处理数据时的主要调试工具)。

为了只看到系列中的一个，我们所要做的就是将其作为字典字段进行索引:

```
df[“*series name*”]
```

您可以调用在任何其他数据分析工具(如 SQL)中使用的任何常用聚合作为方法。如果我想直观地了解一个系列，我最喜欢的方法是 *value_counts* 方法，它显示每个值，以及它在系列中出现的次数，按照幽灵数量的降序排列。其他选项包括*平均值*、*总和*、*计数*和*中位数。*

使用这些方法而不是手动打开 CSV 并自己运行这些函数的实现的一个优点是，其中大多数都是矢量化的(它们使用 SIMD 指令和其他黑魔法)，这意味着它们将快大约 10 倍。这也适用于加法、减法、除法和乘积，它们可以很容易地通过一个系列进行广播。

然而，有时你会想把一个功能应用到一个不那么琐碎的系列中，也许制作熊猫的人没有真正考虑过你的用例。在这种情况下，您可以定义自己的函数，并对您想要修改的系列使用 *apply* 方法。这将是一个有点慢，但仍然运行简单的功能顺利。它相当于 Python 的原生*地图*，将为你的处理*增加大量的灵活性。*

如果您想过滤数据帧，只保留保持某个属性的行，您可以这样做:

```
df_high = df[df["salary"]>=6000]
```

其中*" df[" salary "]>= 6000 "*可以用任何返回一系列布尔值的东西来切换，或者" *df["any series name"]。应用(****f****)"*使得 **f** 为布尔函数。但是，请确定该系列与您要过滤的系列具有相同数量的元素。

最后，请记住，这些过滤后的数据帧在默认情况下是只读的，因为它们是通过引用生成的。如果你想改变它们，只需添加*。loc"* 在数据帧名称之后，第一个括号之前，如下所示:

```
df_low= df.loc[df["salary"]<6000]
```

您也可以将列名作为第二个参数添加到 *loc* 中，以保留单个过滤序列，而不是整个数据帧。这是一个直接来自笔记本的例子。

```
df_low= df.loc[df["salary"]<6000,"salary"]
```

请注意，后者是通过引用制作的切片，因此您对它们所做的任何更改也将在原始数据帧上进行。为了避免这种情况，只需在行尾调用 *copy* 方法。

就是这样，这是一些关于熊猫和笔记本的初步速成课程。我希望你会觉得它很有用，如果你觉得还有什么我应该介绍的，或者有什么你想问我的，你知道在哪里可以找到我。我也欢迎任何批评，不管是好是坏，因为这是我第一篇关于媒体的文章，我还在适应。

如果你想在这方面展开，这里有一篇我写的关于[如何在并行](/trying-out-dask-dataframes-in-python-for-fast-data-analysis-in-parallel-aa960c18a915)上做数据分析的文章。

*有一本 O'Reilly 的书我很喜欢，当我开始我的数据科学之旅时，我发现它非常有用。用 Python* *从零开始叫* [*数据科学，大概也是我得到这份工作的一半原因。如果你读到这里，你可能会喜欢它！*](https://www.bookdepository.com/book/9781491901427/?a_aid=strikingloo&chan=ws)

*附:关注我以获得更多的 Python 教程、技巧和诀窍，如果您发现本文有用，请* [*支持我的写作*](http://buymeacoffee.com/strikingloo) *。*