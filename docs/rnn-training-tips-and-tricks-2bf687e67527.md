# RNN 训练技巧和诀窍:

> 原文：<https://towardsdatascience.com/rnn-training-tips-and-tricks-2bf687e67527?source=collection_archive---------0----------------------->

以下是安德烈·卡帕西关于训练 RNN 管道的一些好建议。链接到最初来自的[。](https://github.com/karpathy/char-rnn#tips-and-tricks)

# 监控验证损失与培训损失

如果你对机器学习或神经网络有些陌生，获得好的模型可能需要一点专业知识。要跟踪的最重要的量是训练损失(在训练期间打印)和验证损失(对验证数据运行 RNN 时偶尔打印一次，默认情况下每 1000 次迭代打印一次)之间的差异。特别是:

*   如果你的训练损失远低于验证损失，那么这意味着网络可能**过度拟合**。解决这个问题的方法是缩小你的网络规模，或者增加辍学率。例如，您可以尝试 0.5 的辍学，等等。
*   如果你的训练/验证损失大致相等，那么你的模型**不符合**。增加模型的大小(层数或每层神经元的原始数量)

# 参数的大概数量

控制模型的两个最重要的参数是`lstm_size`和`num_layers`。我建议你总是使用 2/3 的`num_layers`。`lstm_size`可以根据您拥有的数据量进行调整。这里要跟踪的两个重要量是:

*   模型中的参数数量。这是你开始训练时打印出来的。
*   数据集的大小。1MB 的文件大约有一百万个字符。

这两个应该差不多一个数量级。有点难以分辨。以下是一些例子:

*   我有一个 100MB 的数据集，我使用默认的参数设置(目前打印 150K 的参数)。我的数据量要大得多(100 密耳> > 0.15 密耳)，所以我预计会严重不足。我在想，我有足够的钱让`lstm_size`变得更大。
*   我有一个 10MB 的数据集，正在运行一个 1000 万参数的模型。我有点紧张，我在小心地监控我的确认损失。如果它比我的训练损失大，那么我可能会尝试增加一点点辍学，看看这是否有助于验证损失。

# 最佳模式战略

获得非常好的模型的获胜策略(如果你有计算时间)是总是错误地将网络变大(大到你愿意等待它计算)，然后尝试不同的压差值(在 0，1 之间)。无论哪种模型具有最好的验证性能(写在检查点文件名中的损失，低是好的)，都是您最终应该使用的模型。

在深度学习中，使用许多不同的超参数设置运行许多不同的模型是非常常见的，最终采用任何给出最佳验证性能的检查点。

顺便说一下，你的训练和验证分割的大小也是参数。请确保您的验证集中有足够的数据，否则验证性能将会很嘈杂，并且不能提供很多信息。