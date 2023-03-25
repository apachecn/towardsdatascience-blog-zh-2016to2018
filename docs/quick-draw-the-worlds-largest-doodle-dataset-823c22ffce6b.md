# 快速绘画:世界上最大的涂鸦数据集

> 原文：<https://towardsdatascience.com/quick-draw-the-worlds-largest-doodle-dataset-823c22ffce6b?source=collection_archive---------8----------------------->

谷歌的一个团队开始让猜字谜游戏变得更有趣，最终获得了世界上最大的涂鸦数据集，以及一个强大的机器学习模型。他们是怎么做到的？

# 什么是快速绘制？

“快，画！”是一款最初在 2016 年谷歌 I/O 上推出的游戏，在这款游戏中，一名玩家会被提示画出一个物体的图片，另一名玩家需要猜测这是什么。就像猜字谜一样。

![](img/81b8c3fb027100a6ea52dd3540137b3d.png)

2017 年，谷歌研究中心的 Magenta 团队将这一想法向前推进了一步，使用这个带标签的数据集来训练[草图-RNN](https://magenta.tensorflow.org/assets/sketch_rnn_demo/index.html) 模型，试图实时预测玩家正在画什么，而不是要求第二个玩家进行猜测。游戏[已经上线](https://quickdraw.withgoogle.com/)，现在已经收集了超过 10 亿张手绘涂鸦！

让我们来看看来自快速绘制的一些绘图。这里我们看到西兰花被很多玩家抽中。

![](img/8f7a80200d7f249f7692b4f122ec3246.png)

How would you draw broccoli?

请注意，不同的玩家对海洋的描绘略有不同。

![](img/331a7c5ecffc912d271e3e83a61f2d74.png)

浏览数据集可能非常有趣。如果你发现一些看起来不合适的地方，你实际上可以在这一页上修改它。会让数据对大家更好！

# 快速绘制数据集

该团队已经开源了这些数据，并且采用了多种格式。你可以在他们的 [GitHub 页面](https://github.com/googlecreativelab/quickdraw-dataset)了解更多信息。

![](img/773284db22c2e852f570d826e472e3c5.png)

有 4 种格式:首先是存储在。ndjson)格式。这些文件编码了每个涂鸦的全套信息。它包含每幅画的每一笔的计时信息。

还有一个简化版，存储格式相同(。ndjson)，它应用了一些预处理来规范化数据。简化版也可作为二进制格式，以便更有效地存储和传输。有一些如何使用 Python 和 NodeJS 读取文件的例子。

![](img/568225c2e8efb4407c4e97574bf4a10d.png)

第四种格式采用简化的数据，并将其呈现为 numpy 格式的 28x28 灰度位图。npy 格式，可以用 np.load()加载。

为什么是 28x28？嗯，它是任何现有的用于处理 MNIST 数据的代码的完美替代品。因此，如果你在寻找比 10 个手写数字更新奇的东西，你可以尝试处理 300 多种不同类别的涂鸦。

# 在快速绘制中使用 RNNs

如果你想变得有趣，使用完整的数据集(公平的警告，它相当大！)，你可能想使用递归神经网络(RNN)来完成这项工作，因为它会从笔画的*序列*中学习。在一个奇妙的事件中，有一个专门针对在快速绘制数据集上使用 RNNs 的指南，所以[如果你有兴趣尝试一下，请查看教程](https://www.tensorflow.org/versions/master/tutorials/recurrent_quickdraw)。考虑到训练时间，第一次可能只对数据的一个子集这样做:)

# 快速绘图的数据探索与可视化

如果您想进一步研究数据集，可以使用 Facets 可视化 quickdraw 数据集。Facets 团队甚至冒昧地将其放在网上，并给了我们一些预置来玩！你可以[在这里](https://pair-code.github.io/facets/quickdraw.html)进入页面。我们可以加载一些随机的椅子，看看不同的玩家如何从世界各地画出椅子。

![](img/874c3908745b9a360f445824128c830b.png)

All the chairs

我们还可以看到哪些画被认为是椅子，哪些没有被选中。有许多预置视图也值得一试，它们可以作为进一步分析的有趣起点。

感谢阅读本集[云 AI 冒险](https://goo.gl/UC5usG)。如果你喜欢这个系列，请为这篇文章鼓掌让我知道。如果你想要更多的机器学习动作，一定要关注 Medium 上的[me](https://medium.com/@yufengg)或[订阅 YouTube 频道](https://goo.gl/S0AS51)以捕捉未来的剧集。更多剧集即将推出！