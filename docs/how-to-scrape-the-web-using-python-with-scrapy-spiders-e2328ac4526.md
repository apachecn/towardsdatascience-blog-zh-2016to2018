# 如何使用 Python 和 ScraPy 蜘蛛抓取网页

> 原文：<https://towardsdatascience.com/how-to-scrape-the-web-using-python-with-scrapy-spiders-e2328ac4526?source=collection_archive---------2----------------------->

![](img/dbd515a26a229e8c43c29225641663bb.png)

Each of those hairs scrapes. Source: [Pixabay](https://pixabay.com/en/spider-macro-zebra-spider-insect-arachni-564685/)

有时候 Kaggle 还不够，还需要自己生成数据集。

也许你需要你正在训练的这个疯狂的卷积神经网络的蜘蛛图片，或者也许你为了，嗯，科学的目的想要刮 NSFW 子网格。不管你的理由是什么，浏览网页可以给你带来非常有趣的数据，并帮助你汇编令人惊叹的数据集。

在本文中，我们将使用 ScraPy 来抓取 Reddit 子编辑并获取图片。
有些人会告诉我[使用 Reddit 的 API](https://github.com/praw-dev/praw) 是一种更实用的获取数据的方法，这绝对是真的。确实如此，我可能很快会写一篇关于它的文章。
但是只要我们做得很少，并且不要让 Reddit 繁忙的服务器超负荷工作，应该没问题。所以请记住，本教程仅用于教育目的，如果你需要 Reddit 的数据，你应该使用官方渠道，比如他们的[牛逼 API](https://github.com/praw-dev/praw) 。

那么，我们如何着手清理网站呢？让我们从头开始。

# 检查机器人. txt

首先我们将进入 reddit.com/robots.txt。对于一个站点来说，习惯上是让他们的 *robots.txt* 文件可以从他们的主域访问。它遵循以下格式:

```
User-agent: <pattern>
Disallow: <patterns>
```

其中 U *ser-agent* 描述一种设备类型(我们属于*，通配符模式)，而 *Disallow* 指向一个我们无法抓取的 url 模式列表。
我在那里没有看到/r/*，所以我*觉得*刮一个 subreddit 的主页也可以。出于礼节，我仍然建议你在任何严肃的项目中使用 API。

不尊重一个网站的 *robots.txt* 文件可能会有法律后果，但这主要只是让你看起来像一个卑鄙的人，我们不希望这样。

# 建立我们的项目。

为了用 Python 抓取网站，我们将使用 ScraPy，它是主要的抓取框架。有些人喜欢美丽的声音，但我觉得 ScraPy 更有活力。

ScraPy 的基本抓取单元叫做*蜘蛛，*，我们将通过创建一个空蜘蛛来开始这个程序。

因此，首先，我们将安装 ScraPy:

```
pip install --user scrapy
```

然后我们将开始一个零碎的项目:

```
scrapy startproject project_name 
```

您可以在此输入任何内容，而不是项目名称。该命令将创建一个目录，其中包含许多文件和 python 脚本。

现在，对于我们的最后一个初始化命令，我们将创建我们的第一个蜘蛛。为此，我们将运行 scrapy 的 *genspider* 命令，该命令将一个蜘蛛的*名称*和一个*域 url* 作为其参数。我会给我的取名为*小猫捕手*(小心:剧透)和爬行*reddit.com/r/cats*。

```
scrapy genspider kitten_getter reddit.com/r/cats
```

现在我们只进入/*spider*目录，不关注其余部分。和往常一样，我在[GitHub 项目](https://github.com/StrikingLoo/kitten-getter)中发布了我的代码。

# 搭建我们的第一只蜘蛛

在*蜘蛛*目录中，我们将打开名为 *kitten_getter.py* 的文件并粘贴以下代码:

这里发生了什么事？嗯，每个蜘蛛需要三样东西:一个*解析*方法，一个*开始请求*方法，和一个*名称*。

*   每当我们从控制台启动蜘蛛时，都会使用蜘蛛的名称*和*。
*   从控制台运行 spider 将使其从 *start_requests* 例程启动。
*   我们让例程在 URL 列表上执行 *http* 请求，并在它们的 *http* 响应上调用我们的*解析*方法。

为了运行它，我们所要做的就是在项目的目录中打开我们的终端并运行:

```
scrapy crawl kitten_getter
```

释放你的蜘蛛！让他们漫游网络，攫取宝贵的数据。

如果您运行该命令，它将运行我们刚刚编写的蜘蛛，因此它将发出请求，获取我们提供的 *url_list* 中第一个 *url* 的 HTML，并按照我们要求的方式解析它。在这种情况下，我们所做的就是将整个响应直接写入一个名为“kitten_response0”的文件中(大小约为 140Kb)。

如果你打开它，你会看到它只是我们刮的网站的 HTML 代码。这对我们的下一个目标很有用。

# 识别模式

如果你去链接[reddit.com/r/cats](http://reddit.com/r/cats)寻找小猫图片，你会注意到有两种帖子。

*   点击链接到评论区的文章。
*   直接指向 pic 的帖子

我们还注意到，我们无法在不违反 *robots.txt、*的情况下找到任何与*reddit.com/r/*/comments/**匹配的内容，因此从帖子中提取图片是错误的。然而，如果图片直接链接到 subreddit 的主页，我们可以获得图片的 URL。我们看到那些链接总是在一个*<>*标签中的 *href* 属性，所以我们要做的是调用响应对象的 *xpath* 方法来获得它们。

xPath 是一种在网站的 HTML 树中移动并获取其中一些元素的方法。Scrapy 还为我们提供了 *css* 方法，它允许一种不同的索引和标记元素的方式。我个人发现在浏览器中右键单击一个元素，点击 inspect，然后 *copy xpath* 是一种快速的开始方式，然后我只是稍微摆弄一下输出。

在这个特殊的例子中，因为我们需要的只是每个*<>*元素的 *href* 值，我们将调用

```
response.xpath(‘//a/@href’)
```

这将为每个 *href* 值(ScraPy 库中的一个对象)返回一个迭代器。然后，我们通过调用 extract 方法提取该值的字符串形式，并通过查看它是否以'结尾来检查它是否实际上是一个到图像的链接。png '或'。jpg。
下面是整个改进的解析方法，它现在还创建了一个 html 文件来显示所有图像，而无需下载它们:

因此，我们让我们的蜘蛛再次爬行，输出应该是这样的:

```
Crawled (200) <GET [https://www.reddit.com/r/cats/](https://www.reddit.com/r/cats/)> (referer: None)
[https://i.imgur.com/Au0aqkj.jpg](https://i.imgur.com/Au0aqkj.jpg)
[https://i.imgur.com/Xw90WFo.jpg](https://i.imgur.com/Xw90WFo.jpg)
[https://i.imgur.com/fOINLvP.jpg](https://i.imgur.com/fOINLvP.jpg)
```

其中每个链接都是一只可爱小猫的图片。作为奖励，文件*kittens.html*应该洋溢着可爱。

就是这样！您已经成功抓取了您的第一个网站！

## 保存图像

假设我们想下载图片，而不是制作一个 HTML 文件。然后我们要做的是导入 Python 的*请求*库，以及 *unicodedata* 库。*请求*将完成这项繁重的工作，但是我们需要 *unicodedata* ，因为提取的字符串默认为 *unicode* ，而*请求*需要 ASCII 码。

现在，我们将传递我们的 *scrapy，而不是解析方法。请求*函数将下面的函数作为回调参数:

它所做的只是下载一张图片，并将其保存为 JPG。它还自动增加存储在蜘蛛中的索引属性，为每张图片命名。

## 到处玩:交互式 shell

ScraPy 为我们提供了一个交互式的 shell，在这里我们可以尝试不同的命令、表达式和 xpaths。这是一种比用 *crawl* 命令一遍又一遍地运行整个程序更有效的迭代和调试蜘蛛的方式。要启动 shell，我们只需运行以下命令:

```
scrapy shell ‘http://reddit.com/r/cats’
```

当然，这个网址可以用其他任何网址代替。

## 伸出我们的蜘蛛

如果我们想得到更多的图像，我们可以让 *download_pictures* 方法调用 *scrapy。请求下一页的 URL 上的*，可以从“下一页”按钮的 *href* 属性中获得。我们还可以让蜘蛛将 subreddit 作为参数，或者更改下载的文件扩展名。

总而言之，最好的解决方案通常是最简单的，所以使用 Reddit 的 API 会让我们省去很多麻烦。

我希望你现在能够制作自己的蜘蛛，并获得自己的数据。请告诉我您是否觉得这很有用，以及您认为使用此工具可以生成什么样的好数据集—越有创意越好。

最后，还有一本我喜欢的奥莱利的书。当我开始我的数据科学之旅时，我发现它非常有用，它让我接触到了一个不同的、更容易使用(尽管不太灵活)的 Web 抓取框架。用 Python 叫做[从零开始的数据科学，大概也是我得到这份工作的一半原因。如果你读到这里，你可能会喜欢它！](https://www.bookdepository.com/book/9781491901427/?a_aid=strikingloo&chan=ws)

*关注我，获取更多 Python 教程、技巧和诀窍！*

*你可以在我的* [*个人网站*](http://strikingloo.github.io/wiki) *中看到我正在做的事情以及我最近的文章和笔记。*