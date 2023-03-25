# 我们如何在 Twitter 上使用机器人来获得 1750 倍的廉价参与

> 原文：<https://towardsdatascience.com/how-we-used-bots-on-twitter-to-get-1750x-cheaper-engagement-68f90a6b3f6d?source=collection_archive---------0----------------------->

![](img/43ae47e5660cee3d39e78c4122dcc0c7.png)[](https://www.getyarn.io/yarn-clip/c19ed821-8b52-42eb-b8ac-28d2a3a92c01) [## 你好，你能听到我吗？

### Adele - Hello - Yarn 是通过引用找到视频剪辑的最佳方式。找到电视节目、电影或音乐中的确切时刻…

www.getyarn.io](https://www.getyarn.io/yarn-clip/c19ed821-8b52-42eb-b8ac-28d2a3a92c01) 

## 开始

我们像任何创业公司一样起步；我们有一个看似不错的想法，一个网站和没有用户。[**We ' s YARN**](https://getyarn.io):通过引用查找电影、电视和音乐视频剪辑的搜索引擎。这些剪辑只有几秒钟长，是由我们的网页抓取和索引过程自动生成的。我们相信人们会使用它们；我们需要一种方式让每个人都知道我们的存在。

[](http://www.getyarn.io/yarn-clip/2371e6a1-c91c-4882-ba33-4cf87b831472) [## 事实是，你甚至不知道我们的存在。

### 量子的安慰(2008)惊悚片-纱是最好的方法来找到视频剪辑的报价。在电视中找到准确的时刻…

www.getyarn.io](http://www.getyarn.io/yarn-clip/2371e6a1-c91c-4882-ba33-4cf87b831472) 

## 获取首批用户

我们对用户获取有了初步的想法。我们最初的努力是联系作家和博客，试图获得故事。当没有人认识你的时候，很难得到媒体的关注，但是我们有一些幸运的突破。公关是一项困难的工作，耗费时间，而且不是我们的核心竞争力。在产品发布之后，很难激起人们的兴趣。最终，我们需要能够更好地扩展的东西；我们只有两个人，公共关系既昂贵又不可预测。

[](http://www.getyarn.io/yarn-clip/eaf0c8eb-90ca-4740-aefd-abb88fe90e03) [## 我们需要更多的能量。这就是问题所在。

### 怪异科学(1985) - Yarn 是通过引用找到视频剪辑的最佳方式。在电视节目、电影或…

www.getyarn.io](http://www.getyarn.io/yarn-clip/eaf0c8eb-90ca-4740-aefd-abb88fe90e03) 

## 社会化媒体

社交媒体似乎比公关更具扩展性。这个想法是，人们会张贴奇谈怪论，其他人会看到它们，并依次学习张贴它们。gif 已经被证实了；奇谈比 gif 更好，因为它们在声音和语气的上下文中与你想说的内容完全吻合。我们通过在 Twitter、脸书、Reddit、Slack 和 Pinterest 等社交媒体上发布奇谈来播种这一努力。我们的朋友张贴了一些，以及喜欢和向上投票的帖子。这种方法的伸缩性更好，但仍然受到朋友数量和时间的限制。

[](https://www.getyarn.io/yarn-clip/550d142f-7c4d-4f94-bd07-f9b66386c47a) [## 我们人不够多，友好的。

### 拆迁人(1993) -纱是最好的方式找到视频剪辑的报价。在电视节目、电影中找到确切的时刻…

www.getyarn.io](https://www.getyarn.io/yarn-clip/550d142f-7c4d-4f94-bd07-f9b66386c47a) 

## 推特实验

我们首先利用了我们自己，然后是我们的朋友，我们需要更多。技术自动化有无限延伸的希望。我们在 Twitter 上尝试了三个实验，看看是否有可能使用自动化来提高参与度和认知度。

我们的前提是，因为故事很短，而且与上下文相关，我们会看到更好的参与度，因为即使对一个完全陌生的人来说，它们似乎也更私人。

我们进行了三个实验来测试我们的理论:

1.  手动向陌生人发推文
2.  用机器人发推文，目标是 [@TheEllenShow](https://twitter.com/TheEllenShow) 和 [@JimmyFallon](https://twitter.com/JimmyFallon)
3.  用一个机器人发微博，目标是 [@pewdiepie](https://twitter.com/pewdiepie) 和他自己的[内容](http://pewdiepie.getyarn.io/yarn-popular)

我们针对以下方面进行了优化:

1.  围绕一条推文的任何参与度(回复、点赞、转发、视频浏览量、页面浏览量)，无论浏览量多少。
2.  提高整体参与度。

## 实验 1:手动推文

一个多月来，我们通过我们的 [@yarnspun](https://twitter.com/yarnspun) 账户发出了 400 条推文，尝试了各种技术。我们将推文分为三种类型:普通推文、热门话题推文和对陌生人的回复。制作一条推文大约需要 2-3 分钟；你需要找一个人回复，找一个故事，发一条微博。这个实验总共花了大约 20 个小时:假设 500 美元每小时 25 美元，或者一条推文大约 1.25 美元。

```
Type     Impressions Engagement/tweet Engagement/imp Cost/engagement       Tweets   11,239      27.59%           0.33%          $0.034        
Trending 33,282      69.03%           0.94%          $0.004        
Replies  46,328      78.95%           2.65%          $0.001
```

## 实验 1:手动推文结果

1.  回复比一般的推文更吸引人。除非你是一个显著的影响者，点回复会得到更多的参与。
2.  用一根纱线来强调一点，下面是一个例子:

[https://twitter.com/yarnspun/status/743832410740121600](https://twitter.com/yarnspun/status/743832410740121600)

3.有些热门话题很棒，有些很糟糕。热门话题有利于参与，因为人们通常“实时”发布推文，所以你的推文更有可能被看到。但是，你需要有参与度的话题；这本身就是一篇博文。

4.我们针对拥有 100-10，000 名粉丝的个人进行回复。这不是唯一的规则，但是我们发现在这个范围内参与度增加了。

5.人们知道你的追随者；你的粉丝越多，潜在参与度就越高。用户可以将您视为同事或影响者。

6.我们发现在太平洋时间晚上 10:00-凌晨 1:00 之间有更好的参与度。也许人们在晚上寻求陪伴。清晨的推文也会在上班前到达用户。

[](https://www.getyarn.io/yarn-clip/2dd3d6ba-52bf-4cef-b236-3fc43359f809) [## 他们怎么还用手摘呢？

### 蓝色夏威夷(1961)——Yarn 是通过引用找到视频剪辑的最佳方式。在电视节目、电影或…

www.getyarn.io](https://www.getyarn.io/yarn-clip/2dd3d6ba-52bf-4cef-b236-3fc43359f809) 

## 实验二:推特机器人

我们是一家资源有限的小公司，但拥有无限的创意和技术人才；)我们试图通过创建 twitter 机器人来自动化我们的第一个实验。我们的机器人与吉米·法伦(@jimmyfallon)和艾伦·德杰尼勒斯(@TheEllenShow)的推特互动。机器人监控他们的信息流，发现新的推文。该机器人分析了推文，并使用纱线生成了一个响应。在对机器人进行初始编码后，监控 twitter 账户的设置花费了大约 30 分钟，大致如下:

```
twitterUtil.createWatcher('yarnds', '[https://www.getyarn.io](https://www.getyarn.io/)', {
  skipTweet: false,
  useTextMp4: true,
  pollTime: 24000,
  apiLink: 'statuses/user_timeline',
  params: {
    screen_name: 'jimmyfallon',
    count: 1}
});twitterUtil.createWatcher('yarnds', '[https://www.getyarn.io](https://www.getyarn.io/)', {
  skipTweet: false,
  useTextMp4: true,
  pollTime: 24000,
  apiLink: 'statuses/user_timeline',
  params: {
    screen_name: 'TheEllenShow',
    count: 1}
});
```

我们进行了一个月的实验，回复了 460 条推文。偶尔回应是无意义的；解析一条 tweet，生成回复候选，搜索 YARN，最终选择单个 YARN，每一步都有级联问题。但是，嘿，这是自动的。

以下是一些有趣的回答:

@jimmyfallon

[https://twitter.com/yarnds/status/745592676481826816](https://twitter.com/yarnds/status/745592676481826816)

@TheEllenShow

[https://twitter.com/yarnds/status/744227367334215681](https://twitter.com/yarnds/status/744227367334215681)

## 实验 2: Tweetbots 结果= 50 倍的改进

```
Type     Impressions Engagement/tweet Engagement/imp Cost/engagement       Replies  73,900      60.78%           1.73%          $0.000021
```

自动化肯定是值得的，因为成本很低。尽管我们的最佳手动参与率更高(2.65%对 1.73%)，但它更难扩展。我们的机器人可以以更低的成本(设置和监控成本)进行扩展，并使每次项目的成本降低 50 倍。由于一次性的前期安装成本，我们的成本会随着时间的推移而下降。

通过更多的努力，可以产生更好的机器人响应。通过界定内容搜索的范围，这些响应可以更好地针对 Ellen 和 Jimmy 的特定受众。例如，使用节目嘉宾的内容，如塞斯·罗根、弗莱德·阿米森、基特·哈灵顿(本周《今夜秀》的嘉宾)。

[](https://www.getyarn.io/yarn-clip/aa8eb344-3607-4b16-9f09-c3f4f05c5148) [## 这两者结合得相当好。

### 《权力的游戏》(2011) -第六季第十集- Yarn 是通过引用找到视频剪辑的最佳方法。找到电视节目中的确切时刻…

www.getyarn.io](https://www.getyarn.io/yarn-clip/aa8eb344-3607-4b16-9f09-c3f4f05c5148) 

## 实验三:Pewdiepie 机器人

[菲利克斯·谢尔贝格](https://en.wikipedia.org/wiki/PewDiePie)又名[派迪派](https://www.youtube.com/user/PewDiePie)又名[@派迪派](https://twitter.com/pewdiepie)是 YouTube 上最受欢迎的频道之一，拥有 4500 万订户和超过 100 亿的视频浏览量。

我们试图通过使用他自己的内容来围绕他的 twitter 账户和他的追随者建立互动。我们摄取并分析了大约 50 个视频，大约 9 个小时的视频。我们制作了 11，500 个纱夹。我们的纱线定制网站是:[http://pewdiepie.getyarn.io/](http://pewdiepie.getyarn.io/)。Bot 安装花费了 30 分钟:

```
twitterUtil.createWatcher(‘pewdyarn’,‘[https://pewdiepie.getyarn.io](https://pewdiepie.getyarn.io/)', {
  skipTweet: false,
  useTextMp4: true,
  pollTime: 12000,
  apiLink: ‘statuses/user_timeline’,
  params: {
    screen_name: ‘pewdiepie’,
    count: 1}
});
```

费利克斯大约每天向 580 万粉丝发一条推文，他的推文获得大约 1000 到 5000 次转发。我们配置了一个机器人( [@pewdyarn](https://twitter.com/pewdyarn) )来跟踪他的流并找到新的推文。该机器人抓取了一条推文，分析了其结构，并使用自己视频中的一个故事做出了回应。这个过程在几毫秒内执行，我们每 12 秒检查一次他的账户。

一个例子是:

[](http://pewdiepie.getyarn.io/yarn-clip/abe59524-15e0-4371-b7cb-c49089787b22) [## 去你妈的光明会！

### PewDiePie 动机游戏-纱是最好的方式找到视频剪辑的报价。找到电视节目中的确切时刻…

pewdiepie.getyarn.io](http://pewdiepie.getyarn.io/yarn-clip/abe59524-15e0-4371-b7cb-c49089787b22) 

我们这样做了一个月，发出了 360 条推文，50 万次展示，1000 次链接点击，768 次赞

[https://twitter.com/pewdyarn/status/728885205423919105](https://twitter.com/pewdyarn/status/728885205423919105)

```
Type     Impressions Engagement/tweet Engagement/imp Cost/engagement       Replies  489,000     97%              8.9%          $0.0000006244
```

## 实验 3: Pewdiepie Bot = 1，750 倍改进

我们发现，与之前的机器人相比，性能提高了 35 倍，与手动推文相比，性能提高了 1，750 倍。

1.  我们太快了，在一些情况下，我们第一个回复了他的推文。许多人认为我们实际上是菲利克斯；许多人认为我们是机器人。
2.  这很有效，因为对他自己的观众来说，这是他自己的内容。
3.  我们有很多印象，因为这么多人查看他的推文，并且在一个线程的顶部有所帮助。

[](http://www.getyarn.io/yarn-clip/349c6fbd-636d-4c12-89b0-6991c6c10e22) [## 扩大规模，推出产品，上市，

### 硅谷(2014) -第二季第二集失控贬值- Yarn 是通过引用找到视频剪辑的最佳方式。找到确切的…

www.getyarn.io](http://www.getyarn.io/yarn-clip/349c6fbd-636d-4c12-89b0-6991c6c10e22) 

# 你该怎么办？

根据上下文回应用户。理想情况下，找到一种方法来制作一条你可以自动化的信息，传达给那些已经在讨论你的信息的人。个性化和情境化的消息传递显著提高了参与度。

使用纱线进行接合。它们可以在脸书和 Twitter 上自动扩展成显卡，以增加帖子的参与度。视觉获得更多关注；视频更多。一根纱线的短尺寸在实时社交平台上非常好用。您的信息更加清晰可见，传达了额外的语气，并且大小完全适合移动带宽。奇谈怪论可以在电子邮件中发送[，在空闲时间](https://mixmax.com/developer/yarn)发送[，在](https://slack.com/apps/A06MBNBND-yarn) [iOS](https://itunes.apple.com/us/app/yarn-for-messenger-find-clips/id1047797281?mt=8) 或 [Android](https://play.google.com/store/apps/details?id=io.getyarn.fbmessenger) 上发送 FB messenger，以及任何你可以链接网址的地方。

在 [YARN](http://getyarn.io/yarn-popular) 我们摄取并分析视频内容，以便在对话中使用。我们很乐意为您的内容或社交活动提供帮助。如有任何问题，请联系我们。

我是杰弗里·克劳斯，@geofree，YARN 的创始人。我们是 Y Combinator 的研究员(F3)，致力于让数字对话变得更有趣，更吸引人。