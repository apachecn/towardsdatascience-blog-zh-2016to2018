# 以下是如何使用 Python 将数据预处理速度提高 2-6 倍的方法

> 原文：<https://towardsdatascience.com/heres-how-you-can-get-a-2-6x-speed-up-on-your-data-pre-processing-with-python-847887e63be5?source=collection_archive---------5----------------------->

## 使用这 3 行代码，您的预处理速度提高了 2-6 倍！

![](img/48cab8f7da9455408d8ada4eb48e5157.png)

> 想获得灵感？快来加入我的 [**超级行情快讯**](https://www.superquotes.co/?utm_source=mediumtech&utm_medium=web&utm_campaign=sharing) 。😎

Python 是所有机器学习的首选编程语言。它易于使用，有许多奇妙的库，使处理数据变得轻而易举！但是，当我们处理大量数据时，事情就变得更加棘手了。

如今,“大数据”一词通常指的是至少有几十万甚至*百万*个数据点的数据集！在这样的规模下，每一个小的计算都会累积起来，我们在对流水线的每一步进行编码时都需要考虑效率。当考虑我们的机器学习系统的效率时，一个经常被忽略的关键步骤是*预处理*阶段，在这里我们必须对我们所有的数据点应用某种操作。

默认情况下，Python 程序使用单个 CPU 作为单个进程执行。大多数为机器学习而制造的现代机器都有*至少* 2 个 CPU 内核。这意味着，以 2 个 CPU 内核为例，当您运行预处理时，默认情况下 50%或更多的计算机处理能力不会做任何事情！当您使用 4 核(现代英特尔 i5)或 6 核(现代英特尔 i7)时，情况会变得更糟。

但是值得庆幸的是，在一个内置的 Python 库中有一个隐藏的特性，让我们可以利用我们所有的 CPU 内核！多亏了 Python 的`concurrent.futures`模块，只需要 3 行代码就可以将一个普通的程序变成一个可以跨 CPU 内核并行处理数据的程序。

# 标准方法

让我们举一个简单的例子，我们在一个文件夹中有一个图像数据集；也许我们甚至有成千上万张这样的图片！为了缩短处理时间，我们这里使用 1000。我们希望在将图像传递到我们的深度神经网络之前，将所有图像的大小调整为 600x600。这是你在 GitHub 上经常看到的一些非常标准的 Python 代码的样子。

这个程序遵循一个您在数据处理脚本中经常看到的简单模式:

1.  您从想要处理的文件(或其他数据)列表开始。
2.  使用一个`for`循环一次处理一条数据，然后在每次循环迭代中运行预处理

让我们在一个有 1000 个 jpeg 文件的文件夹上测试这个程序，看看它需要运行多长时间:

```
time python standard_res_conversion.py
```

我的 i7–8700k 有 6 个 CPU 内核，运行时间为 **7.9864 秒**！对于这么高端的 CPU 来说似乎有点慢。让我们看看我们能做些什么来加快速度。

# 最快的方法

为了理解我们希望 Python 如何并行处理事物，直观地思考并行处理本身是有帮助的。假设我们必须执行同样的任务，将钉子敲进一块木头，我们的桶里有 1000 颗钉子。如果我们说每个钉子需要 1 秒钟，那么用一个人我们可以在 1000 秒内完成这项工作。但是如果我们团队有 4 个人，我们会把桶分成 4 个相等的堆，然后团队中的每个人都会处理他们自己的钉子堆。用这种方法，我们只需 250 秒就能完成！

在我们有 1000 张图片的例子中，我们可以让 Python 为我们做类似的事情:

1.  将 jpg 文件列表分成 4 个小组。
2.  运行 Python 解释器的 4 个独立实例。
3.  让 Python 的每个实例处理 4 组较小数据中的一组。
4.  综合 4 个过程的结果，得到最终的结果列表

最棒的是 Python 为我们处理了所有困难的工作。我们只需告诉它我们想要运行哪个函数，以及要使用多少 Python 实例，然后它会完成所有其他的工作！我们只需要修改 **3 行代码**。

从上面的代码可以看出:

```
**with** concurrent.futures.ProcessPoolExecutor() **as** executor:
```

启动的 Python 进程和 CPU 内核一样多，在我的例子中是 6 个。实际的处理线是这样的:

```
executor.map(load_and_resize, image_files)
```

**executor.map()** 将您想要运行的函数和一个列表作为输入，其中列表的每个元素都是函数的一个**单个输入。由于我们有 6 个内核，我们将同时处理列表中的 6 个项目！**

如果我们再次运行我们的程序，使用:

```
time python fast_res_conversion.py
```

我们得到了 1.14265 秒的运行时间，接近 x6 的加速！

*注意:产生更多的 Python 进程和在它们之间移动数据会有一些开销，所以你不会总是获得这么大的速度提升。但总的来说，你的速度提升通常会非常显著*

# 总是超级快吗？

当您有一系列要处理的数据，并且要对每个数据点执行类似的计算时，使用 Python 并行池是一个很好的解决方案。但是，它并不总是完美的解决方案。并行池处理的数据不会以任何可预测的顺序进行处理。如果您需要将处理的结果按照特定的顺序排列，那么这种方法可能不适合您。

您正在处理的数据也需要是 Python 知道如何“处理”的类型。幸运的是，这些很常见。来自官方 Python 文档:

*   `None`、`True`和`False`
*   整数、浮点数、复数
*   字符串，字节，字节数组
*   仅包含可选择对象的元组、列表、集合和字典
*   在模块顶层定义的函数(使用`[def](https://docs.python.org/3/reference/compound_stmts.html#def)`，而不是`[lambda](https://docs.python.org/3/reference/expressions.html#lambda)`
*   在模块顶层定义的内置函数
*   在模块顶层定义的类
*   此类类的实例，其`[__dict__](https://docs.python.org/3/library/stdtypes.html#object.__dict__)`或调用`[__getstate__()](https://docs.python.org/3/library/pickle.html#object.__getstate__)`的结果是可 picklable 的(详见[章节 pick Class Instances](https://docs.python.org/3/library/pickle.html#pickle-inst))。

# 喜欢学习？

在推特[上关注我，我会在这里发布所有最新最棒的人工智能、技术和科学！也请在 LinkedIn](https://twitter.com/GeorgeSeif94)[上与我联系！](https://www.linkedin.com/in/georgeseif/)