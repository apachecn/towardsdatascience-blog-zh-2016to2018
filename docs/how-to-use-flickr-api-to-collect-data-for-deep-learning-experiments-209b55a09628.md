# 如何使用 flickr api 为深度学习实验收集数据？

> 原文：<https://towardsdatascience.com/how-to-use-flickr-api-to-collect-data-for-deep-learning-experiments-209b55a09628?source=collection_archive---------6----------------------->

一旦你踏上了深度学习的旅程，并通过了基本的玩具示例，如在 MNIST 数据集上对手写数字进行分类，下一个合乎逻辑的步骤就是开始从事自己的定制项目。人们遇到的第一个障碍是他们想要做的定制项目的数据可能不可用。如果你知道如何清理网站，你可以很容易地收集到你想要的图片，但是清理网站有两个陷阱:

1.刮一些网站是不合法也不道德的

2.即使你不关心法律或道德，大规模报废也是具有挑战性的，特别是如果有适当的保护措施来阻止报废的话。

除了废弃网络，还有其他选择。以下是您可以在自己的项目中使用图像数据的资源列表:

1.Yelp 公开了大量的数据，他们公开了 20 万张图片的数据集，你可以点击这里查看[https://www.yelp.com/dataset](https://www.yelp.com/dataset)

2.这里也有亚马逊评论数据托管在 http://jmcauley.ucsd.edu/data/amazon/[也包括预处理的图像特征](http://jmcauley.ucsd.edu/data/amazon/)

3.此外，还有一些数据集，如学术界非常常用的 MS COCO、Imagenet 和 CIFAR 10。

如果这些数据集似乎也不能满足您的目的，那么您可以使用 flickr 作为您的数据收集来源。Flickr 有一个 api 可以用来收集图片。您可以使用这个 api 通过标签搜索图像。例如，您可以搜索日常物品/地点和建筑物的图像，如灯柱、披萨、教堂等。我能够使用这个 api 轻松地为一个给定的标签获取大约 600 多张图片。我能够创建一个脚本，通过标签获取图像，并存储在计算机上的一个文件夹中，该文件夹以被搜索的标签命名。我创建了两个脚本，一个获取给定标记的 url，并将其存储在 csv 文件中。另一个脚本从 url 文件中读取链接，获取图像并将它们存储在一个文件夹中。

下面是通过标签获取 Url 的脚本代码， *flickrGetUrl.py* (你可以在这里注册获得密钥和应用程序秘密令牌[https://www.flickr.com/services/apps/create/](https://www.flickr.com/services/apps/create/)

要使用上面的脚本，您可以在终端中运行以下命令:

```
python flickrGetUrl.py pizza 500
```

这将在工作目录中创建一个新文件，其中将有指向 pizza 图像的链接，这将是一个名为 pizza.csv 的. csv 文件。第一个参数是您想要其图像的标记，第二个参数是要尝试的 URL 获取的数量。

第二个文件， *get_images.py* ***，*** 从 url 获取图片并将图片存储在一个文件夹中，下面是它的代码:

要使用第二个文件，请在终端中键入以下内容:

```
python get_images.py pizza.csv
```

这将从存储在 pizza.csv 中的 URL 获取图像，并将它们存储在一个名为 pizza 的文件夹中。

这些是一些常见的方法，人们可以通过这些方法获取图像数据来运行深度学习实验。请记住，上面的脚本可能需要一些时间来下载图像，您可以通过向它们添加多处理功能并在多核机器上运行它们来加速这些脚本。