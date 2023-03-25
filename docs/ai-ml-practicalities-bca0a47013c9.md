# 宣布 AI/ML 的实用性

> 原文：<https://towardsdatascience.com/ai-ml-practicalities-bca0a47013c9?source=collection_archive---------23----------------------->

## [AI/ML 实用性](http://towardsdatascience.com/tagged/ai-ml-practicalities)

## 机器学习利益相关者的有用见解

![](img/f8e29feff37d2d208411e91fe5e51b97.png)

Photo by [Fleur Treurniet](https://unsplash.com/photos/dQf7RZhMOJU?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) on [Unsplash](https://unsplash.com/search/photos/carpentry?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

# 本系列文章:

[数据的不合理有效性](/ai-ml-practicalities-the-unreasonable-effectiveness-of-data-c0bfd44c5057)

[实验周期](/ai-ml-practicalities-the-cycle-of-experimentation-fd46fc1f3835)

[数据越多并不总是越好](/ai-ml-practicalities-more-data-isnt-always-better-ae1dac9ad28f)

[如果“多维”伤脑](/if-multi-dimensional-hurts-your-brain-c137c9c572d6)

有充分的理由，许多商业人士对机器学习(ML)既敬畏又怀疑。ML 已经取得了非常明显的成功:无人驾驶汽车，手写识别使我们几乎所有的邮件都能自动发送，机器翻译接近人类专家的水平并超过人类的速度。与此同时，无数的新闻报道承诺革命、癌症的终结、每项工作的自动化以及人类的替代，这让我们不禁好奇天网何时上线。

在我称为“AI/ML 实用性”的这一系列文章中，我将试图向非从业者揭示机器学习中涉及的一些现实和挑战，以及数据科学家用来解决这些问题的技术。我希望这些微小的见解能让非技术人员，或者至少是非 ML 技术人员，更好地评估声明和产品，消费 ML 服务，并与 ML 技术人员合作。

机器学习和数据科学生活在统计学、编程和商业的交叉点上。许多“新”概念，如特征工程和维数灾难，实际上继承了这些学科中的一个，但对于来自其他学科的人来说是新的材料。一些概念，例如集成方法，是机器学习所特有的。(特征工程、维数灾难和集成方法都是本系列将涉及的主题。)

作为一名程序员来到这个领域，我最大的惊讶与 ML 的经验主义性质有关。编程是一种确定性的活动。一旦你知道你想让计算机如何运行，实现该行为的“正确”路径就很少了，大多数变化都是由于最佳实践的应用。ML 反映了科学研究。你有一个半确定的目标。你的目标和实现目标的途径都是假设，必须经过测试、提炼和再测试。本系列中的一些文章将专门讨论这个过程。

对于经理、客户或机器学习项目的其他发起人来说，ML 的潜力似乎太好了，不容忽视。但是，让合作伙伴开发模型、信任这些模型以及管理它们的生命周期的风险是令人生畏的。

希望这个系列能让 ML 变得更加透明。