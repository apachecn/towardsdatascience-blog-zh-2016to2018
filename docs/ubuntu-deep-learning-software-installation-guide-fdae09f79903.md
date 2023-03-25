# Ubuntu +深度学习软件安装指南

> 原文：<https://towardsdatascience.com/ubuntu-deep-learning-software-installation-guide-fdae09f79903?source=collection_archive---------0----------------------->

![](img/4f29a898df35e51e5723e6bd9dc99862.png)

我最近在报读 [Udacity 的自动驾驶汽车工程师 Nanodegree](https://www.udacity.com/drive) 后，造了一台深度学习机器。你可以在这里阅读我的构建:[花 800 美元](https://medium.com/@ncondo/build-a-deep-learning-rig-for-800-4434e21a424f)构建一个深度学习平台。在这篇文章中，我将描述从一个干净的构建到训练深度神经网络的必要步骤。有很多好的安装指南——特别是来自 floydhub 的[这个](https://github.com/floydhub/dl-setup),大部分内容都是基于它——但是我发现自己不得不挖掘许多不同的资源来正确安装所有的东西。本文的目标是将所有必要的资源整合到一个地方。

# 系统检查

即使在安装操作系统之前，您也应该执行一些基本的检查，以确保您的系统正常启动和运行。为此，我建议遵循 Paul 的硬件 YouTube 频道中的[关于新 PC 构建的前 5 件事](https://youtu.be/LbpqkiaO7q4)。他继续安装 Windows，但视频的前半部分适用于任何机器，不管是什么操作系统。

# 人的本质

首先下载 [Ubuntu 16.04.2 LTS](https://www.ubuntu.com/download/desktop) ，Ubuntu 最新的长期支持版本。然后，用 Ubuntu ISO 创建一个可启动的 u 盘。如果你在 macOS 上，你可以遵循 Ubuntu 官方说明[这里](https://www.ubuntu.com/download/desktop/create-a-usb-stick-on-macos)，或者如果你在 Windows 上，你可以遵循[这里](https://www.ubuntu.com/download/desktop/create-a-usb-stick-on-windows)。一旦你把 Ubuntu ISO 加载到你的 u 盘上，把它插入你的新版本并启动机器。要安装 Ubuntu，你需要进入启动菜单，对我来说是 F11 键，然后选择从 u 盘启动。从那里，您可以简单地按照屏幕上的安装说明。你也可以参考 Ubuntu 的社区 wiki [快速指南:从 USB 记忆棒安装 Ubuntu](https://help.ubuntu.com/community/Installation/FromUSBStickQuick)了解更多细节。

# 基础

从终端运行以下命令来更新软件包并安装一些基本软件:

```
sudo apt-get update
sudo apt-get upgrade
sudo apt-get install build-essential cmake g++ gfortran git vim pkg-config python-dev software-properties-common wget
sudo apt-get autoremove
sudo rm -rf /var/lib/apt/lists/*
```

# Nvidia 驱动程序

您需要根据您的 GPU 型号下载正确的驱动程序，您可以使用以下命令进行检查:

```
lspci | grep -i nvidia
```

查看[专有 GPU 驱动程序 PPA](https://launchpad.net/~graphics-drivers/+archive/ubuntu/ppa) 库，找到您的 GPU 的当前版本。在撰写本文时，GeForce 10 系列的最新版本是 381.22，但我选择了 375.66，这是当前的长期分支版本。使用以下命令安装驱动程序:

```
sudo add-apt-repository ppa:graphics-drivers/ppa
sudo apt-get update
sudo apt-get install nvidia-375
```

然后重新启动计算机:

```
sudo shutdown -r now
```

要再次检查驱动程序是否已正确安装:

```
cat /proc/driver/nvidia/version
```

# 库达

从 [Nvidia 的网站](https://developer.nvidia.com/cuda-downloads)下载 CUDA 8.0 工具包。转到下载目录(或者您选择保存它的地方)并安装 CUDA:

```
sudo dpkg -i cuda-repo-ubuntu1604*amd64.deb
sudo apt-get update
sudo apt-get install cuda
```

将 CUDA 添加到环境变量:

```
echo 'export PATH=/usr/local/cuda/bin:$PATH' >> ~/.bashrc
echo 'export LD_LIBRARY_PATH=/usr/local/cuda/lib64:$LD_LIBRARY_PATH' >> ~/.bashrc
source ~/.bashrc
```

检查是否安装了正确版本的 CUDA:

```
nvcc -V
```

重新启动计算机:

```
sudo shutdown -r now
```

# cuDNN

从 [Nvidia 的网站](https://developer.nvidia.com/cudnn)下载 cuDNN 库。在下载 cuDNN 之前，您需要首先注册 Nvidia 的开发者计划。他们说这可能需要几天的时间，但是我几乎立刻就获得了访问权限。在撰写本文时，cuDNN v5.1 是 TensorFlow 官方支持的版本，所以除非你知道 v6.0 受支持，否则不要发布([他们目前正在开发](https://github.com/tensorflow/tensorflow/issues/8828))。下载后，转到下载目录提取并复制文件:

```
tar xvf cudnn*.tgz
cd cuda
sudo cp -P */*.h /usr/local/cuda/include/
sudo cp -P */libcudnn* /usr/local/cuda/lib64/
sudo chmod a+r /usr/local/cuda/lib64/libcudnn*
```

# Anaconda +包

从 [Continuum 站点](https://www.continuum.io/downloads#linux)下载 Anaconda 安装程序。我选择了 Python 2.7 版本，但是您可以选择任何您想要的版本作为您的默认 Python:

```
bash Anaconda2-4.4.0-Linux-x86_64.sh
```

当安装程序询问您是否想将 Anaconda 添加到您的路径中时，回答“是”(除非您不想将它作为您的默认 Python)。一旦安装了 Anaconda，就可以用所有必要的深度学习库创建一个新的 conda 环境。我将使用 Udacity 的初学者工具包作为一个例子，因为这是我在许多项目中使用的。从[CarND-term 1-Starter-Kit](https://github.com/udacity/CarND-Term1-Starter-Kit)下载(或复制粘贴)environment-gpu.yml 文件。您可以在文件的顶部随意更改环境的名称。从保存环境-gpu.yml 文件的目录中:

```
conda env create **-**f environment-gpu**.**yml
```

要激活您的新环境:

```
source activate carnd-term1
```

现在，您可以通过运行 TensorFlow 的内置 MNIST 示例模型(不再适用于 TensorFlow 版本≥ 1.0)进行测试，以确保一切正常工作。请注意，以下命令将在首次运行时自动下载 12 MB MNIST 数据集:

```
python -m tensorflow.models.image.mnist.convolutional
```

如果在这一点上一切正常，那么恭喜你——你现在已经准备好对热狗进行分类了！

![](img/7f36284252d1502800267d8026df4a6d.png)

[http://www.foodandwine.com/news/silicon-valleys-hot-dog-identifying-app-very-real](http://www.foodandwine.com/news/silicon-valleys-hot-dog-identifying-app-very-real)