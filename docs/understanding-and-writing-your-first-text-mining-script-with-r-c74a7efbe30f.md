# 理解并使用 R 编写您的第一个文本挖掘脚本

> 原文：<https://towardsdatascience.com/understanding-and-writing-your-first-text-mining-script-with-r-c74a7efbe30f?source=collection_archive---------3----------------------->

![](img/74de569e003d50faa8371e61fe227850.png)

## 介绍

数据科学变得流行的原因之一是因为它能够在瞬间或仅仅一个查询中揭示大量数据集的信息。

仔细想想，每天我们以文本的形式给出了多少信息？所有这些信息包含了我们的情感、我们的观点、我们的计划、建议、我们最喜欢的短语等等。

然而，揭示其中的每一个看起来就像是大海捞针，直到我们使用像文本挖掘/分析这样的技术。

文本挖掘考虑了信息检索、词频分析和研究以及模式识别，以帮助可视化和预测分析。

在本文中，我们将经历数据集为进一步分析做准备的主要步骤。我们将使用 R 编写脚本，代码将在 R studio 中编写。

为了实现我们的目标，我们将使用一个名为“tm”的 R 包。这个软件包支持所有的文本挖掘功能，如加载数据，清理数据和建立一个术语矩阵。它在 CRAN 上有售。

## 让我们首先在我们的工作空间中安装并加载这个包。

```
#downloading and installing the package from CRAN
install.packages("tm")#loading tm
library(tm)
```

## 加载数据

要挖掘的文本可以从不同的源格式加载到 R 中。它可以来自文本文件(。txt)、pdf(。pdf)、csv 文件(。csv) e.t.c，但不管源格式如何，要在 tm 包中使用它，就要把它变成一个“语料库”。

语料库被定义为“书面文本的集合，尤其是特定作者的全部作品或关于特定主题的写作主体”。

tm 包使用 Corpus()函数创建一个语料库。

```
#loading a text file from local computer
newdata<- readlines(filepath)#Load data as corpus
#VectorSource() creates character vectorsmydata <- Corpus(VectorSource(newdata))
```

参考本[指南](http://www.r-tutor.com/r-introduction/data-frame/data-import)了解更多关于导入文件到 r。

## 正在清理数据。

一旦我们成功地将数据加载到工作空间中，就该清理这些数据了。我们在这一步的目标是从数据文件中创建独立的术语(单词),然后才能开始计算它们出现的频率。

因为 R 是区分大小写的，我们应该首先将整个文本转换成小写，以避免认为相同的单词“write”和“Write”不同。

我们将删除:网址，表情符号，非英语单词，标点符号，数字，空白和停用词。

停用词:tm 包中常用的英文单词如“a”、“is”、“The”都称为停用词。为了使结果更准确，必须去掉这些词。也可以创建自己的自定义停用词。

```
# convert to lower case
mydata <- tm_map(mydata, content_transformer(tolower))#remove ������ what would be emojis
mydata<-tm_map(mydata, content_transformer(gsub), pattern="\\W",replace=" ")# remove URLs
removeURL <- function(x) gsub("http[^[:space:]]*", "", x)
mydata <- tm_map(mydata, content_transformer(removeURL)
)
# remove anything other than English letters or space
removeNumPunct <- function(x) gsub("[^[:alpha:][:space:]]*", "", x)
mydata <- tm_map(mydata, content_transformer(removeNumPunct))# remove stopwords
mydata <- tm_map(mydata, removeWords, stopwords("english"))#u can create custom stop words using the code below.
#myStopwords <- c(setdiff(stopwords('english'), c("r", "big")),"use", "see", "used", "via", "amp")
#mydata <- tm_map(mydata, removeWords, myStopwords)# remove extra whitespace
mydata <- tm_map(mydata, stripWhitespace)# Remove numbers
mydata <- tm_map(mydata, removeNumbers)# Remove punctuations
mydata <- tm_map(mydata, removePunctuation)
```

## **词干**

词干提取是将相似来源的单词聚集成一个单词的过程，例如“通信”、“交流”、“沟通”。词干分析通过删除后缀和将单词简化为基本形式来帮助我们提高挖掘文本的准确性。我们将使用雪球图书馆。

```
library(SnowballC)mydata <- tm_map(mydata, stemDocument)
```

## 构建术语矩阵并揭示词频

在清理过程之后，我们剩下的是存在于整个文档中的独立术语。这些都存储在一个矩阵中，显示它们的每一次出现。这个矩阵记录了术语在我们的干净数据集中出现的次数，因此被称为**术语矩阵**。

```
#create a term matrix and store it as dtm
dtm <- TermDocumentMatrix(mydata)
```

**词频**:词在数据集中出现的次数。使用术语矩阵中出现的汇编，词频将向我们指示从数据集中最频繁使用的词到最少使用的词。

## 结论

我们刚刚写了一个基本的文本挖掘脚本，然而这只是文本挖掘的开始。获取原始格式的文本并将其清理到这一点的能力将为我们提供方向，如**构建单词云、情感分析**和**构建模型。**

保留这个脚本，因为当我们开始进行情感分析时，它会派上用场。

有任何问题都可以联系我> [@lornamariak](https://twitter.com/lornamariak) 。