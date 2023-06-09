# 用神经网络生成文本

> 原文：<https://towardsdatascience.com/generating-text-with-lstms-b0cdb6fc7bca?source=collection_archive---------2----------------------->

![](img/b6ad4bbb37bc101971508f8b73392060.png)

Can Neural Networks learn to Write?

有许多文章因声称“用 lstm 写另一部哈利波特”或“用 lstm 写说唱歌词”而获得了人气。LSTMs 属于一类更广泛的神经网络模型，称为递归神经网络。“循环”部分来自模型按顺序处理数据的能力，以及将以前的输入值作为最新输入值的输出因素。

序列处理是一个非常有趣的问题，可以应用于价格建模、视频处理，以及我们将在本文中讨论的文本处理。

![](img/d07b9649e28ff95a42593e062f96b4cb.png)

Letters vs. Words, what is the foundation for language in Neural Networks?

当将 RNNs 用于文本、字符和单词级模型时，有两种主要的二分法。这是指将文本编码成整数以便模型处理的方式。在 char 级模型中，我们将每个字母标记为字母语料库中的一个 hot 向量。在单词级模型中，我们将每个单词标记为单词语料库中的一个热点向量。

尽管对每个单独的字符进行编码可能看起来更加繁琐，但这里要避免的瓶颈是结果向量的稀疏性。在文本的训练数据集中通常只使用大约 60 个字符，在同一个数据集中很容易使用 1000 个独特的单词。有很多方法可以使用单词嵌入来尝试和减少单词级编码的维数，但是到目前为止，字符级编码对于简单的推理来说会产生更好的结果，即编码的向量具有更密集的信息。

**字符级网络中的一键矢量编码示例 *(*** *如果你已经熟悉这个请跳过):*

语料库:['a '，' b '，' c '，' d，' e '，' f '，g '，' h '，' i'] — len(语料库)= 9

单词:['坏'] → ['b '，' a '，' d']

b → [0，1，0，0，0，0，0，0，0]，a → [1，0，0，0，0，0，0，0，0，0]，d → [0，0，0，0，1，0，0，0，0]

一些采用这种策略的人选择不使用一键向量编码方法，而是用 ASCII 码对每个字符进行编码，比如 a = 65。以这种方式编码字符的问题是，整数暗示了字母之间的某种顺序，例如:a < b < d。在实际语言中，字母之间的这种组织是不存在的。

当处理神经网络，尤其是具有许多层的深度神经网络时，我们希望尽量避免这样的事情，因为很难确切地说出网络在训练后学到了什么。一个著名的例子是在森林中探测坦克的军事实验。包含坦克/不包含坦克的数据集仅在晴天拍摄了坦克，在阴天拍摄了不包含坦克的田地。因此，深层 CNN 模型学会了对天气进行分类，而不是对坦克的存在进行分类。

一旦将文本编码成向量，就该使用 LSTM 来训练模型了。在这个代码示例中，我们将使用构建在 Tensorflow 之上的 Keras 库来大大简化这项任务。

```
from keras.models import Sequential
from keras.layers import Dense, LSTMmodel = Sequential()
model.add(LSTM(75, input_shape=(X.shape[1], X.shape[2])
model.add(Dense(vocab_size, activation='softmax'))model.compile(loss = 'categorical_crossentropy'
              optimizer = 'adam',
              metrics = ['accuracy'])model.fit(X, y, epochs = 100, verbose = 2)
```

正如我们在上面看到的，建立神经网络并不太困难。与生成文本相关的大部分工作(像许多 ML 项目一样)，是对文本进行预处理，对文本进行标记并将其转换成向量，然后将其输入模型。

我用 JCole 歌词做了一个实验，看看这个模型是否能继续这个序列。该模型在训练中报告了 80%的准确率，但当我给出这些例子时，结果并不令人印象深刻。我们确实注意到该模型不能很好地概括，即使两个输出都不令人满意，但是当训练数据中包含起始种子时，该模型表现得更好。

**示例 1:(训练集中包含的起始种子)**

“没有角色模型，我是她”，20 #歌词包含在训练集中，20 个参数指示模型再生成 20 个角色

模型输出→没有榜样，而我在这里是因为他们没有

(至少在某种程度上这是英语)

**示例 2:(起始种子不包含在训练集中)**

跳入水中，20 #歌词不包含在训练集中

输出→跳入水中，furby 调用 som

这些结果如此糟糕的主要原因是因为我只给模型输入了 500 行 JCole 歌词，如果你有兴趣自己尝试这个实验，我建议使用 kaggle 的 55，000 首歌曲歌词数据集。你应该预料到训练这个模型也需要一些时间，除非你有 GPU 机器或者计划在云上训练你的模型。

如果一个字符或单词级别的递归网络可以生成类似电视节目中的对话或书籍节选的内容，我会感到惊讶。我也对如何使用生成对立网络来生成文本非常感兴趣。有许多流行的论文使用文本生成方法，标题如:“神经网络撰写研究论文标题”。训练这些网络需要大量的数据、计算能力和深度模型，所以如果在本地计算机上运行，请做好准备。

感谢您的阅读，希望这篇文章能帮助您理解 RNN/LSTM 序列网络的文本生成！如果你想用 Python 和 Keras 详细浏览你的第一个角色级 RNN，下面是一篇很好的文章:

[https://machine learning mastery . com/develop-character-based-neural-language-model-keras/](https://machinelearningmastery.com/develop-character-based-neural-language-model-keras/)

# [CShorten](https://medium.com/@connorshorten300)

Connor Shorten 是佛罗里达大西洋大学计算机科学专业的学生。对数据科学、深度学习和软件工程感兴趣。主要用 Python，JavaScript，C++编码。请关注更多关于这些主题的文章。