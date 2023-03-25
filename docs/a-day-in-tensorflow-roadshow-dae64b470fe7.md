# TensorFlow 路演的一天

> 原文：<https://towardsdatascience.com/a-day-in-tensorflow-roadshow-dae64b470fe7?source=collection_archive---------5----------------------->

![](img/d483a32c58a0952e9110159fc0fa0537.png)

**TensorFlow Roadshow Bangalore**

TensorFlow 最初是由谷歌大脑团队的研究人员开发的内部项目，现在已经成为 2018 年最受欢迎的软件库( [Stack Overflow 的 2018 年调查](https://insights.stackoverflow.com/survey/2018/#technology-most-loved-dreaded-and-wanted-frameworks-libraries-and-tools))。谷歌在 2015 年将 TensorFlow 开源，从那以后，它一直在不断发展，在社区方面也在增长。这是一个令人鼓舞的因素，不仅让谷歌变得更好，让更多的研究人员可以访问，也让机器学习社区尝试这个库，并用它来建立模型，从而解决了许多关键问题。关注社区的反馈确实对 TensorFlow 有所帮助，TensorFlow 团队一直在以多种方式与社区互动。昨天，TensorFlow 团队在班加罗尔为机器学习社区组织了一次路演，并进行了精彩的会议。

此次路演出席人数众多，谷歌开发人员关系主管 Karthik Padmanabhan 对开发人员社区表示欢迎并致辞。在过去的几年里，机器学习，更具体地说是深度学习，改变了许多关于技术如何影响人类生活的事情。深度学习的进展导致了计算机视觉和 NLP 相关任务的快速发展。在所有这些进步的帮助下，ML 使许多以前想都不敢想的事情成为可能。TensorFlow 作为一个帮助构建机器学习解决方案的库，正在通过让研究人员将他们解决问题的想法转化为现实来推动机器学习向前发展。Dushyantsinh Jadeja 开始了当天的第一场会议，强调了 ML 在我们生活中的不可避免性，以及 TensorFlow 如何帮助构建了一些改变生活的解决方案。

![](img/926dc24161a916b90d6dc514affb99e2.png)

[Pic Credit](https://twitter.com/GoogleDevsIN)

TensorFlow 用于解决许多问题，如在天文学中检测新的太阳系和行星，通过使用传感器进行实时检测来避免砍伐森林，并自动警告任何非法活动，评估谷物质量，更好地帮助农民和农业，最积极的是，在医学领域帮助医生做出更好更快的决定。作为一个有潜力解决这类问题的库，TensorFlow 成为 ML 使用最多的库之一并不奇怪。

TensorFlow 的项目经理 Sandeep Gupta 在当天的第一次演讲中简要介绍了一些主要的和最常用的 TensorFlow 模块和功能。TensorFlow 提供高级 API，如 Estimators、tf.data 和非常受欢迎的 Keras API，使构建机器学习模型变得简单快捷。保存的 TensorFlow 机器学习模型可以使用 TFLite 转换并部署到移动设备上。TF.js 使 JavaScript 开发人员能够在浏览器本身利用机器学习。Tensor2Tensor 是一个独立的库，提供现成的数据集和模型。当天晚些时候，我们进行了非常有用的讨论，深入了解了一些有用的工具和库的细节。虽然 TensorFlow 到目前为止有超过 1600 名贡献者，但他们中的大多数人都来自谷歌之外！

![](img/3c8b8090b2cef487603e49dbe31e6709.png)

**Using TensorFlow to Prototype and train models**

建立机器学习模型来解决问题包括几个步骤，从数据收集开始，直到保存和部署训练好的模型。Amit Patankar 使用一个心脏病数据集来快速带领我们完成建立机器学习模型的不同步骤。在这样做的同时，他还展示了评估者如何通过提供可以尝试和评估的不同模型来使这一过程变得更容易和更好。这时是沃尔夫·多布森上台的时候了。为了在 TensorFlow 中编码和运行任何东西，首先需要创建一个包含所有必要组件的图，然后从一个会话中运行该图。虽然这种执行模式有其自身的优点，如更好的优化、简化的分布式培训和部署等，但这种方法可能会有点乏味，而且对初学者不太友好。与此相反的是急切执行模式，在这种模式下，执行会立即发生，不需要会话。对于初学者和研究人员来说，这听起来要好得多，可以对他们的模型进行原型化和编码。但是这里的权衡正在失去前一种方法带来的优势。如果我们能获得两种方法的优点，那不是很好吗？这就是亲笔签名的作用！

![](img/5816f37f47b0b0619e0628819b9f6ea6.png)

**AutoGraph —** [**Credit**](https://medium.com/tensorflow/autograph-converts-python-into-tensorflow-graphs-b2a871f87ec7)

AutoGraph 以 eager 模式获取代码，并将其转换为等价的图形。上面的图片是一个例子，它展示了代码在两种模式下是多么的不同，以及 AutoGraph 是多么的有用！通过使用 AutoGraph，可以在 eager 模式下构建原型，在 Graph 模式下部署。Wolff Dobson 还谈到了将于明年发布的 TensorFlow 2.0 正在进行的工作，并要求社区通过 RFCs(征求意见)流程参与设计决策。

接下来的两个演讲是关于使用分布式计算和加速器的 TensorFlow。Priya Gupta 谈到了分布式张量流。根据使用的数据集、选择的架构和其他一些因素，训练机器学习模型可能需要几分钟到几天或几周的时间。与非分布式模式相比，分布式培训有助于大幅减少培训时间。在这种模式下，集群(一组工作器/处理器)用于图形执行，而不是单个处理单元。这不仅导致更少的训练时间，而且导致更好的性能，这可以通过最小的代码改变来实现。TPU 是张量处理单元，具有强大的计算能力。TPUs 与分布式 TensorFlow 一起为构建深度学习模型提供了很好的结果。在他的演讲中，Sourabh Bajaj 解释了快速发展的深度学习方法的计算需求如何导致 TPU 的设计和开发。

![](img/b33fe0134ee69bc83e372f60f65cb120.png)

**Distributed TensorFlow**

在建立机器学习模型时，除了代码之外，还有许多事情需要考虑和担心。收集数据，为模型训练做准备是一个耗时的过程。由于模型的性能和准确性高度依赖于用于训练的数据，因此数据验证和转换非常重要。在高层次上，机器学习模型获取数据并从中学习，以输出可用于推理的训练模型，将此用于生产涉及许多约束和要求。TensorFlow 不仅专注于构建和部署模型，还在增加对机器学习管道其他部分的支持，TensorFlow Extended (TFX)正是这样做的。Kenny Song 通过一系列幻灯片解释了 TFX 是什么以及它的作用。TFX 是一个端到端的 ML 平台，它提供库来验证和分析训练和服务的不同阶段的数据，分析不同数据切片的训练模型等。他还谈到了 TF Hub，这是一个可重用机器学习模块的存储库。模块包含 TF 图及其为某些特定任务训练的学习权重。这些模块可以按原样用于相同的任务，也可以通过一些修改来重用，以解决类似的其他任务。

![](img/8e815007ec9c30787f77d07a0aae3b4c.png)

**TFX**

仅在几年前，没有多少人会想到在浏览器中训练和运行机器学习模型。但这现在已经成为现实，这要归功于 TensorFlow.js. Sandeep Gupta 就此发表了另一篇演讲，他分享了 TensorFlow Playground 如何为 TensorFlow.js 铺平了道路。Playground 是一种交互式神经网络可视化工具，旨在从浏览器运行。开发这个工具的团队必须做大量的工作，因为移植这个机器学习工具以从浏览器运行涉及一些障碍和限制。在这样做的同时，他们意识到了将机器学习引入浏览器的潜力。从浏览器运行 ML 模型有很多好处——不需要安装驱动程序，它是交互式的，与传感器连接良好，数据与用户在一起。所有这些导致了 TensorFlow.js，它是一个用于在浏览器中构建和部署 ML 模型的 JavaScript 库。对于一些可以在你的浏览器上运行的很酷的 ML 项目，请看博客末尾的链接。

![](img/29dde9348c676af610ab5fcf450a3d4c.png)

**Edge TPU —** [**Credit**](https://cloud.google.com/edge-tpu/)

如果模型能在高端设备上高效运行，这还不够。随着手机成为最常用的设备，让 ML 模型尽可能高效地在低端设备上运行的探索也取得了一些成果。软件工程师 Andrew Selle 是 TensorFlow 团队的一员，他带我们了解了 TensorFlow Lite，该软件使移动和嵌入式设备能够运行 ML 模型。直接从设备运行 ML 模型有很多优点。由于不需要联系任何服务器来进行推断，移动 ML 导致了更低的延迟，并且不依赖于数据连接。在这种情况下，数据完全不在设备上，因此避免了任何隐私问题。但这是一项具有挑战性的任务，因为手机和嵌入式设备内存紧张，能耗低，计算能力差。TFLite 使这成为可能，并且已经被许多应用程序用于生产。边缘 TPU 是 TPU 的变体，开发用于为移动通信的边缘设备供电。

![](img/a3e96be19d7fd469a8cdc5a4e93b705c.png)

**TensorFlow for art and music**

随着 ML 在几乎每个领域都找到了自己的位置，艺术家们也投身其中。使用 ML 来提出某种艺术形式已经成为一种日益增长的兴趣。ML 被用来创作自己的绘画、作品、音乐等。Magenta 是一个 TensorFlow 项目，用于使用机器学习生成艺术和音乐。沃尔夫·多布森再次登上了舞台，最后一次讲述了这神奇的一天。他向我们展示了使用 TensorFlow 构建的一些播放音乐和游戏的应用程序的演示。

成为技术的一部分是一个激动人心的时刻。像这样的会议可以提供很多东西，通过参加这样的社区活动，人们可以了解和学习到很多东西，这是令人惊讶的。除了非常有用的谈话，见到 TensorFlow 团队和其他 ML 研究人员真的很高兴。感谢整个谷歌印度团队为社区组织了许多这样的精彩活动！

谷歌为任何对 ML 感兴趣的人免费提供许多资源，从初级速成课程到研究级博客。我写了一个博客，上面有很多这样有用的资源，你可以在这里找到——[谷歌知道如何教](/google-knows-how-to-teach-45e531ab3ada)。

> **你觉得这篇文章有用吗？欢迎留下任何反馈/意见。感谢阅读！！**

***连接:***[***LinkedIn***](https://www.linkedin.com/in/avinash-kappa/)***，***[***Twitter***](http://twitter.com/avinashso13)***和我的*** [***博客******。***](https://theimgclist.github.io/)

# **有用链接**:

[](https://medium.com/tensorflow/autograph-converts-python-into-tensorflow-graphs-b2a871f87ec7) [## AutoGraph 将 Python 转换为张量流图

### 亚历克斯·维尔奇科，丹·摩尔多瓦，沃尔夫·多布森

medium.com](https://medium.com/tensorflow/autograph-converts-python-into-tensorflow-graphs-b2a871f87ec7) [](https://teachablemachine.withgoogle.com/) [## 可教机器

### 可教机器:探索机器学习，活在你的浏览器里。

teachablemachine.withgoogle.com](https://teachablemachine.withgoogle.com/) [](https://www.metacar-project.com/) [## Metacar:浏览器中自动驾驶汽车的强化学习环境。

### 算法:Q-用表学习在这个例子中，目标是演示一个简单的 a…

www.metacar-project.com](https://www.metacar-project.com/) [](https://magenta.tensorflow.org/sketch-rnn-demo) [## 用神经网络绘制在一起

### 更新(2018 . 8 . 2):sketch-rnn 已经移植到 Magenta.js 项目下的 TensorFlow.js！看一看…

magenta.tensorflow.org](https://magenta.tensorflow.org/sketch-rnn-demo) [](https://magenta.tensorflow.org/music-vae) [## MusicVAE:使用机器学习创建乐谱调色板。

### 当一个画家创作一件艺术品时，她首先在艺术家的调色板上混合和探索颜色选项，然后…

magenta.tensorflow.org](https://magenta.tensorflow.org/music-vae) [](/google-knows-how-to-teach-45e531ab3ada) [## 谷歌知道如何教学

### “够了够了！我受够了！”我脑袋里面的声音就是这样磨了几个小时才向我吼的…

towardsdatascience.com](/google-knows-how-to-teach-45e531ab3ada) [](https://magenta.tensorflow.org/blog/2018/05/03/connecting-with-magenta-js/) [## 通过 Magenta.js 连接音乐

### 编者按:在过去的几个月里，我们注意到许多由 creative.ai 的 Tero Parviainen 开发的令人印象深刻的应用程序…

magenta.tensorflow.org](https://magenta.tensorflow.org/blog/2018/05/03/connecting-with-magenta-js/) 

tensor flow Hub—[https://tfhub.dev/](https://tfhub.dev/)

https://dl.acm.org/citation.cfm?id=3098021 TFX 纸—

TensorFlow.js 图库—[https://github.com/tensorflow/tfjs/blob/master/GALLERY.md](https://github.com/tensorflow/tfjs/blob/master/GALLERY.md)