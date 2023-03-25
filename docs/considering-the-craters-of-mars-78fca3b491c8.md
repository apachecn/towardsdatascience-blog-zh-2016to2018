# 考虑到火星的环形山

> 原文：<https://towardsdatascience.com/considering-the-craters-of-mars-78fca3b491c8?source=collection_archive---------11----------------------->

***【数据管理与可视化】，第 1 周***

![](img/17b4aba930d53469397965261af42c0a.png)

This 3-D stereo image taken by the Mars Exploration Rover Spirit’s navigation camera shows the rover’s lander and, in the background, the surrounding martian terrain. Original image from NASA/JPL at: [https://mars.nasa.gov/mer/gallery/press/spirit/20040110a/2NN006EFF02CYP05A000M1-A8R1_br.jpg](https://mars.nasa.gov/mer/gallery/press/spirit/20040110a/2NN006EFF02CYP05A000M1-A8R1_br.jpg)

**目的**

这篇文章是我在 Wesleyan 大学 Coursera 上的[数据分析和解释专业](https://www.coursera.org/specializations/data-analysis)的一系列相关课程的第一篇。在这个项目中，我将把数据科学工具应用到我选择的具体研究问题中；包括数据管理和可视化、建模和机器学习。我目前在第一个课程的第一周，数据管理和可视化。作为第一周的一部分，我选择了一个研究问题和数据集，我将在整个课程中继续研究。在接下来的几周，我将开始为这门课程设置我的第一个 Python 或 SAS 代码，包括我选择的数据集中变量的可视化。

**话题范围**

我有机会使用课程中提供的五个公开数据集中的一个。或者，我可以选择自己的公共数据集。然后，我必须提供一个与特定数据集相关的感兴趣的主题。经过考虑，我决定使用现有的选择之一。这是博尔德科罗拉多大学的斯图尔特·詹姆斯·罗宾斯在 2011 年对火星陨石坑进行研究时使用的数据集。该研究、密码本和数据集的链接在本文底部的参考资料部分。

在回顾了这些材料后，我决定对陨石坑的物理特征感兴趣，特别是直径(DIAM_CIRCLE_IMAGE)和深度(DEPTH_RIMFLOOR_TOPOG)。虽然形态学类别数据(形态学 _ 喷出物 _1 至 3)很有趣，但该数据不适用于数据集中列出的大多数陨石坑。此外，根据我现有的知识，形态学的可用数据可能很难解释。我已决定将我的个人密码本限制为以下变量，这些变量取自火星陨石坑密码本的列表

1.  CRATER_ID:内部使用的环形山 ID，基于行星的区域(1/16)、识别环形山的“通行证”以及识别环形山的顺序。
2.  LATITUDE_CIRCLE_IMAGE:从非线性最小二乘圆的衍生中心到所选顶点的纬度，以手动识别陨石坑边缘(单位为北纬十进制度)。
3.  LONGITUDE_CIRCLE_IMAGE:从拟合到所选顶点的非线性最小二乘圆的衍生中心开始的经度，用于手动识别陨石坑边缘(单位为东十进制度)。
4.  DIAM_CIRCLE_IMAGE:从非线性最小二乘圆拟合到选择的顶点的直径，以手动识别陨石坑边缘(单位为千米)。
5.  DEPTH_RIMFLOOR_TOPOG:沿陨石坑边缘(或内部)人工确定的 N 个点的平均高程(单位为 km)。

a.深度边缘:选择点作为相对地形高点，假设它们是侵蚀最少的，因此沿边缘的原始点最多。

b.深度底:点被选为最低海拔，不包括可见的嵌入式陨石坑。

我将重点讨论陨石坑直径和以公里为单位的深度之间是否存在关联。作为一个延伸目标，我将试图确定是否有一个次要的特征和陨石坑的位置之间的联系。后者在现有时间内可能无法实现。虽然有几个处理位置数据的 Python 库，例如 GeographicLib、GeoPandas 和 ArcPy，但这些库可能需要进行一些调整，以处理特定于火星的位置坐标系。

**已有课题研究和假设**

基于从火星轨道激光高度计获得的数据，在谷歌学术[的一次搜索产生了一些关于火星陨石坑特征的早期研究。该研究在下面的参考文献部分列出。](https://scholar.google.com)

此外，还有追溯到 20 世纪 90 年代的早期研究。上述和早期研究的数据集不容易比较。然而，Aharonson 的研究表明，某些物理特征，如直径和边缘坡度有相关性。由于边缘坡度与陨石坑的深度有些关系，我可以假设陨石坑的直径和深度彼此有直接的关系。在我开始分析之前，他们的相关程度目前还不清楚。

**总结**

我们已经讨论了这个初始帖子的目的，包括数据集的选择，以及基于数据集的研究主题的确定。此外，我们已经基于主题范围确定了数据集的个人码本的内容，并为我们的研究主题制定了假设。我们关于本课程的下一篇文章将关注于分析数据集的程序的创建和输出。感谢您花时间阅读这篇文章，我们将很快回来。

**参考文献**

Robbins，S. J. (2011)，来自新的全球火星陨石坑数据库的行星表面特性、陨石坑物理学和火星火山历史。，网站:Stuart Robbins，天文/地球物理学家在网上，1–239，地址:[http://about . sjrdesign . net/files/thesis/robbin thesis _ large MB . pdf](http://about.sjrdesign.net/files/thesis/RobbinsThesis_LargeMB.pdf)，码本:[https://d 396 qusza 40 orc . cloudfront . net/phoenix assets/data-management-visualization/Mars % 20 crater % 20 code book . pdf](https://d396qusza40orc.cloudfront.net/phoenixassets/data-management-visualization/Mars%20Crater%20Codebook.pdf)，数据集: [https://d18ky98rnyallexpires = 1505865600&Signature = hlpupz 1s 7n 3 wxc 8 zztdon 5 u 7m 6 wzk ~ ogycatsslgxj 45 jjg 2 edw 6 xlgul-dtsfwzjpzon 3 qsfhnzhsqnsrjkowrm 7 oz GCT qx 1 iy 4 it JV 7 pyk qxrhnbedfjwalywkhlzlbk 8 bi 5 cke 5 JX ~ xxxfe ~ sgizuoe 4 l 1 fnwliaoxa _&](https://d18ky98rnyall9.cloudfront.net/_b190b54e08fd8a7020b9f120015c2dab_marscrater_pds.csv?Expires=1505865600&Signature=hlpupZ1s7N3Wxc8zZTdoN5U7M6WZK~ogycatsSlGxj45JJg2eDW6xlGUl-DTSFWzjPBZOn3QSFHnZHsQnSrjKowRM7OZgctqX1iY4iTJV7pYKqXRhnBeDFJHwaLywKHLzlbK8bi5CKE5jX~xHxXfE~sGIzuOE4l1fnWliaoXAXA_&Key-Pair-Id=APKAJLTNE6QMUY6HBC5A)

Aharonson，o .、M. T. Zuber 和 D. H. Rothman (2001 年)，《火星轨道飞行器激光测高仪的火星地形统计:斜率、相关性和物理模型》，地球物理学杂志。Res .，106(E10)，23723–23735，doi:[10.1029/2000 je 001403](http://dx.doi.org/10.1029/2000JE001403)。