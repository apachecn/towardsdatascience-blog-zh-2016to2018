# 谷歌的 AutoML 将改变企业使用机器学习的方式

> 原文：<https://towardsdatascience.com/googles-automl-will-change-how-businesses-use-machine-learning-c7d72257aba9?source=collection_archive---------4----------------------->

![](img/9fec0319833b4cfc5778815d6a8ea5e3.png)

> 想获得灵感？快来加入我的 [**超级行情快讯**](https://www.superquotes.co/?utm_source=mediumtech&utm_medium=web&utm_campaign=sharing) 。😎

Google 的 AutoML 是一个新兴的(alpha 阶段)机器学习工具云软件套件。它基于谷歌在图像识别领域的最新研究，名为[神经架构搜索](https://arxiv.org/abs/1707.07012) (NAS)。NAS 基本上是一种算法，在给定特定数据集的情况下，它会搜索最佳神经网络来对该数据集执行特定任务。AutoML 是一套机器学习工具，允许人们轻松训练高性能的深度网络，而不需要用户拥有任何深度学习或人工智能的知识；你需要的只是标签数据！Google 将使用 NAS 为您的特定数据集和任务找到最佳网络。他们已经展示了他们的方法如何能够实现比手工设计的网络更好的性能。

AutoML 完全改变了整个机器学习游戏，因为对于许多应用程序来说，不需要专业技能和知识。很多公司只需要深度网络来完成更简单的任务，比如图像分类。在这一点上，他们不需要雇佣 5 个机器学习博士；他们只需要有人能够处理移动和组织他们的数据。

毫无疑问，企业如何使用“人工智能”的这种转变将带来变化。但是我们看到的是什么样的变化呢？这种变化对谁有利？所有进入机器学习领域的人会发生什么？在这篇文章中，我们将分析谷歌的 AutoML，以及总体上向[软件 2.0](https://medium.com/@karpathy/software-2-0-a64152b37c35) 的转变，对机器学习领域的企业和开发者意味着什么。

## 更多的开发，更少的商业研究

人工智能领域的很多企业，尤其是初创企业，正在深度学习的背景下做相对简单的事情。他们的大部分价值来自于他们最终组装的产品。例如，大多数计算机视觉初创公司都在使用某种图像分类网络，这实际上将是 AutoML 在套件中的第一个工具。事实上，谷歌的 NASNet，在图像分类方面达到了目前的最先进水平，已经在 TensorFlow 中公开可用了！企业现在可以跳过产品管道中复杂的实验研究部分，直接使用迁移学习来完成任务。因为实验研究减少了，更多的商业资源可以花在产品设计、开发和所有重要的数据上。

说到这个…

## **它变得更加关注产品**

从第一点连接，由于更多的时间花在产品设计和开发上，公司会有更快的产品迭代。公司的主要价值将不再是他们的研究有多伟大和前沿，而是他们的产品/技术设计得有多好。设计的好吗？好用吗？他们的数据管道是否以这样一种方式建立，即他们可以快速方便地改进他们的模型？这些将是新的关键问题，以优化他们的产品，并能够比竞争对手更快地迭代。尖端研究也将不再是提高技术性能的主要驱动力。

现在更像是…

## 数据和资源变得至关重要

既然研究是等式中不太重要的部分，公司如何脱颖而出？你如何在竞争中领先？当然，销售、营销以及我们刚刚讨论过的产品设计都非常重要。但是这些深度学习技术性能的巨大驱动力是你的数据和资源。你的数据越清晰、多样且有任务针对性(即质量和数量)，你就越能使用 AutoML 等软件工具改进你的模型。这意味着获取和处理数据需要大量资源。所有这些都部分表明我们正在远离编写大量代码的本质。

它变得更加…

## 软件 2.0:深度学习成为大多数人工具箱中的另一个工具

使用谷歌的 AutoML 你所要做的就是上传你的标签数据，然后嘣，你就搞定了！对于那些没有深入(哈哈，双关语)这个领域，只想利用技术力量的人来说，这是一个大问题。深度学习的应用变得更加触手可及。编码更少，更多地使用工具套件。事实上，对于大多数人来说，深度学习只是他们工具箱中的另一个工具。Andrej Karpathy 写了一篇关于[软件 2.0](https://medium.com/@karpathy/software-2-0-a64152b37c35) 的伟大文章，讲述了我们如何从编写大量代码转向更多的设计和使用工具，然后让人工智能完成剩下的工作。

但是，考虑到所有这些…

## 创造性科学和研究仍有空间

即使我们有这些易于使用的工具，旅程也不会就此结束！当汽车被发明出来的时候，我们并没有停止改进它们，即使现在它们很容易使用。仍然有许多改进可以用来改进当前的人工智能技术。人工智能仍然不是很有创造力，也不能推理或处理复杂的任务。它需要大量的*标签数据*，获取这些数据既昂贵又耗时。训练仍然需要很长时间才能达到最高的准确度。深度学习模型的性能对于一些简单的任务来说是不错的，比如分类，但在本地化等事情上只能做得相当好，有时甚至很差(取决于任务的复杂性)。我们甚至还没有完全理解内部的深层网络。

所有这些都为科学和研究提供了机会，特别是推进当前的人工智能技术。在商业方面，一些公司，特别是科技巨头(如谷歌、微软、脸书、苹果、亚马逊)需要通过科学和研究来创新过去的现有工具，以便竞争。他们都可以获得大量的数据和资源，设计出色的产品，进行大量的销售和营销等。他们真的可以使用更多的东西来使他们与众不同，这可以来自尖端创新。

这留给我们最后一个问题…

## 这一切是好是坏？

总的来说，我认为我们创造人工智能技术的这种转变是一件好事。大多数企业将利用现有的机器学习工具，而不是创建新的工具，因为他们不需要它。许多人都可以接触到接近尖端的人工智能，这意味着对所有人来说都是更好的技术。人工智能也是一个相当“开放”的领域，像吴恩达这样的主要人物创建了非常受欢迎的课程来教授人们这一重要的新技术。让事物变得更容易接近有助于人们适应快节奏的技术领域。

这样的转变以前发生过很多次。编程计算机是从汇编级编码开始的！我们后来转向了 C 之类的东西。今天许多人认为 C 太复杂了，所以他们使用 C++。很多时候，我们甚至不需要像 C++这样复杂的东西，所以我们只是使用 Python 或 R 这样的超高级语言！我们使用手边最合适的工具。如果你不需要一些超低级的东西，那么你就不必使用它(例如 C 代码优化，从零开始的深度网络的 R&D)，可以简单地使用一些更高级的内置的东西(例如 Python，迁移学习，人工智能工具)。

与此同时，在人工智能技术的科学和研究方面的持续努力至关重要。通过设计新的基于人工智能的产品，我们肯定可以为世界增加巨大的价值。但是有一点需要新的科学来推进。人类的创造力永远是有价值的。

## 结论

感谢阅读！我希望你喜欢这篇文章，并从人工智能技术的当前趋势中学到一些新的有用的东西！这是一篇部分自以为是的文章，所以我很想听听你在下面的回应！

# 喜欢学习？

在 twitter 上关注我，我会在这里发布所有最新最棒的人工智能、技术和科学！