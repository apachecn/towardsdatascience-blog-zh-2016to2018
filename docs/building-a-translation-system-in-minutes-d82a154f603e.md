# 在几分钟内建立一个翻译系统

> 原文：<https://towardsdatascience.com/building-a-translation-system-in-minutes-d82a154f603e?source=collection_archive---------1----------------------->

## 使用 OpenNMT-py 创建基线 NMT 模型

序列到序列(seq2seq)[1]是一种通用的结构，能够做许多事情(语言翻译、文本摘要[2]、视频字幕[3]等)。).关于 seq2seq 的简短介绍，下面是一些不错的帖子:[【4】](https://medium.com/towards-data-science/sequence-to-sequence-model-introduction-and-concepts-44d9b41cd42d)[【5】](https://medium.com/@devnag/seq2seq-the-clown-car-of-deep-learning-f88e1204dac3)。

肖恩·罗伯逊的[教程笔记本【6】](https://github.com/spro/practical-pytorch/blob/master/seq2seq-translation/seq2seq-translation.ipynb)和杰瑞米·霍华德的讲座[【6】](http://course.fast.ai/lessons/lesson12.html)[【7】](http://course.fast.ai/lessons/lesson13.html)是牢牢掌握 seq2seq 技术细节的绝佳起点。然而，在处理现实世界的问题时，我会尽量避免自己实现所有这些细节。重新发明轮子通常不是一个好主意，尤其是当你对这个领域非常陌生的时候。我发现 OpenNMT 项目非常活跃，有很好的文档，并且可以开箱即用:

[](http://opennmt.net/) [## OpenNMT -开源神经机器翻译

### OpenNMT 是一个工业级的开源(MIT)神经机器翻译系统，利用 Torch/ PyTorch…

opennmt.net](http://opennmt.net/) 

还有一些更通用的框架[(例如，[8]](https://github.com/google/seq2seq) )，但是可能需要一些定制来使它适用于您的特定问题。

OpenNMT 有两个官方版本:

> [OpenNMT-Lua](https://github.com/OpenNMT/OpenNMT) (又名 OpenNMT):用 [LuaTorch](http://torch.ch) 开发的主项目。
> 优化稳定的代码，适合生产和大规模实验。
> 
> [OpenNMT-py](https://github.com/OpenNMT/OpenNMT-py) :使用 [PyTorch](http://pytorch.org) 的 OpenNMT 的轻量版。
> 最初由脸书人工智能研究团队创建，作为 PyTorch 的一个样本项目，这个版本更容易扩展，适合于研究目的，但不包括所有功能。

在接下来的部分中，我们将使用 PyTorch 版本。我们将带您了解使用中等规模的数据集创建一个非常基本的翻译系统所需的步骤。

## 步骤 1:获取 OpenNMT-py

将 Github 上的 OpenNMT-py git 存储库克隆到本地文件夹中:

[](https://github.com/OpenNMT/OpenNMT-py) [## OpenNMT/OpenNMT-py

### http://opennmt.net/ py torch 中的 OpenNMT-py -开源神经机器翻译

github.com](https://github.com/OpenNMT/OpenNMT-py) 

如果您打算以后定制或扩展它，那么您可能希望在 Github 上派生存储库。自述文件中还建议:

> Codebase 接近稳定的 0.1 版本。如果你想要稳定的代码，我们目前推荐分叉。

## 步骤 2:下载数据集

这里我们将使用来自[AI Challenger——英汉机器翻译竞赛](https://challenger.ai/competition/translation/subject?lan=en)的数据集。这是一个拥有 1000 万对英汉句子的数据集。英语字幕是从英语学习网站和电影字幕中提取的会话英语。从我的了解来看，大部分的翻译都是爱好者提交的，不一定是专业人士。翻译的中文句子由人工注释者检查。

[](https://challenger.ai/competition/translation/subject?lan=en) [## 英汉机器翻译- AI 挑战者

### 英汉机器翻译-奖金:30 万-提高英汉机器翻译的性能…

challenger.ai](https://challenger.ai/competition/translation/subject?lan=en) 

下载数据集需要注册帐户，可能还需要进行身份验证(不记得后者是否是强制性的)。如果这对你来说是个问题，你可以试试来自 WMT17 的[数据集。](http://www.statmt.org/wmt17/translation-task.html#download)

AI Challenger 数据集存在一些问题:1 .翻译的质量不一致。2.因为许多句子来自电影字幕，所以翻译通常依赖于上下文(与上一句或下一句相关)。但是，数据集中没有可用的上下文信息。

让我们看看现成的模型在这个数据集上的表现。由于内存限制，我将数据集下采样到**100 万个**句子。

(我们假设您将数据集放入 OpenNMT 根目录下的文件夹 ***挑战者*** 。)

## 步骤 3:将数据集转换为纯文本

验证和测试数据集采用 XML 格式。我们需要将其转换为纯文本文件，其中一行由一个句子组成。一个简单的方法就是使用 BeautifulSoup。下面是一段示例代码:

```
with open(input_file, "r") as f:
    soup = BeautifulSoup(f.read(), "lxml")
    lines = [
      (int(x["id"]), x.text.strip()) for x in soup.findAll("seg")]
    # Ensure the same order
    lines = sorted(lines, key=lambda x: x[0])
```

## 第四步:标记英语和汉语句子

输入的句子必须用空格分隔的标记来标记。

对于英语来说，有一些标记符可供选择。一个例子是`nltk.tokenize.word_tokenize`:

```
with open(output_file, "w") as f:
    f.write(
        "\n".join([
             " ".join(word_tokenize(l[1]))
             for l in lines
         ])
    )
```

它变成“这是一个整洁的一-二。沃克给伯顿。”变成“这是一个整洁的一-二。沃克呼叫伯顿。".

对于中文，我们使用最简单的字符级记号化，也就是把每个字符当作一个记号:

```
with open(output_file, "w") as f:
    f.write(
        "\n".join([
            " ".join([c if c != " " else "<s>" for c in l[1]])
            for l in lines
        ])
    )
```

It turns “我就一天 24 小时都得在她眼皮子底下。” into “我 就 一 天 2 4 小 时 都 得 在 她 眼 皮 子 底 下 。”. (Note because the token are space-separated, we need a special token “<s>” to represent the space characters.)

(我没有提供第 3 步和第 4 步的完整代码，因为这真的是初级 Python 编程。你应该能够独立完成这些任务。)

## 步骤 5:预处理数据集

只需在根目录下运行以下命令:

```
python preprocess.py -train_src challenger/train.en.sample \
      -train_tg challenger/train.zh.sample \
      -valid_src challenger/valid.en \
      -valid_tgt challenger/valid.zh  \
      -save_data challenger/opennmt -report_every 10000
```

预处理脚本将遍历数据集，跟踪标记频率，并构建一个词汇表。我在这里遇到了内存问题，不得不将训练数据集向下采样到一百万行，但是我认为原始数据集经过一些优化后应该适合 16GB 的内存。

## 第六步:训练模型

```
python train.py -data challenger/opennmt \
    -save_model models/baseline -gpuid 0 \
    -learning_rate 0.001 -opt adam -epochs 20
```

它会用你的第一个 GPU 来训练一个模型。默认的模型结构是:

```
NMTModel (
  (encoder): RNNEncoder (
    (embeddings): Embeddings (
      (make_embedding): Sequential (
        (emb_luts): Elementwise (
          (0): Embedding(50002, 500, padding_idx=1)
        )
      )
    )
    (rnn): LSTM(500, 500, num_layers=2, dropout=0.3)
  )
  (decoder): InputFeedRNNDecoder (
    (embeddings): Embeddings (
      (make_embedding): Sequential (
        (emb_luts): Elementwise (
          (0): Embedding(6370, 500, padding_idx=1)
        )
      )
    )
    (dropout): Dropout (p = 0.3)
    (rnn): StackedLSTM (
      (dropout): Dropout (p = 0.3)
      (layers): ModuleList (
        (0): LSTMCell(1000, 500)
        (1): LSTMCell(500, 500)
      )
    )
    (attn): GlobalAttention (
      (linear_in): Linear (500 -> 500)
      (linear_out): Linear (1000 -> 500)
      (sm): Softmax ()
      (tanh): Tanh ()
    )
  )
  (generator): Sequential (
    (0): Linear (500 -> 6370)
    (1): LogSoftmax ()
  )
)
```

源语料库和目标语料库的词汇量分别为 50，002 和 6，370。源词汇明显被截断到 5 万。目标词汇量相对较小，因为没有那么多常用汉字。

## 步骤 7:翻译测试/验证句子

```
python translate.py \
    -model models/baseline_acc_58.79_ppl_7.51_e14  \
    -src challenger/valid.en -tgt challenger/valid.zh \
    -output challenger/valid_pred.58.79 -gpu 0 -replace_unk
```

把`models/baseline_acc_58.79_ppl_7.51_e14`换成自己的型号。模型命名要明显:*这是一个经过 14 个历元训练的模型，在验证集*上的准确率为 58.79，困惑度为 7.51。

您也可以使用以下公式计算 BLEU 分数:

```
wget [https://raw.githubusercontent.com/moses-smt/mosesdecoder/master/scripts/generic/multi-bleu.perl](https://raw.githubusercontent.com/moses-smt/mosesdecoder/master/scripts/generic/multi-bleu.perl)perl multi-bleu.perl challenger/valid.zh \
     < challenger/valid_pred.58.79
```

现在你有一个工作的翻译系统！

## 步骤 8:(可选)对输出进行去爆震和转换

如果你想把翻译提交给 AI Challenger，你需要把步骤 4 反过来，然后是步骤 3。同样，它们应该很容易实现。

## 一些例子

English: You knew it in your heart you haven’t washed your hair
Chinese(pred): 你心里清楚你没洗头发
Chinese(gold): 你心里知道你压根就没洗过头

English: I never dreamed that one of my own would be going off to a University, but here I stand,
Chinese(pred): 我从来没梦到过我的一个人会去大学，但是我站在这里，
Chinese(gold): 我从没想过我的孩子会上大学，但我站在这，

English: We just don’t have time to waste on the wrong man.
Chinese(pred): 我们只是没时间浪费人。
Chinese(gold): 如果找错了人我们可玩不起。

以上三个例子，从上到下分别是*语义正确*、*部分正确*、*完全无法理解*。在检查了几个例子后，我发现大多数机器翻译的句子是部分正确的，而且语义正确的句子数量惊人。考虑到我们到目前为止付出的努力如此之少，这个结果还不错。

## 下一步

如果你提交结果，你应该绕过 ***.22*** BLEU。目前最高 BLEU 分数为 ***.33*** ，有很大的提升空间。您可以查看根文件夹中的`opts.py`,了解更多内置模型参数。或者深入代码库，弄清楚事情是如何工作的，哪里可以改进。

其他途径包括对中文句子进行分词、添加命名实体识别、使用发音词典[【10】](http://www.speech.cs.cmu.edu/cgi-bin/cmudict)对未见过的英文名称进行猜测翻译等。

(2017/10/14 更新:如果你使用 [jieba](https://github.com/fxsjy/jieba) 和 jieba.cut 默认设置对中文句子进行分词，你会绕过 ***.20*** BLEU 上的公共 leaderboad。得分下降的一个可能原因是它的中文词汇量大得多。从输出中的 *< unk >* 的数量就可以看出来。)

## 参考资料:

1.  [Sutskever，I .、Vinyals，o .、& Le，Q. V. (2014 年)。用神经网络进行序列对序列学习](https://papers.nips.cc/paper/5346-sequence-to-sequence-learning-with-neural-networks.pdf)。
2.  【纳尔拉帕提，r .】，周，b .【多斯桑托斯，c .】向，B. (2016)。使用序列到序列的 RNNs 和超越序列的抽象文本摘要。
3.  [Venugopalan，s .、Rohrbach，m .、Donahue，j .、Mooney，r .、Darrell，t .、& Saenko，K. (2015)。序列到序列](http://arxiv.org/abs/1505.00487)
4.  [序列对序列模型:介绍和概念](https://medium.com/towards-data-science/sequence-to-sequence-model-introduction-and-concepts-44d9b41cd42d)
5.  [seq2seq:深度学习的小丑车](https://medium.com/@devnag/seq2seq-the-clown-car-of-deep-learning-f88e1204dac3)
6.  [实用 PyTorch:用一个序列翻译成序列网络及注意事项](https://github.com/spro/practical-pytorch/blob/master/seq2seq-translation/seq2seq-translation.ipynb)
7.  [程序员的前沿深度学习，第 2 部分，第 12 讲——注意力模型](http://course.fast.ai/lessons/lesson12.html)
8.  [面向编码人员的前沿深度学习，第 2 部分，第 13 讲——神经翻译](http://course.fast.ai/lessons/lesson13.html)
9.  [Google/seq 2 seq:tensor flow 的通用编解码框架](https://github.com/google/seq2seq)
10.  [CMU 发音词典](http://www.speech.cs.cmu.edu/cgi-bin/cmudict)