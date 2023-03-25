# 使用 Python 中的 Keras 在流失建模数据集上从头开始构建您自己的人工神经网络

> 原文：<https://towardsdatascience.com/building-your-own-artificial-neural-network-from-scratch-on-churn-modeling-dataset-using-keras-in-690782f7d051?source=collection_archive---------1----------------------->

## 用神经网络的力量留住你的客户群！

众所周知，留住客户是许多公司的头等大事；获得新客户的成本可能是留住现有客户的好几倍。此外，了解客户流失的原因和估计与单个客户相关的风险都是设计数据驱动的保留策略的重要组成部分。流失模型可以是一种工具，它将这些元素整合在一起，并提供洞察和输出，推动整个组织的决策制定。因此，它被用来预测哪些客户将离开银行。

# 什么是流失率？

**流失率**(有时也称为**损耗率**)，从最广泛的意义上来说，是对特定时期内从一个集体群体中移出的个人或物品数量的衡量。这是决定一个企业将支持的客户的[稳态](https://en.wikipedia.org/wiki/Steady_state)水平的两个主要因素之一。

# 那么这篇文章的读者是谁呢？

你可以是刚刚完成机器学习 MOOC 的人，也可以是正在寻找基于机器学习的更实用/动手项目的初露头角的数据科学家。

如果你是经济学专业的学生，试图将数据科学的各个方面与你所在领域相关的东西结合起来，这也可能是相关的。

# 我们还在等什么？我们开始吧！！

> 因此，在这个数据集中，我们将处理客户流失建模，也就是说，我们将编写一个人工神经网络来找出客户实际离开银行的原因以及他们之间的依赖关系。
> 这是一个分类问题，即 0-1 分类(如果客户离开，1；如果客户留下，0)
> 我们可能会使用 Theano 或 Tensorflow，但问题是这些库要求我们从头开始编写大部分 Ml 代码，因此我们使用 Keras，这使我们能够编写强大的神经网络，只需几行代码 Keras 在 Theano 和 Tensorflow 上运行，您可以将其视为深度学习的 Sklearn。

# 第 1 部分—数据预处理

```
# Importing the libraries
*import* numpy *as* np
*import* matplotlib.pyplot *as* plt
*import* pandas *as* pd
```

## 导入数据集

将它保存在 Pycharm 项目/项目名称中，并使用 pandas 加载它。

```
dataset = pd.read_csv('Churn_Modelling.csv') 
```

![](img/49194d783846daa3a4318550099210a4.png)

从这些特征中我们可以看出，行号、姓名与客户离开银行没有关系。
所以我们从 X 中删除它们，X 包含从 3 到 12 的特征索引。

```
X = dataset.iloc[:, 3:13].values
```

我们通过在变量 y 中存储第 13 个索引**来存储 y 中的相关值/预测值**

```
y = dataset.iloc[:, 13].values
```

打印出…的值

包含特性的 x→y→包含目标变量

```
print(X)
print(y)
```

**编码分类数据…**
现在我们将特征中的字符串值编码为数值，因为 ML 算法只能处理数字，而不能处理字符串值。
仅有的两个值是性别和地区，需要转换成数字数据

```
*from* sklearn.preprocessing *import* LabelEncoder, OneHotEncoder
```

创建 1 号标签编码器对象以编码区域名称(功能中的索引 1)

```
labelencoder_X_1 = LabelEncoder()
```

编码区从字符串到 3 个编号分别为 0，1，2

```
X[:, 1] = labelencoder_X_1.fit_transform(X[:, 1])
```

创建 2 号标签编码器对象以编码性别名称(特征中的索引 2)

```
labelencoder_X_2 = LabelEncoder()
```

将性别从字符串分别编码为 2 个 no.s，1(男，女)

```
X[:, 2] = labelencoder_X_2.fit_transform(X[:, 2])
```

**现在创建虚拟变量**

```
onehotencoder = OneHotEncoder(categorical_features = [1])
X = onehotencoder.fit_transform(X).toarray()
X = X[:, 1:]
```

将数据集分为训练集和测试集

```
*from* sklearn.model_selection *import* train_test_split
X_train, X_test, y_train, y_test = train_test_split(X, y, test_size = 0.2, random_state = 0)
```

**特征缩放**

```
*from* sklearn.preprocessing *import* StandardScaler
sc = StandardScaler()
X_train = sc.fit_transform(X_train)
X_test = sc.transform(X_test)
```

# 第二部分——现在让我们制作人工神经网络！

> 列出了用随机梯度下降法训练 ANN 的步骤:-
> 1)随机地将权重初始化为接近 0(但不是 0)的小数字。
> 2)在输入层中输入数据集的第一个观察值，每个要素在一个输入节点中。
> 3)从左到右的前向传播，神经元被激活的方式影响每个神经元的激活。
> 受权重限制。传播激活，直到获得预测结果 y。
> 4)将预测结果与实际结果进行比较。测量产生的误差。
> 5)反向传播:从右到左，误差反向传播。根据权重对误差的影响程度来更新权重。学习率告诉我们更新权重的程度。
> 6)重复步骤 1 至 5，并在每次观察后更新权重(强化学习)。
> 或者:重复步骤 1 至 5，但仅在一批观察(批量学习)后更新权重。7)当整个训练集通过人工神经网络时。这完成了一个时代。重做更多的纪元。

## 导入 Keras 库和包

```
*import* keras
```

为了一层一层地建立神经网络

```
*from* keras.models *import* Sequential
```

将权重随机初始化为接近 0(但不是 0)的小数字

```
*from* keras.layers *import* Dense
```

## 正在初始化人工神经网络…

因此，实际上有两种方式来初始化深度学习模型
— — — 1)逐个定义每一层
— — — 2)定义一个图

我们没有在顺序对象中放入任何参数，因为我们将手动定义层

```
classifier = Sequential()
```

## 添加输入层和第一个隐藏层…

这仍然是一个没有答案的问题，直到今天，我们实际上需要多少隐藏层的节点？
没有经验法则，但是您可以将隐藏层中的节点数分别设置为输入和输出层中节点数的平均值。(在 90%的情况下有效！！)
——→此处 avg= (11+1)/2== > 6 因此设置输出 Dim = 6
——→Init 将统一初始化隐藏层权重
——→激活函数是整流器激活函数(Relu)

Input dim 告诉我们输入层中的节点数。这是只做一次，不会被指定在进一步的层。

```
classifier.add(Dense(output_dim = 6, init = 'uniform', activation = 'relu', input_dim = 11))
```

## 添加第二个隐藏层…

```
classifier.add(Dense(output_dim = 6, init = 'uniform', activation = 'relu'))
```

## 添加输出层…

```
classifier.add(Dense(output_dim = 1, init = 'uniform', activation = 'sigmoid'))
```

当我们需要 2 个或更少类别的概率时，使用 Sigmoid 激活函数(类似于逻辑回归)
当因变量超过 2 个类别时，切换到 Softmax。

## 正在编译人工神经网络…

```
classifier.compile(optimizer = 'adam', loss = 'binary_crossentropy', metrics = ['accuracy'])
```

将人工神经网络拟合到训练集

```
classifier.fit(X_train, y_train, batch_size = 10, nb_epoch = 100)
```

# 第 3 部分—进行预测和评估模型

预测测试集结果

```
y_pred = classifier.predict(X_test)
y_pred = (y_pred > 0.5)
```

如果 y_pred 大于 0.5，则返回 true(1)，否则返回 false(2)

```
print(y_pred)
```

**该模型在训练数据上训练和在测试数据上测试时，在两种情况下都给出了大约 86%的准确度。**这从我们的角度来看是伟大的！！！

制作混淆矩阵

```
*from* sklearn.metrics *import* confusion_matrix
cm = confusion_matrix(y_test, y_pred)
print(cm)
```

从混淆矩阵中获得。您可以根据您的混淆矩阵中获得的内容来更改值。

```
print(accuracy)
```

> 恭喜你！你只是为给你这个任务的银行写了你自己的神经网络。您现在可以找出最有可能离开银行的前 10%的客户(这对银行来说非常重要)。该银行现在可能会采取纠正措施，以避免失去客户，如果没有你的帮助，它不会这样做:P

我希望你喜欢我的教程！我将 github(现在是 Gitlab :P)链接附加到代码和数据集上。

[](https://github.com/Sid-darthvader/Artificial-Neural-Network-on-Churn-Modeling-Dataset-for-a-Bank) [## sid-Darth Vader/基于人工神经网络的银行数据集建模

### 人工神经网络对流失建模数据集对银行人工神经网络对流失建模数据集…

github.com](https://github.com/Sid-darthvader/Artificial-Neural-Network-on-Churn-Modeling-Dataset-for-a-Bank) 

参考文献-机器学习课程，作者:基里尔·叶列缅科和哈德琳·德·庞特维斯