# 使用 Keras 开始深度学习

> 原文：<https://towardsdatascience.com/get-started-with-deep-learning-using-keras-a45ee421f3ef?source=collection_archive---------7----------------------->

深度学习处于人工智能革命的最前沿，这是有充分理由的——自然语言处理、图像识别，甚至计算机围棋方面令人难以置信的进步，都要归功于深度神经网络的帮助。

![](img/9779805358f8c021ce6c03e648b24bf1.png)

Alexa in the Amazon Echo uses deep learning to understand human language. [Andres Urena](https://unsplash.com/@andresurena?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

由于大多数神经网络必须完成的任务的复杂性，大多数人会认为很难建立一个神经网络。是的，也许 5 年前那是完全正确的，你必须从头开始建立你的神经网络。但是今天，有了像 [Keras](https://keras.io/) 这样的开源框架，开始变得非常简单。以下是方法。([这里是完整的脚本](https://gist.github.com/labriedion/9e1480a22d2e259f460b56c84a01a186)如果你想跳过前面！)

# 问题:用人口统计学变量预测薪水

首先，我们先找一个玩具问题入手。下面是我找到的一个:[1994 年美国人口普查数据库](https://www.kaggle.com/johnolafenwa/us-census-data)的美国成年人收入数据。我们将尝试使用几个完全连接的神经元层来预测人口普查中给定人员的工资。

老实说，深度神经网络对于这项任务来说可能有点大材小用。传统的数据科学技术，如逻辑回归，非常适合这种情况，并且非常容易解释。尽管如此，这仍然是一个开始使用 Keras 的好方法。

![](img/a7a0986354607c0468016edd585d60e5.png)

[Markus Spiske](https://unsplash.com/@markusspiske?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

## 进口

为了遵循这段代码，首先安装 [Python 3](https://www.python.org/downloads/) 。完成后，打开您的操作系统终端，并键入以下内容:

`pip install **pandas**`

然后在熊猫之后安装以下包: **sklearn** 、 **tensorflow** ，最后是 **keras** 。

首先，我们将从我们的进口开始。该脚本需要:

1.  **熊猫**用于数据操作、
2.  **numpy** 对于一些快速操作，
3.  **sklearn** 对于数据缩放和性能测量，
4.  最后， **keras** ，我们的深度学习框架。

## 加载数据集

我们的数据集包含各种变量，如员工的年龄、教育水平、受教育年限、婚姻状况、职称等。该数据集中包含的最后一个值是工资范围，低于或高于$50，000。我们将使用前面的变量来预测这个目标。

## 准备数据集

下一步是为 Keras 准备数据集。我们需要做到以下几点:

1.  平衡各阶级。不平衡的班级会干扰训练。
2.  将分类变量(例如职业)**转换成一个热点编码变量**。基本上是为每个标签创建一个值为 0 或 1 的新列。
3.  **提取训练和测试数据集的 X** (变量)**和 y** (目标)**值**。
4.  **标度**变量。

# 喀拉斯的模型

现在是精彩的部分！构建 Keras 模型。在 Keras 中有两个接口来构建模型，顺序模型 API 和函数 API。今天，我们将使用顺序模型 API，因为它更容易理解和构建。

这是我们需要做的。

1.  首先，我们将设置一个**模型检查点**对象，以允许我们在训练过程中保存最佳结果。
2.  然后，我们将创建一个**序列**模型对象，我们将向其添加不同的神经层。
3.  我们将需要选择适当的**激活**函数、**优化器**和**损失**函数来构建我们的**二元分类模型**(我们试图预测一个变量的值为 0 或 1)。
4.  最后，我们将训练我们的网络。

## 评估模型的性能

我们表现如何？让我们来衡量我们的模型的性能！我们将测量[精度](https://en.wikipedia.org/wiki/Precision_and_recall)，一个精确分类量的值。

1.  我们将需要**加载我们在模型训练期间保存的权重**，并且**用这些权重编译模型**。
2.  **使用模型预测测试数据集中的值**。
3.  **将**预测的工资与测试数据集中的**实际**工资进行比较。

最终，该模型对于低于 50K 的工资获得了大约 87%的精度，对于高于 50K 的工资获得了大约 72%的精度。

对于一个没有任何特征工程的模型来说还不错…

GithubGist 上的[提供了整个脚本。](https://gist.github.com/labriedion/9e1480a22d2e259f460b56c84a01a186)

我希望你喜欢我的教程！

艾蒂安