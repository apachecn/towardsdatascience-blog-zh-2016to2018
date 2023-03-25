# 如何在 AWS GPU 实例上设置深度学习环境

> 原文：<https://towardsdatascience.com/how-to-set-up-deep-learning-machine-on-aws-gpu-instance-3bb18b0a2579?source=collection_archive---------3----------------------->

# **设定我们的目标**

目标是学习如何在 Amazon 的 AWS GPU 实例上建立一个机器学习环境，通过使用 docker 容器，该环境可以很容易地复制并用于其他问题。

# **设置环境**

第一步是在亚马逊的网络服务上建立一个虚拟机。为此，我们需要选择正确的硬件和软件包来构建深度学习模型。深度学习模型消耗大量计算能力，对非常大的矩阵进行矩阵运算。AWS 上有两个硬件选项。仅限 CPU 或 GPU。GPU 代表图形处理单元。CPU 和 GPU 架构的主要区别在于，GPU 是并行处理器，但更加专业化。相反，CPU 是通用计算架构，在并行计算上做不好。Amazon 允许您为繁重的计算建立一个带有专用 GPU 核心的虚拟机。当然，这增加了一点你的成本，但是如果你考虑到你节省的时间，这是一个很好的交易。

或者，如果你真的想认真对待这个问题，那么我建议你在家里用 nvidia GPUs 构建自己的系统。

下面是我们在 AWS 上设置 GPU 实例所需的步骤:

*   启动实例
*   选择 ubuntu 16.04
*   选择 g2.xlarge — 8 个 vCPU、15Gb RAM、60GB 启动 SSD、1 个 GPU K520
*   选择可用性区域
*   防止意外终止
*   添加存储— 120 GB
*   添加标签，如名称和环境…
*   选择安全组
*   启动并选择键

**连接到实例**

导航到存储 SSH 密钥的目录，使用下面的命令在终端中连接到您的实例

```
ssh -i “your_ssh_key.pem” ubuntu@*[your instance public IP address]*
```

**安装 NVIDIA 驱动程序**

> ref:[https://medium . com/forward-data-science/how-to-set-up-a-deep-learning-environment-on-AWS-with-keras-the ano-b 0f 39 e 3d 861 c](https://medium.com/towards-data-science/how-to-set-up-a-deep-learning-environment-on-aws-with-keras-theano-b0f39e3d861c)

```
sudo apt-get updatesudo apt-get upgrade
```

**要领**

```
sudo apt-get install openjdk-8-jdk git python-dev python3-dev python-numpy python3-numpy build-essential python-pip python3-pip python3-venv swig python3-wheel libcurl3-devsudo apt-get install -y gcc g++ gfortran git linux-image-generic linux-headers-generic linux-source linux-image-extra-virtual libopenblas-dev
```

**NVIDIA 驱动程序**

```
sudo add-apt-repository ppa:graphics-drivers/ppa -ysudo apt-get updatesudo apt-get install -y nvidia-375 nvidia-settings
```

**安装 CUDA 8 库**

```
wget [https://developer.nvidia.com/compute/cuda/8.0/Prod2/local_installers/cuda-repo-ubuntu1604-8-0-local-ga2_8.0.61-1_amd64-deb](https://developer.nvidia.com/compute/cuda/8.0/Prod2/local_installers/cuda-repo-ubuntu1604-8-0-local-ga2_8.0.61-1_amd64-deb)sudo dpkg -i cuda-repo-ubuntu1604-8-0-local-ga2_8.0.61-1_amd64-debsudo apt-get updatesudo apt-get install cudanvidia-smi
```

**为 nvidia GPU 机器设置 docker 引擎(nvidia-docker)**

> 参考:https://docs.docker.com/engine/installation/linux/ubuntu

添加 docker 引擎库

```
sudo apt-get updatesudo apt-get install \ apt-transport-https \ ca-certificates \ curl \ software-properties-commoncurl -fsSL [https://download.docker.com/linux/ubuntu/gpg](https://download.docker.com/linux/ubuntu/gpg) | sudo apt-key add -sudo apt-key fingerprint 0EBFCD88sudo add-apt-repository \ “deb [arch=amd64] [https://download.docker.com/linux/ubuntu](https://download.docker.com/linux/ubuntu) \ $(lsb_release -cs) \ stable”
```

安装 docker 引擎 ce

```
sudo apt-get updatesudo apt-get install docker-cesudo docker run hello-worldsudo usermod -aG docker $USER
```

安装 nvidia-docker

```
# Install nvidia-docker and nvidia-docker-pluginwget -P /tmp [https://github.com/NVIDIA/nvidia-docker/releases/download/v1.0.1/nvidia-docker_1.0.1-1_amd64.deb](https://github.com/NVIDIA/nvidia-docker/releases/download/v1.0.1/nvidia-docker_1.0.1-1_amd64.deb)sudo dpkg -i /tmp/nvidia-docker*.deb && rm /tmp/nvidia-docker*.deb# Test if docker is using nvidia GPUsnvidia-docker run — rm nvidia/cuda nvidia-smi
```

**用 jupyter 笔记本、tensorflow 和机器学习库建立 docker 容器**

为 gpu 拉张量流 docker 图像

```
docker pull tensorflow/tensorflow:latest-gpu-py3
```

用 tensorflow 为 gpu 创建 docker 容器

```
nvidia-docker run -it -name planet -p 8888:8888 tensorflow/tensorflow:latest-gpu-py3 bash
```

码头集装箱内

奠定基础

```
apt-get updateapt-get install sudosudo apt-get updatesudo apt-get install gitsudo apt-get install nano #or your choice of editor 
```

设置 Python 环境

首先让我们看看我们需要什么，我们有什么。我们需要:

Python 3.5，PIP3 9.0.1，带 Python 3.5 内核的 jupyter 笔记本，tensorflow 1.1，带 tensorflow 后端的 keras，还有这些 librarise: cv2 (OpenCV)，sys，os，gc，numpy，pandas，seaborn，matplotlib，scikit-learn (sklearn)，scipy，itertools，subprocess，six，skimage，IPython.display，tqdm，多重处理，concurrent.futures

运行 ipython 并导入下面的库，以确保一切正常。这些库的大部分已经安装在 tensorflow docker 映像中。但是，其中一些可能不包括在内。

```
import sysimport osimport gcimport numpyimport pandasimport seabornimport matplotlibimport sklearnimport scipyimport itertoolsimport subprocessimport siximport skimageimport IPython.displayimport tensorflowimport kerasimport tqdmimport multiprocessingimport concurrent.futuresimport cv2
```

安装缺失的库

```
pip3 install seabornpip3 install scikit-imagepip3 install keraspip3 install tqdmpip3 install opencv-python
```

从这个容器创建一个新的图像，并将其命名为 keras-tf

```
docker commit amazon keras-tf:latest
```