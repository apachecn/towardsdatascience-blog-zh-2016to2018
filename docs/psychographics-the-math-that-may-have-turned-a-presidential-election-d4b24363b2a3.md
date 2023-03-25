# 心理学——可能改变总统选举的数学

> 原文：<https://towardsdatascience.com/psychographics-the-math-that-may-have-turned-a-presidential-election-d4b24363b2a3?source=collection_archive---------10----------------------->

![](img/259e28feb01e998c8d3bac92edb2bbb2.png)

除非你一直躲在岩石下或社交媒体自由区，否则你一定听说过脸书/剑桥分析公司的丑闻。如果指控属实，那么剑桥分析公司所做的事情的合法性(或者说非法性)是毋庸置疑的；他们使用的数据是非法获得的(未经适当同意)。然而，这一事件也引起了人们对他们用来将数据转化为可操作见解的实际算法技术的强烈关注。心理剖析的神秘艺术和微妙科学。

“貌相”这个词会立刻让我们大多数人产生负面情绪。我们都听说过基于种族、性别或年龄的貌相。没有人喜欢因为我们无法控制的特征而被放在一个盒子里。但是，我们不能把貌相和刻板印象混为一谈。种族或性别貌相是刻板印象的例子。侧写是根据对个人的了解来预测一个人的选择和行动的行为。刻板印象是一种狭隘的、完全不科学的形象化形式；这是一种基于无根据的概括对个人形成看法的行为，这种概括主要源于偏见和无知。刻板印象和剖析之间的关系类似于智能设计和物种起源研究之间的关系。这两者都是很大程度上不可信的方法论，用来支持一个人未经证实的信念，它们给整个领域带来了坏名声。

任何科学方法的基础都是从证据中学习。至少一个世纪以来，心理学家一直试图通过分析模型来捕捉人类行为。然而，这些模型的成功和可靠性是相当有限的，尤其是与它们在物理科学中的同行相比。这主要是因为人类远比原子或分子复杂。任何有用的人类行为模型都必须能够将个人的可观察特征与他/她的选择联系起来。然而，可能的特征和选择的空间即使不是无限的，也是非常大的。我们现在知道，人们需要复杂的算法来确定哪些选择是可预测的，哪些特征是重要的预测因素。这些算法的复杂性的一个直接后果是，它们需要大量的数据和计算能力来工作。

人类行为定量理论所需的数据和计算能力直到大约十年或二十年前才出现。心理学家和市场商人一直试图在调查的基础上理解行为。然而，调查作为数据收集机制从根本上说是有限的。他们受到自我选择偏差(即，你只从那些选择回答调查的人那里获得数据)、调查设计偏差(即，你只问那些你根据你的偏差或模型认为重要的问题)和数据稀缺(在前互联网时代，你可以通过调查接触的人数有严重的限制)。因此，这种分析得出的结果根本不足以被称为定量科学。像往常一样，当科学还没有到来时，生活还在继续。一些人(主要是营销人员)试图使用人口统计学参数，如年龄、性别、种族、地点等。，混合了一些描述性统计数据和直觉的魔力酱，对人类行为做出过于简单的概括。这被称为人口统计细分。其他人只是用偏见取代统计数据，并诉诸刻板印象。

随着新世纪的到来，一切都变了。我们将计算转移到了云端，这使得越来越多的人能够训练和验证越来越复杂的算法。然后，社交媒体网络出现了。脸书、推特、Instagram 和它们的表亲是名副其实的人类行为实验室。社交媒体网络摆脱了传统调查所面临的问题。另一方面，他们的问题是数据爆炸。表面上看，脸书或推特上的大部分数据都是琐碎的，可能看起来像噪音。脸书上的赞真的能说出你性格中的有趣之处吗？它们能用来预测你未来的选择吗？在一系列的论文中，Michal Kosinski 和他的合作者发现答案是肯定的。在一篇里程碑式的论文中，他们令人信服地证明了脸书的“喜欢”可以用来辨别个人的性格特征。在接下来的一篇[论文](http://www.pnas.org/content/112/4/1036)中，他们展示了给定足够的数据，计算机算法在这方面比人类更好，包括亲密的家人和朋友。最后，在另一个[重要的后续研究](http://www.pnas.org/content/early/2017/11/07/1710966114.full)中，他们表明这些性格特征可以用来预测一个人可能做出的选择。

这些研究人员的主要贡献不是革命性的新算法。这也不是第一次证明一个领域中过去的行为可以用来预测同一领域中未来的行为；亚马逊和网飞等公司已经在各自的领域展示了这一点。科辛斯基及其合作者展示的是，由看似微不足道的互动(如脸书的“喜欢”)组成的大量数字足迹的总和可以用来预测未来的选择，从而影响完全不相关领域的未来行动。这是一个令人难以置信的强大的认识。

《剑桥分析》利用这些见解来影响选举。但是他们不是第一个使用心理测量学的人，也不会是最后一个。事实上，在科辛斯基的研究之前，心理测量学的潜力在很多方面都是显而易见的。在 T2，有很多公司和机构正在以完全合法的方式使用某种形式的心理图形。例如，经济合作与发展组织正在[进行一项研究](http://www.oecd.org/education/ceri/thestudyonsocialandemotionalskills.htm)，研究各种个人和环境因素对不同城市学生社交和情感技能的影响。目标是为政策制定者提供设计更好的政策工具的见解，以便在学生中促进这些社会情感技能。这项研究将使用[人格特质的五因素模型](https://en.wikipedia.org/wiki/Big_Five_personality_traits)；剑桥分析公司用来建模的模型。经济合作与发展组织的研究是一个明显的例子，表明心理学可以用于明确的好处。

自从认知诞生以来，侧写就一直存在。任何时候你和一个人互动，或者可能和一个有知觉的生物互动，你都会被侧写。大多数或者几乎所有这样的剖析都是基于不完整的信息和“有缺陷的”算法。人们经常因为肤色在机场被骚扰，或者因为姓氏被列入禁飞名单。在这种情况下，很难认为无偏见的数据驱动算法不会提供更好的替代方案。

很大一部分关于心理图形的歇斯底里源于缺乏关于心理图形是什么以及它能做什么的知识。我们每个人都愿意相信自己是独一无二的。在很多方面我们都是。但是，在许多其他方面，我们是可以预测的。心理学不是魔术，它是数学。它只能学习可以学习的东西，只能预测可以预测的东西。使一个人独一无二的东西将永远隐藏在心理分析之外。甚至当涉及到可以被认为是可预测的影响时，它也只能对大型群体做出预计在大部分时间内成立的统计预测。换句话说，给定足够多的数据和足够强大的算法，对于你个性的可预测方面，心理测量学可能在大多数时候都是正确的，但不是所有时候。

区分数据收集的合法性和我们处理数据的道德性是非常重要的。围绕剑桥分析丑闻的问题首先与数据是如何获得的有关。这些问题最容易回答。几乎任何理智的人都会同意，应该有关于收集个人数据的合理法律。更难回答的是，这些数据的合法用途是什么，即使这些数据是在尽可能广泛的同意下收集的。

概括地说，心理测量学可以以两种不同(但相关)的方式使用。第一个是理解并提供大规模的人类行为的见解。经合组织的研究就是这种用法的一个例子。第二是微观目标和说服。这种用法在营销中[最有效。它在选举政治中的效力](http://www.pnas.org/content/early/2017/11/07/1710966114.full)[充其量可能是微不足道的](https://www.vox.com/science-and-health/2018/3/23/17152564/cambridge-analytica-psychographic-microtargeting-what)。在这两种情况下，人们可以认为心理测量学中使用的(数学)方法比现状更符合伦理(更公正)。因此，在目前的状态下，除了数据隐私之外，很少有关于心理测量学的伦理问题；数据隐私并不是心理学独有的问题。

然而，我们关心的是未来。心理测量学，当与认知科学相结合时，有可能通过说服塑造人类行为。人们可以设想将这种技术用于善的方面，例如增强我们的良性方面，如利他主义或帮助有抑郁症症状的个人，或者用于恶的方面，例如利用仇外心理或与病理成瘾相关的特征。这种未来心理学的伦理有点类似于基因编辑的伦理。就像基因编辑、原子能或互联网一样，心理学的这种未来科学将不会受到道德判断的影响。但它的用途不会。我们作为当代的居民，有责任放眼我们集体的道德景观，并确定这一未来的界限。