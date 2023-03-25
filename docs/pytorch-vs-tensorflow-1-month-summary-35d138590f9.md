# PyTorch 与 TensorFlow: 1 个月总结

> 原文：<https://towardsdatascience.com/pytorch-vs-tensorflow-1-month-summary-35d138590f9?source=collection_archive---------0----------------------->

*py torch 与 TensorFlow 合作一个月后的对比。*

![](img/dc186e58178a20d385cd7ec0c1eed138.png)

在我深度学习工作的大部分时间里，我一直是 TensorFlow 的用户。然而，当我加入[英伟达](https://medium.com/u/ab69c39a85e1?source=post_page-----35d138590f9--------------------------------)时，我们决定改用 py torch——只是作为一种测试。以上是我对它的体验。

# 装置

安装非常简单明了。PyTorch 可以通过 PIP 安装，也可以从源代码构建。PyTorch 还提供 Docker 图像，可以用作您自己项目的基础图像。

PyTorch 不像 TensorFlow 那样有指定的 CPU 和 GPU 版本。虽然这使安装更容易，但如果您想同时支持 CPU 和 GPU 的使用，它会生成更多的代码。

值得注意的是 PyTorch 还没有提供正式的 windows 发行版。有到 windows 的非官方端口，但是没有 PyTorch 的支持。

# 使用

PyTorch 提供了一个非常 Pythonic 化的 API。这与 TensorFlow 有很大不同，在 tensor flow 中，您应该定义所有的张量和图形，然后在一个会话中运行它。

在我看来，这会产生更多但更干净的代码。PyTorch 图必须在从 PyTorch `nn.Module`类继承的类中定义。当图形运行时，调用一个`forward()`函数。使用这种“约定优于配置”的方法，图的位置总是已知的，并且变量不会在代码的其余部分全部定义。

这种“新”的方法需要一些时间来适应，但我认为如果你以前在深度学习之外使用过 Python，这是非常直观的。

根据一些评论，PyTorch 还在许多模型上显示出比 TensorFlow 更好的性能。

# 证明文件

文件大部分是完整的。我总能找到函数或模块的定义。与 TensorFlow 相反，在 tensor flow 中，所有函数都只有一个页面，PyTorch 每个模块只使用一个页面。如果你来自谷歌，寻找一个功能，这就有点困难了。

# 社区

显然 PyTorch 的社区没有 TensorFlow 的社区大。然而，许多人喜欢在空闲时间使用 PyTorch，即使他们在工作中使用 TensorFlow。我认为 PyTorch 一退出 Beta，这种情况就会改变。

目前来看，在 PyTorch 中找到精通的人还是有点困难。

这个社区非常大，官方论坛上的问题通常都能很快得到答案，因此很多伟大的神经网络实现的例子都被翻译成 PyTorch。

# 工具和助手

尽管 PyTorch 提供了相当多的工具，但是缺少一些非常有用的工具。缺少的最有用的工具之一是 TensorFlow 的 TensorBoard。这使得虚拟化有点困难。

还有一些很常用的帮手不见了。这比 TensorFlow 需要更多的自写代码。

# 结论

PyTorch 是 TensorFlow 的绝佳替代品。由于 PyTorch 仍处于测试阶段，我希望在可用性、文档和性能方面有更多的变化和改进。

PyTorch 非常 pythonic 化，使用起来感觉很舒服。它有一个很好的社区和文档。据说比 TensorFlow 还快一点。

然而，与 TensorFlow 相比，这个社区仍然很小，并且缺少一些有用的工具，如 TensorBoard。