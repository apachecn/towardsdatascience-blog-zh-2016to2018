# 自然语言处理初学者指南

> 原文：<https://towardsdatascience.com/a-beginners-guide-to-natural-language-processing-e21e3e016f84?source=collection_archive---------0----------------------->

当我开始作为一名数据科学家的旅程时，最吸引人的是寻求理解单词的意义和影响的自然语言处理(NLP)。

NLP 最伟大的方面之一是它跨越了计算研究的多个领域，从人工智能到计算语言学，都在研究计算机和人类语言之间的交互。它主要关注编程计算机准确快速地处理大量的自然语言语料库。什么是自然语言语料库？它是对现实世界语言所表达的语言的研究。它是一种理解文本中一组抽象规则以及语言与另一种语言之间关系的综合方法。

虽然 NLP 在现代计算机工业革命中变得更加重要，但它实际上是令人惊叹的艾伦·图灵的大脑产物，他在帮助破解德国 Enigma 编码机器的同时，还写了一篇题为“[计算机器和智能](http://loebner.net/Prizef/TuringArticle.html)”的文章，该文章首次提出了将人类语言与计算机联系起来的严肃用途。随着技术在我们日常生活中的无处不在，我们已经看到了自然语言处理通过革命性的工具如谷歌翻译、IBM 沃森、语音识别和生成以及情感分析对我们日常生活的影响。

然而，像所有的事情一样，NLP 也有一些问题和缺点。当一个人说话时，它努力产生自然流动的语言，就像你读一部糟糕的电影剧本时一样，听起来像是计算机在说话。虽然有一些方法可以尝试理解语调的变化，但 NLP 仍然难以理解讽刺和幽默。然而，这是一个需要深入研究的领域，我期待着“讽刺突破”出现的那一天。不为别的，就是为了更好地理解朋友偶尔发来的短信或即时消息。

目前可用的一些最有用的计算机/数据科学工具有:

计数向量化、哈希向量化、词频-逆文档频率(TF-IDF)、词汇化、词干分析、解析和情感分析。

[CounterVectorization](http://scikit-learn.org/stable/modules/generated/sklearn.feature_extraction.text.CountVectorizer.html) 是一个 SciKitLearn 库工具，它获取任意数量的文本，并将每个唯一的单词作为一个特征返回，并计算该单词出现的次数。虽然这可以生成许多特性，但这些特性是一些非常有用的参数，有助于避免这种情况，包括停用词、n_grams 和 max_features。停用字词会生成一个不会作为特征包括在内的字词列表。它的主要用途是在“英语”词典中删除无关紧要的单词，如“is，The，a，it，as ”,这些单词可能会频繁出现，但对我们的最终目标几乎没有影响。Ngram_range 选择如何将单词组合在一起。不是让 NLP 分别返回每个单词，而是如果它等于 2，您可以得到类似“Hello again”或“See you later”的结果，如果它等于 3。Max_features 是您选择创建的要素数量。如果你选择它等于无，这意味着你会得到所有的单词作为特征，但如果你设置它等于 50，你只会得到 50 个最常用的单词。

[哈希矢量器](http://scikit-learn.org/stable/modules/generated/sklearn.feature_extraction.text.HashingVectorizer.html)使用“哈希技巧”将文本转换为出现矩阵，每个单词映射到一个特征，使用哈希函数将其转换为哈希。如果该单词在文本主体中再次出现，则它被转换成相同的特征，这允许我们在相同的特征中对其进行计数，而无需在存储器中保留字典。

[TF-IDF](http://scikit-learn.org/stable/modules/generated/sklearn.feature_extraction.text.HashingVectorizer.html) 揭示了在不同的文本主体中，哪些词最有区别。如果您试图看到在一个文档中多次出现但在其他文档中没有出现的单词之间的差异，这将特别有帮助，从而使您可以解释该文档的一些特殊之处。它取决于术语频率、一个单词出现的频率以及逆文档频率，即它在所有文档中是唯一的还是共有的。

[词汇化](http://scikit-learn.org/stable/modules/feature_extraction.html)是将单词的屈折形式组合在一起作为单个方面进行分析的过程。它是一种利用一个词的预期意义来确定“引理”的方法。这在很大程度上取决于在一个句子、段落或更大的文档中正确地找到“预期的词类”和一个词的真正含义。例如，“run”是“running”或“ran”等词的基本形式，或者“better”和“good”在同一个词条中，因此它们被认为是相同的。

词干与词汇归类非常相似，它将单词组合在一起，但与词汇归类不同的是，它将一个单词引用回它的基本形式或词根形式。事实上，在我遇到的描述它的最好的例子中，涉及到词干回溯到它的基本形式。“词干”、“词干化”、“词干化”、“词干化”都是基于单个词“词干”。

[解析](https://en.wikipedia.org/wiki/Parsing#Computer_languages)一般用途是分析一串单词以形成语法规则。对于 NLP 来说，它是对一串单词的分析，产生一个解析树，揭示单词之间的句法关系，它可以包含语义。然而，解析的一个缺点是，何时解析和解析什么完全取决于用户，因为任何段落都可以以他们选择的任何方式解析，从单个字符到整个句子等等。

[情感分析](http://www.nltk.org/howto/sentiment.html)最简单的定义是从一个文档或一组文档中提取单词的解释或主观含义，以确定特定单词或一组单词的“态度”。它最大的用途之一是在脸书、推特和其他社交媒体上识别公众意见的趋势，一个词在特定音调环境中出现的频率，以及解释一组词的音调。

在做任何和所有这些分析之前，我强烈建议您过滤和清理您正在处理的任何文档。我遇到的最好的函数之一是由我的一位老师 Mark Mummert 创建的。虽然它的重点是通过使用漂亮的汤清理 html 文件，但它可以很容易地适用于其他 NLP 清理。

```
def review_to_words(raw_review):
      #Remove html
      review_text=BeautifulSoup(raw_review).get_text()
      #Remove non-letters - Utilized Regex Library
      letters_only = re.sub("[^a-zA-Z]", " ", review_text)
      #Conver to lowercase and split into individual words
      words = letters_only.lower().split()
      #Convert Stop-Words to a set and remove them
      stops = set(stopwords.words("english"))
      meaningful_words = [w **for** w **in** words **if** **not** w **in**    stops]
      #Join the Words back into one string seperated by a  space and return the result.
      **return**( " ".join( meaningful_words ))
```

NLP 的世界是巨大而复杂的，并且仍然是一个需要大量研究的领域。有许多有用的工具，所有这些工具在解释和利用文字世界方面都有极其有益的用途。

对于 python 用户来说，我相信你对 NLP 最有用的资源是 [Sci-Kit Learn 库](http://scikit-learn.org/stable/modules/feature_extraction.html)和 [NLTK 库](http://www.nltk.org/)。