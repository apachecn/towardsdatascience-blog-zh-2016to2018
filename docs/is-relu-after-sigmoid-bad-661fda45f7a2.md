# 乙状结肠后的 ReLU 不好吗？

> 原文：<https://towardsdatascience.com/is-relu-after-sigmoid-bad-661fda45f7a2?source=collection_archive---------4----------------------->

最近有一篇关于深度学习的心理模型的博客文章，从光学角度进行了类比。我们对一些模型都有直觉，但很难用语言表达出来，我相信有必要为这个心智模型共同努力。

![](img/9c8d1f0e90dfa79a3500d8c31a9713da.png)

Sigmoid graph from wikipedia

最近我和 Rajasekhar(为一个 [KWoC](https://kwoc.kossiitkgp.in/) 项目)正在分析不同的激活函数之间如何相互作用，我们发现**在最后两层的 sigmoid 之后使用 relu 会恶化模型**的性能。我们使用 MNIST 数据集和一个四层全连接网络，第一层是 784 维的输入层，然后第二层是 500 维的隐藏层，之后是另一个具有 256 维的隐藏层，最后是 10 维的输出层。除了输入层，我们对每一层的输出使用非线性。由于我们将研究限制在四个激活函数( *ReLU，Sigmoid，Tanh，卢瑟*)，我们可以通过激活函数的不同组合构建 64 个不同的模型。我们在所有模型中使用随机梯度下降，学习率为 0.01，动量为 0.5。我们在所有实验中使用交叉熵损失和 32 的批量大小。我们对每个模型进行了 9 次实验，精确度的平均值和标准偏差显示在[ [nishnik/sherlocked](https://github.com/nishnik/sherlocked/blob/master/inspect_activations/Activations_Results_multiple_iterations.md) 的表格中。我在这里简单总结一下:

1.  如果第一层有 **relu** 激活，第二层和第三层有除(sigmoid，relu)之外的(relu，tanh，sigmoid，relu)的任意组合，则平均测试精度大于 **85%** 。对于( **relu，sigmoid，relu** )，我们得到的平均测试精度为 **34.91%**
2.  如果第一层有 **tanh** 激活，第二层和第三层有除(sigmoid，relu)之外的(relu，tanh，sigmoid，relu)的任意组合，则平均测试精度大于 **86%** 。对于( **tanh，sigmoid，relu** )，我们得到的平均测试精度为 **51.57%**
3.  如果第一层有 **sigmoid** 激活，第二层和第三层有除(sigmoid，relu)之外的(relu，tanh，sigmoid，relu)的任意组合，则平均测试精度大于 **76%** 。对于( **sigmoid，sigmoid，relu** )，我们得到的平均测试精度为 **16.03%**
4.  如果第一层有**卢瑟**激活，第二层和第三层有除(sigmoid，relu)之外的(relu，tanh，sigmoid，relu)的任意组合，那么平均测试精度大于 **91%** 。对于(**卢瑟，sigmoid，relu** )我们得到的平均测试准确率为 **75.16%**
5.  如果最后两层具有( **sigmoid，relu** )的话，精确度的变化也很大

我们也在 CIFAR-10 上进行了实验，结果是相似的[ [链接](https://github.com/rajasekharmekala/sherlocked/blob/c99a8e8df7059a91220f439c762382e4f5ffc973/inspect_activations/CIFAR/Results.md) ](抱歉格式错误)。在最后两次激活为( **sigmoid，relu** )的每种情况下，准确度为 **10%** ，否则准确度≥ **50%。**

然后，我们在每一层中使用批规范进行实验。和其他组合一样，精确度也有了很大的提高。[[MNIST 的结果](https://github.com/nishnik/sherlocked/blob/master/inspect_activations/batchNorm/BatchNormResults.md) ]。同样，在最后一层使用 batchnorm 就像 charm 一样让模型学习。

因此，对于最后两层中的( **sigmoid，relu** )来说，模型不能学习，即梯度不能很好地反向传播。(sigmoid(output_2)* weigh _ 3+bias _ 3)<大多数情况下为 0，或者 sigmoid(output _ 2)正在达到极值(消失梯度)。这两个我还在做实验。在[twitter.com/nishantiam](https://twitter.com/nishantiam)给我一些建议，或者在[[nish Nik/Sherlock](https://github.com/nishnik/sherlocked)上制造一个问题。