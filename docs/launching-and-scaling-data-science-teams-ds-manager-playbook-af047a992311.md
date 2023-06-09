# 启动和扩展数据科学团队:DS 经理行动手册

> 原文：<https://towardsdatascience.com/launching-and-scaling-data-science-teams-ds-manager-playbook-af047a992311?source=collection_archive---------19----------------------->

![](img/97e3f25de27f82939f33b6713eed9f9f.png)

## 构建和管理数据科学团队的最佳实践行动手册

这篇文章旨在成为管理数据科学团队的最佳实践指南。本节摘自我的 [*摘要*](https://medium.com/p/1761572eaa99) *关于启动和扩展数据科学团队的内容，并包含与* [*数据科学 IC*](https://medium.com/p/3fe596eef63d) *剧本的部分重叠。我的目标是一名数据科学家，他作为个人贡献者取得了成功，目前正在努力解决经典的管理问题:我如何扩展、聘用、评估和沟通？在数据科学领域，考虑到合格候选人背景的巨大差异，这是一个更加棘手的问题。这篇文章是为那些问这个问题的人写的:“作为一个数据科学 IC，我已经取得了成功，也承担了责任。我的任务是在 ________ 发展和领导一个团队。现在怎么办？”*

四句话总结:*不断调整你的组织结构。最大化团队的知识转移。有意识地安排你的优先顺序。强调解决方案的最低复杂度。*

# **持续调整您的组织结构**

在我剧本的所有句子四句话总结中，这是迄今为止最笼统/定义不清/晦涩难懂的。我很抱歉。然而，有一个原因:弄清楚如何建立一个数据科学团队真的很难；没有固定的“最佳实践”，它高度依赖于公司和员工，而且完全可以肯定的是，无论您现在有什么设置，都不是六个月前或六个月后的正确设置。我找到的资源和我采访过的经理给出了不同的、通常是矛盾的解决方案。我已经研究了如何建立一个数据科学团队足够长的时间，以确信没有正确的答案。作为一名经理，你能做的最好的事情就是不断调整你的组织结构、数据架构、工作流程和文化，以确保你积极地扩展你的团队来满足你公司当前和未来的需求。

数据科学组织最大的问题:你的团队是集中式的还是嵌入式的？*集中式*:坐在一起工作的数据科学团队，扮演内部顾问的角色。DS 团队的“时间”是分配给业务涉众的资源。*嵌入式*:个体数据科学家坐在特定的产品团队中，他们的日常工作在团队中高度迭代，他们的项目范围有限。DraftKings 有一个集中的团队，Zillow 也是。Wayfair 两样都有。 [Airbnb](https://medium.com/airbnb-engineering/at-airbnb-data-science-belongs-everywhere-917250c6beba) 因为这个原因从集中式迁移到混合式:

> 我们从集中式模型开始，被它提供的相互学习的机会所吸引，并在度量、方法和过去工作的知识上保持一致。虽然这都是真的，但我们最终是在决策业务中，并且发现我们无法成功地做到这一点，因为我们的合作伙伴团队不完全了解如何与我们互动，我们团队中的数据科学家不了解他们要解决的问题的完整背景，也不知道如何使其可行。

Chuong Do 有一篇关于集中式/嵌入式的优秀文章，强调了这两种方法的一些主要缺点。分散化可能会导致知识孤岛、更少的协作机会、更难实现标准(从代码库到雇佣)，以及难以共享基础设施/最佳实践。集中化可能会导致团队缺乏业务背景或合作伙伴的支持，还可能导致“数据科学被视为一种支持功能，回答产品经理的问题，而不是作为真正的思维伙伴运营，并从数据知情的角度积极推动对话。”

在采访 DS 经理时，我普遍发现每个人都认为他们有一个混合解决方案，该方案吸收了集中化和嵌入式的最佳部分。这既是不可能的，也是努力的目标。要认识到，在非常成功的公司里，非常有才华的人对实现这一点的最佳方式意见不一，所以作为经理，你的职责是在这个时候为你的团队做出最佳决策，并完全适应这种结构可能不再有意义的时候。以下是我对构建一个组织结构的想法，并警告说它们可能会在未来发生变化。

从一个集中的团队开始。当你开始雇佣数据科学家时，你会有许多需要支持的业务团队，而数据科学家太少，无法嵌入。在拥有小团队的小公司，知识共享/业务背景/跨职能协作更容易。办公室可以很小，你知道每个人在做什么。“知识筒仓”风险较小。

当算法密集型项目变得足够大，以至于永远需要数据科学家时，嵌入数据科学家。Wayfair 慢慢将数据科学家嵌入他们的定价、推荐和营销团队；认识到这些业务部门是如此基于算法和分析，他们需要由面向业务的数据科学家领导。Zillow 的 Zestimate 团队也是如此。Wayfair 和 Zillow 都有一个集中的数据科学团队来支持他们的其他业务。要小心，从招聘和资源管理的角度来看，嵌入数据科学家更难——这些 IC 需要有高水平的商业敏锐度，他们不会在其他任何事情上工作。因此，在为嵌入式结构招聘员工时，时机更为关键。

嵌入式数据科学家应该尽可能长时间地向中央数据科学部门报告。嵌入肯定会导致业务环境的知识孤岛。这里的目标是尽可能长时间地调整其他一切。数据科学家如何工作、协作、共享代码和结果、评估成功。这是针对*知识转移、*的优化，我们稍后会谈到。

当你捉襟见肘时，向外看，扩大你的团队规模。但是要非常挑剔，尤其是更早的时候。永远不要低估糟糕招聘的复合成本。要深思熟虑，不仅要评估人才，还要看是否是合适的人才。你需要一个研究型的雇员来不断完善业务的核心部分，还是需要一个快速行动的 IC 来支持许多团队，完成许多 2-3 天的问题来获得 90%的正确答案，然后继续下一件事？在接下来的六个月里，这些需求会有什么变化？

最后，根据你的非技术业务团队的能力和支持做出人事决策。他们能力如何？在这家公司做分析师意味着什么:轻 Excel 技能还是重 SQL？您公司的其他部门与数据科学团队的关系如何？业务利益相关者是强烈要求 DS 解决方案，还是抗拒改变？我交谈过的大多数公司都是技术型的(< 10 年历史，裸露的砖块，免费的十字架，站立的桌子，桌上足球)，并希望获得更多的数据科学支持，但我与一家 40 多年历史的媒体公司进行了一些最有趣的对话，该公司以不使用技术和自动化而自豪。我从首席运营官那里得到的建议是:

> 我们不希望这些人在组织中四处游荡，寻找需要解决的问题。最坏的想法是把它们抓来放走。你必须有一个与愿意接受数据文化的利益相关者高度接触的模型。花点时间与目标团队坐下来，听听他们的痛点、机会点，以及对他们的评估。让他们问:“你能给我们展示一下你在做什么吗，它是如何令人沮丧的，我们每天都在做什么，我们是如何获胜的。”

首席运营官的经历让我大开眼界；在我工作过的所有地方，企业都想要更多的数据科学资源。这不是普遍真理。作为一名经理，你需要了解企业的数据文化，以及利益相关者为你的 ICs 提供成功所需的环境和支持的能力。你必须在 DS 团队和公司其他人之间建立信任点，这需要雇佣能够与你的利益相关者合作的 ICs。你的数据科学家越是以商业为导向，你的公司越是技术能力强和开放，这就变得越容易。

# **最大化您团队的知识转移**

我开始我的项目是为了传达这一点，我怎么强调都不为过。IC 数据科学家可以像对待个人运动一样对待他们的工作，而你的工作就是让他们像团队一样工作。Kaggle 和大多数数据科学教育资源都是个人行为。你不能假设一个具有定义的工作流和编码标准的协作团队会自己出现，事实上，我见过相反的情况——个人并肩工作，在他们的本地机器上，用 Python 2.7、3.5 和 r 多次解决相同的问题。

你的目标必须是增加团队成员之间的知识转移和指导，这将产生更好的代码和更强大的团队。这需要从“个人贡献者”到“团队成员”的文化转变。没有这种转变，你就无法培养出工业级的数据科学团队。你必须强调，集成电路评估将不仅仅基于项目，还基于开发其他项目和改进团队工作流程、标准和工具。从代码审查开始。

代码审查:通常，人们认为这是一种在网站崩溃之前检测有缺陷代码的方法。这当然是好的，也是必要的，(当我忘记在“`ORDER BY`”中的“`DESC`”来对产品排名进行排序时，我会喜欢代码审查，这导致 Wayfair 在四个小时内从最差到最好地推荐项目，这在当时是一个 20 万美元的错误)，但当代码不能关闭网站时，或者没有人在技术上像编写它的人一样有能力时，代码审查通常被视为没有必要。

这错过了代码审查最好的部分:这是一个学习机会，一个专门的“双向道”知识转移，迫使团队成员学习其他人如何编写代码。团队中最没有经验的成员可以从审查别人的代码中学到很多他们不理解的东西，而最有经验的成员可以学习如何指导。代码评审不应该只是回答这个问题，“这会破坏网站吗？”。审查应侧重于:

*   这段代码是否可读、有弹性、注释良好、符合团队标准？是正确的抽象/参数化/模块化水平吗？(计算机科学原理)
*   这些数据科学和统计方法是否合理？(数据科学原理)
*   这些连接是否有效，这些表的结构是否针对日常插入进行了优化，它们的设计是否正确(基于它们应该是读的还是读/写的)？这对以任何方式使用 Hive/HDFS/Presto 或 ETL 的团队来说都是必不可少的。(数据库管理员原则)
*   这项工作是正确的，可操作的，令人信服的，表达良好的，可实施的吗？(商业原则)

为了使知识转移和代码审查有效，团队必须对“好”的样子有一套共同的信念。因此，管理人员必须以协作、灵活的方式定义一套团队标准和工作流程——同时告诫人们，鼓励个人寻找更有效、更强大的方法来改进这些标准和工作流程。对这个问题的答案进行逆向工程:“如果我团队中的一名数据科学家想出了一种更好的方法来做某事，那么这种知识如何每天以非选择退出的方式传递给其他人？”Domino 写道:目标是以模块化的方式实现复合协作。“当高绩效的数据科学团队站在前人的肩膀上时，他们的工作效率最高。”这是我理想世界中的一个团队工作流程/标准集的例子:

> 我们的团队使用相同版本的库 Python、Scikit Learn、Presto、Tensorflow 和作为 SQL IDE 的 DBeaver、作为 Python IDE 的 Jupyter 以及用于作业调度的 Airflow。在所有这些领域，人们有不同的偏好和不同的知识基础。这是一种优势——帮助我们提出新的和改进的管道和标准。就像个人因对业务产生积极影响而受到鼓励和奖励一样，他们应该不断寻找更有效或更强大的方法来改善团队的工作流程和习惯。将有代码审查，无论是业务案例，效率案例，以及代码是否写得正确。(符合 PEP8 标准，棉绒)。只有格式正确的代码才会被提交。代码审查被看作是一个教授和学习以及捕捉错误的机会。回购是在项目层面上组织的，人们通过协作工作，让新的集成电路达到速度，并让有经验的集成电路有能力承担教学/领导角色。

关于业务背景和过去团队项目的知识也必须转移。公司有许多名称的解决方案:站立，讲故事活动，学习学院，午餐和学习，头脑风暴会议，知识仓库，维基。Airbnb 有一篇关于数据民主化的博文，他们所有的项目都以一页纸开始，上面写着“项目的意义是什么，我将如何去做，我将如何衡量成功”。

这些解决方案中的任何一个都可以在给定的时间点上为一个团队工作，但是我与之交谈的许多人都警告说它们不能很好地扩展。关于讲故事活动:“我们曾经有两个团队，每个月做一次报告。现在有九个团队——我的人已经工作了六个月，但我不知道三个团队在做什么。”在头脑风暴会议上:“当我们还小的时候，这很有效，现在我有一些我没见过的人给我建议，告诉我如何解决他们以前没见过的问题。”午餐时，他学到:“一旦事情变得太大，人们就不再去了”。

代码评审必须以非选择退出的方式执行，当你的组织规模扩大到原来的三倍时，知识转移也必须如此。您的工作是让您的团队支持有效的流程，并传达数据科学 IC 工作的一个重要部分是记录他或她试图解决的问题、他们如何解决问题以及解决方案的成功。

# **有意识地安排你的优先顺序**

IC 数据科学家和商业利益相关者的一个普遍共识是，他们不知道项目是如何优先排序的。优先级划分的范围从高度数据驱动(“我们根据每个数据科学家周的估计收入增长来考虑所有事情”)到故意不根据数据驱动(“没有必要确定优先级，我们只是做重要的事情，事情不需要被证明是合理的或优先的”)到有点政治性(“这是一个足够小的公司，知道你是否过度使用或未充分使用数据科学资源”)到高度政治性(“我们支持的每个团队都平等地使用我们的时间”)。同样，IC 数据科学家扮演的角色从“我选择我的项目”到“我不选择我的项目，十个项目被分配给十个人，他们不知道为什么。”

作为数据科学领导者，您必须迫使您的公司在“业务影响”是什么以及如何衡量它这两个问题上保持一致，并且您必须确保您的 ICs 和业务利益相关方之间关于优先顺序的对话是自下而上和自上而下的。您必须引入数据科学团队通常不具备的透明度。一位 DS 经理告诉我，“每个项目都有不同的价值，我们无法相互比较。”如果你不能比较项目，除了政治上的，你怎么可能分配它们？在同样的程度上，你必须认识到，在数据科学中，很难解释容易和几乎不可能之间的区别。您的数据科学家对于要做的业务问题肯定有不完善的信息，您的利益相关者对于解决方案需要做多少工作肯定有不完善的信息。您必须从业务利益相关者和 ICs 处获取项目，汇总关于这些项目的预期成本/收益的完整信息集，汇集这些数据，并以一种对各方都显而易见的方式呈现这些数据。

这一过程的关键是确定您的数据科学团队是合作伙伴，而不是服务提供商。你的团队必须通过证明他们的价值来建立信任和权威:寻找好的项目，获得巨大的成功，并就此进行报告。这允许团队开始文化:“如果你有一个想法，我们会和你一起工作，但我们有一个桌子上的座位。”你应该避免门票系统和政治，团队资源按比例分配给依赖的利益相关者。做到这一点的最佳方式是证明你和你的团队有足够的业务背景，可以与他人合作——如果你给我们解决这些问题的空间，我们会的。

# **强调解决方案的最低复杂度**

[数据科学 IC 行动手册](https://medium.com/p/3fe596eef63d)的*了解何时完成* 部分与本部分高度重叠。翻新:通过学术或在线方式接受教育的 IC 数据科学家没有接受评估其算法复杂性的培训。许多 IC 承认他们不知道问题何时解决，大多数表示时间管理是他们正在努力改进的。你的工作是建立准确性与时间和复杂性之间权衡的框架，并帮助你的直接下属找出他们应该继续前进的时间。认识到你的集成电路有一种自然的倾向，那就是花费太长的时间来构建太复杂的解决方案。设置护栏是这项工作的一个重要部分，培养团队的常识和直觉以正确的方式开始解决问题也是如此。

你必须平衡这些标准和个人发展:认识到人们确实需要学习和成长的空间，这需要用新技术解决新问题。众所周知，数据科学组织深受对工具的执着之苦，我一直站在这两方:在我职业生涯的早期，我故意将自己放在较低优先级的点击流项目上，以学习分布式计算原理/Hadoop/Hive，当队友们做了同样的事情，为了构建卷积神经网络而构建卷积神经网络时，我感到沮丧(我们稍后将弄清楚图像分类如何改善业务)。目前，这种对工具的执着是 Tensorflow、Spark Streaming、GANs 和 RNNs，但过去是其他东西，将来也会是其他东西。多米诺称之为“一种‘银弹’思维文化，一些数据科学家认为只要获得 Spark Streaming 和 TensorFlow 就能解决他们的所有问题……在许多情况下，数据科学家将他们的工具争论作为一种荣誉徽章，这个徽章被包装在他们的身份中。”一个 IC 告诉我，“我知道 Tensorflow 不是解决我的问题的正确方法，但总是有同行的压力要使用这个很酷的东西。该公司的博客帖子和学习学院不是为了减少随机森林分类器的客户流失。这个行业发展如此之快，我需要跟上。”另一个人说，她衡量自己的个人成长，她实施了哪些新技术。

这两位数据科学家都不是不讲道理。拥有 Tensorflow 和 Spark Streaming 的经验可以在简历上产生很大的影响，你不会学到用同样的工具解决稍微不同的问题。DS 经理必须意识到具有正确工具/复杂性的解决方案与作为学习机会的解决方案之间的矛盾。您必须在工程解决方案与团队快乐和发展之间找到适当的平衡。像其他事情一样，认识到这是你需要有意识的、透明的和灵活的事情。一个很好的解决方案是给你的团队一个明确定义的时间段，让他们可以用他们想学的工具做他们想做的事情，但也给他们提供指导和阻力:“如果你想做一个更复杂的思考，告诉我你为什么要支付这个成本。”与你的团队交谈，确保他们感觉像是在学习，当你有机会让某人用对他们来说是新的解决方案或技术解决实际问题时，相应地分配“延伸”项目。