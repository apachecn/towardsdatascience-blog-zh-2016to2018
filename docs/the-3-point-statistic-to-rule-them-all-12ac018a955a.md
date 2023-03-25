# 统治他们的三分统计

> 原文：<https://towardsdatascience.com/the-3-point-statistic-to-rule-them-all-12ac018a955a?source=collection_archive---------2----------------------->

**引入 3NG**

![](img/5422b341ed78ba3f8a4fb584b6217953.png)

Image by [patgui](https://pixabay.com/en/basketball-hoop-basket-basketball-1270365/). [CC0 Creative Commons License](https://creativecommons.org/publicdomain/zero/1.0/deed.en).

## 哗哗声

“嘣宝宝！”

90 年代的印第安纳步行者队球迷都知道这个戒指。斯利克·伦纳德叫了一声，这意味着雷吉·米勒刚刚投进了一个三分球。每次发生的时候我都有一种冲动。

20 年后，住在湾区，我一直对斯蒂芬库里(Steph Curry)的崛起心存敬畏。他的三分球是如此鼓舞人心，它彻底改变了比赛。

三分球投篮经常被两个统计数据分析，三分球命中率。大多数球迷认为最好的三分射手在这两方面都很出色。

然而，还没有一个统计数据来对最佳三分射手进行排名。三分球使得像安东尼·沃克这样的射手得到回报，尽管他的命中率只有 36.7%，但他在 2001 年仍然高居联盟榜首。根据三分球命中率，史蒂夫·科尔是历史上最好的，但他的投篮命中率不高。

有没有一种客观的方法可以将三分球命中率和三分球命中率合并成一个重要的数据？它会透露斯蒂芬库里是有史以来最伟大的三分射手吗？历史上最好的三分球球队呢？

## 3NG: 3 点净收益

每一次在球场上，球队都被期望得到一定数量的分数。自从 1980 年引入三分球以来，NBA 平均每球得分约为 1.06 分。

NBA 球员每投中一个 3 分球，他的球队就获得超出期望值的 1.94 分。球员每投失一个 3 分球，他的球队就失去了 1.06 分的期望值。

3NG 的计算公式如下:

让 3P =三分球命中。

让 3Mi = 3 分不中。

设 EV =期望值，每次控球的期望得分。

那么 3NG = 3P * (3 - EV) - 3Mi * EV。

用 1.06 代替 EV(仅 NBA)，我们得到:

3 ng = 3P * 1.94-3 米* 1.06。

换句话说，3NG 是三分球命中率乘以每次投篮的净收益与三分球命中率乘以期望值之间的差值。

实际上，3NG 传达了球员或球队通过投中三分球而获得的超出联盟平均水平的净得分。球员投中三分球会得到奖励，投失会受到惩罚。3NG 可以通过比赛、赛季或整个职业生涯来计算。

让我们检查数据，看看 3NG 如何满足预期。我的例子是基于从 basketballreference.com T2 争论来的数据。

作为开始，让我们从 2018 年开始考虑联盟领袖。

## **3NG 联赛领跑者 2018**

3NG is per-game unless otherwise stated. For all tables EV =1.0644871794871795, the NBA points per possession mean since 1980.

根据 3NG 的数据，2018 年勇士队的场均得分超过联盟平均水平 2.03 分，斯蒂芬·库里投了 3 分。他们还从克莱·汤普森获得了 1.84 分，从凯文·杜兰特获得了 1.16 分。前十名中的每个人都超过了百分之四十。

3NG 的妙处在于可以跨赛季跨代比较玩家。想想有史以来的十大赛季。

## **3NG 史上最佳赛季**

3NG can be weighted by season. I prefer unweighted because it provides an even baseline of comparison.

![](img/162d5fad08b863673f528a75853d8998.png)

[Steph Curry “Sky High” Wallpaper](https://www.deviantart.com/ishaanmishra/art/Steph-Curry-Wallpaper-371933600) by [IshaanMishra](https://www.deviantart.com/ishaanmishra). [Creative Commons 3.0 License](https://creativecommons.org/licenses/by-nc-nd/3.0/).

斯蒂芬·库里惊人地占据了前六名中的四席。斯蒂芬一致获得 2016 年 MVP，甚至超过了他之前的最好成绩。凯尔·科沃尔 2015 年的三分球命中率达到了可笑的 49.2%，稳居第二。

1997 年的格伦·莱斯有点另类。谁是 90 年代最好的三分射手？

## **3 ng 90 年代最佳**

所有的球员都是 90 年代后半期的。比较一下雷吉·米勒和史蒂夫·科尔，分别排在第六和第七位。雷吉投中了更多的三分球，但史蒂夫·科尔的命中率更高。谁是更好的三分射手？3NG 提供了一个有统计意义的答案。

我们也可以用 3NG 来对比球队，像 2018 年的火箭和勇士。2018 年的火箭队打破了一个赛季最多三分球的 NBA 纪录。他们带领勇士队，一支传奇的三分球球队，在西部决赛中打了七场比赛。常规赛谁的三分球投得更好？

## 3NG 勇士队对火箭队 2018，仅限合格球员

Negative 3NG scores mean that the team is scoring less than the league average by the player shooting 3's.

勇士统治着顶端，但火箭拥有中间。最有价值球员詹姆斯·哈登落后于 66 名球员，而安德烈·伊戈达拉和德雷蒙德·格林的贡献为负。

将上表中的 3 分加起来，就可以看出哪支球队在全力投 3 分时净胜分更多。

GSW: 4.32

HOU: 1.95

另一种选择是通过对整个赛季的三分球求和来计算 3NG。这考虑到了交易，伤病和不合格的球员。总结结果如下。

GSW: 3.12

HOU: 0.91

2018 年的勇士队是超级三分球球队，在这两个方面都超过了火箭队一倍。他们在历史上是如何排名的？

## 空前的 3 支顶级球队

The 2018 Warriors would rank #2 by summing qualified players only. Injuries played a big role in 2018.

勇士队和 7 秒或更少的太阳队以 8 个位置统治着这个名单。1997 年的夏洛特黄蜂队是一个惊喜，直到人们意识到他们有格伦·莱斯和德尔库里。前十名包括两个 NBA 冠军和七个联盟决赛选手。勇士队破纪录的 73 胜赛季是一个令人印象深刻的异数。

为了确定最佳三分射手，我们可以将 3NG 相加，然后除以赛季数。

## 3NG 职业平均值

I required a minimum of 5 NBA seasons.

除了队友克莱·汤普森和凯尔·科沃尔，斯蒂芬·库里比名单上的其他人都强。这个列表回答了一个我们还没有提出的问题。是什么让勇士如此有统治力？他们有历史上最好的两个三分射手。

同代人中最令人兴奋的三分射手雷吉·米勒在哪里？他打了 18 个赛季，在 NBA 历史上，除了雷·阿伦，他投中的三分球比任何人都多。我们可以找到雷吉，以及其他伟大的历史射手，在 3 个职业生涯总数。

![](img/bca15bc79dab303dd401364be484fdc1.png)

[Reggie Miller Pacers Wallpaper](https://www.deviantart.com/ishaanmishra/art/Reggie-Miller-Pacers-Wallpaper-266683674) by [IshaanMishra](https://www.deviantart.com/ishaanmishra). [Creative Commons 3.0 License](https://creativecommons.org/licenses/by-nc-nd/3.0/).

## 3NG 职业生涯总计

这是一份漂亮的清单。

## 游戏结束

3 个点的净收益，或者说 3NG，已经超出了我的预期。它在历史上积累得很好，准确地对当代射手和球队进行了排名。3NG 是一个有意义的、可验证的统计数据，传达了有价值的信息

3NG 可以申请任何联赛，WNBA，大学，高中等。用适当的期望值作为比较的基线。它可以被教练、球探和球员用来做出重要的决定。球迷可以使用 3NG 来确定谁是最好的 3 分射手。

鼓励发行 3NG，前提是注明作者 Corey J Wade。[](http://coreyjwade.com)

**见*[*https://github.com/coreyjwade/New_NBA_3-point_Stats*](https://github.com/coreyjwade/New_NBA_3-point_Stats)*关于熊猫的数据分析和附加的 3 点统计。**