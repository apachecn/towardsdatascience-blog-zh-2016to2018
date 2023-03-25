# 数据科学的音乐

> 原文：<https://towardsdatascience.com/the-music-of-data-science-943bf6f14e16?source=collection_archive---------9----------------------->

![](img/edbb5684d50859d12b5a21e0db74e8cb.png)

Photo by: Jorge Guillen

我学到的一件事——或者更确切地说，被提醒的一件事——是统计学就像乐谱上的音乐。就像只用七个音符的音乐家一样，统计学家更喜欢用很少的字母来代表变量、常数和函数。他们都选择了大量难以复制的符号来装饰这几个选定的字母。

当我们将这些统计函数转换成 Python 时，我们发现有许多方法可以使用统计数据的复杂组合。首先，我们必须选择仪器或库。以下是其中的一些库:

[**Numpy**](http://www.numpy.org) **和**[**Math**](https://docs.python.org/2/library/math.html#module-math)**:**Numpy 是一个重要的库，许多其他库都基于它，比如 Pandas 和 Scipy。对于统计，我们可以使用这个清单中的字面函数来重新创建统计函数，如标准差或简单的线性回归。

这个简单的线性回归表达式(针对房屋的预测价格):

```
*β0= y_bar - β1*x_bar                    #y_bar is mean of prices
                                        #x_bar is mean of square ft**β1= r(sqft, price)*(std price/std sqft) #r is Pearson's r value* Y = *β0 + β1X                            #Predicted price*
```

变成了这样:

```
import numpy as npdef beta0_beta1(x_list, y_list):
    x_list_mean = np.mean(x_list)
    y_list_mean = np.mean(y_list)
    x_list_std = np.std(x_list)
    y_list_std = np.std(y_list)
    beta_one = (np.corrcoef(x_list, y_list)[0,1])*.    
               (y_list_std/x_list_std)
    beta_naught = y_list_mean — (beta_one * np.mean(x_list)
return (beta_one, beta_naught)X = beta0_beta1(X_list_sqft, Y_list_price)SLR = beta_naught + (beta_one * X) 
#Simple linear regression model. 
```

作为初学者，在代码中“看到”这些函数的每一步，有助于理解其他更复杂的建模技术是如何在源代码中编写的。

Numpy 和弹**钢琴**一样容易，一样难。你只需触摸琴键就可以发出声音，但只有理解了你想要弹奏的“音符”(即理解了数学)，它才会产生真正的音乐。

[**stats models**](http://statsmodels.sourceforge.net)**:**这个名副其实的库是一个统计极客的工具箱。我们可以调用我们想要使用的函数，比如普通的最小二乘法和其他统计函数。我喜欢 StatsModels 的主要原因是`model.summary()`函数，它有点像熊猫探索性数据分析的`dataframe.describe()`:精彩、清晰、充满有用信息。此外，它还有用于练习和测试的数据集。

普通最小二乘模型:

```
import statsmodels.api as sm#Set up data
X = housing[“sq__ft”]         # X usually means ind variables
y = housing[“price”]          # Y usually means output
X = sm.add_constant(X)        # add intercept (beta_0) to model#Fit the model
model = sm.OLS(y, X).fit()    # OLS is order of least squares
sm.OLS(output, input).fit()   #(fit relationship between beta0 hat  
                                and beta1 hat)#take in X (input) and run the model to get predictions
predictions = model.predict(X)# Print out the statistics
model.summary()
```

在这里，`model.summary()`将输出系数、R2 分数、p 值等。

StatsModels 有点像**小提琴**——同样，发出声音并不难——但如果演奏者缺乏对音乐和乐器本身的理解，那声音绝对可怕。也就是说，由于这些模型“在引擎盖下”有更多的功能，新手可能很容易做出可怕的预测，而不知道如何或为什么。

[**scikit learn**](http://scikit-learn.org)**:**这个库显然是高级的。创建代码的开发人员显然试图优化我们当中那些想要研究需要计算效率的机器学习技术的数据科学家。像 StatsModels 一样，它也有许多用于测试和实践的数据集。

预测工资的多元线性回归:

```
from sklearn import linear_model
from sklearn.metrics import r2_scoreX = subset[['years_experience',          #Choose the variables 
            'years_since_degree']]       
y = subset['salary']                     #Set the target for  
                                          predictionlm = linear_model.LinearRegression()     #Call a function that 
model = lm.fit(X, y)                      runs MLR and fit it to  
                                          data
predictions = model.predict(X)           #Look at predictions (and 
                                          probably use it to plot)
print "Model Score:", model.score(X, y)  #What's the R2? 
```

我非常尊重这个工具包，就像一把**斯特拉迪瓦里琴**一样，这个乐器的极限显然是为专业人士设计的。

关于如何使用各种库来创建模型，以及哪些库更适合不同的用途，有很多讨论。现在，我弹得越来越好，越来越轻松，但我迫不及待地想尝试用斯特拉迪瓦里小提琴演奏维瓦尔第。