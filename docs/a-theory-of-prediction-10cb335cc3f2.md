# 预测理论

> 原文：<https://towardsdatascience.com/a-theory-of-prediction-10cb335cc3f2?source=collection_archive---------3----------------------->

![](img/80b0ceda7f3123cd7f8b3706cd742878.png)

**回顾** [***信号与噪音***](http://www.nytimes.com/2012/10/24/books/nate-silvers-signal-and-the-noise-examines-predictions.html) **作者:张瀚银**

*《信号与噪音》*可能是有史以来关于预测艺术的信息量最大的非技术性书籍。它概述了什么是最好的描述为内特西尔弗的“预测理论”。数据新闻网站 [fivethirtyeight](http://fivethirtyeight.com/) 的创建者 Silver 带领读者经历了一次不同领域的信息之旅，包括气象学、棒球、扑克、金融和政治，记录了要么严重失败，要么非常成功的估计，以及做出这些估计的预测者所采用的策略。成功和失败的预测都有很多东西可以教给我们，西尔弗从这些例子中提炼出了一个有凝聚力的信息:我们天生就不擅长预测，但通过采纳一些原则，我们可以改善我们的估计，并在个人和国家层面上受益。在我看来，这些原则属于三个规则:

1.  像狐狸一样思考
2.  像贝叶斯一样思考
3.  像(篮球)射手一样思考

很难从它们的名称中了解这些规则，所以让我们依次浏览每一个规则。

![](img/edaeb9a11402a6e8cdd7322acc85fe01.png)

# 像狐狸一样思考

在这本书的开头，西尔弗讲述了哲学家伊塞亚·柏林提出的刺猬和狐狸的对立观点。刺猬有一个大主意，而狐狸有许多小主意。每个人都倾向于站在一边或另一边:我们要么相信一个界限分明的黑白世界，要么相信一个没有任何事情是完全确定的灰色世界。每个观点都有自己的优点，但是当谈到预测时，[狐狸会明显更成功](http://longnow.org/seminars/02007/jan/26/why-foxes-are-better-forecasters-than-hedgehogs/)。一只狐狸可以利用各种不同的来源，并假设不同的观点，但刺猬却被锁定在一条思路上。

这最好通过一个简单的例子与预测联系起来。假设我们在一个只有两个政党的国家(多么可笑的概念！)，熊和老虎。我们正在与一位预测对手竞争，他试图在对即将到来的选举的预测上胜过他。无论谁赢了，都将在接下来的四年里出现在主要新闻频道。我们的对手是一只经典的刺猬；他的一个大想法是猛虎组织将在下一次选举中横扫所有对手。另一方面，我们做出了更像狐狸的理性决定。我们相信总体趋势确实有利于老虎，但我们愿意看看故事线可能不直截了当的个别比赛。当到了做出预测的时候，我们依次检查每场比赛，查看所有相关新闻，并对多次投票进行平均，以选出获胜者。我们的对手自信地为每场比赛挑选老虎，支持他的选择，断言他的世界观是不会错的。最终，我们轻松获胜，并有幸在接下来的四年里每天晚上出现在新闻中，因为我们愿意看到细微的差别，而我们的对手却被一个想法蒙蔽了双眼。

这可能是一个人为的例子，但它表明了采用简单的对/错世界观的危险。当你是一只刺猬，只有一个信念，比如老虎队会赢得所有的选举，你听到的每一条新信息都会证实你的信念。你会很高兴地把你所有的信心都放在支持老虎的论文上，而忽略任何相互矛盾的证据。同时，作为狐狸，我们没有偏见，愿意客观地评估每一个数据来源。刺猬们遭受着[确认偏差](https://www.sciencedaily.com/terms/confirmation_bias.htm)的困扰，在那里他们操纵任何证据来符合他们的观点。狐狸能做出更准确的预测，因为它们愿意以开放的心态处理任何问题。在现实中，很难摆脱我们所有的先入为主的信念，但我们可以通过收集尽可能多的信息来源来对抗它们，就像 fivethirtyeight 在构建[选举预测](https://fivethirtyeight.com/features/how-the-fivethirtyeight-senate-forecast-model-works/)时所做的那样。

不幸的是，正如西尔弗指出的那样，在现实世界中，刺猬经常会被人用扩音器广播它们的信息。随着电视新闻和互联网分裂成两个政治派别，中间是一片巨大的无人区，仍然处于中间的人们被极端的狂热分子所淹没。当人们收听一个新闻频道时，他们并不是在寻找客观的报道，他们想听到自己的观点是正确的。当谈到预测时，他们宁愿听刺猬告诉他们，他们的“团队”将横扫立法部门，而不是听狐狸根据客观数据详细描述每场比赛的明智预测。因此，出现在电视和新闻中的大多数预测者是做出大胆预测的刺猬，而不是做出合理但不那么令人兴奋的预测的狐狸。[尽管有证据表明](https://www.amazon.com/Superforecasting-Science-Prediction-Philip-Tetlock/dp/0804136718)那些做出更极端[预测的人是最糟糕的预测者](http://www.businessinsider.com/the-signal-and-the-noise-nate-silver-2012-9)，他们却占据了大部分屏幕时间。尽管如此，这并不妨碍我们在日常生活中成为狐狸。为了像狐狸一样思考，我们需要抛弃我们先入为主的信念，从不同的来源收集信息，倾听争论的双方，并做出反映世界灰色本质的合理预测。

# 像贝叶斯一样思考

在你被“贝叶斯”的奇怪声音吓走之前，让我来解释一下:[托马斯·贝叶斯](https://en.wikipedia.org/wiki/Thomas_Bayes)是 18 世纪的一位统计学家和部长，他以制定基于新证据更新我们对世界的信念的方法而闻名。当我们处理任何新的预测问题时，我们首先需要形成对情况的初始估计，称为[先验。](https://en.wikipedia.org/wiki/Prior_probability)假设我们想预测我们是否会在下一季度开始时获得晋升。已经过去一个月了，因为我们最近工作效率更高，而且与我们的经理关系也很好，所以我们可以把最初的几率定为 50%。贝叶斯定理(他的思想的正式版本)背后的思想是，当我们收集与问题相关的信息时，我们根据数据更新我们的第一个估计。如果我们的经理给我们发了一封赞扬我们工作的电子邮件，我们可能会把升职的可能性提高到 75%。随着事件发生的日期越来越近，如果我们把所有相关的信息都考虑进去，我们的预测应该会收敛到真实的概率上。证据的权重取决于它能在多大程度上降低我们的不确定性。每一步都要考虑先验信息，但是随着新信息(观察值)数量的增加，先验信息在预测中的权重会降低。我们的最终预测是初始估计和观测数据的[组合。](http://www.stat.yale.edu/Courses/1997-98/101/condprob.htm)

在我们的日常生活中，我们无时无刻不在使用贝叶斯推理。当观看体育赛事时，我们在一开始就有一个最终结果是什么的想法，随着比赛的进行，我们更新这个估计，直到最后，我们可以 100%确定结果。同样，股票市场会因新闻而上涨或下跌，因为投资者认为这些信息揭示了一家公司的价值。贝叶斯的观点与所谓的[频率主义者](https://en.wikipedia.org/wiki/Frequentist_inference)世界观形成对比，后者认为某件事情发生的几率仅仅基于过去数据中观察到的事件频率。当预测两周内 2018 年 1 月 21 日下雪的可能性时，这位常客会预测当天下雪的平均可能性，并坚持这一估计，直到出现问题的那一天。然而，作为一个贝叶斯，我们会做一个初步的预测，也许使用历史事件作为一个起点，然后根据新的信息随着时间的推移修改我们的估计。比平均温度更高的天气可能会降低我们预测的降雪概率，而一场从西面逼近的暴风雪会增加降雪概率。[贝叶斯在做出预测方面自然处于优势](https://io9.gizmodo.com/how-bayes-rule-can-make-you-a-better-thinker-471233405)，因为她会根据证据不断改变自己的信念。

贝叶斯思维的另一个重要方面是，预测被表达为一种概率，而不是一个是/否。虽然大多数人倾向于想要一个直接的答案，但我们生活在一个不确定的世界里，是或否是不完全可能的。任何预测都有不确定性，当有人预测股市明天会上涨时，他们掩盖了股市实际上有一个可能的取值范围。最有可能的情况可能是增加，但总有减少的可能。同样，用一系列数值或概率来回答预测问题可能不会让我们上新闻，但这将意味着我们平均来说更接近事实。负责任的数据科学家和预测者必须传达带有不确定性的结果。用一系列价值观来回答不应该被解释为缺乏信心，而是反映了世界的变化。

# 像(篮球)射手一样思考

做好任何事情的最佳方式是失败多次，然后做出调整。世界上最好的篮球射手大约有一半的时间会失败，但是他们不会因此而停止投篮。每次他们投篮不中，都会做些小调整，然后再投一次。虽然在某些领域反复失败可能很难(我们发明了飞机模拟器，让飞行员可以奢侈地失败不止一次，这很好)，但在风险较小的情况下，这很容易实现，我们可以收到[即时反馈](https://www.opm.gov/policy-data-oversight/performance-management/performance-management-cycle/monitoring/feedback-is-critical-to-improving-performance/)。预测，至少在个人层面上，完全符合这些条件。我们可以非常容易地(并且安全地)预测一周后我们的体重，我们最喜欢的球队下一场比赛的比分，或者烹饪爆米花的最佳时间长度，而不会遭受可怕的后果。如果每个人都被要求达到完美的预测标准，世界将会严重缺乏气象学家。相反，每当我们错过目标时，我们会检查失败的原因，并做出我们认为对下次有帮助的调整。

任何人都可能经常失败，但这是关于检查和利用错误来提高我们的表现。众所周知，西尔弗的团队未能预测 2016 年的选举，之后，他们[花了很长时间审视他们做错了什么。](http://fivethirtyeight.com/features/the-real-story-of-2016/) Fivethirtyeight 不进行民意调查，但汇总了各种各样的数据，并根据历史表现对每个数据进行加权。就 2016 年而言，很明显[民调系统性地偏向](http://www.pewresearch.org/fact-tank/2016/11/09/why-2016-election-polls-missed-their-mark/)希拉里·克林顿，西尔弗和他的团队无疑会在下一轮选举中考虑到这一点。Silver 还记录了一个棒球模型，该模型做出了绝对荒谬的预测，经检查，这是由一个错误的字母引起的。如果创建者在失败后抛弃了模型，他们的开发时间就会被浪费，但是他们有智慧不断做出新的预测和错误来隔离问题。像射手一样思考包括不断重复射击-失误-调整-射击循环，在每次迭代中提高性能。

# 在噪音中失去信号

正如西尔弗在整本书中指出的，任何预测模型都有局限性。人类有一种[倾向于看到不存在的模式](https://www.scientificamerican.com/article/patternicity-finding-meaningful-patterns/)，特别是在偶然关联的情况下。[超级碗指标](https://en.wikipedia.org/wiki/Super_Bowl_indicator)，根据哪个联盟赢得超级碗来预测股票市场的表现，在过去 50 年中有 40 年是正确的。然而，NFL 冠军得主并没有影响市场的宏观经济效应，这只是一种令人惊讶的相关性。此外，一些系统，比如天气，对初始条件极其敏感(这些属于[混沌理论](https://en.wikipedia.org/wiki/Chaos_theory)的有趣领域)。天气模拟开始时的一个微小变化会导致截然不同的预测，这就是为什么预测天气和气候是众所周知的困难。尽管如此，由于过去的多次失败，天气预报在过去的几十年里已经有了显著的进步。天气服务创造了数千亿美元的价值，因为风暴预警减少了风暴造成的损失，气候预测带来了更好的收成。虽然我们通常倾向于简单的模型，但有时我们需要一个庞大复杂的模型来进行准确的预测。

另一种预测失败的问题是样本外的情况。这些情况在我们的数据中从未出现过，因此几乎不可能预见到。考虑一个飞行员，他在晴朗的天气里从休斯顿到纽约飞行了 800 次，没有发生任何事故。在他的下一次航班上，一场巨大的飓风正在影响东海岸，航空公司必须决定是否取消他的航班。飞行员争辩说他以前从未坠毁过，因此在这次飞行中他不可能遇到麻烦。然而，这是一个样本外的情况，因为他以前的每次飞行都是在完美的条件下进行的。在这种情况下，谨慎的措施是停飞，因为不确定性太大了。日本对珍珠港的袭击通常被认为是一个样本外的事件，因为外国势力的大规模袭击从未发生在美国本土。尽管有迹象表明日本正在准备一场大规模的军事行动，但美国未能预测到这一事件。信号是有的，但被忽略了，因为像珍珠港这样的袭击以前从未发生过。

在这个盲目相信大数据和复杂模型的时代，西尔弗对只依赖统计数据的预测持必要的批判态度。根据他过去在棒球模型方面的经验，Silver 解释了为什么计算机本身常常不能捕捉到许多人类活动中的所有错综复杂之处。他发现，他自己的基于数字的模型表现不如那些利用人类直觉和数据建立的模型。同样，在计算机第一次击败最好的国际象棋选手后，人们预测人类在游戏中已经失去了所有的相关性。然而，随后的公开世界锦标赛允许任何人和计算机的组合进行比赛，由使用程序和了解游戏知识的人的团队赢得了[。在大多数领域，领域知识加上计算机模型很可能会超越单独依靠其中任何一个的预测。](http://smarterthanyouthink.net/excerpt/)

# 建议

我判断一本书的方法是，我是否能在五分钟的总结中获得所有相关信息，或者是否值得为了额外的见解而阅读整本书。在*信号和噪音*的情况下，我推荐通读整部作品。在这篇文章中，我概述了这本书的基本内容，但跳过了 Silver 用来阐述他的每一个观点的几乎所有真实世界的例子。这本书以直观的风格涵盖了许多统计概念，对于像预测这样看似学术的主题，可读性很强。任何渴望成为数据科学家或想怀疑地检验预测的人都应该阅读这份关于准确预测方法的资料性著作。