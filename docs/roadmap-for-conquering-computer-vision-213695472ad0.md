# 征服计算机视觉的路线图

> 原文：<https://towardsdatascience.com/roadmap-for-conquering-computer-vision-213695472ad0?source=collection_archive---------8----------------------->

## 计算机视觉如何入门

写博客给 ace 机器学习提供指导已经成为一种传统。我很难找到任何这样的计算机视觉路线图和任务清单。作为一名视觉爱好者和顾问，我发现很多人询问掌握计算机视觉的具体路线图(在技能、课程等方面)。下面的博客试图达到这个目的。

应该注意的是，这将是一个长期的、以努力为导向的路线图。我个人不鼓励人们在学习一个硬核领域的时候走捷径。我相信，按照上面提到的路线图，你会在正确理解事物的基础上，在正确的轨道上完成大量严谨的愿景工作。够兴奋了吧？没关系！

## 什么是计算机视觉？

计算机视觉是人工智能的一个子领域，旨在使机器足够智能以理解图像。由于机器学习(和深度学习)是当今世界上最活跃的人工智能子集之一，在机器学习框架中处理计算机视觉将是合适和明智的。为了理解我将要提出的路线图的全部本质，考虑一个通用的机器学习过程是很重要的。

注意:下面的部分有点技术性。只想开始使用路线图的人应该跳过这一步，阅读[食谱部分](#efb0)

## 机器学习是如何工作的？

每个机器学习系统都可以分解为以下两个子系统:

1.  从原始数据(图像、音频、文本等)中提取特征
2.  在特征空间上拟合学习算法

学习算法本质上需要 n 维特征空间来绘制决策边界(在分类范例中)或拟合超平面(就线性回归而言)。为了将原始数据转换到特征空间，我们通常需要两件重要的事情:

a.领域/业务知识

b.数据处理知识

领域知识告诉我们在我们的商业决策中研究数据的重要因素(考虑一个医生告诉你在 MRI 中寻找一个**圆**来检测肿瘤)。然后，数据处理知识将这些因素(来自领域知识)编码到来自数据处理知识的特征空间(考虑使用图像处理从 MRI 检测圆)。

数据处理对于从原始数据中提取重要特征至关重要。如果原始数据是图像的形式，**图像处理**就是我们需要的数据处理知识。如果数据是文本的，我们需要有语言学和文本挖掘知识来提取特征。每种类型的数据都有相应的处理知识。

## 深度学习有多不一样？

深度学习是机器学习的一个子集，有其技术根源。在不深入研究深度学习背后的技术细节的情况下，应该理解的是，深度学习使我们能够摆脱特征提取步骤。我们只是有一个学习算法来做所有的事情。这些深度学习算法可以直接在原始数据空间上工作。现在所有的人工智能炒作都是因为深度学习。

# 配方是什么？

由于我们有两种不同的学习范式，我们可以以两种不同的形式学习和应用计算机视觉。由传统机器学习算法支持的计算机视觉被称为经典计算机视觉。如果计算机视觉是基于深度学习的，则称为现代计算机视觉。

## 经典计算机视觉路线图

与任何机器学习系统一样，经典计算机视觉也由两个主要子系统组成:

1.  图像处理:将数据编码到合理的特征空间(与特征提取相同)
2.  学习算法:适应该空间中的任何相关模型

所以伙计们，掌握计算机视觉与理解上述两件事有着重要的关系。我列出了一些图像处理和计算机视觉的课程来开始学习:

1.  关于[计算机视觉介绍的 Udacity 课程](https://classroom.udacity.com/courses/ud810)
2.  YouTube 上的穆巴拉克·沙阿博士课程
3.  一门纯机器学习的课程对我们的第二步也很重要。你可以选修任何现有的课程。比如吴恩达在 T4 Coursera 上的课程或者南多在 YouTube 上的课程
4.  还需要掌握 OpenCV 等一些开源工具，以便快速实验和学习。你可以看看一些很酷的 [PyImageSearch](https://www.pyimagesearch.com/category/tutorials/) 和 [LearnOpenCV](https://www.learnopencv.com/) 博客

## 现代计算机视觉路线图

现代计算机视觉系统基本上建立在深度学习的基础上。它给了我们灵活性，我们可以在没有经典图像处理知识的情况下完成许多视觉任务。要采用这种方法，请尝试以下课程:

1.  斯坦福的 [Cs231n](https://www.youtube.com/watch?v=vT1JzLTH4G4&list=PL3FW7Lu3i5JvHM8ljYj-zLfQRF3EO8sYv) (用于视觉对象识别的卷积网络)
2.  安德鲁关于[卷积神经网络的课程](https://www.coursera.org/learn/convolutional-neural-networks)
3.  一般深度学习课程，查这个[专精](https://www.coursera.org/specializations/deep-learning)。

我强烈建议读者在深入研究现代计算机视觉之前，至少对经典计算机视觉有所了解。这对以后很有帮助。

# *结论*

列出的大纲将使你们能够开始计算机视觉的不断学习之旅。肯定是很费时间的。这肯定需要很多努力，但这是最基本的要求。我希望每个人都意识到为什么她/他要应用某些算法。该路线图还将帮助您设计和构建针对新问题的定制解决方案。

我花了半个博客来描述一个通用的机器学习过程。你可以把相同的路线图推广到人工智能的其他子领域，包括自然语言处理、语音识别等。