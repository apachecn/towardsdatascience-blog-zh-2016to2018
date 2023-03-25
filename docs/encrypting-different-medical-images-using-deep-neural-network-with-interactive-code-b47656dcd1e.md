# 使用具有交互代码的深度神经网络加密不同的医学图像

> 原文：<https://towardsdatascience.com/encrypting-different-medical-images-using-deep-neural-network-with-interactive-code-b47656dcd1e?source=collection_archive---------2----------------------->

![](img/75cb78d22753d4e829f5f7a0005d1fd9.png)

Gif from this [website](https://giphy.com/gifs/ryan-seslow-x7bsJNqBlIIRG/download)

因此，昨天我讨论了“ [*”隐藏图像:深度隐写术*](/nips-2017-google-hiding-images-in-plain-sight-deep-steganography-with-interactive-code-e5efecae11ed) ”现在让我们将这个网络应用到医疗保健设置中。我们将利用这个网络对各种医学图像进行加密。

请注意，我们将只使用公开可用的医学图像，下面是我们将使用的数据集列表。
1。UCSB 生物分割基准数据集 2。[驱动:用于血管提取的数字视网膜图像](http://www.isi.uu.nl/Research/Databases/DRIVE/download.php)
3。[超声波神经分割](https://www.kaggle.com/c/ultrasound-nerve-segmentation)来自 Kaggle
4。[人体模型 FDA](https://wiki.cancerimagingarchive.net/display/Public/Phantom+FDA) 来自[癌症影像档案](https://wiki.cancerimagingarchive.net/display/Public/Wiki)
5。[皮肤黑色素瘤(CPTAC-CM)](https://wiki.cancerimagingarchive.net/display/Public/CPTAC-CM) 来自[癌症影像档案](https://wiki.cancerimagingarchive.net/display/Public/Wiki)

此外，所有的封面图像都来自这个数据集“[语义图像分割的基准](http://www.ntu.edu.sg/home/asjfcai/Benchmark_Website/benchmark_index.html)”。

**可视化不同类型的医学图像/数据集**

![](img/f9cf1803779f1fefdda98857092bbd8d.png)![](img/7f39676f2fdef67f376da7f35a30c96f.png)

**左图** →来自 [UCSB 生物分割基准数据集](https://bioimage.ucsb.edu/research/bio-segmentation)
**的乳腺癌细胞图像；右图** →来自 [UCSB 生物分割基准数据集](https://bioimage.ucsb.edu/research/bio-segmentation)的 2D 细胞核图像

![](img/37bd055eb2019d3df8c28586f55b8fe6.png)![](img/1d3dd908238c7a8ee333f518396e01f4.png)![](img/17e952b35794ca68965dc1d09ce78c39.png)

来自[驱动器的视网膜图像:用于血管提取的数字视网膜图像](http://www.isi.uu.nl/Research/Databases/DRIVE/download.php)

![](img/ba549ebec9064b04ea8bd98833c2852d.png)![](img/01d27c069fcb76658605823e20784d9e.png)![](img/7b0b0f7622225454400b8a67b8472d05.png)

来自[超声神经分割](https://www.kaggle.com/c/ultrasound-nerve-segmentation)的超声图像

![](img/bfb2a5ad0bc572826c28295f8c9d9361.png)![](img/a1deec08f8b1ad0f93bb4b06c3b4759e.png)![](img/0b7e956a9f21139c4d5ff9b05f9e20c0.png)

来自[仿真模型 FDA](https://wiki.cancerimagingarchive.net/display/Public/Phantom+FDA) 的拟人化胸部仿真模型图像

![](img/4ad0da87c66a0f54030988ebc5289068.png)![](img/7daba668bfeb49f1155b11552d2c3349.png)![](img/536d6967529216d8edfa869fb4245950.png)

[皮肤黑色素瘤(CPTAC-CM)](https://wiki.cancerimagingarchive.net/display/Public/CPTAC-CM) 的脑部放射影像

**网络架构**

![](img/f7d2ceb9c39e0ef5f12ad6bfab146068.png)![](img/60a9a10fede64641586b07664d0545dc.png)![](img/f2c84859fafffce246b615598ca185c0.png)

**红框** →准备图像的神经网络(Prep Network)
**黄框** →隐藏图像的神经网络(Hiding Network)
**绿框** →显示图像的神经网络(Reveal Network)

主要有三个网络，一个是准备图像、隐藏图像和揭示秘密图像。我不会深入解释这个网络，因为我已经在这篇文章 中 [*介绍过了。*](/nips-2017-google-hiding-images-in-plain-sight-deep-steganography-with-interactive-code-e5efecae11ed)

**用例:通过万维网发送医学图像**

![](img/512439709e448bef6838a5d88102aa83.png)![](img/02949d29f6f7288e7acd4874288a8516.png)

**男性图** →希望安全发送医学图像给爱丽丝医生的鲍勃医生
**黑色圆圈** →原始医学图像
**白色矩形** →准备/ 隐藏网络
**红圈** →加密医学图像通过封面图像
**云** →环球网敏感数据哪里会发生不好的事情
**紫圈** →加密图像通过网络
**蓝矩形** →揭露网络
**天蓝色圈** →揭露医学图像
**女图** →希望安全接收一些医学图像的爱丽丝博士

一个简单的用例可以是类似上面的，两个希望安全地交换医学图像的医学专家(或医生和病人)可能使用这个网络来加密和解密他们的图像。

**结果**

![](img/9af674c93f3f47594c6025455d56cf2c.png)![](img/d988ee5cd65cd1a848268a1a09367811.png)![](img/db0923efde7045b8e321ca3ad8880f64.png)![](img/ff28dd1f57b9ae4f710f7f4a3e5e4bcd.png)![](img/183a5f51627a5413e0023bb19813a872.png)

**左上图** →原秘密图
**右上图** →原封面图
**左下图** →准备图
**下中间图** →秘密+封面图
**右下图** →揭露图

对于一些封面图像，网络实际上在隐藏秘密图像方面做得相当好。然而，如下图所示，在某些情况下，网络表现不佳。

![](img/665ce7e6f1cf3337a6fc9b344d451f9b.png)![](img/eaf4b2a4a0d09b6bc2be283dff2c5a84.png)![](img/ee5be3710d0247ab8aeab518ab8f835e.png)![](img/fa8d64538c1ce49a5f06708f48abbdd5.png)![](img/6499b740f948b136064dd97df4345a15.png)

**左上图** →原秘密图
**右上图** →原封面图
**左下图** →准备图
**下中间图** →秘密+封面图
**右下图** →已曝光图

如上所述，隐藏秘密图像后，我们可以看到蜥蜴身上的一些文物，这可能会泄露原始图像。

![](img/e53cf781b5b7eeb84c023d6cf3cba24e.png)![](img/0a6984800642feb8c4a7f6280ac94f4a.png)

**左 GIF** →网络如何在它的最后纪元
**右 GIF** →网络如何过度训练的汇编

以上是网络在最后一个时期以及随着时间的推移如何工作的汇编，随着训练的继续(通过增加时期可以看到)，我们可以看到网络在隐藏图像方面做得更好。

**不同类型医学图像的个体结果**

![](img/7f53cc79231a5da534851811f29b92c1.png)![](img/cd1ff907972cf8dc99c25254f9b1970a.png)![](img/91df0fa8c748b30507db6945e6d3aa93.png)![](img/e16278a9e22afb2ace3729621922b007.png)![](img/91b5b2f4aca2c6533d1a4bb56fddec8d.png)

乳腺癌细胞图像的加密/解密结果

![](img/37748f46f7f5bfcbe1daee6ffdfc2193.png)![](img/7eb9d10f330a7a3417b8ac9e933d57a6.png)![](img/179b32ca702bf9f0f7b06d67d82c4f68.png)![](img/716fc4f931110877c7b5d455524118f6.png)![](img/d6d51b97d8fc3f7da7054863c0d365ce.png)

细胞 2D 核图像的加密/解密结果

![](img/2d49b193776890e074fddc76dc4c12af.png)![](img/59beec73b6dde9e5bb319d0218657f9e.png)![](img/d178b1037b9dd642828a27d9201c0456.png)![](img/5729ca189536946d98221a146a2d562b.png)![](img/5c7834af9562e96481d89eacabf1c077.png)

视网膜图像的加密/解密结果

![](img/93181dbb7f0612b5dbcd82e779f0d3c7.png)![](img/2424604f389543f1b83b363816abd746.png)![](img/e12c6e300d8ff17ffa9181a21dec3617.png)![](img/ad7093a15f474602b67397782793c66a.png)![](img/ee82964e50e36d90dd3ecf8ea50e40b1.png)

超声图像的加密/解密结果

![](img/e78e1637448247031f799d40cd8ea352.png)![](img/09bc7b644d095cd5d6369c26af1c7287.png)![](img/b4c29783433e012b1a94e5d40dbc28f7.png)![](img/9d95a5523a04daea3ecc3d960762bbbf.png)![](img/3710bff7660194cc0ed45b71625ebd79.png)

细胞拟人胸部模型图像的加密/解密结果

![](img/aa1b52f8c68fafcc779f133d91d9f76e.png)![](img/d1de29e9d4e90304b6b6165f762cc5d8.png)![](img/3e6726cfcb1642874d92194e32ea7c0e.png)![](img/9a5badc33853028bea13f5ac35afa0f6.png)![](img/c59a630a765e8f0811292513de0a12ec.png)

脑部放射图像的加密/解密结果

**互动码**

![](img/1b245ee135788f5e4e34925e05a31a5f.png)

对于 Google Colab，你需要一个 Google 帐户来查看代码，而且你不能在 Google Colab 中运行只读脚本，所以在你的操场上复制一份。最后，我永远不会请求允许访问你在 Google Drive 上的文件，仅供参考。编码快乐！

要访问 Google [Colab 上的代码，请点击这里。](https://colab.research.google.com/drive/1vQ2SB0euGzZBA9MJfr_sAvT-nBcRRsHg)

**注意**:我不想托管不属于我的医疗数据，因为我可能会违反他们的数据使用政策。所以这段代码不能直接在线运行。

**透明度**

![](img/ba0dacf10dc05f1efed194a4d2c6525a.png)

为了让这个实验更加透明，我上传了我所有的命令输出到我的 github，如果你想看的话，请点击这里。

**最后的话**

当然，网络并没有做得足够好以至于不能在现实世界中使用，但是这是一个开始。我很高兴看到人工智能如何影响计算机/网络安全。

如果发现任何错误，请发电子邮件到 jae.duk.seo@gmail.com 给我，如果你希望看到我所有写作的列表，请在这里查看我的网站。

同时，在我的 twitter 上关注我[这里](https://twitter.com/JaeDukSeo)，访问[我的网站](https://jaedukseo.me/)，或者我的 [Youtube 频道](https://www.youtube.com/c/JaeDukSeo)了解更多内容。如果你感兴趣的话，我还做了解耦神经网络[的比较。](https://becominghuman.ai/only-numpy-implementing-and-comparing-combination-of-google-brains-decoupled-neural-interfaces-6712e758c1af)

**参考**

1.  [NIPS 2017/Google] —在众目睽睽之下隐藏图像:使用交互式代码的深度隐写术[……]。(2018).走向数据科学。检索于 2018 年 4 月 5 日，来自[https://towards data science . com/nips-2017-Google-hiding-images-in-plain-sight-deep-steganography-with-interactive-code-e 5 efecae 11 ed](/nips-2017-google-hiding-images-in-plain-sight-deep-steganography-with-interactive-code-e5efecae11ed)
2.  s . balu ja(2017 年)。隐藏图像:深度隐写术。神经信息处理系统进展(第 2066-2076 页)。
3.  生物分割|生物图像信息学中心|加州大学圣巴巴拉分校。(2018).Bioimage.ucsb.edu。于 2018 年 4 月 5 日检索，来自[https://bioimage.ucsb.edu/research/bio-segmentation](https://bioimage.ucsb.edu/research/bio-segmentation)
4.  CP tac-CM——癌症影像存档(TCIA)公共访问——癌症影像存档维基。(2018).Wiki.cancerimagingarchive.net。检索于 2018 年 4 月 5 日，来自[https://wiki . cancer imaging archive . net/display/Public/CP tac-CM](https://wiki.cancerimagingarchive.net/display/Public/CPTAC-CM)
5.  幻影 FDA 癌症影像存档(TCIA)公共访问-癌症影像存档维基。(2018).Wiki.cancerimagingarchive.net。检索于 2018 年 4 月 5 日，来自[https://wiki . cancer imaging archive . net/display/Public/Phantom+FDA](https://wiki.cancerimagingarchive.net/display/Public/Phantom+FDA)
6.  驱动:下载。(2018).isi . uu . nl . 2018 年 4 月 5 日检索，来自[http://www.isi.uu.nl/Research/Databases/DRIVE/download.php](http://www.isi.uu.nl/Research/Databases/DRIVE/download.php)
7.  “超声波神经分割| Kaggle”。2018.Kaggle.Com。2018 年 4 月 5 日访问。[https://www.kaggle.com/c/ultrasound-nerve-segmentation](https://www.kaggle.com/c/ultrasound-nerve-segmentation)。
8.  惠，L. (2018)。语义图像分割的基准。Ntu.edu.sg .检索 2018 年 4 月 5 日，来自[http://www . NTU . edu . SG/home/asjfcai/Benchmark _ Website/Benchmark _ index . html](http://www.ntu.edu.sg/home/asjfcai/Benchmark_Website/benchmark_index.html)