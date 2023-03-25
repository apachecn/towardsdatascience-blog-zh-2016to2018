# 音乐定位:品牌的新 Eldorado？

> 原文：<https://towardsdatascience.com/music-targeting-the-new-eldorado-for-brands-48590a0b7108?source=collection_archive---------8----------------------->

虽然关于如何收集和使用私人数据的争议正在激烈进行，虽然《GDPR》的应用正在临近，但替代的数据管理方法非常受欢迎。充分理解这些问题的新演员正在广告业涌现，并带来创新的解决方案来满足广告商的需求。他们的方法与通常使用的方法截然不同，本质上是基于数据科学，分析从未被利用过的数据。

# **“原始数据”，有史以来最大的黑金矿床，却几乎无人问津。**

今天，广告市场是围绕用户的个人数据组织的。大多数情况下，这些数据是通过数据交换平台收集和出售的，在这些平台上，人们可以找到关于特定用户的几乎任何类型的信息，或者更具体地说，关于广告 ID 的信息(IDFA、AAID、cookies 等等)。人们可以访问用户的所有个人数据，如年龄、性别、婚姻状况、购买力等。鉴于数据量巨大，获取这些信息的成本已经低得离谱:每 1000 条信息的购买价格从 0.10 美元到 5 美元不等，换句话说，每条信息从 0.0001 美元到 0.05 美元不等。此外，这些平台中的绝大多数明显缺乏透明度，这使得追踪这些数据的来源成为不可能，并引发了隐私和用户同意的担忧:这些数据是如何收集的？用户同意了吗？

一方面，2018 年 5 月 GDPR 的引入应有助于清理这些不可接受的做法，并规范数据的收集、处理和交易。另一方面，苹果应用商店已经在优步的[最新滥用或者 Teemo 丑闻之后做出了回应。例如，考虑到即将发布的 iOS 11，苹果更新了指导方针。根据第](https://www.nytimes.com/2017/04/23/technology/travis-kalanick-pushes-uber-and-himself-to-the-precipice.html)[5 . 1 . 1 和 5.1.2](https://developer.apple.com/app-store/review/guidelines/#legal) 条，公司将不再被授权收集数据，除非该应用的主要功能之一的性能需要。此外，禁止将这些数据传输给第三方或广告公司。甚至关于网络上的广告定位，苹果已经更新了 Safari，使得未经用户同意几乎不可能通过 cookies 进行跟踪。

# **“一流数据”的终结？**

有了这些限制，品牌如何成功地锁定他们的受众？我们需要立即为这一重大变化做好准备，并开始寻找符合法规的新解决方案。

还有另一种不太为人所知的数据:我喜欢称之为“原始数据”。这类数据就像石油一样:如果不首先处理和提炼，就完全无法使用。我在这里指的是关于一个人日常应用使用的数据，一旦经过分析，就可以非常准确地描述用户的行为和偏好:例如，每天走的步数，安装的应用数量，甚至每天听的音乐类型。

这些数据是由用户生成的，数量巨大，但实际上毫无用处。数据科学提供了新的视角，使得提取用户的行为和习惯成为可能，以便实际绘制他们的品味、偏好和消费模式。

让我们以 SportHeroes 为例，这是一家欧洲初创公司，开发促进和鼓励体育活动的应用程序。这家公司围绕用户的体育活动数据开发了一项业务:他们每天走多少步？他们步行或跑步的距离是多少？他们一周跑几次？多久了？这些信息一旦经过处理，SportHeroes 就能极其精确地理解用户的运动行为。这实际上是耐克(Nike)或阿迪达斯(Adidas)等运动品牌的一个千载难逢的机会，它们可以通过在 SportHeroes network 上开展广告活动，接触到这些充满潜在买家的受众。这种定位不是基于人们是否宣称喜欢体育运动(社交网络上的声明性数据)，而是基于人们实际从事体育运动的方式(SportHeroes 跟踪的行为数据)，这显然是一个对体育品牌更强大、更相关的信号。

另一个例子——也许是最引人注目的——是音乐。你不认为你喜欢的音乐反映了你是谁吗？你不是倾向于根据一个人听的音乐风格马上对他/她形成看法吗？

# **音乐定位:一场革命。**

几十年来，来自世界各地的研究人员一直试图了解音乐对人类的影响。新发现不断涌现:每个人都知道音乐的治疗作用。事实上，俄勒冈大学最近展示了其在阿尔茨海默病治疗中的有效性。来自宾夕法尼亚州乌尔西努斯学院的南希·贝克尔和她的同事强调了音乐在体力活动中的掺杂剂效应，表现惊人地提高了 30%。最后但并非最不重要的一点是，事实证明，音乐在我们的购买行为中扮演着重要角色，甚至可以显著增加商店销售额。美国 Hollister 公司充分理解音乐的这一作用:该服装品牌在其所有商店中将其作为一种强有力的营销工具，以优化顾客流量和促进销售。

但该领域的最新进展之一来自大数据。从今以后，大数据使得通过音乐解读用户的行为和习惯成为可能。

如果没有音乐产业的数字化和音乐流媒体的民主化，这一突破是不可能实现的。由于后者，现在有可能收集数量惊人的收听数据，使品牌能够分析数百万人每天是如何与音乐互动的。

某些流媒体巨头已经开始注意到，他们用户的音乐行为可能是一个额外的、甚至是重要的收入来源。如今，Spotify 销售基于播放列表的定向活动，该公司在短短 3 年内成功创造了近 3 亿美元的广告收入([来源:全球音乐商业](https://www.musicbusinessworldwide.com/spotify-revenue-hit-3-3bn-in-2016-but-net-loss-soared-to-597m/))。这完美地说明了这些基于“原始数据”的新目标工具的效率及其几乎未开发的潜力。

![](img/eaf07bbe1d8015cb673cb33021861e6c.png)

不仅流媒体服务对音乐数据感兴趣，广告公司也是如此。哈瓦斯公关北美公司首席执行官 Maria Salzman 甚至向[《卫报》](https://www.theguardian.com/media-network/2015/oct/08/linking-music-consumer-behaviour-marketers) : *“在未来十年内，播放列表将成为有史以来最强大的消费者行为预测工具”*。然而，我们才刚刚开始探索原始数据的潜力，今天，Spotify 提供了一个简单的目标，仅限于其 7000 万免费客户的受众。

这场代表着通过音乐进行行为定位的革命，远远超出了俘虏听众的货币化，而是在音乐流媒体和许可活动之外建立了一个额外的收入流。它代表了一种全新的数字营销方式，更加感性和不断变化，让品牌真正了解自己与社区的亲和力。这种方法利用了与最普遍的行为之一——听音乐——相关的不断更新的大量数据。

部署这种革命性的方法是 MWM 的雄心和专业知识的核心。短短几年间，全球八大移动音乐应用发行商之一、下载量超过 1 亿次的 MWM 已经成为“音乐数据科学”的先驱。我们积累的数十亿数据使我们能够通过音乐对任何类型的行为进行深入分析。

例如，我们最近的研究表明，一个人对电子游戏的兴趣与你正在听的音乐风格密切相关。通过对 15 万用户进行抽样调查，并通过算法分析他们的行为，我们可以断言，听摇滚和另类摇滚(声音花园乐队、杀手、法兰兹·费迪南、帕帕·罗奇和黑键等乐队)的人对电子游戏感兴趣的可能性是听流行音乐的人(Lady Gaga、玛丽亚·凯莉、卢安、席琳·狄翁、鲍勃·辛克莱等艺术家)的三倍。更一般地说，只要知道他们的音乐习惯，我们就能够明确地识别出一群有超过 95%的机会成为视频游戏爱好者的用户。

![](img/c76b26e1db524344f2d0519ec4ee44b3.png)

这只是一个例子。我们的数据科学家每天都在构建新的模型，使我们能够探索音乐数据分析的非凡可能性。在我们的“MWM 网络”平台内，品牌现在可以从这些发展中受益，以便以一种新的和创新的方式改变他们的混合媒体。

# **结论**

由于智能手机的开发和“原始数据”的出现，广告定位领域正在发生一场革命。它为品牌提供了一个全新的平台，同时加入了*以人为本的营销*运动，并尊重即将到来的新法规…