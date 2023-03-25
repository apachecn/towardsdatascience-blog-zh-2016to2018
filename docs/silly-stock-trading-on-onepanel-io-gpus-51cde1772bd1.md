# Onepanel.io GPUs 上的傻股票交易

> 原文：<https://towardsdatascience.com/silly-stock-trading-on-onepanel-io-gpus-51cde1772bd1?source=collection_archive---------11----------------------->

![](img/a1f34a2f8a0bc16f9493a35e34c437c2.png)

ur 首席执行官 [Mathieu Lemay](https://medium.com/u/f84a70d8f74?source=post_page-----51cde1772bd1--------------------------------) 最近写了一篇关于[各种流行的人工智能模型在罗夏墨迹测试](/rorschach-tests-for-deep-learning-image-classifiers-68c019fcc9a9)中看到了什么的文章。那很有趣。这让我想到当只有噪音时，我们人类能看到多少信号。我想看看当我们使用深度学习**通过首席执行官的个人资料照片预测一家公司的股票市场表现，并使用首席执行官的星座**交易股票时会发生什么。我们与 [Onepanel.io](http://Onepanel.io) 合作，在 GPU 上实现这个想法。

![](img/d680532e1745db7fc2eb8d31087536d9.png)

Using AI to interpret horoscopes and generate alpha. [Mary Kate MacPherson](https://medium.com/u/c1c775be8058?source=post_page-----51cde1772bd1--------------------------------) made the bot a logo and everything. The tagline is “Your Fortune Awaits!”

当然，这些都是*可怕的想法*。你能从这里学到的很简单:坏主意会导致糟糕的模型性能。更重要的是，在数据科学确认该项目在统计学上有意义之前，你不能进行机器学习。

我将使用 [Onepanel.io](http://Onepanel.io) 平台带您了解这个人工智能项目从注册到项目执行的全过程，这会让您感受到我们从他们的解决方案中获得的价值。给你一点背景知识，我非常喜欢 DigitalOcean，但是他们没有 GPU 实例。当您想要在分布式协作中为深度学习项目启动一个开发实例时，AWS 很棒，但是很贵，而且有些机械，即使是从 AMI。我们从 [Onepanel.io](http://Onepanel.io) 中得到的，你将在本文中看到的，是一个平台，我们可以在这个平台上轻松地在一个 GPU 项目的团队中协作，而无需安装并运行所有的库，共享密钥等等。这是一个很棒的界面。

这篇文章的代码可以在 Onepanel.io 这里找到:[https://c . onepanel . io/Daniel-lemay-ai/projects/star-sign-trade-stocks/code](https://c.onepanel.io/daniel-lemay-ai/projects/star-sign-trade-stocks/code)

## 开始:为您的团队旋转 GPU 的 10 个步骤

根据星座买卖股票的第一步是通过[注册程序](https://c.onepanel.io/register)。

![](img/679dd5d46f4858e7bbee529ef735257a.png)

不要忘记点击你的电子邮件中的链接来验证你的电子邮件地址。如果你被提示输入信用卡，那是为了支付你将要启动的 GPU。有[指令做这一切东西](https://help.onepanel.io/getting-started/core-concepts)，但是 TL；像我这种 DR 的人，就是想飞过去，有一个有 GPU 库，源码控制，安全等等的工作环境。

![](img/9e2aad82944cdd1c8e1c3369d5226040.png)

Log in and make a project

![](img/02760826b777a717a6a83bbb389dc35e.png)

Set the details of the project

![](img/dd6212c22862e303a93cbffd1f888462.png)

Create a project workspace

![](img/1ec1319f056ea7f42453e3f78011b9fa.png)

Set the workspace details

![](img/9b31a22db71ae24a8209c01649617697.png)

The instance will take a few minutes to spin up, so note the time message and the status color

![](img/098982e49c08653364c217b309fa5b39.png)

While we wait, let’s invite our team members to the project. No SSH key exchange required!

![](img/5c40f643e01424bc4669be5072cf565a.png)

Great! Our instance is up. Let’s run Jupyter. Notice that tensorboard and terminals are also available with one click in the LAUNCH menu.

![](img/ffe93b533331775b52d7f974052ec3ed.png)

Make a new notebook to show the GPU is working

![](img/76cf135d84f9f9ecb76b5570c8fb5601.png)

Copy-paste [example code from keras.io](https://keras.io/getting-started/sequential-model-guide/) Running!

[这两条线](https://stackoverflow.com/questions/44544766/how-do-i-check-if-keras-is-using-gpu-version-of-tensorflow)显示 GPU 正在工作:

```
from tensorflow.python.client import device_lib
print(device_lib.list_local_devices())
```

![](img/4c3c4e34a0af49db57dfcc3060bd7e3b.png)

OK. It all works!

我们现在有了一个低成本的 GPU 实例，安装了所有正确的东西。那很容易。我们能够共享对项目的访问，这样我们就可以在团队成员之间进行协作。现在是时候用系统做项目了！

## 利用占星术进行投资组合管理

让我们来看看星座和股票交易之间的关系是如何吸引人，但却毫无意义。即使我们找到了相关性，它们也应该是随机的，而不是因果关系。那是因为星座是随机的，而随机，虽然经常很优雅，却什么都预测不到。先从**根据各公司 CEO 的星座投资股市**开始，看看会发生什么……好玩！

[](https://github.com/dcshapiro/onepanel) [## dcshapiro/onepanel

### GitHub 是人们构建软件的地方。超过 2800 万人使用 GitHub 来发现、分享和贡献超过…

github.com](https://github.com/dcshapiro/onepanel) 

## 搜集星座:建立一个数据集

我使用 Travis Riddle[的](https://www.linkedin.com/in/travisriddle/)[代码](http://www.travisriddle.com/Scraping-Horoscopes/)从《纽约邮报》获取星座数据。我只是做了一些修改，让它能在 Python 3 中工作。我还对脚本做了一点修改，使它每天抓取所有的标志，而不是一次一个标志。如果主机生气并阻塞了我的虚拟机的 IP 地址，这使得脚本更容易恢复。我没有使用花哨/昂贵的 IP 轮换策略，我只是[让我们撕开](https://idioms.thefreedictionary.com/let+%27er+rip)看看会发生什么。

首先，我在 3 天的范围内运行脚本。效果很好。接下来，我在 2014 年 1 月 1 日到 2017 年 12 月 31 日的日期范围内运行它。结果进来的时候就存了，以防万一。只坠毁过一次。我继续说道。完成了。那是 4 年的星座数据。我很确定这属于[公平使用条款](https://fairuse.stanford.edu/overview/fair-use/what-is-fair-use/)，但出于版权原因，我不会发布数据集。

![](img/d3d3eec3344e468f42b972e0f2a581de.png)

Here is a look at the raw horoscope text, publication date, star sign, and sentiment

![](img/fbc5db9fea10876bdc4c73083d710802.png)

Horoscope sentiment is a bit more positive than negative, which makes sense for a feel good marketing thing

共有 17，186 行星座数据:4 年中的每一天对应一个星座(12*365*4 = 17，520)。所以我们每年会错过大约 7 天的星座运程。我假设那是没有论文发表的时代，所以没有占星术。现在我们有了“我能预见未来”的数据，让我们来学习数据和市场之间的关系，每天根据这些“信息”进行交易。

仅供参考， [Onepanel.io](http://Onepanel.io) 有一个平台的数据集部分，你可以毫无痛苦地获取数据集来训练模型。它有像 MNIST 和许多其他的东西，一般来说比你自己把数据拉进一个公共数据集要少。

![](img/fda795c5d4786a604aebd2786ca04f0b.png)

All the data.

您还可以将 S3 桶或任何东西捕捉到正在运行的实例上，或者将您自己的数据集添加到系统中。

我们通常解释文本的方法是使用 [spaCy](http://spacy.io) 将星座文本转换成向量，或者使用 [FastText](https://fasttext.cc) 训练一个定制的嵌入模型，然后训练一些神经模型从输入数据中预测东西。但是让我们简单点。我使用 [TextBlob](https://textblob.readthedocs.io/en/dev/) 将文本转换成基于情感的决策。这并不完全是现代投资组合理论。要使用深度学习来计算[真实资产管理的资产分配，你需要大量的 GPU 和专业知识。对于这个项目，我们决定保持它的美好和简单。](http://investifai.com)

```
from textblob import TextBlob
def sentiment(horoscope):
    analysis = TextBlob(horoscope.lower())
    if analysis.sentiment.polarity > 0:
        return 1
    elif analysis.sentiment.polarity == 0:
        return 0
    else:
        return -1
```

为了决定我们是否应该交易，我们测量星座运势，如果它是积极的，我们买入一只股票(或者持有，如果我们已经在)，否则我们卖出。我们把钱平均分配在首席执行官的星座运势为正的股票上。我们跟踪持有情况，以了解我们的投资组合价值随时间的变化。我们将该策略的盈利(亏损)与投资组合的同等权重买入并持有策略进行比较，看看我们的积极策略是否跑赢了市场。让我们非常慷慨地假设费用是 0 美元，尽管实际上费用和税收在主动自动交易策略中起着巨大的作用。

![](img/7aba148cd6a7ef4cabc7441bf756cc56.png)

Our data source gives a whole new meaning to [non-traditional data sources](https://www.import.io/post/financial-analysts-rely-on-non-traditional-data-sources-for-more-informed-investment-decisions/) used to make investment decisions.

现在，在我们进入图表之前，我们需要选择一个投资组合，并获得首席执行官的生日。

我们用 [stocker](https://github.com/WillKoehrsen/Data-Analysis/tree/master/stocker) 来模拟股票市场。最后，由于 stocker 中的怪异之处，我们只是从 stocker 中取出股票历史数据集，并使用这些数字来做我们自己的回溯测试模拟。系统中的公司列表(tickers)可以在[这里](https://github.com/WillKoehrsen/Data-Analysis/tree/master/stocker/data)找到，这是寻找首席执行官的起点。

One CEO per astrological sign. What could possibly go wrong?

## 结果

我们模拟了 5 年的每日交易，以下是我们的发现:

```
**Psychicbot**
Initial Investment: $100.00
Final Balance:      $200.34
Annualized Return:    19.0%
```

![](img/e4dac33ca20c4b9fe10d66f143ee61b0.png)

**我们赚了$$$！？一个基于星座的交易机器人是如何赚钱的？**

![](img/d588b732b2a88b22b5e7ac3b80f94a01.png)

Hold on….

所以，那是 ***理应失败的*** 。为什么会成功？好吧，让我们来看看这是否只是我一生中最大的牛市中的测量结果。让我们后退一步，将结果与这些股票以及 IVV 的基准进行比较。如果我们赚了钱，但少于[买入并持有](https://www.investopedia.com/terms/b/buyandhold.asp)的[等权重](https://www.investopedia.com/terms/e/equalweight.asp)投资组合，或 [ETF 的市场](https://www.ishares.com/us/products/239726/ishares-core-sp-500-etf)，那么这是一个赚钱策略，比简单地持有市场赚得少。这个想法[叫做 alpha](https://www.investopedia.com/terms/a/alpha.asp) 。从逻辑上讲，我们不应该从没有信息的东西中赚钱，所以 alpha 应该是 0 或者负的。所以，让我们来看看…

```
**Equal Weight (EQWT)** Initial Investment: $100.00
Final Balance:      $177.75
Annualized Return:    15.5%
```

![](img/37cd96c1e15883b7a3dc87195b56517e.png)

**我们比 EQWT 多赚了$$$！？**

![](img/bd9f11b08a3b9bea605bc0adc5ec4838.png)

And this is how the IVV index did over the same period… See: calculator [here](https://www.ishares.com/us/products/239726/ishares-core-sp-500-etf#/) and growth rate formula [here](http://www.investinganswers.com/calculators/return/compound-annual-growth-rate-cagr-calculator-1262).

所以……IVV 的年增长率为 9.86%，这大约是同等权重结果的 2/3，大约是我们星座情绪机器人回报的一半。

![](img/2c5e2be812a1b85df027aa8971b13f78.png)

那么…为什么用星座运势交易股票比传统投资策略更有效呢？？魔法？

让我们总结一下我们的现状:

```
**Annualized Return:**IVV                 9.9%  
Equal Weight       15.5%
Psychicbot         19.0%
```

![](img/44f69530e1d30556760e9a2cdb1ce9be.png)

A quick look behind the curtain.

为什么会成功？

**答案:偏见+运气+不收费=假**

偏见是一种狡猾而微妙的东西，它会毁掉一个精心设计的实验。这里的偏见是，我们只在投资组合中包括首席执行官几年没有变化的股票，而首席执行官的变化通常意味着公司的问题。这给了 EQWT 一个优势。我们当时不可能知道这些公司不会更换 CEO。所以，我们有点不小心作弊了，选择稳定的股票导致我们选择表现良好的股票。这种策略的偏差只是相对于 IVV 而言的。对于 EQWT，我们投资于相同的股票，因此我们的情绪机器人最终在线上或线下的位置基本上是一个随机变量。

此外，我们假设费用为 0 美元。这太低了。在现实生活中，积极的策略，我们可以支付 50%至 100%的总利润的费用。有时更多。净收入在这里很重要，因为净利润被再投资，所以有复合效应。积极策略中较低的费用会带来更高的利润。相比之下，IVV 等 ETF 的费用非常低，EQWT re-balanced monthly 等策略的费用也非常低。

![](img/e933a3f33e83d0778d4994729a9ec806.png)

Source: Chapter 3 of [Narang, Rishi K. *Inside the Black Box: A Simple Guide to Quantitative and High Frequency Trading*. Vol. 883\. John Wiley & Sons, 2013.](https://www.amazon.com/Inside-Black-Box-Quantitative-Frequency/dp/B00C7F4KAW/ref=sr_1_1?s=books&ie=UTF8&qid=1532309212&sr=1-1&keywords=inside+the+black+box+second+edition)

那么，你应该相信这种基于占星术的投资策略吗？绝对不行！好运并不意味着好主意。

![](img/3858fa69948c49f23a55dd1ae80c591e.png)

Bias is totally a thing (credit: [xkcd](https://xkcd.com/1827/))

有时，一种方法的不可能性或愚蠢性并不像这里介绍的基于占星术的投资组合管理方案那样明显。有时候很难知道这种方法没有价值。如果你想证明一个策略是错误的，有时你会遇到一些策略，它们的阿尔法值来自偏见，或者仅仅是运气。想象一下，在根据星座运势交易时，有多少 12 只股票的组合，谁的首席执行官生日正确，而且赚的钱不比 EQWT 和 IVV 多。甚至像修改测试周期这样简单的事情也会让“阿尔法”蒸发。你可以用数字撒谎，但纸牌屋很快就会倒塌。当策略转移到纸上交易时，回报不会实现。或者，当参数被测试时，理论就分崩离析了。使用深度学习的真正投资组合管理(例如[invest fai](http://Investifai.com))不仅要对数字，甚至对数字所基于的假设进行严格的测试和再测试。阿尔法模型不仅仅是数字；它们是关于想法的。

这又是一次模拟，但这次考虑了使用滑索的交易成本:

![](img/593337c0735731b2586a2865408f6cbf.png)

Portfolio value using zipline. Initial value is 1 billion dollars. When taking into account fees, we lose almost $1e15 (Also known as **$15,000,000,000**) before swinging up to $2e10.

上面的图表看起来更真实。所以，基本上，因为我们不能负债 150 亿美元，这个模型在第一年就损失了所有的钱。即使我们一开始有 10 亿美元，我们也会全部输掉。之后的一切都无关紧要了。是的，它最终依靠杠杆推动市场上涨，但这是不相干的怪异之处，因为没有保证金账户让我们在那一点上仍然存在。现在我们终于可以看到这个策略是一个可怕的想法。即使存在知道哪些股票几年内不会更换 CEO 的偏见，以及在没有费用的情况下产生超额回报的运气，这种策略也会损失 10 亿美元。

现在你可能很好奇为什么收费这么高。开始了。如果你通过卖出其他 100 美元的股票来买入一只 100 美元的股票，你在买卖时要支付费用。比方说[的费用是 1%](https://www.investopedia.com/terms/b/brokerage-fee.asp) ，尽管机构投资者的费用要低得多。这意味着我们通过卖出和买入移动 100 美元的成本为 1+0.99 美元，总共 1.99 美元(忽略按实际美元金额购买股票的[量化误差](https://en.wikipedia.org/wiki/Quantization_(signal_processing)))。所以我们需要在交易中获得 2%的利润才能达到收支平衡。即使费用只有几个[基点](https://www.investopedia.com/terms/b/basispoint.asp)，但[公司仍然是有利可图的](https://en.wikipedia.org/wiki/Rake_(poker))，因此该战略需要做得比机会更好，并有足够的利润来支付费用，以避免仅在费用上就达到 0 美元。

产生扣除费用后的净利润的策略是资产管理公司所依赖的阿尔法模型。这些模型需要大量的 GPU 计算能力来运行模拟，所以我们应该更多地使用这些强大的 GPU。

## CEO 直面公司业绩

好的。所以我们展示了用占星术交易看起来像一件事(其实不是)。一些更具预测性的模型怎么样？现在，我们已经收集了 CEO 星座的数据集，让我们使用股票代码+“CEO”的图像搜索结果，获取数据来训练一个模型，以预测每家公司的回报。例如:“APPL 首席执行官”。我们知道这应该没有成功的机会。现在让我们通过做实验来证明它。

![](img/a25cbc8f36fb6ee881545950da0ad39b.png)

Image search result for “APPL CEO”

![](img/e08a4227dde108e6f7a3ba7559bcbc20.png)

Gobbling up pictures from the interwebz...

首先，我们使用图像搜索 API 获取首席执行官的图像。我们可以使用谷歌的“知识图搜索 API”从公司代码中获取 CEO 的名字。我们还可以使用图像识别 API 或基于 dlib 的库来检查图像中的人脸，并丢弃没有人脸的照片。

让我们看看我们从什么开始:

![](img/2ac97592cd590015de871626b2bcbb34.png)

显然，对于一些公司来说，刮刀没有得到任何结果，而另一些公司找错了人，比如奥巴马总统和公司的某个人站在一起。有时候照片完全不相关。我们通过过滤掉不包含人脸的图像来丢弃它们。受欢迎的人物会跳到这些文件夹中，因为他们与相同的 CEO 和公司关键词相关，例如“埃隆·马斯克”、“比尔·盖茨”、“比尔·阿克曼”、“沃伦·巴菲特”等等。在[移除重复图片](https://medium.com/@lemaysolutions/a-dozen-helpful-commands-for-data-nerds-9619bc43c5c6)之后，我最终收集了大约 7GB 的图片进行分类。我们可以抓取多少图像没有真正的上限。你只要坚持下去，直到你不再喜欢它。图像文件的数量是 39，262。

![](img/44b14e48008001e1c7405f7e290658ec.png)

我们[收集了 2305 家公司的价格数据](https://github.com/dcshapiro/onepanel/blob/master/CEO%2Bphoto%2Bperformance.ipynb)，其中 1596 家在测试期结束时价值更高。69%的公司价值更高。这还没有考虑股票分割或股息。另外，请注意 stocker 从 Quandl 获取数据，他们每天只允许您获取 50 个查询，除非您有帐户(免费的那种)。因此，没有帐户会导致代码停止运行。我们继续…

我们需要在 GPU 中进行这种将图像嵌入的处理。要将数据集上传到 S3 存储桶，请执行以下操作(使用真正的 AWS 密钥):

```
# install AWS cli
pip install awscli# set the AWS credentials
export AWS_ACCESS_KEY_ID=BLAHBLAHBLAHKEYGOESHERE
export AWS_SECRET_ACCESS_KEY=SECONDRANDOMSTRINGYTHINGHERE# upload files in current directory to onepanel-lemayai-datasets
aws s3 sync . s3://your-awesome-dataset/
```

并从 CLI 将数据集下载到实例，如下所示:

```
# set the AWS credentials
export AWS_ACCESS_KEY_ID=BLAHBLAHBLAHKEYGOESHERE
export AWS_SECRET_ACCESS_KEY=SECONDRANDOMSTRINGYTHINGHERE# download files in current directory 
aws s3 sync s3://your-awesome-dataset/ .
```

我们可以使用 [Onepanel.io](http://Onepanel.io) 的[作业功能](https://help.onepanel.io/jobs/creating-jobs)来完成这些 GPU 图像转换和分类任务。我在过去讨论过 SageMaker 如何比独立的虚拟机好得多，因为它们只在你关心的任务期间运行。作业以同样的方式工作，但是不会将您局限于定制的 SageMaker 库。

与此相关的是，SageMaker 在本地不可用，但 **Onepanel.io 正在为整个平台开发本地私有云部署选项**，这对我们的许多企业客户(例如政府、医疗和金融科技)至关重要，他们无法将其数据集迁移到公共云。

现在是有趣的部分。我们把图像转换成嵌入！如 [keras 应用页面](https://keras.io/applications/)所述，我们可以将图像转换成嵌入向量。

第一步是知道层的名称。我们用下面的代码得到这个结果:

从这里我们知道我们想要的输出图层在最后，就在实际的分类任务之前。如果我们选择“展平”的输出，我们会得到一个非常大的形状(每个图像 25，088 个要素)，因此我们在输出中选择第一个完全连接的图层的输出，该图层只有 4，096 个要素。

最后，我们可以将嵌入向量设置为输入数据 **(x)** ，将相应的盈亏价格设置为输出数据 **(y)** ，以训练一个 DNN **(f)** ，根据嵌入向量预测价格。每个预测都采用 **y=f(x)** 的形式。借助 [test_train_split 函数](http://scikit-learn.org/stable/modules/generated/sklearn.model_selection.train_test_split.html)，我们可以根据训练数据进行训练，并使用测试数据验证结果。

这是二进制分类的一个很好的例子，在 keras.io 网站的“[二进制分类的 MLP](https://keras.io/getting-started/sequential-model-guide/)”下也有一个很方便的例子。

下面是将图像数据集转换为 CSV 文件中的嵌入向量的代码:

下面是用于训练和测试二元分类模型的代码:

最后，结果如下:

![](img/6bba4b49cf9ea0c6331a59b3f9f47945.png)

Why are there more TRUE than FALSE?

为什么真的比假的多？我们知道这不是任何类型的集合不平衡，因为我们在代码中对此进行了修正。如果这个结果是真的，那么我们可以用 CEO 的脸来预测股市的赢家和输家……这是怎么回事？？

![](img/7379b67b3f3c4063595fc45a08b62539.png)

事实证明，我们的刮刀刮出了许多小变化的相同图片(例如上面提到的沃伦·巴菲特和埃隆·马斯克)，这导致了一些测试和训练数据非常相似。移除完全重复的图像并不是一个足够强大的过滤器来真正消除数据中的这个问题。有效地拥有同一个 CEO 的多个形象，并在培训和测试之间认出这些 CEO，就是作弊。所以，再次强调，不要因为结果好就相信结果好。相信他们，因为他们有意义，并帮助你预测事情。

如果你正在经历脚本和驱动程序安装的痛苦，或者通常不喜欢启动和运行机器学习基础设施的过程， [Onepanel.io](http://Onepanel.io) 可能会让你非常感兴趣。新功能会定期上线。这个解决方案被打包到一个简单的 web 界面中。它只是工作。因此，总之，在本文中，您从头到尾详细了解了一个机器学习项目，包括启动服务器、连接团队、收集数据集、对股票数据进行一些分析，以及以一些预测性的东西结束。你看到了你不应该仅仅因为投资策略赚钱就相信它们。

如果你喜欢这篇文章，那么请点击关注按钮，拍拍这个东西，看看我过去读过的一些文章，比如“[如何为人工智能项目](https://medium.com/towards-data-science/how-to-price-an-ai-project-f7270cb630a4)定价”和“[如何聘请人工智能顾问](https://medium.com/towards-data-science/why-hire-an-ai-consultant-50e155e17b39)”另外，[查看 Onepanel.io](http://Onepanel.io) 。

在下一篇文章中，我将介绍我们已经研究了很长时间的东西，它可以帮助企业在内部审计期间自动分析非结构化报告。

下次见！

丹尼尔

[daniel@lemay.ai](mailto:daniel@lemay.ai) ←问好。
[LEMAY . AI](https://lemay.ai)
1(855)LEMAY-AI

您可能喜欢的其他文章:

*   [人工智能和不良数据](/artificial-intelligence-and-bad-data-fbf2564c541a)
*   [人工智能的图像数据集](/image-datasets-for-artificial-intelligence-bbb12615edd7)
*   [人工智能:让你的用户给你的数据贴上标签](https://medium.com/towards-data-science/artificial-intelligence-get-your-users-to-label-your-data-b5fa7c0c9e00)