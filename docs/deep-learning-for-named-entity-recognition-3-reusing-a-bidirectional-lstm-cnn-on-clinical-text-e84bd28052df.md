# 命名实体识别的深度学习#3:在临床文本数据上重用双向 LSTM + CNN

> 原文：<https://towardsdatascience.com/deep-learning-for-named-entity-recognition-3-reusing-a-bidirectional-lstm-cnn-on-clinical-text-e84bd28052df?source=collection_archive---------10----------------------->

![](img/e176635a9ffd7f6185dcbe2d1febe800.png)

## 这篇文章描述了 BLSTM + CNN 网络如何在 i2b2 临床文本中重复使用，以提取药物名称、剂量、频率和处方原因等。重用该模型可使 F1 得分达到 85.65%，与 i2b2 2009 挑战赛的获胜团队不相上下。

在本系列的 [post #2](/deep-learning-for-named-entity-recognition-2-implementing-the-state-of-the-art-bidirectional-lstm-4603491087f1?source=user_profile---------2-------------------) 中，我们在 CoNLL 2003 新闻数据上验证了最先进的模型，一个 BLSTM + CNN 模型，实现了 90%+的 F1 分数。下面是使用 Python 和 Keras 实现的这个网络:

[](https://github.com/mxhofer/Named-Entity-Recognition-BidirectionalLSTM-CNN-CoNLL) [## MX hofer/命名实体识别双向 TM-CNN-CoNLL

### 命名实体识别双向 TM-CNN-CoNLL - Keras 实现 Chiu 和 Nichols (2016)

github.com](https://github.com/mxhofer/Named-Entity-Recognition-BidirectionalLSTM-CNN-CoNLL) 

该模型在我的 arxiv 论文“【https://arxiv.org/abs/1811.05468】”[]中用于医学文本中命名实体识别的*少镜头学习。*

# i2b2 2009 临床文本数据

i2b2 基金会在 2009 年 NLP 挑战赛后发布了文本数据(由参赛团队标注)。总共有 261 份出院小结注明了药物名称(m)、剂量(do)、给药方式(mo)、给药频率(f)、持续时间(du)和给药原因(r)。以下是训练集、验证集和测试集中每个类别的计数:

```
Medication annotations:  9318
Dosage annotations:  4666
Mode annotations:  3513
Frequency annotations:  4229
Duration annotations:  571
Reason annotations:  1694
```

## 下载和预处理

你可以在这里下载数据[(签订*数据使用协议是*必需的 *)* 。一旦您获得访问权限并下载了所有文件，请打开文件夹“annotations_ground_truth”和“training.ground.truth ”,打开并阅读一些注释和相应的文本文件。让自己熟悉注释模式。例如，在“annotations_ground_truth”文件夹中，一个名为“1234.m”的假设注释文件可能是:](https://www.i2b2.org/NLP/DataSets/Download.php)

> m = " Caltrate plus d " 5:1 5:3 | | do = " one " 5:4 5:4

…表示名为“caltrate plus d”的药物包含在名为“1234”(无文件扩展名)的文本文件中，第 5 行，偏移量 1 至 3(含)。类似地，剂量“一”直接跟随在第 5 行偏移 4 处的药物之后。

您必须对文件名进行一些预处理，去掉文件名中任何不必要的文本。我发现最简单的方法是创建一个名为“数据”的文件夹，其中包含两个子文件夹“注释”和“条目”。将相应的文件添加到子文件夹中。目标是从注释到文本条目有一个 1:1 的映射，这些条目可以连接起来创建完整的数据集。对于 id 为“1234”的出院小结，注释和条目均应命名为“1234”。i2b2 2009 数据中的文件以文档 id 开始，因此这个代码片段可以帮助您去掉剩余的文本:

# 将 i2b2 解析为 CoNLL 格式

注释和条目文件名匹配后，我们现在可以将数据从 i2b2 解析为 CoNLL 格式。这涉及到几行代码，下面简要介绍一下。这是 iPython 笔记本:

[](https://github.com/mxhofer/i2b2_2009-to-CoNLL) [## mxhofer/i2b2_2009-to-CoNLL

### 在 GitHub 上创建一个帐户，为 i2b2_2009-to-CoNLL 开发做出贡献。

github.com](https://github.com/mxhofer/i2b2_2009-to-CoNLL) 

首先，解析器为注释和条目建立语料库。注释语料库用于建立注释数据框，包含文档 id、NER 标签(使用 BIO 方案)、注释的行和偏移。条目语料库用于建立条目数据框架，包含文档 id、行、偏移量和相应的单词。它还包含空行关键字(-EMPTYLINE)和新文档的开始关键字(-DOCSTART)。

其次，注释和条目数据框在文档 id、行和偏移列上连接。然后用来自 *nltk.pos_tag* 函数的 POS 标签对结果数据帧进行标记。

最后，在将数据集分成大约 70%的训练数据、15%的验证数据和 15%的测试数据之前，添加名词和动词短语的组块标签。该脚本的输出是一组。txt 文件，即 train.txt、valid.txt 和 test.txt。

请注意解析器的这些限制:

*   不考虑跨换行符的注释(总共 23，991 个注释中丢失了大约 71 个)。
*   Chunk tagger 只标记名词短语(NP)和动词短语(VP)。
*   生物注释方案通过在 XXX 类型的每个序列的开始处的标签“B-XXX”来实现(而不仅仅是为了区分 XXX 类型的连续标签)。

# 重复使用模型的性能— F1 为 85.65%

如果我们的神经网络模型在新数据上表现不好，那么所有的解析又有什么价值呢？这就是机器学习的魅力所在。尽管针对路透社的新闻数据进行了优化，但该模型仍与 i2b2 2009 挑战赛的最佳提交数据持平，F1 得分为 85.65%。这是一个了不起的结果！

在我的实验中，这些参数运行良好:

*   50 个时代
*   0.5%辍学
*   200 个 LSTM 州大小
*   3 卷积宽度
*   那达慕乐观主义者

调整这些参数，看看你能否打破 2009 年的[最先进的分数。](https://www.ncbi.nlm.nih.gov/pubmed/20819856)

# 结论

最后，我们从下载 i2b2 2009 数据集开始，继续将 i2b2 数据解析为 CoNLL 格式，以便重用 BLSTM + CNN 模型。在这个过程中，我们了解到神经网络(在某种程度上)不知道底层数据来自哪个领域。虽然 i2b2 2009 挑战赛中的大多数系统都使用了由临床医生硬编码的手工制作的功能和规则，但没有临床知识(像我一样)但有一些机器学习知识的人可以构建一个强大的命名实体识别工具。

> 您想将此模型用于哪些文本数据？

## 奖金-计算每个注释类别的 F1 分数

如果您想深入了解，请计算每个注释类别的 F1 分数。您应该会发现，该模型在提取药物名称(m)方面表现良好，但在管理原因(r)方面表现不佳。这是有道理的:实施治疗的原因比药物名称要模糊得多。此外，可用于培训的药物名称数量超过 5 倍。

希望代码对您来说很好。如果没有，请在下面的评论中留言，我很乐意帮忙。👋