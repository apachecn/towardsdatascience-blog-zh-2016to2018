# 学术数据科学的现代化

> 原文：<https://towardsdatascience.com/modernizing-academic-data-science-4213794a278e?source=collection_archive---------19----------------------->

## 软件工程提高科学再现性的四个原则

# 摘要

再现性对科学进步至关重要，然而大多数结果是无法复制的。今天的研究论文严重依赖编程分析管道和统计模型。几十年来，软件工程在如何实现高质量、可复制的代码方面已经发展出了最佳实践。然而，科学家通常没有接受过这些最佳实践的培训。这篇文章涵盖了在科学研究中促进更大的可重复性的四个基本原则: [c](https://medium.com/p/4213794a278e#5ed4) [ode review](https://medium.com/p/4213794a278e#11c8) 、 [t](https://medium.com/p/4213794a278e#dad1) [esting](https://medium.com/p/4213794a278e#bd45) 、[版本控制](https://medium.com/p/4213794a278e#9229)和[d](https://medium.com/p/4213794a278e#7399)[documentation](https://medium.com/p/4213794a278e#f3ae)。

![](img/068406b8ae2bc9f5044ac5a3018b59f4.png)

Photo by [Conner Baker](https://unsplash.com/@connerbaker?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 不可复制的研究问题

在我研究生生涯的末期，我把研究[再现性危机](https://www.nature.com/news/1-500-scientists-lift-the-lid-on-reproducibility-1.19970)作为一个兼职项目，这对于一个越来越失望的博士生来说，就像往火上浇汽油一样聪明。一些已发表的研究发现是虚假的这一事实并不新鲜，也不特别令人担忧。然而，令人担忧的是越来越多的证据表明大多数发表的研究发现是错误的。

心理学首当其冲受到负面压力，但是危机影响了大多数领域。例如，整个神经科学的统计能力大约是[的 20%](https://www.nature.com/articles/nrn3475)。虽然最新的[力量姿势](https://www.sciencedaily.com/releases/2017/09/170911095932.htm)或[超感知觉](https://papers.ssrn.com/sol3/papers.cfm?abstract_id=2001721)的后验概率可能不会让你晚上睡不着觉，但在[癌症生物学](https://www.nature.com/articles/483531a)、[遗传学](https://journals.plos.org/plosgenetics/article?id=10.1371/journal.pgen.1006916)或[药理学](https://www.nature.com/articles/nrd3439-c1)中持续失败的重复发现可能会引起关注。

![](img/46078e5f8ae07619a8b87d37a5c93ac4.png)

by [KC Green](http://www.gunshowcomic.com/)

学术界扭曲的激励结构是许多统计滥用的核心。有意义的结果会带来金钱和名声，而无意义的结果则什么都不会。证据的二分法[使得选择性报告变得更加容易，在专业上也更加方便。](https://www.amstat.org/asa/files/pdfs/P-ValueStatement.pdf)

这个问题不仅仅是显性的黑客攻击，而且更隐蔽的是，隐性的黑客攻击来自于丰富的研究人员自由度和我们对正面结果的偏好。这就是为什么再现性在如此多的领域受到关注，包括[机器学习和 AI](https://neurips.cc/Conferences/2018/Schedule?showEvent=12486) 。

# 走向可复制的研究

越来越多的人意识到缺乏再现性，这导致了许多最近的提议，例如[移动显著性阈值](https://psyarxiv.com/mky9j/)，用[效应大小和置信区间](http://www.claridgechang.net/uploads/2/4/9/8/24985510/estimationmethods-eprint.pdf)补充分析，或者切换到[贝叶斯因子](https://www.annualreviews.org/doi/abs/10.1146/annurev-statistics-031017-100307?journalCode=statistics)。这些都是统计学上的原则性建议，但却非常不令人满意。为什么？因为这些充其量只是权宜之计，持久的解决方案需要文化转型。

从这个转变开始的一个地方是软件开发，它是许多学术研究项目的核心。因此，从软件行业借用促进可复制性、透明性和健壮性的原则是合乎逻辑的。

> [代码评审](https://medium.com/p/4213794a278e#11c8)[测试](https://medium.com/p/4213794a278e#bd45)[版本控制](https://medium.com/p/4213794a278e#9229)[文档](https://medium.com/p/4213794a278e#f3ae)

为什么这些实践在学术界几乎不存在？一个原因是研究生院没有跟上数据的指数增长。例如，即使在排名最高的神经科学博士项目中，定量方法和编程基础的课程也是可选的或过时的。哈佛大学为即将入学的研究生开设的定量方法训练营仍然只专注于 Matlab。封闭源代码和昂贵的许可证不太可能成为可复制科学的良方。

结果是学生必须自己发现现代数据科学和开发的最佳实践。但是实验室已经采用了没有支持机器的编程语言。学术实验室不是工业软件开发团队，也不应该是。但是工业中使用的确保可再现性、功能性和透明性的工具与学术界高度相关，并将培育更多可再现的科学。

# 提高科学再现性的核心原则

# 1)代码审查

想一想，与对产生结果的代码关注甚少相比，一份手稿在出版前受到了多么严密的审查。除了原作者之外，有多少人曾经看过这些代码？

在开发团队中，代码通常由多人进行同行评审。这有助于确保预期的功能、缺陷的识别以及对标准的遵守。

你是怎么做到的？逐行阅读代码，并问自己:

*   我能容易地理解代码吗？
*   代码做了它应该做的事情了吗？运行它。
*   代码遵循标准吗？ [Python 风格指南](http://google.github.io/styleguide/pyguide.html)， [R 风格指南](https://google.github.io/styleguide/Rguide.xml)
*   代码[是模块化的](https://www.quora.com/What-is-modular-code-How-do-you-write-it)吗？

![](img/c11e51d72cbc19615a56123570e1f3ee.png)

审查者应熟悉编程语言，但可能不具备生理或统计模型方面的专业知识。因此，纳入防弹测试方案是一个好主意。

# 2)测试

> 第一个原则是你不能欺骗自己——而你是最容易被欺骗的人。— [理查德·费曼](http://calteches.library.caltech.edu/51/2/CargoCult.htm)

软件测试有很多种，有手工的，也有自动的，覆盖了不同程度的粒度。它们通常分为三类，针对单个功能或类的单元测试，针对多个组件的集成测试，以及针对整个项目的端到端测试。测试的目的是验证一个软件的行为符合预期，并预测它会如何崩溃(就像这个癌症研究被一行代码搞垮一样)。

在学术界，最常见的担忧之一是虚假的发现。测试将有助于防止虚假的发现。

例如，线性判别分析通常在许多研究项目中用作分类器。然而，如果不满足模型的假设，分类结果很容易被误解。最关键的假设之一是特征不在[共线](https://en.wikipedia.org/wiki/Multicollinearity)。下面是一个测试示例，如果违反了这个假设，它将停止执行并引发错误。

测试可以加快调试速度。当依赖于依赖于快速发展的库的[复杂代码库](https://www.pnas.org/content/113/28/7900)时，这个事实尤其突出，这些库可能与来自另一个实验室的遗留代码集成在一起。

好的测试需要仔细考虑数据和可能的故障点。这使得编写测试本身成为一项有用的练习。[这里的](https://realpython.com/python-testing/)是更详细的 Python 测试入门指南。

# 3)版本控制

版本控制系统回答了这个问题:“谁在什么时候做了什么？”通过记录和维护可以随时比较的代码版本。它已经存在[大约 50 年了](https://en.wikipedia.org/wiki/Version_control)，但我们不知何故找到了一种方法来保持命名文件*version _ final-edits _ December _ 2018*。如果你不熟悉版本控制，你正在为你的同事和你未来的自己创造更多的工作。

目前最流行的版本控制系统之一是 Git，它与 Github(Git 托管服务)一起使用。

[](https://codeburst.io/number-one-piece-of-advice-for-new-developers-ddd08abc8bfa) [## 新开发者？你昨天就应该学会 Git。

### 我给新开发人员的第一条建议是:学习 Git。

codeburst.io](https://codeburst.io/number-one-piece-of-advice-for-new-developers-ddd08abc8bfa) 

学习 git 是一项可以立即收回成本的投资。它将防止丢失您的工作，不记得昨天工作了什么，没有您的代码的功能版本来共享，等等。

不要让成千上万的“简易 Git 指南”使你相信这是软件工作流程中每个人都非常了解的琐碎方面。就连 Git 的开发者 Linus Torvalds 也称之为“[该死的白痴的一卡车 sh*t:当它坏掉的时候](https://github.com/git/git/blob/e83c5163316f89bfbde7d9ab23ca2e25604af290/README#L13)”。有效使用 Git 需要时间，[这里有一些资源可以帮助](https://try.github.io/)。

# 4)文件

你可能已经发现了强 AI 缺失的关键，但没人能说得清是不是不可能理解你的代码。虽然有少数优秀的[可以手动修补二进制代码](https://blog.0patch.com/2017/11/did-microsoft-just-manually-patch-their.html)，但编辑带有文本字符串的代码可能更容易，它会告诉你代码做了什么。这些有用的字符串称为文档字符串。以下是用 Python 编写的函数的文档字符串示例:

[Sphinx docstrings](http://www.sphinx-doc.org/en/1.7/ext/example_numpy.html)

三重引号`"""`之间的文本解释了这个函数做什么，预期的输入和输出以及它们相关的类型。在 Python 中，包含文档字符串的[约定](https://www.python.org/dev/peps/pep-0257/)是将它们作为每个模块、函数、类和方法的第一条语句。

在编写文档时，您没有义务遵循任何规则，但是使用一个标准，比如 Python 中 NumPy 库的[这个](https://numpydoc.readthedocs.io/en/latest/format.html)，可以自动生成有用的参考指南。经验法则是使用你加入的团队已经习惯的东西，因为细节没有坚持和一致性重要。

文档也不是可读性的唯一方面，但它是一个非常强大的起点。

经过审查、测试、版本控制和记录的软件将极大地提高结果的健壮性和可重复性。

如果有兴趣进一步优化，可以考虑[集装箱化](/how-docker-can-help-you-become-a-more-effective-data-scientist-7fc048ef91d5)带码头、[打包](https://nsls-ii.github.io/scientific-python-cookiecutter/)，以及[持续集成](https://neurohackweek.github.io/software-testing-for-scientists/)。值得记住的是，再现性是一个光谱，小的变化会产生大的影响。

感谢刘冉和克里斯·霍布森的有用的评论，删除了许多不必要的段落，并感谢德克斯特·杜克沃斯提高了例子的质量。

如果你想聊天，请联系我，你可以在这里找到我。