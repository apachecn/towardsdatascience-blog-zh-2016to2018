# 通过 3 个简单的步骤，从你的谷歌位置历史创建一个热图

> 原文：<https://towardsdatascience.com/create-a-heat-map-from-your-google-location-history-in-3-easy-steps-e66c93925914?source=collection_archive---------0----------------------->

![](img/8ce0536d8e9bfd986dea7babe4f0b97d.png)

毫不奇怪，像谷歌这样的公司几乎在跟踪我们的一举一动——如果我们允许的话。然而，通常情况下，访问我们自己正在收集的数据，并以有趣的方式利用它似乎比它应该的更难。在这篇文章中，我将向你展示一个快速的方法，帮助你标出你曾经在世界上的什么地方。好吧，反正既然你开始用安卓手机了。

我一直对我们记忆的工作方式很感兴趣。有许多线索可以帮助我们从大脑深处找回记忆——气味、照片、声音和其他感觉。不同的暗示对不同的人起作用(我的女朋友因为我不记得某些美味晚餐的味道而生我的气。).我发现位置对我来说非常有用，这使得在地图上标出我的每一步行动的能力特别有吸引力。**一旦我知道我去过哪里，我就能回忆起大部分时间我在那里做了什么。**有许多方法可以做到这一点——[草稿图](http://www.scratchmap.org/)是一种流行的方法。如果你想获得更高的保真度，你可能最终会进入数字世界。以下是方法:

1.  首先，让你的头脑被你的[谷歌时间表](https://www.google.co.uk/maps/timeline)所震撼。一定要在笔记本电脑上检查一下，因为移动版本并没有给你所有的功能。这张整洁的地图显示了你去过的所有地方。即实际*场所*如酒吧、餐厅、公司等。这是一个好的开始，但我希望能够看到我去过的每一个地点，我在那里呆了多长时间或多长时间，以及我是如何在它们之间旅行的。因此，让我们利用我们的数据创建一个热图来实现这一点。

![](img/5071f8157db9e03951d5ec409058a692.png)

My Google Timeline — Yeah, I remember that drive down through Italy

2)到[谷歌外卖](https://www.google.com/settings/takeout)，到**下载你的位置数据**。取消选择所有内容，然后检查“位置历史 JSON 格式”。在下一步中，将您的数据下载为 zip 存档，然后解压缩。现在，您的计算机上应该有一个 JSON 文件，其中包含您的手机存储您的位置时的每个实例(如果您启用了位置服务，这种情况会比您想象的更频繁)。请注意，所有这些可能需要一段时间，这取决于有多少关于你的数据。在我过去的三年里，这大约是 200mb。

![](img/b52007350156c5d6ed1768c55b6b2686.png)

Download your Location History from Google Takeout

3)从位置历史可视化工具转到免费使用的[热图功能](https://locationhistoryvisualizer.com/heatmap/)。他们也有一个[付费产品](https://locationhistoryvisualizer.com/)，功能增强，你可以看看(注:我与这家公司没有任何关系。我通过谷歌搜索找到了他们，觉得他们的产品很酷。).类似的服务还有很多，但大多数都有非常低的文件大小限制。
**上传你的 JSON 文件——et voilà，ready 就是你整个位置历史的热图。你可以像谷歌地图一样导航，还可以随意截图。**

![](img/8ce0536d8e9bfd986dea7babe4f0b97d.png)

The heat map adjusts itself to the zoom factor. So many trips to Paris…

如您所见，这是一件相当简单的事情。你可以用它来回忆你徒步旅行的路线，你穿越欧洲的公路旅行，或者你在巴黎的观光旅行；或者向你的朋友炫耀你去过的所有地方。玩得开心！

[*@ Thomas _ Essl*](https://twitter.com/Thomas_Essl)*是写东西的设计师。他还是《七件事》*[](http://seventhings.thomasessl.com)**时事通讯和《产品掘金》* *播客的制作人。在*[*www.thomasessl.com*](https://www.thomasessl.com/blog/2020/4/26/i-dont-know-how-to-draw-why-we-like-other-peoples-art-more-than-ours)*找到更多他的作品。**