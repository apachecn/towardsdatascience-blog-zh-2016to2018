# 使用 SSH 隧道连接到远程 Linux 机器上的 Jupyter 笔记本

> 原文：<https://towardsdatascience.com/connecting-to-a-jupyter-notebook-on-a-remote-linux-machine-277cef04abb7?source=collection_archive---------3----------------------->

![](img/9c33b63aa6325aee9b8b42be96b7476b.png)

Photo by [Florian Olivo](https://unsplash.com/@florianolv?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

## 我的工作桌面是一台功能强大的机器，我用它来进行探索性的数据分析和其他机器学习工作流程。

在这篇文章中，我将回顾当我远程工作时，我是如何连接到我的工作机器并运行 [Jupyter 笔记本](http://jupyter-notebook-beginner-guide.readthedocs.io/en/latest/what_is_jupyter.html)工作负载的。

# SSH 到远程机器

第 1 步是 ssh 到您的远程机器，并使用`--no-browser`选项启动`Jupyter Notebook`到本地端口。

```
user@local_machine$ ssh user@remote_machine 
user@remote_machine$ jupyter notebook --no-browser --port=8889
```

# 设置 SSH 隧道

第 2 步是建立一个从本地机器到远程机器上的`port 8889`的 [SSH 隧道](https://www.ssh.com/ssh/tunneling/)，在远程机器上`Jupyter Notebook`被提供服务。

```
user@local_machine$ ssh -N -L localhost:8888:localhost:8889 user@remote_mahcine
```

下面是 ssh 选项的分类

*   -N 不要执行远程命令。这对于转发端口非常有用
*   -L local _ socket:remote _ socket
    指定到本地(客户端)主机上的给定 TCP 端口或 Unix 套接字的连接将被转发到远程端的给定主机和端口或 Unix 套接字。这是通过分配一个套接字来监听本地端的 TCP 端口(可选地绑定到指定的 bind_address)或 Unix 套接字来实现的。每当连接到本地端口或套接字时，该连接都通过安全通道转发，并且从远程机器连接到主机端口 host port 或 Unix 套接字 remote_socket。
*   也可以在配置文件中指定端口转发。只有超级用户可以转发特权端口。可以通过将地址括在方括号中来指定 IPv6 地址。
*   默认情况下，根据 GatewayPorts 设置绑定本地端口。然而，显式 bind_address 可用于将连接绑定到特定地址。“localhost”的 bind_address 指示监听端口仅被绑定用于本地使用，而空地址或“*”指示该端口应该可从所有接口使用。

**注意**用`-N`运行 SSH 隧道将不会记录任何输出，只要你没有得到一个错误，这意味着你已经建立了一个隧道。

# 在本地浏览器上加载 Jupyter 笔记本

在本地机器浏览器上加载`localhost:8888`，来自远程机器的`Jupyter Notebook`将按预期加载。