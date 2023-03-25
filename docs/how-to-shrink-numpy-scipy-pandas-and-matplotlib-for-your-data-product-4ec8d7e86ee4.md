# 如何为您的数据产品缩小 NumPy、SciPy、Pandas 和 Matplotlib

> 原文：<https://towardsdatascience.com/how-to-shrink-numpy-scipy-pandas-and-matplotlib-for-your-data-product-4ec8d7e86ee4?source=collection_archive---------6----------------------->

如果你是一名在[微服务](https://en.wikipedia.org/wiki/Microservices)框架中部署数据产品的数据科学家或 Python 开发人员，你很有可能需要利用 NumPy、SciPy、Pandas 或 Matplotlib 的公共科学生态系统模块。微服务中的“微”,建议你应该保持组件尽可能的小。然而，当通过 PIP 安装时，这四个模块非常庞大！

让我们探索为什么 NumPy，SciPy，Pandas，Matplotlib 在通过 PIP 安装时如此庞大，并设计一个关于如何在微服务框架内优化您的数据产品的策略(使用[源代码](https://github.com/szelenka/shrink-linalg))。

![](img/eeae85fe7091e3ff786c9a09540dde6e.png)

Python Scientific Modules

我参与的大多数数据产品都是通过 Docker 部署的，所以本文将使用该框架。与微服务框架的“微”保持一致，第一个 [Docker 最佳实践](https://docs.docker.com/develop/dev-best-practices/)围绕着保持你的图像尺寸**小**。如果你对缩小 Docker 图片尺寸的最佳实践感兴趣，在 [Medium](https://medium.com/) 上有很多帖子。这篇文章将完全专注于减少 NumPy、SciPy、Pandas 和 Matplotlib 消耗的磁盘大小。

# 只使用预编译的二进制文件？

关于 NumPy、SciPy、Pandas 和 Matplotlib，你可以在互联网上读到的一般建议是通过你的 linux 包管理器(即 apt-get install -y python-numpy)来安装它们。然而，在实践中，那些包管理人员经常在官方发布的版本后面跟踪多个版本。如果您的数据产品需要这些模块的一个特定版本，这根本就行不通。

![](img/1a1666b505a7c7930d111dcbf231742a.png)

Just trust the package manager?

Anaconda 发行版用比 PIP 更少的内存完成了编译这些库的工作，那么为什么不直接使用 Python 发行版呢？根据我的经验，如果您已经处于需要特定版本的 NumPy、SciPy、Pandas 或 Matplotlib 的状态，那么您很可能还需要来自其他软件包的特定版本。不幸的消息是，那些其他的包可能不会存在于 Conda 库中。所以简单地使用 Anaconda 或 [miniconda](https://conda.io/miniconda.html) ，并引用 conda 存储库不会满足您的需求。更不用说您的 Docker 映像中不需要的 Anaconda 的额外膨胀。记住，我们想让事情尽可能的小。

# 码头工人历史

要查看构建 Docker 映像时运行的命令的逐层影响，[历史命令](https://docs.docker.com/v17.09/engine/reference/commandline/history/)非常有用:

```
$ docker history shrink_linalgIMAGE               CREATED              CREATED BY                                      SIZE                COMMENT435802ee0f42        About a minute ago   /bin/sh -c buildDeps='build-essential gcc gf…   508MB
```

在上面的示例中，我们可以看到一个层产生了 **508MB** ，而我们在该层中所做的只是使用以下命令安装 NumPy、SciPy、Pandas 和 Matplotlib:

```
pip install numpy==1.15.1 pandas==0.23.4 scipy==1.1.0 matplotlib==3.0.0
```

我们还可以使用 [du 命令](https://manpages.debian.org/stretch/coreutils/du.1.en.html)查看映像中消耗的详细包磁盘空间:

```
$ docker run shrink_linalg /bin/bash -c "du -sh /usr/local/lib/python3.6/site-packages/* | sort -h"...
31M /usr/local/lib/python3.6/site-packages/matplotlib
35M /usr/local/lib/python3.6/site-packages/numpy
96M /usr/local/lib/python3.6/site-packages/pandas
134M /usr/local/lib/python3.6/site-packages/scipy
```

这些模块是巨大的！你可能会说这些模块的功能需要那么大的磁盘空间。但是它们不需要这么大。我们可以删除很大一部分(**高达 60%** )不必要的磁盘空间，而不会影响模块本身的性能！

# 爆破优化

NumPy 和 SciPy 的优势之一是对线性代数方法进行了优化。为此，有许多 LinAlg 优化器。在这篇文章中，我们只是使用了 [OpenBLAS](https://www.openblas.net/) 。但是我们需要验证是否安装了 NumPy 和 SciPy 来利用这个库。

```
$ docker run shrink_linalg /bin/bash -c "python -c 'import numpy as np; np.__config__.show();'"...
openblas_info:
    libraries = ['openblas', 'openblas']
    library_dirs = ['/usr/lib']
    language = c
    define_macros = [('HAVE_CBLAS', None)]
...
```

这里重要的是让 OpenBLAS 被识别并映射到正确的目录。这将使 NumPy 和 SciPy 能够利用该库中的 LinAlg 优化。

# PIP 安装选项

当在 Docker 容器中通过 PIP 安装时，保留缓存是没有意义的。让我们添加一些标志来指示 PIP 如何工作。

*   — no-cache-dir
    PIP 使用[缓存](https://pip.pypa.io/en/stable/reference/pip_install/#caching)来防止重复的 HTTP 请求，以便在安装到本地系统之前从存储库中提取模块。当在 Docker 映像中运行时，没有必要保留这个缓存，所以用这个标志禁用它。
*   —编译
    将 Python 源文件编译成字节码。在 Docker 映像中运行时，不太可能需要调试成熟的已安装模块(如 NumPy、SciPy、Pandas 或 Matplotlib)。
*   — global-option=build_ext
    为了通知 C 编译器我们想要在编译和链接期间添加额外的标志，我们需要设置这些额外的“全局选项”标志。

# c 编译器标志

Python 有一个名为 [Cython](http://cython.org/) 的 C-Extension 包装器，它使开发人员能够以类似 Python 的语法编写 C 代码。这样做的好处是，它允许对 C 语言进行大量的优化，同时也便于编写 Python。NumPy，SciPy 和 Pandas 大量利用 Cython！Matplotlib 似乎也包含一些 Cython，但程度要小得多。

这些编译器标志被传递给安装在 docker 文件中的 GNU 编译器。开门见山地说，我们只打算调查其中的一小部分:

*   [禁用调试语句](https://gcc.gnu.org/onlinedocs/gcc/Debugging-Options.html#Debugging-Options) ( **-g0** )
    因为我们将数据产品粘贴到 Docker 映像中，所以我们不太可能对来自 NumPy、SciPy、Pandas 或 Matplotlib 的 Cython 代码进行任何实时调试。
*   移除符号文件 ( **-Wl，— strip-all** )
    如果我们永远不会在 Docker 映像中调试这些包的构建，那么保留调试所需的符号文件也没有意义。
*   [针对几乎所有支持的优化进行优化，这些优化不涉及空间速度权衡](https://gcc.gnu.org/onlinedocs/gcc/Optimize-Options.html#Optimize-Options) ( **-O2** )或[针对磁盘空间的优化](https://gcc.gnu.org/onlinedocs/gcc/Optimize-Options.html#Optimize-Options) ( **-Os** )
    这些优化标志面临的挑战是，虽然它可以增加/减少磁盘大小，但它也会操纵编译后的二进制文件的运行时性能！如果没有明确测试它对我们的数据产品的影响，这些可能是有风险的(但同时，它们是非常有效的)。
*   [头文件的位置](https://gcc.gnu.org/onlinedocs/cpp/Search-Path.html)(**-I/usr/include:/usr/local/include**)
    明确告诉 GCC 在哪里可以找到编译 Cython 模块所需的头文件。
*   库文件的位置(**-L/usr/lib:/usr/local/lib**)
    明确告诉 GCC 在哪里可以找到编译 Cython 模块所需的库文件。

但是在通过 PIP 安装期间设置这些 CFLAG 有什么影响呢？

缺少调试信息可能会使有经验的开发人员提高警惕，但是如果您以后真的需要它们，您总是可以使用标志重新构建 Docker 映像，以再现任何发生的 stacktrace 或 core dump。更大的问题是不可重现的异常..没有符号文件是无法诊断的。但是，没有人会以码头工人的形象这样做。

# 带 CFLAG 比较的 PIP

![](img/63b7bf4edc8166c2e86a5bb4f92e13f5.png)

Disk consumption inside Docker image

采取最佳优化 CFLAG 策略，您可以将 Docker 映像的磁盘占用空间减少 **60%** ！

对于我的数据产品，我只是删除了 debug/符号，并且不执行任何额外的优化。如果你想知道其他人是如何执行类似的 CFLAG 优化的，请查看官方的 Anaconda 食谱，包括 [NumPy](https://github.com/AnacondaRecipes/numpy-feedstock) 、 [SciPy](https://github.com/AnacondaRecipes/scipy-feedstock) 、 [Pandas](https://github.com/AnacondaRecipes/pandas-feedstock) 和 [Matplotlib](https://github.com/AnacondaRecipes/matplotlib-feedstock) 。这些都是针对 Python 的 Anaconda 发行版进行优化的，可能与您的特定 Docker 数据产品部署相关，也可能不相关。

# 单元测试

仅仅因为我们能够收缩每个模块的编译二进制文件，我们就应该进行健全性检查，以验证我们使用的 C 编译器标志没有影响这些模块的性能。

所有这些包都利用了 [pytest](https://docs.pytest.org/en/latest/contents.html) 模块，我们没有将它安装在 Docker 映像中，因为它在生产中没有提供任何价值。我们可以安装它并从内部执行测试，尽管:

*   我们的 Docker 映像中的 NumPy 测试:

```
$ docker run shrink_linalg /bin/bash -c "pip install pytest; python -c \"import numpy; numpy.test('full');\""4675 passed ... in 201.90 seconds
```

*   Docker 映像内部的 SciPy 测试:

```
$ docker run shrink_linalg /bin/bash -c "pip install pytest; python -c \"import scipy; scipy.test('full');\""13410 passed ... in 781.44 seconds
```

*   我们 Docker 图片中的熊猫测试:

```
$ docker run shrink_linalg /bin/bash -c "pip install pytest; python -c \"import pandas; pandas.test();\""22350 passed ... in 439.35 seconds
```

有相当多的测试[被跳过，而](https://docs.pytest.org/en/latest/skipping.html)被 x 失败，这取决于您的数据产品和/或安装的版本，这些都是可以预料到的，或者您可能需要进一步研究。在大多数情况下，如果不是严重的失败，很可能可以继续。

希望当您的数据产品需要 NumPy、SciPy、panases 或 Matplotlib 时，这将有助于引导您使用更小的 Docker 图像！我鼓励您尝试其他 CFLAG，并测试磁盘空间和单元测试性能。

[源代码](https://github.com/szelenka/shrink-linalg)