# 用 Python 进行情感分析(第 1 部分)

> 原文：<https://towardsdatascience.com/sentiment-analysis-with-python-part-1-5ce197074184?source=collection_archive---------0----------------------->

## IMDb 电影评论分类

![](img/39ea1c787da8dfec04a30a4851edbbdf.png)

Photo by [Denise Jans](https://unsplash.com/photos/2We0jzrMLYc?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) on [Unsplash](https://unsplash.com/search/photos/movie-theatre?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

情感分析是数据科学家需要执行的一项常见 NLP 任务。这是一个用 Python 创建准系统电影评论分类器的简单指南。本系列的后续部分将重点改进分类器。

本系列中使用的所有代码以及补充材料都可以在这个 [GitHub 资源库](https://github.com/aaronkub/machine-learning-examples/tree/master/imdb-sentiment-analysis)中找到。

# 数据概述

在这项分析中，我们将使用来自 IMDb 的 50，000 条电影评论的数据集。这些数据由安德鲁·马斯汇编，可以在这里找到: [IMDb 评论](http://ai.stanford.edu/~amaas/data/sentiment/)。

数据被平均分割，25k 条评论用于训练，25k 条用于测试你的分类器。而且每套都有 12.5k 的正面和 12.5k 的负面评论。

IMDb 让用户从 1 到 10 给电影打分。为了给这些评论贴上标签，数据管理员将≤ 4 星的评论标为负面，将≥ 7 星的评论标为正面。5 星或 6 星的评论被排除在外。

# 第一步:下载并合并电影评论

如果你还没有，去 [IMDb 评论](http://ai.stanford.edu/~amaas/data/sentiment/)点击“大电影评论数据集 1.0 版”。一旦完成，在你的下载文件夹中就会有一个名为`aclImdb_v1.tar.gz`的文件。

**快捷方式:**如果您想直接进行数据分析和/或对终端不太熟悉，我在这里放了一个这个步骤创建的最终目录的 tar 文件:[合并的电影数据](https://github.com/aaronkub/machine-learning-examples/blob/master/imdb-sentiment-analysis/movie_data.tar.gz)。双击这个文件应该足以解压它(至少在 Mac 上)，否则终端中的`gunzip -c movie_data.tar.gz | tar xopf —`就会这么做。

## 拆包和合并

遵循这些步骤或者在这里运行 shell 脚本:[预处理脚本](https://github.com/aaronkub/machine-learning-examples/blob/master/imdb-sentiment-analysis/preprocess_reviews.sh)

1.  将 tar 文件移动到您希望存储这些数据的目录中。
2.  打开一个终端窗口，`cd`到你放`aclImdb_v1.tar.gz`的目录。
3.  `gunzip -c aclImdb_v1.tar.gz | tar xopf -`
4.  `cd aclImdb && mkdir movie_data`
5.  `for split in train test; do for sentiment in pos neg; do for file in $split/$sentiment/*; do cat $file >> movie_data/full_${split}.txt; echo >> movie_data/full_${split}.txt; done; done; done;`

# 步骤 2:读入 Python

对于本演练中我们想要做的大多数事情，我们只需要我们的评论在 Python `list`中。确保将`open`指向存放电影数据的目录。

# 步骤 3:清理和预处理

这些评论的原始文本相当混乱，所以在我们进行任何分析之前，我们需要清理一下。这里有一个例子:

```
"This isn't the comedic Robin Williams, nor is it the quirky/insane Robin Williams of recent thriller fame. This is a hybrid of the classic drama without over-dramatization, mixed with Robin's new love of the thriller. But this isn't a thriller, per se. This is more a mystery/suspense vehicle through which Williams attempts to locate a sick boy and his keeper.<br /><br />Also starring Sandra Oh and Rory Culkin, this Suspense Drama plays pretty much like a news report, until William's character gets close to achieving his goal.<br /><br />I must say that I was highly entertained, though this movie fails to teach, guide, inspect, or amuse. It felt more like I was watching a guy (Williams), as he was actually performing the actions, from a third person perspective. In other words, it felt real, and I was able to subscribe to the premise of the story.<br /><br />All in all, it's worth a watch, though it's definitely not Friday/Saturday night fare.<br /><br />It rates a 7.7/10 from...<br /><br />the Fiend :."
```

**注意:**理解并能够使用正则表达式是执行任何自然语言处理任务的先决条件。如果你不熟悉它们，也许可以从这里开始:[正则表达式教程](https://medium.com/factory-mind/regex-tutorial-a-simple-cheatsheet-by-examples-649dc1c3f285)

这是同样的评论现在的样子:

```
"this isnt the comedic robin williams nor is it the quirky insane robin williams of recent thriller fame this is a hybrid of the classic drama without over dramatization mixed with robins new love of the thriller but this isnt a thriller per se this is more a mystery suspense vehicle through which williams attempts to locate a sick boy and his keeper also starring sandra oh and rory culkin this suspense drama plays pretty much like a news report until williams character gets close to achieving his goal i must say that i was highly entertained though this movie fails to teach guide inspect or amuse it felt more like i was watching a guy williams as he was actually performing the actions from a third person perspective in other words it felt real and i was able to subscribe to the premise of the story all in all its worth a watch though its definitely not friday saturday night fare it rates a   from the fiend"
```

**注意:**有许多不同的、更复杂的方法来清理文本数据，它们可能会产生比我在这里所做的更好的结果。我希望本教程的第 1 部分尽可能简单。此外，我通常认为，在花费时间进行可能不必要的转换之前，最好用最简单的可能解决方案获得基线预测。

## …向量化…

为了让这些数据对我们的机器学习算法有意义，我们需要将每个评论转换成数字表示，我们称之为*矢量化*。

最简单的形式是创建一个非常大的矩阵，用一列来表示语料库中的每个独特的单词(在我们的例子中，语料库是所有 50k 篇评论)。然后我们将每个评论转换成包含 0 和 1 的一行，其中 1 表示与该列对应的*语料库*中的单词出现在那个*评论*中。也就是说，矩阵的每一行都非常稀疏(大部分是零)。这个过程也被称为*一次热编码*。

# 步骤 4:构建分类器

既然我们已经将数据集转换成适合建模的格式，我们就可以开始构建分类器了。逻辑回归是我们使用的一个很好的基线模型，原因有几个:(1)它们易于解释，(2)线性模型往往在像这样的稀疏数据集上表现良好，以及(3)与其他算法相比，它们学习起来非常快。

为了简单起见，我只担心超参数`C`，它调整*正则化*。

**注意:**我们用于训练和测试的目标/标签将是相同的，因为两个数据集的结构相同，其中第一个 12.5k 是正的，最后一个 12.5k 是负的。

看起来给我们最高精度的 C 值是`0.05`。

## 列车最终模型

现在我们已经找到了 C 的最佳值，我们应该使用整个训练集训练一个模型，并评估我们在 25k 测试评论上的准确性。

作为一个理智的检查，让我们来看看正面和负面评论中最有区别的 5 个词。我们将通过分别查看最大和最小系数来做到这一点。

这就是了。一个非常简单的分类器，开箱后具有相当不错的准确性。

# 下次

在本系列的下一部分中，我们将研究更复杂的方法来提高分类器的性能。

*   **文本处理**:词干化/词条化，将每个单词的不同形式转化为一个。
*   **n-grams** :除了单词标记(1-gram/unigram)，我们还可以包含单词对。
*   **表示**:代替简单的二进制向量，我们可以使用字数或 *TF-IDF* 来转换这些计数。
*   **算法**:除了逻辑回归，我们还会看看支持向量机的表现。

**点击这里查看:**

[](/sentiment-analysis-with-python-part-2-4f71e7bde59a) [## 用 Python 进行情感分析(第 2 部分)

### 改进电影评论情感分类器

towardsdatascience.com](/sentiment-analysis-with-python-part-2-4f71e7bde59a) 

感谢阅读！这是我在 Medium 上的第一篇帖子，所以请评论你的任何问题或建议。