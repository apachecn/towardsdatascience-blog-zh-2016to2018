# 两个比一个好:集合模型

> 原文：<https://towardsdatascience.com/two-is-better-than-one-ensembling-models-611ee4fa9bd8?source=collection_archive---------4----------------------->

组装起初听起来像是一个非常吓人的词，但实际上看起来很简单…让我用一个类比来解释集合

![](img/a3c3271039c2be3213d9fba409653571.png)

Ensembling algorithms to obtain the cake of accuracy

> 集成有点像整个周末都在看网飞(通用 ML 算法)，这很好，但结合一些披萨或一个特殊的朋友，你就可以放松了，我说的“放松”是指达到 90%的准确率

因此，基本上集成/组合两个或更多的算法可以改善或提高你的性能。但是在组合的背后有一个逻辑…你不能只是随机地组合两个模型，并要求提高精确度…每件事背后都有一个数学…..因此，让我们深入探讨几种您可以尝试的组装方法…(如果你喜欢那种东西)

## 简单平均/加权法:

名字说明了一切…这种组合方法只取两个模型的平均值。但是这是怎么做到的…是山达基吗？

假设你已经使用了名为**的算法将**和结果是

```
*Actual_targets: 1 0 1 1 0 1**Predicted vals: 1 0 1 0 1 0*
```

现在你使用另一种叫做**格雷斯**的算法，结果是

```
*Actual_targets: 1 0 1 1 0 1**Predicted vals: 0 1 0 1 0 1*
```

威尔正确预测了前三个目标，而格雷斯正确预测了后三个目标

如果我们结合这两种算法(WILL & GRACE ),我们会得到一个相当不错的情景喜剧，我们的准确性肯定会增加。这是一个平均模型背后的想法，它是一个非常基本的集合案例。

你可以使用 [Sklearn 的投票分类器](http://scikit-learn.org/stable/modules/generated/sklearn.ensemble.VotingClassifier.html)轻松实现这一点，它甚至允许你为每个算法分配权重。

```
from sklearn.ensemble import VotingClassifier
ensemble=VotingClassifier(estimators=[('Decision Tree', decisiontree), ('Random Forest', forest)], 
                       voting='soft', weights=[2,1]).fit(train_X,train_Y)
print('The accuracy for DecisionTree and Random Forest is:',ensemble.score(test_X,test_Y))
```

我们可以根据性能分配权重或取平均值，即给算法设置相等的权重。

## 装袋方法:

装袋方法和前面的很像，但是我们不用威尔和格蕾丝，而是用速度和激情 1，2，3，4，5，6。这意味着我们不用不同的模型，而是用同一个模型的不同版本。

随机森林是一个著名的 bagging 模型，它使用了多棵树的变种。如果使用相同的树，那么它就是一个袋装决策树。

但是为什么装袋有效呢？我们不是又在做同样的事情吗？

让我们考虑一个模型正在训练，它注意到一个实例，一个蓝眼睛、红衬衫、带着笔记本电脑的白人男子坐在星巴克，我们需要找出他是一个有抱负的编剧的概率…现在，该模型似乎已经深入到无法预测它开始证明这种流行刻板印象的每一个实例的概率。每个符合上述描述的白人都不是编剧…..(他可能是 tho)

> 总的来说，当一个模型概括了一些它不应该概括的东西时，它有很高的方差，打包可以减轻这个问题

使用 Bagging 确保预测不会读取高方差。

bagging 模型中的每个模型(bag)彼此略有不同，并确保我们不会停留在非常深的表示上，并输出一个对测试集更友好的预测。

下面是随机森林的代码，一种装袋方法

```
from sklearn.ensemble import RandomForestClassifier
from sklearn.model_selection import train_test_split
train_x, test_x, train_y, test_y = train_test_split(dataset.drop('target'),dataset['target'])
trained_model = random_forest_classifier(train_x, train_y)
predictions = trained_model.predict(test_x)
```

## 增压方法:

因此，在前面的示例中，我们要么采用几个独立的模型并计算它们的平均值，要么计算它们的加权平均值，但如果我们考虑我们之前的模型表现如何，并将其合并到我们的下一个模型中，会怎么样呢…..有点像用以前模型的预测来推动新模型…好了，你知道了…这就是助推的作用。

此外，重要的是要注意，模型是按顺序添加到集合中的。Boosting 有时会通过减少偏差、方差和许多 kagglers 微调 boosting 算法来获得非常棒的结果。

*基于重量的助力*:

假设您正在训练一个模型，其中根据预测为每个预测的标签分配一个权重(好的预测意味着权重较小，坏的预测意味着权重较大)。这有点像因果报应，除了升压不是一个婊子。

这些权重在集合中的每个模型上累加，随着权重的增加，你明白你需要更好地预测它们，并且你开始将你的模型集中在它们上，而不是那些权重较低的模型上。这一直持续到达到最大精确度或最大模型数量。

这些重量有点像现实生活中的重量，一个人收获越多，自尊越低，因此需要去健身房，这里健身房是助推算法…..你总是需要在健身房得到激励…:)

例如:Adaboost，强分类器是权重低的地方，弱分类器是权重高的地方。Adaboost 旨在通过添加模型和纠正以前模型的错误，将弱分类器转换为强分类器(必修教科书定义)

```
*class* sklearn.ensemble.**AdaBoostClassifier**(*base_estimator=None*, *n_estimators=50*, *learning_rate=1.0*, *algorithm=’SAMME.R’*, *random_state=None)*
```

*残基助推*:

在这种类型的提升中，我们分配权重的绝对值(预测和实际标签之间的差异)，并将其作为新变量分配给下一个模型。但是为什么呢？。…..考虑以下情况

```
Actual value : 1
Predicted Val: 0.75
Weight       : 0.25For next model let use this weight to minimize the error in the next model where we predict the value
```

Whaaaaat 所以只要插入一个损失函数，像梯度下降…是的…这种方法也可以被称为梯度推进

这里梯度指的是有限差分，而不是导数

这种方法的一些优点

*   没有过度拟合(使用学习率)
*   我们不依赖单一的预测树(估计数)

为了防止过度拟合，我们可以采用深度学习[的辍学](https://medium.com/@amarbudhiraja/https-medium-com-amarbudhiraja-learning-less-to-learn-better-dropout-in-deep-machine-learning-74334da4bfc5)概念，并将其应用于集成，这确保了随机性和正则化，并确保我们的模型能够很好地推广。即。如果我们在集合中构建了 4 棵树，那么在构建第五棵树时，我们会故意忽略 2 棵随机的树。例如:DART(漏失符合多重可加回归树)。

很多开发者用来赢得比赛的超级棒的助推算法是

*   [Xgboost](https://machinelearningmastery.com/gentle-introduction-xgboost-applied-machine-learning/) (极限梯度提升)，
*   [Lighbgm](https://medium.com/@pushkarmandot/https-medium-com-pushkarmandot-what-is-lightgbm-how-to-implement-it-how-to-fine-tune-the-parameters-60347819b7fc) (垂直增长的非常快速的梯度推进)
*   Catboost(带有良好的初始参数，可以自动编码分类变量)。

希望你觉得这篇文章有用和有趣…..