# 垃圾邮件过滤中的文本分类应用(上)

> 原文：<https://towardsdatascience.com/applied-text-classification-on-email-spam-filtering-part-1-1861e1a83246?source=collection_archive---------1----------------------->

![](img/8491e71e30c17547c50df65f54169ab6.png)

Email-Spam Filtering

原创文章发表在[我的网站](http://www.sarahmestiri.com/index.php/2017/09/01/applied-text-classification-on-email-spam-filtering-part-1/)。

从上个月开始，我开始在华盛顿大学提供的在线[机器学习专业](https://www.coursera.org/specializations/machine-learning)上工作。第一个课程是关于 *ML 基础*，第二个是关于*线性回归*，第三个课程是关于*分类*。我喜欢这些课程的各个方面，因为它们从头开始教授 ML 算法的实现。当我决定更深入地探索这个领域时，这就是我的目标。但是，老实说，我觉得有一个差距，因为许多问题都没有答案。然后，在阅读了关于如何开始机器学习的内容后，我发现大多数文章都强调了将课程与实践项目相结合的重要性，以便学以致用..而且是那么真实！你只要试着把两者结合起来，你很快就会注意到不同之处！

这是我的第一次实习申请！:D 我选择尝试 ***垃圾邮件过滤*** ，因为这是应用分类中一个非常常见的话题。这很容易理解，因为我们每天都在经历电子邮件中的垃圾邮件过滤。

我遵循了一个简单的入门教程:[电子邮件垃圾邮件过滤:使用 Scikit-learn 的 Python 实现](https://appliedmachinelearning.wordpress.com/2017/01/23/email-spam-filter-python-scikit-learn/)。做完不久，我的大脑开始分析步骤，一堆问题轰炸我的脑袋！

为什么“垃圾邮件和非垃圾邮件数量相等”？什么是词干？除了删除停用词和词条化之外，还有其他清理数据的方法吗？训练集和测试集之间的划分是如何完成的？为什么没有验证集？为什么特别使用朴素贝叶斯分类器和 SVM(支持向量机)？是什么让朴素贝叶斯在文档分类问题上如此受欢迎？等等..

> 扮成威廉。巴勒斯说:“如果你学会放松并等待答案，你的大脑会回答大多数问题。”

我吸了口气，开始一个问题一个问题地回答，有时在网上搜索，试验一些代码的变化，并分析输出。我很高兴地分享结果:

# 1)我们需要的数据

-我们已经看到了多少封电子邮件(将在训练测试集中使用)
-每个标签中有多少封电子邮件(用于检测是否存在不平衡数据)
-一个单词与每个标签关联的频率(用于计算电子邮件是垃圾邮件或垃圾邮件(0 类或 1 类)的概率)

# 2)清理数据

**为什么要清洗单词表？**为了减少得到错误结果的可能性，清理数据是必不可少的，因为一些单词对分类没有影响(它们既不能与垃圾邮件类相关联，也不能与垃圾邮件类相关联),并且有一些单词可以被标准化，以便将意思相同的单词分组并减少冗余。通过对训练数据的质量采取行动，我们可以改变分类器的所谓的[准确度](https://en.wikipedia.org/wiki/Accuracy_and_precision)。因此，移除*停用词、词干*和*词条化*有助于改善机器学习算法的结果。

# 3)朴素贝叶斯

**为什么使用朴素贝叶斯？**朴素贝叶斯具有高效的学习和预测能力，它经常被用来与更复杂的方法进行比较，因为它快速且高度可伸缩(适用于高维数据),正如吴恩达建议的那样，在处理 ML 问题时，从尝试简单快速的算法开始，然后从该点扩展。

**朴素贝叶斯如何简单易行？**朴素贝叶斯基于“贝叶斯”定理，之所以称之为“朴素”，是因为它假设在给定类的情况下*要素是相互独立的*(要素之间没有/几乎没有相关性)，这是不现实的。因此，朴素贝叶斯可以学习单个特征的重要性，但不能确定特征之间的关系。此外，与其他方法相比，朴素贝叶斯的训练时间要短得多，并且不需要太多的训练数据。

**为什么是多项朴素贝叶斯？其他模型比如高斯朴素贝叶斯或者伯努利朴素贝叶斯呢？**

嗯， ***多项式 NB*** 考虑的是特征(在我们的例子中是单词)的 ***频率* *计数*** (出现次数)*而**伯努利 NB** 只关心文档中某个特定特征(单词)的**存在与否**。后者适用于*二进制值*(伯努利，布尔)的特性。鉴于使用**高斯 NB** ，特征是*实值*或*连续*并且它们的分布是高斯的，Iris Flower 数据集是具有连续特征的例子。*

# 4)支持向量机(SVM)

为什么使用 SVM？我没有找到具体的原因，但据我所知，SVM 可以提供高精度的结果，因为它使用了优化程序。SVM 通过搜索最优的分离超平面(**最优超平面**)和**最大化分离类别(在我们的例子中是垃圾邮件和火腿)的间隔**来构建分类器。因此，当维数大于样本数时，SVM 具有一般稳健性和有效性的优点。

与朴素贝叶斯不同，SVM 是一种非概率算法。

【LinearSVC 和 SVC (Scikit-learn)有什么区别？区别在于它们不接受相同的参数。例如，LinearSVC 不接受内核参数，因为它被认为是线性的。SVC 支持更多的参数(C，γ，..)因为它保存了所有可能的[核函数](http://scikit-learn.org/stable/modules/svm.html#svm-kernels)(线性、多项式、rbf 或径向基函数、sigmoid)。

**如何调整 SVM 参数？**调整 SVM 参数提高了算法的性能。其中一些影响更大:

**-内核**:内核就像一个相似度函数。这是一种在可能的高维特征空间中计算两个向量的点积的方法，使用基于一些提供的约束的数据转换到更复杂的空间。核函数有时被称为“广义点积”。

**-Gamma:**“RBF”、“poly”和“sigmoid”的核系数。更高的伽马值将试图按照训练数据集精确拟合，即泛化误差，并导致过拟合问题。

**-C**:(3.15)中的系数 *C* 是一个允许在训练误差和模型复杂度之间进行权衡的参数。小的值 *C* 将增加训练错误的数量，而大的值 *C* 将导致类似于硬边际 SVM 的行为。”约阿希姆(2002)，第 40 页

# 5)分析不同情况下的输出

如果我改变字典的大小会怎样？

改变字典大小意味着改变特征(单词)的数量。因此，我想探索拥有更多功能的影响，以及基于[混淆矩阵](http://scikit-learn.org/stable/modules/generated/sklearn.metrics.confusion_matrix.html)结果的好结果的限制。

我在 size= {3000，5000，6000，7000}上进行测试，发现在 size = 7000 时，SVM 分类开始略有下降(错误识别),而朴素贝叶斯尽管在大小上有所变化，但仍提供了相同的结果。

我认为，在这一点上，可能目标类开始重叠或训练数据过度拟合。我还不确定对这个结果的解释。

如果我尝试高斯和伯努利呢？

显然，引入伯努利不会有所帮助，因为正如我上面解释的，它在我们的情况下没有提供足够的信息，我们需要的是字数，而不是它的存在/不存在。

多项式 NB:
[[129 1]
【9 121]]
高斯 NB:
[[129 1]
【11 119]]
伯努利 NB:
[[130 0 0]
【53 77】]

如我们所见，多项式 NB 优于高斯 NB 和伯努利 NB。

**如果我在 SVM 上尝试**[**grid search**](http://scikit-learn.org/stable/modules/grid_search.html)**来调整 SVM 参数会怎么样？**
Params grid search:param _ grid = { ' C ':[0.1，1，10，100，1000]，' gamma':[1，0.1，0.01，0.001，0.0001]}
找到最佳参数:Gamma:0.0001；列车员:100 元

线性 SVM:
[[126 4]
【5 125】]
多项式 NB:
[[129 1]
【9 121】]
SVM:
[[129 1]
【62 68]]
grid search on SVM:
[[126 4]
【2 128】]

SVM 使用 GridSearch 调整参数，可以获得更好的结果。

## 结论

这就是我使用垃圾邮件过滤软件的第一步！如果你正在考虑开始一个文本分类项目，我希望它对你有所帮助！我会继续分享各种思考和实验。下一次，我将探索更多的训练数据和特征的改进/改变。

这个项目也在 Github 上。

**有用的资源:**

[1] [朴素贝叶斯和文本分类](http://sebastianraschka.com/Articles/2014_naive_bayes_1.html)。
【2】[朴素贝叶斯举例](http://dataaspirant.com/2017/02/06/naive-bayes-classifier-machine-learning/)。
【3】吴恩达解释朴素贝叶斯[视频 1](https://www.youtube.com/watch?v=z5UQyCESW64) 和[视频 2](https://www.youtube.com/watch?v=NFd0ZQk5bR4)
【4】[请像我 5 岁一样解释 SVM](https://www.reddit.com/r/MachineLearning/comments/15zrpp/please_explain_support_vector_machines_svm_like_i/)。
【5】[从实例理解支持向量机](https://www.analyticsvidhya.com/blog/2015/10/understaing-support-vector-machine-example-code/)。

告诉我你在文本分类方面的经验？你用它做什么？你建议用什么方法？有哪些挑战？