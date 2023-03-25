# 使用 Python 并发期货优化数据准备代码

> 原文：<https://towardsdatascience.com/optimize-data-preparation-code-using-python-concurrent-futures-97a15ac580f6?source=collection_archive---------14----------------------->

![](img/f198db31792bc7710c1df72b14638fa1.png)

*最初发表于*【www.easy-analysis.com】

> **只需几行代码，就能让您的 Python 代码为数据准备提供更快的执行速度。利用内置的* [*并发期货*](https://docs.python.org/3/library/concurrent.futures.html)*

*这篇文章将讨论并展示在执行 Python 代码进行数据准备时，如何通过添加几行额外的代码来利用所有的 CPU 内核。*

# *常见的数据准备任务*

*数据科学中一个非常常见的任务是数据准备。一个常见的任务可能是为模型训练准备图像。下面的代码片段演示了遍历文件夹中的图像、操作每个图像并最终保存每个更改的常见任务。*

# *创建文件 compress.py*

*该文件应该包含以下代码。*

*函数 compress_image()检查图像的尺寸，并根据设置的 max_dim 调整尺寸。缩放图像尺寸时，纵横比保持不变。调整大小时，图像保存在内存中，而不是在光盘上，并检查大小。如果不满足 set image_size，则使用变量 max_dim 的较低值递归调用该函数，否则，用新尺寸覆盖图像。如果你想了解更多关于库 [PIL](https://pillow.readthedocs.io/en/5.3.x/) 和[木卫一的信息。BytesIO](https://docs.python.org/3/library/io.html) 请访问各自的文档。*

*下一个任务是使用这个函数来测量对任意数量的图像执行操作需要多长时间。出于测试的目的，我们将再创建一个 Python 文件。*

# *创建文件 test_compress.py*

*该文件应该包含以下代码。*

*上面的代码创建了一个特定文件夹中所有图像文件的列表，在我的例子中是“../data/test_compres/*。png”，此外，我只希望将 PNG 文件添加到列表中。最后，对于列表中的每个图像路径，调用函数 compress_image(file_name)。库时间用于测量执行文件所用的时间。*

*运行 test_compress.py 文件打印时间 **6.6042399406433105** 秒。这有点慢，因此，让我们通过使用 Pythons 的一个核心库 Concurrent futures 来提高速度。*

# *使用 Python 并发期货*

*python 中的并发未来允许我们利用执行代码的机器上所有可用的内核。我们运行的第一个示例只使用了我的 Mac 上四个可用内核中的一个。让我们看看，如果我们使用全部四种方法，我们可以提高多少时间。*

*为了进行这种改进，我们需要导入一个新的库，并稍微更改文件 test_compress.py 中的代码。修改后的 test_compress.py 文件现在应该包含以下内容。*

*这个版本的文件 test_compress.py 与以前的版本略有不同。导入了一个新的库 concurrent.futures。现在以不同的方式调用了进一步的函数 compress_image()。并发期货用于创建可用资源池，我们使用 map 为 list_files 中的每个项目调用函数 compress_image()。就这样，现在让我们再次运行文件。*

*运行 test_compress.py 文件现在打印出时间 **3.422382116317749** 秒。这是大约 90%的改进。*

# *结论*

*您可能已经注意到，如果一个内核可以在大约 6 秒内完成任务，那么四个内核可以在 1.5 秒内完成。然而，这通常取决于你的代码在做什么，对于其他任务来说，它甚至可以扩展得很好。然而，仅仅通过增加两行代码就实现了 **90%** 的改进，这实在是太棒了。*

*为了提高 Python 代码的性能，可能还有许多其他的优化方法。一个有趣的方法是使用 [Cython](https://cython.org/) 将你的 Python 代码编译成 C 代码。Cython 做得很好，你不需要知道任何 C 编程。使用 Cython 的话题不在本文的讨论范围之内，但是，这是一个值得探讨的有趣话题。*

*如果你有兴趣了解我更多。请访问我在 LinkedIn 上的个人简介[https://www.linkedin.com/in/vedranmarkulj/](https://www.linkedin.com/in/vedranmarkulj/)*

*感谢阅读。如果你对我写的关于机器学习和类似主题的未来帖子感兴趣，请在 [Medium](https://medium.com/@vedranmarkulj) 和 [LinkedIn](https://www.linkedin.com/in/vedranmarkulj/) 上关注我。更多文章即将发表。*