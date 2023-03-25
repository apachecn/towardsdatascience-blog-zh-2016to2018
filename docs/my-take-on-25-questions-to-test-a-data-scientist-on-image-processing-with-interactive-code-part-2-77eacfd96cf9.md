# 我用 25 个问题来测试一个数据科学家对交互式代码的图像处理-第 2 部分

> 原文：<https://towardsdatascience.com/my-take-on-25-questions-to-test-a-data-scientist-on-image-processing-with-interactive-code-part-2-77eacfd96cf9?source=collection_archive---------4----------------------->

![](img/b621a279b1dec6c915ec3282d29ad395.png)

GIF from this [website](https://giphy.com/gifs/tech-facial-recognition-computer-vision-BD3GmP2qpDCX6/download)

所以我找到了这个了不起的博主 [Faizan Shaikh](https://www.analyticsvidhya.com/blog/2017/10/image-skilltest/) ，他的博客上有一些令人惊奇的东西！所以请你查一查他，也就是他是“ [*25 题测试一个数据科学家对图像处理的*](https://www.analyticsvidhya.com/blog/2017/10/image-skilltest/) ”的创作者。所以我会试着挑战自己去解决它们，这是第二部分！请注意，我的解决方案不会优化。

此外，对于每个问题，我会截屏正确的答案(原作者张贴的答案)。然而， ***我总是乐于学习和成长*** ，所以如果你知道一个更好的解决方案，请在下面评论。

还有，这是我用来做演示的两张图片。
1。 [*脑 MRI 来自 pixabay*](https://pixabay.com/en/mri-magnetic-x-ray-skull-head-782457/) *2。* [*自然图像来自 pixabay*](https://pixabay.com/en/sunset-lighthouse-dawn-dusk-sun-3120484/)

如果你想看第一部分，[请点击这里](/my-take-on-25-questions-to-test-a-data-scientist-on-image-processing-with-interactive-code-part-1-a6196f535008)。

**11)【是非】JPEG 是一种有损图像压缩技术**

![](img/d4d3feaeaa73820dc3da12b97adaf763.png)

对于这个问题，我不仅好奇 JPEG 是什么，也好奇 PNG 图像格式是什么，所以我决定去查一下。

![](img/cc2970782da46aab3922d543e11c8939.png)

Image from this [website](https://en.wikipedia.org/wiki/JPEG)

![](img/fc319b1f42099a6ac76864e1ed8c4619.png)

Image from this [website](https://en.wikipedia.org/wiki/Portable_Network_Graphics)

所以从上面的图片，我们可以得出两个结论。JPEG 是有损压缩，PNG 是无损压缩。因此答案是正确的。

![](img/216eed7c9836859bab532e1614fa20d6.png)

**12)给定一幅只有 2 个像素，每个像素有 3 个可能值的图像，可以形成多少个可能的图像直方图？**

![](img/39f4749ffee20ed74a7ffb6eda734868.png)

我真的不知道这个问题，但从逻辑上来说。我们有 2 个像素，每个像素都有 RGB 通道。因此，我们已经知道有 3 * 3 = 9 种可能配置组合。但是顺序对于直方图来说并不重要，重要的是像素强度的频率。因此 3 + 3 = 6。(作者和我不同意这一条，如果我错了请告诉我。)

![](img/42e20320dd5fa5b2f606ba5c89893c55.png)

**13) 1D 平均卷积**

![](img/adba96dd83e791b8bd05273f9ca32dad.png)

假设上面的 1D 图像没有填充，我们可以简单地计算一下。(8+5+2)/3 = 15/3 = 5.因此倒数第二个像素值将保持不变。

![](img/4a660db22aea08ffa343139080a360fc.png)

**14) FMRI 问题(脑部核磁共振)**

![](img/63b9417f98821b5d0bbe3890ff4395ba.png)

首先，我需要知道什么是 FMRI，所以我做了一个简单的谷歌搜索。

![](img/36fd2cea17359d03d33ee60d71796347.png)

Image from this [website](https://en.wikipedia.org/wiki/Functional_magnetic_resonance_imaging)

FMRI 似乎是一种测量大脑活动的简单技术。现在让我们看看这个 FMRI 的输出。

![](img/7f52b8bc15d699070ae2294b9e08653f.png)

Image from this [website](https://med.nyu.edu/thesenlab/research-0/research-functional-magnetic-resonance-imaging-fmri/)

我试图找到对 FMRI 输出的解释，但是我不能找到确切的答案。但我找到了一个实验对象的功能磁共振成像结果。它似乎产生了 2D 输出。(然而我的答案是错的。)

![](img/15dd18e22d804109ff14fb5134b7c0e9.png)

**15)以下哪种方法被用作边缘检测的模型拟合方法？**

![](img/0cb107c82eaad5f23dde3f3c5787bda3.png)

为了这个问题，我必须复习很多东西。从什么是模型拟合算法开始。

![](img/d8d5429f3acc5572cf5f81f91fd85d42.png)

GIF from this [website](https://en.wikipedia.org/wiki/Curve_fitting)

从简单的谷歌搜索我们已经可以知道模型拟合算法做什么，在大多数情况下，它看起来像函数逼近。现在让我们回到这个问题，SIFT 是一个特征检测器，Gaussain 检测器可以理解为斑点检测。因此，答案必须是 RANSAC。

![](img/50f1ffe2722ae46db57e3a79a37eeb6a.png)

**16)中值滤波器**

![](img/014bb7bb96601bc2a2fa1bb405da0bbf.png)

我不会说中值滤波是最好的方法，因为这要视情况而定，但是正如我们在下面看到的，它确实可以做到。让我们使用来自[栈溢出帖子](https://stackoverflow.com/questions/22937589/how-to-add-noise-gaussian-salt-and-pepper-etc-to-image-in-python-with-opencv?rq=)的这段代码来添加一些 s [alt 和 pepper 噪声](https://en.wikipedia.org/wiki/Salt-and-pepper_noise)并使用中值滤波器去噪。

![](img/aff482f2403decdb96e527083738c289.png)![](img/bc68e6e0a6c1fa45590390a9cae85edd.png)![](img/023620a55e0fc3293a9e8758248914c5.png)![](img/c197217d7989646e5c33f7eb0758bba6.png)![](img/cedffedefee0856373012fa0558fa01d.png)

**左图** →有椒盐噪声的图像
**右图** →中值滤波后的图像

我们可以清楚地看到中值滤波器的出色表现。

**17)卷积运算**

![](img/d7553f6ff8f08d0840b704c53f838a17.png)

对于这个问题我们需要回顾一下，卷积运算之前内核发生了什么。

![](img/5748b27fa8e22c99dbf50303cdf49b6c.png)

Image from this [website](http://www.songho.ca/dsp/convolution/convolution2d_example.html)

从这个网站上，我们可以清楚地看到一个卷积运算会发生什么。在应用卷积运算之前，我们将内核翻转 180 度。因此，我们可以得出结论，图像将向右移动，而不是向左。

![](img/7c79f941107330f7553885ae9ef18211.png)

**18)图像锐化**

![](img/bcb07ddd1a0e62aa5d6e809ecaaaa133.png)

对于这个问题，我们可以清楚地知道选项 A 不是这种情况，因为从应用身份图像中减去结果图像将导致黑色图像。但是让我们试一试。

![](img/d82ab48292374a4c28331ff630c05b0a.png)![](img/9be22b539cc58e3663fa147e520dbd7e.png)![](img/d82ab48292374a4c28331ff630c05b0a.png)![](img/3231e7f8b92ce4bec152aaa73c392e5e.png)![](img/bbf5c56de8599f149884202f2e04d92c.png)![](img/3231e7f8b92ce4bec152aaa73c392e5e.png)

**左图** →原图
**中图** →中间结果图
**右图** →结果图

上面的图像是按照选项 A 的步骤生成的，正如我们所看到的，没有任何区别。

![](img/d82ab48292374a4c28331ff630c05b0a.png)![](img/6be67fa81b65922090202d2becccda5f.png)![](img/55c700021f55cd2779f4f422b0a9f990.png)![](img/3231e7f8b92ce4bec152aaa73c392e5e.png)![](img/f08999ec353e7c0bec7e3cd3b897a11e.png)![](img/615ab19f0690b9da8552a9dccccd9139.png)

**左图像** →原始图像
**中间图像** →中间结果图像
**右图像** →结果图像

上面的图像是按照选项 B 的步骤生成的，我们可以看到图像中有一些奇怪的伪像，但是从中间生成的图像中，我们可以看到我们正在给原始图像添加边缘。

![](img/11baa7accefd538a0a38985cc2dedc8b.png)![](img/cade6ab4add78f87b96bb231725075eb.png)![](img/4c5dd690fe788846bda716c09536d4c8.png)![](img/bcab8523166651cd6377b3e3e88e41d2.png)

**左图** →原图
右图 →结果图

上面的图像是按照选项 C 的步骤生成的，我们可以看到图像中有一些奇怪的伪像，结果图像看起来没有那么清晰。所以答案会是选项 b。

![](img/43cbbcc9b90f92a1c3a214ced120f5db.png)

**19)信号卷积**

![](img/017e83f04db91e2dd904a69bd85f04e4.png)

对于这个问题，维基页面实际上在首页有直接的答案。

![](img/1ecb1535249becae453920802a098f1c.png)

Image from this [website](https://en.wikipedia.org/wiki/Cross-correlation)

因此，从上图中，我们可以得出结论，运算 1 是卷积运算，运算 2 是互相关运算。因此答案是 b。

Video from this [website](https://www.youtube.com/watch?v=O9-HN-yzsFQ)

上面是一个视频，真实的例子。(然而，作者和我似乎又不同意，如果我错了，请让我知道。)

![](img/bf634caf9b161e4d90f2b92c8738e1c9.png)

**交互代码**

![](img/c08c7c9f5fad890aadd924b5aaa26116.png)

对于 Google Colab，你需要一个 Google 帐户来查看代码，而且你不能在 Google Colab 中运行只读脚本，所以在你的游戏场地上复制一份。最后，我永远不会请求允许访问你在 Google Drive 上的文件，仅供参考。编码快乐！

请注意，我不想滥用任何数据政策。所以对于谷歌 Colab，我找到了下面的图片，它被标记为重复使用。要访问在线代码，请点击此处。

![](img/f13728d2cd2a37b5c5f8dc55221dd604.png)

Image from this [website](https://upload.wikimedia.org/wikipedia/commons/thumb/c/cb/Felix_Titling_sample.png/1280px-Felix_Titling_sample.png)

**最后的话**

在我的下一篇文章中，我会回答所有的问题。敬请关注！

如果发现任何错误，请发电子邮件到 jae.duk.seo@gmail.com 给我，如果你想看我所有写作的列表，请在这里查看我的网站。

与此同时，请在我的推特[这里](https://twitter.com/JaeDukSeo)关注我，并访问[我的网站](https://jaedukseo.me/)，或我的 [Youtube 频道](https://www.youtube.com/c/JaeDukSeo)了解更多内容。如果你感兴趣的话，我还做了解耦神经网络[的比较。](https://becominghuman.ai/only-numpy-implementing-and-comparing-combination-of-google-brains-decoupled-neural-interfaces-6712e758c1af)

**参考**

1.  “JPEG”。2018.En.Wikipedia.Org。2018 年 4 月 11 日访问。[https://en.wikipedia.org/wiki/JPEG](https://en.wikipedia.org/wiki/JPEG)
2.  “便携式网络图形”。2018.En.Wikipedia.Org。2018 年 4 月 11 日访问。[https://en.wikipedia.org/wiki/Portable_Network_Graphics](https://en.wikipedia.org/wiki/Portable_Network_Graphics)。
3.  Pixabay 上的免费图像——核磁共振成像，磁性，x 光，头骨，头部。(2018).Pixabay.com。检索于 2018 年 4 月 9 日，来自[https://pix abay . com/en/MRI-magnetic-x-ray-skull-head-782457/](https://pixabay.com/en/mri-magnetic-x-ray-skull-head-782457/)
4.  Pixabay 上的免费图像——日落、灯塔、黎明、黄昏、太阳。(2018).Pixabay.com。检索于 2018 年 4 月 9 日，来自[https://pix abay . com/en/sunset-light house-dawn-dusk-sun-3120484/](https://pixabay.com/en/sunset-lighthouse-dawn-dusk-sun-3120484/)
5.  Pixabay 上的免费图像——核磁共振成像，磁性，x 光，头骨，头部。(2018).Pixabay.com。检索于 2018 年 4 月 9 日，来自[https://pix abay . com/en/MRI-magnetic-x-ray-skull-head-782459/](https://pixabay.com/en/mri-magnetic-x-ray-skull-head-782459/)
6.  功能磁共振成像(fMRI)——NYU 认知神经生理学实验室。(2018).NYU 认知神经生理学实验室。检索于 2018 年 4 月 11 日，来自[https://med . NYU . edu/thesenlab/research-0/research-functional-magnetic-resonance-imaging-fmri/](https://med.nyu.edu/thesenlab/research-0/research-functional-magnetic-resonance-imaging-fmri/)
7.  [复本]，H. (2018)。如何用 OpenCV 在 Python 中给图像添加噪声(高斯/椒盐等)。Stackoverflow.com。检索于 2018 年 4 月 11 日，来自[https://stack overflow . com/questions/22937589/how-to-add-noise-Gaussian-salt-and-pepper-etc-to-image-in-python-with-opencv？rq=1](https://stackoverflow.com/questions/22937589/how-to-add-noise-gaussian-salt-and-pepper-etc-to-image-in-python-with-opencv?rq=1)
8.  椒盐噪声。(2018).En.wikipedia.org。检索于 2018 年 4 月 11 日，来自[https://en.wikipedia.org/wiki/Salt-and-pepper_noise](https://en.wikipedia.org/wiki/Salt-and-pepper_noise)
9.  我用 25 个问题来测试一个数据科学家关于交互式代码的图像处理-第 1 部分。(2018).走向数据科学。检索于 2018 年 4 月 11 日，来自[https://towards data science . com/my-take-on-25-questions-to-test-a-data-scientist-on-image-processing-with-interactive-code-part-1-a 6196 f 535008](/my-take-on-25-questions-to-test-a-data-scientist-on-image-processing-with-interactive-code-part-1-a6196f535008)
10.  安，S. (2018)。2D 卷积的例子。Songho.ca 于 2018 年 4 月 11 日检索，来自[http://www . song ho . ca/DSP/convolution/convolution 2d _ example . html](http://www.songho.ca/dsp/convolution/convolution2d_example.html)
11.  OpenCV:平滑图像。(2018).Docs.opencv.org。检索于 2018 年 4 月 11 日，来自[https://docs . opencv . org/3 . 1 . 0/D4/d13/tutorial _ py _ filtering . html](https://docs.opencv.org/3.1.0/d4/d13/tutorial_py_filtering.html)
12.  用 Matlab 实现互相关和卷积的可视化(已编辑)。(2018).YouTube。检索于 2018 年 4 月 11 日，来自 https://www.youtube.com/watch?v=O9-HN-yzsFQ
13.  交叉相关。(2018).En.wikipedia.org。检索于 2018 年 4 月 11 日，来自[https://en.wikipedia.org/wiki/Cross-correlation](https://en.wikipedia.org/wiki/Cross-correlation)
14.  曲线拟合。(2018).En.wikipedia.org。检索于 2018 年 4 月 11 日，来自[https://en.wikipedia.org/wiki/Curve_fitting](https://en.wikipedia.org/wiki/Curve_fitting)