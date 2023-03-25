# 关于如何优化深度神经网络学习速率的初级读本

> 原文：<https://towardsdatascience.com/learning-rate-a6e7b84f1658?source=collection_archive---------6----------------------->

![](img/495897678641e5b6ef7822002f8b7dc4.png)

[Victoire Joncheray](https://unsplash.com/@victoire_jonch?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

训练深度神经网络时的一个主要挑战是平衡最终解决方案的质量和达到最终解决方案所需的训练时间。学习率是优化这种平衡的最重要的超参数。

你可以认为小学习率和大学习率具有不同的个性:

*   小学习率谨慎。也就是说，它使网络缓慢而小心地调整。
*   学习率大就是浮躁。也就是说，它调整得很快，但可能会超调。

当你做深度学习时，你想让网络同时快速而精确地学习。在这篇文章中，我向你展示了三种不同的选择，在谨慎和浮躁之间找到平衡:

1.  决定一个既不太低也不太高的学习率，即找到最佳的折衷。
2.  一旦你接近一个最佳解决方案，在训练期间从高到低调整学习速率以减慢速度。
3.  在高学习率和低学习率之间摇摆，创造一个混合体。

这篇文章是作为一个引子，并不涵盖细节。如果你想了解更多，我推荐 Pavel Surmenok 的这篇文章:

[](/estimating-optimal-learning-rate-for-a-deep-neural-network-ce32f2556ce0) [## 估计深度神经网络的最佳学习速率

### 学习率是用于训练深度神经网络的最重要的超参数之一。

towardsdatascience.com](/estimating-optimal-learning-rate-for-a-deep-neural-network-ce32f2556ce0) 

## 选择 1:折衷——固定学习率

最基本的方法是坚持默认值，并希望最好的结果。

第一个选项的更好实现是测试各种可能的值。根据损失的变化，你可以选择更高或更低的学习率。目标是找到仍能减少损失的最快速度。

鉴于这种选择的简单性，固定的学习率会让你走得比你预期的更远。然而，还有更好的选择。

## 选项 2:顺序——随着时间的推移，学习率降低

第二种选择是以高学习速率开始，以利用速度优势，然后切换到低学习速率，以优化结果。有两种主要的变化。

首先，你可以根据损失函数的变化来调整学习速率。也就是说，每当损失函数停止改进时，你就降低学习速率以进一步优化。

第二，您可以应用更平滑的函数形式，并根据训练时间调整学习速率。也就是说，学习率的降低与损失函数没有直接关系。

与固定学习率相比，这两种方法及其变体都是改进，因为它们结合了小学习率和大学习率的优点。然而，还有更大的改进余地。

## 选项 3:混合式学习率——走走停停

主要的想法是，由快转慢非常有帮助，我们应该在训练中不止一次地这样做。还有两个额外的优势:

1.  跳回到高学习率有助于避免局部最优。
2.  高学习率在穿过损失函数的平坦区域时更快。

综上所述，第三种选择是将第二种选择重复几次。

## 但是我们没有优化算法吗？

**是的**，因为最先进的优化算法，如*亚当*根据训练过程改变每个人体重的学习率。如果你想更多地了解亚当，我推荐这篇博文:

[](https://machinelearningmastery.com/adam-optimization-algorithm-for-deep-learning/) [## 深度学习的 Adam 优化算法简介

### 你的深度学习模型的优化算法的选择可能意味着良好结果之间的差异…

machinelearningmastery.com](https://machinelearningmastery.com/adam-optimization-algorithm-for-deep-learning/) 

**没有**，因为即使是 *Adam* 也有两个与学习率相关的参数，分别是`lr`用于学习率和`decay` ( [参见 *Keras* 实现](https://keras.io/optimizers/))。也就是说， *Adam* 需要一个学习速率来开始，以及它是否应该包括衰减函数的信息。如果你想使用这些参数，你需要考虑如何应用选项 1 和 2。

还有一种算法实现了选项 3。它被称为*重启的随机梯度下降*，并由[马克·霍夫曼](https://medium.com/u/cc8070255a47?source=post_page-----a6e7b84f1658--------------------------------)进行了精彩的解释:

[](https://medium.com/38th-street-studios/exploring-stochastic-gradient-descent-with-restarts-sgdr-fa206c38a74e) [## 探索重新开始的随机梯度下降(SGDR)

### 这是我第一篇深度学习的博文。我在 2017 年 1 月左右开始了我的深度学习之旅，在我听说了…

medium.com](https://medium.com/38th-street-studios/exploring-stochastic-gradient-descent-with-restarts-sgdr-fa206c38a74e) ![](img/162cb3e3a0cac11779582ac14b01eac9.png)

[Jens Johnsson](https://unsplash.com/@jens_johnsson?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

## 你下一步应该做什么

我所知道的关于深度学习的最好的课程是由 *fast.ai* 公开提供的课程。还有同一个团队写的刚到 1.0 版本的 Python 包，如果你关心深度学习，[到这里上第一堂课](http://course.fast.ai/lessons/lesson1.html)。

总体来说，你对学习率和训练深度学习有什么看法？请在评论中或在 [Twitter](https://twitter.com/TimoBohm) 上告诉我。我也很乐意在 LinkedIn 上联系。**感谢阅读，留点👏🏻如果这对你有帮助，让我们继续学习吧！**