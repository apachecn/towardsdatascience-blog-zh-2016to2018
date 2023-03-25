# 你真的需要一个数据科学家吗？

> 原文：<https://towardsdatascience.com/do-you-really-need-a-data-scientist-fcdfc226f4e4?source=collection_archive---------8----------------------->

## 仅仅在您的数据库中“插入”一个数据科学家不会带来预期的结果。首先，你需要确保你的数据确实有价值。

![](img/3a82bae264d1ef2d9cc3c9bdd5024bff.png)

您的公司拥有数据，而且很可能是大量数据。数百万行，甚至可能是图像、音频和视频。但是还没有什么可以称之为大数据的东西。随着时间的推移，数据是通过许多系统收集的，无论是您的系统还是第三方系统，如 ERP、CRM 和其他应用程序。你把它存储在某个地方:你的关系数据库、电子表格、NoSQL 数据库或其他任何地方。它甚至可能存储在第三方数据库中(在您使用的一些软件中)。事实是:数据属于你，它被存储起来，你可以访问它。因为数据科学现在是一个热门话题，所以出现了一个常见的错觉:

*如果有数据存储在某个地方，我们需要做的就是雇佣一个数据科学家。他/她一定会从数据中提取一些东西，然后转化为对我们有价值的东西。*

好吧，如果你公司的现状和上面的描述有某种联系…我很抱歉。你可能真的不需要数据科学家。至少，现在还没有。让我们来探讨一个假设的场景，您雇佣了一名数据科学家。

# 假设场景:雇佣一名数据科学家

![](img/7dc54b66e9ecab0fc7e8c5f4f66b1b66.png)

我在就业市场上看到的大多数数据科学家实际上只是在[mooc](http://mooc.org/)中学过 Python、R、pandas 和 scikit-learn 的分析师。他们参加过一些 [Kaggle](https://kaggle.com/) 的比赛，没有什么职业经验。而且他们渴望在“现实世界”中展示自己的知识。

> 这是 21 世纪最性感的工作。 再也没有人想创建仪表盘和报表了，每个人都想和人工智能打交道。

如果你发布一份寻找数据科学家的工作，大量的候选人将会出现，你可能会面临以下情况:

1.  你的面试，测试，筛选，会成功选出一个具备一些机器学习工程素质的数据科学家。他/她将具有良好的编程技能和几种算法及其应用的理论知识。
2.  你将向他/她展示你的“存储在某处的数据”。然后给他/她一个开放式问题来处理。比如:我们希望降低违约风险，或者我们需要增加销售额。
3.  他会尝试使用数据，应用模型和算法来回答你提出的问题。很可能会失败。
4.  然后，你将开始给他一些应该交给商业智能分析师的任务，比如设计一个跟踪日常销售的仪表板，或者做一些自动化的后勤工作。
5.  [过一会儿，你的数据科学家会感到沮丧](/why-so-many-data-scientists-are-leaving-their-jobs-a1f0329d7ea4)。他终究不能把在课程和比赛中学到的东西应用到实践中去。他将开始考虑其他公司的工作岗位，因为他们显然在做真正的数据科学——篱笆那边的草总是更绿。
6.  您的企业不会从雇用数据科学家中获得预期的好处。最多，你会有一个没有动力的 BI 分析师，和一个数据科学家挣得一样多。也许你将不得不寻找一个替代的专业人士。

注意，问题不在数据科学家。他有必要的知识，并努力做好自己的工作。实际问题在于你的数据…也在于缺乏一个真正的科学家。

大多数数据科学家的最大问题是，他们实际上并不是科学家。对快速应用模型和算法的焦虑和渴望最终超过了优秀数据科学工作的重要阶段，如情境化、问题框架、实验设计和数据收集。

# 你的数据(很可能)是垃圾！

交易数据库(存储订单、支付、访问日志等数据的数据库)是专门为存储交易数据而开发的，交易数据支撑着应用程序。这些原始数据对数据科学来说没什么价值。构建这些数据库的开发人员没有考虑，也许也不应该考虑如何将这些数据用于分析。他们只是创建了数据模型，可以提高他们当时正在编写的任何应用程序的性能。

这些事务数据库中的数据可能格式不正确，没有文档，列名没有代表性，没有键一致性，有大量重复或缺失的行，不同来源之间的值不一致，等等。也有可能，无论是大公司还是小公司，有些流程完全依赖于电子表格。因此，您的数据库不会有变更日志或历史数据。流程和控制的缺失，会让每个事件的真相真的很难分辨。

很可能您已经在这些事务数据库上运行了一个商业智能结构。您只需将一些工具直接插入数据库或电子表格，并让一些数据分析师创建报告和仪表板。这非常有效，并为公司带来了价值。你看见了你所做的一切，一切都很好。

但是，当你开始洞察数据如何帮助你的公司增加市场份额、提高效率等等的时候，就到了。你在那里看到了所有这些数据…可用…只是用来创建一些报告和解释已经发生的事情…就等着传递更多的价值。

如果预测未来是可能的呢？优化商业政策呢？还是留住一个刚要离开的客户？你有数据——很多数据。他们在那里…焦躁不安，尖叫，催促被使用。你感觉像伊甸园里的夏娃。蛇问:“你不吃花园里任何一棵树上的果子吗？”。是的，你受到诱惑，雇佣了我在前面的假设情况中提到的那个数据科学家。

![](img/592021650006db8b845c3f40f83b33e8.png)

事情是这样的:将数据存储在事务性数据库中这一简单事实并不意味着你有金矿可挖。它们需要仔细加工并转化为分析基础——这个过程需要很多时间。创建分析数据库需要特定的环境，也需要了解业务特性，以便将单纯的交易转化为有意义的事情。

尽管如此，仍然存在事务性数据实际上是垃圾的情况，因为收集没有按预期进行。我遇到过这样的情况，系统没有存储所有的交易信息，或者存储了转换后的数据，使得无法返回到它们的原始组件。两年多的数据收集可能有助于开发一个奇妙的预测模型，但却增加了公司的存储成本。

让我们把事情说清楚:你有一个数据问题。你最终雇佣的数据科学家将拥有创造价值的所有工具和方法。但是，没有好的数据，他们做的任何事情都是无用的。这不是他们的错，是你的错。在这种情况下，你和数据科学家在一起的每一天，都是看到你的钱从前门离开的一天。

# 雇佣一个科学家。从事数据工作的人。

科学家必须处理实验的每一个步骤，从他们的构想到发表结果。他们通常是拥有物理、化学、数学、统计学或生物学学位的专业人士。他们可能不知道关于机器学习的一切，但是，在阅读的这一点上，我想我已经表达了我的观点:你不一定需要能够实现 scikit-learn 方法的人。

科研流程通常如下:

![](img/8f165efbed949c4da2e0d573c516c4bf.png)

雇用知道如何这样思考的人。在面试时，不要测试候选人是否知道所有的工具和技术。相反，测试他/她是否能遵循上面的思路。寻找表明他们能够遵循科学方法中每一步的情况和证据。具备这些技能的候选人可能会在处理你的业务数据时表现出色。

科学家检验想法和假设。他失败了，学习了，最终想出了一个解决方案。如果你真的需要快速完成工作，那就雇佣一个有经验的科学家。确切了解您的数据情况，然后测试申请人的经验，验证他/她是否已经面临类似的情况。询问他采取了哪种行动，从中学到了什么。一个有经验的科学家能够更快地解决类似的问题。

学习如何使用新工具相对较快。另一方面，学习如何思考是一个漫长而缓慢的过程。通常需要四年或更长时间的自然科学学士学位，有时还需要硕士或博士学位。雇佣一个知道如何思考并具备一些数据科学工具基础知识的人，要比雇佣一个知道很多不同工具但不考虑在哪里以及如何应用它们的人好得多。这并不意味着你不应该雇用一个不知道如何编程的科学家，而是说在选择候选人时，编程技能不应该是最相关的。

你还必须重新思考你所有的流程和数据流。科学家需要了解业务环境，并了解如何收集您的数据。他/她应该对您的应用程序提出修改建议，以便在正确的时间获取正确的数据。他必须确保所有过程都是可重复的。这将要求您的开发人员改变服务，创建新的 API，并满足科学家的需求(他们必须知道如何用可靠的证据来支持他们)。您的技术团队需要给他一定的自主权，以便科学家能够创建新的数据模式、自动化流程并创建新的 ETL。

听着，这并不意味着忘记眼前的回报而只关注长远。你的科学家将致力于组织和提高数据质量。在此过程中，他/她将发现需要优化的流程，并检测一些模式。他将能够运用人类的智慧进行调整，这将在短期内产生回报。与此同时，这将使你离真正需要一个了解机器学习一切的数据科学家更近一步。