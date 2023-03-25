# 将 Twitter 数据流入 MySQL 数据库

> 原文：<https://towardsdatascience.com/streaming-twitter-data-into-a-mysql-database-d62a02b050d6?source=collection_archive---------4----------------------->

![](img/8d46049d538a21259be7acfc001743bb.png)

鉴于我经常看到数据库语言被列为数据科学工作的要求，我认为今天在 MySQL 上发表一篇文章是个好主意。特别是，我想看看我们如何使用 python 和 API 将数据直接传输到 MySQL 数据库中。我最近为一个个人项目这样做了，我想我应该分享我的代码，并为那些可能不熟悉这些工具的人提供一个入门教程。

我们将使用 Twitter API 来搜索包含特定关键字的推文，并将其直接传输到我们的数据库中。一旦我们做到了这一点，数据将随时可供进一步分析。这项任务需要几样东西:

1.  Twitter 帐户和 API 凭证
2.  MySQL 数据库
3.  Tweepy 和 mysql-connector Python 库

# Twitter API

在我们访问 API 之前，您需要设置一个 twitter 应用程序。我不会就此做深入的指导，但简单地说，你需要做以下事情:

*   转到以下网站[https://developer.twitter.com/](https://developer.twitter.com/content/developer-twitter/en.html)并创建一个帐户。(这一步比以前要复杂一些，包括提供一个简短的概要，说明你打算如何处理这些推文，以及它们的用途。我认为这与新的欧盟隐私法有关。)
*   一旦您验证了您的电子邮件，您就可以登录您的帐户。你应该可以在下面的网页上创建一个新的应用:【https://developer.twitter.com/en/apps 
*   填写关于您的应用程序的所有详细信息，然后创建您的访问令牌。
*   记下您的消费者密钥、消费者机密、OAuth 访问令牌和 OAuth 访问令牌机密。这些是连接到 API 所需要的。

想要更完整的教程，我推荐这篇[的博文](https://iag.me/socialmedia/how-to-create-a-twitter-app-in-8-easy-steps/)。完成这些步骤后，如果我们编写了正确的代码，我们的应用程序现在就可以连接到 Twitter 流 API 了。接下来，我将完成 MySQL 数据库的设置，这样我们就有了存储所有数据的地方。

# MySQL 工作台

有许多不同类型的数据库可以用于这个特定的任务，包括 NoSQL 数据库，如 MongoDB 或 Redis。然而，我选择使用 MySQL，因为我对它更熟悉，而且它仍然是最受欢迎的数据库之一。在开始之前，我们需要安装 MySQL Workbench 和 MySQL server。[这里的](https://www.youtube.com/watch?v=iOlJxOkp6sI)是一个视频教程，解释如何安装两者，并设置好一切开始收集数据。

一旦你完成了上面的教程，你应该已经建立了一个连接和一个模式/数据库(我的数据库被形象地称为 **twitterdb** )。在我们设置好 MySQL workbench 并对界面有所熟悉之后，我们最终可以创建一个表来存储我们的 twitter 数据。创建一个表非常简单，我们可以使用 UI 甚至使用查询。使用 UI，我们只需右键单击数据库，然后单击 create a table。然后，我们可以直接输入列名和数据类型。此时，值得考虑我们要存储的数据以及它们将是什么样的数据类型。为了更好地理解我们需要的数据类型，我们应该看一看 TwitterAPI [文档](https://developer.twitter.com/en/docs/tweets/data-dictionary/overview/intro-to-tweet-json)。本质上，我想要写下 **tweet** 的人的**用户名**，创建**的时间**， **tweet** ， **retweet count** ，tweet 发起的**地点**和**地点**(下面有更多信息)。这相当于 6 列加上**主键**，我们可以如下定义数据类型:

*   主键:INT(11)
*   用户名:VARCHAR(255)
*   created_at: VARCHAR(45)
*   推文:文本
*   retweet_count: INT(11)
*   位置:VARCHAR(100)
*   地点:VARCHAR(100)

# 计算机编程语言

好了，现在我们已经建立了数据库，是时候开始学习 Python 代码了。我们希望代码做几件事:

1.  我们想要创建一个允许我们连接到 Twitter API 的类。
2.  我们还需要创建一些连接到数据库的代码，并将数据读入正确的列。

我们将使用 Tweepy 库，这将使我们很容易连接到 API 并开始传输数据。在我们开始之前，我们将再次看一些美味的[文档](https://tweepy.readthedocs.io/en/v3.5.0/)。在 Tweepy 文档中，我们可以找到一些真正有用的类和方法的例子，我们需要用它们来与 API 进行交互。下面的代码是一个简单的例子，它允许我们连接到 API 并从我们的时间线打印推文:

```
import tweepyauth = tweepy.OAuthHandler(consumer_key, consumer_secret)
auth.set_access_token(access_token, access_token_secret)api = tweepy.API(auth)public_tweets = api.home_timeline()
for tweet in public_tweets:
    print(tweet.text)
```

好吧，希望这很简单。看起来我们只需要设置我们的凭证，访问时间线并循环打印出来。我们想做的有点不一样。我们希望将实时推文传输到我们的数据库中，根据[文档](https://tweepy.readthedocs.io/en/v3.6.0/streaming_how_to.html?highlight=streamlistener)，需要做以下三件事:

1.  创建一个从 **StreamListener 继承的类。**
2.  从此类实例化一个对象。
3.  使用此对象连接到 API。

这似乎很简单。让我们看看我们是如何用 Python 来做这件事的( ***完整代码在*** 文章的最后)。我们需要导入一些库，还需要为数据库设置令牌、密钥和密码。我将所有这些保存在 **setting.sh** 文件中，该文件使用下面的代码调用，并将令牌和密钥设置为环境变量。

```
subprocess.call(“./settings.sh”, shell=True)
```

注意，在我们的导入中，我们需要 **mysql-connector** 。同样，这里有一些关于这个库如何工作的有用的例子。我们可以在终端(我在 Mac 上)使用 pip 命令安装任何我们没有的库，如下所示。然后，我们应该能够从脚本中导入这些库。

```
pip install mysql-connector
pip install tweepy
```

接下来，我们需要设置从 StreamListener 继承的类。我们将给这个班三种方法。这些是类已经实现的方法，我们将覆盖它们。下面的代码实现了这一点。

让我们一步一步地检查一下，以确保一切都很清楚。第一个方法， **on_connect()** 只是在我们连接到流时通知我们。每当我们的 HTTP 状态代码不是 200 (200 表示一切正常)时， **on_error()** 方法就会打印一个错误。感兴趣的代码列表:[https://en.wikipedia.org/wiki/List_of_HTTP_status_codes](https://en.wikipedia.org/wiki/List_of_HTTP_status_codes)

好了，下一个方法， **on_data()** 稍微复杂一点。为了理解这一点，我们需要对 tweets 的结构有更多的了解。当我们访问 API 时，我们得到一个 JSON 响应(非常类似于 python 字典的结构)。更多信息[在这里](https://developer.twitter.com/en/docs/tweets/data-dictionary/overview/tweet-object.html)。

本质上，我们返回的 tweet 对象如下所示:

```
{ “created_at”:”Thu Apr 06 15:24:15 +0000 2017", 
  “id”: 850006245121695744, 
  “id_str”: “850006245121695744”, 
  “text”: “1/ Today we’re sharing our vision for the future of the    Twitter API platform!nhttps://t.co/XweGngmxlP", 
  “user”: {}, 
  “entities”: {} }
```

因此，我们有一个 JSON 对象，它包含键、值对(注意，这里没有列出我们将使用的一些属性)。那么，我们实际上想要从中获得什么数据呢？tweet 对象实际上提供了相当多的信息，我建议浏览一下文档，看看您可能对哪些属性感兴趣。对于这个分析，我选择收集用户名、推文创建时间(如果我们随着时间的推移收集推文，这将更有用)、实际推文、推文的国家、位置(更接近本地)以及最后的转发数的数据，这反映在上面的代码中。

最后几行调用了将变量作为参数的 **connect()** 方法。这段代码全部被包装在一个 try 中，除了语句来捕捉我们可能遇到的任何错误。

好了，你可能已经注意到我们还没有创建 connect()方法，所以让我们创建它。这个方法并不奇怪，它会连接到我们的数据库并输入所有的数据。如前所述，该方法接受在 StreamListener 类的 on_data()方法中创建的变量作为参数，并将它们插入到数据库中同名的列中。要连接到我们的数据库，我们只需使用 **connector.connect** 方法，并传入我们的数据库信息，这些信息可以从 MySQL workbench 中找到。如果连接成功，就会创建一个游标对象，允许我们执行 SQL 语句。现在我们可以编写查询，并使用 execute 命令将数据插入到我们的 **twitterdb** 数据库的正确表中。当 is_connected()为 true 时，我们的数据库连接保持打开，并不断地将数据输入数据库，直到我们在终端中终止它(使用 Ctrl+C)。

我们可以创建一个单词列表来过滤流。我是一个高尔夫迷，所以我决定搜索与高尔夫相关的单词。实际上，你可以在这个列表中放入任何你想要的东西。

现在，我们只需要设置我们的脚本，以便在从终端执行文件时调用这些函数。为了访问 API，我们需要将我们的凭证作为参数传递给**oauthandler**方法和 **set_access_token** 方法。接下来，我们通过传入经过验证的 api 对象和侦听器来创建流。我们还可以在这里创建要过滤的单词列表。要启动这个流，我们只需在我们的流对象上调用 **filter** 方法，并将我们的单词列表作为参数传入。我将这个脚本保存为 **StreamSQL.py.**

如果我们想运行这段代码并开始收集 tweets，我们可以使用终端。这里需要注意的一件重要事情是，我们需要确保我们的 SQL server 已经启动并且正在运行，以便脚本能够正常工作，因此在运行脚本之前仔细检查这一点是值得的。

我们可以直接从存储脚本的文件夹中打开终端，只需输入:

```
python StreamSQL.py
```

# 结论

希望这表明用 MySQL 建立数据管道并不复杂，尤其是在利用 Python 提供的强大库时。SQL 是从事数据科学的一个非常重要的方面，如果你曾经在招聘广告中看到过数据科学家的要求，这一点是非常明显的。尽管这并不涉及对 SQL 或编写复杂查询的深入理解，但我认为能够理解这种任务仍然非常有用。

对于那些有兴趣学习更多 SQL 知识的人，我参加了以下免费的[课程](https://classroom.udacity.com/courses/ud198)，我发现它非常适合练习一些关键概念和理解一些更高级的功能。我向任何对提高 SQL 技能感兴趣的人推荐这本书。在以后的帖子中，我将集中精力提取我们收集的原始数据，清理它并执行一些简单的分析，这将说明我们可以用这种类型的数据做一些有趣的事情。

下次见！！

***链接第二部分:***[https://towardsdatascience . com/building-an-ETL-pipeline-in-python-f 96845089635](/building-an-etl-pipeline-in-python-f96845089635)