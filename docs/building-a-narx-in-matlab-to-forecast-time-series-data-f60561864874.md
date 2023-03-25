# 在 MATLAB 中建立 NARX 来预测时间序列数据。

> 原文：<https://towardsdatascience.com/building-a-narx-in-matlab-to-forecast-time-series-data-f60561864874?source=collection_archive---------2----------------------->

![](img/12fb4c998ea31fc5924625a50a7aa818.png)

The Kenduskeag at high flows. At lower flows (you can literally walk out to that rock in the middle).

去年我发表了一篇[文章](https://medium.com/@paddlesoft/forecasting-the-flow-of-the-kenduskeag-a731c5d35aa6)(在 [PaddleSoft](https://medium.com/u/ffe5d69df3f?source=post_page-----f60561864874--------------------------------) 账号上)，在文章中我描述了使用 MATLAB 中内置的 NARX 预测 Kenduskeag 流的流量。然而，我故意没有包括那么多技术细节，因为我试图为普通读者写。好了，我现在已经创建了一个完整的教程，关于如何在 MATLAB 中建立一个带有外源输入的非线性自回归网络(NARX)来预测时间序列数据。

## 教程

这个过程实际上相当简单:导入数据，创建模型，训练模型，然后进行预测。

这个 [CSV](https://github.com/isaacmg/lstm_stream/blob/master/height.csv) 是我们将在教程中使用的 CSV 类型的一个例子。基本上有三列:高度、温度和降雨量。我们希望使用先前的高度以及温度和降雨量来预测当前的高度。

1.  **导入数据**

首先，我们必须导入数据。我们将使用一个简单的函数来实现:

我们用`[X_input,height]=load_data('height.csv')`调用这个函数(注意，即使我们使用 xlsread，它仍然可以处理 CSV 文件)。

**2。培训**

现在让我们看看我们的培训代码:

让我们稍微分解一下这段代码。我们要做的第一件事是使用 tonndata 函数将我们的输入放入单元格数组。接下来，我们必须选择我们的训练函数。我个人在贝叶斯正则化(即 trainbr)方面取得了最大的成功，但是，这可能需要更长的时间。之后，p [reparets](https://www.mathworks.com/help/nnet/ref/preparets.html) 将为我们的 NARX (24)准备正确格式的数据。

输入延迟和反馈延迟参数非常重要，将极大地影响您的预测。我最初选择的方法是绘制降雨量与高度变化的曲线图，以确定降雨量何时对河流高度影响最大。然后我试验了各种配置。也有更多的数学方法来确定它使用互相关[(见 MATLAB 论坛回答这里](https://www.mathworks.com/matlabcentral/answers/84271-narx-time-delay-estimation))。然而，现在，你可以试着看看什么是最好的。

然后，我们在第 28–30 行做出相当标准的决策，如训练/验证/测试分割，最后训练网络(第 30 行)。代码的其余部分是可选的，所以我不会详细介绍。但是，请随意尝试，了解一下有哪些可用的东西。

**3。做出预测**

现在，为了在 CSV 上进行预测，我们将使用新创建的 height_net。我个人发现最简单的方法是将测试数据分成两个 CSV:一个包含 stamps 0:length-240 的输入(temp 和 rain)和高度值，另一个只包含 length-240:length 的输入(*其中 length 是测试 CSV 中的行数)。这大致类似于 10 天天气预报的预测(即使用一个已知的长时间序列，然后预测从天气 API 获得的 10*24 个时间戳)。你可以很容易地在你的 MATLAB 代码中分割数据，但是我发现这样做最简单。考虑到这一点，下面是我们的预测代码:

我们将已知的时间戳序列传入 *open* height net，得到`xf`和`af`。然后我们关闭网络进行多重预测(12)。最后，我们进行实际预测(13)。你现在可以通过看 y2 或者我们重新分配的 unknownY 来看预测。

我希望这个例子足以让你熟悉 MATLAB 中的 NARX(s)。在未来，我希望更深入地研究 NARXs 相对于 LSTMs 等其他 rnn 的优势。

你可以关注我，也可以查看我写的更多内容。