# 构建你的机器学习项目的 22 个智慧金块

> 原文：<https://towardsdatascience.com/structuring-your-machine-learning-project-course-summary-in-1-picture-and-22-nuggets-of-wisdom-95b051a6c9dd?source=collection_archive---------6----------------------->

![](img/e919dca3d01dee6b73979b3b221032cf.png)

## 完成一个机器学习项目很难。这个总结可以帮助你构建你的机器学习项目。

他的帖子研究了 Coursera 深度学习专业化的第三个课程。它给出了关键概念的图形概述，并用一句话概括了每堂课。

为什么是这个总结？我确信所学到的经验教训对所有机器学习从业者都是有帮助的。在阅读完讲座后，我相信读者会从紧凑的讲座摘要和对材料的概述中受益。如果这个帖子激发了你的好奇心，请鼓励[注册](https://www.coursera.org/learn/machine-learning-projects)并亲自探索这个课程。

所以，事不宜迟，让我们进入知识的世界吧。

# 快速迭代

在其核心，每个机器学习项目都是关于有一个想法，实施，然后评估它。你越早重复这些步骤，你就能越早达到你的目标。这种结构与 Eric Ries 的[精益创业精神](https://www.youtube.com/watch?v=sobxOzRjAGg)产生了共鸣，即“建立、衡量和学习”，这在全世界的创业公司、组织和企业中非常流行。

![](img/7e52ee81cb5f82765cc5c07e04539d66.png)

The tools to iterate quickly over new ML ideas

在这个过程的每一点上，你都必须做出重要的决定。你应该收集更多的数据吗？你应该减少偏差或方差吗？你应该如何比较不同的算法？该课程提供了选择最有效的前进步骤的建议。上图总结了这个关键概念，以及对每个迭代步骤的建议。

接下来，您可以在第三个 Coursera 深度学习专业化课程中找到每个讲座视频的一句话摘要。瞧，你的 22 条智慧。

# 讲座摘要

1.  为什么选择 ML 策略——机器学习策略对于快速迭代想法和高效达成项目结果非常有用。
2.  正交化-指的是挑选参数进行调整的概念，这些参数仅调整机器学习模型的一个结果，例如，正则化是减少方差的旋钮。
3.  单个数字评估指标—选择一个评估指标，例如 f1 分数，立即判断多个模型的性能。
4.  满足和优化指标-机器学习模型通常有一个要优化的指标，例如实现最大的准确性，以及应该保持的某些约束，例如在不到 1 秒的时间内计算预测或将模型放入本地内存。在这种情况下，准确性是优化指标，预测时间和内存使用是令人满意的指标。
5.  培训/开发/测试集分布——确保开发和测试集来自同一个分布，并且它们准确地代表团队试图优化的目标。
6.  开发和测试集的大小——考虑到您的训练集以百万计，对训练集使用尽可能多的数据，对开发和测试集使用 1%/1%的数据。
7.  何时改变开发/测试集和度量——如果您发现您的评估度量的等级不再准确地反映您的模型的性能，请考虑重新陈述优化度量，例如，通过添加加权项来严重惩罚您的分类器对真正重要的示例的错误分类。
8.  为什么是人类水平的性能？—贝叶斯误差是分类器可以实现的最佳性能，根据定义，它优于人类水平的性能。贝叶斯和人为误差是评估训练数据是否存在偏差的重要指标。
9.  可避免的偏差——描述训练集错误和人类水平的表现之间的差距。
10.  了解人的水平性能-如果一组专家能够达到 0.7%的错误率，而一个人达到 1%的错误率，则选择 0.7%作为最佳人的水平性能，选择< 0.7%的值作为贝叶斯误差来测试模型性能。
11.  超越人类水平的性能-如果你的算法超越人类水平的性能，那么判断可避免的偏差就变得非常困难，因为你通常不知道贝叶斯误差有多小。
12.  提高模型性能-评估贝叶斯误差和训练集误差之间的差异，以估计可避免的偏差水平。通过比较训练和偏差设定误差来估计方差。尝试不同的技巧来对抗任何形式的错误。
13.  进行错误分析——分析 100 个错误分类的例子，并根据错误分类的原因对它们进行分组。为了改进您的模型，训练您的网络来消除它错误分类某一类型输入的原因可能是有意义的，例如，向它提供更多模糊的图片。
14.  清理错误标记的数据——神经网络在处理随机错误分类方面相当稳定，如果您消除了开发集中的错误分类，也消除了测试集中的错误分类。
15.  快速构建你的第一个系统，然后迭代——快速原型化第一个版本的分类器，然后按照战略方针迭代改进它。
16.  在不同的发行版上进行训练和测试-如果训练集中的数据来自混合数据源，请使用您想要优化的数据创建开发和测试集，例如，如果您想要对手机中的运动鞋图像进行分类，请使用仅包含手机中的运动鞋照片的开发和测试集，但可以随意使用增强的运动鞋 web 图像来训练网络。
17.  数据不匹配的偏差和差异—当您的开发和测试集来自不同的数据分布时，创建一个与训练集具有相同数据分布的训练开发集。这一步有助于您检查是否存在方差、偏差或数据不匹配问题。
18.  解决数据不匹配的问题——如果您有数据不匹配的问题，请执行手动错误分析，并了解训练集和开发/测试集之间的差异。请注意创建人工训练数据，因为可能会出现这样的情况:您只合成了所有可用噪声的一小部分。
19.  迁移学习-如果您有另一个分类器可以受益的较低级别的特征，请使用迁移学习，并切断现有神经网络的最后一层，然后针对新任务训练网络。
20.  多任务学习-使用单个神经网络来检测图像中的多个类别，例如自动驾驶汽车的交通灯和行人。同样，当神经网络识别对多种分类任务有帮助的较低级别的特征时，并且如果您具有平均分布的类数据，这也是有用的。
21.  什么是端到端深度学习——不是使用许多不同的步骤和手动特征工程来生成预测，而是使用一个神经网络来找出底层模式。
22.  是否使用端到端深度学习——端到端深度学习有一些优势，比如让网络自己找出重要的特征，也有一些劣势，比如需要大量的数据，所以它的使用真的必须根据你正在解决的任务或功能的复杂程度来逐个判断。

感谢你阅读这篇文章，我希望它能帮助你回顾你的课程经历。所有功劳属于 [deeplearning.ai](https://www.deeplearning.ai/) ，Coursera 和吴恩达。如果我错过了一个重要的话题，请随时在下面的评论区添加。

如果你对 [Coursera 深度学习专业化](https://www.coursera.org/specializations/deep-learning)和机器学习的更多更新感兴趣，请查看我之前的[两篇](https://medium.com/machine-learning-world/netflix-or-coursera-how-to-finish-andrew-ngs-1st-deep-learning-course-in-7-days-6fa293ee83d8) [帖子](/https-medium-com-janzawadzki-applying-andrew-ngs-1st-deep-neural-network-on-the-titanic-survival-data-set-b77edbc83816)，在 medium 上关注我，如果你有任何其他问题，请随时在 [LinkedIn](http://www.linkedin.com/in/jan-zawadzki) 上给我发消息。