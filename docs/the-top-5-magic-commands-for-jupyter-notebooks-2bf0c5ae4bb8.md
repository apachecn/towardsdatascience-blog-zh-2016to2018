# Jupyter 笔记本的 5 大神奇命令

> 原文：<https://towardsdatascience.com/the-top-5-magic-commands-for-jupyter-notebooks-2bf0c5ae4bb8?source=collection_archive---------7----------------------->

![](img/b1e6136b576ef5ee62af2a8edd81a9b0.png)

Photo by [Jez Timms](https://unsplash.com/@jeztimms?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

Jupyter 笔记本是一种基于网络的交互式工具，机器学习和数据科学社区经常使用。它们用于快速测试，作为报告工具，甚至作为在线课程中高度复杂的学习材料。我最喜欢它们的一个特性叫做**魔法命令**。这些快捷方式极大地扩展了笔记本电脑的功能。这里列出了成为一名更好的数据向导你应该首先学会的五个咒语。我包含了代码片段来说明它们的用法，但是这里也有一个完整的笔记本。

## %time、%timeit 和%%time

你想知道你的代码需要运行多长时间吗？不出所料，你需要使用的魔法命令是**时间**及其变体。我大量使用细胞魔法版( **%%time** )。这是对代码进行基准测试并向其他人指出他们需要多少时间来重新运行您的结果的一种快速方法。

## %matplotlib

如果你以前上过在线课程，你可能会认出这个神奇的命令和 **inline** 参数的组合。使用此命令可确保 Jupyter 笔记本显示您的绘图。这可能是每一个基于笔记本的报告最关键的魔法命令。

## %load_ext 自动重装

这个神奇的命令允许你加载最重要的扩展:**自动加载**。如果没有它，你每次修改你引用的代码时都必须重新加载内核。代码片段为您提供了一个如何使用它的简单示例:

## %系统

如果您想使用 shell，这个神奇的命令会帮您实现。这是非常好的快速检查当前目录和类似的事情。它看起来不怎么样，但却是你腰带里的好工具。

## %谁

这个神奇的命令做了一件非常好的事情:向您显示您的环境中的变量列表。您还可以添加一个参数来定义您想要查看的变量类型，例如函数。

当然，还有比我在这篇文章中提到的更多的魔法命令。官方文件见[此处](https://ipython.readthedocs.io/en/stable/interactive/magics.html)。你用什么魔法命令，为什么？请在评论中或在 [Twitter](https://twitter.com/TimoBohm) 上告诉我。**感谢阅读，留点👏🏻如果这对你有帮助，让我们继续学习吧！**