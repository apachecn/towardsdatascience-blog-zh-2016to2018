# 我如何解决数据科学问题

> 原文：<https://towardsdatascience.com/how-i-approach-a-data-science-problem-714bfbc5c68c?source=collection_archive---------0----------------------->

对于刚刚进入数据科学领域的人来说，事情肯定会变得令人生畏。数据科学有太多的技术和程序会让任何人困惑。

亲身经历过这个问题，并且看到我的大多数同学都在为这个问题而挣扎，我决定写这篇博客，讲述我通常是如何处理数据科学问题的。

**第一步:将你的业务问题陈述转化为技术问题陈述**

与任何其他软件问题类似，数据科学旨在解决商业问题。大多数时候，业务问题陈述是模糊的，可以有多种解释。这主要是因为我们通常在语言中使用不能直接翻译成机器可读代码的定性词汇。

比方说，我们需要开发一个解决方案来降低城市的犯罪率。术语“减少”可以解释为:

*   降低高犯罪率地区的犯罪率
*   降低最常见犯罪类型的犯罪率

与定义问题的客户或业务团队一起确定正确的解释是一个好的实践。

**第二步:决定监督学习技术**

几乎每个数据科学问题的最终目标通常是分类或回归。确定问题的监督技术将有助于你更清晰地理解商业陈述。

让我们来看看降低犯罪率的问题。虽然降低犯罪率的问题更多的是一个政策决定，但根据上面的选择，我们必须决定是否需要进行分类或回归。

*   如果我们需要降低高犯罪率地区的犯罪率，我们就需要确定一个地区的犯罪率。这是一个回归问题。
*   如果我们需要降低最常见犯罪类型的犯罪率，我们就需要确定一个地区最常见的犯罪类型。这是一个分类问题。

再次强调，与定义问题需求的客户或业务团队进行交流，澄清确切的需求，这是一个很好的实践。

**第三步:文献调查**

文献调查是解决任何问题的最重要的一步(也是最容易被忽视的一步)。如果你阅读任何关于数据科学组成部分的文章，你会发现计算机科学、统计/数学和领域知识。因为对一个人来说，在所有可能的领域都拥有专业知识是很不人道的，所以文献调查通常有助于弥补专业知识不足的差距。

在浏览了与某个问题相关的现有文献后，我通常会尝试提出一组假设，这些假设可以形成我的潜在特征集。浏览现有文献有助于你理解该领域中的现有证据，作为在你的问题中采取正确方向的指南。它还有助于解释从预测模型中获得的结果。

回到我们降低犯罪率的问题上来，如果你想预测一个地区的犯罪率，你会考虑人口统计、邻近地区、执法规则等常识因素。文献调查将帮助你考虑额外的变量，如气候、交通方式、离婚率等。

**第四步:数据清理**

如果您与任何在数据科学方面花了一些时间的人交谈，他们总是说他们的大部分时间都花在清理数据上。真实世界的数据总是杂乱无章的。以下是大多数数据集中的一些常见差异，以及如何清理它们的一些技巧:

*   *缺失值*

缺失值是数据集中的空白值。这可能是由于各种原因，如价值未知、未记录、保密等。由于缺少某个值的原因不清楚，因此很难猜测该值。

您可以尝试不同的技术来估算缺失值，从简单的方法开始，如列平均值、中位数等。以及复杂的方法，如使用机器学习模型来估计缺失值。

*   *重复记录*

重复记录面临的挑战是识别重复的记录。在合并来自多个来源的数据时，经常会出现重复记录。这也可能是由于人为错误造成的。要识别重复项，您可以将数值近似到特定的小数位，对于文本值，模糊匹配可能是一个好的开始。识别重复项有助于数据工程团队改进数据收集以防止此类错误。

*   *不正确的值*

不正确的值大多是由于人为错误。例如，如果有一个名为“年龄”的字段，其值为 500，这显然是错误的。拥有数据的领域知识将有助于识别这些值。识别数值列的错误值的一个好方法是手动查看平均值的 3 个标准偏差以上的值，以检查其正确性。

**第五步:特征工程**

特征工程是任何数据科学问题中最重要的步骤之一。一组好的要素可能会使简单的模型适用于您的数据。如果功能不够好，你可能需要复杂的模型。特征工程主要包括:

*   *移除多余特征*

如果某个要素对输出值贡献不大，或者是其他要素的函数，则可以移除该要素。有各种各样的度量标准，如 AIC 和 BIC 来识别冗余特征。有内置的包来执行向前选择、向后选择等操作。删除多余的功能。

*   *转换特征*

某个要素可能与输出列存在非线性关系。虽然复杂模型可以用足够的数据捕捉到这一点，但简单模型可能无法捕捉到这一点。我通常试图想象每列的不同函数，如对数、倒数、二次、三次等。并选择看起来最接近正常曲线的变换。

**第六步:数据修改**

一旦清理了数据，在应用机器学习模型之前，可能需要进行一些修改。最常见的修改之一是将每一列缩放到相同的范围，以便给予所有列相同的权重。其他一些所需的修改可能是数据特定的，例如，如果输出列是倾斜的，您可能需要上采样或下采样。

步骤 7 到 9 是迭代的。

**第七步:建模**

一旦我准备好了数据，我通常会从尝试所有标准的机器学习模型开始。如果是分类问题，一个好的开始将是[逻辑回归](https://en.wikipedia.org/wiki/Logistic_regression)、[朴素贝叶斯](https://en.wikipedia.org/wiki/Naive_Bayes_classifier)、[k-最近邻](https://en.wikipedia.org/wiki/K-nearest_neighbors_algorithm)、[决策树](https://en.wikipedia.org/wiki/Decision_tree)等。如果是回归问题，可以试试[线性回归](https://en.wikipedia.org/wiki/Linear_regression)、[回归树](https://en.wikipedia.org/wiki/Decision_tree)等。从简单模型开始的原因是简单模型需要修改的参数较少。如果我们从一个复杂的模型开始，像[神经网络](https://en.wikipedia.org/wiki/Artificial_neural_network)或[支持向量机](https://en.wikipedia.org/wiki/Support_vector_machine)，有太多的参数可以改变，穷尽所有选项可能会很费时间。

每个机器学习模型都对数据做出一些潜在的假设。例如，线性回归/逻辑回归假设数据来自输入参数的线性组合。朴素贝叶斯假设输入参数相互独立。了解这些假设可以帮助您判断不同模型的结果。将实际值与预测值可视化以了解这些差异通常很有帮助。

**第八步:型号对比**

评估不同机器学习模型的最标准的技术之一是通过[交叉验证](https://en.wikipedia.org/wiki/Cross-validation_(statistics))的过程。我通常选择 10 重交叉验证，但是你可以根据数据的大小选择正确的交叉验证分割。交叉验证基本上得出一个模型的平均性能。这有助于避免选择针对数据表现良好的模型，或者换句话说，避免[过度拟合](https://en.wikipedia.org/wiki/Overfitting)。在交叉验证之前随机化数据通常是一个好的做法。

比较不同模型性能的一个好方法是 ROC 曲线。ROC 曲线有助于您可视化不同模型在不同阈值下的性能。虽然 ROC 曲线给出了模型性能的整体感觉，但根据业务决策，您必须选择性能指标，如[准确性](https://en.wikipedia.org/wiki/Accuracy_and_precision)、[真阳性率、假阳性率](https://en.wikipedia.org/wiki/Sensitivity_and_specificity)、[F1-得分](https://en.wikipedia.org/wiki/F1_score)等。

**第九步:错误分析**

此时，你已经尝试了一堆机器学习模型，得到了结果。这是一个很好的利用时间的方法，不仅仅是查看结果，如准确性或真正的阳性率，而是查看在一些模型中失败的数据点集合。这将帮助您更好地理解数据，并且比尝试所有可能的模型组合更快地改进模型。这是尝试集合模型的时候了，比如[随机森林](https://en.wikipedia.org/wiki/Random_forest)、[梯度推进](https://en.wikipedia.org/wiki/Gradient_boosting)或者你自己的元模型【比如决策树+逻辑回归】。集合模型几乎总是保证比任何标准模型表现更好。

**第十步:改进你的最佳模特**

一旦我有了最好的模型，我通常会根据参数的数量来绘制训练与测试的准确性(或正确的度量标准)。通常，根据数据点的数量来检查训练和测试的准确性是很容易的。基本上这个图会告诉你你的模型是过拟合还是欠拟合。这篇文章[检测过拟合与欠拟合](http://digitheadslabnotebook.blogspot.com/2011/12/practical-advice-for-applying-machine.html)清楚地解释了这个概念。

了解您的模型是过度拟合还是欠拟合将告诉您如何继续下一步。如果模型过度拟合，您可以考虑收集更多数据。如果模型不合适，您可以考虑使模型更复杂。[例如，将高阶项添加到线性/逻辑回归中]

**步骤 11:部署模型**

一旦您有了最终的模型，您会希望模型被部署，以便它自动预测新数据点的输出，而无需重新训练。你可以为简单的模型推导出一个公式，比如线性回归，逻辑回归，决策树等等。对于像 SVM、神经网络、随机森林等复杂的模型来说，就不是那么简单了。我不太熟悉其他语言，但 Python 有一个名为 [pickle](http://scikit-learn.org/stable/modules/model_persistence.html) 的库，它允许你保存模型，并使用它来预测新数据的输出。

**第 12 步:添加反馈**

通常，任何数据科学问题的数据都是历史数据。虽然这可能在一定程度上类似于当前的数据，但它可能无法捕捉当前的趋势或变化。例如，如果您使用人口作为输入参数，虽然 2015-2016 年的人口可能会略有变化，但如果您在 5 年后使用该模型，它可能会给出不正确的结果。

处理这个问题的一个方法是不断用额外的数据重新训练你的模型。这可能是一个很好的选择，但是重新训练一个模型可能很耗时。此外，如果您有大量数据流入的应用程序，这可能需要定期进行。另一个更好的选择是使用主动学习。主动学习基本上试图使用实时数据作为反馈，并自动更新模型。最常见的方法是[批量梯度下降](http://sebastianruder.com/optimizing-gradient-descent/index.html#batchgradientdescent)和[随机梯度下降](https://en.wikipedia.org/wiki/Stochastic_gradient_descent)。基于应用程序使用正确的方法可能是合适的。

**结束语**

数据科学的领域真的很广阔。人们一生都在研究上面讨论的各个主题。作为一名数据科学家，比起研究单个子主题，你更需要解决业务问题。此外，您必须向可能没有足够技术知识的业务团队解释技术过程和结果。因此，虽然您可能不需要对每种技术都有非常深入的了解，但是您需要有足够的清晰度来抽象技术过程和结果，并用业务术语来解释它。

LinkedIn 上的链接:[https://www . LinkedIn . com/pulse/how-I-approach-data-science-problem-Ganesh-n-Prasad？trk=prof-post](https://www.linkedin.com/pulse/how-i-approach-data-science-problem-ganesh-n-prasad?trk=prof-post)

*如果您有任何建议，请留言或通过*[*ganeshn1411@gmail.com*](mailto:ganeshn1411@gmail.com)联系我