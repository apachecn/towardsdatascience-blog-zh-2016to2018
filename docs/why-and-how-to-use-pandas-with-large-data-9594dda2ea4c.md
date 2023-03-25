# 为什么以及如何使用大数据的熊猫

> 原文：<https://towardsdatascience.com/why-and-how-to-use-pandas-with-large-data-9594dda2ea4c?source=collection_archive---------0----------------------->

## 但不是大数据…

![](img/8a5b3f0f3edb73e6bb3831029257d603.png)

[Pandas](https://pandas.pydata.org/) 已经成为在 [Python](https://www.python.org/) 编程语言中用于数据争论和分析的最受欢迎和喜爱的数据科学工具之一。

在现实世界中，数据不可避免地是杂乱的。在清理、转换、操作和分析数据方面，熊猫*真的*是游戏规则的改变者。简单来说，熊猫帮助收拾残局。

# 我的熊猫故事

刚开始学 Python 的时候，很自然的就接触到了 [NumPy](http://www.numpy.org/) (数值 Python)。它是使用 Python 进行科学计算的基础包，为 Python 中的 n 数组和矩阵运算提供了[丰富的有用特性。](https://activewizards.com/blog/top-15-libraries-for-data-science-in-python/)

此外，该库提供了对 NumPy 数组类型的数学运算的向量化，这极大地优化了计算，提高了性能和执行速度。

**NumPy 很酷。**

但是仍然存在对更高层次的数据分析工具的潜在需求。这就是熊猫来拯救我的地方。

从根本上说，Pandas 的功能是建立在 NumPy 之上的，两个库都属于 SciPy 栈。这意味着 Pandas 在很大程度上依赖 NumPy 数组来实现它的操作和计算对象——但是使用起来更方便。

实际上，NumPy 和 Pandas 仍然可以互换使用。高水平的功能和它的方便使用决定了我对熊猫的偏爱。

## 为什么用熊猫大数据——而不是大数据？

大数据和大数据有着鲜明的区别。随着[围绕大数据](https://www.wired.com/insights/2014/04/big-data-big-hype/)的炒作，我们很容易把一切都当成“[大数据](http://blog.syncsort.com/2018/03/big-data/big-data-vs-traditional-data/)”而只是随波逐流。

丹·艾瑞里教授的一个著名笑话:

![](img/29ed2a5ecae718a708b099c0ae5af071.png)

[(Source)](https://www.facebook.com/dan.ariely/posts/904383595868)

“大”和“大”这两个词本身是“相对的”,以我的拙见，大数据是指小于 100GB 的数据集。

Pandas 处理小数据(通常从 100MB 到 1GB)非常有效，性能很少成为问题。

然而，如果你在数据科学或大数据领域，当你处理大型数据集时，你迟早会遇到使用 Pandas 时的一个常见问题——低性能和长运行时间最终导致内存使用不足。

事实上，由于其算法和本地内存限制，Pandas 在大数据方面有自己的局限性。因此，大数据通常存储在计算集群中，以获得更高的可扩展性和容错能力。并且经常可以通过大数据生态系统( [AWS EC2](https://aws.amazon.com/ec2/) 、 [Hadoop](https://hadoop.apache.org/) 等)进行访问。)使用[火花](https://spark.apache.org/)和[许多其他工具](https://www.edureka.co/blog/hadoop-ecosystem)。

最后，在本地机器上使用 Pandas 处理大量数据的方法之一(有一定的内存限制)是减少数据的内存使用。

# 数据大的熊猫怎么用？

![](img/63482405cc36a5b7b4a9048b9e42c317.png)

[(Source)](https://unsplash.com/photos/lO2qvjuM7ec)

那么问题来了:**如何使用熊猫减少数据的内存占用？**

以下解释将基于我对一个匿名大型数据集(40–50gb)的体验，该数据集要求我减少内存使用以适应本地内存进行分析(甚至在将数据集读取到数据帧之前)。

## 1.以区块大小读取 CSV 文件数据

老实说，当我遇到一个错误，我不能从 CSV 文件中读取数据时，我很困惑，只意识到我的本地机器的内存对于 16GB 的 RAM 的数据来说太小了。

好消息来了，熊猫的美:我意识到 **pandas.read_csv** 有一个参数叫 **chunksize** ！

该参数实际上是指为了适应本地内存，在任何时候都要读入数据帧的行数。由于数据包含 7000 多万行，所以每次将大数据集分成许多小块时，我都将 chunksize 指定为 100 万行。

Read CSV file data in chunksize

上面的操作产生了用于迭代的 TextFileReader 对象。严格来说， **df_chunk** 不是数据帧，而是下一步进一步操作的对象。

一旦我准备好了对象，基本的工作流程就是对每个数据块执行操作，最后将它们连接起来形成一个数据帧(如下所示)。通过迭代每个块，在将每个块附加到一个列表之前，我使用函数 **chunk_preprocessing** 执行数据过滤/预处理。最后，我将这个列表连接成一个最终的数据帧，以适合本地内存。

Workflow to perform operation on each chunk

## 2.过滤掉不重要的列以节省内存

太好了。在这个阶段，我已经有了一个数据框架来做所有需要的分析。

为了节省更多的时间用于数据操作和计算，我进一步过滤掉了一些不重要的列，以节省更多的内存。

Filter out unimportant columns

## 3.更改列的数据类型

将 pandas 列数据转换成不同类型的最简单方法是使用`astype().`

我可以说，在 Pandas 中更改数据类型对节省内存非常有帮助，特别是如果您有大量数据要进行密集的分析或计算(例如，将数据输入到您的机器学习模型中进行训练)。

通过减少存储数据所需的位数，我将数据占用的总内存减少了 50%！

试试看。我相信您也会发现这很有用！让我知道进展如何。😊

Change data types to save memory

# 最后的想法

![](img/c0ac31cf1a253889ffb1a611a27f2297.png)

[(Source)](https://unsplash.com/photos/FXFz-sW0uwo)

这就是了。感谢您的阅读。

我希望分享我使用 Pandas 处理大数据的经验可以帮助您探索 Pandas 中的另一个有用特性，通过减少内存使用并最终提高计算效率来处理大数据。

通常情况下，熊猫拥有我们进行数据辩论和分析所需的的大部分特征。我强烈建议你去看看，因为下次它们会派上用场。

此外，如果你真的想学习如何用 Python 进行数据分析，那么这本书就是为你准备的—[**Python for Data Analysis**](https://www.amazon.com/Python-Data-Analysis-Wrangling-IPython/dp/1449319793/ref=as_li_ss_tl?ie=UTF8&linkCode=ll1&tag=admond-20&linkId=3b36a4cb00a369cf09eb1d7af9690b8a)。该书提供了使用 Pandas 在 Python 中操作、处理、清理和处理数据集的完整说明，为在分析中有效使用 Pandas 提供了全面的分步指南。

希望这有所帮助！

一如既往，如果您有任何问题或意见，请随时在下面留下您的反馈，或者您可以随时通过 [LinkedIn](https://www.linkedin.com/in/admond1994/) 联系我。在那之前，下一篇文章再见！😄

## 关于作者

[**Admond Lee**](https://www.linkedin.com/in/admond1994/) 目前是东南亚排名第一的商业银行 API 平台[**Staq**](https://www.trystaq.com)**—**的联合创始人/首席技术官。

想要获得免费的每周数据科学和创业见解吗？

[**加入 Admond 的电子邮件简讯——Hustle Hub**](https://bit.ly/3pGF8jv)，每周他都会在那里分享可行的数据科学职业技巧、错误&以及从创建他的初创公司 Staq 中学到的东西。

你可以在 [LinkedIn](https://www.linkedin.com/in/admond1994/) 、 [Medium](https://medium.com/@admond1994) 、 [Twitter](https://twitter.com/admond1994) 、[脸书](https://www.facebook.com/admond1994)上和他联系。

[](https://www.admondlee.com/) [## 阿德蒙德·李

### 让每个人都能接触到数据科学。Admond 正在通过先进的社交分析和机器学习，利用可操作的见解帮助公司和数字营销机构实现营销投资回报。

www.admondlee.com](https://www.admondlee.com/)