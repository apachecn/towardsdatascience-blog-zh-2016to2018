# 用 Python 进行自然语言处理

> 原文：<https://towardsdatascience.com/natural-language-processing-with-python-b3aad16f578f?source=collection_archive---------3----------------------->

有许多方法可以使用自然语言处理，也称为 NLP。在这篇博客中，我们将讨论计数矢量器，以及它在制作模型时如何有用。

让我们从什么是 NLP 开始。这是一种将单词转化为数值的方法，这样我们就可以根据这些数据进行分析并建立预测模型。

我喜欢用例子来展示，所以今天我们将根据类别用不同的电子邮件从 sklearn 中读取数据集。我们将只阅读其中的四个类别，并在它们上面练习使用 nlp。我们将制作一个模型来确定电子邮件是否属于某个类别。

首先，我们需要阅读一些字典

```
**import** **pandas** **as** **pd**
**import** **numpy** **as** **np**
**import** **matplotlib.pyplot** **as** **plt**
%matplotlib inline
 *# Getting that SKLearn Dataset* 
**from** **sklearn.datasets** **import** fetch_20newsgroups
```

这给了我们一些可以使用的库，并通过我们的电子邮件信息从 sklearn 导入数据集。

```
categories = [
    'alt.atheism',
    'talk.religion.misc',
    'comp.graphics',
    'sci.space',
]
*# Setting out training data*
data_train = fetch_20newsgroups(subset='train',categories=categories,
                                shuffle=True, random_state=42,
                                remove=('headers', 'footers', 'quotes'))
*# Setting our testing data*
data_test = fetch_20newsgroups(subset='test', categories=categories,
                               shuffle=True, random_state=42,
                               remove=('headers', 'footers', 'quotes'))
```

在这个代码块中，我们决定了我们想要的 4 个类别。然后制作我们的训练集和测试集。我们将类别设置为与上述四个类别相同，并删除了页眉、页脚和引号。

```
data_train.keys() #tells us what keys are in our datasetlen(data_train['data']) #looks at the length of the data_train
len(data_train['target'])#looks at the length of the data_test
```

我们要确保数据列和目标列的长度相等，否则在建模时会出现错误。

```
**from** **sklearn.feature_extraction.text** **import** CountVectorizer
 *# Setting the vectorizer just like we would set a model* 
cvec = CountVectorizer(stop_words='english')*# Fitting the vectorizer on our training data* cvec.fit(data_train['data'])
```

我们将 cvec 设置为等于 CountVectorizer，这样以后就可以方便地调用它了。此外，我在 stop_words = 'english '中添加了。这是用我们常用的英语单词，如“the”、“a”、“and”等。这是有用的，所以我们不要把我们的模型建立在这些实际上没有预测意义的单词上。然后，它需要安装在 data_train['data']上。

```
X_train = pd.DataFrame(cvec.transform(data_train['data']).todense(),
                       columns=cvec.get_feature_names())
```

这一步是转换训练数据。的。todense()将把它变成一个矩阵(并且很容易转换成 pandas DataFrame)。使 columns = cvec.get_feature_names()将使每一列等于我们正在分析的一个不同的单词。

```
*# Which words appear the most.*
word_counts = X_train.sum(axis=0)
word_counts.sort_values(ascending = False).head(20)
```

然后，我制作了一个单词计数器，它将查看我们转换后的 X_train 并向上计数单词，制作 sort_values(升序= False)将从最高计数的单词开始，到最低计数的单词。

```
names = data_train['target_names']
```

名称现在等于我们最初读入数据帧的 4 个类别。

```
X_test = pd.DataFrame(cvec.transform(data_test['data']).todense(),
                      columns=cvec.get_feature_names())
```

然后，我们需要像上面对 X_train 所做的那样转换我们的 X_test。

```
y_test = data_test['target']
```

并设置我们的 y_test 来测试我们的模型有多精确

```
**from** **sklearn.linear_model** **import** LogisticRegression
lr = LogisticRegression()
lr.fit(X_train, y_train) 
lr.score(X_test, y_test)
```

我们的最后一步是在 X_train 和 y_train 上拟合我们的模型，然后在 X_test 和 y_test 上测试我们的模型。lr.score(X_test，y_test)让我们知道我们的模型执行得有多好。在这种情况下，它大约有 75%准确性。