# 从工程到数据科学预测的一生——我多么怀念物理

> 原文：<https://towardsdatascience.com/engineering-to-data-science-a-life-of-forecasting-and-how-i-miss-physics-71554eb3c807?source=collection_archive---------10----------------------->

![](img/c6e345ad1cf819c2bb1fd426067ebb6c.png)

Finding oil is a long way from my jupyter notebook

在坚定地进入数据科学领域之前，我作为一名石油工程师在石油和天然气行业工作了 5 年。在几个国家和世界各地工作，从几内亚湾的海上平台到西非丛林中的办公室。这些经历塑造了我作为一名工程师和个人的形象，并对我的定量分析前景产生了巨大影响。

我的大部分工作是在生产优化和预测领域。本质上，我们如何利用现有资源(甚至更少)生产更多的石油，并预测未来我们将生产什么。所有这些都是为了了解油田资产的价值及其未来潜力。

我经常看到我作为一名石油工程师使用物理模型所做的事情，与我现在作为一名数据科学家预测消费者对电子商务的需求所做的事情有许多相似之处。然而，在预测模型中遇到的问题有很大的差异。最终，我意识到，即使作为一名工程师，我不得不管理所有可怕的肮脏数据，我仍然非常想念物理学的基础来帮助指导我的决策。当试图理解数据和信息告诉你什么时，工程和物理定律提供了已知的边界条件。当我们分析一个人购买消费品的理性时，这些物理定律有点难以定义。

## 石油和天然气行业的预测

作为一名在中非工作的工程师，我的工作本质上是获取大量实际测量的数据(特征),并将其转换为预测模型，通常是时间相关模型。这种分析以及需求预测的关键是理解数据，观察什么，如何有效地组合变量以最好地代表您的系统，以及处理不确定性的量化。然而，这两种方法有着根本的区别，一种是从使用物理模型开始的工程，另一种是使用统计模型。我个人认为，将两个阵营结合起来是行业前进的基础。

石油预测像任何其他预测一样需要数据，油井通常已经生产了许多年(0-40 年),大多数油井通常至少有一套标准数据:每天生产多少流体、地面压力和温度。有了这些要素，就可以直接进行预测了。问题是，虽然你有数据，但它在质量和数量上有很大差异。这是对工程师的真正挑战。这使它成为一项迷人的工作。

你的数据可能来自地下 3000 米深处的任何地方，每秒钟都会向你的笔记本电脑发送信号，也可能是一个人跳进一辆皮卡或一艘船去参观油井并读取一个量表——后一种情况比你想象的更常见。事实上，我一直是这样的家伙。因此，从收集数据的性质和收集的内容来看，数据是零星的，有时非常不可靠，有时甚至根本不存在。

这实际上是石油行业的症状，很少有一致性，特别是在数据方面，当你开始预测时，理解这一点是有用的。你会发现的为数不多的一致性之一是，随着油价的每一次下跌，你可能也会看到数据质量的下降。这对时间序列预测有重大影响，通常你的数据在油井寿命开始时是可靠的(这是最大的资本投资)，然后随着时间的推移而退化，通过故障传感器、较差的监控和通常不再是公司投资组合中最性感的东西。

除了简单的数据可用性，实际测量还有巨大的不确定性。很难理解油藏及其产出的流体的复杂性，更不用说有时难以相信自己的测量结果了。最后，当你想到一个油藏时，你是在谈论将微小的针刺(井~12 英寸宽)放入 2-4000 米深的地球，然后使用这些作为你在可以跨越数十公里的东西中的唯一眼睛和耳朵。

因此，石油和天然气预测的困难很大程度上来自于您的数据质量和可用性。虽然我把它说得像一个可怕的问题，你会经常看到工程师们努力从他们的数据中获得意义，以表达他们对未来的看法。你也有一个物理系统！这意味着即使我花了几周/几个月的时间开发我的模型，我也可以给另一个工程师 5-10 个参数，他可以在他的计算器上检查我的预测是否在物理上*合理。最后，我们试图预测的过程在宏观水平上受我们在中学物理中学到的简单压力、温度和体积关系的支配。有了物理学，你就有了一个基础真理，一根引导绳。当你告诉某人投资数千万美元时，这会给你更多的信心。*

## *电子商务方面*

*现在，本周我一直在研究一个完全不同的问题，我为一家电子商务公司建立需求预测模型。本质上，我试图预测人们下周将在网站上购买什么，这样我就可以有足够的库存来满足他们的购买需求。这带来了许多限制，我不想买太多的股票(过度预测)，因为这会花我的钱，我也不想低估太多，因为我会延迟履行客户订单(客户不太满意和/或失去客户)。这是一个财务权衡，我不会在这里深入讨论，但这是一个有趣的问题，研究什么是最佳库存数量。*

*我有一组来自销售端的简单数据(我们每天对给定产品的订单),从谷歌分析我可以得到网站的流量信息，通过结合这两个数据集，你可以得到转化率。我正在为大约 20 000 种产品这样做，所以它需要工业化。这也是一个不连续的数据集，你可能一周什么也没卖出去，突然之间就卖出了 10 件产品。*

> ***我意识到我有时会想念物理系统***

*因此，在我的电子商务案例中，与我在油田的生活相比，我正在处理难以想象的干净数据。我有“传感器”来测量人们在网站上做的一切，这些数据的质量很高，不仅是由谷歌收集的:-)，而且最重要的是，这些数据不是由一个值夜班的人收集的，他花了 8 个小时开车或乘船到偏远的地方去读取仪表。更别说我在雨季的凌晨两点试图在笔记本上记下压力了。*

*所以当我最初开始解决这些问题时，我就像*哇*，我沐浴在良好的数据中。但当我开始了解预测的本质时，我很快意识到我没有事实依据，也没有指南。在预测油井时，你总是有物理学的手给你信心。当我向一个朋友寻求帮助，让他给我对下周股票预测的信心时，我突然发现这是一个非常孤独的地方。*

*问题是，没有物理学，很难知道你所做的是对的。明天我不知道我们会卖出更多还是更少，我可以说昨天我们卖出了 X，前天卖出了 y。但是明天我不知道任何具体的东西，我可以给你概率，告诉你我非常确定我们会卖出超过 1，但是我真的对我的系统没有什么限制。在石油预测中，我可以说“昨天我们生产了 200 桶，这意味着压力下降了 x，这意味着今天我可能会少生产 5 桶”来支持我的预测。*

*在油田的最后，你可以创建复杂的概率模型，但在这之后，你可以创建基于物理的确定性模型，给你信心。人口众多的购买心态，谷歌搜索算法或竞争对手广告活动的影响，以及这如何影响我的流量，我不知道。*

*有了需求预测，你可以而且必须继续创建更好的模型，试图捕捉更多可能的用户行为的代理。但最终，这会导致更复杂的模型，变得更难解释。尽管在历史数据上的性能可以得到很大的提高，但它永远不会给人同样的信心(至少对我来说)。我们在这里所做的在金融预测和其他概率系统中得到了广泛的应用和研究，它们可能会非常成功，但我们应该总是对你实际知道的和你的模型可能学到的东西持批评态度。因为最终至少对我来说，我担心那些大肥尾巴中的一个会打我的脸。*