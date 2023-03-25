# 用 Tensorflow 和 OpenCV 构建实时物体识别应用

> 原文：<https://towardsdatascience.com/building-a-real-time-object-recognition-app-with-tensorflow-and-opencv-b7a2b4ebdc32?source=collection_archive---------0----------------------->

在本文中，我将介绍如何使用 Python 3(具体来说是 3.5)中的[tensor flow(TF)新的对象检测 API](https://github.com/tensorflow/models/tree/master/research/object_detection) 和 [OpenCV](http://opencv.org/) 轻松构建自己的实时对象识别应用程序。重点将放在我创建它时所面临的挑战上。你可以在[我的回购](https://github.com/datitran/Object-Detector-App)上找到完整的代码。

这也是正在运行的应用程序:

![](img/c64e390791fa2ef256b7cf8c78c838e2.png)

**Me trying to classify some random stuff on my desk:)**

# **动机**

[谷歌刚刚发布了](https://research.googleblog.com/2017/06/supercharge-your-computer-vision-models.html)他们新的 TensorFlow 物体检测 API。第一个版本包含:

*   一些[预训练的模型](https://github.com/tensorflow/models/blob/477ed41e7e4e8a8443bc633846eb01e2182dc68a/object_detection/g3doc/detection_model_zoo.md)(特别关注轻量模型，以便它们可以在移动设备上运行)
*   一个 [Jupyter 笔记本](https://github.com/tensorflow/models/blob/master/research/object_detection/object_detection_tutorial.ipynb)示例，其中包含一款已发布的型号
*   [一些非常方便的脚本](https://github.com/tensorflow/models/blob/master/research/object_detection/g3doc/running_locally.md)可用于模型的重新训练，例如，在您自己的数据集上。

我想把我的手放在这个新的很酷的东西上，并有一些时间来构建一个简单的实时对象识别演示。

# **物体检测演示**

首先，我调出了 [TensorFlow models repo](https://github.com/tensorflow/models) ，然后又看了一下他们发布的[笔记本](https://github.com/tensorflow/models/blob/master/research/object_detection/object_detection_tutorial.ipynb)。它基本上走过了使用预训练模型的所有步骤。在他们的例子中，他们使用了[“带 Mobilenet 的 SSD”](https://arxiv.org/abs/1512.02325)模型，但你也可以在他们所谓的[“tensor flow 检测模型动物园”](https://github.com/tensorflow/models/blob/477ed41e7e4e8a8443bc633846eb01e2182dc68a/object_detection/g3doc/detection_model_zoo.md)上下载其他几个预先训练好的模型。顺便说一下，这些模型是在 [COCO](http://mscoco.org/) 数据集上训练的，并根据模型速度(慢、中和快)和模型性能(mAP —平均精度)而变化。

我接下来做的是运行这个例子。这个例子实际上是有据可查的。本质上，它是这样做的:

1.  导入所需的软件包，如 TensorFlow、PIL 等。
2.  定义一些变量，如班级人数、模型名称等。
3.  下载冻结模型(。pb — [protobuf](https://developers.google.com/protocol-buffers/) )并将其加载到内存中
4.  加载一些辅助代码，例如标签转换器的索引
5.  两幅测试图像上的检测代码本身

**注意:**在运行示例之前，请务必查看[设置注意事项](https://github.com/tensorflow/models/blob/master/research/object_detection/g3doc/installation.md)。特别是，protobuf 编译部分非常重要:

```
# From tensorflow/models/research/
protoc object_detection/protos/*.proto --python_out=.
```

如果不运行这个命令，这个示例将无法运行。

然后，我采用他们的代码，并对其进行了相应的修改:

*   删除模型下载部分
*   PIL 是不需要的，因为 OpenCV 中的视频流已经在 numpy 数组中了(PIL 也是一个非常大的开销，特别是当使用它来读取图像或视频流时)
*   TensorFlow 会话没有“with”语句，因为这是一个巨大的开销，特别是当每次会话都需要在每个流之后启动时

然后，我用 OpenCV 把它和我的网络摄像头连接起来。有很多例子向你解释如何去做，甚至是官方文档。所以，我就不深究了。更有趣的部分是我为提高应用程序的性能而做的优化。在我的例子中，我看到了良好的 fps——每秒帧数。

一般来说，许多 OpenCV 示例的普通/朴素实现并不是真正的最佳实现，例如 OpenCV 中的一些函数严重受限于 I/O。所以我不得不想出各种办法来解决这个问题:

*   从网络摄像头读取帧会导致大量的 I/O。我的想法是用[多处理库](https://docs.python.org/3.5/library/multiprocessing.html)将这部分完全转移到不同的 Python 进程。这不知何故没有奏效。在 Stackoverflow 上有一些解释为什么它不工作，但我没有深入探讨这个问题。幸运的是，我从[的 Adrian Rosebrock 的网站“pyimagesearch”](http://www.pyimagesearch.com/2015/12/21/increasing-webcam-fps-with-python-and-opencv/)上找到了一个非常好的例子，使用[线程](https://docs.python.org/3.5/library/threading.html)代替它大大提高了我的 fps。顺便说一下，如果你想知道多处理和线程之间的区别，在 Stackoverflow 上有一个很好的解释。
*   每次应用程序启动时，将冻结的模型加载到内存中是一项很大的开销。我已经为每次运行使用了一个 TF 会话，但这仍然非常慢。那么我是怎么解决这个问题的呢？解决办法很简单。在这种情况下，我使用多处理库将对象检测部分的繁重工作转移到多个进程中。应用程序的初始启动会很慢，因为每个进程都需要将模型加载到内存中并启动 TF 会话，但在此之后，我们将受益于并行性😁

[](https://asciinema.org/a/125852) [## 与多个工作人员一起运行对象检测演示。

### 由 datitran 录制

asciinema.org](https://asciinema.org/a/125852) 

*   减少视频流中帧的宽度和高度也大大提高了 fps。

**注意:**如果你像我一样使用 Mac OSX，并且使用 OpenCV 3.1，OpenCV 的 VideoCapture 可能会在一段时间后崩溃。已经有[个问题提交](https://github.com/opencv/opencv/issues/5874)。切换回 OpenCV 3.0 解决了这个问题。

# 结论与展望

给我一个❤️，如果你喜欢这个职位:)拉代码，并尝试自己。一定要看看 Tensorflow 对象检测 API。到目前为止，从第一眼看上去，它非常简洁明了。我想尝试的下一件事是用 API 训练我自己的数据集，并为我想到的其他应用程序使用预训练的模型。我对应用程序的性能也不完全满意。fps 速率仍然不是最佳的。OpenCV 中仍然有许多我无法影响的瓶颈，但是我可以尝试一些替代方法，比如使用 [WebRTC](https://webrtc.org/) 。然而，这是基于网络的。此外，我正在考虑使用异步方法调用(async)来提高我的 fps 率。敬请期待！

在推特上关注我: [@datitran](https://twitter.com/datitran)