# 如何用 Plotly 创建交互式地图绘图

> 原文：<https://towardsdatascience.com/how-to-create-interactive-map-plots-with-plotly-7b57e889239a?source=collection_archive---------5----------------------->

![](img/21533cbd6d86a14ce82c26b83c68c3c5.png)

“brown wooden map board” by [Brett Zeck](https://unsplash.com/@iambrettzeck?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

毫无疑问，我最喜欢处理数据的一部分是创建可视化。图像是传达信息的最佳工具:它们是即时的，事实上大脑处理它们的速度比文字快几千倍。图像也是普遍的，它们会在我们的脑海中停留更久。最后，我们是懒惰的动物，因此我们更注重视觉效果，而不是文字。有多少次你遇见一个新的人，然后你努力记住他们的名字，而他们的脸却清晰地浮现在你的脑海里？

当我在学术界做研究员时，我意识到我是多么喜欢创造视觉效果:尽管我喜欢进行实验和数值模拟，但我真的很期待项目的那一刻，那时我将能够处理我所有的发现，并创建那些漂亮而强大的图表。是的，我还必须写论文来描述所有的过程，工作流程和研究背后的理论推理，但是能够用很少的图像来交流几个月的工作真是不可思议！所有这些介绍都是为了让你相信图像是重要的，如果你还不这么认为的话。尤其是对于一名数据科学家来说，他需要不时高效地向非技术受众传达信息。

在过去的一周左右的时间里，我一直在处理我的关于美国风暴事件的顶点项目，同时处理包含地理空间信息的约 11，000 个观测数据。我的第一个想法是将所有这些数据绘制在地图上，这样我就可以一眼看出哪些区域最容易受到此类事件的影响。由于我使用的是 Python，Geopandas 将是最直接的选择，但是，我想要更具交互性的库。如果我想改变地图的样式怎么办？或者，如果我只想看到雷暴或冰雹呢？我做了一点研究，发现 plotly 正是我所需要的。我找不到任何好的教程来指导我完成这个过程，所以我在网上看了一些例子，试图通过大量的试验和错误来推断性地理解这是如何工作的。这就是我决定写这篇文章的原因，同时我正在为同一个项目训练一批神经网络，当所有这些纪元来来回回时，我有空闲时间。

> 注意:要做我将要描述的事情，你需要一个与 [plotly](https://plot.ly/#/) 以及 [mapbox](https://www.mapbox.com/) 的账户。特别是因为你需要一个访问令牌，可以在这里[创建](https://www.mapbox.com/account/access-tokens)，还需要一个用户名和 API 密匙，可以在这里[生成](https://plot.ly/settings/api#/)。这一切都是免费的。我还建议您非常精通 Python 字典，因为正如您将注意到的，它们是创建这样的地图图的基础。

我从国家气候数据中心收集了从 2000 年至今美国风暴事件的数据。像往常一样，我不得不做大量的清理工作，处理丢失的数据，设计一些功能。在这个[链接](https://github.com/emmagrimaldi/Capstone_Predicting_Storm_Damages/blob/master/all_storms.csv)你可以找到。csv 文件用于此目的，并为我的顶点项目一般。这些数据包含了这种现象的纬度和经度信息，这对于我们在地图上绘图是非常重要的。它还显示了是否是雷暴、冰雹、山洪爆发等总共七个不同的类别。风暴事件的强度、所处的状态以及发生的时间

在实际绘制之前，我们需要导入和分配一些东西。这里我们需要来自 Mapbox 的访问令牌，以及来自 Plotly 的用户名和 api 密钥。

```
import plotly.plotly as py
import plotly.graph_objs as go
import plotly
import pandas as pd# setting user, api key and access token
plotly.tools.set_credentials_file(username='your_username_here', api_key='your_api_key_here')
mapbox_access_token = 'your_access_token_here'
```

为了生成我们的图，我们将需要两样东西:*数据*和*布局*。我们的变量*数据*将是七个字典的列表(每个事件类型一个)，其中每个字典包含纬度和经度值，以及风暴类型的名称和绘图的标记特征。变量*布局*是一个字典，包含了与数据没有直接联系的图像相关的所有内容:下拉菜单、注释、我们想要如何格式化它们以及我们想要将它们放置在哪里。

# 数据

在读取数据帧并将其分配给一个`df`变量后，我创建了一个包含七种不同风暴类别名称的列表，并将其命名为`event_types`。我遍历了这个列表，并使用它来过滤数据帧，为每种事件类型创建一个字典，并将其附加到*数据*列表。这样我就有了我的字典清单，*数据*。

creating the *data* variable.

我将标记的不透明度值指定为 0.5 的原因是，最有可能的是，有些区域的点数比其他区域多。赋予这些点某种透明度，将允许我们在地图上看到相同颜色的不同色调，对应于不同地区相同现象的不同频率。

我们已经完成了*数据*，现在轮到*布局*，这无疑需要更多的工作。

# 布局

我们来设置一下地图的基本设置。

Initializing the map layout.

现在我们可以开始在地图上添加一些东西来定制它，并按照我们喜欢的方式进行交互。

## 布局['批注']

假设我想放一个上面有白色文本的黑框，指定我的点仅代表从 2000 年至今造成超过 50k 美元经济损失的风暴事件。像这样的一些静态提示，将落在*布局*字典的*注释*键下:对应于*注释*的值将需要是一个字典列表，在这个特定的例子中，一个只包含一个字典的列表，因为我想在我的地图上只放置一个注释。文字上看似繁琐，但做起来绝对比解释容易。

Annotation on the map.

现在是下拉菜单的时候了，它需要被分配给*布局*字典的*更新菜单*键。

## 布局['updatemenus']

我想添加两个下拉菜单:一个选择要可视化的风暴事件的类型，以防我们只想查看类型而不是所有类型，而另一个下拉菜单将允许我们更改背景上的地图类型，默认情况下是暗的。可用地图的名称和样式可以在 Mapbox 上找到。

每一个下拉菜单都是通过一个字典重新定义的，所有这些字典都将包含在一个列表中，我们将分配给`layout['updatemenus']`。

Adding drop-down menus to the map.

## 布局['标题']

如果需要，您可以轻松地为您的地块指定标题。我这样做只是通过做`layout['title'] = 'Storm Events'`。

# 生成剧情！

一旦我们将*数据*和*布局*整理好，我们就可以通过运行以下命令来生成图表:

```
figure = dict(data = data, layout = layout)
py.iplot(figure, filename = 'put_your_name_here')
```

瞧啊！

您可以放大、缩小，从顶部菜单中选择您想要可视化的内容，并从底部菜单中更改地图类型。相当酷！我期待着像这样的其他一些情节，现在我学会了如何去做。希望您发现这也很有用！

![](img/e617ba7ebe3f3cc33ed5ad510738322e.png)

请随意查看:

[这个代码的 Github 库](https://github.com/emmagrimaldi/Capstone_Predicting_Storm_Damages/blob/master/Capstone_interactive_plot.ipynb)

[我的其他中帖。](https://medium.com/@emmagrimaldi)

我的 LinkedIn 个人资料。

**感谢您的阅读！**