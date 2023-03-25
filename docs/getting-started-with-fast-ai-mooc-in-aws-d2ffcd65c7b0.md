# AWS 中的 Fast.ai MOOC 入门

> 原文：<https://towardsdatascience.com/getting-started-with-fast-ai-mooc-in-aws-d2ffcd65c7b0?source=collection_archive---------1----------------------->

[Fast.ai](http://www.fast.ai/) 提供深度学习课程系列。程序由杰瑞米·霍华德教授。有一个[入门](http://course.fast.ai/start.html)页面跟随说明。然而，安装对我来说并不容易。我想分享一些我遇到的问题和解决方法。

首先，我打开了这个[教学视频](http://course.fast.ai/lessons/aws.html)。我的计划是试图模仿教练。p2 类型实例不能立即使用，您需要从 [AWS](https://console.aws.amazon.com/support/) 请求它。AWS 的返回需要时间，所以可以在这段时间内使用 t2 类型的实例。让我们回到我们的电脑，我们需要用 Python 2.7 安装[Anaconda](https://www.continuum.io/downloads)和 [Cygwin](https://cygwin.com/install.html) 。教学视频会警告你与 wget 有关。这是我的第一个暗示。你也应该小心宋承宪。你应该选择 **openssh** 和**而不是跳过安装**。如果跳过，您会在连接实例的最后一步收到以下消息:

> -bash: ssh:找不到命令

如果你跳过它，你应该可以找到你几分钟前安装的安装文件。重复前面的点击，并选择 openssh 软件包和安装。在这些操作中不需要关闭终端。

好了，我们把[视频](https://www.youtube.com/watch?v=8rjRfW4JM2I)调回来。安装 Cygwin 后，我们将在 [AWS 控制台](https://console.aws.amazon.com/)中创建一个用户。重点是赋予管理员权限和保存访问密钥 id 和密钥的凭证。他们需要连接我们的实例。

此后，我们跳到 Cygwin 并写道:

> pip 安装 awscli

一切都是好的，但不要被愚弄。下一步可能是这样的:

> c:\ Users \ serdar \ anaconda 2 \ python . exe:无法打开文件
> 
> [错误 2]没有这样的文件或目录

这个问题有一个[解决页面](http://wiki.fast.ai/index.php/Awscli_in_cygwin)。然而这并没有帮助我。我的解决方案来自[这里](https://www.davidbaumgold.com/tutorials/set-up-python-windows/#installing-cygwin)。用 python 位置的路径更新 bash_profile 让我很开心。

> $ echo " PATH = \ $ PATH:/cyg drive/c/python 27 " > >。bash_profile
> $ source。bash_profile

好的。就是这样。AWS 配置目前可用。输入您之前保存的凭据、选定的区域和输出格式。现在我们需要通过脚本启动现成的实例。这里链接到[。关键是使用原始代码链接。t2 的示例:](https://github.com/fastai/courses/tree/master/setup)

> [https://raw . githubusercontent . com/fastai/courses/master/setup/setup _ T2 . sh](https://raw.githubusercontent.com/fastai/courses/master/setup/setup_t2.sh)

通过使用 wget，我们可以得到它，例如，键入:

> bash setup_t2.sh

将设置我们的实例。

安装完成后，它将打印连接信息。请注意这些信息。我的 ssh 问题是看到这一点。您将使用“ssh -l”和连接地址。终于，你进来了！你可以打开 Jupyter，通过你电脑的浏览器用“实例的 DNS:8888”连接。为了你的预算，不要忘记在完成工作后停止你的实例。祝你 MOOC 愉快..

![](img/f3162a1411bdcdb02e17d2497f8b9ad2.png)