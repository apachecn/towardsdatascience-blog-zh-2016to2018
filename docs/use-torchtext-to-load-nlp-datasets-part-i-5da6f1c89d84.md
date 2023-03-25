# 使用 torchtext 加载 NLP 数据集—第一部分

> 原文：<https://towardsdatascience.com/use-torchtext-to-load-nlp-datasets-part-i-5da6f1c89d84?source=collection_archive---------5----------------------->

## PyTorch Tensors 管道的简单 CSV 文件

![](img/f52a80e0c9a0552466d74ed70a9f1835.png)

如果你是 PyTorch 用户，你可能已经熟悉了 *torchvision* 库，因为 *torchvision* 已经变得相对稳定和强大，而[已经成为 PyTorch 的官方文档](http://pytorch.org/docs/master/torchvision/index.html)。鲜为人知的 *torchtext* 库试图实现与 *torchvision* 相同的东西，但使用 NLP 数据集。它仍在积极开发中，并且有些问题可能需要您自己解决[【1】](https://github.com/pytorch/text/issues/140)[【2】](https://github.com/pytorch/text/issues/177)。尽管如此，我发现它已经相当有用了。最好使用 *torchtext* 并在需要时定制或扩展它(如果你的用例是通用的，也许还可以创建一个 PR)。)而不是自己构建整个预处理管道。

在这篇文章中，我将使用[有毒评论分类数据集](https://www.kaggle.com/c/jigsaw-toxic-comment-classification-challenge)作为一个例子，并尝试使用 *torchtext* 演示一个加载该数据集的工作管道。我以前在一次不成功的尝试中使用过这个数据集，我们将在这里使用相同的标记化方案:

 [## [学习笔记]用于多标签文本分类的 StarSpace

### StarSpace 是一个雄心勃勃的模型，试图解决广泛的实体嵌入问题。它已经被创建并且…

medium.com](https://medium.com/@ceshine/learning-note-starspace-for-multi-label-text-classification-81de0e8fca53) 

## 证明文件

首先需要解决的是文档。在[项目自述](https://github.com/pytorch/text/blob/master/README.rst)之外基本没有简洁的代码示例。下一个最好的事情是在 *test* 文件夹中的单元测试。你必须弄清楚东西在哪里，然后自己把它们放在一起。

您可以使用 sphinx 从 docstrings 构建文档(在本文中我们使用了 *torchtext* 0.2.1):

```
pip install sphinx sphinx_rtd_theme
git clone --branch v0.2.1 [https://github.com/pytorch/text.git](https://github.com/pytorch/text.git)
cd text/docs
make html
```

HTML 页面将位于 *text/docs/build/html* 文件夹中。

我个人觉得直接看源代码比较容易。

## 预处理 CSV 文件

```
import pandas as pd
import numpy as npVAL_RATIO = 0.2def prepare_csv(seed=999):
    df_train = pd.read_csv("data/train.csv")
    df_train["comment_text"] = \
        df_train.comment_text.str.replace("\n", " ")
    idx = np.arange(df_train.shape[0])
    np.random.seed(seed)
    np.random.shuffle(idx)
    val_size = int(len(idx) * VAL_RATIO)
    df_train.iloc[idx[val_size:], :].to_csv(
        "cache/dataset_train.csv", index=False)
    df_train.iloc[idx[:val_size], :].to_csv(
        "cache/dataset_val.csv", index=False)
    df_test = pd.read_csv("data/test.csv")
    df_test["comment_text"] = \
        df_test.comment_text.str.replace("\n", " ")
    df_test.to_csv("cache/dataset_test.csv", index=False)
```

和上一篇文章一样，我把原始数据放在*数据*文件夹中，所有从它导出的数据都放在*缓存*文件夹中。这个 *prepare_csv* 功能的存在主要有两个原因:

1.  您必须自己将训练数据集分为训练数据集和验证数据集。torchtext 不会为你这样做。
2.  需要删除换行符。否则 *torchtext* 无法正确读取 csv 文件。

是为了确保我们每次都有相同的分成。

## 标记化

如前所述，标记化方案与前一篇文章中的相同:

```
import reimport spacy
NLP = spacy.load('en')
MAX_CHARS = 20000def tokenizer(comment):
    comment = re.sub(
        r"[\*\"“”\n\\…\+\-\/\=\(\)‘•:\[\]\|’\!;]", " ", 
        str(comment))
    comment = re.sub(r"[ ]+", " ", comment)
    comment = re.sub(r"\!+", "!", comment)
    comment = re.sub(r"\,+", ",", comment)
    comment = re.sub(r"\?+", "?", comment)
    if (len(comment) > MAX_CHARS):
        comment = comment[:MAX_CHARS]
    return [
        x.text for x in NLP.tokenizer(comment) if x.text != " "]
```

这个函数返回一个注释的标记列表。很长的注释被修剪为 *MAX_CHARS* 个字符，否则 NLP.tokenizer 可能需要很长时间才能返回。

## 加载数据集

```
import loggingimport torch
from torchtext import dataLOGGER = logging.getLogger("toxic_dataset")def get_dataset(fix_length=100, lower=False, vectors=None):
    if vectors is not None:
        # pretrain vectors only supports all lower cases
        lower = True
    LOGGER.debug("Preparing CSV files...")
    prepare_csv()
    comment = data.Field(
        sequential=True,
        fix_length=fix_length,
        tokenize=tokenizer,
        pad_first=True,
        tensor_type=torch.cuda.LongTensor,
        lower=lower
    )
    LOGGER.debug("Reading train csv file...")
    train, val = data.TabularDataset.splits(
        path='cache/', format='csv', skip_header=True,
        train='dataset_train.csv', validation='dataset_val.csv',
        fields=[
            ('id', None),
            ('comment_text', comment),
            ('toxic', data.Field(
                use_vocab=False, sequential=False,
                tensor_type=torch.cuda.ByteTensor)),
            ('severe_toxic', data.Field(
                use_vocab=False, sequential=False, 
                tensor_type=torch.cuda.ByteTensor)),
            ('obscene', data.Field(
                use_vocab=False, sequential=False, 
                tensor_type=torch.cuda.ByteTensor)),
            ('threat', data.Field(
                use_vocab=False, sequential=False, 
                tensor_type=torch.cuda.ByteTensor)),
            ('insult', data.Field(
                use_vocab=False, sequential=False, 
                tensor_type=torch.cuda.ByteTensor)),
            ('identity_hate', data.Field(
                use_vocab=False, sequential=False, 
                tensor_type=torch.cuda.ByteTensor)),
        ])
    LOGGER.debug("Reading test csv file...")
    test = data.TabularDataset(
        path='cache/dataset_test.csv', format='csv', 
        skip_header=True,
        fields=[
            ('id', None),
            ('comment_text', comment)
        ])
    LOGGER.debug("Building vocabulary...")
    comment.build_vocab(
        train, val, test,
        max_size=20000,
        min_freq=50,
        vectors=vectors
    )
    LOGGER.debug("Done preparing the datasets")
    return train, val, test
```

该功能有两个主要组件:`data.Field`和`data.TabularDataset`。`comment`变量指定了`comment_text`列的预处理管道，并在`train`、`validation`、`test`数据集之间共享，因此它们使用相同的词汇。一些细节:

1.  `sequential=True`指定该列保存序列。
2.  `tokenizer=tokenizer`指定记号赋予器。如果输入列足够干净，可以使用内置的标记器，例如`tokenizer="spacy"`。
3.  `fix_length`将所有序列填充或修剪到固定长度。如果未设置，长度将是每批中最长序列的长度。
4.  `pad_first=True`从左侧填充序列。例如，如果目标长度为 5，`A text sequence`将被填充为`<pad> <pad> A text sequence`。
5.  `tensor_type`指定返回的张量类型。由于在大多数情况下我们使用 GPU 来训练模型，将其设置为`torch.cuda.LongTensor`将省去我们稍后将其移动到 GPU 内存的麻烦。
6.  `lower`指定我们是否将所有英文字符设置为小写。

另一个非常方便的特性是`.build_vocab`，它构建了词汇表，因此我们可以在以后将记号/单词转换成整数，并且可以选择性地为您加载预训练的单词向量(`comment.vocab.vectors`将是与当前词汇表对齐的加载向量)。在处检查可用的预训练矢量[。`max_size`设置最大词汇量，`min_freq`设置一个单词在语料库中出现的最少次数。](https://github.com/pytorch/text/blob/v0.2.1/torchtext/vocab.py#L379)

字段`toxic`、`severe_toxic`、`obscene`、`threat`、`insult`、`identity_hate`为二元变量。稍后我们需要将它们组合在一起作为模型的目标。

`data.TabularDataset.splits`做的基本和`data.TabularDataset.__init__`一样，但是同时读取多个文件。测试数据集在单独的调用中加载，因为它没有目标列。我们必须在 csv 文件中以精确的顺序指定字段，并且不能跳过任何列。因此，我们必须明确指定`("id", None)`来跳过第一列。

## 创建批处理并遍历数据集

```
def get_iterator(dataset, batch_size, train=True, 
    shuffle=True, repeat=False):
    dataset_iter = data.Iterator(
        dataset, batch_size=batch_size, device=0,
        train=train, shuffle=shuffle, repeat=repeat,
        sort=False
    )
    return dataset_iter
```

这应该很简单。它会返回一些我们可以迭代的东西，当整个数据集都被读取后就会停止。我们还没有使用高级的`sort`特性，因为我们使用的是固定长度，我们可能不需要这样做。

下面是一个简单的用法示例(针对一个时期):

```
for examples in get_iterator(
            self.train_dataset, batch_size, train=True,
            shuffle=True, repeat=False
        ):
    x = examples.comment_text # (fix_length, batch_size) Tensor
    y = torch.stack([
        examples.toxic, examples.severe_toxic, 
        examples.obscene,
        examples.threat, examples.insult, 
        examples.identity_hate
    ], dim=1)
```

并使用加载的预训练向量(假设您的单词嵌入位于`model.word_em`，训练数据集作为`train_dataset`加载):

```
model.word_em.weight.data = \
    train_dataset.fields["comment_text"].vocab.vectors
```

就是这样！我们差不多已经有了开始构建和训练模型所需的东西。

**20180207 更新:**我注意到`get_iterator`既没有`random_state`也没有`seed`参数。实际上是靠`random`内置模块来管理随机性。因此，您需要执行以下操作之一来获得不同时期之间的不同批次:

1.  在历元之间用不同的种子运行`random.seed(seed)`(在调用`get_iterator`之前)
2.  在`get_iterator`中使用`repeat=True`。您必须自己在循环内部定义一个停止标准，否则 for 循环将永远继续下去。

## 未完待续…

上述方法存在一个重大问题。真的很慢。在我的电脑上，整个数据集加载过程大约需要 7 分钟，而实际的模型训练大约需要 10 分钟。我们应该序列化标记化的序列，也许还应该序列化词汇表，以使它更快。一旦我想通了，我打算在下一篇文章中写下如何去做。

除了序列化，许多事情也可以改进。例如，一个更灵活的训练/验证分割方案会有很大帮助。高级排序机制和压缩序列可能也值得探索。

## 附加链接

更详细的教程:

[](http://anie.me/On-Torchtext/) [## Torchtext 教程

### 大约 2-3 个月前，我遇到了这个库:Torchtext。我漫不经心地浏览了自述文件，意识到…

阿尼.我](http://anie.me/On-Torchtext/) 

第二部分出版:

[](https://medium.com/@ceshine/use-torchtext-to-load-nlp-datasets-part-ii-f146c8b9a496) [## 使用 torchtext 加载 NLP 数据集—第二部分

### 序列化和更容易的交叉验证

medium.com](https://medium.com/@ceshine/use-torchtext-to-load-nlp-datasets-part-ii-f146c8b9a496) 

## Python 环境

*   Python 3.6
*   数字版本 1.14.0
*   熊猫
*   PyTorch 0.4.0a0+f83ca63(应该很接近 0.3.0)
*   火炬文本 0.2.1
*   空间 2.0.5

## 这篇文章中使用的所有代码

(不包括示例用法。)