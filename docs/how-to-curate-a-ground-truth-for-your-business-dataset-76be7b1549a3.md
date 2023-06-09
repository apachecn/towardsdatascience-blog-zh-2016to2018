# 如何为您的业务数据集策划一个基本事实

> 原文：<https://towardsdatascience.com/how-to-curate-a-ground-truth-for-your-business-dataset-76be7b1549a3?source=collection_archive---------11----------------------->

如果你是一名实践数据科学家，总是给你一个精确标记的数据集来应用先进的机器学习算法，这篇文章不适合你。这是为我们公司世界中的其他人准备的，他们必须争取有用的数据集来解决我们团队面临的业务问题！

![](img/8e31ce19b171b4782d830780fd9e3b32.png)

[Iris Dataset](https://archive.ics.uci.edu/ml/datasets/iris)

很有可能你已经在[虹膜数据集](https://archive.ics.uci.edu/ml/datasets/iris)上运行了实验，用于演示基本的机器学习模型。对于教育目的来说，这是一个很好的数据集，但我经常看到有抱负的数据科学家在试图将这些技能应用于现实世界的问题时感到沮丧。为什么学术界的项目与现实世界中的项目如此不同？

你也可以将同样的故事情节扩展到“Kaggle Cowboys”，他们非常擅长在精心策划的问题陈述和数据集上优化机器学习算法，但在处理现实世界的商业问题时，往往很难捕捉到同样的吸引力。同样，在比赛中运用你的技能和在公司项目中运用你的技能有什么区别？

![](img/37bb97579513791592a230f37f96d16d.png)

[Adding labels to your dataset](https://commons.wikimedia.org/wiki/File:A_Charming_Cartoon_Businesswoman.svg)

我对这种差异的观察集中在**数据注释**的艺术/科学上，或者简单地给你的观察加上一个标签！业务问题通常会提交给数据科学团队，他们的意图是好的，但缺乏关键数据点，这使得训练任何有监督的机器学习算法都不可能。对于那些不在业务领域的人，回想一下 Iris 数据集。它类似于只包含原始属性的数据集，没有最终的类来识别正在测量的虹膜类型。或者与此相反，只捕获最终的类，而不捕获有助于做出最终分类决策的属性。

不幸的是，对于一些遗留的业务流程来说，制定决策的决策因素没有被数字化地捕获。解决这些差异的正确方法是与流程工程师和应用程序开发人员合作，告诉他们我们需要收集的数据点，以便在未来应用机器学习算法。然后提出变更请求，以在生产系统上实现这些特性，并可能在流程中重新培训工人。然后我们可以开始考虑在未来的某一天应用机器学习。然而，您的业务发展速度可能会要求您还需要找到一个针对所有历史数据的解决方案，并在本财政季度内展示一个机器学习概念验证！

当数据缺少关键信息时，我们如何弥合这一差距，以利用可用的历史数据来展示机器学习的潜力？

答案出人意料地简单，却很难掌握。利用[调查设计](https://www.questionpro.com/features/survey-design/)中的原则，用人类智慧简单地增加你的历史数据。注释原始数据集的学科通常不与机器学习算法一起教授，但应该这样。如果你看看发表在知名期刊和学术界的文章，就会发现有一整套关于他们如何从主题专家(SME)那里获取实验数据的实践。让我们花点时间来探索这一规律，以及如何将它引入我们的业务数据集。

![](img/2aeedcaf51b129ee512dcfb4a4a13a59.png)

[Google’s reCAPTCHA](https://www.google.com/recaptcha/)

谷歌巧妙地将数据注释实践掌握到一项名为 [reCAPTCHA](https://www.google.com/recaptcha/) 的“免费”服务中。reCAPTCHA 可用于任何网站，并为他们提供抵御[机器人](https://en.wikipedia.org/wiki/Internet_bot)的高级安全性，同时引导人类访问者的精神能量解决数字化文本或注释图像的低技能“验证码”。他们利用多人的智慧来构建这个带注释的数据集。这有助于解释任何单个人的偏见，并开发一个“真理”数据集，该数据集是关于被标记的观察结果的普遍接受的“真理”。当然，这里的问题是，任何利用 reCAPTCHA 的网站都在将数据注释任务反馈给 Google 的专有数据集！

整个流程是平滑无缝的，允许一大群人用标签来增加先前未标注的数据集。这为先进的机器学习算法打开了闸门，以解决有趣的商业问题！随着数十亿人访问数十亿个网站，其中一些参与了 reCAPTCHA，谷歌成为人工智能行业的领先公司有什么好奇怪的？企业应该如何利用这群互联网工作者来注释他们自己的专有数据集？我们能为我们的业务数据集复制这个工作流吗？

![](img/7487ea66cd84cabfd7c4b26673b26a53.png)

[Mechanical Turk](https://www.mturk.com/)

亚马逊也看到了这一潜力，但他们没有让一群匿名的互联网工作者注释自己的专有数据集，而是将其作为一个名为 [Mechanical Turk](https://www.mturk.com/) 的 API 服务公开。这项服务提供了一个惊人的平台，将任何拥有未注释数据集的人与一群愿意以最低报酬注释数据集的工作者联系起来。就数据注释而言，我认为 MTurk 是每个数据注释平台都应该追求的“黄金标准”,这主要是因为可用劳动力的规模可以转化为对数据集进行注释的速度。在实践中，当员工完成交给他们的任务时，你通常会给他们每分钟 0.12 到 0.25 美元的报酬(大约每小时 8 到 15 美元)。

就我们所处理的商业问题的规模而言，你可以在 MTurk 上以非常便宜的价格获得你的数据集注释！对于给定数据集中的数万个观测值，您通常可以期望 MTurk 上的一大群工作人员在几个小时内完成任务，而费用仅为几百美元！但是这里有一个大陷阱…

一般来说，任何互联网用户一般都有资格完成低技能的注释任务，如数字化文本或注释图像。然而，当手头的注记任务需要具有专业知识的高素质工人时，愿意为低工资的数据集进行注记的人数开始显著下降。对于这些类型的数据集，目前的策略是通过“面试”确定哪些员工可以参与，并提高每项任务的报酬。

在商业中，你不太可能有一个足够简单的数据集，只需要 MTurk 工人的低技能工作。除了数据集的复杂性之外，更大的挑战很可能是你的数据集是你的企业专有的，你的[信息安全](https://en.wikipedia.org/wiki/Information_security)团队将规定你不能将它暴露给互联网上的任何匿名工作者。那么这是否意味着众包你的数据标注任务是不合适的呢？

![](img/233e012ff2a5e126af01bb194225e669.png)

[Figure Eight](https://www.figure-eight.com/)

[Figure Figure Figure](https://www.figure-eight.com/)(原名 CrowdFlower)提供了许多与 MTurk 相同的功能，但增加了让工人在 [NDA](https://en.wikipedia.org/wiki/Non-disclosure_agreement) 下操作的承诺。当然，我们不断添加到工人人才库来注释我们的数据的请求越多，我们的人才库就越小，报酬就越高。

当您深入研究您的数据集以定义您要求 NDA 手下的这些工人执行的任务时，您可能会得出这样的结论:数据集的复杂性如此错综复杂，以至于需要非常具体的知识才能正确地进行注释。在某些情况下，这种知识可能在你的业务中根深蒂固，以至于指望公司以外的任何人都能正确回答是不合理的。这就引出了一个问题，为什么我们不能让我们内部的主题专家(SME)来为我们做注释工作呢？如果我们要求内部员工注释我们的数据，为什么我们需要第三方服务？

![](img/dc1e1c35906beb8ac9995b0a152f4217.png)

[PyBOSSA](https://pybossa.com/)

要求内部 SME 注释数据的挑战通常集中在一致性、规模、优先级、相关性和紧迫性上。通常，你需要帮助注释数据集的 SME 在他们的职业生涯中已经超越了对单调乏味工作的兴趣。这些挑战大多可以通过良好的领导、沟通和适当的激励来解决。然而，对于剩下的技术挑战，请查看 [PyBOSSA](https://pybossa.com/) 。PyBOSSA 是一个开源的众包应用程序，源自 [BOSSA](http://boinc.berkeley.edu/trac/wiki/BossaReference) ，构建于 Python、Redis 和 PostgreSQL 之上。

使用 PyBOSSA，我们可以公开 reCAPTCHA、MTurk 或 Figure Figure 8 的相同功能，以帮助注释我们的数据集。该解决方案的美妙之处在于，您拥有整个基础设施，并且可以对其进行定制，以满足您的**确切的**业务需求。这意味着您可以在内部部署它，在特定身份认证要求的保护下满足您的信息安全和法律团队的要求。以及将每个活动的现成 RESTful API 路由到 SME 的现有工作流中，以*最小化注释数据的摩擦*(类似于 reCAPTCHA)。它还包括基本的活动指标和基本的 web 界面，供您起草任何 HTML DOM 和 JavaScript 来处理任何类型的注释任务。

不管你选择哪个众包平台来注释你的数据集，仍然有一个关于注释质量的问题，以及我们能在多大程度上信任这些工作者？？

为了回答这些问题，在以后的文章中，我们将介绍一些业务数据集的 [**数据注释最佳实践**](https://medium.com/@szelenka/data-annotation-best-practices-for-business-datasets-4a818e2c9643) 。