# 比赛日会提升世界杯歌曲吗？

> 原文：<https://towardsdatascience.com/do-match-days-boost-the-fifa-world-cup-soundtrack-d75d7ef785c1?source=collection_archive---------8----------------------->

![](img/4a862a5f515bc5848204bcc957d6a991.png)

音乐是足球文化的一部分。国际足联官方歌曲和当地主题曲将所有参加今年世界杯的国家团结在一起。除了提升比赛期间的气氛，这项活动还让艺术家们获得了非同寻常的世界报道和名声。我们都记得像夏奇拉的《T0》、《Waka Waka》、《T1》和皮特保罗的《T2》、《我们是一家》、《T3》这样的热门歌曲，尽管它们分别是在 8 年前和 4 年前发行的。

由于这项活动的巨大影响力和音乐在支持者中的统一性，我想知道我们是否可以追踪本届世界杯国歌的结果，看看这些是否会在比赛日增加——如果是这样，这是否因国家而异？

在俄罗斯 2018 年世界杯期间，官方国歌将由尼基·贾姆、威尔·史密斯和 Era Istrefi 代表，名为[‘尽情狂欢’](https://open.spotify.com/track/4DnHUy01jEA4b5ydC3HgsT?si=ej3TthGUSFGLrt-3YczT_A)。出于这一分析的目的，我们将跟踪这首歌的表现，以衡量事件的影响。

## 数据集

对收集的数据进行简短描述，以便进行实际分析；

*   由于我没有该曲目的实际每日流媒体数据(这是不公开的)，我决定偏离 Spotify 排行榜，寻找有排行榜的国家。日线图是在曲目发行日到淘汰赛第一轮之间刮出来的。
*   我们需要所有比赛的时间表，以便与 Spotify 的每日图表相匹配。这些都可以通过国际足联网站获得。

## 比赛日会导致更高的图表位置吗？

当我们比较参赛国家的每日图表位置，并将它们与各个国家比赛的日子进行比较时，我们可以确定在比赛日是否出现增长。当综合数据集中的所有国家并计算平均值时，我们得出结论**在比赛日**增加了 12 个位置。

为了让它更有趣，我在下面突出了几个国家，图表显示了当特定国家有世界杯比赛时，图表位置的增加。

看看瑞典，当他们的国家队比赛时，我们看到平均增加了 25 个位置。在 16 强决赛对阵瑞士的那天，这首歌在 Spotify 每日排行榜上从第 49 位上升到第 8 位。

![](img/28ed0c9cdeb8ff38170faa6bfa38c202.png)

Chart increase for Sweden

当所谓的*红魔*登台时，比利时也显示出图表位置的显著增加。同时，似乎在非比赛日有更多的高峰。

![](img/879d003ee329ca9efadf2f10d83e1ec8.png)

Chart increase for Belgium

冰岛的支持者总是令人印象深刻，在体育馆里制造威胁的声音。他们的比赛日平均涨幅是 30 个位置。

![](img/4659d12b615fe37ead9071d1ce877b7e.png)

Chart increase for Iceland — period between May 25th and June 15th means no chart inclusion

当看法国时，我们也可以得出结论，赛道往往会在比赛日爬上图表，除了对秘鲁的比赛，赛道在法国图表中下降，但在第二天增加。

![](img/179cba73338bbfd5d17a4218d0dd0492.png)

Chart increase for France — period between May 25th and June 12th means no chart inclusion

## 淘汰

当研究这个问题时，我首先想到的事情之一是，被淘汰出局的国家是否也会停止聆听世界杯官方国歌。当看着最受欢迎的参赛者之一巴西时，我们看到了一些值得注意的事情——这首歌在 6 月 19 日从他们的 Spotify 排行榜上消失了，而他们直到 7 月 6 日仍在争夺世界杯。巴西在这条赛道上的最高排名是第 105 位，与其他(周边)国家相比，这个排名相当低。

对于其他一些被淘汰的国家(可能比预期的要早)，我们不一定会看到图表的大幅下降。像德国、阿根廷和墨西哥这样的国家在不得不离开俄罗斯后，头寸略有减少，但在失望情绪消失后的几天里，头寸仍然很大，甚至有增加的趋势。

相反，冰岛似乎已经厌倦了“尽情享受”，因为在他们输给克罗地亚两天后，我们不会在他们的本地 Spotify 排行榜上找到这首歌。

![](img/bbaf8a2c9731e6ef1ddd2b7aea120dea.png)

Chart positions after being knocked out

## 英国更喜欢其他方式

虽然原声音乐出现在一些当地的 Spotify 排行榜上，但它没有能够到达英国，甚至在比赛日也没有。英国人似乎更关心他们自己的地方国歌 [*三狮*](https://open.spotify.com/track/1b7kLkJkr6XRYgWqVmDm05?si=8zYwtFhNSo2q6r57JUkHrQ)*s*(1996 年发行)*，*迅速攀升至 Spotify 排行榜，并自今日起占据榜首。

应用与我们对 FIFA 原声所做的相同的方法，我们看到在比赛日平均增加了 118 个位置(只看原声，有多个版本)。

![](img/de9ae529953057045042bd4e8f7c2559.png)

Chart increase for U.K. — looking at the Three Lions anthem

英格兰跟着他们的“三狮军团”国歌一起唱，我并不感到惊讶，因为我们在荷兰已经习惯了跟着唱像[“Viva Hollandia”](https://open.spotify.com/track/7MFkgZDyAoMO2JNlZXbOAh?si=ryfIi8h9TZ6JZHE93hCxWw)和[“Wij Houden Van Oranje”](https://open.spotify.com/track/0TWJWAmDfD5z5jtM4tqOy6?si=TKSRIz6CTpy5aNjHd31zLA)这样的歌曲。虽然我不需要检查这些歌曲，这也可以解释为什么我有一些空闲时间进行这样的分析。；-)

## 放弃

*   这将是最准确的时候，看看每个国家的实际流计数，不管图表是否包含。因为这些都不是公开的，所以我选择使用 spotifycharts.com 的网站。很有可能在比赛日发生的其他活动会导致现金流上升，从而导致图表位置上升。
*   Spotify 是唯一被考虑的流媒体服务。鉴于该服务并非在所有参与国家都可用，因此一些国家被排除在此分析之外。
*   一些国家可能会有当地的国歌在比赛期间也有所提高，但这些没有被考虑在内。

**感谢阅读！如果你喜欢读这篇文章👏🏻很感激。**

我写的更多故事:

[](/the-diffusion-of-latin-and-reggaeton-69113f9929dd) [## 拉丁语和雷鬼音乐的传播

### 公平地说，我们很多人都在跟着当今最热门的拉丁和雷鬼音乐一起唱歌，而不一定…

towardsdatascience.com](/the-diffusion-of-latin-and-reggaeton-69113f9929dd) [](https://medium.com/@boplantinga/music-travels-but-what-s-the-travel-time-8fd7279e1aa6) [## 音乐的旅行时间是多少？⏱

### 我们生活在一个数字世界，因此信息和音乐的传播速度比以往任何时候都快。从来没有…

medium.com](https://medium.com/@boplantinga/music-travels-but-what-s-the-travel-time-8fd7279e1aa6) [](/does-spotifys-chart-on-sunday-differ-from-the-rest-of-the-week-d7272f300508) [## Spotify 周日的图表与本周其他时间有所不同吗？

### 看看我自己的倾听行为，我经常承认我在周日倾向于有不同的偏好…

towardsdatascience.com](/does-spotifys-chart-on-sunday-differ-from-the-rest-of-the-week-d7272f300508) [](/pusha-t-vs-drake-backing-up-their-diss-tracks-with-data-1083e6159c75) [## Pusha T vs. Drake:用数据备份他们的 diss 轨迹

### 也许没有必要花太多时间来介绍德雷克和普什塔之间的矛盾

towardsdatascience.com](/pusha-t-vs-drake-backing-up-their-diss-tracks-with-data-1083e6159c75)