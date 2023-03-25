# 如何使用 Selenium 和 Docker 刮测网站

> 原文：<https://towardsdatascience.com/an-introduction-to-web-browser-automation-with-selenium-and-docker-containers-c1bcbcb91540?source=collection_archive---------2----------------------->

你想不费吹灰之力就能访问大量数据并测试你的网站吗？你是否有堆积如山的计算机程序可以处理的在线繁重工作？然后你需要**浏览器自动化**和 **Selenium WebDriver** 。Selenium 是测试和抓取 Javascript 呈现的网页的首要工具，本教程将涵盖在任何操作系统上设置和使用它所需的一切。Selenium 依赖项可以下载到运行在 Linux 虚拟机上的 **Docker 容器**中；因此，这些技术被介绍和讨论。最后，介绍了 Docker 中的编程，以及设置 Selenium 并将其绑定到 **RStudio** 的分步协议。准备好进入浏览器自动化的世界吧，在那里你单调乏味的任务将被委托给一个守护进程，你的前额叶官能将被解放出来专注于哲学。

# **为什么**

**为什么要用 Selenium 来自动化浏览器？**

为什么要使用 Selenium 来自动化 web 浏览器？如上所述，两个主要原因是 **web 测试**和**数据抓取**。如果没有网络测试，像苹果这样的公司的程序员将无法在新功能上线前检查它们是否如预期那样工作，这可能会给用户带来不幸的错误，(就像发生在 iOS 12 T21 更新中的那些错误一样)。虽然当苹果这样的公司发布有问题的软件时，客户通常会感到震惊，但 iPhone 的复杂性和每次更新的新功能或更新功能的数量(iOS 12 几乎有 100 个)至少极有可能发生一些事故。不仅每个新组件都必须测试，而且它与手机其他部分的交互也必须检查。

然而，通过彻底的测试可以避免错误，这就是浏览器自动化的用武之地。虽然[手动测试](https://techbeacon.com/how-tech-giants-test-software-theres-no-one-way-qa)仍然是测试协议不可或缺的组成部分，但是完全手动测试如此多的复杂功能及其交互是不切实际的。有了浏览器自动化，用例可以在不同的环境中被测试成千上万次，从而找出只在不寻常的情况下才会出现的错误。然后，当苹果推出另一个重大更新时，它可以重新运行一个保存的测试协议，而不是设计一个新的，称为[回归测试](http://www.testingeducation.org/k04/RegressionExamples.htm)。因此，自动化测试允许公司提高客户满意度并避免错误。

![](img/25121d6fb8454f56e85daea724c852a2.png)

Photo by [Jeremy Bezanger](https://unsplash.com/@unarchive?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

在编程环境中驱动 web 浏览器的第二个原因是 web 抓取，这是从 web 页面中提取内容以便在您自己的项目或应用程序中使用的过程。虽然 web 抓取可以在没有 Selenium 这样的 web 驱动程序的情况下执行，但这种工具的功能是有限的。这些“无驱动”包，包括 R 中的 [Rvest](https://blog.rstudio.com/2014/11/24/rvest-easy-web-scraping-with-r/) 和 Python 中的 [beautiful soup](https://www.crummy.com/software/BeautifulSoup/) ，不能在浏览器中执行 Javascript，因此不能访问任何 [Javascript 渲染的元素](http://stanford.edu/~mgorkove/cgi-bin/rpython_tutorials/Scraping_a_Webpage_Rendered_by_Javascript_Using_Python.php)。虽然他们可以下载一个网站的源代码作为 HTML 文档，但是他们不能访问任何来自用户交互的数据。这是由于 HTML、CSS 和 Javascript 合作构建现代网页的特殊方式。

当你第一次打开一个网站时，你看到的内容来自于它的源代码，在谷歌 Chrome 中按 Ctrl+Shift+I 就可以随时查看，这个源代码主要是用 HTML 和 CSS 编写的，前者负责网站的[结构](http://motherfuckingwebsite.com/)，后者负责网站的[风格](http://www.csszengarden.com/)。虽然对 HTML 和 CSS 的详细讨论超出了本文的范围，但我们需要知道的是， [HTML 标签和 CSS 选择器](https://www.codingdojo.com/blog/html-vs-css-inforgraphic/)格式化 web 元素，两者的结合赋予每个 web 元素自己唯一的标识符。这些唯一的标识符允许无驱动 web 抓取器区分 web 元素，并仅从源代码中提取相关信息。

然而，当用户与网站交互时，Javascript 会形成新元素并改变现有元素。这些元素不包含在页面的源代码中，因此不能被上面提到的包访问。例如，大多数网站要求用户在访问信息前登录，这意味着登录后的一切都将超出无人驾驶网络抓取器的范围。如果互联网真的是“信息高速公路”，源代码只是一个入口，它在入口就结束了。有了 Selenium，只要我们知道如何驾驶，我们就可以驶向目的地数据，人口无限。

![](img/e0c753f79dd54724d4a88205583bd861.png)

Photo by [Nick Fewings](https://unsplash.com/@jannerboy62?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 怎么做

**虚拟机和容器**

Selenium 不能简单地下载和运行，因为它需要 1)特定的库和 2)特定的操作系统(例如，它不能在 [OS X](https://www.seleniumhq.org/about/platforms.jsp) 上运行)。虽然在过去**虚拟机(VM)**提供了这样的依赖关系，但是今天使用**容器**和 **Docker** 的组合要高效得多。为了理解为什么，我们首先需要回顾一下这些工具的用途和基础。

虚拟机是一台计算机在另一台计算机中的模拟。一台主机内可以运行多个虚拟机，主机和仿真之间的交互由一个 [**虚拟机管理程序**](https://www.techopedia.com/definition/4790/hypervisor) 管理。因为虚拟机包含了物理计算机所做的一切，所以它们经常被用来" T6 "测试新的操作系统(OS ),而不必购买新的硬件。其原因是，尽管许多人认为操作系统实际上是可以互换的，但它们之间可能存在显著差异。操作系统不仅仅是主板上的一个徽章；相反，它是您必须用来让硬件做您想要做的事情的工具集。如果操作者是一个画家，那么操作系统就是画笔，是从思想到现实的翻译者，可以限制或扩展可能性。因此，由于您选择的操作系统可能会产生深远的影响，因此轻松测试新操作系统的能力是非常宝贵的。

虽然虚拟机对于测试新的操作系统仍然有用，但是容器是为软件提供库的更好的方法。因为你下载的每个 VM 都有自己的操作系统，使用一个来简单地提供库就像为周末野营旅行打包你的整个衣柜:当然你可能有你需要的一切，但是四个纽带和翼尖看起来肯定是多余的。(也许就拿[四分之一布洛克鞋](https://www.realmenrealstyle.com/half-brogues-oxfords-video/)吧，老兄。)这就是[容器](https://www.cio.com/article/2924995/software/what-are-containers-and-why-do-you-need-them.html)的用武之地。如果虚拟机是贪婪的过度包装者，容器就是残酷的随身携带的填鸭式包装，因为每个容器只带有运行一个软件所必需的库。机器上的每个容器共享 1 (Linux) [OS](https://forums.docker.com/t/in-docker-for-windows-does-each-container-run-in-separate-vm/19192/5) ，这大大减少了它们的存储空间和运行时间。但是他们从哪里得到这个 Linux 操作系统来运行呢？这就是 Docker 的用武之地。

# **码头工人**

Docker 是运行和分发容器的领先软件，它的主要目的是提供容器运行的 Linux 操作系统。这个 Linux 操作系统在 [Windows](https://stackoverflow.com/questions/48251703/if-docker-runs-natively-on-windows-then-why-does-it-need-hyper-v) 和 [Mac](https://docs.docker.com/docker-for-mac/docker-toolbox/#the-docker-for-mac-environment) 中由每个操作系统的本机管理程序管理(分别是 [Hyper-V](https://en.wikipedia.org/wiki/Hyper-V) 和 [HyperKit](https://github.com/moby/hyperkit) )。因此，在启动和运行时，容器可以利用 Linux 操作系统的各个方面，包括它的文件系统和内核。两个重要的 Linux 组件，**守护进程**和**命令行接口(CLI)** ，共同组成了 [**Docker 引擎**](https://searchitoperations.techtarget.com/definition/Docker-Engine) ，它用于执行容器的大部分任务。docker [守护进程](http://www.linfo.org/daemon.html)是一个在后台运行的服务器，等待特定的事件或者用户调用它。当我们想要在 Docker 中完成一些事情时，我们使用 CLI 向守护进程(也称为`dockerd`)发送消息。调用 Docker 守护进程的命令称为 **Docker 命令**，它们的使用遵循一个通用模板，我将在下面介绍。

[大多数](https://docs.docker.com/engine/reference/run/#general-form) docker [命令](https://docs.docker.com/engine/reference/commandline/docker/)包含一个**动作**，一个**路径**，和**选项。****动作**被写成 docker，后跟我们希望守护进程做的事情。例如，如果我们希望守护进程启动一个容器，它必须运行一个**映像**，所以动作是`docker run`。(图像只是一个文件，当它被执行时，启动容器——如果容器是一个蛋糕，那么图像就是食谱。)路径**指定了我们希望守护进程对哪个文件执行操作，以及文件的位置。在`docker run`中，路径将告诉守护程序在哪里可以找到图像，(默认情况下是 [**Docker Hub**](https://hub.docker.com/) ，Docker 的基于云的图像**存储库**以及图像文件的名称。如果一个文件有不同的版本，可以通过提供一个**标签来选择一个；**如果未指定标签，则自动提取最新版本。最后，**选项**修改命令。以`docker run`为例，有数百个选项(你可以在它的[参考页](https://docs.docker.com/engine/reference/commandline/run/#options)上看到)。虽然您可以忽略或使用大多数的缺省值，但是有些确实需要指定，正如我们将在下面看到的。**

# **docker 运行中的选项**

因为`docker run`启动我们的容器，所以它是最重要的 Docker 命令之一。因此，它有如此多的选择是有道理的。这可能会使代码看起来复杂，正如您在下面启动 Selenium ChromeDriver 的示例中看到的那样:

```
docker run -d -v LOCAL_PATH://home/seluser/Downloads -p 4445:4444 — shm-size = 2g — name YOUR_CONTAINER_NAME selenium/standalone-chrome
```

事实上，上面的代码非常简单，在动作和路径之间只指定了 5 个选项。下面我们来回顾一下这 5 个选项。

1.  `-d` **选项**告诉容器以[分离模式](https://medium.freecodecamp.org/dockers-detached-mode-for-beginners-c53095193ee9)运行，这意味着在后台运行。这使得应用程序的输出保持隐藏，允许我们继续使用终端。
2.  `**-v**` **选项**被称为**绑定挂载，**对于数据抓取是必不可少的。这个选项告诉 Docker 将容器运行所在的 Linux VM 中的某个目录绑定到主机(即我们的家庭计算机)中的某个文件夹。这意味着下载到那个 Linux 目录的任何东西都将被转移到我们在机器上指定的文件夹中。当你关闭 Docker 和你正在运行的容器时，保存到它们的数据不会持久，所以这是保存我们数据的一个非常重要的步骤！(另一个选项是使用[永久卷](https://docs.docker.com/storage/volumes/#start-a-service-with-volumes)。)要使用`-v`选项，首先在您的家庭计算机上指定您想要将数据转移到的文件夹，然后在 Linux VM 上指定您想要使用的目录，用冒号分隔。当您真正开始运行 Selenium 时，请确保让您的代码将数据保存到您指定的 Linux 目录中！
3.  `**— shm-size**` **选项**增加了/dev/shm 目录的大小，这是一个[临时文件存储系统](https://en.wikipedia.org/wiki/Tmpfs)。这是因为容器上的默认共享内存对于 Chrome 来说太小了。在这次 [github 讨论](https://github.com/docker/docker-py/issues/1249)之后，我成功地将大小设置为 2g。
4.  `-p` **选项**指定容器和 Linux VM 应该通过哪些端口连接。我们首先在 Linux VM 上指定端口，然后在容器上指定端口。Selenium 映像[默认公开端口 4444](https://rpubs.com/johndharrison/RSelenium-Docker) ，这里我们使用 4445 作为主机端口。当我们稍后将 Linux VM 和其中的 Selenium 容器绑定到 RStudio 时，我们将使用这个面向外部的端口 4445。
5.  `**— name**`选项允许我们给容器一个特定的名称。如果我们不指定名称，Docker 将使用其默认命名系统为我们的容器命名，这实际上非常酷。UUID 是一长串难以阅读和记忆的数字和字母，Docker 没有使用它，而是随机合并一个形容词和一位著名的科学家，不知何故,[组合](https://github.com/moby/moby/blob/master/pkg/namesgenerator/names-generator.go)总是很吸引人。(在一个名为 kickass_chandrasekhar 的容器中编码感觉也很好。)

就是这样！这 5 个选项是你创建上面复杂的`docker run`命令所需要的。

既然我们已经牢牢掌握了 Selenium、VMs、containers 和 Docker，是时候最终下载并设置 Selenium ChromeDriver 了。我们走吧！

# **下载和设置 Docker 和 Selenium ChromeDriver 的步骤**

1.  下载适合您的操作系统和工作类型(商务、个人等)的 [Docker](https://store.docker.com/editions/community/docker-ce-desktop-windows) 版本。).Docker 同时提供[企业](https://docs.docker.com/ee/supported-platforms/#docker-certified-infrastructure)和[社区版](https://docs.docker.com/docker-for-windows/install/) s (CE)。对于那些想更深入了解容器的人来说，莫比把容器的组件分开，允许用户像组装乐高玩具一样单独组装。对于我们的目的来说，Docker CE 可以很好地工作。
2.  下载[T22](https://www.seleniumhq.org/download)[chrome driver](http://chromedriver.chromium.org/downloads)。
3.  在 Windows 中，您需要确保启用了虚拟化，以便 Docker 可以启动 Linux VM。您可以通过导航到 BIOS 并启用虚拟化来实现这一点(在 BIOS 中称为 VT-AMD)。(要访问 BIOS，请在 Windows 启动时按 F10，然后转到系统配置。)
4.  执行安装和设置 Docker 的步骤。最后，码头工人的特色鲸鱼将出现在码头。
5.  通过在终端中键入`docker pull selenium/standalone-chrome`来获取 Selenium ChromeDriver 的图像。因为我们没有指定版本，所以将提取最新的版本。你应该看到`Using default tag: latest: Pulling from selenium/standalone-chrome`。然后你会看到`Status: Downloaded newer image for selenium/standalone-chrome:latest`
6.  使用上面的命令运行 Selenium Chromedriver。记得用您想要使用的文件夹和名称替换 LOCAL_PATH 和 YOUR_CONTAINER_NAME。

```
docker run -d -v LOCAL_PATH://home/seluser/Downloads -p 4445:4444 — shm-size = 2g — name YOUR_CONTAINER_NAME selenium/standalone-chrome
```

现在我们已经设置并运行了 Docker，我将向您展示如何使用 RSelenium 将它绑定到 RStudio。如果你不是 R 用户，有关于如何将 Selenium 绑定到其他编程语言的文章，比如 [Python](https://selenium-python.readthedocs.io/installation.html#introduction) 和 [Ruby](https://github.com/SeleniumHQ/selenium/wiki/Ruby-Bindings) ，或者你可以简单地在 Docker CLI 中编写脚本。

# **下载硒元素**

1.  在 RStudio 的控制台中键入`install.packages(RSelenium`。
2.  然后:`library(RSelenium)`
3.  设置 Chrome 驱动程序的选项。还有[其他的](https://support.saucelabs.com/hc/en-us/articles/115002325914-Customizing-Chrome-with-ChromeOptions)可以设置，但是这三个是必不可少的。第一个将阻止弹出窗口，第二个将确保文件下载不需要您的提示，第三个将决定下载文件的最终位置(应该是您之前在`docker run`中指定的 Linux VM 中的目录)。

```
eCaps <- list(
 chromeOptions = 
 list(prefs = list(
 “profile.default_content_settings.popups” = 0L,
 “download.prompt_for_download” = FALSE,
 “download.default_directory” = “home/seluser/Downloads”
 )
 )
)
```

4.创建从 R 到 Linux 虚拟机的绑定。浏览器名称是 Chrome，端口是在`docker run`中指定的端口，`extra capabilities`是在上面的步骤 3 中指定的，而`remoteServerAddr`是 Linux VM 的 IP。

```
remDr <- remoteDriver(browserName= “chrome”, port=4445L, extraCapabilities = eCaps, remoteServerAddr = "192.168.99.100",)
```

5.最后，键入`remDr$open`将把 R 绑定到虚拟 OS。在您的全球环境中，您应该看到 remDr 是一个`<Object containing active binding>`。

那都是乡亲们！现在，您已经准备好开始使用 Docker 进行令人惊叹的 web 测试和数据抓取项目了！非常感谢您的阅读，如果有任何问题，请随时关注我的 twitter 账号@halfinit。下次见！