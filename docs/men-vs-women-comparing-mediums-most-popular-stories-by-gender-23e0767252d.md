# 男人 vs 女人:媒体最受欢迎帖子的比较

> 原文：<https://towardsdatascience.com/men-vs-women-comparing-mediums-most-popular-stories-by-gender-23e0767252d?source=collection_archive---------6----------------------->

为了好玩，我从网上搜集了 1000 篇今年 3 月至 10 月写的《灵媒》最受欢迎的故事。我使用 Python 的 lxml 包从这些帖子中构建了一个数据集，其中的特征是作者、标签、阅读时间、回复数、点击数和域名。

从作者的名字，我用 R 的性别包确定了他或她的性别。当 R 说不清的时候，我通过查看个人资料图片或阅读帖子来寻找线索。给定数据集，我比较了男性和女性发表的帖子有何不同。

![](img/96883166b6ed40ee2b5c8c8b1de2a818.png)

**1。计数**

男性发表的文章是女性的 3 倍多。73.6%最受欢迎的帖子由男性撰写，22.6%由女性撰写，其余由某个团体或组织撰写，如中等职员。

**2。标签**

我取了女性和男性帖子中标签数量的比率，并在分子和分母中添加了一个常数 1.01。女性职位比男性职位更有可能是:

```
 ** Tag Ratio Fcount Mcount**
          Women  16.8     16      0
       The List  15.9     15      0
        Russian  10.9     10      0
 Fat Acceptance   6.9      6      0
  Body Positive   6.9      6      0
       Feminism   5.5     21      3
         Sexism   5.0      4      0
     Brain Body   5.0      4      0
           Rape   4.0      3      0
        Mueller   4.0      3      0
```

而男性的帖子更有可能被贴上:

```
 **Tag Ratio Mcount Fcount**
         UX Design  16.8     16      0
             Apple  15.0     44      2
          Business  13.2     52      3
            iPhone  12.9     12      0
      Productivity  11.8     82      6
  Entrepreneurship  10.2    121     11
 Self Driving Cars   9.9      9      0
   Web Development   9.0     53      5
    Product Design   9.0     17      1
 Augmented Reality   8.9      8      0
```

**3。鼓掌次数、响应次数和阅读时间**

女性的帖子平均获得的掌声更少。

但是她们的帖子比男性的回复更多。

这些差异在统计学上并不显著，但如果样本量更大，可能会显著。两组的平均阅读时间持平，都在 9 分钟左右。

**4。域名**

我还计算了男性和女性帖子的域名比例。偏向男性职位的领域往往是出版物。

```
 **Domain** **Ratio** **MCount FCount**
 journal.thriveglobal.com  21.9     43      1
   medium.com/the-mission  10.0     59      5
              uxdesign.cc   8.9      8      0
          thinkgrowth.org   8.9      8      0
           mondaynote.com   8.9      8      0
                rantt.com   8.9      8      0
               500ish.com   8.9      8      0
       medium.com/incerto   8.9      8      0
    betterhumans.coach.me   6.9      6      0
             uxplanet.org   6.5     12      1
```

而偏向女性帖子的域名更有可能是作者自己的媒体托管网站。

```
 **Domain** **Ratio** **FCount MCount**
                 medium.com/@Amy_Siskind  15.9     15      0
                  medium.com/the-hairpin   5.0      4      0
                medium.com/@thefatshadow   4.0      3      0
               medium.com/@girlziplocked   4.0      3      0
                             thelily.com   4.0      3      0
              medium.com/@caityjohnstone   4.0      3      0
              medium.com/behavior-design   3.0      2      0
 medium.com/conquering-corporate-america   3.0      2      0
             medium.com/code-for-america   3.0      2      0
                    medium.com/@krisgage   3.0      2      0
```

我的直觉是，女性在写博客、写自己、写个人经历和写政治时会采取更内省的方式。相比之下，男性倾向于关注最新的科技热潮或如何制造东西。