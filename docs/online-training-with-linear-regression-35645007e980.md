# 线性回归在线学习

> 原文：<https://towardsdatascience.com/online-training-with-linear-regression-35645007e980?source=collection_archive---------4----------------------->

使用 Python 实现线性回归和梯度下降在线学习的简单方法。

![](img/7214f8cfdab31ae9becf82633be0a542.png)

Source [http://tomtunguz.com/key-ingredient-machine-learning](http://tomtunguz.com/key-ingredient-machine-learning/)

你有没有想过，机器学习应用程序如何在少数情况下实时给出结果？机器学习从业者如何在容纳新样本/或训练数据的同时，一次又一次地训练大数据？如果是的话，这里有一个幼稚的指南。我将解释一些关于我们如何使用 Python 实现在线学习机器学习算法的基本概念。

**下面是 python 中源代码的 Github 链接和一个样本数据集，按照 Coursera 吴恩达机器学习课程给出的问题集**。

[](https://github.com/bmonikraj/linear-regression-scratch/blob/master/LinearGD.py) [## bmonikraj/线性回归-擦除

### 这是一个从头开始线性回归的 Python 代码，带有梯度下降实现和…

github.com](https://github.com/bmonikraj/linear-regression-scratch/blob/master/LinearGD.py) 

大多数机器学习应用程序遵循以下方法。这是一个迭代过程，每一步都可能根据需要执行。这三个基本步骤是

1.  数据收集和预处理
2.  数据集上的计算
3.  可视化和分析

*在本文中，我将主要关注上面提到的第二个子模块——数据集上的* ***计算。***

现在，为了更好地理解，我将用代码同时解释这些概念。

**在线训练** :-这是一种学习步骤的改进方法，其中所用模型的权重根据新到达的数据进行更新，而不是重新训练整个模型。这基本上是一种循序渐进的学习方法。这种训练的应用主要可以在实时应用中找到，实时应用为此目的使用预测或分类模型。

```
import pandas as pd
import numpy as np 
data = pd.read_csv("path_to_file.csv")
Y = data["Name of the Column"]
del data["Profit"]
bias = pd.Series(1,index=range(len(Y))) 
data["Bias"] = bias
Header_X_Bias = list(data.columns.values)
Header_X_Bias = Header_X_Bias[:-1]
Header_X_Bias.insert(0,"Bias")
data = data[Header_X_Bias] 
X = np.array(data)
Y = np.array(Y)
Y = Y.reshape(len(Y),1)
Theta = [0,0]
Theta = np.array(Theta)
Theta = Theta.reshape(2,1)#Here Theta is the global variable
```

我们将使用 Pandas 库从文件中读取数据集，使用 NumPy 模块执行线性代数计算。

在导入模块之后，我将数据集读入“数据”变量。从那里，我分离出了数据集的因变量“y”(在我们的例子中是“利润”(更多细节请参考 GitHub 参考资料))，并在 X 集中添加了一个值为 1 的 Bias 列。给定的数据集只有一个要素。

> y:利润
> 
> X : Xi 是 X 矩阵的第 I 行向量(X 的维数是‘m’X ^ 2 ),其中 Xi 的维数是 2。
> 
> X[0]:偏置；X[1]:原始单一特征

对于 numpy 计算，X 和 Y 都以适当的矩阵格式被重新整形，并且初始θ也被声明为值[0，0]，因为特征维数是 2，包括偏差。

```
def cost(X,Y,Theta):    
     Hypothesis = np.dot(X,Theta)    
     Error = Y - Hypothesis    
     #Matrix method for calculating Cost    
     Cost = np.dot(Error.T,Error)/(2*len(Error))    
     return Cost[0][0]
```

然后，定义代价函数，它以三个参数作为输入，第一个是 X，第二个是 Y，第三个是θ。在函数内部，它首先计算假设，然后以矢量化的方式计算成本。

```
Let 
m = Total number of training examples
d = Total number of features including bias = 2
Dimension of X : (m,d)
Dimension of Y : (m,1)
Dimension of Theta : (d,1)
Dimension of Loss : (m,1)
```

*注意:总是建议尽可能使用矢量化形式的计算，而不是循环，使用像 numpy 这样的线性代数库，以便大大优化计算时间。*

```
alpha = 0.01
Iterations = 1500
```

“alpha”和“迭代”步骤被声明为常量。

在算法中，我们把迭代次数作为收敛的决定因素。你也可以把θ和ε阈值的变化作为收敛的决定因素。

```
def gradient(X,Y,Theta,Iterations,alpha):    
     for i in range(Iterations):        
          Loss = Y - np.dot(X,Theta) + (np.dot(Theta.T,Theta)*0.001)
          Cost = cost(X,Y,Theta)        
          Loss = Loss*(-1)        
          dJ = (np.dot(X.T,Loss)*2)/len(Y)
          Theta = Theta - (alpha*dJ)
     return Theta
```

上面的代码片段是梯度下降函数的定义。它以 X，Y，Theta，迭代次数和 alpha 为参数。

它循环到“迭代”步骤，并且在每一步中，它根据成本值修改它的θ值。在每个循环中，都要计算损耗。

> 损失= Y 假设
> 
> (np.dot(Theta。t，Theta)*0.001) =这是正则项
> 
> 假设=点积(X，θ)

计算损失后，在每个循环中，每个参数的偏导数(对应于特征的θ值)以矢量化格式计算如下

> (2 *点积(X.T，损失))/m

最后θ被更新为

> θ=θ-(α*偏导数)

在上述类似的过程中，θ是更新“迭代”次数，梯度函数返回最终更新的θ。

```
Theta = gradient(X,Y,Theta,Iterations,alpha)#Here Theta is the global variable
```

到目前为止，我们已经得到了一种计算更新后的θ的方法，其中考虑了损失和迭代次数。

现在我们的任务是拥有在线学习的功能。基本上，在线学习是一个改进的中间步骤，我们只更新“K”次损失的权重，认为“K”是 x 中没有的新数据。

因为，我们将使用从梯度函数中获得的更新和训练的θ，在每个在线学习实例中，θ将总是更新的。下面我将向你展示一个定义在线学习功能的代码片段。

```
def online_train(Xn, Yn, Theta):
     Loss = Yn - np.dot(Xn,Theta) + (np.dot(Theta.T,Theta)*0.001)
     Loss = Loss*(-1)        
     dJ = (np.dot(Xn.T,Loss)*2)/len(Y)
     Theta = Theta - (alpha*dJ)
     return Theta
```

online_train 函数类似于梯度函数，只是差别不大。区别在于传递给函数的θ。在梯度函数的情况下，传递的θ是随机初始化值[0，0]，而在 online_train 函数的情况下，θ是从梯度函数返回的预训练的θ。

```
K = Total number of new data (Generally K = 1)
Dimension of Xn : (K,d)
Dimension of Yn : (K,1)
```

每当有新数据到来时，就会调用 online_train 函数，比如

```
#On Arrival a new single data vector
Theta = online_train(X_new, Y_new, Theta)
#X_new : new data vector
#Y_new : Y value corresponding to X_new
```

对于预测，使用θ，可以预测测试数据的值。

> 在线学习最好的部分是它非常适应新的训练集，并且它可以基于新的训练集更新 Theta，甚至不需要重新训练整个数据集。

在线学习的一个主要优点是，在实时应用程序中实现时，它非常节省空间。在第一个训练阶段之后，可以丢弃 X 和 Y，因为我们已经得到了θ，它可以在将来用于进一步的计算。

此外，在线学习也可以用于完整的训练，每次在循环中传递单个数据向量，并在每个循环步骤中更新 Theta(权重)。通过这种方式，可以可视化收敛，并且如果感觉收敛已经实现，可以停止迭代以实现计算优化。该方法在某种程度上类似于神经网络概念。

如果你听说过在线学习，那么我想你一定也听说过批量培训。批量训练是在线学习的修订版，其中*‘K’>1*，主要是*‘K’=批量*，这取决于*‘m’(训练集中的观察次数)*。两种算法在收敛期间访问相同的最小值点，只是它们在训练数据样本的不同点更新它们的θ。

如果我们列出在线学习的好处

*   这对于空间优化非常有用，因为空间是一个问题。
*   它可以在实时应用中使用，其中模型应该适应新的数据点。
*   在线学习最重要的应用可以在*网络应用*中找到，这些应用基于*预测建模、分类和推荐系统的原理。*

随着对机器学习算法的更好理解，并深入研究权重更新的数学概念，如梯度下降、反向传播、 [adam](http://ruder.io/optimizing-gradient-descent/index.html#adam) 等，人们可以很容易地设计出针对特定机器学习算法的在线训练解决方案。关键是以尽可能好的方式从数学上理解算法。

> **在线学习的进步可以为在产品中实时实现机器学习方法带来巨大的转变，并为产品的用户提供更好的用户体验。**最后，**向着理想的人工智能只差一步。**

进一步阅读

*   优化者—[http://ruder.io/optimizing-gradient-descent/index.html](http://ruder.io/optimizing-gradient-descent/index.html)
*   在线学习—[https://www . coursera . org/learn/machine-learning/lecture/ABO2q/online-learning](https://www.coursera.org/learn/machine-learning/lecture/ABO2q/online-learning)