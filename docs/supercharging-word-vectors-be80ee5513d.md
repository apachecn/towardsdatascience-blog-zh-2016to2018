# 增压词向量

> 原文：<https://towardsdatascience.com/supercharging-word-vectors-be80ee5513d?source=collection_archive---------13----------------------->

## 一个在你的 NLP 项目中提升快速文本和其他单词向量的简单技术

![](img/7e446c90b7797f726d2ba76ecc34072a.png)

在过去的几年中，单词向量在创建单词间语义联系的能力方面具有变革性。现在，将这些输入到深度学习模型中进行分类或情感分析等任务已经成为常态。

尽管如此，我一直对基于词频分析的简单能力印象深刻。*术语频率逆文档频率* (TF-IDF)可以很容易地向非技术受众解释，结果也很容易解释。

> 本文探讨了如何将 TF-IDF 与单词向量相结合，从而得到既易于解释又能捕捉语言中存在的微妙语义关系的输出。

虽然互联网上充斥着使用 TF-IDF ( [见此处](https://medium.freecodecamp.org/how-to-process-textual-data-using-tf-idf-in-python-cd2bbc0a94a3))和词向量方法([见此处](/word-to-vectors-natural-language-processing-b253dd0b0817))进行“词袋”分析的例子，但这些几乎总是被视为相互排斥的技术。本文探讨了如何将 TF-IDF 与单词向量相结合，从而得到既易于解释又能捕捉语言中存在的微妙语义关系的输出。使用 fastText 方法创建单词向量，我们还将能够创建一个模型，该模型可以处理词汇表之外的单词，并对拼写错误和打字错误具有鲁棒性。

## 快速文本单词向量

本文假设读者对单词向量有一定的了解，但是值得一提的是 fastText，以及它与更广为人知的 word2vec 创建单词向量表示的方法有何不同。FastText 由脸书开发，2017 年开源了一个稳定版本。

fastText 和 word2vec 之间最明显的区别是 fastText 使用 n-gram 字符拆分单词。例如，“林肯郡”(英国的一个郡)将被分为:

```
Lin, inc, nco, col, oln, lns, nsh, shi, hir, ire
```

其中 n=3。这种方法是对 word2vec 的重大改进，原因有二:

1.  推断词汇外单词的能力。例如，上述模型会理解“兰开夏郡”(也是英国的一个县)与林肯郡相关，因为这两个词之间有“shire”(或“shi”、“hir”和“ire”)的重叠。
2.  对拼写错误和错别字的鲁棒性。很容易看出，相同的字符级建模也意味着 fastText 足够健壮，可以处理拼写变化。这在分析社交媒体内容时特别有用。

关于 fastText 如何工作的详细回顾可以点击[这里](/fasttext-under-the-hood-11efc57b2b3)查看。

## 给我看看代码！

本文的其余部分将介绍一个简单的例子，该例子将在一系列文档上训练一个 fastText 模型，将 TF-IDF 应用于这些向量，并使用它来执行进一步的分析。

有问题的文件是公司提交的现代奴隶制声明，以解释它们正在内部和供应链中采取的消除现代奴隶制的步骤。下面的文章展示了在分析之前如何清理这些数据:

[](/clean-your-data-with-unsupervised-machine-learning-8491af733595) [## 使用无监督的机器学习清理您的数据

### 清理数据不一定是痛苦的！这篇文章是一个如何使用无监督机器学习的快速例子…

towardsdatascience.com](/clean-your-data-with-unsupervised-machine-learning-8491af733595) 

如果你想继续下去，可以在这里找到包含所有代码的 colab 笔记本。

**第一步。标记文本并创建短语**

我们将使用 spaCy 将每个文档分割成一个单词列表(标记化)。我们还将通过删除停用词、标点符号并使用 Gensim 库转换为小写来清理数据:

```
#The dataframe is called 'combined' it has a column called 'text' containing the text data for each company#creates a list of documents with a list of words inside:
text = []
for i in combined.text.values:
  doc = nlp(remove_stopwords(strip_punctuation(strip_non_alphanum(str(i).lower()))))
  tokens = [token.text for token in doc]
  text.append(tokens)
```

然后我们将共同的术语粘在一起。例如，当每个文件都提到现代奴隶制时，将这两个词合并成一个短语“现代 _ 奴隶制”是很有用的。Gensim 库使这变得简单，代码如下:

```
common_terms = ["of", "with", "without", "and", "or", "the", "a"]
# Create the relevant phrases from the list of sentences:
phrases = Phrases(text, common_terms=common_terms, threshold = 10, min_count=5)
# The Phraser object is used from now on to transform sentences
bigram = Phraser(phrases)
# Applying the Phraser to transform our sentences is simply
tokens = list(bigram[text])
```

我们现在有一个文档列表，每个文档包含一个单词列表。常用短语已被组合成词。

**第二步。训练快速文本模型**

这只需要使用 Gensim 库的 fastText 模型的一行代码。您可以设置 n 元大小(“窗口”)以及向量的维度大小(“大小”)。这里，我们创建了 100 维的向量，并使用了 3:

```
model = FastText(tokens, size=100, window=3, min_count=1, iter=10, sorted_vocab=1)
```

我们可以使用以下内容来检查模型:

```
similarities = model.wv.most_similar(restrict_vocab=10000, positive=['contract'])
```

这将返回:

```
subcontract   0.9493274688720703 
sub_contract  0.9349175095558167 
contractual   0.9346154928207397 
contracts     0.9312876462936401 
contractor    0.9068889617919922
```

为了展示 fastText 如何处理不在词汇表中的单词，我们可以试着将 contract 拼错为'**contract**'。从下面，我们可以看到模型没有问题解释这个词，即使它从来没有见过！

```
contract      0.9389102458953857 
contracts     0.9058693051338196 
contrary      0.9027011394500732 
contractor    0.8995087742805481 
contractual   0.885408341884613
```

**第三步。将 TF-IDF 应用于矢量**

这一步首先使用 Scikit 为每个文档创建 TF-IDF 信息。然后，它将权重应用于文档中的每个单词向量，然后对整个文档进行平均:

```
#TF-IDF 
text = []
for i in tqdm(tokens):
  string = ' '.join(i)
  text.append(string)
tf_idf_vect = TfidfVectorizer(stop_words=None)
final_tf_idf = tf_idf_vect.fit_transform(text)
tfidf_feat = tf_idf_vect.get_feature_names()#Applying TF-IDF scores to the model vectorstfidf_sent_vectors = []; # the tfidf-w2v for each sentence/review is stored in this list
row=0;
errors=0
for sent in tqdm(tokens): # for each review/sentence
    sent_vec = np.zeros(100) # as word vectors are of zero length
    weight_sum =0; # num of words with a valid vector in the sentence/review
    for word in sent: # for each word in a review/sentence
        try:
            vec = model.wv[word]
            # obtain the tf_idfidf of a word in a sentence/review
            tfidf = final_tf_idf [row, tfidf_feat.index(word)]
            sent_vec += (vec * tfidf)
            weight_sum += tfidf
        except:
            errors =+1
            pass
    sent_vec /= weight_sum
    #print(np.isnan(np.sum(sent_vec)))tfidf_sent_vectors.append(sent_vec)
    row += 1
print('errors noted: '+str(errors))
```

现在，我们的语料库中的每个文档都有了超级 TF-IDF 单词向量！是时候测试一下了…

## 结果

现在我们有了与 TF-IDF 权重相结合的快速文本向量，使用余弦相似性来检测结果就很容易了。下面的代码根据查询字符串找到一个匹配的公司，然后根据它们的 Modern Slavery returns 生成一个最相似的公司列表。

```
compName = 'Deloitte'query = [combined.loc[combined.Company.str.contains(compName)].iloc[0]['FT_tfidf']]
query = np.array(list(query))
query = np.nan_to_num(query)vectors = np.array(list(combined.FT_tfidf.values))
vectors = np.nan_to_num(vectors)cosine_similarities = pd.Series(cosine_similarity(query, vectors).flatten())for i,j in cosine_similarities.nlargest(10).iteritems():
  print(str(i) + '-' + combined.Company.iloc[i] + " " + str(j))
```

这里的例子是专业服务公司德勤。从下面的结果我们可以看到，最相似的回报来自相关公司(即德勤自己的另一个财政年度的回报，以及 DLA 律师事务所和安永会计师事务所，另一家“四大”会计师事务所):

```
Deloitte LLP                  1.0000000000000002 
Deloitte LLP                  0.949480726712043 
DLA Piper International LLP   0.8635928917765654 
Travers Smith LLP             0.8495683187822698 
EBSCO International Inc.      0.8405915834557236 
Vink UK                       0.8356471225683573 
Ernst & Young LLP             0.8345225966043321 
IFS UK Ltd                    0.8288755547154663 
First Central Insurance       0.8279939308519769 
TalkTalk Telecom Group PLC    0.8260778120395709
```

希望这篇文章展示了将单词向量与 TF-IDF 技术结合起来可以产生强大的结果，并且也易于解释和说明。在此演练中生成的文档向量现在可以用于文本分类、聚类以及进一步分析(例如，发现公司回报如何随时间或跨行业变化的趋势)。