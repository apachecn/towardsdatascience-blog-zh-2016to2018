# 给深度学习增加不确定性

> 原文：<https://towardsdatascience.com/adding-uncertainty-to-deep-learning-ecc2401f2013?source=collection_archive---------0----------------------->

![](img/be2aa0c0630a0bbfcf63e91c3f7990a4.png)

## 如何使用 Edward 和 TensorFlow 构建深度学习模型的预测区间

统计建模和机器学习之间的区别日益模糊。他们都从数据中学习并预测结果。主要的区别似乎来自于不确定性估计的存在。不确定性估计允许假设检验，尽管通常以可伸缩性为代价。

> 机器学习=统计建模-不确定性+数据

理想情况下，我们通过向机器学习添加不确定性来融合两个世界的优点。最近在变分推理( **VI** )和深度学习( **DL** )方面的发展使这成为可能(也叫 [**贝叶斯深度学习**](http://dustintran.com/blog/a-quick-update-edward-and-some-motivations) )。VI 的好处在于，它可以很好地随数据大小伸缩，并且很好地适应允许模型合成和随机优化的 DL 框架。

向模型添加不确定性的一个额外好处是，它促进了 [**基于模型的机器学习**](https://blog.dominodatalab.com/an-introduction-to-model-based-machine-learning/) 。在机器学习中，预测的结果是你的模型的基础。如果结果没有达到标准，策略就是“向问题扔数据”，或者“向问题扔模型”，直到满意为止。在基于模型(或贝叶斯)的机器学习中，您被迫指定数据和参数的概率分布。想法是首先明确地指定模型，然后检查结果(比点估计更丰富的分布)。

## 贝叶斯线性回归

这是一个给简单的线性回归模型增加不确定性的例子。简单的线性回归预测标签 *Y* 给定数据 *X* 权重 *w.*

> Y = w * X

目标是通过最小化损失函数找到未知参数 *w* 的值。

> (Y - w * X)

让我们把这个转换成一个概率。如果假设 *Y* 为高斯分布，则上述等价于最大化以下**相对于 *w* 的数据似然**:

> p(Y | X，w)

到目前为止，这是传统的机器学习。要给你的体重估计增加不确定性，并将其转化为贝叶斯问题，只需在原始模型上附加一个**先验分布**即可。

> p(Y | X，w) * p(w)

请注意，这相当于通过贝叶斯规则反演原始机器学习问题的概率:

> p(w | X，Y) = p(Y | X，w) * p(w) /常数

给定数据的权重的概率( *w* )就是我们对于不确定性区间所需要的。这是重量 *w* 的**后验分布**。

尽管添加先验在概念上很简单，但计算通常很难；也就是说，*常数*是一个很大的、不好的积分。

## 蒙特卡罗积分

概率分布的积分近似值通常是通过抽样来完成的。对分布进行采样和平均将得到期望值的近似值(也称为**蒙特卡罗积分**)。所以让我们把积分问题重新表述成一个期望问题。

上面的*常数*从数据和权重之间的联合分布中整合出权重。

> 常数= ∫ p(x，w) dw

要将其重新表述为期望值，引入另一个分布， *q* ，并根据 *q* 取期望值。

> ∫ p(x，w) q(w) / q(w) dw = E[ p(x，w) / q(w) ]

我们选择一个 *q* 分布，这样就很容易从中取样。从 *q* 中抽取一串 *w* ，取样本平均值得到期望值。

> E[ p(x，w) / q(w) ] ≈样本均值[ p(x，w) / q(w) ]

这个想法我们以后会用到变分推理。

## 变分推理

变分推理的思路是，你可以引入一个**变分分布， *q*** ，带**变分参数， *v*** ，把它变成一个优化问题。分布 *q* 将近似于后验分布。

> q(w | v) ≈ p(w | X，Y)

这两个分布需要接近，一个自然的方法将最小化它们之间的差异。通常使用 Kullback-Leibler 散度( **KL 散度**)作为差分(或变分)函数。

> KL[q || p] = E[ log (q / p) ]

KL 散度可以分解为数据分布和证据下界( **ELBO** )。

> KL[q || p] =常数- ELBO

*常量*可以忽略，因为它不依赖于 *q* 。直觉上， *q* 和 *p* 的分母相互抵消，剩下 ELBO。现在我们只需要在 ELBO 上进行优化。

ELBO 只是具有变化分布的原始模型([在这里详细计算](http://davmre.github.io/inference/2015/11/13/elbo-in-5min))。

> ELBO = E[ log p(Y | X，w)*p(w) - log q(w | v) ]

为了获得对 *q* 的期望值，使用蒙特卡罗积分(取样并取平均值)。

在深度学习中，通常使用随机优化来估计权重。对于每个小批，我们取损失函数的平均值来获得梯度的随机估计。类似地，任何具有自动微分的 DL 框架都可以将 ELBO 估计为损失函数。唯一的区别是你从 *q* 中取样，平均值将是期望值的一个很好的估计，然后是梯度。

## 构建预测间隔的代码示例

让我们对一些生成的数据运行贝叶斯简单线性回归示例。以下内容也适用于 DL 模型，因为它们只是简单的线性回归。我们使用一个名为 Edward (基于 [TensorFlow](https://www.tensorflow.org) )的[贝叶斯深度学习库来构建模型。](http://edwardlib.org)

[Gist 上有完整的代码。](https://gist.github.com/tokestermw/a9de2ef498a09747bbf673ddf6ea4843)

第一步是定义权重的先验分布。

```
weight = ed.models.Normal(mu=tf.zeros(1), sigma=tf.ones(1))
```

在深度学习中，`weight`将是点估计。在贝叶斯 DL 中，`weight`是一个分布，它的后验概率由下面的变分分布来近似。

```
qw_mu = tf.get_variable(tf.random_normal([1]))
qw_sigma = tf.nn.softplus(tf.get_variable(tf.random_normal([1]))
qweight = ed.models.Normal(mu=qw_mu, sigma=qw_sigma)
```

注意，变化参数`qw_mu`和`qw_sigma`是估计的，而`weight`不是。从`qweight`采样将给出`weight`的后验不确定性区间(假设方差参数固定)。

我们将线性回归定义为预测模型。任何 DL 型号都可以替代。

```
nn_mean = weight * x + bias
```

数据似然性的构造类似于先验分布(假设方差参数固定)。

```
nn = ed.models.Normal(mu=nn_mean, sigma=tf.ones(1))
```

ELBO 度量取决于数据似然性和先验。数据似然性由标签数据绑定。每个先验分布必须由变分分布约束(假设变分分布之间是独立的)。

```
latent_vars = {weight: qweight, bias: qbias}
data = {nn: y}
```

运行推理优化很容易，因为:

```
inference = ed.KLqp(latent_vars, data)
inference.run()
```

为了提取不确定性区间，我们直接对变分分布`qweight`进行采样。这里，前两个矩是在采样 100 次后提取的。

```
mean_, var_ = tf.nn.moments(qweight.sample(100))
```

我们还可以将不确定性区间添加到预测中(也称为**后验预测区间**)。这可以通过复制模型并用所有后验概率替换先验来完成(本质上是在对数据进行训练之后更新模型)。

```
nn_post = ed.copy(nn, dict_swap={weight: qweight.mean(), bias: qbias.mean()})
```

再次运行图表 100 次，我们得到预测的不确定性区间。

```
mean_, var_ = tf.nn.moments(nn_post.sample(100), feed_dict={x: data})
```

现在我们有了整个分布，假设检验就容易了。

```
tf.reduce_mean(nn_post.sample(100) > labels)
```

## 但是等等，还有更多，辍学间隔！

如果完全贝叶斯是一个太多的投资，不确定性区间可以计算从神经网络通过辍学层。

机器学习模型的正则化和贝叶斯模型中的先验分布之间有很强的联系。例如，经常使用的 L2 正则化本质上是高斯先验([更多细节在此](http://stats.stackexchange.com/a/163450))。

Dropout 是一种根据伯努利分布随机清除神经元的技术。这可以通过 KL 散度标准下的高斯过程来近似(这里有[更多细节，因为我不确定数学上的](http://mlg.eng.cam.ac.uk/yarin/blog_3d801aa532c1ce.html#uncertainty-sense))。

在实践中，这意味着我们在漏失层打开的情况下运行模型 100 次，并查看结果分布。

```
# keep the dropout during test time
mc_post = [sess.run(nn, feed_dict={x: data}) for _ in range(100)]
```

`mc_post`的样本均值是估计值。对于不确定性区间，我们简单地计算样本方差加上以下反精度项:

```
def _tau_inv(keep_prob, N, l2=0.005, lambda_=0.00001):
    tau = keep_prob * l2 / (2\. * N * lambda_)
    return 1\. / tau
```

这给了我们原则上的不确定性区间，而不需要太多投资。

```
np.var(mc_post) + _tau_inv(0.5, 100)
```

## 摘要

要获得不确定性区间，您可以:

1.  在模型中加入一个先验，通过变分推理逼近后验，然后从后验中取样
2.  运行现有模型很多次，并打开丢弃层

混合解决方案是在感兴趣的特定参数上增加不确定性。在自然语言建模中，这可能只是将单词嵌入作为潜在变量。