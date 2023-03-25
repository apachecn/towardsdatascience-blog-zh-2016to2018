# 数据科学码头工人

> 原文：<https://towardsdatascience.com/docker-for-data-science-9c0ce73e8263?source=collection_archive---------0----------------------->

如果你选择了数据科学的道路，你应该知道很多工具，比如 [python](https://www.python.org/) 、 [NumPy](http://www.numpy.org/) 、[熊猫](https://pandas.pydata.org/)、 [Matplotlib](https://matplotlib.org/) 、 [SciPy](https://www.scipy.org/) 、 [Jupyter notebook](http://jupyter.org/) 、 [scikit-learn](http://scikit-learn.org/stable/) ，甚至可能还有 [Apache Spark](https://spark.apache.org/) …

有很多工具是为了让生活变得更简单而创造的，例如[Anaconda](https://anaconda.org/)——为开源和私人项目提供强大的协作和软件包管理。

我想展示另一个好工具——[Docker](https://www.docker.com/)。它有助于你在任何地方的工作环境。

我将展示从安装 Anaconda 和升级软件包到构建 Docker 映像并将其发布到 Docker hub 上的所有步骤。

# 蟒蛇

如果你刚刚开始自己的数据科学之路，你可以找到很多书籍和课程( [Coursera](https://www.coursera.org/) 、[Udemy](https://www.udemy.com/)……)。几乎所有地方都应该从安装环境开始。你可以使用 python 和 [pip](https://pypi.python.org/pypi/pip) 或者安装 Anaconda。

在这里，我将向您展示如何安装 Anaconda 并更新所有依赖项。

有很好的[文档](https://conda.io/docs/user-guide/install/index.html)如何在任何平台(Mac OS，Linux，Windows)上安装。由于我主要使用 mac，我将展示 Mac OS 的所有步骤。但是对于 Linux 和 Windows 来说，几乎是一样的。让我们打开[在 macOS 上安装](https://conda.io/docs/user-guide/install/macos.html)页面，点击[Anaconda installer for MAC OS](https://www.anaconda.com/download/)链接下载安装程序。

![](img/856d140bb5785c0cef28bf037f10e509.png)

我来安装 Python 3.6 版本 [64 位图形安装程序(569 MB)](https://repo.continuum.io/archive/Anaconda3-5.0.1-MacOSX-x86_64.pkg) 。只需点击链接并下载它。像往常一样，在 mac 上安装软件很容易。

运行安装后，您可以看到一些步骤。单击下一步:

![](img/17fd0331e67685b970e783e105f5eaa1.png)

你可以看到一些安装信息，点击下一步:

![](img/15ce065084351d7b4725dff0acdf2bf8.png)

阅读许可证:

![](img/5426b361f915c00ef002f9be89d7a60b.png)

然后单击下一步:

![](img/3332727d50f77d00e51df6d566faacf2.png)

您需要同意许可证:

![](img/1be29fafe6c1defd979df809bc85a0b4.png)

选择安装位置:

![](img/65f76c5440a7f3861e4f484725791f74.png)

对于所有的包，我们需要将近 2 Gb 的磁盘空间。单击安装:

![](img/01c8c434b854416ecddc9c0822bc4c66.png)

等待:

![](img/807948e08439f3b59757460b7e070f5e.png)

就这么定了。我们可以删除安装程序:

![](img/ed49046d95e3cc933805b565bc68bf9d.png)

要测试它，只需打开终端并运行命令 **conda** :

![](img/7ca54b8b629ba6738604eca82194c175.png)

[Conda](https://conda.io/docs/) 是一个针对任何语言——Python、R、Ruby、Lua、Scala、Java、JavaScript、C/ C++、FORTRAN——的包、依赖和环境管理。

下一步是更新所有依赖项。我们需要更新 conda、anaconda 和所有软件包:

```
conda update conda
```

![](img/62421813cf441c2738c925f205cf0fdf.png)

```
conda update anaconda
```

![](img/b119f27ba03e0c196e0536e794187628.png)

```
conda update --all
```

![](img/ed436744cdffcb9272be67582aa2a341.png)

即使您刚刚安装了 anaconda，您也可能需要更新一些软件包。

![](img/817fbc6b64b256333698a54c84ae0af4.png)

最终我们可以看到

![](img/81209fc284be442d22eabb837c916c7a.png)

仅此而已。为了测试它，我们可以运行 jupyter notebook:

```
jupyter notebook
```

![](img/48549b535b08c5d32bab5ecbb32d9df5.png)

并在浏览器中查看:

![](img/4b87cc94e073b1714339f53288c5988f.png)

为了当地的发展仅此而已。但是如果你需要为其他 PC 共享你的环境(家庭和工作或者为团队共享)，你应该使用 Docker。

# 码头设备

要使用 Docker，我们需要安装它。打开[文档](https://docs.docker.com/engine/installation/)页面。我们将使用**社区版** (CE)版本，因为它是免费的，对我们来说没问题。

![](img/5f567d35db560414e0aa104553fa6642.png)

和之前一样我选择 [Docker for Mac (macOS)](https://docs.docker.com/docker-for-mac/install/) 。

![](img/84b2bc4a568dd18f5ebdbf178cec6428.png)

我会使用稳定频道

![](img/842690c1f633d25f4867b6d6b944591c.png)

点击链接并下载。打开后，只需拖放它

![](img/b8d695e4ebf2e59986dfb80f42540674.png)

在你的程序中有了它之后。运行它:

![](img/597e6c2bbfcf120d63e4d1dcf9967733.png)

您会看到图标

![](img/74785298cfaf9ef206419261f39130c8.png)

起作用了。让我们在命令行中运行它:

![](img/10032dd1623a956ac5fa0371d6a9189f.png)

我写了一篇文章，描述了一些有用的案例:[使用 Docker](https://hackernoon.com/making-right-things-using-docker-7296cf0f6c6e) 做正确的事情。我建议在我们继续之前阅读它。

# Docker 图像

下一步是创建一个图像并保存在 [github](https://github.com/) 上。我希望你有 gihub 帐户。如果没有，很容易[创建它](https://help.github.com/articles/signing-up-for-a-new-github-account/)。

接下来我将展示如何创建 Dockerfile 和构建映像，运行它和停止它。但是在我们需要创建工作目录和空 Dockerfile 之前:

```
mkdir docker-data-science
cd docker-data-science
touch Dockerfile
```

![](img/170375b4f57b713625f3542d85826258.png)

并在您最喜欢的 IDE 中打开它。因为我们已经安装了 jupyter 笔记本，所以我们可以使用它:

![](img/79dd9921e43be48f9dc9df298db4efa6.png)![](img/5905bd9cf1ab86ca8a70fa302cd7fec0.png)

打开它

![](img/20e76571299fb9dc2ca390516aa81ac3.png)

我将为我们的映像使用 linux 的 [Ubuntu](https://hub.docker.com/_/ubuntu/) 版本。这是最流行的 linux 发行版。你可以在[文档](https://conda.io/docs/user-guide/install/linux.html)中阅读如何安装 Anaconda for linux。不像 mac os 那么容易。

让我们打开[下载页面](https://www.anaconda.com/download/#linux)，复制 Python 3.6 [64 位(x86)安装程序(525 MB)](https://repo.continuum.io/archive/Anaconda3-5.0.1-Linux-x86_64.sh) 的链接。

这是我们的文档:

```
# We will use Ubuntu for our image
FROM ubuntu# Updating Ubuntu packages
RUN apt-get update && yes|apt-get upgrade# Adding wget and bzip2
RUN apt-get install -y wget bzip2# Anaconda installing
RUN wget [https://repo.continuum.io/archive/Anaconda3-5.0.1-Linux-x86_64.sh](https://repo.continuum.io/archive/Anaconda3-5.0.1-Linux-x86_64.sh)
RUN bash Anaconda3-5.0.1-Linux-x86_64.sh -b
RUN rm Anaconda3-5.0.1-Linux-x86_64.sh# Set path to conda
ENV PATH /root/anaconda3/bin:$PATH# Updating Anaconda packages
RUN conda update conda
RUN conda update anaconda
RUN conda update --all# Configuring access to Jupyter
RUN mkdir /opt/notebooks
RUN jupyter notebook --generate-config --allow-root
RUN echo "c.NotebookApp.password = u'sha1:6a3f528eec40:6e896b6e4828f525a6e20e5411cd1c8075d68619'" >> /root/.jupyter/jupyter_notebook_config.py# Jupyter listens port: 8888
EXPOSE 8888# Run Jupytewr notebook as Docker main process
CMD ["jupyter", "notebook", "--allow-root", "--notebook-dir=/opt/notebooks", "--ip='*'", "--port=8888", "--no-browser"]
```

我描述了每个步骤，并在 Anaconda 安装部分中放置了到 Anaconda 文件的链接。所以我们需要看到:

![](img/be32608a3b1b532c44c6c7b990adf94a.png)

为了构建映像，我们需要运行命令:

```
docker build -t docker-data-science .
```

你可以看到每个步骤，从下载 Ubuntu 镜像开始:

![](img/a57d44811dc4f83444c0c1f19a47cb75.png)

和更新:

![](img/835127124db55118b1364acfa98a7090.png)

下载 Anaconda 安装程序:

![](img/bec2f3dcc0d7cce52ff8d22051f8a900.png)

安装 Anaconda:

![](img/cab3c1d393e3d2c29c72e1297aaffe40.png)

和更新软件包:

![](img/6e5e16aab10bc4d5dfe665a7a4782803.png)

您可以随时停止构建并重新运行它。Docker 保存每一步，这样你就可以从最后一点继续。如果您需要向 Dockerfile 添加一些新的包或其他依赖项并继续构建映像，它会很有帮助。

![](img/b99754e53c79508e6258264653c45084.png)

终于完成了。我们可以看到我们下载并创建的图像:

```
docker images
```

![](img/ee858235dc30fd7c6af1e0efcad81697.png)

保存 Docker 文件后，不要忘记在本地停止 jupyter 笔记本，因为我们将使用 Docker 容器中的相同端口:

![](img/9da21e92e2139c4af8dedd0bfa1e7c88.png)

现在，我们可以基于新映像运行容器了:

```
docker run --name docker-data-science -p 8888:8888 -v "$PWD/notebooks:/opt/notebooks" -d docker-data-science
```

![](img/a944beda71c897ff42ae0f163c6df9b9.png)

并打开 [http://localhost:8888/](http://localhost:8888/)

![](img/a77a463d2b41334f0820ab44fe6c2a26.png)

输入**根**

![](img/c74ad2e0e388df76798f35aacffdcf05.png)

如果我们创建一个新文件

![](img/dad76ed818048dbae63571dc2b30a755.png)

我们可以在子文件夹中看到它:

![](img/a564dd24e075205b84b5ffe765992dca.png)

所以我们的地图起作用了。我们可以运行容器，创建文件，测试它，它将被保存在 PC 上。

要停止容器运行:

```
docker rm -f docker-data-science
```

![](img/9f0d83540cef57df14791df26e06dada.png)

我们可以在浏览器中看到:

![](img/1fb47046094280ecaa653d0ce7993a36.png)

# 开源代码库

现在是时候在 github 上保存我们的 docker 文件了。之后，我们开始与 Docker hub 合作。

登录 github 后，点击创建新的存储库按钮或直接打开[https://github.com/new](https://github.com/new)。

![](img/84c167b8e61ab4e820159d93675f7810.png)

在我们看到添加和保存文件的步骤后:

![](img/37e7fe0a4022f20c64008d3634c5d4cc.png)

所以让我们发出这些命令:

```
git init
echo "notebooks" > .gitignore
git add .
git commit -m "first commit"
```

![](img/53b37bdacfe8ec9218eb4b0756ec8004.png)

命令**回显【笔记本】>。gitignore** 创建。gitignore 文件，我们可以把不想保存的目录或文件放在 git 中。在我们的例子中是笔记本。您只需要存储源代码。

接下来:

```
git remote add origin https://github.com/evheniy/docker-data-science.git
git push -u origin master
```

![](img/5bbc02032718d8f500c090dd974e75bf.png)

我们刚刚在 github 上保存了 docker 文件。如果我们重新加载页面，我们可以看到:

![](img/0ae8e55fe8ebad0dfd7b10026139e62d.png)

但是我们看到消息说我们需要创建 README.md 文件来描述我们的存储库。让我们来做吧:

![](img/6c51eb51471c91bd235ad6f0a577e8ed.png)

保存并刷新页面:

![](img/2d5546c13d56716b62e4cb3dce61b522.png)

现在您可以发出命令:

```
git clone [https://github.com/evheniy/docker-data-science.git](https://github.com/evheniy/docker-data-science.git)
```

并在任何带有 Docker 的 PC 上构建映像。但是如果你不想等半个小时，而它的建设，你可以建立一次，并保存在 Docker 枢纽。

# 码头枢纽

Docker hub 是一个类似 github 的 Docker 图片存储。你需要创建帐户，然后你可以存储自己的图像。

![](img/34f99fa801a6c019c663d544f3209770.png)

要存储图像，您需要点击[创建存储库](https://hub.docker.com/add/repository/)链接，并执行后续步骤:

1.  选择一个名称空间*(必需)*
2.  添加存储库名称*(必填)*
3.  添加简短描述
4.  将降价添加到完整描述字段
5.  将其设置为私有或公共存储库

![](img/90ef31a73ac0ef521fb9bab1b1d42dce.png)

保存后，我们将它放在 docker hub 上:

![](img/b1cdb7d887519a59dfb2b75254647e9d.png)

之后，我们需要提升我们的形象。但是在我们需要登录之前:

```
docker login
```

![](img/ecaab78a1223fbfcdc4236f6b406279c.png)

接下来，我们需要设置 docker 用户

```
export DOCKER_ID_USER="evheniy"
```

制作新标签:

```
docker tag docker-data-science evheniy/docker-data-science
```

和推送图像:

```
docker push evheniy/docker-data-science
```

![](img/7c02831fb7f2c6aacd019c818730a281.png)

我们的形象是一个巨大的。

![](img/bc3ce1d48cb234e97909700fe829c6bf.png)

我们可以看到刚刚创建的标记:

![](img/62e06c11f29d11d5193c8c9426b43820.png)

现在我们可以运行命令了

```
docker pull evheniy/docker-data-science
```

在任何电脑上获取图像

![](img/258c3ac60be567feeb2954841fcd45ca.png)

我们可以看到更新的 docker hub 配置文件:

![](img/c3e0976646e2906411b0ca74eabeb006.png)

# 自动构建

手动制作图像也可以。但是如果你在 github 上修改代码，Docker hub 可以帮助你自动构建它。为此，我们需要创建自动化构建:

![](img/f5d2d98e984a17dfa32c1cd5e7a188c8.png)

并链接 github 或 bitbucket 帐户—保存代码的地方:

![](img/12b488c595d095a8f97a6c3ada2ca2fe.png)

如果你是第一个，你应该连接你的账户。让我们为我们的 github 帐户制作:

![](img/35989ad59806dd694b24a44b1d59f4c0.png)

一些有用的信息:

![](img/e5333cb4aaaf4c9f328106fb707d3ffe.png)

和链接账户。我们需要达成一致:

![](img/7c265972ee05471ce7e36d8a20c637b8.png)

我们可以看到结果:

![](img/83de2860008042f7e8e443a48cbaa000.png)

现在我们可以创建自动构建:

![](img/f519351ce952f7e02659cf3c044beae0.png)

为此，我们需要选择 github 存储库，在我们的例子中是 docker-data-science:

![](img/02d1f3cc30a2b7eb3a1e915a40dd8626.png)

一些配置:

![](img/3c1c8b89283ac9b96453fb3d1d93e217.png)

我们需要重命名构建存储库，因为我们有其他同名的存储库:

![](img/9275e245a38f7d543c5c23ccea2881a7.png)

我们刚刚为我们的存储库创建了一个新的自动化构建:

![](img/3ecae0965fcf0f8a0c951bea3f7dacf8.png)

让我们更新代码来运行 build:

![](img/96d788751f06a6fa16f1458bf65673c4.png)

我们看到我们的变更正在排队:

![](img/48c23f3ac8cac3ec1b3ad18cd6fff120.png)

几分钟后，is 开始构建:

![](img/3876c67d11b3bcb129141f6317e259b5.png)

要构建所有步骤，我们需要等待 40 分钟:

![](img/4c1d3e85b85b14f3dcde99cd619c784c.png)

之后我们可以看到结果:

![](img/48f1823a52085d2ecf1ca1fe1777e621.png)![](img/33b6c205ff58af236d13c22e0cb9dc40.png)![](img/d76ffe169a4f4461dafa2f44c0582c29.png)

在本文中，我们用 Anaconda 和 docker 映像创建了本地环境。每当我们在 Docker hub 上保存一个新的映像时，以及在我们创建了自动构建配置之后，跳过构建。我们将在下一篇文章中使用这张图片，在那里我将展示一些关于机器学习和大数据的有用东西。

# 参考

*   [使用 Docker 做正确的事情](https://hackernoon.com/making-right-things-using-docker-7296cf0f6c6e)
*   [用 Docker 从零开始进行 TensorFlow 对象检测](https://medium.com/@evheniybystrov/tensorflow-object-detection-with-docker-from-scratch-5e015b639b0b)
*   [https://anaconda.org/](https://anaconda.org/)
*   [https://www.docker.com/](https://www.docker.com/)
*   [http://jupyter.org/](http://jupyter.org/)