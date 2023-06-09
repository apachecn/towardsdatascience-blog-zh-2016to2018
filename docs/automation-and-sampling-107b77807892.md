# 自动化和取样

> 原文：<https://towardsdatascience.com/automation-and-sampling-107b77807892?source=collection_archive---------13----------------------->

![](img/7fa178eb74844097a2a211d14b8821ec.png)

正如我前面提到的，我已经从爱立信过渡到了 Shopify。作为这种转变的一部分，我开始体验公共交通(以前工作离家需要 15 分钟的车程，现在，在市中心工作，我必须乘火车上下班)。今天早上，我旁边有一个女人正在用她的电脑工作，显然是在编辑一份文件，或者更可能是在里面写评论。在编辑的几分钟内，她打了一个电话，通过电话要求某人修改文档中的一些措辞，并痛苦地口述这些修改(几个词)。这个游戏大概进行了几次；在文档中编写注释，然后呼叫某人进行适当的编辑。本来可以是简单的编辑，然后通过电子邮件发送编辑过的文档变成了明显痛苦的听写练习。重点不在于找出她为什么不通过电子邮件发送文档，这可能很简单，因为没有数据计划，不愿意等待 wifi 连接，谁知道呢，但非自动化“过程”的使用使通常很简单的事情(在文档中编辑几个句子)变成了痛苦的听写体验。这也有带宽有限的后果，因此只有少数意见可以在该文件上进行修改。

这让我想起了前段时间和一个朋友的对话。他提到了他感到自豪的是，在他的组织中为两个数据源的提取、转换和存储建立了数据管道。一些数据是由他公司的系统生成的。靠近数据源，他有一个服务器，用于收集和减少/转换数据，并将结果作为文本文件存储在本地文件系统中。他每天都查看该服务器上的提取过程，以确保它仍在运行，每隔几天，他就会将新的文本文件从该服务器下载到一个服务器群和一个数据库中，他通常使用该数据库来对数据进行分析。如你所见，这也是一个痛苦的、非自动化的过程。因此，数据量很可能比自动化过程更有限，因为我的朋友需要手动满足这些管道的需求。

在 Shopify，我很高兴能够对我想分析的数据进行自动 ETL(提取、转换和加载)处理。如果您想了解一下 Shopify 在 ETL 方面提供了什么，我邀请您观看来自 Franç oise Provencher 的[Shopify 的数据科学](https://www.youtube.com/watch?v=I_ZWWkxIRy8)视频演示，她略微谈到了这一点以及 Shopify 数据科学家工作的其他方面。简而言之，我们使用 [pyspark](https://spark.apache.org/docs/latest/api/python/index.html) 和我们的数据工程师开发的定制库，从我们的来源提取、转换和加载数据到前厅数据库，公司中的任何人都可以使用该数据库获得关于我们业务的信息和见解。如果您通过 Franç oise video 收听，您将会理解自动化 ETL 方案的好处之一是我们将原始数据(大部分不可用)转换为存储在前厅数据库中的信息。然后，这些信息可用于进一步处理，以提取对公司有价值的见解。你会立即看到好处。一旦这样一个管道建立起来，它就会自动地执行它的工作，并且由于我们的数据工程团队，它会一直自我监控。显然，如果出现问题，必须有人采取行动并纠正这种情况，否则，您可以忘记管道，它总是更新的数据可供所有人使用。无需一个人花费大量时间监控和手动导入数据。一个必然的结果是，新信息的带宽相当高，我们可以获得大量的信息来进行分析。

让这么多信息唾手可得带来了新的挑战，而这些挑战是那些拥有手动管道的人从未遇到过的。对整个群体进行分析变得越来越困难和低效。你需要从样本的角度开始思考。取样时要记住几个因素:样本大小和你要取样的东西。

有数学和分析的方法来确定你的样本量，但是一个快速的方法是从一个适度的随机样本开始，进行你的分析，查看你的结果并保存它们。然后，你重复这个循环几次，看看你是否一直得到相同的结果。如果你的结果变化很大，你可能没有足够大的样本。否则你是好的。如果尽可能有效地提高未来的可重复性很重要，您可以尝试减少您的样本量，直到您的结果开始变化(此时您应该恢复到以前的样本量)，但如果不是这样，足够好就是足够好！只要记住那些样本必须是随机的！如果你一次又一次地使用相同的样本重复你的分析，你没有证明任何事情。在 SQL 术语中，它是以下两者之间的区别:

```
SELECT * FROM table 
TABLESAMPLE BERNOULLI(10)
```

另一方面，这将产生 10%的*表*的随机样本:

```
SELECT * FROM table 
LIMIT 1000
```

将很可能总是产生相同的前 1000 个元素…这不是一个随机样本！

你应该记住的另一个考虑是关于你应该取样什么，你想要观察的人群是什么。比方说，用 Shopify 的行话来说，我有一个所有商家客户的表，其中包含一个 orders 表的外键。如果我想了解客户执行了多少订单，那么被观察的人群是来自客户，而不是订单。换句话说，在这种情况下，我应该随机抽取我的客户，然后查看他们有多少订单。我不应该对订单进行抽样，以汇总每个客户的订单，并希望这将产生预期的结果。

从视觉上，我们可以看到，从订单中抽样会导致我们错误地认为每个客户平均执行两个订单。随机重采样将导致相同的错误结果。

而从客户那里取样，将得出每个客户平均执行四个订单的正确答案。

总结一下，假设您有手动(甚至半手动)ETL 管道，那么您需要将它们自动化，以提供一致性和吞吐量。一旦做到这一点，你最终会发现采样的乐趣(和需要)。取样时，您必须确保选择了合适的样本群体，并且您的样本是随机选取的。最后，你总是可以通过分析找到合适的样本量，但是通过几次试验，如果你的发现在大量随机样本中保持一致，你很可能就没事了。

*封面照片由* [*Stefan Schweihofer 在 Pixabay*](https://pixabay.com/en/users/stux-12364/) *。*

*原载于 2018 年 9 月 18 日*[*【thelonenutblog.wordpress.com】*](https://thelonenutblog.wordpress.com/2018/09/18/automation-and-sampling/)*。*