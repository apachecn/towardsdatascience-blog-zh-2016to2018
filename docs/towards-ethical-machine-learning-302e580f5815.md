# 走向伦理机器学习

> 原文：<https://towardsdatascience.com/towards-ethical-machine-learning-302e580f5815?source=collection_archive---------11----------------------->

## 数据采集员道德责任探析

我辞掉了工作，加入了一个密集的数据科学训练营。我理解大量可用数据背后的价值，这些数据使我们能够创建预测性的机器学习算法。除了在专业层面上认识到它的价值，作为消费者，我也从这些技术中受益。每当我发现自己陷入音乐的窠臼时，我都会依赖 Spotify 的 Discover Weekly。我经常惊讶于 Spotify 的算法和其他机器学习模型如何如此准确地预测我的行为。事实上，当我第一次坐下来写这篇文章时，我停下来看了一段 Youtube 视频。20 分钟后，我意识到 Youtube 的推荐算法有多好。虽然我非常清楚地看到了机器学习的好处，但认识到并减轻其潜在的危险也是必不可少的。

虽然 Spotify 或 Youtube 推荐我不喜欢的歌曲或视频不会产生任何毁灭性的后果，但并非所有的机器学习算法都是如此。我第一次意识到机器学习的潜在后果是在我看到 Marshal Project 和 FiveThirtyEight 之间的合作时，他们讨论了机器学习在刑事司法系统中的使用。我认为机器学习只会给支离破碎的刑事司法系统带来好处。毕竟，美国占世界总人口的 5 %,却有世界 25%的被监禁人口。除了不成比例的监禁人口，美国的司法系统是建立在种族主义的警察和起诉，飙升的累犯率，以及对非暴力毒品罪犯过于严厉的惩罚。机器学习的引入难道不会对一个深陷危机的系统有所帮助吗？

虽然乍一看，机器学习似乎除了帮助美国破碎的刑事司法系统之外什么也做不了，但在阅读了更多关于正在实施的一些努力之后，我意识到谨慎行事至关重要。确定一个人被捕后的命运是机器学习被引入刑事司法系统的一种方式，它体现了依赖机器学习做出可能改变生活的决定的风险。

传统上，在一个人被逮捕后不久，法官就会面临一个困难的决定，即他们是否必须在监狱中等待出庭日期，这可能是几个月或几年，或者他们是否可以在家里等待出庭日期，有或没有限制。为了做出这一决定，法官必须分析被告，并且必须预测他们是否认为此人有逃跑的风险——如果被释放回家，此人将不会出庭受审，或者有犯下其他罪行的风险。

事实上，一个人被赋予做出可能改变生活的决定的权力是不公平的，并且为有偏见的决定留下了空间。随着机器学习的进步，已经引入了各种技术，试图消除法官的偏见，并基于数据而不是个人的唯一判断做出决定。这些技术通常被称为风险评估工具，由非营利组织和营利公司开发。劳拉和约翰·阿诺德基金会(LJAF)开发的公共安全评估(PSA)就是这些风险评估工具之一。PSA 使用历史数据，根据与未能出庭的风险和再次犯罪的风险相关的分数，标记那些被视为高风险的人。似乎利用基于历史数据的算法可以减少决定个人命运的偏差，而不是将全部决定权交给一个法官，但许多人发现这些风险评估加剧了他们试图消除的歧视性做法。

这些算法为模型特征分配分数，例如年龄、性别、收入、药物使用和以前的定罪。尽管 PSA 在 29 个城市和州使用，以及许多其他风险评估工具在全国范围内使用，但这些模型中使用的变量和权重的综合列表仍然是专有的，通常被称为“黑盒”。范德比尔特法学院刑事司法项目主任克里斯托弗·斯洛博金(Christopher Slobogin)表示，“风险评估应该是不被允许的，除非双方都能看到进入其中的所有数据。这应当是一个公开的、全面的对抗式诉讼程序。”如果我们想确保这些算法努力减少偏差，理解它们所基于的数据是很重要的。

这些风险评估的透明度是无价的，因此公众以及依靠他们做出决定的法官可以充分理解导致他们提出建议的途径，包括这些建议所依据的原始数据。毕竟，正如 IBM Research 指出的，“人工智能系统的好坏取决于我们输入的数据。不良数据可能包含隐含的种族、性别或意识形态偏见。许多人工智能系统将继续使用坏数据进行训练，使这成为一个持续存在的问题。但我们相信偏见可以被驯服，能够解决偏见的人工智能系统将是最成功的。”

作为数据科学家，我们必须认识到我们的道德责任，用无偏见的数据训练机器学习模型。出发点之一是允许透明度和制衡制度，以确保模型所基于的数据是公平和公正的。关于风险评估工具，理解所使用的数据是至关重要的，特别是考虑到困扰美国刑事司法系统的历史问题，如种族主义和对非暴力毒品犯罪的过于严厉的处理。我们必须理解这些数据，以便脱离这些偏见，而不是让它们永久化。

减少机器学习算法偏差的一个步骤是通过纽约大学 AI Now 研究所提出的算法影响评估(AIAs)。AIA 从“黑盒”方法导致恶性循环的想法延伸而来，不断远离对这些算法的理解，并削弱解决可能由此产生的任何问题的能力。AI 现在建议使用 AIAs 来处理公共领域中机器学习的使用，创建一套标准要求。通过 AIAs，AI 现在旨在通过公开列出和解释所使用的算法系统向公众提供清晰度，同时允许公众对这些系统提出争议，开发审计和评估流程，并提高公共机构理解他们所使用的系统的内部能力。

类似于使用 AIAs 来促进机器学习的透明度，国防高级研究计划局(DARPA)建议将可解释的人工智能(XAI)作为解决方案的一部分。如下图所示，XAI 致力于创造更多用户可以理解和信任的可解释模型。

虽然在 DARPA 的网站上似乎还没有对 XAI 的任何清晰、简明的描述，但他们表示，XAI 原型一直在进行测试，目标是在去年 11 月完成第一阶段的系统评估。在网站上，他们还表示，“在项目结束时，最终交付的将是一个工具包库，由机器学习和人机界面软件模块组成，可用于开发未来可解释的人工智能系统”

AIAs 和 XAI 只是组织朝着更道德、更透明的机器学习模型努力的两个例子。随着机器学习继续以爆炸式的速度增长，肯定会有更多的想法被引入，以确保这种监管。不管这些想法背后的具体细节如何，重要的是保持一个围绕机器学习模型的透明和理解的系统，在此期间，在机器学习过程的所有阶段都要仔细检查数据，以确保公平的做法，不会使偏见永久化。

**来源:**

[](https://www.themarshallproject.org/2015/08/04/the-new-science-of-sentencing) [## 量刑的新科学

### 马修·康伦、鲁本·菲舍尔-鲍姆和安迪·罗斯贝克的互动图形

www.themarshallproject.org](https://www.themarshallproject.org/2015/08/04/the-new-science-of-sentencing) [](https://fivethirtyeight.com/features/prison-reform-risk-assessment/) [## 是否应该根据尚未犯下的罪行来判刑？

### 这个故事是与马歇尔计划合作制作的。长期以来，刑事判决都是基于…

fivethirtyeight.com](https://fivethirtyeight.com/features/prison-reform-risk-assessment/) [](https://www.washingtonpost.com/news/fact-checker/wp/2015/07/07/yes-u-s-locks-people-up-at-a-higher-rate-than-any-other-country/?utm_term=.9de388bf6ac6) [## 是的，美国的入狱率比其他任何国家都高

### 2015 年 7 月 7 日“一个严峻的事实是，美国的人口不到世界人口的 5 %,然而我们有…

www.washingtonpost.com](https://www.washingtonpost.com/news/fact-checker/wp/2015/07/07/yes-u-s-locks-people-up-at-a-higher-rate-than-any-other-country/?utm_term=.9de388bf6ac6) [](https://hbr.org/2016/12/a-guide-to-solving-social-problems-with-machine-learning) [## 用机器学习解决社会问题的指南

### 这是周日晚上。你是一个大城市的副市长。你坐下来看电影，向网飞寻求帮助…

hbr.org](https://hbr.org/2016/12/a-guide-to-solving-social-problems-with-machine-learning) [](https://harvardlawreview.org/2018/02/bail-reform-and-risk-assessment-the-cautionary-tale-of-federal-sentencing/) [## 保释改革和风险评估:联邦判决的警示故事

### 从新泽西州到德克萨斯州，再到加利福尼亚州，全国各地都在讨论、实施和诉讼保释改革…

harvardlawreview.org](https://harvardlawreview.org/2018/02/bail-reform-and-risk-assessment-the-cautionary-tale-of-federal-sentencing/)  [## 审前风险评估现已向所有感兴趣的司法管辖区开放；研究顾问委员会…

### 纽约-劳拉和约翰·阿诺德基金会(LJAF)正在扩大访问一套资源，将有助于…

www.arnoldfoundation.org](http://www.arnoldfoundation.org/laura-and-john-arnold-foundation-makes-pretrial-risk-assessment-available-to-all-jurisdictions-announces-expert-panel-to-serve-as-pretrial-research-advisory-board/) [](https://papers.ssrn.com/sol3/papers.cfm?abstract_id=3225350) [## 算法风险评估和青年的双刃剑。

### 在判刑时，年轻可以被认为是一个减轻情节，因为它与减少…

papers.ssrn.com](https://papers.ssrn.com/sol3/papers.cfm?abstract_id=3225350) [](https://www.research.ibm.com/5-in-5/ai-and-bias/) [## 人工智能和偏见- IBM 研究-美国

### 五年内，有偏见的人工智能系统和算法的数量将会增加。但是我们会相应地处理它们…

www.research.ibm.com](https://www.research.ibm.com/5-in-5/ai-and-bias/) [](https://medium.com/@AINowInstitute/algorithmic-impact-assessments-toward-accountable-automation-in-public-agencies-bd9856e6fdde) [## 算法影响评估:走向公共机构的可问责自动化

### 2018 年 4 月 9 日更新:我们发布了一份新报告，完整描述了我们对算法影响评估的提议…

medium.com](https://medium.com/@AINowInstitute/algorithmic-impact-assessments-toward-accountable-automation-in-public-agencies-bd9856e6fdde) [](https://ainowinstitute.org/) [## 爱现在研究所

### 纽约大学的人工智能研究所是一个跨学科的研究中心，致力于理解…

ainowinstitute.org](https://ainowinstitute.org/) [](https://www.darpa.mil/program/explainable-artificial-intelligence) [## 可解释的人工智能

### 图一。对可解释的人工智能在机器学习方面的巨大成功的需求导致了人工智能的洪流…

www.darpa.mil](https://www.darpa.mil/program/explainable-artificial-intelligence)