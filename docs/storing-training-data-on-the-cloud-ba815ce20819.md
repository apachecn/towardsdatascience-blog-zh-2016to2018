# 在云上存储训练数据

> 原文：<https://towardsdatascience.com/storing-training-data-on-the-cloud-ba815ce20819?source=collection_archive---------18----------------------->

![](img/933acc7e7585b5fdbfcac7e44445de8c.png)

当你建立深度学习模型时，你可能会受益于更多的数据来训练。您可能会耗尽本地计算机上的空间，因此需要想办法在云上存储数据集，然后在训练模型时访问它们。

我最近在一个图像分类项目中遇到了一个障碍，我的笔记本电脑没有足够的存储空间来增加我的训练数据集的大小。做多输出的时候，(2 类以上)，图像识别问题，每类存储 10–100，000 张图像，(尤其是中等+分辨率)，会产生很多 GBs！

在本文中，我将介绍一些帮助我使用云存储的技巧，以便在模型训练期间制定批次。

# 动机

**1gb 存储多少钱？**

![](img/42d28c80e48c697b8d3f494da6318bbd.png)

Example Image

上面的图像是 54 KB 压缩的，(png)，大小为 266x168x3，(分辨率 x RGB 颜色通道)。

因此，您可以用 1 GB 存储 18，518 个这样的图像。

云存储的美妙之处在于，我们可以免费在线存储许多 GB，甚至以相当低的成本存储 TBs，1,000 GBs 而在我们的本地机器上存储几 GB 可能会很成问题/完全不可能。

**在训练我的模型时，我如何访问托管在云上的数据？**

我想通过这篇文章传达的主要思想是，我的数据集的命名约定如何帮助我从 URL 中检索批量图像。这很重要，因为在训练模型时，我们希望在每次迭代中随机选择一批图像输入到模型中。数据集太大，无法将整个数据集加载到内存中并从中选择随机实例。因此，我使用了一种命名约定，它允许我用索引对批处理进行分区，然后相应地从 bucket URL 获取图像。

您将获得一个指向您的存储桶的 URL，您可以使用随机数生成器将所需的索引附加到 URL 请求中。这将有助于您解析 bucket，以便为训练模型制定这些批处理。

例如，如果您将数据集中的每一项命名为:SHOE_1，SHOE_2，… SHOE_9998，SHOE_9999。您将能够用类似于以下内容的公式表示 URL 获取字符串:

```
#PSEUDOCODE
i = randomnumber()
fetch_item = url + ‘SHOE_’ + i
new_item = httprequest(fetch_item)batch.append(new_item)
```

通过对数据集使用一致的命名约定，您可以轻松地解析和提取随机批次进行训练，而无需将数据集加载到内存中。

请查看这篇堆栈溢出文章，了解如何使用 python 访问 s3 文件，如图像:

[](https://stackoverflow.com/questions/44043036/how-to-read-image-file-from-s3-bucket-directly-into-memory) [## 如何将图像文件从 S3 桶直接读入内存？

### 你的技能值多少钱？使用我们的工资计算器，了解像您这样的开发人员的收入，现在…

stackoverflow.com](https://stackoverflow.com/questions/44043036/how-to-read-image-file-from-s3-bucket-directly-into-memory) 

**结论**

总之，我希望这篇文章能启发你思考，一旦数据集变得太大而无法在本地机器上托管，你打算如何存储数据集。我认为尽快这样做很好，尤其是如果你正在使用 google colab 环境来训练你的模型。这样你就可以很容易地从 google colab 中访问你的数据。请分享您用于在云上托管定制机器学习训练数据集的任何想法或策略。感谢阅读！

# [CShorten](https://medium.com/@connorshorten300)

Connor Shorten 是佛罗里达大西洋大学计算机科学专业的学生。对软件经济学、深度学习和软件工程感兴趣。