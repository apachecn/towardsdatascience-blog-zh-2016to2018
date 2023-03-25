# 数字海洋堆栈上的深度学习？还没有

> 原文：<https://towardsdatascience.com/deep-learning-on-the-digitalocean-stack-not-quite-yet-5c408e7d1a41?source=collection_archive---------3----------------------->

所以你想要一个更便宜的解决方案来运行你的深度学习代码。AWS 每月给你大约 1K 的账单，但你的业务逻辑真的需要深度学习的魔力。更糟糕的是，你不能仅仅调用一个 API 就让它消失。不，不。你处理的数据太多了。但是你喜欢云。你想待在云中。让我们一起踏上尝试新事物的旅程。

![](img/cfd8256dbf1eff8c93c7d9cda8c52ac5.png)

Docker 是一种虚拟化(特别是容器化)技术。docker 上有很多很棒的程序可以运行。非常棒，但是记住 DigitalOcean 仍然没有 GPU 支持，所以在这些容器中的机器学习将会非常慢。

也许如果我们在一个[高 CPU 水滴](https://www.digitalocean.com/products/compute/high-cpu/)上运行我们的深度学习代码，性能会足够好，价格会更低？我记得看过一篇文章，说 DigitalOcean CPU 实例在一些工作负载上可以在成本和性能上打败 AWS。也许是 word 嵌入模型工作得很好。我记不清了，也找不到文章了…那我们去了解一下吧！科学！

首先让我们看看成本。今天(2017 年 8 月 10 日)AWS 上的一个 [p2.xlarge 费用为 0.900 美元/小时。它拥有惊人的 K80 GPU。DigitalOcean 中的 32 CPU“高 CPU”水滴费用为 0.952 美元/小时。这台机器有 48 GB 内存，适合我们想要的任何单词嵌入模型。系统也有 SSD。事实上，数字海洋只是固态硬盘。我喜欢它。让我们在两个平台上运行诗人的 TensorFlow，看看我们在执行时间方面的表现如何。](https://aws.amazon.com/ec2/instance-types/p2/)

DigitalOcean droplet 是从 Docker 的一次点击安装开始的。这加速了张量流设置。当它旋转起来的时候，跑上去就很有趣，感觉所有这些 CPU 的原始力量就像某种自大狂巫师一样。好的。回去工作。TensorFlow 安装在 Docker 上又快又好。“你好，世界！”验证脚本有效。

在指令中，我们跳过了“[我赶时间](https://codelabs.developers.google.com/codelabs/tensorflow-for-poets/#4)”这一部分，因为我们真的想处理一些数字。瓶颈文件的创建速度比在 p2 实例上慢一些。我可以告诉你，我过去经常这么做。结果将是不可思议的。让我们通过运行来量化:

```
time python retrain.py  \   
--bottleneck_dir=bottlenecks   \ 
--how_many_training_steps=500    \
--model_dir=inception    \
--summaries_dir=training_summaries/basic  \  
--output_graph=retrained_graph.pb    \
--output_labels=retrained_labels.txt    \
--image_dir=flower_photos
```

高 CPU DigitalOcean 虚拟机上的结果是:

```
2017-08-11 00:01:07.773065: Step 480: Train accuracy = 92.0%
2017-08-11 00:01:07.773155: Step 480: Cross entropy = 0.345928
2017-08-11 00:01:07.825522: Step 480: Validation accuracy = 88.0% (N=100)
2017-08-11 00:01:08.424932: Step 490: Train accuracy = 91.0%
2017-08-11 00:01:08.425017: Step 490: Cross entropy = 0.354291
2017-08-11 00:01:08.477905: Step 490: Validation accuracy = 90.0% (N=100)
2017-08-11 00:01:09.032948: Step 499: Train accuracy = 94.0%
2017-08-11 00:01:09.033039: Step 499: Cross entropy = 0.262198
2017-08-11 00:01:09.084237: Step 499: Validation accuracy = 90.0% (N=100)
Final test accuracy = 89.8% (N=353)
Converted 2 variables to const ops.**real    6m42.118s**
user    86m49.452s
sys     30m17.000s
```

因此，在不到 7 分钟的时间内(6*60+42 = 360+42 = 402 秒)，我们生成了重新训练 CNN 最后一层的瓶颈文件，以获得大约 90%的分类准确率。

让我们在 AWS 上运行同样的测试。在 AWS 上用 p2 在 Docker 之外运行要快得多。对于相同的命令和相同的数据集，我得到了以下时间:

```
2017-08-11 00:02:32.848994: Step 480: Train accuracy = 94.0%
2017-08-11 00:02:32.849077: Step 480: Cross entropy = 0.273793
2017-08-11 00:02:32.907405: Step 480: Validation accuracy = 85.0% (N=100)
2017-08-11 00:02:34.094489: Step 490: Train accuracy = 87.0%
2017-08-11 00:02:34.094567: Step 490: Cross entropy = 0.433323
2017-08-11 00:02:34.153172: Step 490: Validation accuracy = 88.0% (N=100)
2017-08-11 00:02:34.694696: Step 499: Train accuracy = 90.0%
2017-08-11 00:02:34.694771: Step 499: Cross entropy = 0.370968
2017-08-11 00:02:34.753167: Step 499: Validation accuracy = 94.0% (N=100)
Final test accuracy = 89.8% (N=353)
Converted 2 variables to const ops.**real    4m10.831s**
user    3m27.290s
sys     0m29.820s
```

相同的结果，但只是超过 4 分钟，而不是不到 7 分钟(4*60+11 = 240+11 = 251)。考虑到 DigitalOcean droplet 每小时比 AWS 实例贵一点，我认为这里有一个明显的赢家。**AWS 的加速比是**的 1.6 倍(402/251)，而**的成本减少了 5.7%**(0.952 比 0.900)。坚持用 AWS 做你的 CNN 工作，把数字海洋水滴留给非 GPU /更小的东西。

坚持住！我找到文章了！

[](http://minimaxir.com/2017/07/cpu-or-gpu/) [## 云 CPU 上的 TensorFlow 基准测试:比云 GPU 更便宜的深度学习

### 我一直在与 Keras 和 TensorFlow 合作几个个人深度学习项目。然而，培训模式的深度…

minimaxir.com](http://minimaxir.com/2017/07/cpu-or-gpu/) 

那为什么他们说牛逼我说扯淡？让我们开始吧！

1.  本文作者 Max Woolf 从源代码中编译了 tensorflow，以利用 AVX 和 SSE 指令。懒惰的我，我没有那样做。接得好。
2.  他的工作量是 MLP 的 MNIST 分类，然后是 CNN，还有其他东西，而我的工作量是《盗梦空间 v3》的 CNN。
3.  总的来说，他有点同意我的结果，CPU 在运行时间方面与 GPU 不匹配。他运行的每一个 CPU 测试都比 GPU 慢。
4.  对于成本比较，我们不同意。他使用了[谷歌的超级 CPU 怪兽](https://cloudplatform.googleblog.com/2017/05/Compute-Engine-machine-types-with-up-to-64-vCPUs-now-ready-for-your-production-workloads.html)，定价为 0.509 美元/小时，GPU 定价为 0.745 美元/小时。这远远低于 DigitalOcean 的高 CPU 0.952 美元/小时和 AWS 的 p2 实例 0.900 美元/小时。平心而论，我们可以将我们的 p2 降级为 g 2.2x 大，并从那里继续下滑，而不是尖叫着跑到数字海洋。是的，给谷歌云平台一个旋转。关于 GCP 的单位工作成本，马克斯说得很有道理。

所以，最后我们都是对的。你可以在 CPU 上做 ML，但是要确保它确实能为你省钱，因为它肯定不会为你节省任何时间。

希望这对于正在考虑更便宜的机器学习选项的解决方案架构师来说是一个很好的比较。如果你喜欢这篇文章，那么请推荐它，分享它，或者给它一些爱(❤).

编码快乐！

-丹尼尔
[丹尼尔@lemay.ai](mailto:daniel@lemay.ai) ←打个招呼。
[LEMAY . AI](https://lemay.ai)
1(855)LEMAY-AI

您可能喜欢的其他文章:

*   [人工智能和不良数据](/artificial-intelligence-and-bad-data-fbf2564c541a)
*   [人工智能:超参数](/artificial-intelligence-hyperparameters-48fa29daa516)
*   [人工智能:让你的用户给你的数据贴上标签](https://medium.com/towards-data-science/artificial-intelligence-get-your-users-to-label-your-data-b5fa7c0c9e00)