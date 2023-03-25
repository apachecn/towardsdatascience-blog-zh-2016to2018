# 教程:使用 OpenCV 的网络摄像头绘画应用程序

> 原文：<https://towardsdatascience.com/tutorial-webcam-paint-opencv-dbe356ab5d6c?source=collection_archive---------3----------------------->

这是一个关于如何构建 OpenCV 应用程序的教程，该应用程序可以跟踪对象的移动，用户可以通过移动对象在屏幕上绘图——我称之为网络摄像头绘画。

![](img/9b93e3c67a55ad78a507f29dded77962.png)

# 项目描述

给定实时网络摄像头数据，这个类似 paint 的 python 应用程序使用 OpenCV 库来跟踪感兴趣的对象(在本例中是一个瓶盖),并允许用户通过移动对象来绘制，这使得绘制简单的东西既令人敬畏又具有挑战性。

您可以在此访问完整的项目代码:

 [## acl21/Webcam_Paint_OpenCV

### 更新]:我将不再关注与回购相关的问题或邮件，因为我目前非常忙…

github.com](https://github.com/acl21/Webcam_Paint_OpenCV) 

# 工作示例

# 代码要求

这个应用程序是用 Python 3.6 编写的，它使用了非常著名的 OpenCV 库。OpenCV 是一个计算机视觉和机器学习软件库，包括许多常见的图像分析算法，可以帮助我们构建定制的智能计算机视觉应用程序。

按照[这篇中帖](https://medium.com/@akshaychandra21/how-to-install-opencv-and-keras-in-python-3-6-f5f721f0d0b3)在 Python 3 中安装 OpenCV。

# 视频跟踪快速介绍

## 视频跟踪

为了执行视频跟踪，一种算法分析连续的视频帧，并输出帧之间的目标运动。算法多种多样，各有优缺点。在选择使用哪种算法时，考虑预期用途很重要。视觉跟踪系统有两个主要组成部分:目标表示和定位，以及过滤和数据关联。

视频跟踪是使用摄像机在一段时间内定位一个移动对象(或多个对象)的过程。它有多种用途，其中一些是:人机交互、安全和监控、视频通信和压缩、增强现实、交通控制、医学成像和视频编辑。

更多信息，[见](http://opencv-python-tutroals.readthedocs.io/en/latest/py_tutorials/py_tutorials.html)。

## 什么是等高线？

如果你是 OpenCV 的新手，在阅读本文中的代码之前，最好先浏览一下这个关于轮廓的教程页面。也看看这个 Jupyter [笔记本](https://github.com/akshaychandra21/OpenCV_Basic_Exercises/blob/master/11%20-%20Contours.ipynb)的轮廓。需要明确的是，术语“边缘”和“轮廓”经常互换使用，但这是不对的。如果你对这种差异的理解模糊不清，请浏览一下 ResearchGate 网站上的[这个问题帖子](https://www.researchgate.net/post/What_is_the_difference_between_edge_and_contour)。

# 代码解释

## 第一步:初始化一些东西

首先，我们导入必要的库。

然后，我们初始化在以下步骤中使用的变量。

*blueLower* 和 *blueUpper* numpy 阵列帮助我们找到蓝色的帽子。一旦发现蓝帽，内核*有助于使其平滑。 *bpoints* 、*gppoints*、 *rpoints 和 ypoints* deques 分别用于存储蓝色、绿色、红色和黄色屏幕上绘制的点。*

## 步骤 2:设置绘画界面

这是一件痛苦的事情。我们必须手动设置框架上每个彩色框的坐标。我们使用 OpenCV 函数***cv2 . rectangle()***来绘制盒子。

## 第三步:开始阅读视频(一帧一帧)

现在我们使用 OpenCV 函数 ***cv2。VideoCapture()*** 从视频文件或网络摄像头实时逐帧读取视频(使用 while 循环)的方法。在这种情况下，我们将 0 传递给方法以从网络摄像头读取数据。

为了使用方便，我们可以添加完全相同的画图界面。

## 步骤 4:找到感兴趣的轮廓(瓶盖)

一旦我们开始读取网络摄像头馈送，我们就会借助***cv2 . in range()***方法不断在帧中寻找蓝色对象，并使用在步骤 0 中初始化的 *blueUpper* 和 *blueLower* 变量。一旦我们找到轮廓，我们做一系列的图像操作，并使其平滑。它们让我们的生活变得更轻松。如果你想知道更多关于这些操作——腐蚀、变形和扩张，请查看[这个](https://docs.opencv.org/3.0-beta/doc/py_tutorials/py_imgproc/py_morphological_ops/py_morphological_ops.html)。

一旦我们找到轮廓(当找到轮廓时，如果 条件通过，则为 ***)，我们使用轮廓的中心(蓝色帽)在屏幕上绘制它的移动。下面的代码做了同样的事情。***

上面的代码找到轮廓(最大的一个)，使用***cv2 . minenclosingcircle()***和 ***cv2.circle()*** 方法围绕它画一个圆，借助***cv2 . moments()***方法得到找到的轮廓的中心。

## 第五步:开始绘图并存储绘图

现在我们开始跟踪轮廓中心接触屏幕的每一个点的坐标，以及它的颜色。我们将这些不同颜色的点存储在不同的 deques 中(*b 点*、*g 点*等)。).当轮廓的中心接触到我们在步骤 1 中放在屏幕上的一个彩色框时，我们将这些点存储在其各自的颜色队列中。

## 第六步:在屏幕上显示图画

到目前为止，我们将所有点存储在它们各自的颜色队列中。现在我们用他们自己的颜色加入他们。OpenCV 函数 ***cv2.line()*** 为我们做到这一点提供了便利。下面的代码做了同样的事情。

一旦我们用一条线连接每一帧中的所有点，并把它放在我们用 ***cv2.imshow()*** 方法创建的两个窗口上，它就像一个绘画应用程序一样完美地工作了。当我们进入 循环从网络摄像头读取数据时，从 ***循环中掉出后，我们释放摄像头并使用以下代码行销毁所有窗口。***

就是这样！我们在 OpenCV 中成功地使用了一堆基本的图像处理工具和操作，创建了一个类似绘画的应用程序——网络摄像头绘画！

# 执行

```
> python Webcam_Paint_OpenCV.py
```

# **结论**

这是 OpenCV 图像处理能力的简单演示。如果你觉得这很有趣或容易或无聊，或者认为这没有什么挑战性，我建议你尝试添加更多的绘画功能，如橡皮擦，不同类型的笔刷，颜色填充选项等。为了真正感受 OpenCV 有多有趣。

希望这个教程是有趣的。感谢阅读。

活也让别人活！
答

![](img/e66597517477776e5267b92d82f03c58.png)

Photo by [Kelli Tungay](https://unsplash.com/photos/Sj0nhVIb4eY?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) on [Unsplash](https://unsplash.com/?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)