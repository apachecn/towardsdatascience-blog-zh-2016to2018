# 为什么在深度学习中我们需要比反向传播更好的学习算法

> 原文：<https://towardsdatascience.com/why-we-need-a-better-learning-algorithm-than-backpropagation-in-deep-learning-2faa0e81f6b?source=collection_archive---------2----------------------->

![](img/0e2c8ecf7af1ff4cbe11820ed03db653.png)

Source — toptal -[https://www.toptal.com/](https://www.toptal.com/)

我们都同意一件事，即反向传播是一种革命性的学习算法。当然，它已经帮助我们训练了几乎所有的神经网络结构。在 GPU 的帮助下，反向传播将几个月的训练时间减少到了几个小时/几天。它允许有效地训练神经网络。

我想到了它被广泛采用的两个原因，(1)我们没有比反向传播更好的方法，(2)它有效。反向传播是基于微分的链式法则。

问题在于反向传播算法本身的实现。为了计算当前层的梯度，我们需要下一层的梯度，所以当前层是锁定的，我们不能计算梯度，除非我们有下一层的梯度。如果我们的网络中有 1000 层，我们的第一层必须等到永远更新它的权重。神经网络的前几层很糟糕，不能正常更新。有时，在 Sigmoid 激活函数的情况下，当我们往回传播时，梯度消失或爆炸。

当我们做决定的时候，我们是基于我们当前的观察和之前的学习来做决定的。目前的神经网络或深度学习算法不是按照我们做决定的方式设计的。我们的经验决定了我们的决定。例如，当我们走路时，我们使用视觉、听觉和感官输入来做出决定。我们利用从一个任务中学习来学习其他任务。

反向传播算法的局限性:

*   这是缓慢的，所有以前的层被锁定，直到当前层的梯度计算
*   它遭受消失或爆炸梯度问题
*   它遭受过拟合和欠拟合问题
*   它只考虑预测值和实际值来计算误差和计算梯度，与目标函数相关，部分与反向传播算法相关
*   它在计算与目标函数相关、与反向传播算法部分相关的误差时，不考虑类之间的空间、关联和去关联关系

DeepMind 的合成渐变显示了一种变通方法，但它不是一种解决方案。在我看来，我们必须从头开始思考，设计一种新的学习算法，它可以高效地学习，并可以帮助我们的网络实时学习。

免责声明:这是我的个人观点，完全基于我的学习和研究。我邀请大家分享对此的想法。

> 感谢您的阅读。
> 
> 如果你想取得联系，你可以通过 ahikailash1@gmail.com**的**联系我

**关于我:**

我是 [MateLabs](http://www.matelabs.in) 的联合创始人，我们在那里建立了 [Mateverse](https://www.mateverse.com) ，这是一个 ML 平台，让每个人都可以轻松地建立和训练机器学习模型，而无需编写一行代码。

> **注**:最近，我出版了一本关于 GAN 的书，名为《生成对抗网络项目》，书中涵盖了大部分广泛流行的 GAN 架构及其实现。DCGAN、StackGAN、CycleGAN、Pix2pix、Age-cGAN 和 3D-GAN 已在实施层面详细介绍。每种架构都有专门的一章。我已经使用 Keras 框架和 Tensorflow 后端用非常简单的描述性语言解释了这些网络。如果你正在开发 GANs 或者打算使用 GANs，请阅读一下，并在**ahikailash1@gmail.com**与我分享你的宝贵反馈

[](https://www.amazon.com/Generative-Adversarial-Networks-Projects-next-generation/dp/1789136679) [## 生成对抗网络项目:使用 TensorFlow 构建下一代生成模型…

### 探索使用 Python 生态系统的各种生成性对抗网络架构关键特性使用不同的…

www.amazon.com](https://www.amazon.com/Generative-Adversarial-Networks-Projects-next-generation/dp/1789136679) 

> 可以从[http://www . Amazon . com/Generative-Adversarial-Networks-Projects-next-generation/DP/1789136679](http://www.amazon.com/Generative-Adversarial-Networks-Projects-next-generation/dp/1789136679)[https://www . Amazon . in/Generative-Adversarial-Networks-Projects-next-generation/DP/1789136679？FB clid = iwar 0 x2 pdk 4 ctxn 5 gqwmbbkigib 38 wmfx-sqcpbni 8k 9z 8 I-kcq 7 vwrpjxm 7 I](https://www.amazon.in/Generative-Adversarial-Networks-Projects-next-generation/dp/1789136679?fbclid=IwAR0X2pDk4CTxn5GqWmBbKIgiB38WmFX-sqCpBNI8k9Z8I-KCQ7VWRpJXm7I)[https://www . packtpub . com/big-data-and-business-intelligence/generative-adversarial-networks-projects？FB clid = iwar 2 otu 21 fam fpm 4 suh _ HJmy _ drqxovwjzb0k z3 zis bfb _ MW 7 inycqqv7 u 0 c](https://www.packtpub.com/big-data-and-business-intelligence/generative-adversarial-networks-projects?fbclid=IwAR2OtU21faMFPM4suH_HJmy_DRQxOVwJZB0kz3ZiSbFb_MW7INYCqqV7U0c)