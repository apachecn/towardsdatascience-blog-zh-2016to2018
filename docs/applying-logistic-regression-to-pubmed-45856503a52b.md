# 逻辑回归在 PubMed 中的应用

> 原文：<https://towardsdatascience.com/applying-logistic-regression-to-pubmed-45856503a52b?source=collection_archive---------24----------------------->

![](img/79cd9055bd86548fedfe655a1866390f.png)

> “该指南是权威性的。现实往往是不准确的。”
> 
> ――道格拉斯·亚当斯，《银河系漫游指南》

在我的上一篇[文章](https://blog.usejournal.com/cancer-research-are-we-moving-in-the-right-direction-35932e93745b)中，我们研究了如何从 PubMed 收集的大量医学文章中获得有意义的见解，PubMed 是一个免费的生物医学和生命科学文献档案库。这一次，我们将继续与医学文章合作，创造一些完全不同的东西。我们将使用监督机器学习来识别特定主题的文章。

由于数据科学家是一个长时间保持坐姿的人(你现在可能正在伸直你的背)，我决定建立一个文本分类模型来预测一篇文章是否是关于肌肉骨骼疾病的。

为了实现这一点，我们将需要一个带标签的数据集，它将包含等量的关于肌肉骨骼疾病和其他医学状况的文章。一种方法是遵循这些简单的步骤。

# **收集文章**

这很简单。物品存放在[这里](https://www.nlm.nih.gov/databases/download/pubmed_medline.html)。我们只需要下载合理数量的文件并解压。

# **解析 XML 文件**

像上次一样，我们将使用[这个漂亮的 Python 解析器](https://github.com/titipata/pubmed_parser)从文件中获取必要的数据。

```
#return list of dictionariesdicts_out = pp.parse_medline_xml('data/pubmed19n0001.xml.gz', year_info_only=False, nlm_category=False)
```

因此，每篇文章都将由一个包含所有基本属性的字典来表示，如标题、摘要、pmid、作者、期刊、出版日期等。幸运的是，在我们的小实验中，我们真正需要的两样东西是摘要和网格术语。

# 处理网格术语

你会问我，这些术语是什么？MeSH(医学主题词)是美国国家医学图书馆的受控词汇词库，用于为 PubMed 检索文章。本质上，它是一个按层次排列的医学术语词汇表，每个医学术语都有一个唯一的 ID。由于 MeSH 经常用于描述 PubMed 的期刊文章的主题内容，我们可以使用 MeSH 术语作为一篇文章是否是关于肌肉骨骼疾病的良好指标。

当然，我们可以从 [MeSH 官方网站](https://meshb.nlm.nih.gov/record/ui?ui=D009140)手动收集与肌肉骨骼疾病相关的 MeSH 术语，然而，对此有一个更简单的解决方案。你可以在 [NLM 网站](https://www.nlm.nih.gov/databases/download/mesh.html)上找到一个名为 d2019.bin 的 ASCII 文件，其中有完整的网格术语列表。如您所见，该文件的结构很难处理。这是一个网状记录的样子。

![](img/1e602a666c38a23e39872dc81137adf2.png)

MeSH record

我们需要做的就是收集属于网格树的期望部分(C05)的所有网格 id。为此，我们将查看词汇表中的所有记录，并获取树位置(表示为“MN”)以 C05 开始的所有记录的网格 id(表示为“UI”)。*注意，一些网格术语可能有几个树位置。*

![](img/80e8be3582474b71e3c451b5399b36eb.png)

MeSH IDs related to Musculoskeletal Diseases (a fraction of a full list)

好了，现在我们准备好给数据集加标签了。假设我们的文章已经存储在数据库中(假设 [MongoDB](https://www.mongodb.com/what-is-mongodb) )，通过检查一篇文章是否具有属于新收集的 C05 网格 id 列表的网格 id，很容易对它们进行标记。完成这项任务的有效方法是使用 [Celery](http://docs.celeryproject.org/en/latest/getting-started/first-steps-with-celery.html) 来并行处理工作。

# **接下来是激动人心的部分**

为了将给定的抽象分类到这两个类别中的一个，我们将建立一个逻辑回归模型。但是首先，我们需要改变摘要的表示。我们将使用通用语句编码器将每个摘要编码成一个高维向量。通用句子编码器是在大型语料库上预先训练的，可以用于各种任务(情感分析、分类等)。该模型以单词、句子或段落作为输入，并输出 512 维向量。

最后，我们将使用 Python 的 [scikit-learn 库](https://scikit-learn.org/stable/modules/generated/sklearn.linear_model.LogisticRegression.html)来拟合 Logistic 回归模型并进行预测。

![](img/4cb9f5e862b94865200b1e55da4b21f7.png)

Confusion matrix

看起来我们做得很好。混淆矩阵表明，我们的模型在 84%的情况下正确预测了肌肉骨骼疾病的摘要。其他主题的摘要在 86%的情况下被正确预测。

# **结论**

由于医学文献通常使用相似的术语，并且有大量的文章描述了一种以上的疾病，因此很难达到 100%的准确性。然而，这个例子告诉我们，建立一个可靠的分类模型并使用它来准确地识别特定主题的医学文章是可能的。随着科学出版物的数量每年增长越来越快，使用机器学习来促进搜索并自动将小麦从谷壳中分离出来变得越来越重要。