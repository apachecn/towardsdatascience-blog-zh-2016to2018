# [学习笔记]用于多标签文本分类的 StarSpace

> 原文：<https://towardsdatascience.com/learning-note-starspace-for-multi-label-text-classification-81de0e8fca53?source=collection_archive---------1----------------------->

![](img/81331d3f6bf650eae9555d56c6619a88.png)

StarSpace 是一个雄心勃勃的模型，试图解决一系列与实体嵌入相关的问题。它由脸书人工智能研究所(FAIR)创建并开源。我还没有在论文中读到模型[的细节，但是很容易假设它扩展了 FAIR 之前的文本嵌入库](https://arxiv.org/pdf/1709.03856.pdf) [fastText](https://github.com/facebookresearch/fastText) 。StarSpace 旨在成为一个直接有效的强基线，也就是说，你为新数据集或新问题训练的第一个模型。

在这篇文章中，我将写下我是如何(试图)让 StarSpace 处理来自 Kaggle 的[有毒评论分类挑战](https://www.kaggle.com/c/jigsaw-toxic-comment-classification-challenge)的数据集的。该数据集表示多标签文本分类问题，即，可以将多个标签分配给单个评论。由于 [fastText 不完全支持多标签分类](https://github.com/facebookresearch/fastText/issues/72)，我认为 StarSpace 可能是一个不错的选择。

## 剧透/TL；博士；医生

我发现 StarSpace 在当前状态下存在两个主要问题，使其无法作为该数据集的基线:

1.  没有`predict`命令:目前只提供`test`，给出描述性的模型评估。额外的工作需要你自己完成，以获得每个评论的标签概率的清晰输出。
2.  评估度量是不可定制的:由于缺乏清晰的预测输出，我们依赖于来自`test`命令的模型评估。但是我看不出有什么方法可以让它显示我们关心的这个数据集的列级平均日志损失。

另外，StarSpace 的文档也不是很全面。这也是为什么我会写这篇文章的原因。

## 安装 StarSpace

Starspace 是用 C++写的，要自己搭建。在我的 Linux Mint 18.3 环境中，这非常简单。只需克隆 Git repo 并运行`make`:

```
git clone https://github.com/facebookresearch/Starspace.git
cd Starspace
make
```

将编译好的可执行文件`starspace`复制到你的路径中的某个地方(我个人使用`~/bin/`

在其他环境中，你可能需要跳过额外的关卡来满足[需求](https://github.com/facebookresearch/StarSpace#requirements)。请查看链接以获取进一步的说明。

## Python 环境

*   Python 3.6
*   pandas 0.22.0，joblib 0.11，tqdm 4.19.5(使用了非常基础的 API，所以确切的版本应该无关紧要。如果你遇到任何问题，请在这篇文章发表后检查是否有任何重大更新。)
*   空间 2.0.5
*   sci kit-学习 0.19.1

## 准备数据集

不言而喻，您需要首先将数据集下载到您的计算机上。我通常将数据集放在项目根目录下的`data`子文件夹中，并将从原始数据集派生的所有内容放在`cache`子文件夹中。我们将在下面使用相同的设置。

下一步是清理和[标记](https://www.wikiwand.com/en/Lexical_analysis#Tokenization)注释。在这里，我使用了来自 [spacy，](https://spacy.io/)的英文分词器，删除换行符和一些其他标点符号，并将注释精简到 20，000 个字符。(清洗方案通常依赖于数据集。对于这个数据集，肯定有比这篇文章中简单的方案更好的方案。例如，你可以过滤掉停用词。)

在注释被标记化之后，我们将这些标记与它们相关联的标签结合起来，并以 StarSpace 可以识别的格式保存它们(这里我们使用 fastText 格式)。

我们使用 25%的训练数据集作为验证。结果分别保存到`cache/train.txt`和`cache/val.txt`。注意因为我还没有找到一种容易提取预测的方法，所以这里不处理测试数据集。

快速文本格式非常简单。在连续的标记之间放置一个空格，每行一个注释/实例，并在末尾放置标签。标签用数字编码，并为无毒的评论创建一个虚拟标签(StarSpace 不接受空标签列表)。

例如，这是训练数据集中的第一条注释:

```
22256635,"Nonsense?  kiss off, geek. what I said is true.  I'll have your account terminated.",1,0,0,0,0,0
```

处理后，它变成:

```
Nonsense ? kiss off , geek . what I said is true . I 'll have your account terminated . __label__0
```

## 训练和评估模型

要训练模型，请在命令行中运行以下命令:

```
starspace train -ngrams 2 -minCount 10 -thread 4 -trainFile cache/train.txt -model cache/starspace.model
```

该模型使用 unigram 和 bigram，要求一个令牌至少出现 10 次才被考虑，并使用 4 个线程。更多参数可通过`starspace -h`找到。

StarSpace 将模型保存到`cache/starspace.model`，将文字嵌入和标签嵌入向量保存到`cache/starspace.model.tsv`。您可以分析或可视化这些嵌入向量，以获得进一步的见解。

要使用验证数据集评估模型，请运行以下命令:

```
starspace test -testFile cache/val.txt -model cache/starspace.model -predictionFile cache/starspace.pred
```

除了命令行输出之外，它还将逐例评估写入`cache/starspace.pred`。一个例子:

```
Example 68:
LHS:
IT 'S HER QUOTE . WHAT CAN'T YOU UNDERSTAND ? Here is the official press release Now , please , can we stop this nonsense ? 
RHS: 
__label__0 
Predictions: 
(--) [0.542934] __label__6 
(--) [0.055087] __label__4 
(--) [0.0467122]        __label__2 
(++) [-0.0127831]       __label__0 
(--) [-0.23463] __label__1
```

另一个例子:

```
Example 116:
LHS:
STOP , you had better stop putting all that stupid junk on my IP page . read it . you 'll see what i 'll do if you do it again . 
RHS: 
__label__0 
Predictions: 
(++) [0.529969] __label__0 
(--) [0.416814] __label__2 
(--) [0.388696] __label__4 
(--) [0.34913]  __label__3 
(--) [0.240833] __label__1
```

注意，数据集中的大多数评论都是没有毒性的( *__label__6* )，所以我在这里真的是精挑细选了一些例子。

分配给每个标签的值的含义不清楚。我想答案可以在报纸上找到。

## 包扎

不幸的是，StarSpace 还没有准备好作为有毒评论数据集的基线。这篇文章尽我们所能记录了所需的步骤，以供将来参考。希望星际空间将继续发展，并真正为生产做好准备。我们会回来买更多的。