# 如何在对 Tensorflow 一无所知的情况下将 Keras 模型带到 Android 上

> 原文：<https://towardsdatascience.com/how-to-convert-from-keras-to-tflite-with-zero-knowledge-of-tensorflow-5448a296ae67?source=collection_archive---------5----------------------->

![](img/9280ca5c5dad6ab6eee7c620690b33dc.png)

Photo by [Rodion Kutsaev](https://unsplash.com/photos/0VGG7cqTwCo?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) on [Unsplash](https://unsplash.com/search/photos/android?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

你刚刚创造了喀拉斯最惊人的神经网络。但是有一个问题。

目前，您的网络只存在于您的计算机上。它不能从那里改变世界。

你首先想到的是把它放到你的手机上。现在手机比人多，这是一个让你的神经网络真正到达需要它的人手中的极好策略。

在 Android 手机上运行神经网络最流行的库之一是 Tensorflow Lite。要使用它，您需要将 Keras .h5 文件转换为 Tensorflow。tflite 文件。对于那些不熟悉 Tensorflow 的 Keras 用户来说，这可能是一项艰巨的任务。在本文中，我将展示如何在几乎不了解 Windows 操作系统上的 Tensorflow 的情况下实现这一点。

第一步，我们要将 Keras .h5 文件转换成张量流。pb 文件。为此，我们将下载 keras_to_tensorflow 工具，可在此处找到[。](https://github.com/amir-abdi/keras_to_tensorflow)

[](https://github.com/amir-abdi/keras_to_tensorflow) [## 阿米尔-阿卜迪/喀拉斯 _ 托 _ 滕索福尔

### keras _ to _ tensorflow——将训练好的 keras 模型转换成推理 tensor flow 模型的通用代码

github.com](https://github.com/amir-abdi/keras_to_tensorflow) 

在您下载了 repo 并将您的模型(我们称之为 model.h5)添加到该文件夹后，使用命令行导航到该文件夹并键入:

```
python keras_to_tensorflow.py -input_model_file model.h5
```

您将生成一个 Tensorflow model.pb 文件。请注意，除非指定，否则此。pb 模型将被称为 output_node，这对于下一个转换步骤非常重要。您还应该知道输入节点的名称，在本例中是 input_1。

现在我们需要转换张量流。pb 文件到 Tensorflow Lite。使用 toco 工具创建 tflite 文件。不幸的是，我似乎无法让这个工具在 Windows 上正常工作，我看到许多论坛记录了其他人经历类似的挣扎。为了解决这个问题，我们将使用 Linux。

首先下载 VirtualBox [这里](https://www.virtualbox.org/wiki/Downloads)(这是我们的虚拟机)和正确的 Ubuntu 版本(对我来说是 64 位)从[这里](http://releases.ubuntu.com/16.04/)。

我在这里找到了一篇关于如何用 Ubuntu 安装 VirtualBox 的优秀文章。跟着它走。我遇到的一个问题是 VirtualBox 只允许我安装 32 位版本的 Ubuntu。为了解决这个问题，我需要访问我的 BIOS 并启用虚拟化技术。

一旦你启动并运行了 Ubuntu，右击桌面打开一个终端。

您将需要安装 python，这可以通过使用:

```
$ sudo apt-get install python3.6
$ sudo apt-get install python3-pip python3-dev
```

然后安装 Tensorflow。

```
$ pip3 install tensorflow==1.6.0
```

我发现 Tensorflow 1.5 无法转换我的模型的一些功能，但这个问题在 Tensorflow 1.6 中消失了。以后的任何事也应该是好的。

现在我们需要将我们的模型引入 Ubuntu。我个人就是用 Dropbox 做到的。在 Windows 中，我把这个模型添加到我的 dropbox 中，在 Ubuntu 中打开这个 dropbox，然后把它添加到我的桌面上。

现在我们准备好进行转换了。假设模型的输入是 299x299 RGB 图像，我们将编写以下命令。请注意这是如何影响我们放入- input_shape 参数中的内容的。此外，整个表彰应写成一行。

```
/home/ryan/.local/bin/toco --input_file=Desktop/model.pb            --output_file=Desktop/model.tflite                                  --input_format=TENSORFLOW_GRAPHDEF 
--output_format=TFLITE --input_shape=1,299,299,3                    --input_array=input_1 --output_array=output_node                    --inference_type=FLOAT
```

您的桌面上应该有一个. tflite 文件。只需将它发送回 Windows(我再次使用 Dropbox)，瞧，你可以在你的 Android 程序中使用它。

希望这篇文章能有所帮助，如果你有更好的方法将 Keras 神经网络从 Windows 安装到你的 Android 手机上，请在评论中告诉我！