# 使用 Jupyter 笔记本电脑进行远程计算

> 原文：<https://towardsdatascience.com/remote-computing-with-jupyter-notebooks-5b2860f761e8?source=collection_archive---------1----------------------->

![](img/f55c0a283551f21b866f9d844af07c8a.png)

[Source](https://www.lynda.com/Linux-tutorials/Linux-Desktops-Remote-Access/517442-2.html)

 [## 想在数据科学方面变得更好吗？

### 当我在我发布独家帖子的媒体和个人网站上发布新内容时，请单击此处获得通知。](https://bobbywlindsey.ck.page/5dca5d4310) 

不久前，我让 AWS 为我提供了一个唯一的 URL，我可以导航并使用 Jupyter 笔记本。我钦佩它的便利性和能力，只要启动一个计算程序，关上笔记本电脑，就知道我的计算会继续进行。然而，根据您的使用情况，使用 AWS P2 实例会非常昂贵，对我来说，每月大约 600 美元。所以，我想我可以用这些钱建造一台电脑，作为深度学习的平台，偶尔玩玩视频游戏。

这篇文章描述了配置设置*一旦*你已经建立了你的电脑并且安装了像 Ubuntu 这样的 Linux 版本。事实证明，下面的配置对我来说比 AWS 更容易设置，在别名的帮助下，连接到我的服务器比以往任何时候都容易。我们开始吧！

# 装置

所以首先你需要在你的 Ubuntu 服务器上安装以下软件:

*   Anaconda ，它将提供许多您需要的默认 Python 包
*   openssh-server，可以安装以下软件:`sudo apt-get install openssh-server -y`
*   tmux，可以安装`sudo apt-get install tmux -y`

如果您需要查看 openssh-server 的状态或重启它，请在您的终端中键入以下内容:

```
systemctl status ssh sudo service ssh restart
```

# 本地连接到您的服务器

为了确保大多数事情都设置正确，我们首先需要验证您可以连接到本地网络上的服务器。

好吧！所以在你的服务器上，打开位于`/etc/ssh/sshd_config`的`sshd_config`文件。要对其进行更改，您需要 sudo 权限。文件打开后，您需要指定连接时要使用的端口。无论您选择什么，我强烈建议不要使用默认端口 22。假设您决定使用端口 22222。在您的`sshd_config`文件中有一个名为`Port`的条目，您应该这样编辑它:

```
Port 22222
```

在`AllowUsers`下，输入您登录服务器时使用的用户名。

```
AllowUsers your_username
```

接下来，将`PasswordAuthentication`设置为`yes`。

最后，为了确保 Ubuntu 不会阻止端口 8888 上的网络流量，我们需要调整它的 iptables:

```
sudo iptables -I INPUT -p tcp -s 0.0.0.0/0 --dport 8888 -j ACCEPT sudo iptables -I INPUT -p tcp -s 0.0.0.0/0 --dport 443 -j ACCEPT sudo netfilter-persistent save
```

现在，记下您的服务器的 IP 地址。这可以通过在您的终端中键入`ifconfig`并查找类似于`inet 192.168.1.112`的内容来找到。一旦您在本地网络上确定了服务器的 IP 地址，就该拿起您的笔记本电脑并尝试登录了:

```
ssh your_username@192.168.1.112 -p 22222
```

如果你得到一个终端提示，你就成功了！

# 远程连接到您的服务器

现在，设置远程计算的全部目的是，当你在别人的网络上时，你可以离开你的房子，远程进入你的服务器。要做到这一点，只需要做一些更改(要求您仍然在自己的网络上):

*   如果您的笔记本电脑上还没有一组公钥和私钥，[生成它们](https://help.github.com/articles/generating-a-new-ssh-key-and-adding-it-to-the-ssh-agent/)
*   将您的公钥复制到您的服务器:`ssh-copy-id your_username@192.168.1.112 -p 22222`
*   通过在您的服务器终端键入`curl 'https://api.ipify.org'`来识别您的服务器的 WAN IP 地址(注意，您的 ISP 经常更改此地址，这就是我使用 [Google Wifi](http://amzn.to/2vnHIjk) 的原因，它允许我从任何地方检查我的 WAN 地址)
*   在路由器上，打开端口转发。使用我们的示例，您需要将端口 22222 转发到端口 22222。

现在尝试使用您找到的 WAN 地址远程访问您的服务器！

```
ssh your_username@server_wan_ip -p 22222
```

如果你看到一个提示，干得好！最后一件事是在你的`sshd_config`文件中设置`PasswordAuthentication`为`no`，因为现在你用 ssh 密钥登录；这样，没有人可以尝试暴力破解您的密码。

你现在可以从你的网络之外访问你的服务器，去拿你自己的星巴克咖啡:)。

# 启动远程 Jupyter 笔记本

现在所有困难的工作都已完成，您可以通过以下步骤轻松使用远程 Jupyter 笔记本电脑:

1.  ssh 进入你的服务器:`ssh your_username@server_wan_ip -p 22222`
2.  开始一个新的 tmux 会话，您以后可以轻松地从该会话中退出:`tmux new -s session-name`
3.  无浏览器启动 jupyter-笔记本:`jupyter-notebook --no-browser --port=8889`
4.  现在，在笔记本电脑上的新终端中，将服务器端口流量转发到笔记本电脑的本地端口:`ssh -N -L localhost:8888:localhost:8889 your_username@server_wan_ip -p 22222`
5.  在您的网络浏览器中，导航到`localhost:8888/tree`，您应该会看到您的 Jupyter 笔记本！

现在，您可以在笔记本电脑上轻松使用 Jupyter 笔记本，而是使用服务器的强大资源来进行计算。

最后一件事，在想通了上面的步骤后，我想我应该通过使用别名和函数使这个过程变得更简单。以下是我添加到笔记本电脑的`.bashrc`文件中的相关行:

```
server-connect() {
    ssh your_username@$1 -p 22222
}jn-connect() {
    ssh -N -L localhost:8888:localhost:8889 your_username@$1 -p 22222
}
```

我添加到服务器的`.bashrc`文件中的行:

```
alias jn-remote="jupyter-notebook --ip='*' --no-browser --port=8889"
```

现在，上述 5 个步骤变成了:

1.  `server-connect server_wan_ip`
2.  `tmux new -s session-name`
3.  `jn-remote`
4.  打开新终端并键入`jn-connect server_wan_ip`
5.  导航至`localhost:8888/tree`

就是这样！现在，你可以在咖啡店里一边用笔记本电脑放松，一边运行你正在构建的那些疯狂的神经网络。祝你好运！

*如果你喜欢我在这里写的东西，一定要看看我的* [*个人博客*](https://bobbywlindsey.com) *，那里有我在媒体上没有的文章。*

*原载于 2017 年 8 月 10 日*[*bobbywlindsey.com*](https://www.bobbywlindsey.com/2017/08/10/remote-computing-with-jupyter-notebooks/)*。*