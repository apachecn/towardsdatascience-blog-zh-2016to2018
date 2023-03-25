# 贝叶斯线性回归简介

> 原文：<https://towardsdatascience.com/introduction-to-bayesian-linear-regression-e66e60791ea7?source=collection_archive---------0----------------------->

![](img/44bc1413c37a18478d547913a940fdfd.png)

## 解释线性建模的贝叶斯方法

贝叶斯与频率主义者的辩论是我觉得观看比参与更有趣的学术辩论之一。我认为学习两种[统计推断](https://en.wikipedia.org/wiki/Statistical_inference)的方法并在适当的时候应用它们会更有成效，而不是狂热地跳到某一方。按照这种思路，最近，我一直在努力学习和应用贝叶斯推理方法，以补充我的研究生课程中涵盖的频率统计。

在应用贝叶斯推理中，我首先关注的领域之一是贝叶斯线性建模。学习过程中最重要的部分可能只是向他人解释一个想法，这篇文章是我试图介绍贝叶斯线性回归的概念。我们将简要回顾线性回归的 frequentist 方法，介绍贝叶斯解释，并查看应用于简单数据集的一些结果。我在这篇文章中没有提到代码，但是它可以在 Jupyter 笔记本的 [GitHub 上找到。](https://github.com/WillKoehrsen/Data-Analysis/blob/master/bayesian_lr/Bayesian%20Linear%20Regression%20Demonstration.ipynb)

# 频率线性回归概述

线性回归的 frequentist 观点可能是您在学校中熟悉的观点:该模型假设响应变量(y)是权重乘以一组预测变量(x)的线性组合。完整的公式还包括一个误差项，以说明随机采样噪声。例如，如果我们有两个预测值，等式是:

![](img/87822c8668f1173a3ce5b4258d36c959.png)

y 是响应变量(也称为因变量)，β是权重(称为模型参数)，x 是预测变量的值，ε是一个[误差项，表示随机采样噪声](https://stats.stackexchange.com/questions/129055/understanding-the-error-term)或模型中未包含的变量的影响。

线性回归是一个简单的模型，使其易于解释:β_0 是截距项，其他权重β显示了增加预测变量对响应的影响。例如，如果β_1 为 1.2，那么 x_1 每增加一个单位，响应将增加 1.2。

我们可以使用矩阵方程将线性模型推广到任意数量的预测值。将常数项 1 添加到预测值矩阵以说明截距，我们可以将矩阵公式写成:

![](img/716fa292e856b7be5b37d2e9f83a6bf2.png)

从训练数据学习线性模型的目标是找到最好地解释数据的系数β。在 frequentist 线性回归中，最好的解释是指最小化残差平方和(RSS)的系数β。RSS 是已知值(y)和预测模型输出(ŷ，发音为 y-hat，表示估计值)之间的总平方差。残差平方和是模型参数的函数:

![](img/9ef22b94bcb8eeb15b142c8cd7e201b6.png)

对训练集中的 N 个数据点求和。我们在此不再赘述细节([查看此参考文献了解推导过程](http://www.stat.cmu.edu/~cshalizi/mreg/15/lectures/13/lecture-13.pdf))，但该方程具有模型参数β的封闭形式解，可将误差降至最低。这被称为β的[最大似然估计](https://www.quantstart.com/articles/Maximum-Likelihood-Estimation-for-Linear-Regression)，因为在给定输入 X 和输出 y 的情况下，这是最有可能的值。以矩阵形式表示的封闭解为:

![](img/287b6074c95279fc578879e88599a5db.png)

(同样，我们必须将“帽子”放在β上，因为它表示模型参数的估计值。)不要让矩阵数学把你吓跑了！感谢像 Python 中的[Scikit-learn](http://scikit-learn.org/stable/modules/generated/sklearn.linear_model.LinearRegression.html)这样的库，我们通常不必手工计算(尽管编写线性回归代码是一个好的实践)。这种通过最小化 RSS 来拟合模型参数的方法被称为[普通最小二乘法](https://en.wikipedia.org/wiki/Ordinary_least_squares) (OLS)。

我们从 frequentist 线性回归中获得的是仅基于训练数据的模型参数的单一估计。我们的模型完全由数据提供信息:在这个视图中，我们需要知道的模型的一切都编码在我们可用的训练数据中。

一旦有了β-hat，我们就可以通过应用我们的模型方程来估计任何新数据点的输出值:

![](img/63c14879fe594dccda313195b92fc049.png)

以 OLS 为例，我们可以对 15000 次锻炼观察的持续时间和消耗的卡路里的真实世界数据进行线性回归。下面是通过求解模型参数的上述矩阵方程获得的数据和 OLS 模型:

![](img/0a5a49df0d3466d3f040783e072bcf2d.png)

有了 OLS，我们得到了模型参数的一个*单个*估计，在这种情况下，就是线的截距和斜率。我们可以写出 OLS 提出的方程:

```
calories = -21.83 + 7.17 * duration
```

根据斜率，我们可以说，每增加一分钟的锻炼，就会多消耗 7.17 卡路里。这种情况下的截距并没有那么有帮助，因为它告诉我们，如果我们运动 0 分钟，我们将燃烧-21.86 卡路里！这只是 OLS 拟合过程的一个产物，该过程找到最小化训练数据上的误差的线，而不管它在物理上是否有意义。

如果我们有一个新的数据点，比如说锻炼持续时间为 15.5 分钟，我们可以将它代入等式，得到燃烧卡路里的点估计值:

`calories = -21.83 + 7.17 * 15.5 = 89.2`

普通最小二乘法为我们提供了输出的单点估计，我们可以将其解释为给定数据的最有可能的估计。然而，如果我们有一个小的数据集，我们可能希望将我们的估计表示为可能值的分布。这就是贝叶斯线性回归的用武之地。

# 贝叶斯线性回归

在贝叶斯观点中，我们使用概率分布而不是点估计来制定线性回归。响应 y 不是作为单个值来估计的，而是假设从概率分布中提取的。响应从正态分布取样的贝叶斯线性回归模型为:

![](img/220bca506de9dd2082ace8aa53e0d46d.png)

输出 y 由以平均值和方差为特征的正态(高斯)分布产生。线性回归的平均值是权重矩阵的转置乘以预测矩阵。方差是标准差σ的平方(乘以单位矩阵，因为这是模型的多维公式)。

贝叶斯线性回归的目的不是找到模型参数的单个“最佳”值，而是确定模型参数的后验分布。不仅响应是由概率分布生成的，而且模型参数也被假定来自一个分布。模型参数的后验概率取决于训练输入和输出:

![](img/c28e8fccee8e7e7ec8a8634831d3eb5d.png)

这里，P(β|y，X)是给定输入和输出的模型参数的后验概率分布。这等于数据的似然性 P(y|β，X)乘以参数的先验概率并除以归一化常数。这是贝叶斯定理的一个简单表达式，贝叶斯推理的基础:

![](img/cad4662c3c32a36d0feb7a88ad9e18df.png)

让我们停下来想想这意味着什么。与 OLS 相反，我们有一个模型参数的后验*分布*，它与数据的可能性乘以参数的*先验*概率成正比。在这里，我们可以观察到贝叶斯线性回归的两个主要好处。

1.  **先验:**如果我们有领域知识，或者对模型参数应该是什么的猜测，我们可以将它们包括在我们的模型中，不像 frequentist 方法那样假设关于参数的所有知识都来自数据。如果我们事先没有任何估计，我们可以使用[无信息先验](https://stats.stackexchange.com/questions/27813/what-is-the-point-of-non-informative-priors)来获得正态分布等参数。
2.  **后验:**执行贝叶斯线性回归的结果是基于数据和先验的可能模型参数的分布。这使我们能够量化我们对模型的不确定性:如果我们有更少的数据点，后验分布将更加分散。

随着数据点数量的增加，似然性会冲掉先验，在无限数据的情况下，参数的输出会收敛到从 OLS 获得的值。

将模型参数公式化为分布概括了贝叶斯世界观:我们从最初的估计开始，我们的先验，随着我们收集更多的证据，我们的模型变得不那么错误。贝叶斯推理是我们直觉的自然延伸。通常，我们有一个初始假设，当我们收集支持或反对我们想法的数据时，我们改变了我们的世界模型(理想情况下，这是我们推理的方式)！

# **实现贝叶斯线性回归**

在实践中，对于连续变量，评估模型参数的后验分布是困难的，因此我们使用抽样方法从[后验中抽取样本，以近似后验](https://stats.stackexchange.com/questions/307882/why-is-it-necessary-to-sample-from-the-posterior-distribution-if-we-already-know)。从分布中抽取随机样本来近似分布的技术是[蒙特卡罗方法](https://en.wikipedia.org/wiki/Monte_Carlo_method#Applied_statistics)的一个应用。蒙特卡洛采样有多种算法，最常见的是马尔可夫链蒙特卡洛的[变体(关于 Python](https://s3.amazonaws.com/academia.edu.documents/39690347/750.pdf?AWSAccessKeyId=AKIAIWOWYYGZ2Y53UL3A&Expires=1523672050&Signature=t6fJD5FhDmFjBALtL1%2FUQgGOz9Y%3D&response-content-disposition=inline%3B%20filename%3DMarkov_chain_Monte_Carlo_algorithms_for.pdf) 中的应用，参见本文[)。](/markov-chain-monte-carlo-in-python-44f7e609be98)

## 贝叶斯线性建模应用

我将跳过这篇文章的代码(有关 PyMC3 中的实现，请参见笔记本)，但实现贝叶斯线性回归的基本过程是:指定模型参数的先验(在本例中我使用了正态分布)，创建一个将训练输入映射到训练输出的模型，然后让马尔可夫链蒙特卡罗(MCMC)算法从模型参数的后验分布中抽取样本。最终结果将是参数的后验分布。我们可以检查这些分布来了解发生了什么。

第一个图显示了模型参数的后验分布的近似值。这些是 1000 步 MCMC 的结果，意味着该算法从后验分布中提取了 1000 步。

![](img/658c3385f8b2029eebc19cec2ac8ea83.png)![](img/e365d5565c8510b0fe900aad203abd73.png)

如果我们将斜率和截距的平均值与 OLS 的进行比较(OLS 的截距为-21.83，斜率为 7.17)，我们会发现它们非常相似。然而，虽然我们可以使用平均值作为单点估计，但我们也有模型参数的一系列可能值。随着数据点数量的增加，该范围将缩小并收敛到一个单一值，该值代表模型参数的更大置信度。(在贝叶斯推理中，变量的范围被称为可信区间，它的[解释与频率推理中的置信区间](https://en.wikipedia.org/wiki/Credible_interval)略有不同)。

当我们想要显示贝叶斯模型的线性拟合，而不是只显示估计值时，我们可以绘制一系列线，每条线代表模型参数的不同估计值。随着数据点数量的增加，线条开始重叠，因为模型参数中的不确定性减少了。

为了演示模型中数据点数量的影响，我使用了两个模型，第一个模型使用了 500 个数据点，左边显示了结果拟合，右边的模型使用了 15000 个数据点。每个图显示了从模型参数后验中提取的 100 个可能的模型。

![](img/f66826c7f1362e009528cebe54e6f232.png)![](img/8ec93dc6d32018ac4b46f040f3872d54.png)

Bayesian Linear Regression Model Results with 500 (left) and 15000 observations (right)

当使用较少的数据点时，拟合中有更多的变化，这代表了模型中更大的不确定性。对于所有的数据点，OLS 和贝叶斯拟合几乎是相同的，因为先验被数据的可能性冲掉了。

当使用我们的贝叶斯线性模型预测单个数据点的输出时，我们也得不到单个值，而是一个分布。下面是运动 15.5 分钟消耗卡路里数的概率密度图。红色垂直线表示来自 OLS 的点估计。

![](img/6492054b892feaed708bc4cfcb2bef10.png)

Posterior Probability Density of Calories Burned from Bayesian Model

我们看到，燃烧卡路里数的概率峰值在 89.3 左右，但完整的估计是一个可能值的范围。

# 结论

不要在贝叶斯与频率主义的辩论(或任何争论)中偏袒任何一方，学习这两种方法更有建设性。这样，我们可以在正确的情况下应用它们。

在我们只有有限的数据或有一些先验知识要在我们的模型中使用的问题中，贝叶斯线性回归方法既可以包含先验信息，又可以显示我们的不确定性。贝叶斯线性回归反映了贝叶斯框架:我们形成一个初始估计，并在收集更多数据时改进我们的估计。贝叶斯观点是一种直观的看待世界的方式，贝叶斯推理是一种有用的替代方式。数据科学不是站在哪一边，而是找出工作的最佳工具，掌握更多技术只会让你更有效率！

一如既往，我欢迎反馈和建设性的批评。可以通过推特 [@koehrsen_will](http://twitter.com/@koehrsen_will) 联系到我。

**来源**

1.  [https://www . quant start . com/articles/Bayesian-Linear-Regression-Models-with-pymc 3](https://www.quantstart.com/articles/Bayesian-Linear-Regression-Models-with-PyMC3)
2.  [http://twiecki.github.io/blog/2013/08/12/bayesian-glms-1/](http://twiecki.github.io/blog/2013/08/12/bayesian-glms-1/)
3.  [https://wise odd . github . io/tech blog/2017/01/05/Bayesian-regression/](https://wiseodd.github.io/techblog/2017/01/05/bayesian-regression/)
4.  [PyMC3 简介](https://pdfs.semanticscholar.org/8085/b60ce1771647f11ccc4728397275b502f359.pdf)