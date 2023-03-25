# 举重数据和探索性数据分析第 1 部分

> 原文：<https://towardsdatascience.com/powerlifting-data-and-exploratory-data-analysis-part-1-6f21d79ac5db?source=collection_archive---------3----------------------->

几周前，我发现自己在互联网上搜索起重数据。我一直喜欢健身，我想知道是否有任何可行的数据集，可以量化我的一个爱好。然后我偶然发现打开了 Powerlifting 的力量举数据库。OpenPowerlifting 存储了全国各地 Powerlifting 会议上完成的 300，000 多台电梯的数据。我很快发现自己在搜索著名的举重运动员，如马克·贝尔、布兰登·莉莉、斯坦·埃弗丁和乔尼·坎迪托。找到那些大人物后，我也在数据集中搜索朋友*咳* AlanZhou *咳*。我对 OpenPowerlifting 收集的数据感到高兴和好奇。我决定，当我有时间时，我会重新访问数据集，看看是否有什么我可以做的。

![](img/727aecb1ad28cc04eeeb36b37df2cc03.png)

以下是 OpenPowerlifting 对他们的数据的看法:“OpenPowerlifting 项目旨在创建一个永久、准确、方便、可访问、开放的世界举重数据档案。为了支持这一使命，所有的 OpenPowerlifting 数据和代码都可以以有用的格式下载。没必要刮这个网站。”过去我花了几天时间浏览网站，破译写得很糟糕的 HTML 代码，很高兴在 OpenPowerlifting 的 Github 上找到了一个可随时下载的 CSV 文件。

其他数据工程师、数据科学家、软件开发人员和数据分析师喜欢举重，这让我欣喜若狂。OpenPowerlifting 背后的人和我有两个共同点，举重和数据科学。我决定把计时器打开半小时，看看我能做些什么数据。我实施这个时间限制仅仅是因为我可以看到自己在数据上花费了过多的时间，而且我还有其他更紧迫的截止日期要赶。

下表是三大电梯的简单分类。我把数据分成男性和女性两组。此外，我已经包括了总数和威尔克斯系数的平均值。

![](img/0a3423793f7874e22d9034ae9ba80f93.png)

现在，我要说的是，其中的一些测量对我来说似乎有点偏差。为了更好地理解分布情况，我绘制了直方图，并将它们包含在下面。力量举重的年龄分布特别有希望。我们看到大量年轻人加入这项运动并参与竞争。这可能是由于 YouTube 健身名人的受欢迎程度，如马克·贝尔、奥马尔·伊素福、沉默的麦克、杠铃旅和坎迪托训练总部。希望这一趋势在未来继续下去。

![](img/2e991ec2f909c297d6d22559d3dbdab5.png)![](img/dfe63ef964e87408f443aa03db6b4a82.png)![](img/d30f8e007d285d9cae994361e1a0611a.png)![](img/96dcb95a935c2d4fb2b1ac481a60ca3c.png)![](img/f685101562addafd4dfaebd673308c1f.png)![](img/5e053a2ce8b47d4d1b3a6ced3e62f031.png)![](img/a631d58e7f64f4c04499737845a5aa49.png)![](img/d696278db47513e402f6860f38df42f0.png)![](img/58c1d4f875b9f7c05fce7cd7cf159635.png)![](img/faaa045f208ab7457d5f48ddb4d31ac1.png)![](img/528c95c98bc50f754c96a7efff5c3e51.png)![](img/9e63d80c1495f582ae9735706954381f.png)![](img/97a51bc42bf0696b3d4586f093cbfba3.png)

观察男性和女性的散点图表明，体重和举重之间存在正相关。这是显而易见的。但是基于这些散点图，我相信与深蹲相比，体重增加一个单位增加一个人的板凳深度最少，硬拉增加最多。我将在本分析的第二部分尝试证明(或否定)这一假设。我把这个小项目分成两部分的原因是，在我的 30 分钟结束时，我只完成了很少的工作。我计划在第二部分做一些实际的机器学习，所以敬请关注！

![](img/c3c6ea19e5a1b0a0194cc8bb5447c18c.png)![](img/5e86f458c657fdb1cea41bb42977a6f8.png)![](img/a1dfbf2c06100572188bd0caf16a9292.png)![](img/288c3d9063bd9d8c3f88d73a46998b1a.png)![](img/89201adfeb4ca6be53f3b5c2847e4436.png)![](img/c1810d7716260018d2feba1882838c46.png)![](img/c913ca32b1e8e6e1a54728215ea38d0b.png)![](img/ccb6cd1a588d14bf0cbd4bff05cc953c.png)

一旦我完成分析，我会在 Github 上公开我的 Jupyter 笔记本。下面是打开力量举重的链接。

[http://www.openpowerlifting.org/data.html](http://www.openpowerlifting.org/data.html)

https://github.com/sstangl/openpowerlifting