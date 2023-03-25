# 欠采样算法的缺点:CCMUT 和 E-CCMUT

> 原文：<https://towardsdatascience.com/shortcoming-of-under-sampling-algorithms-ccmut-and-e-ccmut-10fd24f54852?source=collection_archive---------17----------------------->

*什么、为什么、可能的解决方案和最终效用*

![](img/928190493afc00ca080da1031ad7e82a.png)

在我以前的一篇文章中， [*欠采样:不平衡数据的性能助推器*](/under-sampling-a-performance-booster-on-imbalanced-data-a79ff1559fab) :

[](/under-sampling-a-performance-booster-on-imbalanced-data-a79ff1559fab) [## 欠采样:不平衡数据的性能助推器

### 在某些情况下，用于开发机器学习/深度学习模型的数据集通常是…

towardsdatascience.com](/under-sampling-a-performance-booster-on-imbalanced-data-a79ff1559fab) 

我已经在成人人口普查数据上应用了基于聚类质心的多数欠采样技术(CCMUT) ，并证明了模型性能改进 w.r.t 最新模型，“*成人人口普查收入水平预测的统计方法*【1】。但是使用 CCMUT 开发的改进模型存在一个潜在的缺点。

**CCMUT/E-CCMUT 的缺点:**

所开发的模型给出了 90.78%的验证准确度，高于最先进的 88.16%。但是，在模型开发中，CCMUT 已经删除或欠采样了 68%的标签为 0 的实例(多数类实例)。因此，这 68%的实例既不包括在模型的训练集中，也不包括在模型的验证集中。因此，可以创建另一个数据集，**测试集**保存这些欠采样的实例，所有实例的标签都是 0。之后，在如此创建的**测试集**上测试经过训练和验证的模型。

令我十分惊讶的是，16810 个实例中只有 3 个被正确分类。这对开发出来的车型是一个很大的潜在威胁。而 Chakrabarty 和 Biswas [1]的最新模型在相同的 16810 个实例中正确地给出了**16000 个**，，从而保留了模型的通用性。现在的问题是，如果通过引入欠采样开发的模型导致了这样的错误，那么欠采样带来的性能改善究竟是什么？

是的，欠采样确实提高了性能。通过欠采样创建的模型的验证集有 3，151 个混合标签(0 和 1)的实例。在该验证集上，该模型从 3，151 个实例中正确分类了 **2，861 个(90.78%的准确率)，而当在这 3，151 个实例上测试时，最新技术从 3，151 个实例**中给出了 **2，589 个正确预测(82.16%的准确率)。因此，在这方面，性能肯定有所提升。但与此同时，还有一个表现不佳的典型案例。**

**退税原因:**

这种缺点的主要原因在于 CCMUT 和 E-CCMUT 的成帧。这些算法是使用特征空间几何和聚类理论构建的。因此，欠采样实例实际上并不重要。它们是离群值，很有可能处于任何机器学习算法绘制的决策边界的错误一侧的**边界**或**。换句话说，这些实例可以被解释为难以正确分类的实例。所以，我只是倒回 CCMUT/E-CCMUT 算法工作声明，并进行修正:**

*离聚类中心最远的数据点最难分类/预测，而离聚类中心最近的实例最容易分类/预测***。**

**因此，在更容易分类/预测的实例中，CCMUT/E-CCMUT 表现得更好，从而提高了性能。但是当在难以分类的实例上测试时，CCMUT/E-CCMUT 表现不佳。这是因为在欠采样的情况下，训练集没有获得足够的难以分类/预测的实例，并且仅获得更接近多数类的聚类质心的实例，这些实例更容易分类/预测。当在这样的实例(困难的实例)上进行测试时，模型会完全混乱，并在大多数情况下预测错误的标签。**

****消除缺点的方法:****

1.  **欠采样应该以很小的百分比进行(1-10%)。然后，模型开始学习一些难以分类的实例。但是，如果这不能在它自己的验证集中获得任何性能改进，它可能会失败。**
2.  **欠采样之后是随机选择:这里，在欠采样少于 50%之后，在欠采样实例中，可以进行数据点的随机选择。那么这些随机选择的数据点可以包含在模型的训练集中。另外，未被随机选择的剩余欠采样实例可以形成**测试集。****

**我完全出于直觉设计了这些方法，这些方法背后没有数学依据。这些方法也不能总是在每个数据集上获得性能提升。**

****CCMUT/E-CCMUT 的效用在哪里？****

**排序程序(**最易分类— — >难分类**)在识别样本性质中起着非常重要的作用。因此，CCMUT/E-CCMUT 可用于**数据集细化或数据清理**。如果数据集已准备好，但它包含噪声实例，这对机器学习模型开发可能是一种威胁。因此，数据集创建者/编译器( [UCI](https://archive.ics.uci.edu/ml/index.php) / [Kaggle](https://www.kaggle.com/) )可以使用 CCMUT/E-CCMUT 对这类实例进行欠采样，从而有利于预测分析。**

****撰写本文的动机****

**我要感谢 [Victor Deplasse](https://medium.com/u/bed655a6610a?source=post_page-----10fd24f54852--------------------------------) 作为我这篇文章背后的动力，他通读了我的文章“ [*欠采样:不平衡数据的性能助推器*](/under-sampling-a-performance-booster-on-imbalanced-data-a79ff1559fab) ”，并质疑欠采样算法 CCMUT 和 E-CCMUT 的一致性，因为我确信它们是完美的性能助推器。我还要感谢 [TDS 团队](https://medium.com/u/7e12c71dfa81?source=post_page-----10fd24f54852--------------------------------)给了我一个平台，让我以文章的形式交流我的想法，并得到我的读者的认可。**

**非常感谢[数据科学](https://towardsdatascience.com/)！！！**

****参考文献****

**[1]查克拉巴蒂和比斯瓦斯，2018 年。成人人口普查收入水平预测的统计方法。 *arXiv 预印本 arXiv:1810.10076* 。**