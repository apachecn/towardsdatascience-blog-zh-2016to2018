# 命令行上的文本挖掘

> 原文：<https://towardsdatascience.com/text-mining-on-the-command-line-8ee88648476f?source=collection_archive---------4----------------------->

![](img/a41f7fecf2d435051575570dafeb41ef.png)

在过去的几天里，我一直在考虑写一些关于使用原始 bash 命令和正则表达式来挖掘文本的经验。当然，网上有更复杂的工具和库来处理文本，而不用写那么多行代码。例如，Python 有内置的 regex 模块“re ”,它有许多处理文本的丰富特性。另一方面,“BeautifulSoup”有很好的内置功能来清理原始网页。我使用这些工具来更快地处理大型文本语料库，并在我觉得懒得写代码时使用。

大多数时候，我更喜欢使用命令行。我在命令行上感觉很自在，尤其是当我处理文本数据的时候。在本教程中，我使用 bash 命令和 regex 来处理原始和杂乱的文本数据。我假设读者对 regex 和 bash 命令有基本的了解。

我展示了如何将 bash 命令如' grep '、' sed '、' tr '、' column '、' sort '、' uniq '、' awk '与 regex 一起使用来处理原始和混乱的文本，然后提取信息。作为一个例子，我使用了与世界图书馆合作的古登堡计划提供的莎士比亚全集。

## **先看文件**

莎士比亚作品的全部作品都可以从这个[链接](https://ocw.mit.edu/ans7870/6/6.006/s08/lecturenotes/files/t8.shakespeare.txt)下载。我下载了莎士比亚的全部作品，并将其放入一个文本文件:“Shakespeare.txt”。好了，现在让我们开始查看文件大小:

```
ls -lah shakes.txt***### Display:***
-rw-r--r--@ 1 sabber  staff   5.6M Jun 15 09:35 shakes.txt
```

ls '是 bash 命令，它列出了某个目录中的所有文件和文件夹。-l '标志显示文件类型、所有者、组、大小、日期和文件名。-a '标志用于显示所有文件，包括隐藏的文件。标志“h”-我最喜欢的标志之一，因为它显示文件大小，这是人类可读的格式。shakes.txt 的大小是 5.6 兆字节。

## **探索者正文**

好了，现在让我们读一下文件，看看里面有什么。我使用“less”和“tail”命令来浏览文件的各个部分。命令的名称说明了它们的功能。“less”用于一次一屏地查看文本文件的内容。它类似于“more ”,但具有允许在文件中向前和向后导航的扩展功能。“N”标志可用于定义行号。同样,“tail”显示了文件的最后几行。

```
less -N shakes.txt***### Display:***
      1 <U+FEFF>
      2 Project Gutenberg’s The Complete Works of William Shakespeare, by William
      3 Shakespeare
      4
      5 This eBook is for the use of anyone anywhere in the United States and
      6 most other parts of the world at no cost and with almost no restrictions
      7 whatsoever.  You may copy it, give it away or re-use it under the terms 
```

看起来前几行不是莎士比亚的作品，而是一些关于古腾堡计划的信息。同样，文件末尾也有一些与莎士比亚作品无关的行。因此，我将使用“sed”删除文件中所有不必要的行，如下所示:

```
cat shakes.txt | sed -e '149260,149689d' | sed -e '1,141d' > shakes_new.txt
```

上面的代码片段删除了尾部从 14926 到 149689 的行，然后删除了前 141 行。不想要的行包括一些关于法律权利，古腾堡的项目和工作内容的信息。

## 基本分析

现在让我们使用‘pipe |’和‘awk’对文件进行一些**统计**。

```
cat shakes_new.txt | wc | awk '{print "Lines: " $1 "\tWords: " $2 "\tCharacter: " $3 }'**### Display**
Lines: 149118 Words: 956209 Character: 5827807
```

在上面的代码中，我首先使用“cat”提取文件的整个文本，然后输入“wc”来计算行数、字数和字符数。最后，我用‘awk’来显示信息。计数和显示的方式可以用许多其他方式来完成。请随意探索其他可能的选择。

## **文本处理**

现在是清理文本以便进一步分析的时候了。清理包括，将文本转换为小写，删除所有数字，删除所有标点符号，删除高频词(停用词)。处理不限于这些步骤，这取决于目的。由于我打算展示一些基本的文本处理，所以我只关注上面的步骤。

首先，我把所有的大写字符/单词转换成小写，然后去掉所有的数字和标点符号。为了执行处理，我使用 bash 命令“tr ”,它翻译或删除文本文档中的字符。

```
cat shakes_new.txt | tr 'A-Z' 'a-z' | tr -d [:punct:] |  tr -d [:digit:] > shakes_new_cleaned.txt
```

上面的代码片段首先将整个文本转换为小写，然后删除所有标点符号和数字。上述代码的结果:

```
**### Display before:**
      1 From fairest creatures we desire increase,
      2 That thereby beauty’s rose might never die,
      3 But as the riper should by time decease,
      4 His tender heir might bear his memory:
      5 But thou contracted to thine own bright eyes,
      6 Feed’st thy light’s flame with self-substantial fuel,
      7 Making a famine where abundance lies,
      8 Thy self thy foe, to thy sweet self too cruel:
      9 Thou that art now the world’s fresh ornament,
     10 And only herald to the gaudy spring,
     11 Within thine own bud buriest thy content,
     12 And, tender churl, mak’st waste in niggarding:
     13   Pity the world, or else this glutton be,
     14   To eat the world’s due, by the grave and thee. **### Display after:** 1 from fairest creatures we desire increase
      2 that thereby beautys rose might never die
      3 but as the riper should by time decease
      4 his tender heir might bear his memory
      5 but thou contracted to thine own bright eyes
      6 feedst thy lights flame with selfsubstantial fuel
      7 making a famine where abundance lies
      8 thy self thy foe to thy sweet self too cruel
      9 thou that art now the worlds fresh ornament
     10 and only herald to the gaudy spring
     11 within thine own bud buriest thy content
     12 and tender churl makst waste in niggarding
     13   pity the world or else this glutton be
     14   to eat the worlds due by the grave and thee
```

**标记化**是自然语言处理中的基本预处理之一。标记化可以在单词或句子级别上执行。在本教程中，我将展示如何对文件进行标记。在下面的代码中，我首先使用“cat”提取干净的文本，然后使用“tr”及其两个标志:“s”和“c”将每个单词转换成行。

```
cat shakes_new_cleaned.txt | tr -sc ‘a-z’ ‘\12’ > shakes_tokenized.txt**### Display (First 10 words)** 1 from
      2 fairest
      3 creatures
      4 we
      5 desire
      6 increase
      7 that
      8 thereby
      9 beautys
     10 rose
```

现在我们已经把所有的单词都标记出来了，我们可以回答这样一个问题，在整个莎士比亚作品中最常用/最不常用的单词是什么？为此，我首先使用' sort '命令对所有单词进行排序，然后使用带有'-c '标志的' uniq '命令找出每个单词的频率。“uniq -c”与 Pandas 或 SQL 中的“groupby”相同。最后，根据单词出现的频率，按升序(最不频繁)或降序(最频繁)对单词进行排序。

```
*cat shakes_tokenized.txt | sort | uniq -c | sort -nr > shakes_sorted_desc.txt***### Display**29768 the   28276 and  21868 i   20805 to  18650 of  15933 a      14363 you   13191 my   11966 in  11760 that*cat shakes_tokenized.txt | sort | uniq -c | sort -n > shakes_sorted_asc.txt***### Display**1 aarons       1 abandoner    1 abatements     1 abatfowling          1 abbominable  1 abaissiez    1 abashd         1 abates              1 abbeys       1 abbots
```

上述结果揭示了一些有趣的观察结果。例如，十个最常用的单词不是代词就是介词或连词。如果我们想找出更多关于作品的抽象信息，我们必须去掉所有的停用词(介词、代词、连词、情态动词等。).也要看对象的用途。人们可能只对介词感兴趣。在这种情况下，保留所有的介词是没问题的。另一方面，最不常用的词是“放弃”、“减少”、“羞愧”。

## **删除停止字**

在下一步中，我将展示“awk”的用法，以删除命令行中的所有停用词。在本教程中，我使用了 [NLTK 的英语停用词列表](https://gist.github.com/sebleier/554280)。我还增加了几个单词。以下代码的细节可以在[这个 StackOverflow](https://stackoverflow.com/questions/3978626/shell-to-filter-prohibited-words-on-a-file) 的回答中找到。awk 不同选项的详细信息也可以在 awk 的手册中找到(命令行上的‘man awk ’)

```
awk ‘FNR==NR{for(i=1;i<=NF;i++)w[$i];next}(!($1 in w))’ stop_words.txt *shakes_tokenized*.txt > shakes_stopwords_removed.txt
```

好了，去掉停用词后，让我们像上面一样按升序和降序排列单词。

```
*cat* shakes_stopwords_removed*.txt | sort | uniq -c | sort -nr > shakes_sorted_desc.txt***### Display most frequent**3159 lord   2959 good  2924 king  2900 sir
2634 come   2612 well  2479 would 2266 love
2231 let    2188 enter*cat* shakes_stopwords_removed*.txt | sort | uniq -c | sort -n > shakes_sorted_asc.txt***### Display least frquent**1 aarons       1 abandoner    1 abatements     1 abatfowling          1 abbominable  1 abaissiez    1 abashd         1 abates              1 abbeys       1 abbots
```

我们看到莎士比亚最常用的词是“主”，后面是“好”。“爱”这个词也是出现频率最高的词。最不常用的词保持不变。语言学或文学专业的学生可能会从这些简单的分析中解读信息或获得更好的洞察力。

## **我们来讨论一下**

由于我们已经完成了一些必要的处理和清理，在下一篇教程中，我将讨论如何执行一些高级分析。在那之前，如果你有任何问题，请随意提问。看到错别字、错误或者有更好的建议请评论。你可以联系我:

```
Email: sabbers@gmail.com
LinkedIn: [https://www.linkedin.com/in/sabber-ahamed/](https://www.linkedin.com/in/sabber-ahamed/)
Github: [https://github.com/msahamed](https://github.com/msahamed)
Medium: [https://medium.com/@sabber/](https://medium.com/@sabber/)
```