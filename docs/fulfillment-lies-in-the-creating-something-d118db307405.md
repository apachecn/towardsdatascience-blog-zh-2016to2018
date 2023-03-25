# 成就感在于创造一些东西

> 原文：<https://towardsdatascience.com/fulfillment-lies-in-the-creating-something-d118db307405?source=collection_archive---------15----------------------->

## 这篇博文旨在提供对生成性对抗网络及其数学的完整直观理解，包括它的张量流实现。

注意:读者应该对深度学习有一个基本的了解。

在 2014 年的一篇论文中，蒙特利尔大学的博士生伊恩·古德菲勒介绍了一篇名为《生成对抗网络》的论文，他在米兰的导师包括[约舒阿·本吉奥](http://www.iro.umontreal.ca/~bengioy/yoshua_en/index.html)和[亚伦·库维尔](https://aaroncourville.wordpress.com/)。这篇论文被引用了 5000 多次，并得到了所有深度学习主要先驱的评论。阅读下面 Yann Lecun 对 Gan 的评论。

> 生成对抗网络是近十年来机器学习中最有趣的想法——Yann le Cun(脸书人工智能主任)

这清楚地表明，生成对立网络做了一些真正有趣的事情。从现在起，我们将把生成性对抗网络称为甘的网络。Gan 很有趣，因为他们可以学习近似数据分布(也称为模仿真实数据)，这是非常强大的，因为现在它可以生成现实中不存在的图像、音频剪辑、视频。在某种意义上，你可以认为甘是艺术家。的一句名言解释了甘的直觉。

> 我不能创造的东西，我不理解——理查德·费曼(美国理论物理学家)

甘的作品产生的效果非常强大，2018 年 10 月，佳士得数码屋(Christie's digital house)一家法国公司在一场拍卖中以 432，000 美元的价格出售了一幅由甘创作的肖像(图 1.0)。

![](img/93a9ab3490c8cdae33eef289c82acb4f.png)

fig 1.0 Portrait by a Gan. source: [here](http://www.artfixdaily.com/news_feed/2018/08/21/461-ai-generated-artwork-heads-to-christies-in-a-first-ever-sale)

# **为什么要生成对抗性模型？**

这部分是 OpenAI 关于[甘的博文摘要。这个想法是，如果 Gan 在创建数据方面变得更好，那么它也能够比任何其他算法更好地理解世界上存在的数据。随着他们学习数据的底层表示，他们将会更好地理解它。Gan 属于生成模型类，基于可微分生成网络的方法，因此它们是实现这一点的方法之一，其他一些流行的方法有:](https://blog.openai.com/generative-models/)

1.  [变型自动编码器](https://www.doc.ic.ac.uk/~js4416/163/website/autoencoders/variational.html)。
2.  Rnn 的像 [LSTM 的](http://colah.github.io/posts/2015-08-Understanding-LSTMs/)或者 [PixelRnn 的](https://arxiv.org/abs/1601.06759)。
3.  [RNN-RBM 顺序输出](http://deeplearning.net/tutorial/rnnrbm.html)。
4.  [卷积玻尔兹曼机器](https://en.wikipedia.org/wiki/Convolutional_Deep_Belief_Networks)等等。

稍后将详细介绍这些模型，但想法是一样的，所有这些架构都试图了解定义数据的底层表示。生成模型通常属于一个活跃的研究领域，称为[表示学习](https://opensource.com/article/17/9/representation-learning)，即从未标记的数据中学习表示。以上所有的工作方式都不同，但他们都试图学习概率分布。

# **甘的作品如何？**

Gan 的想法很容易理解，它由两个架构组成:一个是鉴别器，用于估计样本为真实样本的概率；另一个是生成器，用于生成接近真实样本的数据。随着时间的推移，鉴别器在鉴别方面变得更好，而生成器在生成更接近真实样本的样本方面变得更好(图 1.1 用于说明)。这两个模型的目标是相反的，因为当生成器最大化鉴别器出错的概率的同时，鉴别器尽力预测正确的标签(真的或假的)。如果你仔细想想一个模型在帮助另一个变得更好，这个想法也会变得有趣。

![](img/530da235258d816f2625a428dbc84540.png)

fig 1.1\. Block Diagram of Gan’s. source: [here](https://www.researchgate.net/figure/General-block-diagram-of-generative-adversarial-networks-GANs_fig1_317388182)

考虑图 1.2。这里 p(x)(右边蓝色的一个)是图像的真实分布，pθ(x)(绿色的一个)是从通过我们的神经网络的高斯分布中提取的点。最初，这些点将是完全随机的，但与训练一样，我们的模型参数θ被优化以最小化两个分布之间的差异(通过使用 [KL 散度](https://en.wikipedia.org/wiki/Kullback%E2%80%93Leibler_divergence)或任何其他手段)。

![](img/ff1d2342249d7c9b41e76dd0ebcda932.png)

fig 1.2 source: [here](https://blog.openai.com/generative-models/)

发生器和鉴别器可以是相同或不同的结构，在 Gan 的最初论文中，他们通过提供随机噪声作为发生器的输入来使用多层感知器，并且另一个多层感知器模型被用作鉴别器，其检测样本 a 是产生的还是真实的。在本文的后面部分，我们将通过使用一种基于卷积的架构(称为 DCGAN(深度卷积生成对抗网络))来构建我们自己的生成神经网络，但首先让我们将 GAN 背后的数学放入正确的位置。我会尽量让数学简单，但要确保你的基础知识顺序正确。

# **制定目标函数**

目标是通过将随机噪声 *z* 作为输入经过神经网络 G(z； *θg* )其中 *θg* 是生成器神经网络的参数，把任何一个神经网络看成 a，`**Universal function approximator**`，我们还将定义另一个神经网络 D(*x；θd* )鉴别器，这个网络将给出概率 *d(x)* 【在 0 到 1 之间】 *x* 来自真实数据而不是 *Pg，*当 G 被训练来最小化 log(1D(G(z))时，D 被训练来预测正确的标签。G 的最小化项 log(1D(G(z)))是有意义的，因为最初，概率 D(G(z))会很低，因为发生器刚刚开始产生样本，鉴别器很容易预测样本是真的还是假的，所以 log(1D(G(z))的总体结果很大。所以当 G 的目标是最小化时，D 的目标是最大化，把这整个情况看作一个最小-最大博弈 V(D，G)

![](img/384afbe5c66d08cc37b87469deeaced3.png)

fig 1.3 Loss function of Gan’s source [here](https://arxiv.org/abs/1406.2661)

打破上面的等式，

首先，Ex∞pdata(x)[log(D(x))]—期望[最大化生成的样本 x 是实数]。

第二，Ez∞pz(z)[log(1D(G(z))]—期望[最小化生成的样本不是真实的。]

上述等式本质上是一个最小-最大博弈，其中一个人试图最小化，另一个人试图最大化。让我们以机器学习中的 L2 正则化为例来理解这种最小-最大博弈情况。在正则化中，正则化项防止权重非常小，但同时优化器最小化总体目标函数，这通常导致小的权重值。这种情况类似于 gan 的最小-最大情况，但不同之处在于，Gan 的最小化和最大化都是由优化器本身控制的。

理想情况下，D(x)的最佳值应该是 0.5，这意味着它不能区分真样品或假样品，即

d∫(x)=*Pdata(x)/*(*Pdata(x)+Pg(x)*)和 *Pg = Pdata。*

同时训练 D(*x；θd* )和 G(z； *θg* 理论上看起来不错但实际操作中我们通常训练 D(*x；θd* )对于我们每次训练 G(z； *θg* )使得鉴频器始终强于发生器。

# **生成性对抗网络训练。**

考虑下面给出的图。

![](img/a2c4eb7847769db28d35bdd8aa36d093.png)

fig 1.4 source [here](https://arxiv.org/pdf/1406.2661.pdf)

这个想法很简单，训练网络进行一些迭代，直到结果看起来令人满意。

重复以下步骤 N 次迭代:

步骤 1:对于 n 次迭代中每一次，K 次迭代:
`1.select a mini-batch of noise samples (z1....zm)
2.select a mini-batch of real-data samples (r1....rm)
3.train the discriminator with using max-min loss function [eq(1)] over m samples.`

第二步:对于下一次迭代:
`1.select a mini-batch (z1.....zm) from p(z)
2.train the generator by minimizing the log probability of generator making mistake.`

最初的论文提到了使用 SGD 进行训练，但建议使用更强大的优化器，如 Adam、Adagrad、rms-prop 等。

# **什么是 DCGAN？**

DCGAN 或深度卷积生成对抗网络是对以前的 GAN 的一大改进。**虽然理论上甘的架构看起来很简单，实际操作起来却很难训练出一个甘**。原因是，梯度必须流过鉴别器和发生器，由于这个事实，Gan 的训练变得不稳定。我们将在本文的后面部分讨论这些主题，但首先，让我们试着理解 DCGAN 的架构。

DCGAN 本身是一类基于深度卷积的架构，由两个卷积神经网络组成，具有以下约束条件(根据原始论文，阅读:[此处为](https://arxiv.org/pdf/1511.06434.pdf))。以下约束是在执行广泛的模型搜索后得出的。

1.  用步长卷积替换所有池层，通过这种方法，网络不是依赖池层(最大池或平均池)来执行空间下采样，而是自己学习。
2.  移除所有完全连接的层以创建更深层次的架构，因为完全连接的层会影响网络的收敛。
3.  使用批处理标准化来稳定深层之间的梯度流。
4.  对生成器中的所有层使用 ReLu 激活，除了使用 tanh 的输出层和使用 sigmoid 或 softmax 的鉴别器。

在 DCGAN 的原始论文中提出的体系结构包含一个产生噪声的输入层以及发生器中的四个去卷积层和鉴别器中的四个卷积层以及一个输出层。

![](img/1ce21a20a5009662ea2a5dc82463a97c.png)

fig 1.5 DCGAN architecture. source [here](https://gluon.mxnet.io/_images/dcgan.png)

如果你不知道转置卷积，那么看看 Naoki Shibuya 的博客文章。

# 构建深度卷积生成对抗网络。

本次演示的代码取自于[tensor layer](https://github.com/tensorlayer)DCGAN 的原始实现，经过修改后可用于 [Sully Chen](https://github.com/SullyChen) 数据集和分层可视化，以真正理解网络是如何学习的。该数据集是为训练自动驾驶汽车而建立的，它包含 63k 张道路驾驶图像，以及相应的转向角和其他值，如油门踏板等。他们使用这个数据集的主要想法是看看 Gan 能多好地模拟真实的驾驶场景，如其他行人、车辆等。如果你从更大的角度来看，这个实验很棒，因为现在你不需要依赖安装在你车前的摄像头来捕捉道路图像，你只需要坐在一个房间里就可以产生无尽的数据。虽然用生成的图像来训练自动驾驶汽车的想法有多好？我将把它留待进一步讨论。

注意:模型的所有权重都使用 Glorot 初始化器初始化。典型的 Glorot 初始化如下所示，

步骤 1: std_dev = sqrt(2.0/(扇入+扇出))
步骤 2:权重=正态分布(平均值=0.0，stddev)

如果你想了解更多关于 Glorot 初始化器或者为什么使用它，点击[这里](http://andyljones.tumblr.com/post/110998971763/an-explanation-of-xavier-initialization)。

# **Tensorflow 实现的生成器**

让我们看看生成器的代码。代码的流程如下:

1.定义发生器参数，如输出图像大小，过滤器参数，伽玛初始化。

2.输入层的维数为 100，即 100 个随机数，由均值= 0、标准偏差= 1.0 的均匀分布生成。

3.输入层与 64 *8*64*64 单元的密集层相连。

4.重塑致密层高度= 64，宽度= 64，深度= 512

5.使用批量归一化进行归一化，gamma 采样自均值= 1.0、std_dev=0.02 且 ReLu 激活的均匀分布。

6.DeConv2d 或简单转置卷积层，内核=(5，5)，步幅= (h=2，w=2)，深度 64 *4，零填充。

7.使用 ReLu 激活进行批处理标准化。

8.DeConv2d 或简单转置卷积层，内核=(5，5)，步幅= (h=2，w=2)，深度 64 *4，零填充。

9.使用 ReLu 激活进行批处理标准化。

10.DeConv2d 或简单转置卷积层，内核=(5，5)，步幅= (h=2，w=2)，深度 64 *4，零填充，双曲正切激活。

# 鉴别器的张量流实现

1.  定义模型参数，如输出图像大小，过滤器参数，伽玛初始值。
2.  卷积层，内核大小= (5，5)，步距= (h=2，w=2)，具有 Relu 激活和零填充。
3.  卷积层，内核大小= (5，5)，步距= (h=2，w=2)，具有 Relu 激活和零填充。
4.  使用 gamma =正态分布(平均值= 1.0，标准差= 0.02)进行批量标准化。
5.  卷积层，内核大小= (5，5)，步距= (h=2，w=2)，具有 Relu 激活和零填充。
6.  使用 gamma =正态分布(平均值= 1.0，标准差= 0.02)进行批量标准化。
7.  卷积层，内核大小= (5，5)，步距= (h=2，w=2)，具有 Relu 激活和零填充。
8.  使用 gamma =正态分布(平均值= 1.0，标准差= 0.02)进行批量标准化。
9.  展平图层。
10.  带有 1 个隐藏单元的输出层，带有 Sigmoid 激活。

# **让我们将生成的结果与真实结果进行比较**

我不得不说结果看起来比预期的更有希望。在此之前，让我们看看真实数据图像是什么样的(下面的样本是从真实数据集中随机抽取的，目的是看看 gans 在理解数据方面有多好)。

![](img/464f3d7ed9cf902f73a7c2385169590b.png)![](img/0609e2388e8309740873ea479357004f.png)![](img/3ec3901b3c15c1613d4910712e587114.png)![](img/2c1e13ad1631f565ee8f6f6921c797b2.png)![](img/e8204cf5c2e2cac727748418ab955a10.png)![](img/b50a5f888caab82be9d2767fe4844af8.png)![](img/8a4b9ffc90dde818af3a74572f2058e7.png)![](img/7a4b6cb31681610d5601d6fd9691c7c3.png)![](img/4e5c96b70efe8543afffcafdd68aa13a.png)![](img/d808304c32fd275a40b5604dad88060b.png)![](img/87c70d6b388d6c50510e084b27461699.png)![](img/5b38df9d22607369784f722fc28a2c43.png)

fig 1.6 Random samples from train dataset [source](https://github.com/SullyChen/driving-datasets)

现在生成的样本，

![](img/18c00af7045863584053202773782e76.png)![](img/e8314d5b5fd884c64962d58f7243ec3f.png)![](img/efc20f0b55ac9944275a07ad9dc33c1b.png)

epoches from 1st to 3rd

![](img/93b0cb0dfa0e6a52d541afea19743efa.png)![](img/40c0f2849bf817932e2758f00d61be95.png)![](img/17340054f8145f57e4a71c9b0cfb7f56.png)

epoches from 4th to 7th

![](img/d4cdd2193a1f4539b5cf3e19732e7f63.png)![](img/88e62ce2140343c57689a976b7d79e75.png)![](img/ed61d6ce90af5f66dec1ec733e97230f.png)

fig 1.7 Incremental progress shown using epoches as progress points.

如您所见，第一个时期模型的样本只能获得数据的本质，但随着时期数量的增加，输出变得更好，在第 10 个时期，输出变得更好。这个网络被训练了 30 个 epoches，让我们来想象一下。从 epoches 11 到 epoches 排列成一行的图像。

![](img/cfa9a078dc7065a3081f2c9360039e41.png)![](img/6c5f8fa4fcbfbf8e3729ca8d1b95c5f6.png)![](img/225f94d287948655edae771ab4bb9109.png)![](img/319772e2b10dc34ee96628821d23b49e.png)![](img/19f4ed249c938ed50fc955bbe24ff95a.png)![](img/f78e961338558e5d15ecd4d35b2e7e25.png)![](img/701f994adae3d207b3033c8fcf9a48aa.png)

fig 1.8 Generated samples from 11th epoch to 30th(some results skipped)

接近最后一个时代，结果开始捕捉甚至微小的建筑，如轮胎，灌木等。尽管效果不错，但生成器有时仍无法捕捉道路和山脉之间的模糊边缘，导致两者融合。结果仍然是完全可以接受的。

# 发电机和鉴频器损耗。

Gan 的会聚不稳定，这直接反映在损耗值的变化中。下面提供了我们 gan 架构的损耗值。

```
Epoch: [ 2/30] [ 139/ 997] time: 0.4634, d_loss: 0.67223823, g_loss: 0.71121359
Epoch: [ 2/30] [ 140/ 997] time: 0.4640, d_loss: 1.28069568, g_loss: 1.98622787
Epoch: [ 2/30] [ 141/ 997] time: 0.4628, d_loss: 1.44974589, g_loss: 0.46058652
Epoch: [ 2/30] [ 142/ 997] time: 0.4819, d_loss: 1.02387762, g_loss: 1.69937968
Epoch: [ 2/30] [ 143/ 997] time: 0.4781, d_loss: 0.59786928, g_loss: 1.81390572
Epoch: [ 2/30] [ 144/ 997] time: 0.4632, d_loss: 0.96302533, g_loss: 0.62419045
Epoch: [ 2/30] [ 145/ 997] time: 0.4622, d_loss: 0.62077224, g_loss: 1.15416789
Epoch: [ 2/30] [ 146/ 997] time: 0.4726, d_loss: 0.57695013, g_loss: 1.11101508
Epoch: [ 2/30] [ 147/ 997] time: 0.4843, d_loss: 0.64481205, g_loss: 1.35732222
Epoch: [ 2/30] [ 148/ 997] time: 0.4617, d_loss: 0.46775422, g_loss: 1.74343204
Epoch: [ 2/30] [ 149/ 997] time: 0.4668, d_loss: 0.60213166, g_loss: 0.84854925
Epoch: [ 2/30] [ 150/ 997] time: 0.4637, d_loss: 0.75188828, g_loss: 1.56600714
Epoch: [ 2/30] [ 151/ 997] time: 0.4644, d_loss: 0.80763638, g_loss: 0.80851054
Epoch: [ 2/30] [ 152/ 997] time: 0.4685, d_loss: 0.66286641, g_loss: 0.79960334
Epoch: [ 2/30] [ 153/ 997] time: 0.4619, d_loss: 0.64668310, g_loss: 1.32020211
Epoch: [ 2/30] [ 154/ 997] time: 0.4742, d_loss: 0.46545416, g_loss: 1.55003786
Epoch: [ 2/30] [ 155/ 997] time: 0.4970, d_loss: 0.94472808, g_loss: 0.49848381
Epoch: [ 2/30] [ 156/ 997] time: 0.4768, d_loss: 0.78345346, g_loss: 1.03955364
```

我们可以清楚地观察到鉴频器和发电机损耗值上升和下降了多少。与 gans 训练相关的问题将在本博客的后半部分讨论。

# 使用发生器和鉴别器的分层可视化来解释 Gan

在我们看到的大多数应用中，涉及到神经网络，它们就像一个黑盒，即从一方给出数据并得到输出。我们通常不了解网络内部发生了什么，即使是在抽象的层面上，所以我决定在这个方向上迈出一步，并试图了解生成器和鉴别器中每一层的输出发生了什么，网络是如何生成或鉴别数据的。

注:所有分层输出都是在 [Sully Chen](https://github.com/SullyChen) 上用 30 个历元训练网络后归一化前得到的。

## **发电机**

***第 1 层:*** 正如所料，最初网络只产生一些噪声。

![](img/dd11023a6e206a211cd3acaeb9c1577d.png)

fig 1.9 1st Layer output

***第二层:*** 网络开始生成一些模式。

![](img/545674be6368c6016a8fec15eb1873a1.png)

fig 1.10 2nd Layer output.

***第三层:*** 有用的模式出现了。

![](img/8a3292d317fb062d1ae1f400678d9214.png)

fig 1.11 3rd Layer output

***第 4 层:*** 将应用 tanh 激活的最终图案。

![](img/908516446a8b338c08ec353567301ecf.png)

fig 1.12 4th Layer Output

## 鉴别器

***第 1 层:*** 我们知道较低层用于提取简单的特征，如水平或垂直边缘检测，在我们的实验中观察到了同样的情况。

![](img/329abe805d0be2df871dce1fc1dcde21.png)

fig 1.13 1st Layer output.

***第二层:*** 第二层的输出在第一层输出后完成。我们能理解的不多，但所有这些对网络本身都很重要。

![](img/2d611582b287e79d13942c7673c414a5.png)

fig 1.14 2nd Layer output.

***第三层:*** 尺寸变小，预测仅与网络相关。

![](img/5852b520d024d170b846ab086427e715.png)

fig 1.15 3rd Layer output.

***第四层:*** 网络越深入，很难做出任何解读，但网络就是这么做的。

![](img/384301d8528a1130b64ebc6dc65dbcbe.png)

fig 1.15 4th Layer output.

这些可视化让我们对网络如何预测以及每一层的输出发生了什么有了一些基本的了解，但我们只是触及了表面，要了解这些网络在做什么还有很多。这个开发的完整代码可以在我的 github [这里](https://github.com/Abhinavfreecodecamp/DCGAN)找到。

# 甘的问题

理论上，gan 的工作听起来不错，似乎应该收敛到全局最小值，但实际上，当生成器 G 和鉴别器 D 都表示为神经网络时，收敛的机会急剧下降，并且在大多数情况下，收敛将停止在鞍点。

在训练甘时，主要观察到以下问题

1.  **纳什均衡问题:**这个问题主要是由于 Gan 最小-最大公式而存在的。在这种情况下，一个函数的两个部分只能在一个点上达到最佳值，在静止状态下，它保持不稳定。下面的例子摘自[深度学习书籍](https://www.deeplearningbook.org/)，考虑一个函数 F(u，v) = uv，其中我们必须最小化 u 的值和最大化 v 的值，那么当 u=v=0 时，F(u，v)的唯一最优值存在。
2.  **模式崩溃:**在这种情况下，Gan 无法捕获所有类别的数据，只能生成某些类型的模式。通常这种问题不会发生，但如果发生，可能会有问题。
3.  **梯度递减:**当鉴别器或生成器的梯度变得很小时，这种情况很常见，它几乎停止了另一个的学习。这主要发生在鉴别器的情况下，因为它变得更好，因此梯度变得如此之小，以至于生成器无法学习分布。为了避免这个问题，建议在实际训练整个 gan 之前不要训练鉴别器。

稳定 Gan 仍然是深度学习领域的一个活跃的研究领域，许多学术机构和组织都在从事这方面的工作。如果你有兴趣更多地了解与甘退房相关的当前问题，这是[的博客文章](https://medium.com/@jonathan_hui/gan-why-it-is-so-hard-to-train-generative-advisory-networks-819a86b3750b)，作者 [Jonathan hui](https://medium.com/@jonathan_hui) 。

# 应用程序

虽然 Gan 的建立牢记合成数据，并从本质上制作模型，以了解更多关于构成真实世界数据的表示，但不幸的是，Gan 的大多数当前使用并不友好，许多甚至是灾难性的，其中一个著名的例子是 [deep fakes](https://en.wikipedia.org/wiki/Deepfake) ，其想法是创建人说或做某事的假图像。这些企图会对社会和国家造成严重损害。尽管如此，gan 还是有一些很酷的应用，

1.  在网上服装行业，顾客可以在实际购买之前，通过试穿人工生成的虚拟版本来了解服装的外观。
2.  生成卡通人物，而不是依赖昂贵的软件。
3.  而不是依赖于工作室。一个经典的例子是最近中国国家新闻机构新华社上传了一段人工智能新闻主播而不是真人的报道。虽然他们没有透露他们的模式是什么，但我在这里采取了有根据的猜测。

# 摘要

生成对抗网络是目前深度学习中最有前途的技术之一。它们是建立在非常简单且感觉最普通的想法上的。我们的，似乎是甘的一个很好的实验，我会在我的博客里写更多的。完整的代码可以在[这里](https://github.com/Abhinavfreecodecamp/DCGAN)找到。

# 参考

甘的初始论文:[的](https://arxiv.org/abs/1406.2661)

DCGAN 论文:[https://arxiv.org/abs/1511.06434](https://arxiv.org/abs/1511.06434)

tensorlayer dcgan 实现:[https://github.com/tensorlayer/dcgan](https://github.com/tensorlayer/dcgan)

CNNVIS(可视化库):【https://github.com/InFoCusp/tf_cnnvis 

[open ai 的生成模型博客](https://blog.openai.com/generative-models/)

甘的问题:[https://medium . com/@ Jonathan _ hui/Gan-why-it-so-hard-training-generative-advisory-networks-819 a86b 3750 b](https://medium.com/@jonathan_hui/gan-why-it-is-so-hard-to-train-generative-advisory-networks-819a86b3750b)

[github/carpedm 20/DCGAN-tensor flow](https://github.com/carpedm20/DCGAN-tensorflow/blob/master/model.py)

[应用课程](https://appliedaicourse.com)

Glorot 初始化器:[andyl Jones . Tumblr . com/post/110998971763/an-explain-of-Xavier-initial ization](http://andyljones.tumblr.com/post/110998971763/an-explanation-of-xavier-initialization)

深度学习书籍:[www.deeplearningbook.org/](https://www.deeplearningbook.org/)