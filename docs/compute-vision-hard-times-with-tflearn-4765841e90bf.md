# 计算机视觉:TFLearn 的艰难时期

> 原文：<https://towardsdatascience.com/compute-vision-hard-times-with-tflearn-4765841e90bf?source=collection_archive---------4----------------------->

![](img/06a063e3bbec80767a26838ba3dc1dd3.png)

我的故事从我玩够了 OpenCV 3.3.0 并决定更进一步尝试一些我从未尝试过的酷东西开始，所以 TensorFlow 是一个像 Thor's hammer 一样闪亮的大东西，每个人都想亲自动手，但不知道它是如何工作或做什么的(当然我们都听到过时髦的词——机器学习、数据科学、计算机视觉)。

我的工作机器是顶级 Macbook Pro，这意味着我有非常不同的体验(来自 ubuntu、fedora 等。)同时安装我工作需要的软件。当我开始使用 OpenCV 时，我使用了[Python 3.6+OpenCV 3.3 docker image](https://hub.docker.com/r/jjanzic/docker-python3-opencv/)，这显然不太适合调试。所以，我决定让我的笔记本电脑有点“脏”(经常试图让笔记本电脑远离我开发所需的软件，这就是为什么我使用虚拟机，运行代码的容器)。

我做的第一件事是谷歌搜索“opencv 3 mac os ”,发现了几篇文章，不幸的是，github 问题试图通过安装 opencv，只要看看[你应该做什么来安装 opencv](https://www.learnopencv.com/install-opencv3-on-macos/) 。跳到故事的结尾，我只能说一句话:“艰难时期带来了很多不必要的乐趣”。使用 OpenCV 最令人沮丧的部分是需要为您创建的每个 virtualenv 创建一个符号链接。尽管安装 OpenCV 的经历很糟糕，但我倾向于认为它是一个强大的工具——25 行代码的[人脸识别，10-15 行代码的微笑识别](https://github.com/denismakogon/functions-hub/blob/master/object_detections/smiles/detect.py)。

我的遗愿清单中的下一项是基于 TensorFlow 的库— [tflearn](http://tflearn.org/) 。我需要感谢所有开发人员和做 TensorFlow 打包的人——你们做了大量的工作来使用户体验尽可能的流畅。

```
scipy
h5py
numpy
tensorflow
tflearn
```

这是使 TensorFlow 在本地工作所需安装的软件数量，但似乎没有现成的解决方案可以利用代码运行的机器的全部计算能力(如果您需要 CUDA GPU 能力或您的 CPU 支持 SSE4.2，AVX 指令—编译您自己的 TensorFlow 二进制文件)。TensorFlow 团队还为 Docker 维护了大量[图片。](https://github.com/tensorflow/tensorflow/tree/master/tensorflow/tools/docker)

不幸的是， **tflearn** 部署并不理想，我甚至可以说——糟透了，因为社区没有明确的指导方针，尤其是对于版本控制。这种问题会导致像**分割错误**这样的严重错误。从一开始 **tflearn** 就在依赖版本方面遇到了麻烦，导致了大量与 segfault 相关的问题，以及 Stackoverflow 上大量请求帮助克服 segfault 关键问题的主题。这里有一个例子可以说明依赖性管理有多差:

```
$pip install tensorflow tflearn
...
Successfully installed bleach-1.5.0 html5lib-0.9999999 markdown-2.6.9 numpy-1.13.1 protobuf-3.4.0 six-1.10.0 tensorflow-1.3.0 tensorflow-tensorboard-0.1.6 werkzeug-0.12.2 Pillow-4.2.1 olefile-0.44 tflearn-0.3.2
```

在第一次启动时，您会看到您需要安装更多软件包:

```
$python
Python 3.6.2 (v3.6.2:5fd33b5926, Jul 16 2017, 20:11:06)
[GCC 4.2.1 (Apple Inc. build 5666) (dot 3)] on darwin
Type "help", "copyright", "credits" or "license" for more information.
import tflearn
hdf5 is not supported on this machine (please install/reinstall h5py for optimal experience)
Scipy not supported!
```

好吧，我明白为什么维护者这么做了，tflearn 可以在没有 hdf5 和 scipy 的情况下工作。但是实际上，大多数 ML 项目都在使用这个库，那么为什么不能让它们默认为必需的呢？无论如何，在安装完所有这些包后，我们有以下内容:

```
bleach==1.5.0
h5py==2.7.1
html5lib==0.9999999
Markdown==2.6.9
numpy==1.13.1
olefile==0.44
Pillow==4.2.1
protobuf==3.4.0
scipy==0.19.1
six==1.10.0
tensorflow==1.3.0
tensorflow-tensorboard==0.1.6
tflearn==0.3.2
Werkzeug==0.12.2
```

好吧，对维护者的另一个问题是，如果你建议用户安装特定的附加软件，为什么不指定你需要的版本？我认为我已经做得足够多了——安装了最新版本并构建了可以工作的 env，但是我是多么的鲁莽…

假设我的环境是可复制的，我可以构建一个 docker 映像，并让我的应用程序在其中运行。尽管有一个应用程序在我的机器上运行得很好，但每当我试图启动它时，Docker image 总是死机，而且，除了退出代码 139 之外，它一直在无声地死机。

在花了几个小时阅读、谷歌搜索之后，我找到了 github 的第七期。乍一看，它与 tflearn 毫无关系。但是据说尽量降级到***tensor flow-1 . 0 . 0***。好吧，我决定从头开始，除了记住我需要安装的使 tflearn 导入静默的包。最后，我列出了以下需求:

```
scipy==0.19.1
h5py==2.7.1
numpy==1.13.1
tensorflow==1.0.0
tflearn==0.3.2
```

令我惊讶的是，我的应用程序在 upstart 期间没有失败。

![](img/d0ee88c9b413c79f7b0d9c4eb9e7c59e.png)

这意味着相对于 TensorFlow、SciPy、NumPy、h5py 的最新版本，tflearn 的最新版本被破坏。这不是开发者期望得到的。

作为结论，我只能说我在开始使用 TensorFlow 时获得的体验是非凡的，尽管花费了数小时为所有必要的包寻找可行的版本，但我倾向于同意 tflearn 带来了另一个高级 API 来构建深度神经网络模型并进行预测，这似乎非常有用(至少在我的特定情况下)。