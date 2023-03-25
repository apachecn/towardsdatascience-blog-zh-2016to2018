# 这会让你知道你有多需要用 Airbnb 旅行

> 原文：<https://towardsdatascience.com/this-will-make-you-know-how-much-you-need-to-travel-with-airbnb-442abd51c8c4?source=collection_archive---------25----------------------->

## 基于西雅图、波士顿 Airbnb 数据集

![](img/a39ff678be9916d7bbfd282e568d2c13.png)

Photo by [Zhifei Zhou](https://unsplash.com/@phoebezzf?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

![](img/38d7fee6167105ea13b22e34eac360a3.png)

Photo by [Lance Anderson](https://unsplash.com/@lanceanderson?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

这些图片促使你去赶飞机吗？如果你的答案是肯定的，你和我一样是一个“嗜 Hodophile”。暂时远离现实，成为一名观察者是一种充满敬畏的感觉——它让你感激你所拥有的，你所能做的，你的朋友和家人。**旅行让你再次成为自己** —万岁！老实说，我最喜欢的回忆之一是在保加利亚坐在长椅上喝着啤酒看着人们走过(那里还好，至少没人说什么)。

我不知道你有多富裕，但作为一名学生，预算一直是一个问题，让我不敢坐飞机。我已经使用 Airbnb 很长时间了，因为我喜欢使用 Airbnb 比使用酒店或其他住宿更自由，此外，我碰巧找到了价格更低的家一样的地方。 ***然而，在走访不同城市时，我观察到 Airbnb 的价格各不相同。*** 我早就该走进数据世界了。

数据集由来自 Kaggle 的关于[西雅图](https://www.kaggle.com/airbnb/seattle/data)和[波士顿](https://www.kaggle.com/airbnb/boston) Airbnb 的信息组成，每个数据集有 3818 和 3585 行。我的目标很明确: ***【了解 Airbnb 价格】*** 。为此，我有一些问题，我很乐意与你分享我的答案。

# 1.两个城市实际上存在价格差异吗？

**给你答案，是的有区别**。看看我是怎么得出这个答案的。由于单价是 1000 美元，并且由于一些异常值(非常昂贵的 Airbnb ),很难区分两个城市之间的差异。

![](img/8ae86779469667b91a9a35b55edb95cd.png)

Figure 1: price distributions of Seattle and Boston

经过价格的对数变换后，我可以更清楚地看到这种区别。

![](img/415cb4f4a42eca79a574568aa20c0f91.png)

Figure 2: log-price distributions of Seattle and Boston

我们观察到**波士顿 Airbnb 的价格普遍较高，而且更加分散**。西雅图对数价格分布显示出更加对称的方面，而波士顿对数价格分布似乎稍微偏左，这意味着波士顿租金价格形成较高。从技术上讲，西雅图和波士顿的对数价格均值分别为 4.68 和 4.94，标准差分别为 0.57 和 0.65。

**根据** [**Trulia**](https://www.trulia.com/) **，这对应的是每月租金中位数。平均而言，波士顿的月租金中位数约为 2900 美元，而西雅图的月租金中位数约为 2700 美元。因此，这个结果并不奇怪。然而，有趣的是**两个城市的销售价格中位数显示出相反的趋势:西雅图的房地产销售价格中位数平均为 69 万美元，波士顿为 61 万美元。**使用价格与租金比率的概念，我们得出波士顿为 17.53，西雅图为 21.30。根据一般经验，如果该比率从 16 到 20，那么购买房产是有风险的，如果是 21+，那么租房比买房好得多。因此，波士顿似乎是一个更有希望买房的地方。**

# 2.同一个主机拥有多少 Airbnb 房产？

自从我确认存在价格差异，我想找到更多的为什么。我的第一个兴趣是少数“超级”主机是否决定了市场价格。是这样吗？

![](img/8f517af340bd2b0e364863b68ff09066.png)

Figure 3: Average property numbers owned by a host

波士顿的房东平均拥有更多 Airbnb 房产。这可能意味着，决定 Airbnb 房产价格的主机数量减少了。但是我们现在还不能确定任何事情，所以让我们继续深入研究。

![](img/4cc61464a3f73c49d83cf7b1bc429bfe.png)

Figure 4: Distribution of number of properties in Seattle

![](img/5208ee4f09a6931ea767a6086be666f9.png)

Figure 5: Distribution of number of properties in Boston

查看单个分布，我们可以确认在波士顿有更多的“超级”主机。因此，我们可以假设这些“超级”主机将波士顿的 Airbnb 价格定得更高。但我没说他们真的有。有了预测模型，我就能知道这个预测器排在哪里了。

# 3.价格如何基于位置传播——均匀还是不均匀？

我的下一个问题是，位置是否会影响两个城市的 Airbnb 价格，因为繁忙的位置价格较高似乎是很自然的。我使用邮政编码作为位置信息的代理。

![](img/c76642dd12e003ddab16bf5fc17664d9.png)

Figure 6: Seattle location distribution by zip code

![](img/eafe81240d1275e9a6c061267fdd611e.png)

Figure 7: Boston location distribution by zip code

就扩散程度而言，根据标准差，两个城市的位置分布没有显示出太大差异。然而，我们注意到一个模式，它不是均匀分布的。当我们将分布与来自 [Trulia](https://www.trulia.com/) 的租金地图([西雅图](https://raw.githubusercontent.com/soo-pecialist/Udacity_Nanodegrees/master/Data%20Scientist/04_Data_Scientist_Blogging/image/Seattle_rent.png)和[波士顿](https://raw.githubusercontent.com/soo-pecialist/Udacity_Nanodegrees/master/Data%20Scientist/04_Data_Scientist_Blogging/image/Boston_rent.png))进行比较时，**高比例位置对应于高租金区域**；因此对价格分配有更大的影响。

![](img/906fa0cac04d527dedd929f3dbde3b29.png)

Figure 8\. Seattle and Boston rent heatmaps

毕竟，大多数 Airbnb 房产都拥挤在城市的热点地区。

# 4.我的模型能很好地预测价格吗？

嗯，没有什么是完美的——至少在数据科学领域是这样。然而，我对我的预测模型的性能非常满意。在表 1 中，预测价格与实际价格相差不远。

![](img/c66cfc123c5380219756623a1abba3c4.png)

Table 1: T*rue price values and predicted price values*

*【****我知道这可能让人望而生畏。这只针对好奇的乡亲们*** *:*

*我采用了臭名昭著的* ***极限梯度推进(又名 XGBoost)回归器*** *作为框架，并结合了穷举* ***网格搜索交叉验证。*** *在每次迭代中，用 5 重交叉验证对模型进行评估，并找到给出最佳性能的最佳参数。*

*与交叉验证 RMSE 相比，测试 RMSE(均方根误差)有所降低，约为每 1，000 美元 78.0201，这是有希望的——这意味着* ***该模型在野外的表现优于实践中的*** *！】*

*如需了解更多详情，请访问我的*[*Github*](https://github.com/soo-pecialist/Udacity_Nanodegrees/tree/master/Data%20Scientist/04_Data_Scientist_Blogging)*！*

# 5.价格最重要的预测因素是什么？

![](img/6acfb3dac6a6b0e1c525ac930194ab21.png)

Figure 9\. Top 10 important features for Airbnb price

现在，我们获得了十大重要功能列表，内容如下:

*   zip_has:邮编所在位置的 Airbnb 房产数量。位置繁忙程度的指示器。
*   浴室:浴室数量。
*   host _ days:host 托管 Airbnb 房产的天数。
*   卧室:卧室数量。
*   minimum_nights:您需要预订的最低住宿天数。
*   extra_people:比主人多的客人额外收费。
*   床位:床位数
*   guests _ included:Airbnb 酒店可以容纳的客人数量。
*   availability_365:一年中可用的天数。
*   评论数量:评论的数量。

如果你看到上面的列表，它们在预测价格中扮演重要角色似乎是很自然的。第一，**如果这个地方是一个旅游景点等繁忙的地方，对住宿的需求应该更多；从而提高了价格。** **浴室、卧室、床的数量反映了舒适度的高低。没有人愿意在洗手间前排队等候或睡在沙发上。**如果你已经经营 Airbnb 房产很长时间了**，你应该知道市场是如何波动的。从数据科学的角度来看，**你更可预测**，因为你比一个新主持人更有可能引领市场趋势。**最少住宿、额外客人费用和最多客人人数是由主办方设定的规则，并直接计入价格计算中。**所以这三个因素排名靠前是有说服力的。最后，**一年中可用的天数和评论的数量反映了 Airbnb 房产的受欢迎程度。自然，高需求导致更高的价格。****

我们没有看到“host_has”(一个主人拥有的财产数量)排在前 10 名(实际上排在第 13 名)——这里的一个要点是，我们的本能在某些情况下不是最好的工具，这个世界需要数据科学家！(哈哈)

# 结论

在这个项目中，我们深入研究了西雅图和波士顿的 Airbnb 数据集，发现了一些有趣的模式:

1.  波士顿是使用 Airbnb 旅行比西雅图更贵的城市。这一现象符合目前两个城市每月租金中位数的趋势——波士顿的房租更贵。
2.  波士顿的房东平均拥有更多 Airbnb 房产，也有更多“超级”房东。因此，这可能意味着几个超级主持人可以左右城市的平均价格。
3.  高比例位置对应于西雅图和波士顿的高租金区域。因此，我们可以假设租金、位置和 Airbnb 价格密切相关。
4.  根据重要特征，我们知道受欢迎程度、位置、舒适度和物业规则会影响 Airbnb 的价格。

这个项目提出了一些问题，如两个城市的价格是否存在差异，价格如何基于位置传播，以及最重要的预测因素是什么。希望这个项目回答了这些问题，并为你提供了什么可以影响 Airbnb 价格的见解。 ***我相信这种预测模型可以为顾客提供有关旅行预算和计划的信息，并为主人为他们的房产定价提供参考。***

尽管如此，仍有很大的改进空间。**我张开双臂欢迎任何反馈和建议** —最后，**我们数据科学家分享！做这个项目时，我渴望去旅行，我也想去。无论您在哪里旅行，无论是环游世界还是数据世界。我祝你旅途好运。一路顺风！**

## 与作者联系

*你可以在* [*LinkedIn*](https://www.linkedin.com/in/soohyeonseankim/) 上和他联系

*如需了解更多详情，请访问他的* [*Github*](https://github.com/soo-pecialist/Udacity_Nanodegrees/tree/master/Data%20Scientist/04_Data_Scientist_Blogging)