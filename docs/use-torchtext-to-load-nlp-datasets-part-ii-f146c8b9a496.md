# 使用 torchtext 加载 NLP 数据集—第二部分

> 原文：<https://towardsdatascience.com/use-torchtext-to-load-nlp-datasets-part-ii-f146c8b9a496?source=collection_archive---------2----------------------->

## 序列化和更容易的交叉验证

![](img/fbffadcb6639bc3913dbdc4a9e31e852.png)

[Source](https://pixabay.com/en/paper-document-business-book-text-3091439/)

[](/use-torchtext-to-load-nlp-datasets-part-i-5da6f1c89d84) [## 使用 torchtext 加载 NLP 数据集—第一部分

### PyTorch Tensors 管道的简单 CSV 文件

towardsdatascience.com](/use-torchtext-to-load-nlp-datasets-part-i-5da6f1c89d84) 

在第一部分中，我们讨论了如何从 csv 文件加载文本数据集，标记文本，并通过 *torchtext* 将它们放入张量。现在，我们将解决该解决方案中的两个问题(仍然使用有毒评论数据集):

1.  **加载时间太长**:每次你想运行一个新的实验，重新加载数据集会浪费你很多时间。
2.  **交叉验证方法的有限选择**:实际上只有一个选择——可以通过 *seed* 和 *VAL_RATIO* 参数控制的随机分割。

## 序列化

首先，`TabularDataset`不幸的是不能直接序列化。我们从观察开始寻找替代方案，在`__init__`方法中，`TabularDataset` [将文件读入示例列表](https://github.com/pytorch/text/blob/v0.2.1/torchtext/data/dataset.py#L164):

```
with io.open(os.path.expanduser(path), encoding="utf8") as f:
    if skip_header:
        next(f)                                   
    examples = [make_example(line, fields) for line in f]
```

下一个观察是`TabularDataset`的超类`Dataset`接受一个参数`examples`(一个例子列表)。所以现在很清楚，我们需要的是**序列化来自** `**TabularDataset**` **实例的示例，并根据请求**创建 `**Dataset**` **实例。额外的好处是序列化了`comment`字段实例。**

更具体地说，以下是一般的工作流程:

```
def read_files():
    comment = data.Field(...)
    train = data.TabularDataset(...) 
    test  = data.TabularDataset(...)
    comment.build_vocab(...)
    return train.examples, test.examples, commentdef restore_dataset(train_examples, test_examples, comment):
    train = data.Dataset(...)
    test  = data.Dataset(...)
    return train, test
```

前两个返回的变量是重建数据集的基本组件。如果你愿意，你可以改装一个`comment`字段实例，但是如果你不这样做，它会更快。初始化数据集时，只需插入`comment`作为字段之一。

## 交叉验证

因为现在我们从一系列例子而不是 CSV 文件中创建数据集实例，所以生活变得容易多了。**我们可以按照我们想要的任何方式拆分示例列表**，并为每个拆分创建数据集实例。对于分类任务，我通常更喜欢分层 K-Fold 验证。但因为有毒评论数据集是多标签的，所以更难做分层。我们将在下面的章节中使用简单的 K-Fold 验证。

## 把它放在一起

完整代码请参考帖子末尾。以下是新解决方案与第一部分中的旧解决方案之间的一些比较:

1.  我们使用和以前完全一样的记号赋予器。
2.  尽管不必创建训练/验证分割，我们仍然需要一个简化的`prepare_csv` 函数来从原始 CSV 文件中删除 *\n* 字符。
3.  这个简洁的库`joblib`可以消除通过`pickle`模块显式序列化的需要。它的一个特性是**对输出值**的透明和快速磁盘缓存，这可以通过创建一个缓存设置`MEMORY`并在任何想要缓存的函数上使用`**@MEMORY.cache**` **装饰器**来实现(在本例中，函数`read_files`读入 CSV 文件并返回两个示例列表和一个字段实例)。
4.  主函数`get_dataset`现在返回**一个生成器**和**一个测试数据集**。生成器为每次迭代提供一个训练数据集和一个验证数据集，并在您运行所有可用的 K 次迭代后实现 K 重验证。

以下是在 5 重验证方案下训练 5 个模型的脚本示例:

```
train_val_generator, test_dataset = get_dataset(
    fix_length=100, lower=True, vectors="fasttext.en.300d",
    n_folds=5, seed=123
)
for fold, (train_dataset, val_dataset) in \
    enumerate(train_val_generator):
    # Initialize a model here...
    for batch in get_iterator(
        train_dataset, batch_size=32, train=True,
        shuffle=True, repeat=False
    ):
        # Train the model here...
    # Create prediction for val_dataset and get a score...
    # Create inference for test_dataset...
```

如果你忘了，这里有一个特征和目标提取的例子:

```
x = batch.comment_text.data
y = torch.stack([
        batch.toxic, batch.severe_toxic, batch.obscene,
        batch.threat, batch.insult, batch.identity_hate
    ],dim=1)
```

## 加速

这取决于你的 CPU 的能力和你的磁盘的读取速度。在我的电脑里，第一次调用`get_dataset`需要 6 分多钟，之后大约 1 分钟。

## 有毒注释数据集加载器的更新版本