# 批量标准化

> 原文：<https://towardsdatascience.com/batch-normalization-8a2e585775c9?source=collection_archive---------0----------------------->

这个想法是，我们不只是将网络的输入标准化，而是将网络中层的输入标准化。它被称为“批量”归一化，因为在训练期间，我们通过使用当前小批量中的值的均值和方差(通常是零均值和单位方差)来归一化每一层的输入。

# 批量标准化的好处

批量归一化优化网络训练。已经证明它有几个好处:

1.  **网络训练更快** —每次训练*迭代*实际上会更慢，因为正向传递过程中需要额外的计算，反向传播过程中需要训练额外的超参数。然而，它应该收敛得更快，所以训练应该更快。
2.  **允许更高的学习速率** —梯度下降通常需要较小的学习速率来使网络收敛。随着网络越来越深，它们的梯度在反向传播过程中变得越来越小，因此它们需要更多的迭代。使用批量标准化允许我们使用更高的学习率，这进一步提高了网络训练的速度。
3.  **使权重更容易初始化** —权重初始化可能会很困难，在创建更深的网络时甚至更困难。批量标准化似乎允许我们在选择初始权重时不那么小心。
4.  **使更多的激活功能变得可行** —一些激活功能在某些情况下不能很好地工作。Sigmoids 很快就会失去梯度，这意味着它们不能用于深层网络。ReLUs 经常在训练中消失，在那里他们完全停止学习，所以我们需要小心输入他们的值的范围。因为批量标准化调节进入每个激活函数的值，在深度网络中似乎不太好工作的非线性实际上又变得可行了。
5.  **简化更深层次网络的创建** —由于上面列出的前 4 项，当使用批量标准化时，更容易建立和更快地训练更深层次的神经网络。事实证明，更深的网络通常会产生更好的结果，这很好。
6.  **提供一点规范化** —批量规范化会给你的网络增加一点噪音。在某些情况下，比如在初始模块中，批处理规范化已经被证明和删除一样有效。但是一般来说，将批量规范化视为一种额外的规范化，可能允许您减少一些可能添加到网络中的丢失。
7.  **总体上可能给出更好的结果** —一些测试似乎显示批量标准化实际上提高了训练结果。然而，它确实是一种帮助更快训练的优化，所以你不应该认为它是一种让你的网络变得更好的方法。但由于它让你更快地训练网络，这意味着你可以更快地迭代更多的设计。它还能让你建立更深层次的关系网，这通常会更好。因此，当你把所有因素都考虑进去时，如果你用批量规范化来构建你的网络，你可能会得到更好的结果。

> 我们在`fully_connected`函数中给层添加了批量标准化。以下是一些要点:
> 
> 批量归一化的图层不包含偏差项。在 tf.layers.conv2d()和 tf.layers.dense()中设置 use_bias=False
> 
> TensorFlow 有`[tf.layers.batch_normalization](https://www.tensorflow.org/api_docs/python/tf/layers/batch_normalization)`函数来处理数学。
> 
> 我们告诉`tf.layers.batch_normalization`网络是否在训练。这是重要的一步。
> 
> 我们在调用激活函数的之前添加了规范化**。**

```
def conv_layer(prev_layer, layer_depth, is_training):
  conv_layer = tf.layers.conv2d(prev_layer, layer_depth*4, 3, strides, 'same', use_bias=False, activation=None)
  conv_layer = tf.layers.batch_normalization(conv_layer, training=is_training)
  conv_layer = tf.nn.relu(conv_layer)
```

学分:来自课堂讲稿:[https://classroom.udacity.com/nanodegrees/nd101/syllabus](https://classroom.udacity.com/nanodegrees/nd101/syllabus)