# 假日电影的自然语言处理观——第二部分:用 LSTM 的《在喀拉斯》生成文本

> 原文：<https://towardsdatascience.com/an-nlp-view-on-holiday-movies-part-ii-text-generation-using-lstms-in-keras-36dc1ff8a6d2?source=collection_archive---------28----------------------->

![](img/d0f795f4c8cdce630271e90a3930475c.png)

Photo by [rawpixel](https://unsplash.com/@rawpixel?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

继续这篇博文的第一部分,让我们看看是否可以用输入序列训练一个 RNN，并用它来生成一些新的序列。

这部分的代码可以在[这里](https://github.com/TDehaene/blogposts/blob/master/nlp_christmas/part_2_text_generation.ipynb)找到。完整的代码和数据(在熊猫数据框架中)也在回购中。

# 这个想法

使用 RNN 的工具生成文本已经做了很多次了(在这里插入安德烈·卡帕西的博客文章)。但是尽管如此，当你有大量的文本时，这仍然是一件有趣的事情。对 AI 古怪博客大声喊出来！

除此之外，我还对算法在不同参数下的行为感兴趣。

# 数据

我们对第一部中所有确定的圣诞电影都使用小写字幕。这里有一个片段:

```
" never had. we got bikes, toys, video games, all sorts of stuff. don't those marines or firemen collect toys for those kids? why are we spending the money? it's christmas. oh, bah, humbug. no one ever did anything like that for me when i was a kid. jimmy! when i get out on set, i don't want to see o"
```

# 编码数据

很容易做到:我们将语料库分割成 N 个半冗余的文本片段(作为输入)，紧随其后的字符作为输出(要预测的实际内容)。

我们每次跳过 5 个字符的事实有望成为一种规则，避免记忆整个连续的句子。

输入序列中的每个输入字符都是一次性编码的:

![](img/84419da570b13edac393ab4ef668c319.png)

one-hot encoded input sequence

在这之后，我们将数据分成训练和测试集(20%)。

# 模型

在摆弄了一些超参数之后，我注意到下面的设置在验证损失方面取得了良好的结果:

👉提示:使用 **recurrent_dropout** 在处理 RNN 氏症时，不要在中间添加简单的辍学层(如果你感兴趣，[这篇文章](https://arxiv.org/pdf/1512.05287.pdf)是一个很好的来源)。

👉注意:当堆叠 LSTM 层时，将 **return_sequences** 设置为 True，除了最后一个 LSTM 层。

授予:

```
_________________________________________________________________
Layer (type)                 Output Shape              Param #   
=================================================================
lstm_1 (LSTM)                (None, 40, 512)           1191936   
_________________________________________________________________
lstm_2 (LSTM)                (None, 512)               2099200   
_________________________________________________________________
dense_1 (Dense)              (None, 69)                35397     
=================================================================
Total params: 3,326,533
Trainable params: 3,326,533
Non-trainable params: 0
_________________________________________________________________
```

# 训练时间

该模型训练了 50 个时期，以及一些回调函数:

# 参数研究

为了尝试感受模型在各种参数下的表现，我们将对不同模型中的训练测试损失进行比较。

我们每次都从基本模型开始:

*   2 个 LSTM 层，经常性辍学率为 70%
*   顶部 1 个致密层

为了稍微加快训练过程，我们只使用 20%的数据进行训练，另外 20%用于测试。

当然，一些参数也会严重影响训练时间(数据量、隐藏层数等)。).如果这是一个问题，也应该考虑到这一点！

## 更改落差大小

👉我们可以清楚地看到，增加压差极大地减少了过度拟合

## 更改堆叠 LSTM 图层的数量

👉总的来说，没有太大的区别，2 层收敛更快，但是训练起来更慢

## 更改隐藏状态大小

👉单元越多，过度拟合的可能性越大(学习能力越强)，但收敛速度更快(尽管训练速度较慢)

## 更改输入集的大小

对于最后一次运行，我设置了两个实验，一个有 20%的训练数据可用，另一个有 80%的训练数据可用(同样的 20%每次用于测试集):

不出所料:添加更多的数据会带来最大的性能提升。

这让我想起了 Peter Norvig(以及其他人)的一篇著名论文:[数据的不合理有效性](https://static.googleusercontent.com/media/research.google.com/en//pubs/archive/35179.pdf)

# 输出

摆弄够了，让我们看看输出是否有任何好处！

我们要做的是:

1.  取一个 40 个字符的起始种子句子
2.  编码它
3.  预测下一个第 41 个字符
4.  将窗口向前滚动一个字符，以便预测第 42 个字符
5.  重复

为了预测下一个字符，我们使用“温度”原理。我们不是简单地选取概率最高的字符，而是从一个概率分布中取样(为什么会这样马上就清楚了):

让我们看看这是如何发挥作用的。起始种子是:

> "那是圣诞节的前一天晚上，t . "

## 使用温度= 0.00

> **那是圣诞节的前一天晚上，我成了圣诞树的一部分。我不知道我是最好的圣诞树。我不知道我是最好的圣诞树。我不知道我是最好的圣诞树。我不知道我是最好的圣诞树。我不知道我是最好的圣诞树。我不知道我是最好的圣诞树。我不知道我是最好的圣诞树。我不知道**

## 使用温度= 0.25:

> **那是圣诞节的前一天晚上，我感到非常抱歉。我想我很抱歉我很抱歉。我不知道我和那个问题一样。你猜怎么着我不知道我有多漂亮。我在想，最好的圣诞夜圣诞树是最好的圣诞礼物之一，圣诞晚会的圣诞树让我大吃一惊。对不起，我会说我太美了。我不知道。我不知道**

## 使用温度= 0.50:

> 那是圣诞节的前一天晚上，几年前我本打算让你参加假日射击赛。我会跟你谈的。你认为我和我最喜欢的人做了那么多，然后我把我们带到了树上。我想我很抱歉。我不知道。他们做了什么？呃，好吧，有个舞会，我得去拿些我不能说的关于树灯的东西。不不不不太好了。现在，我只是一个坏想法

## 使用温度= 0.75:

> **那是圣诞节的前一天晚上，t** 他看着自己。你有点太固执了。这对你来说是一种力量。嘿，我得走了。先生。好吧，我会和所有新饼干的负责人谈谈。[笑]我不能说不。不，不，不，不，无论如何。嗨。珍妮小姐的艾米丽你不会很好的。那是我父亲的感冒。[笑]什么？有六天时间我要离开他们。是啊，难道你不知道吗？n

## 使用温度= 0.9:

> **那是圣诞节的前一天晚上，请给我一棵好圣诞树。你说得对。伙计，提尔不会来帮忙了吗？这是他们从来没有谈过的交易。哦，何亚！呵呵。不，我知道这不是你不了解我的原因。你会没事的。嗯嗯。你知道你去哪里奇怪。哦，嗯，让我看看那一年妈妈可以变身了。欢迎来到**

我们看到，使用概率最高的预测字符(温度= 0)只会将我们带入一个重复的循环。

提高温度会赋予模型更多的“创造力和自由”。在某种程度上,[叹息]和[笑声]混在了一起。

气温升高还会增加出现晦涩难懂的单词的风险，比如“goving”和“sost”，以及不连贯的句子。

# 故事板

嗯，看起来这位模特肯定学到了一些圣诞概念，比如树和灯。

尽管如此，这似乎还不是一个完整的电影剧本😕…

也许如果我们添加一些图纸，事情会看起来更好！让我们试试上面生成的内容(温度 0.5)

> 👉*提示史诗绘画技能*

![](img/023fc9110c79d0f6de4759cd62459ae2.png)

嗯…不是真的，虽然它是…

现在让我们把电影写作交给专业人士吧。

如果有人想玩的话，我也上传了模型的最佳重量到 GitHub。

更多生成的片段(种子每次都以粗体突出显示)，它甚至似乎在某个时候制作了一首歌曲:

```
**what do you mean you forgot the presents?** i think that you would like to see you that parade. ♪ ♪ we wish you a terrible and children ♪ ♪ the annie last night ♪ ♪ i would love to a project ♪ ♪ and the way ♪ ♪ it's a little bit ♪ ♪ we're selling a little bit since when it was a camera for you. i don't know what i mean. i know that i'm going to be pretty good to ask you again. come on, come on. i mean, i was a consider make one of your li**it was the night before christmas, and t**he problem to the prince was that i would have to be the bigger of the family. you were about the boys and the holidays. and i don't know. i love the store. i don't know what i don't know what i see that is a decorating christmas tree and i have to get it. i don't know what i don't know what i was the boys and i can do it for the company of the company this company and the tree is the boys and i have**i don't know jake, the christmas tree** in new york can be a christmas pageant. i mean, i can't take it to the best virgin suits and the holidays were making this year's man. you know, it's been a lot of ready to be the past for us and we'll get back to the store that you can ask you a movie. it's good to make a boyfriend. yeah, i think you're gonna be next week. well, he's a sweet. i could be in the day that you don't mind if i don't want t**are you okay, didn't you hear the jingle**? i mean, i broke my mom so look at this. well, i have to be a sec. i'm so sorry. it's not a basketball, i don't think that's really good. thanks. i love you, and i wish you two pass. i know. i do... you know. i think i'm thinking about your life. the book. so thank you. so let's just the book room. oh, good to get ahead. so you can relat**are you okay, didn't you hear the jingle**? well, i didn't think that's cool. i know it, that's what i was the perfect for the holidays. doesn't see you and i wish you a very nice guy. you know, i told you it mean it cookies. you know, i told you it mean it. we're going to be a surprises. i know. i think i'm the best christmas trees. i think i know what they have been for christmas? maybe i could see why you don't want to go to play that yo
```

# 恰当的标题

不亚于一个圣诞奇迹，AI weirdness 刚刚发布了一篇关于为一部圣诞电影命名的博文。以下是其中的一些:

> 圣诞生物案例
> 春天的圣诞节
> 一只圣诞猫
> 一只圣诞虱子
> 接近圣诞节
> 树屋圣诞节
> 圣诞节的喧闹

就这样吧，如果你有任何反馈、意见或想法，请随时告诉我。

节日快乐！