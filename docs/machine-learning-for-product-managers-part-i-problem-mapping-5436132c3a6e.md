# 面向产品经理的机器学习第一部分——问题映射

> 原文：<https://towardsdatascience.com/machine-learning-for-product-managers-part-i-problem-mapping-5436132c3a6e?source=collection_archive---------3----------------------->

机器学习和人工智能是最近的热门话题。因此，我发现许多产品经理和潜在的产品经理找到我，问我他们如何才能成为更好的 ML 项目经理。由于机器学习和产品管理的交叉是一个相当全面的话题，一篇文章无法公正地描述它。因此，我打算把它分成三部分

**第一部分——问题映射:什么类型的问题最适合机器学习**

[**第二部分——ML 技能:在构建利用机器学习的产品时，项目经理还需要哪些额外的技能组合**](https://medium.com/@productmanagement/machine-learning-for-product-managers-part-ii-ml-skills-ce7c3cee3246)

[**第三部分——警告:在构建使用机器学习**](/machine-learning-for-product-managers-part-iii-caveats-79803a7548ef) 的产品时，有哪些常见的错误

这篇文章将重点讨论如何确定你试图解决的问题是否需要机器学习。

# 关注用户

使用机器学习构建伟大产品的首要原则是关注用户需求。我发现的一个常见误解是，人们认为机器学习在某种程度上从根本上改变了项目经理的技能。机器学习本身并不是目的。机器学习是解决真实用户需求的工具。我发现许多人(和公司)有一项很酷的人工智能技术，并且认为这项技术本身就证明了它的用途。如果你有一项很酷的技术可以应用，想想通过这项技术可以解决什么问题，或者增强什么体验。

话虽如此，有很多初创公司利用机器学习提出了很酷的创新，但并不总是专注于解决用户的问题/需求。它们在生态系统中起着非常重要的作用——它们推动技术前沿。这些初创公司往往会被收购，很少能够自己找到适合市场的产品。然而，作为一个项目经理，如果你试图建立一个服务于用户的产品，你的用户所面临的问题，而不是技术，需要成为你的关注点。

这给我们带来了挑战— *如何判断 ML 可以帮助解决哪些用户/业务问题*。就其核心而言，ML 最适合于需要某种**模式识别**的问题。通常这些问题可以分为几种不同的类型

![](img/6c368fe607b30b0ef7c750365d290f83.png)

# 用户被过多的数据淹没

如果用户必须筛选大量数据来完成任务，ML/AI 是一个很好的工具。谷歌和必应等搜索技术使用各种机器学习算法为用户呈现最佳结果。例如，当你搜索菜谱时，哪个菜谱会排在你的第一个搜索结果的前面，这是基于对你自己的搜索模式以及与你相似的人的搜索和点击行为模式的了解。分类问题也能够利用机器学习。如果你想对数百万个关于教育的帖子进行聚类，机器学习可以帮助对它们进行分组，假设你已经通过提供已知关于教育的类似文档的合理样本来训练模型。

# 需要复杂认知能力的问题

自动驾驶汽车需要能够理解周围的环境。自动分类照片的图库应用程序需要能够检测地点、人和事物。这需要复杂的认知技能，建造这种智能机器的唯一方法是向它提供大量数据，并通过模式识别进行学习。

# 预测和预报

最常见的问题之一是预测用户是否喜欢某个商品。用户会喜欢新闻订阅中的故事吗？购买了 Dropbox 订阅的用户会流失吗？如果您的问题需要您预测用户是否会执行某个动作，ML 是一个很好的选择。

同样，如果你想预测 2018 年 12 月的销售情况(并且你的业务基本面没有发生巨大变化)，你可以输入过去所有年份的历史数据，ML 模型可以在考虑季节性的同时进行预测。销售预测、库存使用——所有这些都可以从 ML 中受益。

# 异常检测

ML 的一大好处是发现数据中的异常。由于 ML 擅长模式识别，任何不符合被认为是规范的行为模式的东西都可以很容易地被检测出来。欺诈检测是一个主要的应用。如果我的用卡模式不同于我的常规行为，这可能表明我的卡已经被破坏。类似地，如果黑客试图访问企业内部网，查看入侵者的访问模式，人们可以使用机器学习轻松标记这种行为。

# 通过建议帮助决策

如果你想帮助你的用户做决定，比如说，提供购买、参观、观看等方面的建议。机器学习是一个很好的工具。通过观察人们观看类似事物、购买类似产品、访问类似网站的模式，机器学习可以减轻做出决定的认知负担。通过浏览建议，可能没有浏览整个视频目录或搜索所有项目的用户能够找到与他们的兴趣相似的项目。

# 与人类互动的体验

如果你正在寻找建立一个与人类交流的服务，因此需要理解自然语言，ML 是需要的。所有正在开发的助手技术——Alexa、Siri、谷歌助手——都是通过能够将人类语言翻译成可操作的任务来工作的。教机器理解人类语言需要向它们提供大量关于语言、语音语调的数据，然后让机器开始将语音映射到语言、指令和任务。

# 增加/创造新的体验

增加/创造新的体验是 ML 最令人兴奋的领域。有没有一种体验，因为上面提到的所有能力，现在成为可能？SnapChat 过滤器是如何使用 ML 增强体验的一个很好的例子。通过使用面部识别算法，SnapChat 滤镜能够检测面部轮廓，从而将创意滤镜覆盖在面部，从而使照片分享更加有趣。你能想到更多可以通过 ML/AI 改进的体验吗？