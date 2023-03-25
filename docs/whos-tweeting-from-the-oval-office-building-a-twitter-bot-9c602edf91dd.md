# 谁在椭圆形办公室发微博？

> 原文：<https://towardsdatascience.com/whos-tweeting-from-the-oval-office-building-a-twitter-bot-9c602edf91dd?source=collection_archive---------8----------------------->

# 建造一个推特机器人

![](img/7b2ee61d64c90b0e9ae4659e27edc540.png)

Look my Twitter bot!

在我的最后一篇帖子中，我描述了我如何使用机器学习建立一个模型，该模型可以预测川普本人或他的一名助手是否在他的账户上发推文。在这篇文章中，我将详细介绍我如何用 Twitter bot 部署这个模型。

我是 Greg Rafferty，湾区的数据科学家。你可以在我的 [github](https://github.com/raffg/trump-tweet-author-identification) 上查看这个项目的代码。

## 谁在椭圆形办公室发微博？

我开发了一个推特机器人[@ who sintheeval](https://twitter.com/whosintheoval)，它转发唐纳德·特朗普的每一条推文，并预测这条推文是由特朗普本人还是他的一名助手写的。如果你对这个小项目的起源感到好奇，请继续阅读本系列中的[上一篇文章](/whos-tweeting-from-the-oval-office-96ea5b60c03)，或者继续阅读以了解我是如何构建 Twitter bot 的！

## 模型部署

如果不能以其他人可以理解的格式打包，世界上最好的数据科学模型就一文不值。在这篇文章中，我将展示如何通过 Twitter 上的机器人部署我的模型。网上有很多教程讨论我在模型中使用的各种机器学习模型，这就是为什么我在以前的帖子中没有详细介绍它们。然而，当我开始构建这个机器人时，我发现网上缺乏关于如何构建 Twitter 机器人的明确信息，所以这篇文章将比以前的文章更具技术性，并将包括您需要的启动和运行机器人的代码。

在本教程中，我们将创建一个机器人，它会监视 [@realDonaldTrump](https://twitter.com/realDonaldTrump) 的任何推文，一旦有东西发布，机器人就会询问[@ who sintheval](https://twitter.com/whosintheoval)是谁发布的，是特朗普本人还是他的一名助手。

## 先决条件

1.  一个推特账户(去 https://twitter.com/signup 创建了一个)
2.  计算机编程语言
3.  Tweepy，一个使用 Twitter 的有用库(`pip install tweepy`)

## 从 Twitter 获取 API 访问权限

如果你正在构建一个 Twitter 机器人，你需要做的第一件事就是访问 Twitter 的 API。访问[apps.twitter.com](https://apps.twitter.com/)，用机器人将要发布的 Twitter 账户登录。填写表格并选中所有必要的复选框。登录后，访问“密钥和访问令牌”选项卡，生成一个新的访问令牌，您将需要使用它来验证您的应用程序。

## 存储凭据

你应该**永远不要**分享这些私钥，所以最好将它们隐藏起来，不要让任何你公开发布的代码看到。我在项目的根文件夹中创建了一个名为`.env`的文件夹，在本教程中，我假设你也是这样做的。在名为`twitter_credentials.json`的文件夹中创建一个新文件，并按照以下格式粘贴您的密钥和访问令牌(将所有大写单词替换为您的应用程序在 twitter 上的仪表板中的值):

Twitter 提供了一个下载数据的 REST API，但是对于实时数据来说，它不是你的最佳选择。如果你经常查看新的推文，你会很快达到速率限制。所以对于这个机器人，我们将使用流式 API。

## 初始设置

现在在你的项目文件夹中创建一个`twitterbot.py`文件(这个文件夹包含我们刚刚创建的`.env`文件夹)。在你喜欢的文本编辑器中打开`twitterbot.py`，导入 Tweepy 和 json。当然，Tweepy 是我们用来与 Twitter 交互的工具，json 将允许我们读取这些密钥并访问令牌。我们还将导入睡眠，这样如果达到 Twitter 的速率限制，我们可以暂时暂停我们的机器人:

## OAuth 认证

现在，让我们加载这些凭据，并设置 Tweepy 进行身份验证并连接到 Twitter:

## 识别要观察的用户

下一步，我们需要知道我们将监视的用户的 Twitter ID，在本例中是 [@realDonaldTrump](https://twitter.com/realDonaldTrump) 。[Gettwitterid.com](http://gettwitterid.com/)是一个做一件事的简单网站，我认为从它的 URL 就足够明显了。输入 Twitter 用户名，它会输出相应的用户 ID。对于‘realDonaldTrump’，这是`25073877`。让我们把它赋给代码中的一个变量(作为一个字符串！).

## 使用 Tweepy 流式传输

Tweepy 有一个对我们来说超级有用的类叫做`StreamListener`。我们将从它继承并重新定义`on_status`函数来执行我们想要的动作。让我们把我们的新类叫做`TrumpStreamListener`，因为稍后当我们开始流过程时，我们将指示这个类监视我们在上面的`realDonaldTrump`变量中指定的帐户。当我们这样做的时候，让我们也重新定义一下`on_error`函数。这个函数可以在 Twitter 返回错误时执行一个操作。在这种情况下，我们将注意错误`420`，这意味着我们已经达到了速率限制。每当你的机器人达到速度限制，Twitter 允许你重新进入的时间就会成倍增加，所以我们希望暂停在`420`上，并尝试在一分钟内重新连接，而不是不停地敲打 Twitter。

让我们浏览一下这个`on_status`函数。当我们开始流式传输时，我们将指示机器人监视由 realDonaldTrump 变量指定的帐户，但这也会捕捉任何关于他的转发或提及。我们只想要来自 Trump 的帖子，所以我们需要在函数的开头有一个`if`子句。我们发布的实际 tweet 将包含一个到 Trump tweet 的链接，因此我们分配了`url`变量，该变量根据流监听器抓取的 tweet 数据创建链接，并在`status`变量中定义。接下来，我们将编写实际的推文，它将是“这是谁发的推文，[@ Who sintheval](https://twitter.com/whosintheoval)？特朗普还是幕僚？”后面是特朗普的原推文，比如:

最后，我们将使用 Tweepy 的`update_status`函数，该函数将该 tweet 发布到我们的提要中。

为了让我的 Twitter 机器人预测@realDonaldTrump 推文的作者，我没有立即调用`api.update_status`，而是定义了一个新的`post_tweet`函数，它打开了我的机器学习模型的一个保存的 pickle 文件，调用了`.predict`和`.predict_proba`方法，然后编写并发布了推文。如果你想做一些比发布脚本化的 tweet 更复杂的事情，这就是你要编码的逻辑。如果你想了解更多细节，你可以在我的 GitHub 上找到我的完整代码。

## 开始流

现在，我们需要定义一个函数，确保在出现错误或暂时失去互联网连接的情况下，流不会死亡。如果由于任何原因中断，这个函数将自动重新开始流。它还指示流监听器对象遵循由变量`realDonaldTrump`、`25073877`定义的帐户。

## 开始做事

最后，让我们开始行动吧！最后这些命令将实例化该类，并调用`start_stream`函数来打开机器人。

最后，这些片段组合成了一个完整的程序:

你有它！如果你运行这个程序，它将持续监控 Twitter 上任何关于@realDonaldTrump 的活动。如果该活动是特朗普的账户发布的，那么你的机器人会向你自己的账户发布一条推文，询问[@ who sintheeval](https://twitter.com/whosintheoval)账户是否是特朗普本人或助手发布的。