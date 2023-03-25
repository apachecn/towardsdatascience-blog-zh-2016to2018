# 面向数据科学家的 Python(和非 Python)制图工具概述

> 原文：<https://towardsdatascience.com/overview-python-and-non-python-mapping-tools-for-data-scientists-d27a8aaa7f8b?source=collection_archive---------11----------------------->

![](img/e68440a13f471ec5d6795d343ec9d157.png)

通常，数据需要在地理基础上理解。因此，数据科学家应该熟悉他们可以使用的主要制图工具。

我以前做过相当多的地图绘制工作，但鉴于我目前在温尼伯市担任交通资产数据科学家这一职位的中心性质，我很快就清楚我需要对地理地图绘制前景进行仔细的调查。

有一系列令人眼花缭乱的选择。鉴于我的主要编程语言是 python，我将从该生态系统中的工具开始，然后我将转到可用于地理分析的其他软件工具。

**Python 中的绘图工具**

[*地质公园*](http://geopandas.org/)

GeoPandas 是一个很棒的库，它使得用 Python 管理地理数据变得很容易。

在其核心，它本质上是 [pandas](https://pandas.pydata.org/) (任何使用 python 的数据科学家都必须知道的库)。事实上，它实际上是建立在 pandas 之上的，具有像“GeoSeries”和“GeoDataFrame”这样的数据结构，这些数据结构用有用的地理数据处理功能扩展了等效的 pandas 数据结构。所以，你得到了熊猫的所有优点，加上地理上的优势。

GeoPandas 结合了几个现有的地理数据分析库的功能，每个库都值得熟悉。这包括 [shapely](http://toblerity.org/shapely/project.html) 、 [fiona](http://toblerity.org/fiona/) ，以及通过 [descartes](https://pypi.org/project/descartes/) 和 [matplotlib](https://matplotlib.org/) 内置的地理测绘功能。

您可以像熊猫一样将空间数据读入 GeoPandas，GeoPandas 可以处理您期望的地理数据格式，如 GeoJSON 和 ESRI 形状文件。加载数据后，您可以轻松地更改投影、执行几何操作、地理聚合数据、使用空间连接合并数据以及执行地理编码(这依赖于地理编码包 [geopy](https://geopy.readthedocs.io/en/stable/) )。

[*底图*](https://matplotlib.org/basemap/)

底图是基于 matplotlib 构建的地理绘图库，matplotlib 是 python 绘图库的鼻祖。与 matplotlib 类似，底图非常强大和灵活，但代价是获取所需的地图有些耗时和繁琐。

另一个值得注意的问题是，[底图最近在 2016 年接受了新的管理，并将被 Cartopy](https://matplotlib.org/basemap/users/intro.html) 取代(如下所述)。尽管底图将维持到 2020 年，但 Matplotlib 网站指出，现在所有的开发工作都集中在 Cartopy 上，用户应该转向 Cartopy。因此，如果您计划使用底图，请考虑使用…..

[*漫画式*](https://scitools.org.uk/cartopy/docs/latest/)

Cartopy 提供了几何变换能力以及映射能力。与底图类似，Cartopy 向 matplotlib 公开了一个接口，用于在数据上创建地图。matplotlib 为您提供了强大的制图能力和灵活性，但缺点是相似的:与其他选项相比，创建一个漂亮的地图相对来说更复杂，需要更多的代码和修改才能得到您想要的东西。

[*geoplotlib*](https://github.com/andrea-cuttone/geoplotlib/wiki/User-Guide#tiles-providers)

Geoplotlib 是 Python 的另一个地理制图选项，似乎是一个高度灵活和强大的工具，允许静态地图创建、动画可视化和交互式地图。我从未使用过这个库，它看起来相对较新，但它可能是一个值得关注的未来。

[*gmplot*](https://github.com/vgm64/gmplot)

gmplot 允许你使用“类似 matplotlib 的界面”在谷歌地图上轻松地绘制多边形、线和点。这使您可以快速轻松地绘制数据，并利用谷歌地图固有的交互性。可用的绘图包括带填充的多边形、落点、散点、网格线和热图。对于快速简单的交互式地图来说，这似乎是一个很好的选择。

[*Mapnik*](http://mapnik.org/)

Mapnik 是一个用 C++编写的工具包(带有 Python 绑定),用于制作严肃的地图应用程序。它的主要目标是在网络上开发这些地图应用。它似乎是一个重型工具，为你今天在网上看到的许多地图提供动力，包括 [OpenStreetMap](https://www.openstreetmap.org/#map=3/71.34/-96.82) 和 [MapBox](https://www.mapbox.com/) 。

[T5【叶】T6](https://github.com/python-visualization/folium)

follow 让你可以利用流行的 [leaflet.js](https://leafletjs.com/) 框架来创建交互式地图，而无需编写任何 JavaScript 代码。这是一个很棒的库，我在最近几个月经常使用(我用 leav 为我的[温尼伯树数据博客文章](http://www.marknagelberg.com/adventures-in-open-data-hacking-winnipeg-tree-data/)生成了所有的可视化)。

Folium 允许您绘制点、线和多边形，生成 choropleth 地图和热图，创建地图层(用户可以自行启用或禁用)，并为您的地理数据生成弹出工具提示(额外的好处是:这些工具提示支持 html，因此您可以真正地自定义它们，使它们看起来更好)。地图上使用的标记和线条也可以进行大量的定制。

总的来说，leav 在特性、可定制性和编程简易性之间取得了很好的平衡。

[*阴谋地*](https://plot.ly/)

Plotly 是一家提供大型在线数据分析和可视化工具套件的公司。Plotly 的重点是提供一些框架，使得在 web 上呈现可视化更加容易。您用 Python(或 R)编写代码，与 plotly 库对话，可视化效果使用极其强大的 D3.js 库呈现。为了体验什么是可能的，请查看他们的网站，该网站展示了一系列制图的可能性。

除了图表和绘图工具之外，他们的网站上还有一大堆感兴趣的相关产品，值得一看。我特别感兴趣的一个是 [Dash](https://plot.ly/products/dash/) ，它允许您仅使用 Python(不需要 JavaScript 或 HTML)创建响应性数据驱动的 web 应用程序(主要为仪表板设计)。这是我一定会去看看的，可能会在不久的将来发表一篇“深入数据科学”的帖子。

[*散景*](https://bokeh.pydata.org/en/latest/)

Bokeh 是一个专门研究浏览器中呈现的交互式可视化的库。这包括[的地理数据和地图](https://bokeh.pydata.org/en/latest/docs/user_guide/geo.html)。与 Dash 类似，也有可能使用 Bokeh 来创建交互式 web 应用程序，这些应用程序实时更新数据并响应用户输入(它通过“ [Bokeh 服务器](https://bokeh.pydata.org/en/latest/docs/user_guide/server.html#userguide-server)”来实现这一点)。

**其他制图工具**

很明显，在 python 生态系统之外还有大量的绘图工具。这里有一些你可能想看看的简要总结。请记住，这个列表中遗漏了大量的工具。这些只是我比较熟悉的一些工具。

[*开普勒. gl*](https://uber.github.io/kepler.gl/#/)

开普勒是一个基于网络的应用程序，允许您探索地理数据。这是优步在 2018 年 5 月下旬发布的全新工具。你可以在网站上使用该软件——开普勒是一个客户端应用程序，没有服务器后端，所以所有数据甚至都驻留在你的本地机器/浏览器上。然而，它不仅仅是在开普勒网站上使用；您可以[安装应用程序](https://github.com/uber/kepler.gl)并在本地主机或服务器上运行它，您也可以将它嵌入到您现有的 web 应用程序中。

该程序有一些很棒的功能，拥有你在交互式地图应用程序中期望的大多数基本功能，加上一些非常棒的附加功能，如[分割地图](https://github.com/uber/kepler.gl/blob/master/docs/f-map-styles/6-split-maps.md)和[回放](https://github.com/uber/kepler.gl/blob/master/docs/h-playback.md)。网站上展示的例子非常漂亮，软件也很容易使用(非程序员也可以使用)。

本用户指南提供了更多关于开普勒的信息，它能做什么，以及如何做。我非常期待为一些即将到来的项目检查它。

[*地图框*](https://www.mapbox.com/)

Mapbox 提供了一套与制图相关的工具，旨在帮助开发人员创建使用地图和空间分析的应用程序。它提供了一系列服务，从为您的网站创建精美的地图到帮助您构建地理处理应用程序。

[这个概述](https://www.mapbox.com/help/how-mapbox-works-overview/)提供了一个可行的好主意。Mapbox 提供了一组底图图层，允许您自定义地图、添加自己的数据以及构建 web 和移动应用程序。它还提供了使用地理编码、方向、空间分析和其他功能等工具来扩展 web 应用程序功能的选项。虽然 Mapbox 不是免费服务，但他们似乎有慷慨的免费 API 调用限制(见他们的定价[这里](https://www.mapbox.com/pricing/))。

*重型 GIS 应用*

此处未包括但对从事制图的人来说非常重要的是成熟的 GIS 应用程序，如 [ArcGIS](https://www.arcgis.com/features/index.html) 和 [QGIS](https://www.qgis.org/en/site/) 。这些都是非常强大的工具，值得地理数据分析师了解。注意 ArcGIS 相当贵；然而，这是一个非常值得了解的行业标准。QGIS 也相当普遍地被使用，并且具有免费和开源的优势。

这篇文章中有什么明显的疏忽吗？请在下面的评论中告诉我或者给我发一封[邮件](mark@marknagelberg.com)。

本帖原载于[我的个人网站](http://www.marknagelberg.com/overview-python-and-non-python-mapping-tools-for-data-scientists/)。

*要访问我共享的 Anki deck 和 Roam Research notes 知识库，以及关于间隔重复和提高学习效率的技巧和想法的定期更新，* [*加入“下载 Mark 的大脑”。*](http://downloadmarksbrain.marknagelberg.com/auth)

**更多资源**

[Python 数据可视化库](https://blog.modeanalytics.com/python-data-visualization-libraries/)
[必要的 Python 地理空间库](http://spatialdemography.org/essential-python-geospatial-libraries/)
[所以您想要使用 Python 制作地图](https://sensitivecities.com/so-youd-like-to-make-a-map-using-python-EN.html#.Wx8Pg1Mvyu6)
[使用 Python 可视化地理数据](https://www.youtube.com/watch?v=ZIEyHdvF474)
[使用 Python 中的开源工具可视化地理空间数据](https://www.youtube.com/watch?v=HzPSVwyP2Y0) (YouTube)以及附带的[GitHub Repo of notebook](https://github.com/kjordahl/SciPy-Tutorial-2015)

*底图*

[底图教程](https://basemaptutorial.readthedocs.io/en/latest/index.html)
[用 Python 制作底图](https://rabernat.github.io/research_computing/intro-to-basemap.html)

*Mapnik*

[map Nik Wiki](https://github.com/mapnik/mapnik/wiki)
[map Nik——也许是迄今为止最好的 Python 地图平台](http://geospatialpython.com/2009/02/mapnik-maybe-best-python-mapping.html)
[控制你的地图](http://alistapart.com/article/takecontrolofyourmaps)

*叶子*

[Python 教程用叶子制作多层传单网络地图](https://www.youtube.com/watch?v=Ftczp3bx1uw) (YouTube)
[用 Python 制作互动地图](https://blog.prototypr.io/interactive-maps-with-python-part-1-aa1563dbe5a9)(三部分)
[用叶子制作互动犯罪地图](https://blog.dominodatalab.com/creating-interactive-crime-maps-with-folium/)

*散景*

[Python:用 Python 库 Bokeh 绘制数据](https://www.youtube.com/watch?v=P60qokxPPZc)
[用 Bokeh 绘制交互式地图](https://automating-gis-processes.github.io/2017/lessons/L5/interactive-map-bokeh.html)