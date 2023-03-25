# 医学图像的计算机视觉特征提取 101 第 1 部分:边缘检测/锐化/模糊/浮雕/超像素

> 原文：<https://towardsdatascience.com/computer-vision-feature-extraction-101-on-medical-images-part-1-edge-detection-sharpening-42ab8ef0a7cd?source=collection_archive---------5----------------------->

![](img/22074260e1c8852226ed388c2a9f6df2.png)

Gif from this [website](https://media.giphy.com/media/CLa9sW9t8yv6/giphy.gif)

所以今天，我只想回顾一下计算机视觉中的一些核心概念，我希望把重点放在应用上，而不是理论上。如果你是计算机视觉的新手，我强烈推荐你观看我在下面链接的这个视频系列来了解这个理论。(视频系列很长，但是真的很好看。)

此外，这是我们将要使用的公开可用数据的列表。
1。 [*乳腺癌细胞来自 UCSB 生物分割基准数据集*](https://bioimage.ucsb.edu/research/bio-segmentation)
2。 [*细胞 2D 来自 UCSB 生物分割基准数据集*](https://bioimage.ucsb.edu/research/bio-segmentation)
3。 [*驱动:用于血管提取的数字视网膜图像*](http://www.isi.uu.nl/Research/Databases/DRIVE/download.php)
4。 [*从 kaggle 数据集进行超声神经分割*](https://www.kaggle.com/c/ultrasound-nerve-segmentation)
5。 [*脑部核磁共振来自 pixabay*](https://pixabay.com/en/mri-magnetic-x-ray-skull-head-782459/)
6。 [*脑部核磁共振来自 pixabay*](https://pixabay.com/en/mri-magnetic-x-ray-skull-head-782457/)
7。 [*来自 pixabay 的自然图像*](https://pixabay.com/en/sunset-lighthouse-dawn-dusk-sun-3120484/)

请注意，这篇文章是让我回顾计算机视觉中的一些基本概念。

**2D 卷积**

![](img/c8d9e515abd981bd98989bdc756f01f2.png)

Image from this [website](http://www.songho.ca/dsp/convolution/convolution2d_example.html)

**卷积是对两个*[](https://en.wikipedia.org/wiki/Function_(mathematics))**【f 和 g】进行一个* [*的数学运算*](https://en.wikipedia.org/wiki/Operation_(mathematics)) *产生第三个函数，也就是通常被视为原始函数之一的修改版，赋予* [*的积分逐点将两个函数的*](https://en.wikipedia.org/wiki/Pointwise) *相乘作为其中一个函数的量***

**2D 卷积运算是计算机视觉的核心，因为这是我们将要使用的主要运算，所以请确保您理解这个概念。如果您需要帮助，请[单击此处查看](http://www.songho.ca/dsp/convolution/convolution2d_example.html)由 [Song Ho Ahn](mailto:song.ahn@gmail.com) 提供的 2D 卷积运算的分步示例。**

****原始图像****

**![](img/f2de11dd3bc65e3368b1549f7ca52944.png)****![](img/1820ac8ad0296c84e4a68c8117923629.png)****![](img/511ff6ca45a4ad15d522d412ec6f337c.png)****![](img/7cadd37b557729dc11f40f4fc874a4fa.png)****![](img/3607571ab3a1c00a15287cbba2d10f7c.png)****![](img/a16316fc85ac440aeb81311d374ec76f.png)**

****左上** → [*来自 UCSB 的乳腺癌细胞生物分割基准*](https://bioimage.ucsb.edu/research/bio-segmentation)
**中上** → [*来自 UCSB 的细胞 2D 生物分割基准数据集*](https://bioimage.ucsb.edu/research/bio-segmentation)
**右上** → [*驱动:用于血管提取的数字视网膜图像*](http://www.isi.uu.nl/Research/Databases/DRIVE/download.php)
**左下** → [*来自 kaggle 数据集的超声神经分割【图*](https://www.kaggle.com/c/ultrasound-nerve-segmentation)**

**在我们对这些图像做任何事情之前，让我们实际上看看每个图像看起来像什么，最后下面是我们的[自然图像](http://Natural Image from pixabay)看起来像什么。**

**![](img/d4448f30f2c31ef9e9fd17215921a32b.png)**

****身份内核****

**![](img/e9982aaf66fa639651058375ede85233.png)**

**让我们从简单的开始，身份内核不改变给定的图像，因为它是身份内核。如下图所示，图像看起来完全一样，除了所有图像都是黑白的。**

**![](img/e7f63be84c8f51432ef902d8542df412.png)****![](img/b25ff9a1178567d5e02e5d502454fb9c.png)****![](img/558c81bf7415eed17847057b70ccdbb1.png)****![](img/14d8369b2cfe1cce3fe9f61a811b1803.png)****![](img/fa870b853a5b729d1ffe8c1fba8e53b4.png)****![](img/d01b3fdab17637717543da7ffc71e733.png)****![](img/ad17584d39e55dcd858b89dfcf23fde2.png)**

****边缘检测(水平)****

**![](img/51d3f2b4f96e495d691db1cb04380c35.png)**

**现在让我们看看每张图片的水平方向有多少变化。令我印象深刻的一个非常有趣的观察是自然图像，我们可以看到来自照明光束的光无处可寻。**

**![](img/e67ccae97718edfb35f2939c5da799c1.png)****![](img/75519fac4b4deb593911ad7dff7413e7.png)****![](img/f46da194842e6f5736913fac34397b4c.png)****![](img/3df3d0428db92aa703f13b62f8e1b56a.png)****![](img/e6f8ae96e338e0ce74166298f4ea5d4d.png)****![](img/eb24c07dea7738b8e6d6e26b027e7aa6.png)****![](img/df5af49b9bbd730d9b0d8a3e7561752d.png)**

****边缘检测(垂直)****

**![](img/6bdfb382bd9d31e5dd5de55a4944326b.png)**

**现在让我们看看在垂直方向上对每张图片所做的修改。我们可以观察到光束在自然图像中返回。**

**![](img/52aa221b6feefbba5d12f0ce25bdc3a8.png)****![](img/e029df74d66cacf6a1d28cd2ad289f21.png)****![](img/b8dc4778e6e0f1dbda557033d96befbe.png)****![](img/8552af81a8777fa9dd3c6adecd03ce07.png)****![](img/addf3e8f9ee7bc2d9f3652e241ef454c.png)****![](img/3bd41bb379d18c0dfae8286ecaa5a075.png)****![](img/d2b72385f4981017b46ee5d6a7682760.png)**

****边缘检测(梯度幅度)****

**![](img/40e5dd8de3f684152a77d773883632b5.png)**

**因为我们现在在水平方向和垂直方向都有变化，我们可以用平方和的平方根来得到梯度的大小。请看下面这个维基页面上的[方程式的截图。](https://en.wikipedia.org/wiki/Sobel_operator)**

**![](img/117cfa89f678deac58bdf16bdc7f9cb7.png)**

**Image from this [website](https://en.wikipedia.org/wiki/Sobel_operator)**

**![](img/9fabcb748c5da9ee49c02eab202c5454.png)****![](img/8f8f376da54c76205404d7f839574e82.png)****![](img/30165b280d3b522365c30da713e47b29.png)****![](img/77bfcff13519dca097dba16d14859448.png)****![](img/c538b8643bc86bdafe904d34f4447045.png)****![](img/72eec8b0b5598e534d4cf21b21e0bf95.png)****![](img/4f0b0a1f53811af1572ad9622b5986eb.png)**

****边缘检测(渐变方向)****

**![](img/68bbd2e05d75905cd98fb599aff8b5dd.png)**

**因为我们现在在水平方向和垂直方向都有变化，我们可以通过使用反正切函数获得梯度方向。请参见下面的方程式。**

**![](img/944d1f68a3903b9c30e2186d1bbbd865.png)**

**Image from this [website](https://en.wikipedia.org/wiki/Sobel_operator)**

**当我们可视化原始结果时，我们会得到如下结果。看起来非常奇怪的图像…**

**![](img/70c56cd1bf95db63489e09fb746b4ca7.png)****![](img/35811016cb6843371f825dbd02c5ca0d.png)****![](img/538263f7b9bf23d817df236c434460fc.png)****![](img/2e436438fc371e340f08d503173464ec.png)****![](img/b91323201d3f4d48d2a889254e6785b7.png)****![](img/9addaae4699ffc555b3f454114db5b4c.png)****![](img/ed796074b46085f1ed33fa72486b305d.png)**

****索贝尔滤波器(梯度幅度)****

**![](img/e307173a14bd2d88e3c1940998db3ba3.png)**

**现在，让我们采取一个索贝尔过滤器，并获得梯度幅度。我个人看不出我们使用的过滤器有什么大的不同。**

**![](img/23b613136d6a220775e17936f268fe75.png)****![](img/33a0b007d2857968dfbc9ec28f1f357e.png)****![](img/6f72796ab0faa376323639e1e9b6a639.png)****![](img/43214478d173c283db1e361352f1f45e.png)****![](img/d11150c1fc74300409fda97a81a583e3.png)****![](img/00bb4a85a36768b00dc3dc4ab118474c.png)****![](img/58bf7800ec983d887dde4e6911c234c5.png)**

****索贝尔滤镜(渐变方向)****

**![](img/d9c2a838c23acb665725fc3dce6d6320.png)**

**同样，对于梯度方向，我看不出我们使用的过滤器和索贝尔过滤器之间有很大的区别。**

**![](img/9bd7a904fddf8d222984660be25b04fd.png)****![](img/89aa749253cf936f9701eebbeb8ad904.png)****![](img/d4b0e9cbfae0709fefe23a2fc1a25cab.png)****![](img/a40dd460d40189617e61624fb6497aba.png)****![](img/e0d549fa671087cd237db06457a825ef.png)****![](img/74788e9da49d10ae540aa5d5a273e9a7.png)****![](img/73bd605b736eb3528b37730ab32cd76f.png)**

****高斯模糊****

**![](img/9644e388248e3bd75bf520b807617fb5.png)**

**我们也可以模糊图像，通过使用高斯滤波器，如果我们看到这个滤波器，它看起来会像下面这样。这个操作通常被称为[高斯模糊](https://en.wikipedia.org/wiki/Gaussian_blur)。**

**![](img/5456ed5a1203e279466fa1225db314ee.png)**

**Image from this [website](https://homepages.inf.ed.ac.uk/rbf/HIPR2/gsmooth.htm)**

**![](img/75681541e4d2fdcabe0c2eb44ebb9ff8.png)****![](img/0fb189441f57896e49171f599408a956.png)****![](img/0c1f1a4e3d5fa29113058987f99dea89.png)****![](img/8398a3346d027a7f33f1a8018879b360.png)****![](img/6c4498897017aabe010f481f1ddb4f41.png)****![](img/01228c07dbe872471adee144ccdde605.png)****![](img/2ca19cebe43d1a3b264f2cc98487cebe.png)**

****锐化边缘****

**![](img/4a747a97b3043c5560187caf782db441.png)**

**我们还可以锐化图像，首先找到图像的边缘，然后将该值添加到原始图像中。这将锐化图像的边缘。**

***锐化图像非常类似于寻找边缘，将原始图像和边缘检测后的图像彼此相加，结果将是一个边缘得到增强的新图像，使其看起来更清晰。*——[洛德的计算机图形学教程](http://lodev.org/cgtutor/filtering.html#Sharpen)**

**![](img/b6386397fe6fc44c76150ab18a14fd4e.png)****![](img/fee3a952d3c38156a117c62ffa24a1ec.png)****![](img/3602051d1259a021ee8bc14d8aed0a43.png)****![](img/3e9250d7901367f25f3f796173173f0b.png)****![](img/ee613fddbb631e3ad727e980c740570d.png)****![](img/e5b3a4a9e73174f2240f0de45851051e.png)****![](img/ebddcaefc03072659d3ed2f1d152baeb.png)**

****浮雕****

**![](img/044b7e6116af2ea12964e75db6b72444.png)**

**为了给图像一个阴影效果，我们也可以使用浮雕滤镜，并添加 128 的偏差。我其实不知道这个滤镜，直到我从 [Lode 的计算机图形学教程](http://lodev.org/cgtutor/filtering.html#Sharpen)上看到。结果看起来非常类似于水平方向上的边缘检测。**

**"*一个浮雕滤镜给图像一个 3D 阴影效果，这个结果对于图像的凹凸贴图非常有用。它可以通过在中心的一侧取一个像素，并从中减去另一侧的一个像素来实现。像素可以得到正的或负的结果。要将负像素用作阴影，将正像素用作光线，对于 bumpmap，图像的偏移为 128。*——[洛德的计算机图形学教程](http://lodev.org/cgtutor/filtering.html#Sharpen)。**

**![](img/9cffd55f17d3a46a92e7de9757e9bd60.png)****![](img/0304d0ed5f69e09022e73e5e22e96829.png)****![](img/5d229b99140f861bc2b60a55637f8378.png)****![](img/b21b100ba887338a7181c7143ee6c18c.png)****![](img/50872826207b00560ed25f6148f3f08d.png)****![](img/1d199a2c2d44c6b715e66cd5c2c0bf03.png)****![](img/4bebbf05250662c2becdb56dbfb0e329.png)**

****超级像素****

**![](img/e0df90cd933694dad20757ca6abdcbad.png)**

**最后，为了好玩，让我们使用 [skimage](http://scikit-image.org/) 库中实现的[简单线性迭代聚类](http://scikit-image.org/docs/dev/api/skimage.segmentation.html#skimage.segmentation.slic)制作超级像素。**

**![](img/b0176deddbcba2754d56f834a6071bb1.png)****![](img/ebe1fac192a482e082ac1a8c888bc17b.png)****![](img/6893566cf39acd7dafb65426d5dc2db5.png)****![](img/ff839a6691f8a2575c5a1d7f2b4ef9fb.png)****![](img/94a359903dcdf75145f47604982bff50.png)****![](img/45f40ae88dfd38a3a66414babd62440a.png)****![](img/bacaff452d7328837dedaa0cbe26913b.png)**

****互动码****

**![](img/f94c913250ca94fe946c094daea83abd.png)**

**对于 Google Colab，您需要一个 Google 帐户来查看代码，而且您不能在 Google Colab 中运行只读脚本，所以请在您的操场上创建一个副本。最后，我永远不会请求允许访问你在 Google Drive 上的文件，仅供参考。编码快乐！**

**请注意，我不想滥用任何数据政策。所以对于谷歌 Colab，我找到了下面的图片，它被标记为重复使用。要访问在线代码，请点击此处。**

**![](img/f13728d2cd2a37b5c5f8dc55221dd604.png)**

**Image from this [website](https://upload.wikimedia.org/wikipedia/commons/thumb/c/cb/Felix_Titling_sample.png/1280px-Felix_Titling_sample.png)**

****最后的话****

**我想发这个帖子已经很久了，很高兴知道我终于发了。我希望尽快再发一篇关于[猪](https://en.wikipedia.org/wiki/Histogram_of_oriented_gradients)特征或者[筛](https://docs.opencv.org/3.0-beta/doc/py_tutorials/py_feature2d/py_sift_intro/py_sift_intro.html)特征的帖子。**

**如果发现任何错误，请发电子邮件到 jae.duk.seo@gmail.com 给我，如果你想看我所有写作的列表，请在这里查看我的网站。**

**同时，在我的推特上关注我[这里](https://twitter.com/JaeDukSeo)，访问[我的网站](https://jaedukseo.me/)，或者我的 [Youtube 频道](https://www.youtube.com/c/JaeDukSeo)了解更多内容。如果你感兴趣的话，我还做了解耦神经网络[的比较。](https://becominghuman.ai/only-numpy-implementing-and-comparing-combination-of-google-brains-decoupled-neural-interfaces-6712e758c1af)**

****参考****

1.  **接替艾伦。(2018).YouTube。2018 年 4 月 9 日检索，来自[https://www.youtube.com/watch?v=2S4nn7S8Hk4&list = pltizwl 5 stv 3d 4 uq 6 pvzxkrlkp _ 3 xocotn](https://www.youtube.com/watch?v=2S4nn7S8Hk4&list=PLtizWl5sTV3d4uQ6PvzXKrlkp_3XOCotN)**
2.  **安，S. (2018)。2D 卷积的例子。Songho.ca 于 2018 年 4 月 9 日检索，来自[http://www . song ho . ca/DSP/convolution/convolution 2d _ example . html](http://www.songho.ca/dsp/convolution/convolution2d_example.html)**
3.  **内核(图像处理)。(2018).En.wikipedia.org。检索于 2018 年 4 月 9 日，来自[https://en . Wikipedia . org/wiki/Kernel _(image _ processing)](https://en.wikipedia.org/wiki/Kernel_(image_processing))**
4.  **图像压花。(2018).En.wikipedia.org。检索于 2018 年 4 月 9 日，来自 https://en.wikipedia.org/wiki/Image_embossing**
5.  **罗斯布鲁克(2014 年)。用 Python 访问单个超像素分割。PyImageSearch。检索于 2018 年 4 月 9 日，来自[https://www . pyimagesearch . com/2014/12/29/access-individual-super pixel-segmentations-python/](https://www.pyimagesearch.com/2014/12/29/accessing-individual-superpixel-segmentations-python/)**
6.  **生物分割|生物图像信息学中心|加州大学圣巴巴拉分校。(2018).Bioimage.ucsb.edu。检索于 2018 年 4 月 9 日，来自[https://bioimage.ucsb.edu/research/bio-segmentation](https://bioimage.ucsb.edu/research/bio-segmentation)**
7.  **驱动:下载。(2018).isi . uu . nl . 2018 年 4 月 9 日检索，来自[http://www.isi.uu.nl/Research/Databases/DRIVE/download.php](http://www.isi.uu.nl/Research/Databases/DRIVE/download.php)**
8.  **超声波神经分割| Kaggle。(2018).Kaggle.com。检索于 2018 年 4 月 9 日，来自[https://www.kaggle.com/c/ultrasound-nerve-segmentation](https://www.kaggle.com/c/ultrasound-nerve-segmentation)**
9.  **Pixabay 上的免费图像——核磁共振成像，磁性，x 光，头骨，头部。(2018).Pixabay.com。检索于 2018 年 4 月 9 日，来自[https://pix abay . com/en/MRI-magnetic-x-ray-skull-head-782457/](https://pixabay.com/en/mri-magnetic-x-ray-skull-head-782457/)**
10.  **Pixabay 上的免费图像——日落、灯塔、黎明、黄昏、太阳。(2018).Pixabay.com。检索于 2018 年 4 月 9 日，来自[https://pix abay . com/en/sunset-light house-dawn-dusk-sun-3120484/](https://pixabay.com/en/sunset-lighthouse-dawn-dusk-sun-3120484/)**
11.  **Pixabay 上的免费图像——核磁共振成像，磁性，x 光，头骨，头部。(2018).Pixabay.com。检索于 2018 年 4 月 9 日，来自[https://pix abay . com/en/MRI-magnetic-x-ray-skull-head-782459/](https://pixabay.com/en/mri-magnetic-x-ray-skull-head-782459/)**
12.  **Python 中的 DICOM:用 PyDICOM 和 VTK 将医学图像数据导入 NumPy。(2014).PyScience。检索于 2018 年 4 月 9 日，来自[https://pyscience . WordPress . com/2014/09/08/DICOM-in-python-importing-medical-image-data-into-numpy-with-pydicom-and-VTK/](https://pyscience.wordpress.com/2014/09/08/dicom-in-python-importing-medical-image-data-into-numpy-with-pydicom-and-vtk/)**
13.  **Matplotlib 图:移除轴，l. (2018)。Matplotlib 绘图:删除轴、图例和空白。Stackoverflow.com。检索于 2018 年 4 月 9 日，来自[https://stack overflow . com/questions/9295026/matplotlib-plots-removed-axis-legends-and-white-spaces](https://stackoverflow.com/questions/9295026/matplotlib-plots-removing-axis-legends-and-white-spaces)**
14.  **SciPy . signal . convolved 2d—SciPy v 1 . 0 . 0 参考指南。(2018).Docs.scipy.org。2018 年 4 月 9 日检索，来自[https://docs . scipy . org/doc/scipy/reference/generated/scipy . signal . convolved . html](https://docs.scipy.org/doc/scipy/reference/generated/scipy.signal.convolve2d.html)**
15.  **matplotlib，R. (2018 年)。在 matplotlib 中删除保存的图像周围的空白。Stackoverflow.com。检索于 2018 年 4 月 9 日，来自[https://stack overflow . com/questions/11837979/removing-white-space-around-a-saved-image-in-matplotlib](https://stackoverflow.com/questions/11837979/removing-white-space-around-a-saved-image-in-matplotlib)**
16.  **NumPy . arctan—NumPy 1.14 版手册。(2018).Docs.scipy.org。2018 年 4 月 9 日检索，来自[https://docs . scipy . org/doc/numpy/reference/generated/numpy . arctan . html](https://docs.scipy.org/doc/numpy/reference/generated/numpy.arctan.html)**
17.  **空间过滤器-高斯平滑。(2018).Homepages.inf.ed.ac.uk 检索于 2018 年 4 月 9 日，来自[https://homepages.inf.ed.ac.uk/rbf/HIPR2/gsmooth.htm](https://homepages.inf.ed.ac.uk/rbf/HIPR2/gsmooth.htm)**
18.  **SciPy . n image . filters . Gaussian _ filter—SciPy v 0 . 15 . 1 参考指南。(2018).Docs.scipy.org。2018 年 4 月 9 日检索，来自[https://docs . scipy . org/doc/scipy-0 . 15 . 1/reference/generated/scipy . ndimage . filters . Gaussian _ filter . html](https://docs.scipy.org/doc/scipy-0.15.1/reference/generated/scipy.ndimage.filters.gaussian_filter.html)**
19.  **高斯模糊。(2018).En.wikipedia.org。检索于 2018 年 4 月 9 日，来自[https://en.wikipedia.org/wiki/Gaussian_blur](https://en.wikipedia.org/wiki/Gaussian_blur)**
20.  **模块:分段—对 0.14 版开发文档进行分段。(2018).Scikit-image.org。检索于 2018 年 4 月 9 日，来自[http://scikit-image . org/docs/dev/API/skim age . segmentation . html # skim age . segmentation . slic](http://scikit-image.org/docs/dev/api/skimage.segmentation.html#skimage.segmentation.slic)**