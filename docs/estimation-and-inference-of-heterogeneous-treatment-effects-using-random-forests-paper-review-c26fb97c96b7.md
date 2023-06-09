# “使用随机森林估计和推断异质处理效果”论文综述

> 原文：<https://towardsdatascience.com/estimation-and-inference-of-heterogeneous-treatment-effects-using-random-forests-paper-review-c26fb97c96b7?source=collection_archive---------1----------------------->

*本文原载于*【blog.zakjost.com】

*我最近读了很多技术论文，觉得用比学术论文不那么正式的语言来总结会很不错。我可能会也可能不会这样做不止一次。*

# *一.动机*

*[本文](https://arxiv.org/abs/1510.04342)是关于尝试使用随机森林(RF)进行因果分析。RF 在构建分类或回归预测模型方面非常流行，但是对结果做出经典的统计声明并不容易。例如，你的置信区间是多少？你如何得到 p 值？*

*再者，本文想对*的因果*的影响做出主张，或者说是一种治疗的效果。例如，大学对收入有什么影响？由于许多原因，这很难做到，但最根本的是，你没有你确切需要的数据，即每个人都上过大学和没上过大学时发生了什么的数据。这当然是不可能的，因为在现实中，个人要么上过大学，要么没有，你不知道如果另一种情况发生会发生什么——即反事实。这种构建问题的方式被称为“潜在结果框架”，本质上是假设每个人都有多种潜在结果，这取决于他们是否接受了治疗。*

*本文中的一个关键假设，以及这种类型的一般因果估计技术，是*未发现*中的一个。这意味着一旦你控制了感兴趣的变量，一个人是否接受治疗是随机的。这使我们能够将附近的点视为微型随机实验。在药物试验的情况下，你可以随机分配治疗，所以这不是问题，但如果你在分析观察数据，治疗已经被分配了——一个人上过大学或没上过大学。*

*为了支持无根据性，你需要选择和衡量所有会影响治疗分配的变量。对于四年制学位对收入的因果影响，你可能想要选择的协变量之一是家庭收入，因为某人是否上大学可能与其家庭收入相关。年龄可能是另一个因素，因为 18 岁的人比 50 岁的人更有可能上大学。这个想法是，当你看着绘制在你的家庭收入/年龄图上的两个相邻点时，是否上大学的决定应该是随机的。这是很有价值的，因为你可以把没有上过大学的人的收入减去上过大学的人的收入，你就可以估计出大学在家庭收入/年龄空间中的影响。*

*但是这很难，因为你可能会忘记一个重要的变量，或者你没有这个变量的数据。因此，这个假设肯定不完全正确，但它可能是正确的，足以给出有用的答案。*

*但是假设你有正确的协变量，作者使用随机森林将数据分成自相似的组。随机森林是一种*自适应*最近邻方法，它决定空间的哪些部分对您来说是相似的，而大多数最近邻技术倾向于平等对待所有距离。然后，他们添加了约束条件，即生成的叶子至少有处理和未处理两类，然后可以通过减去平均值来计算每个叶子的因果影响，就好像这是一个随机实验一样。一旦完成单棵树的评估，就可以对每棵树的评估进行平均。他们称这种实现为因果森林。*

# *二。这里有什么新鲜事*

*如上所述，要在传统统计学中做出稳健的统计声明，您需要像 p 值和置信区间这样的东西。这就需要知道统计量的*渐近抽样分布*。在这种情况下，这意味着我们需要知道，如果我们在无限数量的树木/数据上对随机森林估计量的平均处理效果进行采样，分布会是什么样子。如果我们有这样的数据，那么我们可以这样说:“平均处理是 0.3，这种来自随机机会的可能性小于 0.1%”*

*这里的内在假设是，我们可以推导渐近/无限数据情况的属性，并将其应用于有限样本的真实情况，但这通常是传统统计中的情况。*

*先前的工作已经完成，使渐近分析的随机森林，但本文建立的限制，需要适用于他们的因果森林。其中一个约束需要“诚实的树”，他们提出了两种增长算法。*

# *三。方法*

*完整的证明是非常复杂的，我不会试图在这里重新创建它，但我会简要概述一些约束和它们使什么。*

*首先，从传统随机森林的先前工作中重新创建了一个渐近理论，表明它是一个均值为零的渐近高斯估计量，这意味着它是无偏的。他们还提到了一种叫做*无限小刀切*的技术，用于估计方差，其中包括有限样本校正。*

*作者能够通过包含“诚实树”的概念来利用先前的工作。基本的想法是，你不能使用结果变量来分割*和*估计平均影响——你必须选择一个或另一个。他们提出了两种方法来做到这一点。第一种是双样本树，将数据一分为二:一半用于估计影响，另一半用于进行拆分。分割标准是最小化结果变量的 MSE。在双样本情况下，看起来您可能丢弃了一半的数据，但这种情况是针对单棵树的，随机森林正在对每棵树的新训练集进行采样，因此您最终将使用所有数据进行分割和估计。*

*另一种方法是通过生长“倾向树”,这是一种分类树，旨在预测治疗类别而不是结果，然后结果变量仅用于估计每片叶子内的影响。他们强加了一个停止标准，使你停止分裂，以保持任何叶中每个处理类的最小值。这是必要的，这样你就可以比较结果来估计效果。*

*通过使用诚实树并依赖于未发现的假设和树叶内的处理类别重叠，他们能够稍微修改传统的处理，以给出相同的无偏高斯渐近结果。*

# *四。实验结果*

*他们的基线使用的是 k-NN 算法。他们用已知条件创建一些模拟实验，模拟常见问题，然后应用因果森林和 k-NN 方法。*

*第一个实验将所有 x 的真实治疗效果保持为零，但在结果和治疗分配之间建立了关联，从而测试算法校正协变量以消除偏差的能力。这就像让算法自动计算出年龄和家庭是重要的，以及如何将它们分开。他们在不同的训练数据维度上多次运行该实验。他们报告了 MSE 和覆盖率，即真实值在估计值的 95%置信区间内的频率。因果森林比 10-NN 有一个数量级的改进，比 100-NN 有 5 倍的改进。CF 维持~0.95 的覆盖率到 10 维之后开始降级。10-NN 在 0.9 范围内保持了合理的覆盖率，100-NN 表现很差。值得注意的是，k-NN 的置信区间比 CF 宽得多，因此改进的覆盖率更令人印象深刻。*

*第二个实验有恒定的主效应和倾向，但真正的治疗效果只取决于两个协变量。然后，他们对不相关协变量的数量进行了缩放，以了解该算法在不相关协变量存在的情况下发现这种异质性治疗效果的能力。令人惊讶的是，CF 在高维比低维表现更好。他们通过注意到森林的方差取决于树木之间的相关性来解释这一点，并提出树木之间的相关性以及总体方差在更高维度时会降低。结果与实验 1 相似，MSE 好得多，或者至少与随维度扩展的更一致的覆盖相当。*

*注意到置信区间的覆盖范围在特征空间的边缘开始退化，特别是对于高维。这被解释为是一种由偏差支配的情况，在无限数据的渐近极限中会消失。值得注意的是，在边界的偏差是典型的树木，特别是最近邻非参数估计一般。*

# *动词 （verb 的缩写）讨论*

*虽然因果分析不是我的专长，但对于假设无发现的最近邻法来说，这似乎是一个很好的进步。在维持名义覆盖率的同时，MSE 的显著改善令人印象深刻。*

*然而，我发现这篇论文的几个方面令人困惑。特别是那些与树的分裂标准相关的。在倾向树的情况下，他们正在训练一个分类器来分离治疗类别，但他们反过来要求每个叶子中类别的异质性约束，这与分裂标准直接相反。*

*类似地，在双样本框架中，它们被分割以最小化结果 MSE，该 MSE 将具有相似结果值的点分组。但关键是，在通过治疗分类分离要点后，结果是不同的，这种差异就是平均治疗效果。再一次，分裂标准似乎与最终目标相反。为此，他们参考了一篇论文(Athey 和 Imbens [2016])，其中可能包含澄清。*

*最后，有一句话我不明白，但听起来很困扰。*

> ***备注 4。**(多点测试)我们注意到，通常不可能同时为所有 x 构建定义 4b 意义上的正则因果树。在实践中，如果我们想要建立一个因果树，可以用来预测许多测试点，我们可能需要分配不同的树对不同的测试点有效。然后，当在特定 x 进行预测时，我们将在该 x 被指定为有效的一组球座视为相关的森林，并对其应用定理 11。(第 19 页)*

*我不确定这是运营开销，还是更基本的东西。*

*你怎么想呢?*