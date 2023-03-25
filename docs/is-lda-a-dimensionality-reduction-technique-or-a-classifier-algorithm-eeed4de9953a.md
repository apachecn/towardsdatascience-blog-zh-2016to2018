# LDA 是降维技术还是分类器算法？

> 原文：<https://towardsdatascience.com/is-lda-a-dimensionality-reduction-technique-or-a-classifier-algorithm-eeed4de9953a?source=collection_archive---------0----------------------->

# 介绍

在我的上一篇帖子中，我开始了一个关于维数减少的讨论，这是在执行分类任务([https://meigarom.github.io/blog/pca.html](https://meigarom.github.io/blog/pca.html))之前使用主成分分析(PCA)对结果的真正影响。在这篇文章中，我将继续讨论这个主题，但现在，谈论线性判别分析(LDA)算法。LDA 被作者定义为一种降维技术，然而一些来源解释说 LDA 实际上作为线性分类器工作。

为了更好地理解这些定义，我在这里提出了一个简单的测试:我将对同一个数据集应用 LDA 两次，每次使用不同角色的 LDA。通过这两种方法来衡量准确性，我们可以清楚地了解哪种角色最适合 LDA。因此，让我们开始吧。

# 关于 LDA 的一点点

LDA 背后的想法很简单。从数学上来说，我们需要找到一个新的特征空间来投影数据，以最大化类的可分性。显然，第一步是找到一种方法来测量每个新的候选特征空间的分离能力。每个类的投影平均值之间的距离可以是度量之一，但是只有这个距离不是一个很好的度量，因为它没有考虑数据的分布。1988 年，一位名叫罗纳德·费雪的统计学家提出了以下解决方案:最大化代表均值之间差异的函数，通过对类内变异性的测量进行标准化。Fisher 的建议基本上是最大化每个类的平均值之间的距离，并最小化类本身内的分布。因此，我们提出了两个衡量标准:阶级内部和阶级之间。然而，只有当我们假设数据集具有正态分布时，这种公式才是可能的。这种假设可能会带来一个缺点，因为如果数据的分布显著非高斯，LDA 可能不会表现得很好。在接下来的帖子中，我将深入研究 LDA 算法的代码，展示它的实现，在这里我关心的是找到最初问题的答案。LDA 是线性分类器还是降维技术？。

# 实验的细节

本文中提出的实验包括对公共数据集的分类任务。基本上，该数据集包含来自 3 种不同类型车辆的 846 个观察值和 18 个特征。这些特征对应于这些车辆形状的物理度量，例如紧密度、圆形度和半径。可以在这里查看数据集:[https://archive . ics . UCI . edu/ml/datasets/Statlog+(Vehicle+Silhouettes](https://archive.ics.uci.edu/ml/datasets/Statlog+(Vehicle+Silhouettes))。原始数据集带来了对 4 种车辆的观察，为了能够绘制新的特征空间，我将只从 3 个类别中选取示例。

# LDA 作为一种分类算法

正如我在开始时提到的，我将对同一个数据集应用 LDA 两次，每次使用不同的角色。在第一种方法中，LDA 将作为分类器工作，之后它将降低数据集的维度，神经网络将执行分类任务，之后将比较两种方法的结果。

在 R 语言中，LDA 作为分类器非常简单，如下所示:

```
**library**( dplyr )*# Load dataset*
data_raw = read.csv( "../dataset/vehicle.csv", stringsAsFactor = FALSE )data = data_raw %>% filter( class == "bus" | class == "opel" | class == "van" )*# Scale dataset*
maxs = apply( data[,1:18], 2, max )
mins = apply( data[,1:18], 2, min )
dataset = as.data.frame( scale( data[,1:18], center = mins, scale = maxs - mins ) )
dataset = cbind( dataset, "class" = data$class )*# Split dataset*
index = sample( 1:nrow( dataset ), round( nrow( dataset )*0.6 ), replace = FALSE )
X_train = dataset[ index, ]
test = dataset[ -index, ]
```

首先，我加载了数据集，只过滤了 bus、open 和 van 类中的行。后来，我使用标准技术缩放数据集，然后我将数据集分为训练集和测试集，分别包含 60%和 40%的示例。准备好数据集后，我们可以对训练数据集应用 LDA。

```
*# Model Discriminant Analysis*
**library**( MASS )
model = lda( class ~ ., data = X_train )*# Ploting LDA Model*
projected_data = as.matrix( X_train[, 1:18] ) %*% model$scaling
plot( projected_data, col = X_train[,19], pch = 19 )
```

LDA 使用 MASS R 库建模，它带来了一对模型参数，如组的先验概率、组均值和线性判别系数。这里最重要的结果是系数，它们是描述数据将被投影到的新特征空间的值。LDA 将维数从原始特征数减少到 C-1 个特征，其中 C 是类别数。在这种情况下，我们有 3 个类，因此新的特征空间将只有 2 个特征。上图是仅有两个要素的新要素空间的绘图，我们可以看到数据的新位置，三个类之间有一些点重叠，但总的来说，数据集是相当可分的。在训练阶段之后，我们需要测量所获得的模型的准确性。

```
*# Model Testing*
X_test = test[, !( names( test ) %in% c( "class" ) ) ]  
model.results = predict( model, X_test )*# Results - Confusion Matrix*
**library**( caret )
t = table( model.results$class, test$class )
print( confusionMatrix( t ) )## Confusion Matrix and Statistics
## 
##       
##        bus opel van
##   bus   87    5   0
##   opel   2   75   0
##   van    0    5  78
## 
## Overall Statistics
##                                                   
##                Accuracy : 0.952381                
##                  95% CI : (0.9182971, 0.9751557)  
##     No Information Rate : 0.3531746               
##     P-Value [Acc > NIR] : < 0.00000000000000022204
##                                                   
##                   Kappa : 0.9285056               
##  Mcnemar's Test P-Value : NA                      
## 
## Statistics by Class:
## 
##                      Class: bus Class: opel Class: van
## Sensitivity           0.9775281   0.8823529  1.0000000
## Specificity           0.9693252   0.9880240  0.9712644
## Pos Pred Value        0.9456522   0.9740260  0.9397590
## Neg Pred Value        0.9875000   0.9428571  1.0000000
## Prevalence            0.3531746   0.3373016  0.3095238
## Detection Rate        0.3452381   0.2976190  0.3095238
## Detection Prevalence  0.3650794   0.3055556  0.3293651
## Balanced Accuracy     0.9734266   0.9351884  0.9856322
```

第一种方法的最后一部分是在模型中应用测试数据集，并测量其结果。正如我们所看到的，LDA 作为一个分类器达到了大约 95%的准确率，这是一个非常好的结果。LDA 基本上将数据投影到新的线性特征空间中，显然，如果数据是线性可分的，分类器将达到高精度。现在我们已经知道了 LDA 像分类器一样工作的准确性，让我们检查第二种方法。

# LDA 作为一种降维算法

线性判别分析也作为一种降维算法工作，这意味着它将维数从原始的减少到 C-1 个特征，其中 C 是类的数目。在这个例子中，我们有 3 个类和 18 个特征，LDA 将从 18 个特征减少到只有 2 个特征。约简后，神经网络模型将应用于分类任务。这里的程序与上一个几乎相同。看一看它。

```
*# New Dataset*
new_X_train = as.matrix( X_train[,1:18] ) %*% model$scaling
new_X_train = as.data.frame( new_X_train )
new_X_train$class = X_train$class
```

我将使用在先前的训练阶段中已经定义的线性判别式的系数来将训练数据集投影到新的特征空间中，这个新的空间将是新的训练数据集。

```
*# Multi-Class -> Transforming The Labels*
new_X_train = cbind( new_X_train, opel = new_X_train$class == "opel" )
new_X_train = cbind( new_X_train, van = new_X_train$class == "van" )
new_X_train = cbind( new_X_train, bus = new_X_train$class == "bus" )
new_X_train = new_X_train[, !( names( new_X_train ) %in% c( "class" ) ) ]
```

在建模之前，我们需要转换数据集，以便能够将神经网络模型用作多类分类问题。修改包括为每个标签创建具有值“真”或“假”新列。例如，所有来自“opel”的示例将在“opel”列下获得“True”值，在其他标签列中获得“False”值。因此，数据集可以用于多类模型。

```
*# Model Neural Network*
**library**( neuralnet )
n = names( new_X_train )
f = as.formula( "opel+van+bus ~ LD1+LD2" )
nn = neuralnet( f, new_X_train, hidden = 3, linear.output = FALSE, lifesign = "full", 
                threshold = 0.02, stepmax = 1e6 )## hidden: 3    thresh: 0.02    rep: 1/1    steps:    1000  min thresh: 0.7307363799
##                                                    2000  min thresh: 0.7307363799
##                                                    3000  min thresh: 0.7307363799
##                                                    4000  min thresh: 0.6448965006
##                                                    5000  min thresh: 0.4485495524
##                                                    6000  min thresh: 0.3650681285
##                                                    7000  min thresh: 0.2756491212
##                                                    8000  min thresh: 0.2137706548
##                                                    9000  min thresh: 0.1995287711
##                                                   10000  min thresh: 0.173313487
##                                                   11000  min thresh: 0.173313487
##                                                   12000  min thresh: 0.173313487
##                                                   13000  min thresh: 0.173313487
##                                                   14000  min thresh: 0.1420902136
##                                                   15000  min thresh: 0.08689029484
##                                                   16000  min thresh: 0.08255271673
##                                                   17000  min thresh: 0.0690402594
##                                                   18000  min thresh: 0.04721150014
##                                                   19000  min thresh: 0.02719626607
##                                                   19149  error: 11.92918 time: 4.03 secs
```

神经网络模型开始非常简单，只有 3 个隐藏层，0.02 的均方误差阈值和 1e6 最大历元。这里的想法是在新的数据集上应用神经网络，并检查我们是否可以达到更好的结果。测试数据集将与前面方法中使用的相同。

```
*# Testing*
X_test = as.matrix( test[,1:18] ) %*% model$scaling
nn.results = compute( nn, X_test )*# Results*
print( "... resulting ..." )## [1] "... resulting ..."idx = apply( nn.results$net.result, c(1), **function**( x ){ which( x == max( x ) ) } )
predictions = c( "opel", "van", "bus")[idx]*# Confusion Matrix*
**library**( caret )
t = table( predictions, test$class )
print( confusionMatrix( t ) )## Confusion Matrix and Statistics
## 
##            
## predictions bus opel van
##        bus   84    3   1
##        opel   4   81   3
##        van    1    1  74
## 
## Overall Statistics
##                                                 
##                Accuracy : 0.9484127             
##                  95% CI : (0.9133996, 0.9722494)
##     No Information Rate : 0.3531746             
##     P-Value [Acc > NIR] : < 0.00000000000000022 
##                                                 
##                   Kappa : 0.9224872             
##  Mcnemar's Test P-Value : 0.7667396             
## 
## Statistics by Class:
## 
##                      Class: bus Class: opel Class: van
## Sensitivity           0.9438202   0.9529412  0.9487179
## Specificity           0.9754601   0.9580838  0.9885057
## Pos Pred Value        0.9545455   0.9204545  0.9736842
## Neg Pred Value        0.9695122   0.9756098  0.9772727
## Prevalence            0.3531746   0.3373016  0.3095238
## Detection Rate        0.3333333   0.3214286  0.2936508
## Detection Prevalence  0.3492063   0.3492063  0.3015873
## Balanced Accuracy     0.9596402   0.9555125  0.9686118
```

神经网络模型在应用于具有降维的数据集时达到了大约 93%的准确度。这个结果非常非常接近第一种方法的结果。这是 LDA 最佳工作模式的重要线索。

# 两种方法的结果

嗯，我想我们完成了实验。奇怪的是，两个结果非常接近，就准确性而言，这表明 LDA 在两种模式下都能很好地工作。比较实现的难度，第一种方法比第二种方法更简单，LDA 模型自己提供分类结果，而在第二种方法中，我们有更多的代码，因为 LDA 只是降低维度，而神经网络模型负责执行分类任务。

# 结论

写这篇文章的动机问题是:LDA 是一种降维技术还是一种分类器？在我看来，基于实现的结果和努力，答案是 LDA 在两种模式下都工作得很好，在分类器模式和降维模式下也是如此，我会给你支持这个结论的论据。

首先，您正在处理的数据集的特征将指导您决定是应用 LDA 作为分类器还是降维算法来执行分类任务。线性判别分析的主要内容基本上是将类别的独立样本线性移动到不同的特征空间，因此，如果您的数据集是线性可分的，只应用 LDA 作为分类器，您将获得很好的结果。然而，如果数据集不是线性可分的，LDA 将尝试在另一个空间中尽可能最大程度地组织数据集的线性可分性，但由于数据的非线性特征，它仍然是类之间重叠的例子。在这种情况下，您将需要使用另一种分类模型来处理非线性数据，如具有多个隐藏层的神经网络、具有径向基函数的神经网络或具有非线性核的 SVM。

总之，LDA 模式的决定将取决于您手中的数据集，如果它是线性可分的，LDA 作为分类器是一种快速的解决方案，具有很好的结果，但如果数据集的特征是非线性的，LDA 将是应用于数据集的额外工具，以便尝试“使事情变得更好”或促进后分类器的工作。

我看过一些这方面的作品，作者将 PCA 和 LDA 结合起来，都是为了降维。首先，主成分分析通过其方差进行特征约简，然后 LDA 应用线性维数约简，最后在这个修改的数据集上执行分类器模型。为了改善分类任务的结果，这绝对是一个要遵循的路线图。

# 谢谢！

完整的代码可以在我的 git hub 仓库和数据集上找到。

感谢你花时间阅读这篇文章。我真的很感激，随时欢迎反馈。

回头再聊。