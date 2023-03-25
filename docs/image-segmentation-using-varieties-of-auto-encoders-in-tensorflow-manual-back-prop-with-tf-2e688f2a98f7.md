# 在 Tensorflow 中使用各种自动编码器进行图像分割

> 原文：<https://towardsdatascience.com/image-segmentation-using-varieties-of-auto-encoders-in-tensorflow-manual-back-prop-with-tf-2e688f2a98f7?source=collection_archive---------8----------------------->

![](img/d59b1f8d6586a0eb5488e7ea2de426f1.png)

GIF from this [website](https://giphy.com/gifs/neil-141amBdjqs9Vvy)

我想我需要练习表演分段，因此我发了这个帖子。我也想探索自动编码器的世界。下面是我想探究的案例。

***情况 1)普通全卷积自动编码器
情况 2)多损耗自动编码器
情况 3)重复全卷积自动编码器
情况 4)全卷积*** [***变分自动编码器***](/intuitively-understanding-variational-autoencoders-1bfe67eb5daf)

***情况 1)普通全卷积自动编码器***

![](img/5dc03caf4e2fbb35bb16255a14e96a65.png)![](img/ee3502c7d4aa85a989ea4a20a4c7acad.png)

**蓝框** →卷积层
**红框** →转置卷积层

现在，上述网络具有最简单的结构，其中输入是彩色图像，输出是分段的掩蔽图像。现在让我们看看这个网络的结果。

![](img/e15eecf5c7ca2c9f07f1b2644efe4d4e.png)![](img/29a91f8e2a2f2f25558939150e49bbb0.png)![](img/af28e408786eb315d2739cacd806bdaa.png)![](img/d384d27ae317a2efc7ff1e81ee629e4c.png)![](img/d6c57749672256004c8d584e380352d3.png)![](img/33f396c59223f52ac5d0f3da624e56b6.png)![](img/719d1a8e4407f5fae7cff84e31e14542.png)![](img/4d53de83537cdf6966d628ad2509a747.png)![](img/5f6dc29c92b1ee644f24b75dc44bfcdf.png)![](img/9f424143ef2f30f754e5bbe4da1a660f.png)![](img/5a46a4114abbbe814e938668e04bf02b.png)![](img/12c56765eeb8c4daaba5508ab6171af6.png)

总的来说，网络在检测图像中的物体方面做得不错，正如上面测试图像的结果所示。它实际上做了一个很好的基础网络，下面是为结果创建的 gif。

![](img/4a1c7498f3308047163caf46d2991923.png)![](img/cfed6cb592ec3cdff6d03fdeffdfc4a7.png)![](img/aa4c050ab6371449e8532df90ad2d834.png)

**左图 Gif** →为一段时间内的训练图像生成的遮罩
**右图 Gif** →为一段时间内的测试图像生成的遮罩
**右图** →训练期间的一段时间内的成本

***情况 2)多损耗自动编码器***

![](img/a2f566d7fd1f766c1e8d9ca39cbb6410.png)

Image from this [paper](https://arxiv.org/abs/1806.11266)

这次我的灵感来自于论文“用于由粗到细的密集语义图像标注的门控反馈细化网络”[](https://arxiv.org/abs/1806.11266)*，论文作者不仅引入了多损失函数，还引入了门控网络。在这里，我将只看到多重损失函数的影响。*

*![](img/e15eecf5c7ca2c9f07f1b2644efe4d4e.png)**![](img/6f4807ac0a3e9893a099c244027c7296.png)**![](img/2efe31030c3c5a4dad789b2ee4665436.png)**![](img/d384d27ae317a2efc7ff1e81ee629e4c.png)**![](img/045d54e03ec1a4383c264e069b9b6d24.png)**![](img/6af05808c6ce63a7096aeb0a003e841b.png)**![](img/719d1a8e4407f5fae7cff84e31e14542.png)**![](img/242f6fc9d2d1d81d3733f88dba7bf4f0.png)**![](img/69f4a9663cb0e428776d4e36fb828b03.png)**![](img/9f424143ef2f30f754e5bbe4da1a660f.png)**![](img/e54cdc453643afff560a1603a42a75c0.png)**![](img/569f15291a373fda4124bbdf3578bef5.png)*

*对于具有多重损失函数的网络，看起来图像的某些部分比基本网络更清晰。这个网络做得很好，特别是粉红色花的形象。*

*![](img/3484a2cc106e5935078a07d77f93f3ce.png)**![](img/d0afaad93939a9adf3f26ffd89e22356.png)**![](img/15508788bf8d9b8660481c581c5bad2b.png)*

***左图 Gif** →生成的训练图像随时间变化的蒙版
**右图 Gif** →生成的测试图像随时间变化的蒙版
**右图** →训练期间的成本随时间变化*

****情况 3)重复全卷积自动编码器****

*![](img/5c413e444637f856483134a746cb980b.png)**![](img/a804eb9ec2336dc1263bb4cd450f8062.png)*

***蓝框** →卷积层
**红框** →普通重复和卷积层*

*现在，这个网络的架构与之前的完全相同，但我只是想看看如果我们不使用转置卷积运算会发生什么。而只是天真地重复像素值两次，并执行卷积？*

*![](img/e15eecf5c7ca2c9f07f1b2644efe4d4e.png)**![](img/77338f4e49b12ee3e0823b75f0fd2989.png)**![](img/ae20640c5fb4f9de943aac37699f4a81.png)**![](img/d384d27ae317a2efc7ff1e81ee629e4c.png)**![](img/703545ae94cc365182ca84bd303280f7.png)**![](img/2d17dfe1ddaa0ddf98b94e9216984076.png)**![](img/719d1a8e4407f5fae7cff84e31e14542.png)**![](img/7d75634ef60fc0601727a3e576c09647.png)**![](img/b0378a7516545ab3a94560f4ac1e2d2b.png)**![](img/9f424143ef2f30f754e5bbe4da1a660f.png)**![](img/6659415bde6766282356cece79cb2c6b.png)**![](img/abc0d570875405b437e684b5c663d0e3.png)*

*如上所述，当我们仅使用卷积运算并天真地重复像素来执行上采样时，生成的遮罩是位清晰和平滑的。但是，我们可以在生成的蒙版中观察到一些随机的黑点。*

*![](img/eec5bba12ec0d816399b087faf2c325c.png)**![](img/75432a78d6de65463d659b7e55e86302.png)**![](img/799bb7be8d60c603c3c17e4377be7c65.png)*

***左图 Gif** →为一段时间内的训练图像生成的遮罩
**右图 Gif** →为一段时间内的测试图像生成的遮罩
**右图** →训练期间的一段时间内的成本*

****情况 4)全卷积*** [***变分自动编码器***](/intuitively-understanding-variational-autoencoders-1bfe67eb5daf)*

*![](img/280a24c542b2adb6d297702e5375a67d.png)*

*我从未见过任何用于分段目的的自动编码器的变化。(如果有人知道论文的名字请告诉我！)但我想使用它，下面是生成的面具。关于这个网络生成的掩码，我注意到的一件事是掩码中存在的噪声量，这可能是因为我们在训练期间从正态分布中抽取样本。*

*![](img/e15eecf5c7ca2c9f07f1b2644efe4d4e.png)**![](img/ec7486c54162006f571c9e8b4ddbb721.png)**![](img/55c7609c105318f337343a3a3836bd42.png)**![](img/d384d27ae317a2efc7ff1e81ee629e4c.png)**![](img/078ff5aa62002a8a5d6098b453a62cc7.png)**![](img/36d11c0397f6a91c1b0abc679ee0b6ed.png)**![](img/719d1a8e4407f5fae7cff84e31e14542.png)**![](img/8d7055faf868cb336c45abbe1c088555.png)**![](img/ee8a1ab811f1368fee62c97f763d311d.png)**![](img/9f424143ef2f30f754e5bbe4da1a660f.png)**![](img/7d83fdcb4ae7c38c036736c396e36299.png)**![](img/0d5d1795ae525313f274fd31f7405476.png)**![](img/25b1c7a90655a538b8398a37be519d93.png)**![](img/df3c7d58ad121f468979f586a9ca3d68.png)**![](img/36e609547d93fbb39c01a0b679549d3b.png)*

***左图 Gif** →为一段时间内的训练图像生成的遮罩
**右图 Gif** →为一段时间内的测试图像生成的遮罩
**右图** →训练期间的一段时间内的成本*

***互动码***

*![](img/537cb375ef8c16c1de7ca6c2282dc432.png)*

*对于 Google Colab，你需要一个 Google 帐户来查看代码，而且你不能在 Google Colab 中运行只读脚本，所以在你的操场上复制一份。最后，我永远不会请求允许访问你在 Google Drive 上的文件，仅供参考。编码快乐！*

*要访问[案例 a 的代码，请点击此处。](https://colab.research.google.com/drive/1CdAuZnJKmx9TQnSMns_DUE3MMMwKWlBT)
要访问[案例 b 的代码，请点击此处。](https://colab.research.google.com/drive/1y1vpxTSedxtZdnE8dNtNAz01B8nCd1Ye)
要访问[案例 c 的代码，请点击此处。](https://colab.research.google.com/drive/10K11Sjft_csE-oOU81u5dvVWGyodpQ0W)
要访问[案例 d 的代码，请点击此处](https://colab.research.google.com/drive/1qLa3iSlAzMxg9FerX5ooiFTHkx_6qLVV)。*

***最后的话***

*现在必须指出的是，所有这些模型都不能直接相互比较。超参数存在差异。但是我们仍然可以得到每个建筑的总体效果。*

*如果发现任何错误，请发电子邮件到 jae.duk.seo@gmail.com 给我，如果你想看我所有写作的列表，请点击这里查看我的网站。*

*同时，在我的推特[这里](https://twitter.com/JaeDukSeo)关注我，并访问[我的网站](https://jaedukseo.me/)，或我的 [Youtube 频道](https://www.youtube.com/c/JaeDukSeo)了解更多内容。我还实现了[广残网，请点击这里查看博文 pos](https://medium.com/@SeoJaeDuk/wide-residual-networks-with-interactive-code-5e190f8f25ec) t。*

***参考***

1.  *图像调整:转换图像内容—浏览 v0.15.dev0 文档。(2018).Scikit-image.org。检索于 2018 年 7 月 9 日，来自[http://sci kit-image . org/docs/dev/user _ guide/transforming _ image _ data . html](http://scikit-image.org/docs/dev/user_guide/transforming_image_data.html)*
2.  *伊斯兰，m .，罗昌，m .，那霸，s .，布鲁斯，n .，，王，Y. (2018)。用于由粗到细的稠密语义图像标注的门控反馈细化网络。Arxiv.org。检索于 2018 年 7 月 9 日，来自[https://arxiv.org/abs/1806.11266](https://arxiv.org/abs/1806.11266)*
3.  *直观地理解变分自动编码器-走向数据科学。(2018).走向数据科学。检索于 2018 年 7 月 9 日，来自[https://towards data science . com/直觉-理解-变分-自动编码器-1bfe67eb5daf](/intuitively-understanding-variational-autoencoders-1bfe67eb5daf)*
4.  *图片|张量流。(2018).张量流。检索于 2018 年 7 月 9 日，来自[https://www.tensorflow.org/api_guides/python/image](https://www.tensorflow.org/api_guides/python/image)*
5.  *tf.random_normal | TensorFlow。(2018).张量流。检索于 2018 年 7 月 9 日，来自[https://www.tensorflow.org/api_docs/python/tf/random_normal](https://www.tensorflow.org/api_docs/python/tf/random_normal)*