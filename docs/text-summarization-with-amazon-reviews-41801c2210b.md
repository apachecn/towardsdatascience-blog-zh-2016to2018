# 带有亚马逊评论的文本摘要

> 原文：<https://towardsdatascience.com/text-summarization-with-amazon-reviews-41801c2210b?source=collection_archive---------0----------------------->

![](img/816c31be666ee279af402251f056c6a1.png)

With many products comes many reviews for training.

在本文中，我们将使用亚马逊的美食评论来构建一个可以总结文本的模型。具体来说，我们将使用评论的描述作为我们的输入数据，使用评论的标题作为我们的目标数据。要下载数据集并了解更多信息，您可以在 [Kaggle](https://www.kaggle.com/snap/amazon-fine-food-reviews) 上找到它。如果你决定建立一个像我这样的模型，你会发现它能够生成一些非常好的摘要:

*描述(1):咖啡味道好极了，价格也这么便宜！我强烈推荐这个给大家！*

*总结(1):大咖*

*描述(2):这是我买过的最难吃的奶酪！我再也不会买它了，希望你也不要买！*

*汇总(二):omg 毛总额*

代码是用 Python 写的，TensorFlow 1.1 将是我们的深度学习库。如果您还没有使用 TensorFlow 1.1，您将会看到构建 seq2seq 模型与以前的版本有很大的不同。为了帮助生成一些很棒的摘要，我们将在编码层使用双向 RNN，在解码层使用注意力。我们将建立的模型类似于潘新和刘泳的模型，来自“用于文本摘要的具有注意力的序列到序列模型”( [GitHub](https://github.com/tensorflow/models/tree/master/textsum) )。此外，相当多的信用应该给予 Jaemin Cho 教程( [GitHub](https://github.com/j-min/tf_tutorial_plus/tree/master/RNN_seq2seq/contrib_seq2seq) )。这是我使用 TensorFlow 1.1 的第一个项目，他的教程确实帮助我将代码整理得井井有条。

*注意:和我的其他文章一样，我将只展示我作品的主要部分，并且我将删除大部分的评论以保持简短。你可以在我的*[*Github*](https://github.com/Currie32/Text-Summarization-with-Amazon-Reviews)*上看到整个项目。*

# 准备数据

首先，我将解释我是如何清理文本的:

*   转换成小写。
*   用更长的形式代替收缩。
*   删除任何不需要的字符(这一步需要在替换缩写后完成，因为省略号将被删除。注意连字符前的反斜杠。如果没有这个斜杠，所有在连字符前后的字符之间的字符都将被删除。这可能会产生一些不必要的效果。举个例子，输入“a-d”会去掉 a，b，c，d)。
*   停用词只会从描述中删除。它们与训练模型不太相关，因此通过移除它们，我们能够更快地训练模型，因为数据更少。它们将保留在摘要中，因为它们相当短，我希望它们听起来更像自然短语。

```
def clean_text(text, remove_stopwords = True):

    # Convert words to lower case
    text = text.lower()

    # Replace contractions with their longer forms 
    if True:
        text = text.split()
        new_text = []
        for word in text:
            if word in contractions:
                new_text.append(contractions[word])
            else:
                new_text.append(word)
        text = " ".join(new_text)

    # Format words and remove unwanted characters
    text = re.sub(r'https?:\/\/.*[\r\n]*', '', text,  
                  flags=re.MULTILINE)
    text = re.sub(r'\<a href', ' ', text)
    text = re.sub(r'&amp;', '', text) 
    text = re.sub(r'[_"\-;%()|+&=*%.,!?:#$@\[\]/]', ' ', text)
    text = re.sub(r'<br />', ' ', text)
    text = re.sub(r'\'', ' ', text)

    # Optionally, remove stop words
    if remove_stopwords:
        text = text.split()
        stops = set(stopwords.words("english"))
        text = [w for w in text if not w in stops]
        text = " ".join(text)return text
```

我们将使用预先训练的词向量来帮助提高我们的模型的性能。过去，我曾为此使用过 GloVe，但我发现了另一组单词嵌入，名为[concept net number batch](https://github.com/commonsense/conceptnet-numberbatch)(CN)。基于其创造者的工作，它似乎优于 GloVe，这是有道理的，因为 CN 是包括 GloVe 在内的嵌入的集合。

```
embeddings_index = {}
with open('/Users/Dave/Desktop/Programming/numberbatch-en-17.02.txt', encoding='utf-8') as f:
    for line in f:
        values = line.split(' ')
        word = values[0]
        embedding = np.asarray(values[1:], dtype='float32')
        embeddings_index[word] = embedding
```

我们将把我们的词汇限制在 CN 中或者在我们的数据集中出现超过 20 次的单词。这将允许我们对每个单词都有非常好的嵌入，因为模型可以更好地理解单词在它们出现更多次时是如何相关的。

在构建您的`word_embedding_matrix`时，将它的“dtype”NP . zeros 设置为 float32 是非常重要的。默认值是 float64，但这不适用于 TensorFlow，因为它希望值限制为 _32。

```
embedding_dim = 300
nb_words = len(vocab_to_int)word_embedding_matrix = np.zeros((nb_words, embedding_dim), 
                                 dtype=np.float32)
for word, i in vocab_to_int.items():
    if word in embeddings_index:
        word_embedding_matrix[i] = embeddings_index[word]
    else:
        # If word not in CN, create a random embedding for it
        new_embedding = np.array(np.random.uniform(-1.0, 1.0, embedding_dim))
        embeddings_index[word] = new_embedding
        word_embedding_matrix[i] = new_embedding
```

为了帮助更快地训练模型，我们将根据描述的长度从最短到最长对评论进行排序。这将有助于每一批具有相似长度的描述，这将导致更少的填充，从而更少的计算。我们可以根据摘要的长度进行二次排序，但是这将导致大量的循环来组织数据。另外，我怀疑它会减少多少额外的填充，因为摘要相当短。

由于描述或摘要中的 UNK 令牌的数量，一些评论将不包括在内。如果描述中有超过 1 个 UNK 或摘要中有任何 UNKs，则不会使用该评论。这样做是为了确保我们用有意义的数据构建模型。只有不到 0.7%的词是 UNKs，所以不会有很多评论被删除。

```
for length in range(min(lengths_texts.counts), max_text_length): 
   for count, words in enumerate(int_summaries):
      if (len(int_summaries[count]) >= min_length and
          len(int_summaries[count]) <= max_summary_length and
          len(int_texts[count]) >= min_length and
          unk_counter(int_summaries[count]) <= unk_summary_limit and
          unk_counter(int_texts[count]) <= unk_text_limit and
          length == len(int_texts[count])
         ):
          sorted_summaries.append(int_summaries[count])
          sorted_texts.append(int_texts[count])
```

# 构建模型

我们需要为这个模型做一些占位符。大多数都是不言自明的，但是有几个需要明确的是，`summary_length`和`text_length`是一个批处理中每个句子的长度，`max_summary_length`是一个批处理中摘要的最大长度。

```
def model_inputs():

    input_data = tf.placeholder(tf.int32,[None, None], name='input')
    targets = tf.placeholder(tf.int32, [None, None], name='targets')
    lr = tf.placeholder(tf.float32, name='learning_rate')
    keep_prob = tf.placeholder(tf.float32, name='keep_prob')
    summary_length = tf.placeholder(tf.int32, (None,), 
                                    name='summary_length')
    max_summary_length = tf.reduce_max(summary_length, 
                                       name='max_dec_len')
    text_length = tf.placeholder(tf.int32, (None,),       
                                 name='text_length')return input_data, targets, lr, keep_prob, summary_length, 
       max_summary_length, text_length
```

为了构建我们的编码层，我们将使用 LSTMs 的双向 RNN。如果您使用过 TensorFlow 的早期版本，您会注意到这种布局与通常创建两个层的方式不同。通常，只需将两个 LSTMs 包装在 tf.contrib.rnn.MultiRNNCell 中，但这种方法不再可行。这是我发现的一个方法，尽管代码更多，但也同样有效。

一些需要注意的事项:

*   你需要使用`tf.variable_scope`，这样你的变量可以在每一层重用。如果你真的不知道我在说什么，那就去看看 TensorFlow 的 [word2vec 教程](https://www.tensorflow.org/tutorials/word2vec)。
*   因为我们使用的是双向 RNN，所以需要将输出连接起来
*   初始化器与潘和刘模型中的初始化器相同。

```
def encoding_layer(rnn_size, sequence_length, num_layers, 
                   rnn_inputs, keep_prob):

    for layer in range(num_layers):
        with tf.variable_scope('encoder_{}'.format(layer)):
            cell_fw = tf.contrib.rnn.LSTMCell(rnn_size,                                             
                  initializer=tf.random_uniform_initializer(-0.1, 
                                                            0.1,       
                                                            seed=2))
            cell_fw = tf.contrib.rnn.DropoutWrapper(cell_fw, 
                                      input_keep_prob = keep_prob) cell_bw = tf.contrib.rnn.LSTMCell(rnn_size,    
                  initializer=tf.random_uniform_initializer(-0.1,      
                                                            0.1, 
                                                            seed=2))
            cell_bw = tf.contrib.rnn.DropoutWrapper(cell_bw, 
                                      input_keep_prob = keep_prob) enc_output, enc_state = tf.nn.bidirectional_dynamic_rnn( 
                                       cell_fw, 
                                       cell_bw,                     
                                       rnn_inputs,
                                       sequence_length,
                                       dtype=tf.float32)

    enc_output = tf.concat(enc_output,2)

    return enc_output, enc_state
```

为了创建我们的训练和推理解码层，TF 1.1 中有一些新的函数..为你分解事情:

*   `TrainingHelper`从编码层读取整数序列。
*   `BasicDecoder`用解码单元和输出层处理序列，输出层是全连接层。`initial_state`来自我们的`DynamicAttentionWrapperState`，你很快就会看到。
*   `dynamic_decode`创建将用于培训的输出。

```
def training_decoding_layer(dec_embed_input, summary_length, 
                            dec_cell, initial_state, output_layer, 
                            vocab_size, max_summary_length):

    training_helper = tf.contrib.seq2seq.TrainingHelper( 
                         inputs=dec_embed_input,
                         sequence_length=summary_length,
                         time_major=False) training_decoder = tf.contrib.seq2seq.BasicDecoder(
                          dec_cell,
                          training_helper,
                          initial_state,
                          output_layer) training_logits, _ = tf.contrib.seq2seq.dynamic_decode(
                            training_decoder,
                            output_time_major=False,
                            impute_finished=True,
                            maximum_iterations=max_summary_length)
    return training_logits
```

如你所见，这与训练层非常相似。主要区别是`GreedyEmbeddingHelper`，它使用输出的 argmax(被视为 logits)并通过一个嵌入层传递结果以获得下一个输入。虽然是要求`start_tokens`，但是我们只有一个，`<GO>`。

```
def inference_decoding_layer(embeddings, start_token, end_token,        
                             dec_cell, initial_state, output_layer,
                             max_summary_length, batch_size):

    start_tokens = tf.tile(tf.constant([start_token],  
                                       dtype=tf.int32),  
                                       [batch_size], 
                                       name='start_tokens')

    inference_helper = tf.contrib.seq2seq.GreedyEmbeddingHelper(  
                           embeddings,
                           start_tokens,
                           end_token)

    inference_decoder = tf.contrib.seq2seq.BasicDecoder(
                            dec_cell,
                            inference_helper,
                            initial_state,
                            output_layer)

    inference_logits, _ = tf.contrib.seq2seq.dynamic_decode(
                              inference_decoder,
                              output_time_major=False,
                              impute_finished=True,
                              maximum_iterations=max_summary_length)

    return inference_logits
```

虽然解码层可能看起来有点复杂，但它可以分为三个部分:解码细胞、注意力和获取我们的逻辑。

解码单元格:

*   只是一个辍学的双层 LSTM。

注意:

*   我用 Bhadanau 代替 Luong 作为我的注意力风格。使用它有助于模型更快地训练，并可以产生更好的结果(这里有一篇[好论文](https://arxiv.org/abs/1703.03906v2)，它比较了这两者以及 seq2seq 模型的许多方面)。
*   `DynamicAttentionWrapper`将注意力机制应用于我们的解码单元。
*   `DynamicAttentionWrapperState`创建我们用于训练和推理层的初始状态。由于我们在解码层使用双向 RNN，我们只能使用前向或后向状态。我选择向前，因为这是潘和刘选择的状态。

```
def decoding_layer(dec_embed_input, embeddings, enc_output,  
                   enc_state, vocab_size, text_length, 
                   summary_length, max_summary_length, rnn_size, 
                   vocab_to_int, keep_prob, batch_size, num_layers):

    for layer in range(num_layers):
        with tf.variable_scope('decoder_{}'.format(layer)):
            lstm = tf.contrib.rnn.LSTMCell(rnn_size,
                  initializer=tf.random_uniform_initializer(-0.1, 
                                                            0.1,     
                                                            seed=2))
            dec_cell = tf.contrib.rnn.DropoutWrapper(
                           lstm, 
                           input_keep_prob = keep_prob)

    output_layer = Dense(vocab_size,
           kernel_initializer = tf.truncated_normal_initializer(  
                                    mean=0.0, 
                                    stddev=0.1))

    attn_mech = tf.contrib.seq2seq.BahdanauAttention(
                      rnn_size,
                      enc_output,
                      text_length,
                      normalize=False,
                      name='BahdanauAttention') dec_cell = tf.contrib.seq2seq.DynamicAttentionWrapper(dec_cell,
                                                          attn_mech,
                                                          rnn_size)

    initial_state = tf.contrib.seq2seq.DynamicAttentionWrapperState(
                        enc_state[0],
                        _zero_state_tensors(rnn_size, 
                                            batch_size, 
                                            tf.float32)) 

    with tf.variable_scope("decode"):
        training_logits = training_decoding_layer(
                              dec_embed_input, 
                              summary_length, 
                              dec_cell, 
                              initial_state,
                              output_layer,
                              vocab_size, 
                              max_summary_length)

    with tf.variable_scope("decode", reuse=True):
        inference_logits = inference_decoding_layer(
                              embeddings,  
                              vocab_to_int['<GO>'], 
                              vocab_to_int['<EOS>'],
                              dec_cell, 
                              initial_state, 
                              output_layer,
                              max_summary_length,
                              batch_size) return training_logits, inference_logits
```

我们将使用我们之前创建的`word_embedding_matrix`作为我们的嵌入。编码和解码序列都将使用这些嵌入。

这个函数的其余部分主要是收集前面函数的输出，以便它们可以用于训练模型和生成新的摘要。

```
def seq2seq_model(input_data, target_data, keep_prob, text_length,  
                  summary_length, max_summary_length, 
                  vocab_size, rnn_size, num_layers, vocab_to_int,  
                  batch_size):

    embeddings = word_embedding_matrix

    enc_embed_input = tf.nn.embedding_lookup(embeddings, input_data)
    enc_output, enc_state = encoding_layer(rnn_size, 
                                           text_length, 
                                           num_layers,  
                                           enc_embed_input,  
                                           keep_prob)

    dec_input = process_encoding_input(target_data, 
                                       vocab_to_int, 
                                       batch_size)
    dec_embed_input = tf.nn.embedding_lookup(embeddings, dec_input)

    training_logits, inference_logits  = decoding_layer(
         dec_embed_input, 
         embeddings,
         enc_output,
         enc_state, 
         vocab_size, 
         text_length, 
         summary_length, 
         max_summary_length,
         rnn_size, 
         vocab_to_int, 
         keep_prob, 
         batch_size,
         num_layers)

    return training_logits, inference_logits
```

创建批处理是非常典型的。这里我唯一想指出的是创建了`pad_summaries_lengths`和`pad_texts_lengths`。这些包含一批内的摘要/文本的长度，并将用作输入值:`summary_length`和`text_length`。我知道寻找这些输入值似乎是一种奇怪的方法，但这是我能找到的最好的/唯一的解决方案。

```
def get_batches(summaries, texts, batch_size): for batch_i in range(0, len(texts)//batch_size):
        start_i = batch_i * batch_size
        summaries_batch = summaries[start_i:start_i + batch_size]
        texts_batch = texts[start_i:start_i + batch_size]
        pad_summaries_batch = np.array(pad_sentence_batch(  
                                  summaries_batch))
        pad_texts_batch = np.array(pad_sentence_batch(texts_batch))

        pad_summaries_lengths = []
        for summary in pad_summaries_batch:
            pad_summaries_lengths.append(len(summary))

        pad_texts_lengths = []
        for text in pad_texts_batch:
            pad_texts_lengths.append(len(text))

        yield (pad_summaries_batch, 
               pad_texts_batch, 
               pad_summaries_lengths, 
               pad_texts_lengths)
```

这些是我用来训练这个模型的超参数。我不认为它们有什么太令人兴奋的，它们很标准。《100 个纪元》可能已经引起了你的注意。我使用这个较大的值，以便我的模型得到充分的训练，并且只在早期停止(当损失停止减少时)时停止训练。

```
epochs = 100
batch_size = 64
rnn_size = 256
num_layers = 2
learning_rate = 0.01
keep_probability = 0.75
```

我将跳过构建图表和如何训练模型。有一些很好的东西，比如我如何合并学习率衰减和早期停止，但我认为不值得在这里占用空间(如果你感兴趣，请查看我的 [GitHub](https://github.com/Currie32/Text-Summarization-with-Amazon-Reviews) )。

# 生成您自己的摘要

您可以创建自己的描述，也可以使用数据集中的描述作为输入数据。使用你自己的描述会更有趣一些，因为你可以很有创意地去看看模型创建了什么样的摘要。下面的函数将使用我之前描述的`clean_text`函数为您的模型准备描述。

```
def text_to_seq(text): text = clean_text(text)
    return [vocab_to_int.get(word, vocab_to_int['<UNK>']) for word in text.split()]
```

我们需要加载相当多的张量来生成新的摘要。尽管如此，这并不太难。运行会话时，`input_data`和`text_length`需要乘以`batch_size`以匹配模型的输入参数。您可以将`summary_length`设置为您喜欢的任何值，但是我决定将其设置为随机的，以保持事情的精彩。要记住的主要事情是将其保持在模型训练的摘要长度范围内，即 2–13。

```
input_sentence = "*This is the worst cheese that I have ever bought! I will never buy it again and I hope you won’t either!*"
text = text_to_seq(input_sentence)#random = np.random.randint(0,len(clean_texts))
#input_sentence = clean_texts[random]
#text = text_to_seq(clean_texts[random])checkpoint = "./best_model.ckpt"loaded_graph = tf.Graph()
with tf.Session(graph=loaded_graph) as sess:

    loader = tf.train.import_meta_graph(checkpoint + '.meta')
    loader.restore(sess, checkpoint) input_data = loaded_graph.get_tensor_by_name('input:0')
    logits = loaded_graph.get_tensor_by_name('predictions:0')
    text_length = loaded_graph.get_tensor_by_name('text_length:0')
    summary_length =  
         loaded_graph.get_tensor_by_name('summary_length:0')
    keep_prob = loaded_graph.get_tensor_by_name('keep_prob:0')

    answer_logits = sess.run(logits, {
         input_data: [text]*batch_size, 
         summary_length: [np.random.randint(4,8)], 
         text_length: [len(text)]*batch_size,
         keep_prob: 1.0})[0]# Remove the padding from the tweet
pad = vocab_to_int["<PAD>"]print('\nOriginal Text:', input_sentence)print('Text')
print('Word Ids:    {}'.format([i for i in text if i != pad]))
print('Input Words: {}'.format([int_to_vocab[i] for 
                                 i in text if i != pad]))print('\nSummary')
print('Word Ids: {}'.format([i for i in answer_logits if i != pad]))
print('Response Words: {}'.format([int_to_vocab[i] for i in 
                                  answer_logits if i != pad]))
```

最后要注意的一点是，我只用 50，000 条评论的子集来训练我的模型。由于我使用的是 MacBook Pro，如果我使用所有的数据，我可能需要几天时间来训练这个模型。只要有可能，我就会使用 FloydHub 上的 GPU 来训练我的模型。如果您有时间，您可以将数据和 ConceptNet Numberbatch 上传到 FloydHub，然后用所有评论训练模型。让我知道怎么走！

这个项目到此为止！我希望你觉得它很有趣，并且学到了一些东西。尽管这个模型执行得相当好，并且只有一个数据子集，但是尝试扩展这个体系结构来提高生成的摘要的质量是很好的。[这篇论文](https://www.aclweb.org/anthology/C/C16/C16-1229.pdf)同时使用了 RNNs 和 CNN 来预测情绪，也许在这里也可以。如果有人尝试这样做，如果你能在下面的评论区贴一个链接就太好了，因为我会很高兴看到它。

感谢您的阅读，如果您有任何问题或意见，请告诉我！干杯！