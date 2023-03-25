# 使用 TensorFlow 创建拼写检查

> 原文：<https://towardsdatascience.com/creating-a-spell-checker-with-tensorflow-d35b23939f60?source=collection_archive---------2----------------------->

![](img/ea332960a453eb2ea64cb945692855ca.png)

Soon typos will be a thing of the past!

机器学习最重要的一个方面是处理好干净的数据。自然语言进展项目存在使用人类书写的文本的问题，不幸的是，我们不擅长书写。想想在 Reddit 的帖子和评论数据集中会有多少拼写错误。出于这个原因，我认为一个非常有价值的项目是做一个拼写检查器，这将有助于缓解这些问题。

我们将在这个项目中使用的模型非常类似于我在我的文章“使用亚马逊评论的文本摘要”(两者都是 seq2seq 模型)中所写的模型，但是我添加了一些额外的代码行，以便可以使用网格搜索来调整架构和超参数，并且可以使用 TensorBoard 来分析结果。如果你想更详细地了解如何将 TensorBoard 添加到你的代码中，那么看看“[用 TensorFlow 和 TensorBoard](https://medium.com/@Currie32/predicting-movie-review-sentiment-with-tensorflow-and-tensorboard-53bf16af0acf) 预测电影评论情绪”。

本文的主要焦点将是如何为模型准备数据，我还将讨论模型的其他一些特性。我们将在这个项目中使用 Python 3 和 TensorFlow 1.1。数据由来自[古腾堡计划](http://www.gutenberg.org/ebooks/search/?sort_order=downloads)的二十本流行书籍组成。如果你有兴趣扩大这个项目，使它更加准确，有数百本书可以在古腾堡项目上下载。另外，看看用这个模型可以做多好的拼写检查也是很有趣的。

*查看完整代码，这里是其* [*GitHub 页面*](https://github.com/Currie32/Spell-Checker) *。*

为了让您预览这种模式的能力，这里有一些精选的示例:

*   ***拼写*** *比较难，****whch****就是****wyh****你需要每天学习。*
*   **拼写**难，**哪个**是**为什么**你需要每天学习。
*   *她在****th****country****vrey****为多莉卖命。*
*   对多莉来说，她在 T4 这个国家的最初几天非常艰难。
*   ***Thi*** *真是了不起****impression iv*******thaat****我们应该马上调查一下！**
*   *这个确实是令人印象深刻的****我们应该马上调查一下**！***

***为了让事情更有条理，我把所有我们会用到的书都放在他们自己的文件夹里，叫做“书”。下面是我们将用来加载所有书籍的函数:***

```
***def load_book(path):
    input_file = os.path.join(path)
    with open(input_file) as f:
        book = f.read()
    return book***
```

***我们还需要每本书的唯一文件名。***

```
***path = './books/'
book_files = [f for f in listdir(path) if isfile(join(path, f))]
book_files = book_files[1:]***
```

***当我们将这两个代码块放在一起时，我们将能够将所有书籍中的文本加载到一个列表中。***

```
***books = []
for book in book_files:
    books.append(load_book(path+book))***
```

***如果您想知道每本书有多少单词，您可以使用以下代码行:***

```
***for i in range(len(books)):
    print("There are {} words in {}.".format(len(books[i].split()), book_files[i]))***
```

****注:如果不包含* `.split()` *，则返回每本书的人物数量。****

***清理这些书的正文相当简单。因为我们将使用字符而不是单词作为模型的输入，所以我们不需要担心删除停用的单词，或者将单词缩短到它们的词干。我们只需要删除不想包含的字符和多余的空格。***

```
***def clean_text(text):
    '''Remove unwanted characters and extra spaces from the text'''
    text = re.sub(r'\n', ' ', text) 
    text = re.sub(r'[{}[@_](http://twitter.com/_)*>()\\#%+=\[\]]','', text)
    text = re.sub('a0','', text)
    text = re.sub('\'92t','\'t', text)
    text = re.sub('\'92s','\'s', text)
    text = re.sub('\'92m','\'m', text)
    text = re.sub('\'92ll','\'ll', text)
    text = re.sub('\'91','', text)
    text = re.sub('\'92','', text)
    text = re.sub('\'93','', text)
    text = re.sub('\'94','', text)
    text = re.sub('\.','. ', text)
    text = re.sub('\!','! ', text)
    text = re.sub('\?','? ', text)
    text = re.sub(' +',' ', text) # Removes extra spaces
    return text***
```

***我将跳过如何制作`vocab_to_int`和`int_to_vocab`字典，因为这是非常标准的东西，你可以在这个项目的 [GitHub 页面](https://github.com/Currie32/Spell-Checker)上找到。但是，我认为有必要向您展示输入数据中包含的字符:***

```
***The vocabulary contains 78 characters.
[' ', '!', '"', '$', '&', "'", ',', '-', '.', '/', '0', '1', '2', '3', '4', '5', '6', '7', '8', '9', ':', ';', '<EOS>', '<GO>', '<PAD>', '?', 'A', 'B', 'C', 'D', 'E', 'F', 'G', 'H', 'I', 'J', 'K', 'L', 'M', 'N', 'O', 'P', 'Q', 'R', 'S', 'T', 'U', 'V', 'W', 'X', 'Y', 'Z', 'a', 'b', 'c', 'd', 'e', 'f', 'g', 'h', 'i', 'j', 'k', 'l', 'm', 'n', 'o', 'p', 'q', 'r', 's', 't', 'u', 'v', 'w', 'x', 'y', 'z']***
```

***我们可以删除更多的特殊字符，或者让文本全部小写，但是我想让这个拼写检查器尽可能有用。***

***数据在输入模型之前会被组织成句子。我们将拆分每个时间段的数据，每个时间段后面跟一个空格(“.”).其中一个问题是，有些句子以问号或感叹号结尾，但我们没有考虑到这一点。幸运的是，我们的模型仍然能够学习问号和感叹号的用法，只要这两个句子加起来没有最大句子长度长。***

***仅举一个例子:***

*   ***今天是美好的一天。我想去海滩。*(这将被分成两个输入句子)****
*   ***今天天气好吗？我想去海滩。*(这将是一个长输入句子)****

```
***sentences = []
for book in clean_books:
    for sentence in book.split('. '):
        sentences.append(sentence + '.')***
```

***我在 loydhub.com 的 T2 使用 GPU 来训练我的模型(我强烈推荐他们的服务)，这为我节省了几个小时的训练时间。尽管如此，为了正确地调优这个模型，仍然需要 30-60 分钟来运行一次迭代，这就是为什么我限制了数据，以便它不需要更长的时间。这当然会降低我们模型的准确性，但由于这只是个人项目，所以我不介意权衡。***

```
***max_length = 92
min_length = 10good_sentences = []for sentence in int_sentences:
    if len(sentence) <= max_length and len(sentence) >= min_length:
        good_sentences.append(sentence)***
```

***为了跟踪这个模型的性能，我将把数据分成训练集和测试集。测试集将由 15%的数据组成。***

```
***training, testing = train_test_split(good_sentences, 
                                     test_size = 0.15, 
                                     random_state = 2)***
```

***就像我最近的一些项目一样，我将按长度对数据进行排序。这导致一批句子的长度相似，因此使用较少的填充，并且模型将训练得更快。***

```
***training_sorted = []
testing_sorted = []for i in range(min_length, max_length+1):
    for sentence in training:
        if len(sentence) == i:
            training_sorted.append(sentence)
    for sentence in testing:
        if len(sentence) == i:
            testing_sorted.append(sentence)***
```

***也许这个项目最有趣/最重要的部分是将句子转换成有错误的句子的功能，这些句子将被用作输入数据。在这个函数中，错误以三种方式之一产生:***

*   ***两个字符的顺序将被交换(hlelo ~hello)***
*   ***会多加一个字母(heljlo ~ hello)***
*   ***一个字符不会打(helo ~hello)***

***三个错误中任何一个发生的可能性都是相等的，任何一个错误发生的可能性都是 5%。因此，平均每 20 个字符中就有一个包含错误。***

```
***letters = ['a','b','c','d','e','f','g','h','i','j','k','l','m',
           'n','o','p','q','r','s','t','u','v','w','x','y','z',]def noise_maker(sentence, threshold):

    noisy_sentence = []
    i = 0
    while i < len(sentence):
        random = np.random.uniform(0,1,1)
        if random < threshold:
            noisy_sentence.append(sentence[i])
        else:
            new_random = np.random.uniform(0,1,1)
            if new_random > 0.67:
                if i == (len(sentence) - 1):
                    continue
                else:
                    noisy_sentence.append(sentence[i+1])
                    noisy_sentence.append(sentence[i])
                    i += 1
            elif new_random < 0.33:
                random_letter = np.random.choice(letters, 1)[0]
                noisy_sentence.append(vocab_to_int[random_letter])
                noisy_sentence.append(sentence[i])
            else:
                pass     
        i += 1
    return noisy_sentence***
```

***本文中我想向您展示的最后一件事是如何创建批处理。通常，人们会在训练他们的模型之前创建他们的输入数据，这意味着他们有固定数量的训练数据。然而，我们将在训练模型时创建新的输入数据，将`noise_maker`应用于每一批数据。这意味着对于每个时期，目标(正确的)句子将通过`noise_maker`被反馈，并且应该接收新的输入句子。使用这种方法，从某种稍微夸张的意义上来说，我们拥有无限量的训练数据。***

```
***def get_batches(sentences, batch_size, threshold):

    for batch_i in range(0, len(sentences)//batch_size):
        start_i = batch_i * batch_size
        sentences_batch = sentences[start_i:start_i + batch_size]

        sentences_batch_noisy = []
        for sentence in sentences_batch:
            sentences_batch_noisy.append(
                noise_maker(sentence, threshold))

        sentences_batch_eos = []
        for sentence in sentences_batch:
            sentence.append(vocab_to_int['<EOS>'])
            sentences_batch_eos.append(sentence)

        pad_sentences_batch = np.array(
            pad_sentence_batch(sentences_batch_eos))
        pad_sentences_noisy_batch = np.array(
            pad_sentence_batch(sentences_batch_noisy))

        pad_sentences_lengths = []
        for sentence in pad_sentences_batch:
            pad_sentences_lengths.append(len(sentence))

        pad_sentences_noisy_lengths = []
        for sentence in pad_sentences_noisy_batch:
            pad_sentences_noisy_lengths.append(len(sentence))

        yield (pad_sentences_noisy_batch, 
               pad_sentences_batch, 
               pad_sentences_noisy_lengths, 
               pad_sentences_lengths)***
```

***这个项目到此为止！尽管结果令人鼓舞，但这一模式仍有局限性。如果有人能按比例放大这个模型或改进它的设计，我将不胜感激！如果你有，请在评论中发表。新设计的一个想法是应用[博览会的新 CNN 模式](https://code.facebook.com/posts/1978007565818999/a-novel-approach-to-neural-machine-translation/?utm_campaign=Artificial%2BIntelligence%2Band%2BDeep%2BLearning%2BWeekly&utm_medium=email&utm_source=Artificial_Intelligence_and_Deep_Learning_Weekly_13)(它实现了最先进的翻译效果)。***

***感谢阅读，我希望你学到了新的东西！***