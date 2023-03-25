# 《权力的游戏》回顾

> 原文：<https://towardsdatascience.com/a-look-over-game-of-thrones-fdcb887e0631?source=collection_archive---------5----------------------->

《权力的游戏》被公认为是这十年来最好的惊险、动作、战略、奇幻电视剧。该片于 2011 年上映，在很短的时间内就获得了认可，全球平均收视率达到 2570 万。这部剧是根据乔治·R·R·马丁写的系列丛书*【冰与火之歌】*改编的。

*如果你还没看这部剧或读过这本书，下一部分会有剧透提醒*

# 关于这个节目—

该剧展示了整个系列中的许多“家族”，但主要集中在其中的四个——坦格利安、史塔克、兰尼斯特和拜拉席恩。它讲述了四大家族如何不择手段地试图获得铁王座。

*“字面上任何可能的意思”*

故事从劳勃·拜拉席恩坐在铁王座上开始，奈德·史塔克是国王之手，瑟曦·兰尼斯特是他的妻子，丹妮莉丝·坦格利安和她的哥哥流亡海外。在这一系列的某个时候，劳勃去世，他的儿子乔佛里成为下一任国王。他的第一个命令是砍掉内德的头，他实际上是罗伯特的好朋友和助手。与此同时，丹的哥哥也被多斯拉克人杀死，她成了多斯拉克人卓戈·卡奥的妻子。在展览的后期，一系列事件发生了(按顺序)

1.  内德的儿子罗柏·史塔克试图为父亲报仇，在一次晚宴上被他认为是自己忠诚的人杀死。
2.  乔佛里在他叔叔提利昂的婚礼上喝啤酒时中毒身亡。
3.  提利昂得知他最爱的女人实际上是被他雇佣并逃跑后，杀死了他的父亲泰温。
4.  琼恩·雪诺死了，并再次由梅丽珊卓转世。(他什么都不知道)。
5.  坦格利安家族的丹妮莉丝，她名字中的第一个，未燃烧者，安达尔人、洛依纳人和先民的女王，弥林的女王，大草海的卡丽熙，王国的守护者，七大王国的女统治者，锁链的破坏者和龙之母。
6.  琼恩·雪诺现在是北境之王，瑟曦是铁王座和七大王国的女王，丹妮莉丝现在在维斯特洛的龙石岛。白鬼要来维斯特洛了，而且
7.  冬天来了…

# 关于数据集—

Kaggle 基于书籍中的信息提供了来自三个数据源的数据集。

*   首先是 **battles.csv** 包含克里斯·阿尔邦的《五王之战》数据集，可以在这里找到:[https://github.com/chrisalbon/war_of_the_five_kings_dataset](https://github.com/chrisalbon/war_of_the_five_kings_dataset)。这是一个伟大的系列中所有战斗的集合。
*   其次，我们有来自艾琳·皮尔斯和本·卡勒的**角色-死亡。这个数据集是作为他们的贝叶斯生存分析的一部分创建的，可以在这里找到:[http://allendowney . blogspot . com/2015/03/Bayesian-Survival-Analysis-for-game-of . html](http://allendowney.blogspot.com/2015/03/bayesian-survival-analysis-for-game-of.html)**
*   最后，我们有了一个更全面的人物数据集和**人物预测。这来自冰与数据之歌的团队，他们是从[http://awoiaf.westeros.org/](http://awoiaf.westeros.org/)刮来的。还包括他们对哪个角色会死的预测，其方法论可以在这里找到:[https://got . show/machine-learning-algorithm-predicts-death-game-of-thrones](https://got.show/machine-learning-algorithm-predicts-death-game-of-thrones)**

在这些数据中，你能找到哪些关于这个奇幻世界复杂政治格局的洞见？

# 接近—

正如前面提到的，我们需要在数据集内部找到洞察力，基本上我们需要对它进行 EDA。关于这篇文章，我们将分析 **battles.csv** 数据集，在后续文章中，我们将尝试获得尽可能多的见解(以及其他数据集)。

我在这里问的问题是——哪一个国王赢得了最多的战斗，用了多少人？

我们将首先尝试绘制一些当前特征的图表，然后尝试查看它们是否显示任何有用的信息。

# 笔记本—

要获得带有适当解释、输出和数据集文件的完整代码，请参考我的 GitHub 帐户—

[](https://github.com/saahil1292/Game-Of-Thrones) [## saahil 1292/权力的游戏

### 在 GitHub 上创建帐户，为《权力的游戏》的开发做出贡献。

github.com](https://github.com/saahil1292/Game-Of-Thrones) 

# 参考文献—

*   [http://quizfactory.com/simple/got2/desktop/top.jpg](http://quizfactory.com/simple/got2/desktop/top.jpg)
*   [https://www . quora . com/How-much-people-watch-Game-of-Thrones/answer/Tini-Bancroft？srid=MrHW](https://www.quora.com/How-many-people-watch-Game-of-Thrones/answer/Tini-Bancroft?srid=MrHW)
*   [https://www . quora . com/What-the-full-title-of-daene rys-Targa ryen/answer/Mahesh-Shrestha-23？srid=MrHW](https://www.quora.com/What-is-the-full-title-of-Daenerys-Targaryen/answer/Mahesh-Shrestha-23?srid=MrHW)