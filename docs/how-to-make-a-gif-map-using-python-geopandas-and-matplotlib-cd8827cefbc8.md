# 如何使用 Python、Geopandas 和 Matplotlib 制作 gif 地图

> 原文：<https://towardsdatascience.com/how-to-make-a-gif-map-using-python-geopandas-and-matplotlib-cd8827cefbc8?source=collection_archive---------5----------------------->

## 不需要 Photoshop:只使用 Python 和命令行制作动画图表。

![](img/aaa57430300aeaaf833a413672afdf19.png)

更新:我增加了一个归一化功能来保持每张地图的彩条图例范围相同。下面和 Github 上的 Jupyter 笔记本中的代码已更新。请看下面关于怪异渲染的注释。

作为一种语言，Python 非常灵活。这使得有时仅用几行代码就可以实现许多不同的可视化。但是现在有了这么多不同的图表网站和软件，为什么还要写代码呢？难道我们就不能用 GUI 上传一个 csv 文件，调整范围，点击导出 png 然后收工吗？

是的。你当然可以。有时，如果您需要快速、一次性的图表或地图，这是最佳选择。但是当你需要制作大量的地图——大量的地图时，使用 Python 的真正力量就发挥出来了。

在本教程中，我将在[之前的文章](/lets-make-a-map-using-geopandas-pandas-and-matplotlib-to-make-a-chloropleth-map-dddc31c1983d)的基础上，讲述如何使用 [Geopandas](http://geopandas.org/) 和 [Matplotlib](https://matplotlib.org/contents.html) 制作 choropleth 地图。我们将以此为起点，所以如果你想赶上进度，请在这里查看帖子[或在 Github](/lets-make-a-map-using-geopandas-pandas-and-matplotlib-to-make-a-chloropleth-map-dddc31c1983d) 上查看我的 [Jupyter 笔记本。](https://github.com/bendoesdata/make-a-map-geopandas)

这次我将介绍如何创建一个动画 gif choropleth 来显示地理数据(因为它是一张地图)和随时间的变化。最重要的是，本教程将从头到尾只使用 Python 和一些命令行工具。无需在 Photoshop 中寻找正确的菜单/下拉菜单/窗口。

正如我在之前的教程中提到的，这可能不是所有用例的最佳工作流。但是如果速度、再现性和一致性是你优先考虑的，我认为这是一个好办法。

# 为什么是 gif 图？

在过去的几年里，gif 图表似乎在社交媒体上变得非常流行。《金融时报》和《经济学人》等出版商花了更多时间来简化和完善他们的数据，即在 Twitter 和 Instagram 等平台上获得更多参与的格式。也许更重要的是，gif 图表允许在静态图表所显示的基础上有一个新的故事层。

这有几种不同的用法:

*   通过注释或突出显示引导用户浏览同一图表中最重要的点
*   通过比较显示两个不同的图表
*   显示相同的图表随时间的变化(这是我将要介绍的内容)

这个列表并不详尽，用例还在继续扩展。总之:移动图表既酷又有用(如果使用正确的话)。

让我们开始映射。

# 如何制作 gif 图

不必重述[之前教程](https://github.com/bendoesdata/make-a-map-geopandas)的每一个步骤，以下是你应该开始的:

1.  在 shapefile 中加载
2.  加载 csv 格式的数据以进行可视化
3.  连接两个数据框
4.  绘制地图并开始设计风格。

现在，我们将使用 Python 中 for()循环的强大功能，使用多个不同的列生成相同的地图。因为我们想显示随时间的变化，我们需要确保我们的数据包含多年的变量。为了便于标记，让我们确保数据的每个列标题都是一个年份数字。

为了遍历列，我们需要一个字符串列表来调用每一列的名称。让我们创建一个包含每列年份的列表变量(格式化为字符串)。让我们也设置一个输出路径，这样我们所有的地图都保存到一个文件夹中。

```
*# save all the maps in the charts folder*
output_path = 'charts/maps'

*# counter for the for loop*
i = 0

*# list of years (which are the column names at the moment)*
list_of_years = ['200807','200907','201007','201107','201207','201307','201407','201507','201607']
```

最后，在创建映射之前，我们希望为 vmin 和 max 值设置一个一致的全局变量。这设置了颜色范围的值。如果没有预先设置，Matplotlib 将在每次 for 循环迭代时更改 choropleth 的范围，因此很难看到值是如何随时间增加或减少的。

```
*# set the min and max range for the choropleth map*
vmin, vmax = 200, 1200
```

# 编写 for 循环

一旦开始使用它们，for()循环就相当简单了。for()循环的语法是这样的:

*   对于 list_of_years 列表中的每一年，运行以下代码。
*   当列表中的所有年份都完成了代码时，停止循环。

以下是使用上一教程中底图要点的代码:

```
*# start the for loop to create one map per year*
**for** year **in** list_of_years:

    *# create map,* UDPATE: added plt.Normalize to keep the legend range the same for all maps
    fig = merged1.plot(column=year, cmap='Blues', figsize=(10,10), linewidth=0.8, edgecolor='0.8', vmin=vmin, vmax=vmax,
legend=True, norm=plt.Normalize(vmin=vmin, vmax=vmax))

    *# remove axis of chart*
    fig.axis('off')

    *# add a title*
    fig.set_title('Violent crimes in London', \
              fontdict={'fontsize': '25',
                         'fontweight' : '3'})

    *# this will save the figure as a high-res png in the output path. you can also save as svg if you prefer.*
    filepath = os.path.join(output_path, only_year+'_violence.jpg')
    chart = fig.get_figure()
    chart.savefig(filepath, dpi=300)
```

如果您运行这段代码并打开刚才设置的输出路径文件夹，您应该会看到许多地图，每张地图都有略微不同的阴影，代表不同年份的数据。但是这可能会让浏览者感到困惑:一旦地图动画化，他们怎么知道年的增量呢？

我们可以在底部设置一个简单的日期范围(2007–2015)，但是 for()循环给了我们一个更好的解决方案。因为我们已经将每一列的年份作为字符串保存在变量中，所以我们可以为每个地图添加不同的注释(对应于数据的年份)。

根据我们的 for()循环，变量“year”将是 for 循环每次运行时的列 year。使用这个逻辑，我们可以插入“year”作为 fig.annotate()参数的变量。

现在，每次 for 循环运行时，不同的年份将作为注释插入到地图上。使用相同的逻辑，我们也可以设置文件名以每年开始，这样就很容易找到每年对应的地图。在 for()循环中添加这段代码将会添加一个 year 注释。

```
*# create an annotation for the year by grabbing the first 4 digits*
    only_year = year[:4] *# position the annotation to the bottom left*
    fig.annotate(only_year,
            xy=(0.1, .225), xycoords='figure fraction',
            horizontalalignment='left', verticalalignment='top',
            fontsize=35)
```

重新运行代码，您的地图应该会被新的地图所替换，每个地图的左下角都有一个年份注释。如果你从上到下浏览地图，你可以开始对你的 gif 有个大概的了解。

# 制作 gif

对于最后一步(gif 制作)，我们将离开 Python 并打开终端(或您使用的任何命令行编辑器)。导航到您在终端中保存所有文件的目录。

遗憾的是，我找不到让 ImageMagick 将我的 png 地图批量转换成 jpg 格式的方法。我似乎无法让 matplotlib 将我的图表保存为。jpg(得到一个在[这个堆栈溢出线程](https://stackoverflow.com/questions/8827016/matplotlib-savefig-in-jpeg-format)上描述的错误)。所以我找到了一个变通方法，使用一个叫做 sips 的 Mac 程序(教程和讲解者[在这里](https://robservatory.com/use-sips-to-quickly-easily-and-freely-convert-image-files/))。但本质上，它是一个命令行工具来批量转换文件。对于非 Mac 用户，我相信也有类似的解决方案！

将这段代码复制/粘贴到您的终端上，将所有的 png 文件转换成 jpg 文件。

```
for i in *.png; do sips -s format jpeg -s formatOptions 70 "${i}" --out "${i%png}jpg"; done
```

有许多方法可以制作 gif，但我使用 ImageMagick 有几个原因:

*   安装和设置相对容易
*   它允许您在一行代码中设置过渡时间、裁剪大小和文件格式
*   它超级快

ImageMagick 的文档非常全面。如果你还没有安装它，看看他们的[安装和文档页面](https://imagemagick.org/script/convert.php)。

要检查是否正确安装了 ImageMagick:

1.  在终端中，键入:

```
convert -version
```

2.如果已经安装了 ImageMagick，将显示带有版本和版权声明的消息。一旦您在系统上安装了 ImageMagick，请导航到包含我们刚刚制作的所有地图的目录。现在我们需要运行一行代码来创建 gif。简而言之，它是这样做的:

*   **转换**:获取所有这些文件并进行更改
*   **-delay 60** :设置进入下一幅图像之前每幅图像之间经过的时间
*   **-循环 0** :设置无限循环
*   **<插入所有将被转换的文件名>**
*   **my_map.gif**

下面是在终端中使用的代码:

```
convert -delay 60 -loop 0 2008_violence.jpg 2009_violence.jpg 2010_violence.jpg 2011_violence.jpg 2012_violence.jpg 2013_violence.jpg 2014_violence.jpg 2015_violence.jpg 2016_violence.jpg new_map_normal.gif
```

现在检查您创建的新文件“new_map_normal.gif ”,您应该会看到类似这样的内容:

![](img/aaa57430300aeaaf833a413672afdf19.png)

*The gif above still renders as changing color range here on Medium, but the code outputs the correct file and the legend is correct when you view the file on other sites* ¯\_(ツ)_/¯*. Download it and view in a different programme to see for yourself (and anyone who knows why this might be, let me know!).*

就是这样！我们有一张 gif 图。

当然，现在这开始提出一系列新的问题:为什么近年来颜色变深了？暴力犯罪显著上升了吗？这种格式的 Gif 地图的局限性在于很难看清每个行政区背后的实际数字。

为此，更复杂的组合图可能比在右边包含一个图例更有用(下一次要处理的图例！).然而，作为一种揭示数据背后的故事的方法，使用 gif 地图可以成为更深入分析的一个很好的切入点。另外，你可以把任何一种有时间序列数据的图表转换成 gif 图表——我选择地图是因为它们很酷。

和往常一样，你可以在 Github 上找到复制本教程所需的所有代码(Jupyter 笔记本、shapefiles 和数据)。如果你有任何问题，在推特上找我[。](https://twitter.com/bendoesdata)

> *感谢阅读！我还出版了一份关于数据可视化、数据科学和技术交叉的每周时事通讯。每一版都包含可供阅读的文章、可供探索的可视化内容、可供分析的数据集以及更多可供学习的教程。你可以在这里* [*报名。*](https://mailchi.mp/7029eac7f34a/data-curious-signup)