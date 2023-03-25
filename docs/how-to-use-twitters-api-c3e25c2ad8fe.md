# 如何使用 Twitter 的 API

> 原文：<https://towardsdatascience.com/how-to-use-twitters-api-c3e25c2ad8fe?source=collection_archive---------2----------------------->

![](img/799ff64b1d8db9a965a187b0e1bf2bb5.png)

为什么要使用 API？API 代表应用程序编程接口(API ),它们允许您访问仅在服务器上可用的资源。让我们学习如何使用 twitters 的 API。

首先，你需要一个 twitter 账户，并附上你的电话号码作为验证。然后你需要去[apps.twitter.com](https://apps.twitter.com/)并创建应用程序，这样我们就可以引用 Twitter 为该应用程序生成的相应密钥。这些是我们将在应用程序中用来与 Twitter API 通信的键。

现在进入代码，我们需要安装 python-twitter API 库

```
!pip install python-twitter
```

确保你理解 Twitters 的 API 使用规则，如果你不遵守他们的规则，他们会阻止你访问数据。这里有一个链接，链接到他们的一些费率限制【https://dev.twitter.com/rest/public/rate-limiting 

现在我们有了我们的应用程序，我们有了与应用程序相关联的键。

## 按键设置

*   **消费者密钥**——在你的应用页面“密钥和访问令牌”下找到它
*   **consumer _ secret**——位于“密钥和访问令牌”选项卡中的 **consumer_key** 下方
*   **access_token_key** —您需要点击“生成令牌”按钮来获取此信息
*   **access_token_secret** —按下“生成令牌”后也可用

我们将在下面的函数中插入这些特定的键

```
import twitter, re, datetime, pandas as pdclass twitterminer():request_limit   =   20    
    api             =   False
    data            =   []

    twitter_keys = {
        'consumer_key':       , #add your consumer key
        'consumer_secret':     , #add your consumer secret key
        'access_token_key':    , #add your access token key
        'access_token_secret':  #add your access token secret key
    }

    def __init__(self,  request_limit = 20):

        self.request_limit = request_limit

        # This sets the twitter API object for use internall within the class
        self.set_api()

    def set_api(self):

        self.api = twitter.Api(
            consumer_key         =   self.twitter_keys['consumer_key'],
            consumer_secret      =   self.twitter_keys['consumer_secret'],
            access_token_key     =   self.twitter_keys['access_token_key'],
            access_token_secret  =   self.twitter_keys['access_token_secret']
        )def mine_user_tweets(self, user=" set default user to get data from", mine_retweets=False):statuses   =   self.api.GetUserTimeline(screen_name=user, count=self.request_limit)
        data       =   []

        for item in statuses:mined = {
                'tweet_id': item.id,
                'handle': item.user.name,
                'retweet_count': item.retweet_count,
                'text': item.text,
                'mined_at': datetime.datetime.now(),
                'created_at': item.created_at,
            }

            data.append(mined)

        return data
```

然后，我们需要实例化我们的类，以便能够使用我们上面的函数。

```
mine = twitterminer()
```

让我们使用 API 从唐纳德·特朗普的推特上获取一些推文

```
# insert handle we like
trump_tweets = miner.mine_user_tweets("realDonaldTrump")
trump_df = pd.DataFrame(trump_tweets)
```

我们现在已经成功地将来自唐纳德·特朗普推特的推文拉到我们的本地机器上进行分析。祝你收集数据顺利。