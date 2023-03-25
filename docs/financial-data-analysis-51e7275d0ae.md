# 机器学习算法可以帮助我们评估金融决策的风险

> 原文：<https://towardsdatascience.com/financial-data-analysis-51e7275d0ae?source=collection_archive---------11----------------------->

## 贷款资格预测:创建预测模型

![](img/6999a37544690c11871e3bdf888f9dd0.png)

本文是构建预测模型系列文章的最后一部分。在前两部分([第一部分](https://medium.com/@sabber/financial-data-analysis-80ba39149126)和[第二部分](https://medium.com/@sabber/financial-data-analysis-bf4b5e78c45c)，我展示了如何处理和清理原始数据。然后在[的第三部分](https://medium.com/@sabber/financial-data-analysis-2f86b1341e6e)，我将展示如何执行一些探索性的数据分析(EDA)来理解数据和特性工程。在处理和 EDA 的最后，我们现在有了 30 个最相关的特性，包括目标变量 loan_status。在文章的最后，也是最激动人心的部分，我将展示如何从一个简单的模型开始，然后继续构建一个高精度的复杂模型。在这项工作中，我能够构建一个基于树的 LightGBM 模型，准确率为 87%,而初始的逻辑模型预测准确率为 66%。在接下来的部分中，我描述了用必要的理论和代码改进模型的一步一步的过程。好了，让我们从导入一些必要的库开始。

**导入库**

```
import numpy as np
import pandas as pd
import matplotlib.pyplot as plt
import seaborn as snsfrom sklearn.linear_model import LogisticRegressionimport lightgbmfrom sklearn.utils import resample
from sklearn.ensemble import RandomForestClassifier
from sklearn.preprocessing import StandardScaler
from sklearn.model_selection import train_test_split
from sklearn.model_selection import cross_validate, KFoldfrom sklearn.metrics import recall_score, roc_auc_score, f1_score
from sklearn.metrics import accuracy_score, roc_auc_score, \ classification_report, confusion_matrix
```

**读取所选功能**

```
df_selected = pd.read_csv(‘./data/df_selected.csv’)df_selected.describe(include = 'all')
```

## 看阶级比例

在我们开始任何事情之前，让我们检查一下数据集中的类比率。如果一个类的例子数量很少，而另一个类的例子数量很多，我们就认为数据是不平衡的，我们必须在创建模型之前解决这个问题。以下代码显示了我们使用的数据的类比率:

```
df_selected.loan_status.value_counts(normalize=True)0    0.783494
1    0.216506
Name: loan_status, dtype: float64df_selected.loan_status.value_counts()
0    358436
1     99048
Name: loan_status, dtype: int64
```

显然，上面打印的结果显示数据集是不平衡的。1 类(已注销)是少数类别，占全部样本的 21.6%。该数据集中，按时还清贷款的申请人比被取消贷款的申请人更多。这对于机器学习模型来说是一个问题，因为该模型将主要基于具有更多示例的类来学习其参数。因此，模型会偏向多数阶级。要解决不平衡的阶级问题，我们可以使用许多技巧，例如:

(1)分配类权重(2)使用具有交叉验证的集成算法(3)对少数类进行上采样或对多数类进行下采样

在我的另一篇博客([用不平衡数据集提高机器学习模型性能的三种技术](http://In my other blog ('Three techniques to improve machine learning model performance with imbalance datasets') I wrote a blog post describing the above three techniques. In this work, I have tried all the techniques and found upsampling minority class improves the model's generalization on unseen data. I the code below I upsample minority class with Scikit-learn 'resample' method.))中，我解释了这三种技术的细节。我还展示了如何逐步提高模型的性能。在这项工作中，我尝试了所有的技术，发现对少数类进行上采样可以提高模型对未知数据的泛化能力。因此，在本文中，我只关注重采样技术。

## 对少数民族类进行重采样

处理高度不平衡数据集的一种流行技术叫做重采样。虽然该技术在许多情况下被证明是有效的，以解决不平衡的阶级问题，但是，它也有其弱点。例如，来自少数类的过采样记录会导致过拟合，而从多数类中移除随机记录会导致信息丢失。在向上采样小类和向下采样大类中，我发现带有向上采样的模型在未知数据上表现得更好。下面的代码显示了这种机制:

```
df_major = df_selected[df_selected.loan_status == 0]
df_minor = df_selected[df_selected.loan_status == 1]df_minor_upsmapled = resample(df_minor, replace = True, n_samples = 358436, random_state = 2018)df_minor_upsmapled = pd.concat([df_minor_upsmapled, df_major])df_minor_upsmapled.loan_status.value_counts()1    358436
0    358436
Name: loan_status, dtype: int64
```

在上面的代码中，我首先将类分成两个数据帧:1。df_major 和 2。df_minor。然后，我使用 df_minor 将其向上采样到与主类相同的数字，即 358436。请注意，我将替换选项保持为 true。如果我被向下采样，那么我会保持替换选项为假。最后，我将上采样的 minor 类与 major 类连接起来。看看 loan_status 特性的类，它们现在很明显是平衡的。我建议以迭代的方式尝试不同的技术，直到找到更好的模型。

**数据标准化**

在本节中，我通过去除每个样本的平均值，然后除以标准偏差来重新调整数据。零均值和单位标准差有助于加快模型的优化。我使用了 Scikit-learn StandardScaler 方法。在此之前，我将数据集分为训练和测试部分。以下代码不言自明:

```
X = df_minor_upsmapled.drop('loan_status', axis = 1)
Y = df_minor_upsmapled.loan_statusxtrain, xtest, ytrain, ytest = train_test_split(X, Y, test_size=0.25, random_state=0)mms = StandardScaler()
mms.fit(xtrain)
xtrain_scaled = mms.transform(xtrain)
```

现在我们的数据已经准备好了，我继续下一步:构建模型。正如我之前所说，从逻辑回归这样的简单算法开始有助于保持事情的简单，并作为复杂模型的基准。

## 逻辑回归模型

逻辑回归是一种借用统计学的建模技术。这是第一个算法，应该从复杂模型开始，并不断向复杂模型发展。这种算法相对简单，易于实现，这就是为什么我总是首先从这种技术开始，并记录模型的性能，用于将来复杂模型的基准测试。它帮助我轻松直观地前进。好的，让我们看看逻辑回归是如何执行的:

```
logisticRegr = LogisticRegression()
logisticRegr.fit(xtrain_scaled, ytrain)LogisticRegression(C=1.0, class_weight=None, dual=False, fit_intercept=True, intercept_scaling=1, max_iter=100, multi_class='ovr', n_jobs=1, penalty='l2', random_state=None, solver='liblinear', tol=0.0001, verbose=0, warm_start=False)
```

在上面的代码中，我用默认的 LR 参数训练模型。下面，我使用训练数据中使用的相同标准化参数均值和标准差来标准化测试数据，然后预测测试数据。

```
xtest_scaled = mms.transform(xtest)
lr_pred = logisticRegr.predict(xtest_scaled)
```

为了查看测试数据的性能，我编写了一个名为“evaluate_model”的函数，它打印不同的评估标准:1)准确性，2) ROC-AUC 得分，3)混淆矩阵和 4)详细的分类报告。

最后，让我们看看逻辑回归的表现:

```
evaluate_model(ytest, lr_pred)Accuracy of the model: 0.66409066053633

Classification report: 
             precision    recall  f1-score   support

          0       0.66      0.68      0.67     89877
          1       0.67      0.65      0.66     89341

avg / total       0.66      0.66      0.66    179218

Confusion matrix: 
[[60846 29031]
 [31170 58171]]
```

上述预测并不是一个坏的开始，但并不令人印象深刻。模型的精度只是在随机猜测之上一点点。我们看到最简单的模型给出了 66%的准确度。因此，我们还有很多工作要做，以制造一个高效的模型。我们必须选择一个更好的算法并调整其超参数，以使模型优于逻辑回归模型。

如果我们使用下面的代码查看特征的唯一值，我们会看到几乎 60%的特征是分类的，其余的是连续的。显示器看 [GitHub jupyter 笔记本](https://github.com/msahamed/lending_loan_prediction/blob/master/eda.ipynb)。

```
features = pd.DataFrame([], columns = ['feature_name', 'unique_values'])
for i, col in enumerate(df_selected.columns):
    features.loc[i] = [col, len(np.unique(df_selected[col]))]features.sort_values('unique_values')
```

选择合适的模型是数据科学家面临的另一个挑战。有时，在尝试不同的算法之前，即使是经验丰富的数据科学家也无法判断哪种算法的性能最佳。在我们最终的数据集中，几乎 60%的特征是绝对的。在这种情况下，基于树的模型可能是更好的选择。尽管如此，还是很难预测。如果基于树的算法执行得不太好，我们可以尝试另一种算法，如神经网络。然而，在这个项目中，我将首先尝试基于树的集成技术:bagging(随机森林)和 boosting (LightGBM)算法。好吧，让我们从随机森林开始。

**随机森林(RF)模型**

随机森林是一种灵活且易于使用的机器学习集成算法。该算法是如此的轻巧和有效，即使没有超参数调整，它也可以产生很好的结果。它也是最常用的算法之一，因为它简单，而且可以用于分类和回归任务。方法的详细内容可以在 Scikit-Sklearn 网页上或者在这篇博文中找到:[https://towardsdatascience . com/the-random-forest-algorithm-d 457d 499 fcd。](/the-random-forest-algorithm-d457d499ffcd.)让我们开始建模:

在上面的函数中，我首先定义了超参数。随机森林的重要超参数是估计数和树的最大深度。我更喜欢迭代地寻找最优超参数。在这种情况下，我首先从少量估计量开始，然后慢慢增加。我发现这个手动过程比使用 GridSearchCV 或 RandomSearch 更有效、更直观。还有另一种称为贝叶斯超参数优化的技术，可以用来找到一组合适的超参数。这种方法似乎更有效率和效果。在我的下一个项目中，我会尝试另一个项目。关于该技术的更多细节可以在[威廉·科尔森](https://towardsdatascience.com/@williamkoehrsen?source=post_header_lockup)的博客文章“[机器学习贝叶斯超参数优化的概念解释](/a-conceptual-explanation-of-bayesian-model-based-hyperparameter-optimization-for-machine-learning-b8172278050f)”中找到。如前所述，我从最重要的超参数的一个小数值开始，一旦找到最佳值，我就从下一个有影响的超参数开始，依此类推。

好了，是时候看看随机森林在测试数据上的表现了:

```
evaluate_model(ytest, rfpred, rfpred_proba)ROC-AUC score of the model: 0.8054282761077389
Accuracy of the model: 0.7304177035788816

Classification report: 
             precision    recall  f1-score   support

          0       0.75      0.69      0.72     89877
          1       0.71      0.77      0.74     89341

avg / total       0.73      0.73      0.73    179218

Confusion matrix: 
[[61972 27905]
 [20409 68932]]
```

哇，拉多姆森林做得更好。几乎 11%的准确率来自逻辑回归，这是一个伟大的成就，证明了基于树的模型在分类数据上表现良好。

因为进一步调整超参数并不能提高模型性能，所以我停止了对随机森林模型的研究。我尝试了其他超参数并增加了 n_estimators，但没有帮助。这些困难帮助我决定使用梯度增强树。因为我不打算进一步使用随机森林，所以让我们检查一下模型的健壮性。找出交叉验证的一种方法。

**随机森林模型的交叉验证**

交叉验证是在实践中使用独立数据集评估模型及其泛化能力的有效方法之一。如果该模型在不同褶皱上的表现是一致的，那么我们可以说该模型是稳健的并且表现良好。在下文中，我们使用 Scikit-Sklearn 交叉验证方法测试 RF 模型的稳健性:

```
scoring = [‘accuracy’, ‘recall’, ‘roc_auc’, ‘f1’]scores = cross_validate(rf, X = xtrain_scaled, y = ytrain, scoring = scoring, cv = 10, return_train_score = False, verbose = 10, n_jobs= -1)
```

正如你在上面的代码中看到的，我使用了四个不同的评估指标(“准确性”、“回忆”、“roc_auc”、“f1”)来判断模型的泛化能力。让我们看看结果:

```
scores{'fit_time': array([519.01635194, 519.08185387, 518.91476393, 514.854949  ,
        491.86662292, 491.60002613, 492.63204885, 491.53150296,
        296.78954768, 297.04013801]),
 'score_time': array([10.45862293, 10.56293011, 10.65213013, 10.40467215, 10.21855617,
        10.32528472, 10.14684105, 10.33355498,  6.49080539,  6.19442701]),
 'test_accuracy': array([0.72979206, 0.7310196 , 0.73124279, 0.73332589, 0.73371648,
        0.72999163, 0.7343625 , 0.72937785, 0.73207477, 0.73201399]),
 'test_recall': array([0.76737272, 0.77194352, 0.77324415, 0.77840951, 0.77421033,
        0.77122896, 0.77728641, 0.77037422, 0.77174923, 0.77271545]),
 'test_roc_auc': array([0.80477504, 0.80651739, 0.80334897, 0.8090473 , 0.80837787,
        0.80377023, 0.80829186, 0.80413098, 0.80798087, 0.8082303 ]),
 'test_f1': array([0.73977216, 0.74178689, 0.74226804, 0.74502063, 0.74427079,
        0.74087428, 0.74548241, 0.74022496, 0.74248735, 0.74268672])}
```

上面的代码片段打印出了交叉验证的结果。如果您仔细观察不同的评估指标，您会发现该模型在各个褶皱中表现一致，因此非常健壮。让我们计算更具体的指标，如均值和方差:

```
print('F1 score# (1) mean: {} (2)variance: {}'.format(np.mean(scores['test_f1']), np.var(scores['test_f1'])))
print('Recall score# (1) mean: {} (2)variance: {}'.format(np.mean(scores['test_recall']), np.var(scores['test_recall'])))
print('Accuracy score# (1) mean: {} (2)variance: {}'.format(np.mean(scores['test_accuracy']), np.var(scores['test_accuracy'])))### mean and variance of the merics:
F1 score# (1) mean: 0.7424874224946193 (2)variance: 3.4239691671447294e-06
Recall score# (1) mean: 0.7728534498486367 (2)variance: 9.340496661280428e-06
Accuracy score# (1) mean: 0.7316917565649772 (2)variance: 2.6660110240196636e-06
```

很高兴看到每个评估指标都有非常低的方差，这再次证实了模型的稳健性。虽然模型很稳健，但我还不开心。我们需要提高模型性能。接下来，我将尝试一种基于梯度增强树的算法。有许多基于梯度增强树的算法可用，如 XGBoost、LightGBM、CataBoost 等。我发现 LightGBM 速度更快，在分类数据上表现良好。

**LightGBM 型号**

在上面的代码中，我首先以类似于我在随机森林模型中所做的方式手动找到最佳超参数。我发现模型中最重要的参数是“n 估计量”和“最大深度”。让我们看看模型对测试数据的预测和表现:

```
### Performance report
ROC-AUC score of the model: 0.9586191656898193
Accuracy of the model: 0.890546708477943

Classification report: 
             precision    recall  f1-score   support

          0       0.93      0.85      0.89     89877
          1       0.86      0.94      0.90     89341

avg / total       0.89      0.89      0.89    179218

Confusion matrix: 
[[75963 13914]
 [ 5702 83639]]
```

业绩报告似乎很有希望。逻辑回归的准确率提高了 35%，随机森林模型的准确率提高了 23%。我在这里停止优化其他超参数。我大概花了 4 个小时才找到以上两个超参数。现在我使用同样的技术交叉验证来关注模型的稳健性。

如果你看看下面的结果，你会看到 LightGBM 在不同的训练中表现一致，这正是我们想要建立的。我们到了。

```
--------------- cv: 1 --------------------
ROC-AUC score of the model: 0.9483670270426987
Accuracy of the model: 0.8754963684890869

Classification report: 
             precision    recall  f1-score   support

          0       0.91      0.83      0.87     53472
          1       0.84      0.92      0.88     54059

avg / total       0.88      0.88      0.88    107531

Confusion matrix: 
[[44284  9188]
 [ 4200 49859]]--------------- cv: 2 --------------------
ROC-AUC score of the model: 0.9478880521895745
Accuracy of the model: 0.8756730617217361

Classification report: 
             precision    recall  f1-score   support

          0       0.92      0.83      0.87     53861
          1       0.84      0.92      0.88     53670

avg / total       0.88      0.88      0.88    107531

Confusion matrix: 
[[44633  9228]
 [ 4141 49529]] --------------- cv: 3 --------------------
ROC-AUC score of the model: 0.9490374658331387
Accuracy of the model: 0.8780909691158829

Classification report: 
             precision    recall  f1-score   support

          0       0.92      0.83      0.87     53504
          1       0.85      0.92      0.88     54027

avg / total       0.88      0.88      0.88    107531

Confusion matrix: 
[[44457  9047]
 [ 4062 49965]]--------------- cv: 4 --------------------
ROC-AUC score of the model: 0.9490402272662238
Accuracy of the model: 0.8781188680473538

Classification report: 
             precision    recall  f1-score   support

          0       0.92      0.83      0.87     53958
          1       0.85      0.92      0.88     53573

avg / total       0.88      0.88      0.88    107531

Confusion matrix: 
[[44916  9042]
 [ 4064 49509]]--------------- cv: 5 --------------------
ROC-AUC score of the model: 0.9486846347287888
Accuracy of the model: 0.8762112898725937

Classification report: 
             precision    recall  f1-score   support

          0       0.91      0.83      0.87     53764
          1       0.84      0.92      0.88     53766

avg / total       0.88      0.88      0.88    107530

Confusion matrix: 
[[44671  9093]
 [ 4218 49548]]
```

在这个项目中，我没有讨论任何关于过度拟合的问题。希望我会写另一篇关于这个话题的博客。

非常感谢您的阅读。我希望你喜欢整个系列。完整的代码可以在 [Github](https://github.com/msahamed/lending_loan_prediction) 上找到。我很想收到你的来信。如果您发现我在代码中或任何地方犯了任何错误，请随时写下评论。如果你愿意，你可以联系我:

```
Email: sabbers@gmail.com
LinkedIn: [https://www.linkedin.com/in/sabber-ahamed/](https://www.linkedin.com/in/sabber-ahamed/)
Github: [https://github.com/msahamed](https://github.com/msahamed)
Medium: [https://medium.com/@sabber/](https://medium.com/@sabber/)
```