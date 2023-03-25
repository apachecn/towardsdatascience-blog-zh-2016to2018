# 数据-产品-科学家-管理者

> 原文：<https://towardsdatascience.com/the-data-product-scientist-manager-469cc1d21f9?source=collection_archive---------8----------------------->

机器学习、人工智能、深度学习、数据科学有什么区别？近年来，围绕这些概念的巨大讨论使得它们似乎可以互换使用。

几个月前，我在 meetup 上做了一次演讲，从游戏学习算法的角度回顾了机器学习算法的历史。因为这个演讲不需要预先的知识，所以我先简单介绍了一下人工智能(AI)和机器学习(ML)

*   人工智能是制造机器的科学，模仿我们人类感知的智能行为
*   ML 是人工智能的一个分支，涉及从数据中学习“智能”的算法(而不是显式编码)
*   深度学习是 ML 中非常特殊的方法，它使用人工神经网络和海量数据

在准备演讲的时候，我也在努力参考数据科学(DS ),以帮助我的听众理解当前所有的宣传术语。但是 DS 是 AI 内部的学科吗？ML 内？这难道不仅仅是统计学的一个花哨名称吗？…我最后把它漏掉了。

![](img/1f2c57d431a7c1ecc009362987e28908.png)

几天前，我偶然发现了一集精彩的 ML 播客[会说话的机器](http://www.thetalkingmachines.com/)，尼尔·劳伦斯[做了一个观察](http://www.thetalkingmachines.com/blog/2017/7/14/getting-a-start-in-ml-and-applied-ai-at-facebook)，让我明白了一切。劳伦斯认为，数据挖掘产生于我们在这个时代收集的新数据流，这些数据流是由传感器和交互产生的海量数据，其使命是从中提取价值。换句话说——“在这里，我们有所有这些数据，我们能用它们做什么？”

# 寻找假说

这可能看起来像一个小的技术细节，但它使所有的差异。在经典的实验中，科学家会提出一个假设，收集数据来验证或否定它，然后进行必要的统计分析。对于 DS，没有这样的先验假设。所以 DS 的核心变成了**提出这些假设**，然后在我们已经有的数据中验证它们。但是提出假设是谁的工作呢？

有几个相关的角色需要考虑:

*   数据(和业务)分析师非常了解如何争论数据和查询数据，并将根据明确的业务目标运行分析(按需或自行)。但他们的角色和心态并不是去寻找新的目标，或者找到破坏性的新方法来实现这些目标
*   数据和 ML 工程师构建了收集和处理数据的技术和库。他们喜欢看到自己的系统被用来产生强大的洞察力和能力，但把自己视为产生和验证这些假设的基础设施，而不是用户
*   数据科学家将他们强大的统计学和 ML 技能应用于上述数据基础设施，以构建模型，从经过验证的假设中实现新的功能和用户价值。但是模型不是在真空中建立的；他们需要一个明确的使命，源自一个经过验证的假设(甚至是一个有待验证的假设)
*   产品经理是典型的假设创造者类型。他们分析市场，会见客户，深入分析和业务数据，然后他们创建产品假设，收集到路线图中。但是他们很少使用上述“大”数据基础设施来产生假设，主要是由于技术知识的差距

![](img/7237434ce2daeed206e04e7f1e30a615.png)

# 一个新的角色出现了

为了让数据得到充分利用，我们需要的是**一个新角色**，后两者的混合体。**数据科学产品经理**是具有产品经理的直觉和以用户为中心的思维的数据科学家，或者是具有数据科学家的数据探索直觉的产品经理。这需要哪些技能？

*   强烈的数据直觉，探索数据的能力和愿望，包括在有帮助和无帮助的情况下，运用直觉识别特定模式和趋势
*   以用户为中心的思维，看到数据背后的用户和真实生活场景，就像黑客帝国中的 Neo
*   技术敏锐，虽然不一定是编码。今天的 DS 和 ML 工具变得越来越商品化，从头开始编写的需求越来越少
*   非常强的优先排序能力；从数据中创建假设可能很容易，几乎太容易了。因此，需要进一步探索最有前途的，把它们变成一个潜在的路线图。
*   能够与数据团队密切合作，并“说他们的语言”来快速验证、理解产品化成本，并估计大量此类假设的 ROI

虽然这一角色仍然可以由两个协同工作的个人(项目经理和数据科学家)之间的强大合作伙伴关系来完成，但很明显，拥有所有这些技能的单个个人将更高效地取得成果。事实上，在 LinkedIn 上快速搜索一下[就会发现，这种综合角色正在涌现，需求也在激增。](https://www.google.com/search?q=site:linkedin.com+%22product+manager+data+science%22)

*(原帖发布于:*[*https://alteregozi . com/2017/10/05/the-data-product-scientist-manager/*](https://alteregozi.com/2017/10/05/the-data-product-scientist-manager/)*)*