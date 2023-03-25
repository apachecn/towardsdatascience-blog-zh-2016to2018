# [ML]网络培训—了解一些细节

> 原文：<https://towardsdatascience.com/ml-network-training-getting-into-some-details-ad33198626ee?source=collection_archive---------3----------------------->

# **TL；博士**

人工神经网络训练包括一个迭代的正向(推理)和反向(反向传播)过程，目的是调整网络参数，以减少隐含(即采样+噪声)定义的目标函数的相似距离

# **长版**

让我们考虑一个人工神经网络学习传递函数

![](img/4d3783c5e5e9691aaf57ffe3851200c4.png)

ANN Transfer Function. X is the Data Space and \Omega the Parameters Space

在监督学习中，训练 ANN 的目的是使网络传递函数逼近目标传递函数，该目标传递函数不是以显式封闭形式而是以隐式采样形式提供，作为一组训练数据点。

此外，不幸的是，样本不仅考虑目标传递函数，还考虑一些附加噪声。我们想让人工神经网络学习尽可能少的噪音，以达到**更好的泛化性能**。

![](img/e15e3305ab28b546ecf6e8b8fc1e21c6.png)

Line1 : Target Function. Line2 : Noise. Line3: Dataset as a collection of Target Function samples with some added Noise

这种目标函数表示提出了两个主要问题

*   离散与连续信息:人工神经网络传递函数是连续且可微分的(要求能够使用基于梯度的方法),而目标函数表示是离散的，因此人工神经网络本质上应该学会在数据点之间正确插值
*   噪声:ANN 对噪声了解得越多，它就越不能正确地进行归纳

ANN 训练通常意味着解决网络参数状态空间中的最小化问题，该最小化问题涉及与一些网络参数化相关的 ANN 传递函数和目标之间的相似性度量，因此是我们的噪声目标函数采样。

让我们将这种相似性度量视为关于训练数据点子集定义的目标函数，我们称之为**批次**作为单个贡献的总和

![](img/ebc23c07d3463ffc2c9ebc810bb0b893.png)

The Similarity Measure or Error Function is defined with repsect to a Batch of Points as Sum of each single Data-Point Contribution

培训通常包括两种类型的网络信息传播:

*   前向信息传播，旨在从一些输入(即训练数据)开始执行推理
*   反向信息传播，旨在根据误差(定义为推断数据与训练数据差异的函数)进行参数拟合

前向信息传播发生在数据空间，而参数空间是固定的

![](img/835c82a4a1df7b2df047cf66381434ae.png)

Training Forward Step aimed at computing a specific Data-Point contribution

反向信息传播，称为反向传播，发生在参数空间，而数据空间是固定的

![](img/a5342892c25e7f951a10d6e86a684314.png)

Backpropagation of the Information using Gradient computed on a Batch

训练是一个两级迭代过程:一次迭代包括一个从批量数据到错误的前进步骤和一个从错误到参数更新的后退步骤

*   第一组迭代旨在覆盖所有训练数据，一批接一批，从而完成一个**时期**
*   第二组迭代逐个历元周期地考虑一个历元

当批量大小等于训练集大小时，则使用**标准梯度下降**方法，并且计算的梯度是**真实梯度**:相对于所有可用信息计算的梯度。在这种情况下，1 次迭代= 1 个时期。

该算法允许最佳的收敛，但是从内存和计算的角度来看，它都非常昂贵，因为在执行参数更新之前使用了所有可用的信息**，因此有可能提高性能。**

培训成本随着用于执行更新步骤的信息量的增加而增加。

然后，策略可以是使用减少的信息集来执行更新步骤，从而减少批量大小，并因此计算真实梯度的**近似值。**

近似梯度相对于真实梯度的差异在于，噪声/信号比将随着平均效应的降低而增加。此外，使用这种方法，完成一个时期的迭代次数将增加，但是从计算和存储的角度来看，它们将变得更便宜。