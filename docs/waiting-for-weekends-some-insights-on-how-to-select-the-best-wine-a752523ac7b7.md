# 等待周末:如何挑选最好的葡萄酒

> 原文：<https://towardsdatascience.com/waiting-for-weekends-some-insights-on-how-to-select-the-best-wine-a752523ac7b7?source=collection_archive---------18----------------------->

![](img/50410484c4228e53f7189e175f32cb08.png)

市场上有大量的葡萄酒可供选择，对于葡萄酒爱好者来说，选择最好的葡萄酒永远是一个追求。美国、法国、西班牙、德国和许多其他葡萄酒国家的葡萄酒品种繁多，在任何酒类商店都很容易买到。价格也会有很大差异。根据我的经验，更高的价格并不总是保证更好的质量。有了这么多选择，买酒的最佳诀窍是什么？

为了回答这个问题，我使用了 2017 年收集的来自 [Kaggle](https://www.kaggle.com/zynicide/wine-reviews) 的葡萄酒评论数据，并在下面检查了典型客户在购买葡萄酒时可能感兴趣的几个问题。这些数据涵盖了来自 43 个国家的 129971 篇葡萄酒评论。每篇葡萄酒评论都包含标题、国家、省份、地区、品种、价格、0-100 分、品酒师描述和品酒师信息等信息。

# **1。哪些国家是主要的葡萄酒生产国，哪些种类的葡萄酒在这些国家的评价最高？**

![](img/295fcc9f8ed16b99b1b9163f8bf033f6.png)

首先，令人惊讶的是，美国生产的葡萄酒数量是第二大葡萄酒生产国法国的三倍。第三大葡萄酒生产国意大利的葡萄酒数量与法国大致相当。还值得注意的是，两个南美国家(智利和阿根廷)的产量高于奥地利和德国等欧洲国家。

接下来，数据显示，在美国最受欢迎的葡萄酒生产葡萄是黑皮诺、赤霞珠和霞多丽。对欧洲国家来说，这些品种并不那么受欢迎。所以，当你购买欧洲生产的葡萄酒时，请记住这一点。有趣的是，尽管欧洲国家距离很近，但它们生产最多的品种却是不同的。在法国，大多数生产的葡萄酒是波尔多风格的混合红，意大利是混合红，西班牙是 Tempranillo，葡萄牙是葡萄牙红，奥地利是 Grüner Veltliner，德国是 Riesling。

然而，产量最高的葡萄酒并不意味着质量最好。下面是产量最高的 15 种葡萄酒的图表，中间值用点数表示。因此，如果你购买美国葡萄酒，你最好的选择是黑皮诺、西拉和波尔多风格的混合红葡萄酒。对于第二大生产国法国来说，最好的品种是雷司令、黑比诺、香槟混合、琼瑶浆、马尔贝克和灰比诺。如果你更喜欢意大利葡萄酒，搜索桑娇维塞格罗索和内比奥罗。

令人惊讶的是，所有奥地利葡萄酒的评级都非常高，如雷司令、布劳夫兰基施(blaufrnkisch grüner velt liner)、长相思(Sauvignon Blanc)等(见下表)。与法国葡萄酒类似，奥地利最高等级的葡萄酒品种是雷司令。请记住，虽然法国有许多品种的中等质量低于奥地利。

![](img/6c1763e464f85bd710a62426355efa50.png)

Variety quality analysis for US, France, Italy and Austria. You can find similar analysis for other countries [here](https://github.com/lenuel/Wine-Analysis/blob/master/wine-analysis.ipynb).

# 2.不同价格类别中评价最好的葡萄酒有哪些？

现在让我们来看看葡萄酒的价格。为了让你购物更容易，我分析了四个价格类别。在这里，我们将只展示价格低于 50 美元的前两个类别的分析，而对于价格高于 50 美元的另外两个类别的分析，您可以在这里找到。

***$ 0-$ 15***

你可能会认为，如果你付不到 15 美元，就不可能品尝到高评价的葡萄酒。我会给你惊喜，但你可以，这是清单:

![](img/fde1a1c1ab80fd7fdaae53c10a067224.png)

最有可能的是，这些葡萄酒可能很难在你当地的杂货店找到(尤其是在这篇文章之后)，但是你可以尝试使用这个网站[https://1000corks.com/](https://1000corks.com/)搜索葡萄酒，并检查你附近是否有特定葡萄酒的位置。

![](img/e2c1dcc0f86c439cc02b3abf3e412e03.png)

让我们也检查一下价格和质量之间是否有任何关联。观察点数 vs 价格的散点图，你可以看到它看起来更像一个扭曲的三角形，而不是一条直线。很明显，有很多葡萄酒的质量与其价格不符。

***$ 15-$ 50***

如果你认为在前面的类别中价格和质量之间没有相关性，看看 15-50 美元类别中的散点图——它只是一个完全没有相关性的正方形。在这个价格类别中，高价格根本不能保证质量。例如，有很多酒的评分只有 80 分，价格从 15 美元到 45 美元不等。然而，请记住，总的来说，与以前的价格类别相比，有更多的葡萄酒评分超过 90 分。

如果你想品尝这一类别中的一些最好的葡萄酒，下面列出了评级最高的葡萄酒。例如，你可以尝试售价 44 美元的葡萄酒，比如加州索诺马海岸生产的夏敦埃酒，它被打了 99 分(见下文)。

![](img/86cfeb7a6e4a3be2eabc840637f1ae4a.png)

# 3.对于不同国家生产的最佳葡萄酒，最常见的描述是什么？

这个数据库的有趣之处在于它包含了葡萄酒评论。因此，我按国家分析了评级最高的葡萄酒(> 87 分)中的受欢迎程度，并将其映射到国家轮廓上。你可以看到每个国家对葡萄酒的描述都有一些独特之处。

我不需要再讲什么了，这些图片上有几千个字。选择你的鞋底所属的国家！(你可以找到更多的国家 wordcloud。pdf 文件[此处](https://github.com/lenuel/Wine-Analysis/tree/master/WineWordcloud)

## 美国

![](img/2c20ea665c48c84cedbc2cd668c12350.png)

## 法国

![](img/d1be62a230c5faf98b507c326366f21e.png)

## 意大利

![](img/87b0e94c5c2176f2487434c370483a8f.png)

## 西班牙

![](img/f843d3d1503fb514efbd588fd6da831c.png)

## 奥地利

![](img/4bc78e44dcda9cb04f8c097bd340f3b9.png)

# 4.葡萄酒的质量可以预测到什么程度？

由于在大多数情况下，价格并不是葡萄酒质量的最佳指标，我很好奇如何使用现有的机器学习工具来预测葡萄酒的质量。因此，我创建了一个模型，使用国家、价格、品种和省份等特征以及从标题中提取的其他三个特征(如年份、起泡和年份)来预测质量点。实际分数和预测分数之间的散点图如下所示，训练模型能够预测质量，r2 分数为 0.49，均方误差为 4.9 点。

![](img/1f4b295f6014586ca0b3efbbd248b380.png)

虽然图中有明显的散点，但这比简单地看价格和质量之间的相关性要准确得多。那么，对于一款高品质的葡萄酒来说，什么特征是最重要的呢？该算法显示，影响葡萄酒评级的主要特征是价格和年份。有趣的是，接下来影响评级最大的特征是加州省、黑皮诺品种、霞多丽品种、混合红品种和华盛顿省。所以，根据经验，当你不知道你想要什么样的葡萄酒时，你可以选择加州的黑比诺葡萄酒，这是陈年的，价格你可以承受。

![](img/e51b7571a0fe9e7e6c45d3fe126617a2.png)

# 结论

在这篇文章中，我根据对从 [Kaggle](https://www.kaggle.com/zynicide/wine-reviews) 获得的 2017 年葡萄酒评论数据的分析，展示了一些关于如何选择最佳葡萄酒的见解。

*   在葡萄酒数量最多的国家，最受欢迎的葡萄酒品种是美国的黑皮诺，法国的波尔多风格的混合红，意大利的混合红，西班牙的坦普拉尼洛。
*   美国评价最高的葡萄酒品种是黑比诺、西拉和波尔多风格的混合红，法国是雷司令、黑比诺、香槟混合、琼瑶浆、马尔贝克和灰比诺，意大利是桑娇维索和内比奥罗。
*   奥地利生产的葡萄酒有很多评级很高的品种，其中评级最高的品种是雷司令。
*   有些酒即使价格低于 15 美元，也有很高的评价。价格区间较高(15-50 美元)的葡萄酒并不能保证质量更好。
*   葡萄酒的质量主要与价格和年份有关。地理位置也很重要，最好的葡萄酒通常来自加州和华盛顿。在各种类型的葡萄中，黑比诺、霞多丽和混合红通常是质量较好的标志。

> ***是时候去商店买酒了吗？***

要了解更多关于这个分析的内容，请点击这里的链接[查看我的 Github。](https://github.com/lenuel/Wine-Analysis)