# 改变工程师的思维模式:从如何到为什么

> 原文：<https://towardsdatascience.com/the-engineering-mindset-6b9d9368207e?source=collection_archive---------8----------------------->

![](img/046cb2c15b8eb2efd46ff68a54eb6b25.png)

在一个数据泛滥、信息泛滥的世界里，我们可以在家里舒适地从看不见的云上发现外面的天气；是时候后退一步，问一些重要的问题了。

作为这篇文章的前言，我想提一下，我是技术的倡导者。认为一个没有技术有效解决我们问题的理想世界存在是愚蠢的。也就是说，指数级增长会带来难以预料的结果。这就是为什么在当今社会意识是关键。技术幕后的神秘绿人到底是谁？

## 介绍:工程师思维

当一名工程师在工作中遇到问题需要解决时，他首先想到的问题通常是**如何**？我将如何创建此解决方案？我将使用什么样的技术？应该有多高的性能？这个可以优化吗？效率似乎是感兴趣的话题。然而，这里的问题是，没有人问**为什么是**。我为什么要造这个火箭？为什么我的公司资助战争？为什么我不能和家人分享我的工作？

计算机伦理领域的先驱 James H. Moor 简洁地解释了这个问题:

> “多年来，计算机一直被用来计票。现在，选举过程正变得高度计算机化……在上次美国总统选举期间，电视网络不仅在加州投票结束之前，而且在纽约投票结束之前就预测了结果。事实上，当宣布获胜者时，半数以上的州仍在进行投票。问题不再是“在一场公平的选举中，计算机计票的效率有多高？而是“什么是公平的选举？“有些人在投票前就知道结果，这合适吗？问题在于，计算机不仅将每位候选人的选票制成表格，还可能影响这些选票的数量和分布。不管是好是坏，我们的选举过程正在改变。”

是时候改变工程思维了，不要只问怎么做。相反，我们必须停下来问 ***为什么*。**

# 我们什么时候应该问为什么？

我想说，我们应该一直问为什么，尽管为了提供具体的指导，我将讨论我们应该保持警惕的三个技术领域。

1.  **机器学习偏差**
2.  **数据收集和隐私**
3.  **人工智能**

## 从机器学习偏差开始

在过去的一年里，我致力于公平模型的数据科学研究。为了更全面地了解这个主题，我强烈推荐 [Hanna Wallach](https://medium.com/u/f3d398e4ca62?source=post_page-----6b9d9368207e--------------------------------) 关于[大数据、机器学习和社会科学](https://medium.com/@hannawallach/big-data-machine-learning-and-the-social-sciences-927a8e20460d)的文章。

对我来说，一开始是对数据偏差的介绍，但很快演变成了一个关于机器学习危险的大开眼界的项目。我专注于修正在信用评分和贷款审批领域种族偏见的 ML 预测因子。但我意识到，还有一个更大的系统性问题不容忽视。

这个问题从 ProPublica 的[风险评估案例研究](https://www.propublica.org/article/how-we-analyzed-the-compas-recidivism-algorithm)中可以看得最清楚。在这起事件中，机器学习被用来预测罪犯成为惯犯的可能性。这种可能性被汇总成一个叫做风险评估的分数。这些评估在整个监禁过程中都要用到，有时甚至在法官做出最终决定之前交给他们。

这种情况的问题是，这些预测器的训练数据充满了冗余编码的*偏差和歧视。事实证明，与白人罪犯相比，更多的非裔美国罪犯被给予了不公平的高评价。这个机器学习预测器无意中延续了种族主义的犯罪不公。*

想象一下，如果创造这些预测器的数据科学家花时间注意到这个问题。尽管高性能的预测器仍然可以延续几代人的歧视，但重点是高度面向 ML 模型的优化和准确性。

它只是将问题从*“我如何优化这个模型的性能？”* To，*“我为什么要创建这个模型？”如果更多的思考放在工作的目的上，预测者的社会分支可能不会那么容易被忽略。*

## 数据收集和隐私

随着 GDPR 的全面展开，数据隐私已经成为科技行业内一个敏感的话题。很明显，在缺乏隐私的情况下，处理某人的数据可能会导致意想不到的后果。从不准确的数据中解读出的相关性和趋势会间接地将人们归类到一个他们无法抹去的角色中。[参见:[被遗忘的权利](https://gdpr-info.eu/issues/right-to-be-forgotten/)

数据隐私官员开始质疑数据处理和删除的道德含义，但还没有围绕收集敏感数据的讨论摆在首位。在资本主义社会，公司利益相关者对收入的兴趣显然高于隐私。在美国，GDPR 变成了合规游戏，因为科技巨头害怕被罚款。历史上，美国公民的数据透明度一直是一种公关噱头，伴随着 GDPR 对欧洲公民的要求。

首席信息官们开始担心加州隐私法案，因为他们不能再依靠美国客户数据来弥补欧洲基于隐私开发的收入损失。缺少的对话包括在服从和原则之间的动机转换。

不要问，“*我们如何在遵守 GDPR 法案的情况下停止收入损失？”*我们必须问，“*为什么我们的收入首先依赖于敏感数据的收集？”*

忽略前者而问后者会让高管们暂停下来。或许为基于广告的收入模式收集敏感数据并不是唯一的解决方案。也许这将激发人们考虑一种根本不需要收集数据的基于订阅的替代方案。

## 人工智能

人工智能很容易成为伦理辩论的目标，原因很简单，因为机器人让人类感到恐惧，因为它们只不过是想象中的虚构物。随着神经网络研究的飞速发展，恐怖谷正成为一个旅游目的地。抛开对终结者的恐惧不谈，我们感兴趣的话题在于，劳动力自动化不再是过去的担忧。机器人正在取代我们的工作，创造了一个新的失业世界。

当我听我的同事们讨论最新的机器人厨师或自动驾驶汽车时，我看到他们的眼睛充满了可能性。创造这些突破性生物的工程师们对一项技术创新如此兴奋，他们没有意识到他们可能正在编码自己的未来。现在, [Miso Robotics](https://misorobotics.com/) 已经创造了一个汉堡翻转助手，但是在几年内，这些汉堡可以很容易地成为建筑设计或代码行。

人类认为，机器人永远无法取代护士的温柔触摸，或音乐家的创造性灵魂。可惜，[他们已经有了](https://www.youtube.com/watch?v=7Pq-S557XQU)。思维模拟理论的研究目前正在进行中，这是对机器人同理心的推动。帮助机器人更好地理解人类的开创性尝试。这是一个误用工程思维的完美例子。

# 我问过为什么，我喜欢这个答案…这样够了吗？

我们再来仔细看看[模拟心智理论](https://www.scientificamerican.com/article/how-to-make-a-robot-use-theory-of-mind/)。这一理论如果正确地付诸实践，将使机器人能够预测周围人的预期需求和行动。配备 SToM 的人工智能理论上能够更好地理解人类，在预测帮助他们的最佳方式之前模拟他们可能的行动。[这篇论文](https://arxiv.org/pdf/1802.08129.pdf)解释了机器人如何能够解释他们决策背后的推理。随着神经网络变得越来越复杂，这个概念变得越来越困难。

这项研究似乎在伦理上是积极的。赋予机器人与人类共情的能力，并为它们的决策提供透明度，这听起来好得令人难以置信！这个理论让机器人理解我们，更重要的是，让**我们理解它们**。

现在想想移情决策对成功至关重要的工作。自动化从来不是这些领域的担忧，因为机器人无法站在人类的角度思考问题。随着心理模拟理论的出现，[移情就业](https://www.forbes.com/sites/mohanbirsawhney/2018/03/10/as-robots-threaten-more-jobs-human-skills-will-save-us/#5eb7a4ad3fce)不再是人类的保证。

很明显，这项研究试图解决人工智能的伦理问题。但是工程思维占了上风，深深埋藏的负面影响在不久的将来很容易浮出水面。Moor 甚至认为，劳动自动化可能会重新定义我们对工作的整个概念。

> “传统工作可能不再被定义为通常发生在特定时间或特定地点的事情。对我们来说，工作与其说是做一件工作，不如说是指导计算机去做一件工作。随着工作观念开始改变，与旧观念相关的价值观将不得不被重新审视。”

# 结论

21 世纪的技术扩张时代已经被定义为**如何**。人类已经突破了自动化、预测和机器人的界限。我们已经创造了一个世界，在这个世界里，创业公司只有在追求收益而非道德的情况下才能生存。工程师们甚至在意识到他们自己工作的含义之前，就被分配了工作并创造了最佳解决方案。

早就应该进行系统性变革了。袖手旁观只会使我们已经陷入的问题永久化。作为工程师，是时候停止询问我们如何创造了。说，*“我不做，别人会”*是借口。相反，花点时间停下来想一想。真的觉得。然后问问你的老板、同事，最重要的是，问问你自己:

# **为什么？**