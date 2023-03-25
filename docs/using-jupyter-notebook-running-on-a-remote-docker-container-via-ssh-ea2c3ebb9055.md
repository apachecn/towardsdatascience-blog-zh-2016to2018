# 使用 SSH 在远程 Docker 容器上运行 Jupyter

> 原文：<https://towardsdatascience.com/using-jupyter-notebook-running-on-a-remote-docker-container-via-ssh-ea2c3ebb9055?source=collection_archive---------3----------------------->

![](img/f345f77e87d21ab110554e0bf385625c.png)

Own source

## 设置您的环境，使之与远程实例中运行的 Docker 和 Jupyter 一起工作

在我的研究中，我使用机器学习/深度学习算法，我主要使用 Python 开发这些算法。因此，我发现测试不同的框架很有用(比如 *Keras、PyTorch、Tensorflow…* )。这导致我使用 **Docker** ，特别是 [ufoym/deepo](https://github.com/ufoym/deepo) 图像，因为它提供了一个非常完整的环境。

我使用一台装有 GPU 的远程机器，在那里我安装了 Docker。在编程和测试一些代码片段的时候，我发现使用 [**Jupyter**](https://jupyter.org/) 真的很有帮助。我以前在本地使用过 Jupyter…但是这里的问题是**我如何远程使用运行在另一台机器上的 Jupyter**。除此之外，如果我在我的远程机器上使用 Docker 容器会怎么样呢？

在本教程中，我将尝试分享我是如何解决这个*难题*并使本地使用 Jupyter web 应用程序在远程机器上运行代码成为可能的经验。

# 我的工作环境

让我首先说明我的工作环境设置。

![](img/530e23a8ce4e343bd32711321a24ee59.png)

Own source, drawn using [Google Draw](https://docs.google.com/drawings)

1.  我用我的普通笔记本电脑作为主机。
2.  我通过 ssh 连接到我部门的服务器(**远程**)。
3.  然后，我连接到服务器中一台特定的机器，这台机器有 GPU。
4.  最后，我启动了那台机器中的**码头工人**集装箱。

在我的例子中，我使用的机器是上图中标为 **GPU** 的机器，它恰好只能从服务器中的另一台机器访问。因此有了双 ssh……但是在你的情况下，你可以从外部世界直接连接到你的 GPU 机器。

这听起来很乱，但请耐心听我说，你只需要一劳永逸地组织起来。

# 先决条件

## 码头工人

确保在你的远程机器上安装了[**docker**](https://docs.docker.com/install/linux/docker-ce/centos/)**。如果你不熟悉 Docker，请查看一些可用的资源(我发现[这个](https://www.youtube.com/watch?v=YFl2mCHdv24)和[这个](https://blog.scottlowe.org/2014/03/11/a-quick-introduction-to-docker/)很有帮助)。如果您的目标是 GPU 支持环境，请查看下一步。**

## **GPU 支持**

**如果您的远程机器有 GPU，您可能想利用这一事实。确保你已经安装了 [**NVIDIA 驱动**](https://github.com/NVIDIA/nvidia-docker/wiki/Frequently-Asked-Questions#how-do-i-install-the-nvidia-driver) 。还需要安装 [**nvidia-docker**](https://github.com/NVIDIA/nvidia-docker) 。请注意，通过安装 nvidia-docker，我们会自动安装最后一个稳定版本的 [docker-ce](https://www.docker.com/community-edition) ，因此您之前不需要显式安装 docker。**

# **设置连接**

## **主机—远程**

**Jupyter 笔记本运行在机器的某个端口上。因此，基本的想法是使该端口可以从您的主机到达。幸运的是， **ssh** 提供了-L 选项来指定端口转发。**

```
$ ssh -L <host port>:localhost:<remote port> user@remote
```

**在我的例子中，我在两端使用端口 9999，即 *<主机端口> = <远程端口> = 9999。***

## **远程— GPU**

**这里的方法和以前完全一样。**

```
$ ssh -L <remote port>:localhost:<GPU port> user@GPU
```

**再次，我用 *<主机端口> = <远程端口> = 9999* 。**

## **GPU — docker**

**这一步略有不同。首先，您需要使用您喜欢的 docker 映像创建一个 docker 容器。在我的例子中，如上所述，我使用的是 [ufoym/deepo](https://github.com/ufoym/deepo) 。因为我想要 GPU 支持，所以我将使用`nvidia-docker`来创建容器。**

```
$ nvidia-docker run -it \
-p <GPU port>:<container port> \
--name <container name> \
ufoym/deepo  bash
```

**注意选项`-p`，它告诉 docker 容器进行端口转发。通过这种方式，我们可以从外部世界访问在某个端口上运行的应用程序。这里我也用 *<主机端口> = <远程端口> = 9999* 。**

**顺便说一下，创建容器时一个非常有用的选项是`-v`，它允许您从 docker 容器中访问机器文件。**

## ****运行 Jupyter****

**一旦所有的隧道都建立好了，我们就可以启动我们的 jupyter 应用程序了。我们将使用`0.0.0.0` ip 和创建 docker 容器实例时使用的相同端口。同样，在我的例子中，我使用了选项`--allow-root`，因为我在我的容器中是 root，除非我使用这个选项，否则 Jupyter 不会运行。**

```
$ jupyter notebook --ip 0.0.0.0 --port <container port> --allow-root
```

**哦，如果你喜欢新的很酷的 [jupyter lab](https://github.com/jupyterlab/jupyterlab) ，就用下面的命令代替。**

```
$ jupyter lab --ip 0.0.0.0 --port <container port> --allow-root
```

**现在，在我的主机上，我只需简单地进入 localhost:9999 就可以了。**

# **旁注**

## **启用更多端口**

**如果您有一个类似的工作环境，我建议启用更多的远程访问端口。您可以使用`-L <local port>:localhost:<remote port>`将它们添加到您的 ssh 命令中。这样，如果您碰巧在 docker 机器的某些端口上运行其他服务，您可以轻松地远程访问它们。**

## **自动 ssh 登录**

**当您向 ssh 命令添加更多选项时，它会增加大小。一个选项是在`~/.ssh/config`文件下定义一个主机。在我的例子中，我添加了一个用户`<name>`:**

```
Host <name>
  Hostname <remote IP>
  Port <remote port for SSH (typically 22)>
  IdentityFile <path to rsa key>
  LocalForward 9999 127.0.0.1:9999
```

## ****有误差吗？****

**测试隧道是否工作的一个好方法是使用 python 中的`http.server`。这样，您可以检查每个阶段端口是否被正确转发。使用以下命令在特定端口上运行简单的 http 服务器。**

```
$ python -m http.server <remote port>
```

**注意，这适用于 python3，对于 python2 版本，使用`python -m SimpleHTTPServer <remote port>`代替。**

**如果你有任何其他困难，请在评论中留下，我很乐意帮助你！**