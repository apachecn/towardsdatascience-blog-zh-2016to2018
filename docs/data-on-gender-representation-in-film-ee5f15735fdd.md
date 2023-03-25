# 好莱坞还有女人问题吗？

> 原文：<https://towardsdatascience.com/data-on-gender-representation-in-film-ee5f15735fdd?source=collection_archive---------22----------------------->

## 电影中的性别表现数据

![](img/9e47b4ce36f2619ec2cde438e9861978.png)

# 来自维基百科的好莱坞数据

维基百科是电影数据的宝库。不幸的是，这很难大规模实现。幸运的是，python wptools 库允许我们获取所有这些有趣的维基百科信息框。

我写了一些脚本来收集和清理 2000 年至 2018 年好莱坞电影的所有维基百科数据。然后，我使用 python“ntlk”男性和女性姓名语料库，按性别对所有演员、制片人和导演进行分类。当然，这个过程中存在一些误差。电影中的许多人不在这些名单中(如“雷德利”斯科特)。此外，一些男女通用的名字如“Leslie”会导致分类错误。我花了很多时间尽可能地手工清理东西。我设法得到的数据表明，不到 10%的好莱坞演员/作家/制片人不能被归类为男性或女性。

通过对 90%的数据进行分类和修正，很容易就能了解好莱坞电影中性别表现的状况。正如你在下面看到的，这个行业还有很多工作要做。

# 2018 年的不平等

下表显示了 2018 年参与制作好莱坞电影的男女比例。在所有情况下(演员、导演、制片人、编剧和音乐)，男性都远远多于女性。

![](img/f4603cf0f8d011aed74f48a2dc0349ca.png)

即使我们慷慨地假设大部分未分类的名字都是女性，这种不平等仍然是明显的。

下面我绘制了 2018 年好莱坞电影中表现最佳的演员的条形图，以及所有演员的整体性别代表。从蓝色的海洋来看，很明显女性的代表性远远不足。这一点在编剧、导演、配乐领域尤为明显。

## 演员

![](img/14ee740c91262a39e0531f3fea0086b7.png)![](img/b2fa76ef8d9907ee0cae012f30737503.png)

## 董事

![](img/1d0c561dea247864348331302631d463.png)![](img/f30dc1bd7fbdfcdce444f94f7b5f44fa.png)

## 生产者

![](img/7211846cc4157a39a9d572007d712e4c.png)![](img/3a0195644fe00d4b1e88a09c7155338f.png)

## 作家

![](img/0412204f8d7ec961594f1c99995db553.png)![](img/21e25a21ad30cf71af2ad603dcfb0320.png)

# 音乐

![](img/a2b35bfdfde106054d4311a4e6d4e3b0.png)![](img/a28f33227cb005ecc85489208c9ee9d2.png)

# 趋势

回顾 2000 年的数据，可以看出女性在电影中的惨淡表现是否有所好转。下面我画出了电影中女性比例的趋势。

## 演员

![](img/70106e031a38ff6b3a54e234fb0b49a4.png)![](img/1ecda8ac3abab01f0fe25ee19fbd4609.png)

## 董事

![](img/de54e1f8a6cec0f86fd3eb341aa24d45.png)![](img/e292a0581915f8afd14fd3e625eb2343.png)

## 生产者

![](img/554be536cc96f9f64e4715e5375fa491.png)![](img/ccaafd13da7e01de6d4b859425a87534.png)

## 作家

![](img/44c508253f45f555fdd559be04102a1c.png)![](img/b09215017add1397564db9dc1cb97b8e.png)

## 音乐家

![](img/a00819abe127b6816cfc2890fa655739.png)![](img/746e1cb1c293d123b5a7af29842328f4.png)

虽然女性在表演、导演、制作和写作方面的代表性正在缓慢提高，但在女性为电影评分方面似乎没有进展。

# 结论

从以上数据可以明显看出，好莱坞的性别代表性仍然是一个主要问题。我个人认为，这一事实是美国国内票房收入增长乏力的部分原因。不到 15%的电影由女性编剧或导演。当这些电影不能代表一半的观众时，它们不赚钱有什么奇怪的吗？

# 笔记

本项目使用的所有代码和数据都可以在 https://github.com/taubergm/HollywoodGenderData[的 github 上获得](https://github.com/taubergm/HollywoodGenderData)

1—[https://pypi.org/project/wptools/](https://pypi.org/project/wptools/)

2—【http://www.nltk.org/howto/corpus.html 

3—【https://www.boxofficemojo.com/yearly/ 

有关这一趋势的其他确认，请参见:—[https://www . the guardian . com/film/2018/Jan/04/Hollywood-diversity-sees-no-improvement-in-2017-report-finds](https://www.theguardian.com/film/2018/jan/04/hollywood-diversity-sees-no-improvement-in-2017-report-finds)