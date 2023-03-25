# 如何提出数据科学可以解决的问题？

> 原文：<https://towardsdatascience.com/how-to-ask-questions-data-science-can-solve-e073d6a06236?source=collection_archive---------0----------------------->

![](img/e24bda24912e221ccf9f986d6c09582c.png)

我的学生经常很难找到好的数据科学问题。

通常，这是因为他们还没有弄清楚问题如何映射到数据解决方案。我发现将布鲁姆的分类法与数据技术结合起来描绘一幅更清晰的画面是很有见地的。

数据科学工具初看起来可能非常有限，但是我们可以用工具的语言重新表述大多数现实世界的问题。

## ***我们可以问什么样的问题？***

布鲁姆的分类法对教育者用来引导学生的学习目标进行了分类。我发现对洞察力进行分类也很有用。毕竟，如果我们提供适用的洞察力，我们就扮演了教育者的角色。

[布鲁姆的分类学](https://en.wikipedia.org/wiki/Bloom's_taxonomy)也提出了我们可以问的问题来测试学生。这些相同的问题带来了卓越的见解。

我们将学习过程分为 6 个目标，每个目标都有相关的问题。作为数据科学家，我们可以提出、解决和分享这些问题来获得洞察力。

## **布鲁姆分类法中的认知目标**

*   记得吗——什么人，什么事，什么地方，什么时候发生了什么事？
*   **了解—** 你能总结一下发生了什么吗？
*   **应用—** 当……发生时会发生什么？
*   **分析—** 什么是……的关键部分和关系？
*   评估— 这是最好的方法吗？
*   创造——你能预测在新的条件下……会发生什么吗？

## **可用工具**

(从业者可能想跳过这个概述)

这个行业有很多工具，但是你可以把它们分成几个部分。

***R/Python/SQL/Etc***

用 SQL，R，Python 等进行数据操作。允许我们搜索和汇总数据。

这些工具回答与记忆和理解相关的问题。“我的最大用户最后一次购买是什么时候？”

***假设检验***

仅仅因为我们按组划分数据并不意味着我们找到了关系。 [**假设检验**](https://en.wikipedia.org/wiki/Statistical_hypothesis_testing) 告诉我们我们的数据是否适用于新的情况。"猫的图片比狗的图片带来更多的流量吗？"

***场景分析***

[**情景分析**](https://en.wikipedia.org/wiki/Scenario_analysis) 分析各种条件下未来可能出现的多种结果。我们创造许多可能的场景，然后预测会发生什么。"如果我们提高产品价格，会发生什么？"

***优化***

[**优化**](https://en.wikipedia.org/wiki/Mathematical_optimization) 是一个巨大的领域，但它一般会问简单却难以回答的，最大化和最小化问题。"什么样的供应路线能最大限度地降低递送包裹的成本？"

***强化学习***

[强化学习](https://en.wikipedia.org/wiki/Reinforcement_learning)实时观察数据并优化结果。"在游戏 Flappy Bird 中，我应该什么时候点击才能生存？"

***统计建模与机器学习***

这变得很棘手，因为这些是巨大的领域。让我们来看几个主要任务:

*   [分类](https://en.wikipedia.org/wiki/Statistical_classification)和[回归](https://en.wikipedia.org/wiki/Regression_analysis)——“多少…？”，“什么样的…？”
*   [特征选择](https://en.wikipedia.org/wiki/Feature_selection)——“什么变量是相关的？”
*   [降维](https://en.wikipedia.org/wiki/Dimensionality_reduction)——“我的数据的关键组成部分是什么？”
*   [聚类](https://en.wikipedia.org/wiki/Cluster_analysis)——“我能对我的数据进行分类吗？”
*   [异常检测](https://en.wikipedia.org/wiki/Anomaly_detection)——“这个观察奇怪吗？”

***分类回归***

[分类](https://en.wikipedia.org/wiki/Statistical_classification)和[回归](https://en.wikipedia.org/wiki/Regression_analysis)回答诸如“我的数据和一个或多个结果之间有关系吗？”[分类](https://en.wikipedia.org/wiki/Statistical_classification)重点预测群体“这是 A 还是 B？”。[回归](https://en.wikipedia.org/wiki/Regression_analysis)关注数量“多少—或—多少？”

***特征选择***

[特征选择](https://en.wikipedia.org/wiki/Feature_selection)确定数据中的哪些特征与特定结果相关。假设我们想要识别一种水果是苹果还是橘子。我们在数据中使用颜色和甜度作为水果的特征。一个[特征选择](https://en.wikipedia.org/wiki/Feature_selection)算法将缩小颜色作为一个有用的鉴别器，因为苹果和橘子都是甜的。

***降维***

[降维](https://en.wikipedia.org/wiki/Dimensionality_reduction)获取数据并将其降维至核心成分。这就像图像压缩，我们用较少的信息显示相同的图像。假设我们有一次性刀叉和餐盘的销售数据。一个[维度缩减](https://en.wikipedia.org/wiki/Dimensionality_reduction)可能显示一次性用具销售的一列。我们大概会问“我的销售数据中的关键模式是什么？”

***聚类***

[聚类](https://en.wikipedia.org/wiki/Cluster_analysis)试图获取数据，并自动将相似的观察结果组合在一起。我们可以把我们的数据组织和处理成几种类型的观察结果的集合。我们问“我是否有任何不同类型的*客户，或者他们都是完全独特的？”*

***异常检测***

[异常检测](https://en.wikipedia.org/wiki/Anomaly_detection)回答一个观测值是否属于数据集。我们大概会问“这个温度读数正常吗，还是很奇怪？”需要注意的是，我们通常可以简化这个问题。一个[分类](https://en.wikipedia.org/wiki/Statistical_classification)问题，询问“这是不是很奇怪？”就像是[的异常探测](https://en.wikipedia.org/wiki/Anomaly_detection)。

## **它们是如何组合在一起的？**

我在下面列出了常见的数据科学问题，按照[布鲁姆的分类法](https://en.wikipedia.org/wiki/Bloom's_taxonomy)整理。每个问题都被重新表述，以使用一种常见的数据科学技术。问题按照从最容易回答到最难回答的顺序排列。

***记住—*** *什么人，什么事，什么地方，什么时候发生了什么事？*

我们用 SQL、R、Python 等来回答关于数据收集和操作的记忆问题。

> *特定用户使用什么浏览器浏览本网站？*

我们使用 SQL、R 或 Python 在数据中找到用户，以及他们使用的浏览器。

> *特定用户是如何找到这个网站的？*

我们使用 SQL、R 或 Python 以及记录的流量源在我们的数据中找到用户。

***明白—*** *你能总结一下发生了什么吗？*

我们通过汇总或总结数据来回答理解性问题。

> 我的用户倾向于使用什么浏览器？

同样，使用 SQL、R 或 Python，我们可以在数据中按浏览器统计用户数量。

***应用—*** *当…，会发生什么？*

我们通过要求我们的结果一般化来回答应用问题。 [**假设检验**](https://en.wikipedia.org/wiki/Statistical_hypothesis_testing) ， [**交叉验证**](https://en.wikipedia.org/wiki/Cross-validation_(statistics)) ， [**实验方法**](https://en.wikipedia.org/wiki/Experiment) 是保证通用性的技术。

> *晒太阳的时间和植物的高度有关系吗？*

这是一个 [**回归**](https://en.wikipedia.org/wiki/Regression_analysis) 的问题，Y = f(X)。Y =植物高度。f 代表捕捉关系的任何模型。X =植物在阳光下度过的时间。

> *这款空调在未来 3 年会出现故障吗:会还是不会？*

这是一个 [**分类**](https://en.wikipedia.org/wiki/Statistical_classification) 的问题，Y = f(X)。Y = {失败，不要失败}。f 代表捕捉关系的任何模型。x 是记录空调故障历史和相关特征的数据。

> *哪种动物出现在给定的图片中？*

这也是一个 [**分类**](https://en.wikipedia.org/wiki/Statistical_classification) 问题，Y = f(x)，有时也叫[多类分类](https://en.wikipedia.org/wiki/Multiclass_classification)。Y = {狗猫马其他}。f 代表捕捉关系的任何模型。数据 X 是编码成表格形式的图像。

> 该客户购买产品的可能性*是多少？*

这是一个 [**分类**](https://en.wikipedia.org/wiki/Statistical_classification) 问题，Y=f(X)，Y = {买，不买}。x 是与客户购买习惯相关的数据。许多算法将能够给你落入特定类别的概率。

> *这笔银行交易是否属于欺诈？*

这是一个 [**分类**](https://en.wikipedia.org/wiki/Statistical_classification) 的问题，Y=f(x)。Y = {欺诈，不欺诈}。x 是银行交易数据。[异常检测](https://en.wikipedia.org/wiki/Anomaly_detection)也可以处理这个问题。[异常检测](https://en.wikipedia.org/wiki/Anomaly_detection)即使你没有贴上欺诈标签的过往数据，也可能行得通，但这是个更难的问题。

***分析—****……的关键部分和关系是什么？*

要回答分析问题，你需要将数据分解开来，寻找模式。 [**特征选择**](https://en.wikipedia.org/wiki/Feature_selection) 、 [**降维**](https://en.wikipedia.org/wiki/Dimensionality_reduction) 、 [**聚类**](https://en.wikipedia.org/wiki/Cluster_analysis) 是关键工具。

> *哪些因素最能预测电力需求？*

这是一个[](https://en.wikipedia.org/wiki/Regression_analysis)****回归与 [**特征选择**](https://en.wikipedia.org/wiki/Feature_selection) **，** Y=f(X)的问题。Y =所需电量。f 代表任何能捕捉你的数据和电力需求之间关系的模型。x 可能具有价格、温度、季节、地区和许多其他特性。为了找到最重要的因素，我们使用 [**特征选择**](https://en.wikipedia.org/wiki/Feature_selection) 来去除不预测电力需求的因素。****

> *****苹果和橘子的主要区别是什么？*****

****这是一个 [**分类**](https://en.wikipedia.org/wiki/Statistical_classification) 与 [**特征选择**](https://en.wikipedia.org/wiki/Feature_selection) 的问题，Y=f(X)。Y = {苹果，橘子}。f 代表捕捉数据中关系的任何模型。x 有很多特征，比如身高，体重，颜色，味道，和韧性。 [**特征选择**](https://en.wikipedia.org/wiki/Feature_selection) 找到最能区分苹果和橙子的特征。****

> *****在我的 HVAC 系统中，哪组传感器倾向于相互变化(或相反)？*****

****这是一个 [**聚类**](https://en.wikipedia.org/wiki/Cluster_analysis) 问题，因为我们将相似的传感器彼此分组。我们将传感器数据组织为行，将读数的时间*组织为列。*****

> *****在我的暖通空调系统中，哪种传感器组合最能显示系统的整体健康状况？*****

****这是一个 [**降维**](https://en.wikipedia.org/wiki/Dimensionality_reduction) 的问题。我们获取大量数据，并将其转化为几个关键绩效指标[](https://en.wikipedia.org/wiki/Performance_indicator)**。在这种情况下，我们将不同传感器的数据组织为不同的列。******

> *******哪些观众喜欢同类电影？*******

******这很奇怪，因为我们试图将相似的用户和相似的电影分组。这是典型的 [**推荐引擎**](https://en.wikipedia.org/wiki/Recommender_system) **。**我们还可以写一个更简单的应用程序为“这个用户喜欢这组电影吗？”或者甚至简单到“这个用户会喜欢这部电影吗？”******

> *****成功的 CEO 有哪些共同的领导实践？*****

****这乍一看像是一个分组问题。一旦你读懂了字里行间的意思，就会回到关键的区别上来。所有成功的 CEO 都要吃饭，所有不成功的 CEO 也一样。我们对预测成功的因素更感兴趣。****

*******评估—*** *这是最好的方法吗？*****

****要回答评估问题，您需要将您的数据推断到复杂的假设案例中。****

> ****我们能否通过更好地为不同产品定价来节省资金？****

****这就归结为 [**场景分析**](https://en.wikipedia.org/wiki/Scenario_analysis) 。我们提出了几种定价方案，然后用模型预测它们的效果。这就很可能涉及到[](https://en.wikipedia.org/wiki/Statistical_classification)**[**回归**](https://en.wikipedia.org/wiki/Regression_analysis) ，以及批判性思维。******

*******创造—*** *你能预测在新的条件下……会发生什么吗？*****

****创造问题要求你创造新的最优解。****

> ****我的送货车应该走什么路线？****

****这是一个众所周知的 [**优化**](https://en.wikipedia.org/wiki/Mathematical_optimization) 问题。主要标准是在按时交货的同时，尽量减少燃料费用。****

> ****我们应该在哪里设立新的办公地点？****

****这里我们需要优化到一个特定的标准。一个简单的就是利润最大化，但实际上考虑的更多。为了编写一个[优化](https://en.wikipedia.org/wiki/Mathematical_optimization),我们需要能够评估位置。这将我们带回到应用、分析和评估阶段。****

> *****我应该把这个广告放在网页的什么位置，这样浏览者最有可能点击它？*****

****你可以把它写成一个[优化](https://en.wikipedia.org/wiki/Mathematical_optimization)，但是还有更好的选择。移动一个广告并观察它的表现并不昂贵。这意味着我们可以实验，而不是提前决定。尝试定位广告并测试其效果。你甚至可以通过 [**A/B 测试**](https://en.wikipedia.org/wiki/A/B_testing) 或者 [**强化学习**](https://en.wikipedia.org/wiki/Reinforcement_learning) 来自动化这个过程。****

> *****我的自动冷却和加热系统应该将温度调高、调低还是保持不变？*****

****这是 [**强化学习**](https://en.wikipedia.org/wiki/Reinforcement_learning) 的好区域。您的冷却系统会根据输入数据进行调整，如电价、时间和您的偏好。****

****有了锤子，一切都成了钉子****

****我们应该先提问。很容易陷入我们的数据和工具中。我们忘记了还有更广泛的问题可以解决。****

****数据问题遵循从容易到困难的连续体。问许多小问题会带来进步，让你获得意想不到的真知灼见。****

*****如果您觉得这很有帮助，请推荐并分享，点击💚这样其他人就能看到了。*****