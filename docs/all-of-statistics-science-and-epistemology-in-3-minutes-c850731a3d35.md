# 所有的统计学，科学和认识论在 8 分钟内。

> 原文：<https://towardsdatascience.com/all-of-statistics-science-and-epistemology-in-3-minutes-c850731a3d35?source=collection_archive---------1----------------------->

所有的统计学、科学、认识论、“数据分析”、“机器学习”(或者更确切地说，“学习”)等等。归结到一点:通过看尽可能少的页数来“判断”尽可能多的书。更一般地说，它们都遵循以下步骤(这实际上是一篇[前一篇](https://medium.com/towards-data-science/thoughts-on-data-science-and-epistemology-d816b3019743)的扩展):

1.  现实就是这样。它很复杂，很大，并且有许多活动部件。如果我们对处理一个特定的问题感兴趣，我们可能永远不会看到全部，也不需要真正理解全部。
2.  有数据，我们可以看到和测量的现实的子集。它是从现实中取样的，几乎肯定是通过某种实际上并非随机的机制。
3.  我们对现实有一些想法。我们如何得到它们，没有一致的逻辑。有些事情，我们只是相信。有些事情，我们通过查看数据来了解。有些事情，我们相信通过按照某种逻辑把点连接起来。

造就“科学”的是将信念和数据联系起来的态度:至少，原则上，我们不会把信念看得太重，以至于我们不会忽视数据。然而，即便如此，数据和信念的相对权重还是不同的。[阿里尔·鲁宾斯坦对 Dani Rodrik 的新书](http://arielrubinstein.tau.ac.il/papers/100.pdf)的精彩评论为一场关于经济学(就此而言，整个社会科学)有多“科学”的长期争论提供了一个窗口。是对数据的态度，比如对科学方法的整体强调，良好的研究设计，以及对数据的有效利用，有利于科学吗？或者，正如罗德里克所强调的那样，是演绎逻辑思维使某些东西成为科学吗？或者更现实地说，是两者的结合？当然，这是一个古老的争论，可以追溯到几个世纪以前，甚至更久。Vladimir Vapnik 不仅是统计学习方法的发明者，也是一位专注于哲学思考的严肃思想家，他关于支持向量机的书在一旁引导了波普尔和其他人对科学和经验思维之间联系的思考——例如，占星术(其从业者越来越多地开发出一致的逻辑和复杂的数学模型来支持他们的工艺，不管它有什么价值)或气象学(其产品仍然不完全可靠，特别是中期或长期)是否符合科学的条件。

值得指出的是，鉴于牛顿物理学(或后来的量子力学)诞生时可用的技术，它们提供了相对较少的明显可检验的预测，许多预测是相当错误的(尽管它们也解释了为什么它们是错误的——并且潜在地为如何评估它们铺平了道路，如果可用的技术允许它们的话)。看起来，科学之所以存在，是因为某种东西并不盲目地服从于经验主义或理论。它让自己被经验数据证明是错误的，但只是在它自己的内在逻辑中提出的条件下。换句话说，科学依赖于一个条件逻辑:不是 X 无条件地是 X，而是只要 A、B 和 C 成立，X 就是 X。因此，从经验证据来看，第一运动定律并不是明显错误的——因为在地球上很少看到永恒的运动，而大多数人都是在地球上收集数据的——因为它列出了它成立的条件(例如，缺乏摩擦，这排除了我们拥有的大部分数据)。

科学的条件逻辑对于它在现实生活中的实际应用，或者说缺乏(直接的)普遍的实际适用性是至关重要的。科学奠定了围绕其理论基础构建的大图景，但现实是噪音。为了科学地对待现实生活，我们需要分两步走:放下大局，然后确定现实生活偏离大局的不同方式。通常，成功的工程利用了第二步，而不是第一步。继续一个愚蠢的例子，牛顿第一运动定律对于实际用途来说是相当无用的——如果所有的东西永远保持运动，我们就不需要担心燃料效率之类的问题。交通运输面临的真正挑战是摩擦，也就是牛顿第一定律所说的“噪音”。但是没有必要完全理解摩擦为什么以及如何工作，只需要理解摩擦在特定情况下的作用。所以我们有一个巨大的表格，里面有各种情况下的摩擦常数，可以用来近似摩擦如何影响运动，我们可以用它来实际应用牛顿定律。(顺便说一句，这就是为什么我发现对“数据科学”中的“科学”的痴迷可能令人不安。我看不出大数据及其噪音如何能提供比牛顿物理学更具体应用的一般理论。我希望大数据能够发现可以作为有用起点的一般原则，但对于特定应用，我们需要弹簧常数、摩擦常数和所有其他半规则基础上重复出现的条件噪声测量值——半规则是指它们的影响是概率分布，而不是无噪声的标量。我们拥有的数据越大，我们就越能够将数据分割成符合特定条件的子集，并为这些条件估计适当的“常数”，以及建立规则和常数可能不可靠适用的子集——本质上是建立效应分布方差的一部分。本质上，我们需要更好地理解噪音，对大模式的偏离。非常坦率地说，要看到大的模式，我们不需要该死的大数据——如果它们足够大，我们甚至可以在小数据中看到它。当然，这更多的是一种工程思维，也许是一种古老的思维。)

这种更倾向于附加条件的思维方式，意味着数据收集的方法不同于目前的做法。目前太多的想法似乎集中于收集更多的数据，而不考虑数据来自哪里。但是大多数数据是无趣的、普通的、可预测的。为了估算火星的“常数”，我们需要火星数据，而不是地球数据。如果我们真的想知道火星常数，我们需要花费数十亿美元发送探测器到那里收集一些观察数据，而不是派人去纽约市周围收集大量数据。换句话说，大数据更有用，因为作为收集它的副产品，我们可能会捕捉到更多我们可以使用的不寻常数据——数据越大，在大量不太有用的数据中可能会有越多有用的小数据。但是，根据数据收集过程，即使是最大的数据也可能无法捕捉到我们需要的足够多的数据。

当然，我们估计的这些常数不会是“真理”，而是“有条件的真理”——我们认为是真实的东西，因为我们在特定的条件下，从我们所拥有的我们(应该)知道是不完整的数据中归纳地了解到了它们。(但是，演绎真理也不是——它们之所以是真理，只是因为我们认为现实的逻辑基础实际上是广泛适用的逻辑基础。除了通过公理化的断言，我们怎么知道呢？甚至建立我们“演绎地”认为是真实的东西也需要经验主义，或者至少可以从经验主义中获益。)它们把我们带回了统计学的根本问题(相对于概率论):我们不知道真相。我们只能从现有的数据中推断出真相。如果我们看到数据中的模式，我们只在我们能扔出数据的范围内信任它们——有时，我们不能扔得很远。知道我们能把数据扔多远(以及收集和分析它们的方法)是我们需要时刻牢记的事情。现代技术大大增强了数据分析的力量，但一些数据仍然非常沉重，难以丢弃，而且无论如何，没有数据可以跨越无限的距离。如果数据说答案是三，不代表答案是三。这仅仅意味着，在我们的询问下，数据显示答案是三。因此，我们相信我们收集和分析数据的方式如此反映现实，以至于我们认为答案是三——这在大多数情况下可能足够好——但有时可能不是。我们可能想知道“有时”可能是什么，也许是考虑我们是否应该购买保险的时候。

我在这里并不完全公平:实验的精神，或数据科学人士的 A/B 测试，反映了这种逻辑，即“自然”可用的数据是有限的，有时需要寻找非自然的数据。然而，更早的实验设计著作，对拉丁方块和其他怪物的详细描述，更热衷于为正确的比较建立适当的匹配，以及为理论产生必要的信心所需的功效计算。人们可能会考虑如何在现代环境中创造性地使用过去精心设计的实验设计。条件均值和方差对于建立实验效应的显著性是至关重要的:我们知道如果 P(X1 |除了 A=0 之外，其他都一样)！= P(X1 |除了 A = 1 之外所有我们知道的都一样)，也许 A 和 X1 有关系。但这将概率论的逻辑叠加到了统计学上:我们(认为我们)知道 A 有影响，所以我们会审问数据来找出答案，即使是通过创造自然界罕见的半人工数据(通过实验。)但是，通常，我们并没有一个很好的理由来预测 A 是否对 X1 有影响。我们希望依靠某种算法来发现是否存在像 a 这样的变量。

嗯，我们已经一直在做这种事情:我们有列联表(或数据透视表)。我们切割数据，并显示它们的条件均值和其他统计数据，这取决于它们属于哪个子集。潜在的警告是，表越大越难理解——但算法不会被这种事情困扰。我们已经在分类方案中使用相似性度量:利用这一点来人为地为所有感兴趣的变量创建“除了一个(或两个或三个)之外，所有变量都相似”的分数，并比较条件均值，这并不是一个很大的飞跃。条件上足够大的差距意味着→值得用人眼进一步看的东西。这在逻辑上相当于一个巨大的拉丁方，存在有趣的缺口，无论是样本中条件平均值的计算值，还是潜在分组缺乏足够的数据，都由编程到算法中的逻辑确定。(潜在的同样有趣的方法是关注条件方差:一些变量组合在一起，可能会产生非常可靠的预测；其他组可能会导致真实数据到处都是。预测分析人士可能会对发现这样的团块感到震惊，但这只是寻求人类更密切的关注和想象力。)

科学不是数据，数据不是科学，科学也不是演绎逻辑。事实上，科学是对数据的巧妙利用与基于条件概率的演绎逻辑的创造性融合:不是 A → B，而是 A →B|X，Y，但不是 Z，所有有趣的发现与其说与 A →B 有关，不如说与“X，Y，但不是 Z”有关(A → B 很好，但它可能不需要大数据或巧妙的实验。)我认为，当科学被应用于实际目的时，条件概率理论更加适用。我们没有永动机。我们需要知道在不同的环境下需要担心多少摩擦——噪音，如果你愿意的话。重要的是，我们现在越来越多地拥有技术和数据来系统地分析大量数据中的噪声，也就是说，如果我们愿意走这条路的话。不幸的是，有条件的部分经常被委托给“技术细节”:所以人们被说服，“嘿，科学！说 A →B！”不，不是真的。真正的“科学”在于“X，Y，而不是 z”。具有讽刺意味的是，经过适当的训练，人类实际上非常擅长破译条件概率，即使有大量的细微差别。人类不擅长的是钻研大量数据，并看到大的模式——因此人类过度拟合和过度概括，看到罕见的情况太多了。如果人工智能可以帮助指出人类真正有用的地方，并让他们远离他们容易看到不存在的东西的地方，那将是一件很酷的事情。

PS。原标题是 3 分钟，然后我岔开话题，继续说了很长时间…