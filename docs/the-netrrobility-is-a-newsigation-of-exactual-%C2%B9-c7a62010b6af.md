# 可靠性是精确的新定义

> 原文：<https://towardsdatascience.com/the-netrrobility-is-a-newsigation-of-exactual-%C2%B9-c7a62010b6af?source=collection_archive---------11----------------------->

一个机器人写了这个标题。它是由递归神经网络生成的。我用互联网档案馆的回溯机抓取了 Medium 的[头条页面](https://medium.com/topic/popular)上过去一个月所有热门内容[的标题。关于 WaybackMachine 的 API 的更多信息可以在](https://web.archive.org/web/*/https://medium.com/topic/popular)[这里](https://archive.org/help/wayback_api.php)找到，我收集媒体标题的 Python 代码[这里](https://gist.github.com/allisonmorgan/9c0744edaa02fecef27286e49c4cae21)找到，生成标题的完整列表[这里](https://gist.github.com/allisonmorgan/696bd8590853b89af1df14dbaaff6728)找到。(手指交叉这篇博文迅速走红🤞)

受[一些](https://qz.com/1025049/a-computer-used-artificial-intelligence-to-create-new-dog-names/) [伟大的](https://arstechnica.com/information-technology/2017/05/an-ai-invented-a-bunch-of-new-paint-colors-that-are-hilariously-wrong/) [例子](http://www.slate.com/blogs/future_tense/2017/05/09/an_a_i_created_new_dungeons_and_dragons_spells.html)的启发，我开始自己学习如何做这件事。这篇博客文章将详细介绍所需的软件，提供如何使用代码的说明，并展示一个使用来自 [DBLP](http://dblp.uni-trier.de) 的计算机科学出版物标题的例子，这是一个在线计算机科学参考书目。TL；DR —跳到“有趣的东西”来检查生成的标题。

# 本质的

如果你想在家里尝试这个，你需要安装 [torch-rnn](https://github.com/jcjohnson/torch-rnn) (改编自 [char-rnn](https://github.com/karpathy/char-rnn) )。设置可能需要一些工作。我强烈推荐 Jeffrey Thompson 的[教程](https://www.jeffreythompson.org/blog/2016/03/25/torch-rnn-mac-install/)在 Mac 上安装。torch-rnn 和 char-rnn 都实现了[递归神经网络](https://en.wikipedia.org/wiki/Recurrent_neural_network) (RNNs)，给定训练数据(一组字符序列)学习生成文本，一个字符接一个字符。安德烈·卡帕西在这里有一篇非常详细的博客文章。

在设置好 torch-rnn(上面教程中的步骤 1-5)之后，我从 DBLP 出版物索引下载了最新的 dblp.xml.gz 文件。计算机科学出版物的这个数据集将用于训练我们的 rnn。DBLP 将其数据打包成一个巨大的 XML 文件。因为我们只需要这些出版物的原始标题，所以我们需要做一些数据预处理来抓取相关的标题。

在这里，我将从三个特定的计算机科学会议中选择会议录。通过这种方式，我们可以跨研究主题比较和对比生成的标题。我选择了[知识发现与数据挖掘会议](https://en.wikipedia.org/wiki/SIGKDD)(KDD)[神经信息处理系统会议](https://en.wikipedia.org/wiki/Conference_on_Neural_Information_Processing_Systems)(NIPS)[计算系统中人的因素会议](https://en.wikipedia.org/wiki/Conference_on_Human_Factors_in_Computing_Systems)(池)。

为了得到这些会议记录的标题，我写了[这个 Go 脚本](https://gist.github.com/allisonmorgan/8a973a86e9bd353e57e44377e1bcc3c1)。如果你想运行它，你需要[安装 Go](https://golang.org/doc/install) 和 [go 获取](https://golang.org/doc/articles/go_command.html#tmp_3)在[序言](https://gist.github.com/allisonmorgan/8a973a86e9bd353e57e44377e1bcc3c1#file-dblp-go-L3-L11)中的包。该脚本接受会议的命令行参数:

```
>>> go run dblp.go "chi"
2017/09/12 20:01:34 Reading in DBLP gzipped file
2017/09/12 20:01:34 Decoding all proceedings from file
2017/09/12 20:03:23 Done decoding
2017/09/12 20:03:23 Number of CHI titles: 13885 Average title length: 67.01498019445445
2017/09/12 20:03:23 Output data to chi.txt
```

现在，确保您的`chi.txt`文件位于您的本地副本 [torch-rnn](https://github.com/jcjohnson/torch-rnn) 的数据文件夹中。(以下步骤对应于 Jeffrey Thompson 教程中的步骤 6–8。)要为 RNN 准备数据，请运行:

```
python scripts/preprocess.py \
--input_txt data/chi.txt \
--output_h5 data/chi.h5 \
--output_json data/chi.json
```

接下来，要训练您的神经网络，运行如下内容:

```
th train.lua -input_h5 data/chi.h5 -input_json data/chi.json -seq_length 60 -batch_size 1 -gpu -1
```

(还有更多[可以指定的](https://github.com/jcjohnson/torch-rnn/blob/master/doc/flags.md)命令行标志。)这一步会花很长时间，而且“[你的电脑会变得很热](https://www.jeffreythompson.org/blog/2016/03/25/torch-rnn-mac-install/)”但是一旦这些都完成了，你就可以从你的 RNN 取样了:

```
th sample.lua -checkpoint cv/checkpoint_581150.t7 -length 2000 -gpu -1 -temperature 0.1 > data/chi_sample.txt
```

`length`标志对应于你想要输出多少字符，而`temperature`标志指定了你希望你的神经网络输出在 0-1 范围内有多新颖。更高的温度会产生很多废话，更低的温度会产生很多重复。

# 有趣的东西

好了，现在让我们考虑一下我们貌似合理的 KDD，日本和中国的头衔。请记住，当从我们的递归神经网络中采样时，我们可以指定我们希望输出有多新颖。我将展示这里选择的三个会议的两个极端。

KDD 是关于知识发现和数据挖掘的[会议](https://en.wikipedia.org/wiki/SIGKDD)，如果我们要求我们的 RNN 生成一个可预测的标题(`temperature = 0.1`，那么结果是相当可预测的:

请注意许多对[数据挖掘](https://en.wikipedia.org/wiki/Data_mining)、[分布式计算](https://en.wikipedia.org/wiki/Distributed_computing)和[顺序挖掘](https://en.wikipedia.org/wiki/Sequential_pattern_mining)的引用，这是该研究领域的典型。如果我们给我们的 RNN 更多的创作自由，一些古怪的标题就会产生:

这里有一些看似可信的 KDD 头衔的大样本，分别是低创造力的 T8 和高创造力的 T10。

接下来，NIPS 上覆盖的研究主题，[关于神经信息处理系统的会议](https://en.wikipedia.org/wiki/Conference_on_Neural_Information_Processing_Systems)，包括机器学习(特别是神经网络)和人工智能。以下是一些低创造力的标题:

正如我们所料，出现了许多对[神经元](https://en.wikipedia.org/wiki/Artificial_neuron)和[随机梯度下降](https://en.wikipedia.org/wiki/Stochastic_gradient_descent)的引用。同样，如果我们增加`temperature`，我们可以产生一些非常古怪的标题:

点击这里查看貌似合理的 NIPS 头衔的完整样本:高创造力[和低创造力](https://gist.github.com/allisonmorgan/7d864a4cb902870b8eda56ead4e726c8)。

最后，CHI，[计算系统中人的因素会议](https://en.wikipedia.org/wiki/Conference_on_Human_Factors_in_Computing_Systems)，是一个关于人机交互的会议。第一，低创意标题:

同样，这些标题有一定的意义。注意互动的许多表象，“的研究”和“的效果”。(后两者仅从 [CHI 2017](http://dblp.uni-trier.de/db/conf/chi/chi2017.html) 就返回了 18 个标题。)和现在更有创意的标题:

这里可以找到关于[低创造力](https://gist.github.com/allisonmorgan/ec8d5fec3f30bfd57f7b40db994c6189)和[高创造力](https://gist.github.com/allisonmorgan/6c5aba17084c2d4b314b5f9ca09946e1)似是而非的 CHI 标题的完整样本。

一般来说，这个练习证实了如果你给你的机器学习算法提供有偏见的数据，你会得到有偏见的结果。在这里，我们使用这些有偏见的结果来比较和对比不同会议的标题风格。我希望这个例子是有趣的，并且激励你去尝试 torch-rnn。

# 脚注

1:其他更合适的标题可能是:“数据科学家学会了一个很酷的技巧，”或者“内容战略家讨厌这个女人。”我曾经写过关于 clickbait 的文章；你可以在这里阅读《T4》。谁知道呢，你可能会用一个简单的技巧快速减肥。

2: [这个](https://gist.github.com/allisonmorgan/696bd8590853b89af1df14dbaaff6728#file-medium-txt-L218)对于一台机器来说，独立编写是一个特别可怕的条目。