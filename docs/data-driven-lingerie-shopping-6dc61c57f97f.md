# 数据驱动的内衣购物

> 原文：<https://towardsdatascience.com/data-driven-lingerie-shopping-6dc61c57f97f?source=collection_archive---------3----------------------->

> 如果爱情是盲目的，为什么情趣内衣如此受欢迎？多萝西·帕克

黑色蕾丝还是红色网纱，高腰还是低腰？胸罩还是胸罩？有带子还是没有？丁字裤还是男孩裤？关于睡衣也有类似的问题。但是当我看到价格时，“这个和那个，这些和那些”就变成了“只有这个胸罩和这个内裤”。

我经常问自己:我如何比较，并得到最好的价值价格？维多利亚的秘密品种繁多，但是比卡尔文·克莱恩贵吗？汉克·朱岳鹏比卡尔文·克莱恩便宜吗？又或许梅西百货的卡尔文克莱恩比官网或者亚马逊的卡尔文克莱恩便宜？

由于包含从内衣品牌网站(美国)提取的数据的数据集被发布在 [kaggle](https://www.kaggle.com) 上，我们现在可以分析哪里可以获得最佳价值。也许你会为下一次购买得到灵感，或者你会找到打折胸罩的品牌。

# **分析**

**目的**:确定哪些内衣品牌的价格最低

**数据集**:从美国(。com)内衣品牌网站包括亚马逊和梅西百货。它包含以下信息:

*   产品名称，
*   以美元为单位的最高零售价格(MRP ),
*   以美元为单位的价格(折扣价),
*   产品类别，
*   产品描述，
*   评级数量和评级分数，
*   可用尺寸，
*   总尺寸
*   和颜色。

从分析中排除:评级数量和评级分数、可用尺寸和总尺寸。

**品牌**:

*   卡尔文·克莱因:官网(OW)，亚马逊和梅西百货
*   Topshop
*   维多利亚的秘密:OW 和亚马逊
*   汉克·朱岳鹏:噢，亚马逊和梅西百货
*   沃科尔:噢，亚马逊和梅西百货

![](img/9947449a122584539bd73f473761ffe3.png)

**分析的结构**

在以下部分中，我们将分析不同的问题，即:
1。每个品牌提供的产品种类是什么，他们最贵的产品是什么？
2。哪些品牌有低价的文胸、内裤、胸罩？还有，维多利亚的秘密和卡尔文·克莱恩的比较。
3。总结(您可以直接跳到这里查看见解)

为了进行分析，我们将使用与[群集图](https://seaborn.pydata.org/generated/seaborn.swarmplot.html#seaborn.swarmplot)、[小提琴图](https://datavizcatalogue.com/methods/violin_plot.html)重叠的[箱线图](https://datavizcatalogue.com/methods/box_plot.html)以及平均值和中间值的组合。虚拟化是用 python 的包 seaborn 完成的。

在下一篇博客 中，我们将找出哪些品牌提供的折扣最高，并看看其他一些有趣的发现。更详细的分析请访问 [github](https://github.com/jkokatjuhha/lingerie-shopping) 。

# **1。产品种类**

# **1.1 每个品牌有多少产品？**

在对数据进行广泛过滤后，我决定不考虑尺寸。制造商将通过其产品名称或型号来定义独特的产品。然而，想象只有一个胸罩模型，但是有 9 种不同的颜色。您认为它们是独立独特的产品吗？我决定我会，所以一个独特的产品是由一种颜色和一个模型来定义的。在图 2 中，我们看到亚马逊维多利亚的秘密只销售一种型号的胸罩，但颜色不同，看起来像完全不同的产品。(图 3)

![](img/7667e80d20ac64476eca2f1043f4b7e9.png)

Fig.2: The number of producs as defined by its name and color and a number of unique products defined only by its product name

维多利亚的秘密共有 455 款独特的产品型号。这意味着如此大量的产品(3.5K)是因为该品牌提供了大量不同的颜色。这不就是所有女人想要的吗？；有趣的是，Calvin Klein OW 的销量比梅西百货的 Calvin Klein 还要少，亚马逊 CK 的销量甚至比这两家还要少。

![](img/f1491eededba3c80123dabc066cd41a8.png)

Fig.3: Amazon has the same model of Victoria’s Secret ultimate maximum support sport bra in 9 different colors (three are shown).

# **1.2 品牌提供什么好的产品？**

我们将根据产品类别对产品进行总结，例如丁字裤、衬裤、平角裤等将归入“内裤”类别。bralettes 有一个相当有趣的趋势，所以我们将 bralette 与 bra 区分开来。

一些有趣的事实(快捷方式):

*   汉克·朱岳鹏、亚马逊和梅西百货的卡尔文·克莱恩提供大量包装(主要是内裤)。
*   维多利亚的秘密的主要产品是内裤和运动服(主要由运动胸罩组成)，其次是胸罩和胸罩。
*   梅西百货的 Calvin Klein 比 OW 销售更多的内裤和胸罩，也有更多的产品类别。
*   华歌尔的主要产品是女裤。
*   Topshop 还有母婴用品和梅西百货的 Wacoal shape 产品。
*   一些品牌出售吊带裤、紧身衣裤、婴儿装、长袍、和服、塑身衣和眼罩。

维多利亚的秘密在主要类别(内裤、胸罩、胸罩)中销售最多的产品，除了卡尔文·克莱恩的睡衣。(图 4)

![](img/072dfb9315107bacd323811ac372236d.png)

Fig. 4: Number of products in each category offered by top 6 brands

# **1.3 最贵的物品是什么？**

图 5 显示了每个品牌最贵的商品(基于最高零售价格(MRP))。

![](img/1500127cddbe8cb441747950d809efe7.png)

Fig. 5: Yes, Hanky Panky sells a jar full of panties for $615 (30 pairs of panties)

# **2。最贵或低价的品牌有哪些？**

**2.1 都是关于胸罩的……**

基于 MRP(图 6):

*   Topshop 和维多利亚的秘密的文胸(不到 31 美元)比其他品牌便宜。
*   最贵的文胸由汉克·朱岳鹏提供(但总共只有 15 款)。
*   如果你喜欢 Wacoal，先去梅西百货看看。梅西百货的 Wacoal 有第二贵的胸罩。然而，它销售更多的胸罩，也涵盖了其他代表的价格范围。
*   梅西百货的 Calvin Klein 卖的胸罩比 OW 贵，但也涵盖了 OW 的价格范围。
*   Topshop、维多利亚的秘密和 Macy's Calvin Klein 都有一些非常贵的胸罩，价格超过 70 美元。

![](img/b86de65ecbaefe62b2b25c0453178680.png)

Fig. 6: MRP comparison of bras for each brand

**2.2 低价内裤？**

*   汉克·朱岳鹏的内裤是最贵的。内裤分两组:少的和贵的(基于小提琴剧情 MRP 双峰分布)。
*   维多利亚的秘密有 ca 最低价的内裤。1.2K 型号，大多数价格为 10.50 美元。(但是，不要忘记有些品牌有“33 美元 3 个”或“XX 美元 5 个”的优惠。)
*   Topshop 的大部分内裤的 MRP 为 10 美元，但也有一些更贵的内裤。
*   大多数卡尔文克莱恩内裤的 MRP 是 13 美元。

![](img/b5cc574424c114d55787059e5aaf3bee.png)

Fig. 7: MRP comparison of panties for each brand

然而，别忘了亚马逊和梅西百货也出售卡尔文·克莱恩的成套内裤。大多数内裤的价格是 11 美元，但是亚马逊 Calvin Klein 也提供每条内裤大约 9.75 美元的包装。(图 8)

![](img/58563f885702f81b290ca03476a0695e.png)

Fig. 8: MRP per panty from a pack

**2.3 低价 bralettes？**

*   汉克·朱岳鹏有最贵的胸罩。
*   维多利亚的秘密有最便宜的 bralettes(亚马逊 Wacoal 更便宜但只有 7 件)。

![](img/0448a9941f884bfe2b70eb92d04ca6ba.png)

Fig. 9: MRP comparison of bralettes for each brand

**2.4 卡尔文·克莱恩 vs 维多利亚的秘密**

卡尔文·克莱恩几乎在所有类别都比维多利亚的秘密贵，除了“其他”。有几个昂贵的维多利亚的秘密胸罩，然而，也有许多便宜的维多利亚的秘密胸罩相比卡尔文克莱恩。(图 10)

维多利亚的秘密的 Bralettes 和 activewear 呈现双峰分布，这意味着有两个主要的价格类别。

![](img/d0584494990efdeed7798a29ac8f155b.png)

Fig. 10: MRP comparison of categories of Calvin Klein and Victoria’s Secret

# **3。总结**

**文胸:**

*   如果你是 Wacoal 或者 Calvin Klein 的粉丝，先去梅西百货看看。虽然 Macys 的文胸平均比 OWs 贵，但它提供的文胸更多，产品价格也在其他代表的价格范围内。
*   Topshop 和 Victoria's Secret 的文胸比其他品牌便宜(不到 31 美元)。

**内裤:**

*   维多利亚的秘密有最便宜的带 ca 的内裤。11.50 美元以下的 1.2K 机型。
*   Topshop 的大部分内裤都有 10 美元的 MRP。
*   大多数卡尔文克莱恩内裤的 MRP 为 13 美元，但检查一下亚马逊卡尔文克莱恩和梅西百货的包装。

**小册子:**

*   维多利亚的秘密有最便宜的胸罩。

# 祝你下次购买内衣愉快！

[![](img/83dc7212ef8502659c81086ad58b8d96.png)](http://buymeacoff.ee/YqodIpL)