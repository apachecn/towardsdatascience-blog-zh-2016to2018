# 关于渐变的更多信息

> 原文：<https://towardsdatascience.com/more-about-the-gradient-bf97e4d9c1c5?source=collection_archive---------6----------------------->

在与 Eugene 的交谈中，我列举了一些具体的例子，这些例子可能对我最近的一页“克服消失梯度问题”有所帮助。

考虑玩迷宫游戏的递归神经网络。给出了连续的选择，要么走“右边的路”要么走“左边的路”,只有在选择少于一定数量的情况下才给予奖励。假设在所有*成功的*剧里，网络选择了‘右侧路径’，而*失败的*剧**有很大一部分是从‘左侧路径’**开始的。“左”是一个错误的决定。而且是第*个第*个决定。这是典型梯度下降失败的地方；损失函数仅“拉动”网络的最顶层的*，因此它没有“力”来改变这些**早期错误**。*

随着递归变得更深(更大的迷宫，需要更长的时间来完成)，梯度下降更难识别早期的错误。然而，**该错误仅出现在失败的案例中**。基于协方差的方法可以立即捕捉到这类误差。

现在，考虑协方差提醒我们有问题的另一个实例，不同于从输出层传播的损失函数:图像分类器，它通常(> 0.95)正确识别“猫”…然而，它失败的时间集中在一些特征的*缺失*周围。通常活跃的神经元反而变得沉默。(“两只耳朵”举例？)我们希望将我们的“拉力”施加到错误的神经元上，鼓励它们像正确的神经元一样放电(即使双耳并不完全可见)。

**一点数学:**

作为对反馈的回应，我将深入研究负协方差的一个玩具示例。除了训练中的任何其他图像之外，您的“猫”分类器在训练集中有七幅真实猫的图像(称它们为 A、B、C、D、E、F、G)。你通过你的网络发送每一张“猫”的图像，并勤勉地为你的计算保存每一个神经元的活动。根据正确或错误的分类对它们进行分类——假设，在我们的例子中，所有的 A、B、C、D 都被正确地分类为“cat ”,而 E、F、G 被标记为其他东西。只看我们的*第一个*神经元 x1，我们可以问:“在{A，B，C，D}中，平均**激活是多少？**同** for {E，F，G}？”以下是第一个神经元激活的一些样本值:**

x1[A]: 0.7 __________ 精确激活值的平均值

x1[B]: 0.8

x1[C]: 1.1

x1[D]:1.0 _ _ _ _ _ _ _ _ _ _ _ _

x1[E]:0.2 _ _ _ _ _ _ _ _ _ _ 错误激活值的平均值

x1[F]: 0.3

x1[G]: 0.0_________

准确批次的平均值为 S=0.9，而错误批次的平均活化值为 R = 0.16666……它们表现不同，这可能是分类错误的原因。

现在，我们求这两个平均数的平均值:(0.9+0.16666)/2 = T = 0.53333。注意这是如何平等地对待平均值的，*不管每批中图像的数量*(如果有 5 个以上的错误图像，但是它们的平均值仍然是 0.16666，那么它们之间的平均值*仍然是 0.53333)。*

下一步是随机(替换)挑选一对，每批一对。画出这样一对的例子:x1[ **C** 和 x1[ **F** 。从它们中的每一个中减去组合平均值 T。然后，将它们相乘。即，(x1[**C**]-T)*(x1[**F**]-T)= U(C，F)。继续画线对，计算它们的 U(v，w ),一边画一边将每个结果相加。有了足够的样本后，将你的总和除以你尝试的配对数。如果和是负的，就保留它，这是围绕 t 的负协方差。

协方差值可以*加到你的梯度*上，当它接近每个神经元的时候。(您可以尝试使用一个非常小的因子来加权协方差，在添加到梯度之前将其乘以 0.01，并将该行为与具有更强协方差力的网络进行比较，比如 0.1…)

希望这能清晰地勾勒出一个粗略的实现！

**正如 Eugene 提到的，可能仍然存在问题:**

如果只有一个**少数**例子是误分类，而**有许多**神经元，那么每个神经元都有很小的“偶然协方差”机会，这仅仅是因为批量小。这可能会将网络参数推向错误的方向，并使网络处于不良振荡状态。一个有用的统计视角，只有通过探索才能得到验证！

然而，*似乎*在一小批错误中观察到的协方差(统计上可能，有许多神经元)将**仍然**在应该施加“拉力”**的地方，考虑到这是*错误*中的共性，而不是成功。协方差不会出现在许多神经元中，就好像它们是随机样本一样；这是一个结构化的环境。我仍在试图想象一种情况:观察到负协方差，而我们不想消除它？直觉上，我们应该总是喜欢让我们错误的网络信号更像正确的，不是吗？**