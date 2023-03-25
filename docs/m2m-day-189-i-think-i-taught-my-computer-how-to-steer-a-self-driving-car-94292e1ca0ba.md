# M2M 第 189 天:我想我教会了我的电脑如何驾驶自动驾驶汽车。但是，我就是想不出如何证实这个事实。

> 原文：<https://towardsdatascience.com/m2m-day-189-i-think-i-taught-my-computer-how-to-steer-a-self-driving-car-94292e1ca0ba?source=collection_archive---------8----------------------->

> 这篇文章是为期 12 个月的加速学习项目[月掌握](https://medium.com/@maxdeutsch/m2m-day-1-completing-12-ridiculously-hard-challenges-in-12-months-9843700c741f)的一部分。今年五月，[我的目标是打造无人驾驶汽车的软件部分](https://medium.com/@maxdeutsch/m2m-day-182-attempting-to-build-a-self-driving-car-809fab9e4723)。

昨晚，在“[绝望的编码坑](https://medium.com/@maxdeutsch/m2m-day-188-whyd-i-sign-up-for-this-the-coding-pit-of-despair-b2c8b6246332)”中溺死了几个小时后，我终于取得了一些前进的进展。

作为一个提醒，在过去的几天里，我一直在试图找到并运行可以根据道路的输入图像生成转向指令(为我的自动驾驶汽车)的代码。

经过几个小时的搜索和摆弄，我找到了一篇由英伟达(一家制造自动驾驶汽车硬件和软件的公司)的研究人员撰写的学术论文。

![](img/c900df00a85f2b96b91c805d0e37b2fc.png)

正如论文摘要中所述，他们设计的系统可以*“将单个前置摄像头的原始像素直接映射到转向命令”*。

这正是我需要的！

我随后在 Github 上找到了英伟达系统的 TensorFlow 实现，经过几次尝试，我实际上已经能够根据英伟达的一些数据来“训练模型”。

快速澄清一些术语:1。“模型”是描述如何将像素转换成转向指令的函数，以及 2。“训练模型”意味着使用机器学习技术迭代地改进这个函数。我将在以后的文章中更详细地解释这个过程。

无论如何，NVIDIA 的数据集包括 25 分钟逐帧分解的视频，其中每一帧都标有*真实转向角度*(即人类驾驶员使用的转向角度)。

![](img/4feaf5afeda4f3aeb2636ab02e4d70d7.png)

Frame 45,522

在终端，我运行程序来训练模型，尽管有一些警告，它开始工作了:

![](img/a2021a0674603691545cc898b1ceb908.png)

在这个屏幕截图中，一个“步骤”描述了每次通过系统输入一部分数据进行训练。“纪元”只是一个包含多个步骤的更广泛的类别。

为了训练这个模型，我使用了 30 个时期，每个时期几十步。

截图中的“损失”描述了*型号*(或功能)的准确程度。从概念上讲，为了计算损失，将真实转向角与模型预测的转向角进行比较。差额越大，损失越大。

最终，当训练模型时，程序使用一些数学技巧(我将在以后的帖子中描述)来试图通过每个迭代步骤减少损失。

于是，“训练模型”不过是“减少损失”。

这是一个曲线图，X 轴是步长，Y 轴是损耗。(昨晚，使用一种叫做 Tensorboard 的东西，我的电脑在训练时绘制了这个)。

![](img/8670ea0bbfedeec92499f1e111655b8c.png)

例如，在步骤 126，损失的值为 5.708。

![](img/c096b87813f4ba89d67eacdecf498c12.png)

而在步骤 3，241，几乎 6 小时后，损失具有明显更好的值 0.1615。

![](img/253d2ff2b9fa5fde638f6f5913e6800c.png)

现在训练已经正式完成，模型现在理论上已经可以驾驶汽车了，超级酷。

当我今天下班回家时，我试图测试这个模型(又名“驾驶汽车”)，看看它的表现如何。可悲的是，当我试图运行程序时，我得到了这个错误…

![](img/d00e6227fcb6add30c9fe08ee9ce0e40.png)

我花了整整一个小时试图克服这个问题，但似乎我下载的一些代码无法在 Mac 上运行(即使可以，我也找不到让它运行的方法)。

我有一个解决办法的想法，但它必须等到明天。

与此同时，我可以庆祝这样一个事实，即我(很可能)已经有了一个功能合理、经过训练的自动驾驶汽车模型，可以随时投入使用。希望明天，我能弄清楚如何实际使用它…

> 阅读[下一篇](https://medium.com/@maxdeutsch/m2m-day-190-the-car-is-driving-itself-74490ae509de)。看了[以前的帖子](https://medium.com/@maxdeutsch/m2m-day-188-whyd-i-sign-up-for-this-the-coding-pit-of-despair-b2c8b6246332)。

## [马克斯·多伊奇](http://max.xyz)是一名痴迷的学习者、产品建造者、为期[个月以掌握](http://MonthToMaster.com)的试验品，以及 [Openmind](http://OpenmindLearning.com) 的创始人。

## 如果你想跟随 Max 长达一年的加速学习项目，请确保跟随[这个媒介账户](https://medium.com/@maxdeutsch)。