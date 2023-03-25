# 我的第一个机器学习项目 PT1

> 原文：<https://towardsdatascience.com/my-1st-machine-learning-project-pt1-2f4368f63f52?source=collection_archive---------11----------------------->

这一刻终于到来了

![](img/fba528881dea32654e16d02aa73c2f1b.png)

[WOCinTechChat](https://www.flickr.com/photos/wocintechchat/25497596540/in/photostream/)

大家好！希望你们都过得好。我正在兑现我在[上一篇文章](https://medium.com/the-data-logs/machine-learning-as-explained-by-a-beginner-dd5b8119a45)中的承诺。今天我们将关注线性回归。数据集是关于国王郡地区 2014 年 5 月至 2015 年 5 月间出售的房屋。你可以在 [Kaggle](https://www.kaggle.com/harlfoxem/housesalesprediction) 上自己下载。这是一个寻找数据集和练习机器学习的好地方。如果你和我一样兴奋，让我们开始吧。

## 线性回归

线性回归是一种使用变量(或机器学习的特征)来预测定量变量(目标变量)的算法。更具体地说，它试图拟合一条穿过数据的直线，该直线最好地表示预测变量和目标变量之间的关系。

想起我在上一篇帖子里举的工资例子。你试图根据某人多年的工作经验来预测你的薪水。工资是目标变量，工作年限是预测变量。这个数据集的目标变量是房屋的价格。我们的预测者呢？我们把这个装进去，看看有什么发现。他们所代表的东西可以在[这里](https://rstudio-pubs-static.s3.amazonaws.com/155304_cc51f448116744069664b35e7762999f.html)找到。

```
Housing_df <- read.csv("kc_house_data.csv")
str(Housing_df)
'data.frame': 21613 obs. of  21 variables:

$ id           : num  7.13e+09 6.41e+09 5.63e+09 2.49e+09 1.95e+09 ...

$ date         : Factor w/ 372 levels "20140502T000000",..: 165 221 291 221 284 11 57 252 340 306 ...

$ price        : num  221900 538000 180000 604000 510000 ...
 $ bedrooms     : int  3 3 2 4 3 4 3 3 3 3 ...
 $ bathrooms    : num  1 2.25 1 3 2 4.5 2.25 1.5 1 2.5 ...

$ sqft_living  : int  1180 2570 770 1960 1680 5420 1715 1060 1780 1890 ...

$ sqft_lot     : int  5650 7242 10000 5000 8080 101930 6819 9711   7470 6560 ...

 $ floors       : num  1 2 1 1 1 1 2 1 1 2 ...
 $ waterfront   : int  0 0 0 0 0 0 0 0 0 0 ...
 $ view         : int  0 0 0 0 0 0 0 0 0 0 ...
 $ condition    : int  3 3 3 5 3 3 3 3 3 3 ...
 $ grade        : int  7 7 6 7 8 11 7 7 7 7 ...
 $ sqft_above   : int  1180 2170 770 1050 1680 3890 1715 1060 1050 1890 ...

$ sqft_basement: int  0 400 0 910 0 1530 0 0 730 0 ...

$ yr_built     : int  1955 1951 1933 1965 1987 2001 1995 1963 1960 2003 ...

$ yr_renovated : int  0 1991 0 0 0 0 0 0 0 0 ...

$ zipcode      : int  98178 98125 98028 98136 98074 98053 98003 98198 98146 98038 ...

$ lat          : num  47.5 47.7 47.7 47.5 47.6 ...

$ long         : num  -122 -122 -122 -122 -122 ...

$ sqft_living15: int  1340 1690 2720 1360 1800 4760 2238 1650 1780 2390 ...

$ sqft_lot15   : int  5650 7639 8062 5000 7503 101930 6819 9711 8113 7570 ...
```

不算价格，我们总共有 20 个可能的预测变量。如果只选一个，那就叫简单线性回归。如果我们选择 1 以上，多元线性回归。很简单，不是吗？现在我们如何选择我们应该使用的变量呢？一种方法是使用所有的东西来建立一个模型，根据一个特定的标准来评估这个模型，然后一个接一个地移除每个预测因子。我们将执行的另一个方法是对每个预测值进行逻辑思考，看看创建模型和做出好的预测是否有统计学意义。更具体地说，我们将保留本质上是有序的变量。

所以现在让我们开始移除一些。ID & zipcode 可以走了。它们都是标签，不会从数字上告诉你一个数字是否比另一个好。id 2346 并不比 2342 好。所以现在我们只剩 18 个了。接下来，我们将删除纬度和经度。这些只是地图上房子的坐标。对于线性回归模型，它们是不需要的。

其他 16 个暂时可以接受。尽管我们需要做些调整。如果您查看日期变量，您会注意到它不是最易读的。最重要的是，它也不是定量的。所以我们将它转换成数字:

```
#Converting to regular text first
Housing_df$date<- as.Date(as.Date(as.character(Housing_df$date),"%Y%m%d"))#Now converting to number
Housing_df$date <- as.numeric(Housing_df$date)
```

太棒了。我们现在准备进入机器学习的下一步。

## 训练和测试集

在机器学习中，你将数据分成两组:训练集和测试集。定型集是您的模型从中学习的数据部分。测试集用于查看您的模型对以前没有见过的数据的预测效果。为什么不用整个数据集来代替呢？如果你这样做了，你的模型在预测新值方面就不会做得很好，因为它捕捉了特定数据集的模式。这么看吧。你通过模拟考试来准备考试。你只研究你在课堂上得到的答案和问题出现的顺序，而不理解概念。当真正的考试以随机顺序出现问题时，你不会做得很好。

我们要做的第一件事是考虑有多少数据将进入训练集，哪些数据将留给测试集。通常越多越好，所以我会把数据分成 80-20 份训练测试。

```
#Split data into training and test setslibrary(caTools)set.seed(1234)
split <- sample.split(Housing_df$price, SplitRatio = 0.8)
training_set <- subset(Housing_df, split == T)
test_set <- subset(Housing_df, split == F)
```

已经完成了。训练集包含 17，839 个观察值，测试集将包含 3，774 个观察值。如果你对布景有疑问，seed 是为团队工作准备的。当一个数据集被分割时，最终出现在一个数据集和另一个数据集之间的观察值是随机的。由于一个随机数发生器。当你设置了一个种子，你就说明了 rng 从哪里开始。这使得您的工作具有可重复性，这在数据科学中非常重要。所以如果你的同事想评价你的工作，或者想看看你哪里出了问题，只要输入相同的种子，他们就可以下载相同的数据集，得到和你一样的观察结果。

现在我们开始做好事了。我们要建立一个线性回归模型！终于！

## 模型结构

线性回归模型基本上是一个数学公式的表示。不要担心，我们不会在第 1 部分得到超级数学；只是想给你提个醒。这就是我们将如何建立这个模型。

```
model1 <- lm(formula = price ~ date + bedrooms + bathrooms + sqft_living + sqft_lot + floors + waterfront + view + condition + grade + sqft_above + sqft_basement + yr_built + yr_renovated + sqft_living15 +  sqft_lot15, data = training_set )
```

首先，我们使用 lm 函数让我们知道我们在构建什么样的算法。价格是我们的目标变量，所以我们首先列出它。在~之后，我们列出所有我们想用来预测的变量。最后，让我们陈述我们用来构建模型的数据集。如果我们想看看它是什么样子:

```
Call:
lm(formula = price ~ date + bedrooms + bathrooms + sqft_living + 
    sqft_lot + floors + waterfront + view + condition + grade + 
    sqft_above + sqft_basement + yr_built + yr_renovated + sqft_living15 + 
    sqft_lot15, data = trainig_set)Residuals:
     Min       1Q   Median       3Q      Max 
-1415203  -110571   -10066    92170  4203618Coefficients: (1 not defined because of singularities)
                Estimate Std. Error t value Pr(>|t|)    
(Intercept)    4.687e+06  2.860e+05  16.384  < 2e-16 ***
date           1.027e+02  1.463e+01   7.017 2.34e-12 ***
bedrooms      -4.568e+04  2.354e+03 -19.406  < 2e-16 ***
bathrooms      4.805e+04  3.893e+03  12.342  < 2e-16 ***
sqft_living    1.707e+02  5.195e+00  32.859  < 2e-16 ***
sqft_lot      -6.375e-03  5.472e-02  -0.117  0.90725    
floors         2.323e+04  4.246e+03   5.470 4.56e-08 ***
waterfront     6.149e+05  2.042e+04  30.116  < 2e-16 ***
view           4.267e+04  2.534e+03  16.840  < 2e-16 ***
condition      1.979e+04  2.824e+03   7.009 2.49e-12 ***
grade          1.226e+05  2.515e+03  48.748  < 2e-16 ***
sqft_above    -1.207e+00  5.074e+00  -0.238  0.81199    
sqft_basement         NA         NA      NA       NA    
yr_built      -3.663e+03  7.966e+01 -45.989  < 2e-16 ***
yr_renovated   1.346e+01  4.408e+00   3.053  0.00227 ** 
sqft_living15  2.293e+01  4.045e+00   5.669 1.46e-08 ***
sqft_lot15    -6.312e-01  8.790e-02  -7.181 7.22e-13 ***
---
Signif. codes:  0 ‘***’ 0.001 ‘**’ 0.01 ‘*’ 0.05 ‘.’ 0.1 ‘ ’ 1Residual standard error: 220400 on 17823 degrees of freedom
Multiple R-squared:  0.6607, Adjusted R-squared:  0.6604 
F-statistic:  2314 on 15 and 17823 DF,  p-value: < 2.2e-16
```

呜！我们的模型建成了！但是这些数字是怎么回事？星星呢？模型有多好？这些是我们将在第 2 部分中回答的问题。我现在不想打击你们。

如果你学到了新的东西，并准备好第 2 部分，点击推荐按钮！