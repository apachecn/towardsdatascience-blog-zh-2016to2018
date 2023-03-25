# 从短文本中提取关键词

> 原文：<https://towardsdatascience.com/extracting-keywords-from-short-text-fce39157166b?source=collection_archive---------0----------------------->

![](img/60d1a8bc40d18c2688e375ccfc0a2e50.png)

> ***专业提示:更多关于人工智能和机器学习的文章、故事、代码和研究，请访问我的网站:***[***【www.theapemachine.com】***](http://www.theapemachine.com)

多年来，我多次遇到提取关键词的问题，尤其是从单句、标题或问题等短文本中提取关键词。

在 NLP 中处理[短文本一直是一个众所周知的问题，而识别关键词](http://www.wangzhongyuan.com/tutorial/ACL2016/Understanding-Short-Texts/)[尤其是](https://www.google.fr/search?q=particularly&spell=1&sa=X&ved=0ahUKEwjBkKeJlqLVAhXCWBoKHRCCDTYQBQglKAA)很难做到。
这些年来，我尝试了许多方法，从我自己基于一个非常流行的文字游戏非常天真地实现了一个简单的评分算法，到快速自动关键词提取(RAKE ),如果没有大型文档要处理，没有一种方法表现得非常好。

最终，随着我对现代常见的机器学习模型越来越熟悉，我开始将关键词提取视为“简单！翻译问题，在这篇文章中，我们将回顾导致一个伟大的关键字提取器，我们现在可以在生产中使用的旅程。

我第一次需要找到一个短句中最重要的关键词时，我正在为一家心理学家公司开发一些软件，他们正在开发一个平台，其中有一个模块需要从语言挑战数据库中提取相关的句子。

这个想法是要将要检索的句子与前一个句子的关键词进行匹配，由于我当时对机器学习知之甚少，所以我用自己的天真解决方案来计算句子中每个单词的拼字得分，并从这些结果中取出得分最高的第 n 个单词，并假设它们是关键词。

如果不是因为一些在拼字游戏中得分很高的停用词，这实际上非常有效。
当然，你可以在这个方法中部署一个非索引列表，事情就会迎刃而解，这甚至是相当可扩展的，因为任何语言都只包含你想要过滤掉的单词，你只需要定义这个列表一次。

# 耙子

下一个实现是快速自动关键词提取，它实际上只比前一个方法好一点点，并且仍然使用非索引列表。

如今，RAKE 已经作为即插即用模块在许多编程语言中实现，您应该不用花太多时间来试验它，但是我警告您，不要让它在短文本上欺骗您。大多数时候，它实际上并没有提取任何关键词，只是删除了停用词。因为文本很短，所以没有那么多其他的词可以“搜索”。

# 发明一种新的语言！

最终，我明白了一些事情:如果像序列到序列神经网络这样的现代翻译方法可以翻译语言，或者给出从序列中学习到的响应，为什么我们不能将关键词视为其自身的“语言”。
一种没有语法的语言，可以翻译成(甚至可能翻译成？).

这似乎是一个很好的想法:简单地训练一个序列，对标有关键词的短句数据集进行神经网络排序，并让网络将新句子“翻译”成我们的关键词“语言”。

所以让我们开始吧！

# 1.生成数据集

现在我有了新方法，我面临的一个挑战是找到一个数据集进行训练，我相信你会认识到这是一个困难。可能会让你吃惊的是，真的没有任何现成的数据集有短句与文本中实际存在的关键词相匹配。当然，有许多关于关键词提取的数据集，但它们都有较大的文本，这些文本已经被手动标注了关键词。

幸运的是，我身边的一个人给了我一个绝妙的主意:使用博客圈真正的大玩家的博客标题，并使用这些页面上的关键字元标签来找到与标题相关的关键字。事实上，这些博客非常关注 SEO，因此我们可以假设这些数据是经过精心策划的。

剩下唯一要做的事情是确保删除所有实际上不在标题中的关键词，尽管你可能想考虑甚至保留这些关键词，因为一旦你的神经网络训练完毕，你不仅能够从一个句子中提取准确的关键词，甚至能够提取相关的关键词。
不过，我会把这留给未来的实验。

下面的 Ruby 脚本正是我用来在 Lifehacker 和 TechCrunch 上搜索关键词的脚本。

```
require 'mechanize'

**class** **Scrape**

  **def** initialize
    @mechanize = Mechanize.new
    @visited   = []
    @v_titles  = []

    @csv = File.open('/home/theapemachine/data/keywords-web2.csv', 'a')
  **end**

  **def** run(urls)
    scraped_urls = []

    urls.each **do** |url|
      **if** !@visited.include?(url)
        **begin**
          rawpage = @mechanize.get(url)

          rawpage.links.uniq.each **do** |a|
            scraped_urls << a.href
          **end**

          page_title = ''

          rawpage.search('title').each **do** |title|
            page_title = title.text.downcase.strip
          **end**

          keywords = rawpage.at('head meta[name="keywords"]')['content'].split(', ').delete_if{|k| page_title.scan(k.downcase).empty?}

          **if** !page_title.empty? && !keywords.empty? && !@v_titles.include?(page_title)
            puts
            puts "TITLE:    #{page_title}"
            puts "KEYWORDS: #{keywords.join(',')}"

            @csv << %("#{page_title}","#{keywords.join(',').downcase}"\n)
          **end**

          @v_titles << page_title
        **rescue**
        **end**
      **end**
    **end**

    @visited += urls
    run(scraped_urls.uniq)
  **end**

**end**

@scrape = Scrape.new
@scrape.run([
  'http://lifehacker.com/',
  'http://www.techcrunch.com'
])
```

# 2.准备数据

我们现在需要准备收集的数据，这样我们就可以在 Pytorch 序列中使用它来排序我接下来将列出的模型。我使用了下面非常简单的 Ruby 脚本来确保训练数据中的每一行首先有文章标题，然后是制表位，然后是关键字。

```
require 'csv'

train = File.open('./keywords.train', 'w')

csv_text = File.read('/home/theapemachine/data/keywords-web.csv')
csv      = CSV.parse(csv_text, headers: **false**)

csv.each **do** |row|
  **begin**
    train.puts row[0] + "\t" + row[1]
  **rescue**
  **end**
**end**
```

# 3.序列对序列模型

我们现在可以使用 Pytorch 编写我们的序列到序列神经网络，事实上，简单地使用他们的教程部分列出的代码就可以很好地完成这个任务。我在培训周期结束时添加了一个简单的提示，这样我就可以轻松地测试网络。
在以后的版本中，你会想要保存你的模型以备后用，因为在我的笔记本电脑上训练花了很长时间。

由肖恩·罗伯逊(Sean Robertson)的原 Pytorch [源代码](http://pytorch.org/tutorials/intermediate/seq2seq_translation_tutorial.html)翻译而来。

```
**import** unicodedata
**import** string
**import** re
**import** random
**import** time
**import** math

**import** torch
**import** torch.nn **as** nn
**from** torch.autograd **import** Variable
**from** torch **import** optim
**import** torch.nn.functional **as** F

USE_CUDA  = True
SOS_token = 0
EOS_token = 1

**class** **Lang**:
    **def** __init__(self, name):
        self.name       = name
        self.word2index = {}
        self.word2count = {}
        self.index2word = {0: "SOS", 1: "EOS"}
        self.n_words    = 2

    **def** index_words(self, sentence):
        **for** word in sentence.split(' '):
            self.index_word(word)

    **def** index_word(self, word):
        **if** word not in self.word2index:
            self.word2index[word]          = self.n_words
            self.word2count[word]          = 1
            self.index2word[self.n_words]  = word
            self.n_words                  += 1
        **else**:
            self.word2count[word] += 1

**def** unicode_to_ascii(s):
    **return** ''.join(
        c **for** c in unicodedata.normalize('NFD', u'' + s)
        **if** unicodedata.category(c) != 'Mn'
    )

**def** normalize_string(s):
    s = unicode_to_ascii(s.lower().strip())
    s = re.sub(r"([.!?])", r" \1", s)
    s = re.sub(r"[^a-zA-Z.!?]+", r" ", s)

    **return** s

**def** read_langs(lang1, lang2, reverse=False):
    **print**("Reading lines...")

    lines = open('keywords.train').read().strip().split('\n')
    pairs = [[normalize_string(s) **for** s in l.split('\t')] **for** l in lines]

    **if** reverse:
        pairs       = [list(reversed(p)) **for** p in pairs]
        input_lang  = Lang(lang2)
        output_lang = Lang(lang1)
    **else**:
        input_lang  = Lang(lang1)
        output_lang = Lang(lang2)

    **return** input_lang, output_lang, pairs

MAX_LENGTH = 100

good_prefixes = (
    "i am ", "i m ",
    "he is", "he s ",
    "she is", "she s",
    "you are", "you re "
)

**def** filter_pair(p):
    **return** len(p[0].split(' ')) < MAX_LENGTH and len(p[1].split(' ')) < MAX_LENGTH

**def** filter_pairs(pairs):
    **return** [pair **for** pair in pairs **if** filter_pair(pair)]

**def** prepare_data(lang1_name, lang2_name, reverse=False):
    input_lang, output_lang, pairs = read_langs(lang1_name, lang2_name, reverse)
    **print**("Read %s sentence pairs" % len(pairs))

    pairs = filter_pairs(pairs)
    **print**("Trimmed to %s sentence pairs" % len(pairs))

    **print**("Indexing words...")
    **for** pair in pairs:
        input_lang.index_words(pair[0])
        output_lang.index_words(pair[1])

    **return** input_lang, output_lang, pairs

input_lang, output_lang, pairs = prepare_data('eng', 'fra', False)
**print**(random.choice(pairs))

**def** indexes_from_sentence(lang, sentence):
    **return** [lang.word2index[word] **for** word in sentence.split(' ')]

**def** variable_from_sentence(lang, sentence):
    indexes = indexes_from_sentence(lang, sentence)
    indexes.append(EOS_token)

    var = Variable(torch.LongTensor(indexes).view(-1, 1))

    **if** USE_CUDA:
        var = var.cuda()

    **return** var

**def** variables_from_pair(pair):
    input_variable  = variable_from_sentence(input_lang, pair[0])
    target_variable = variable_from_sentence(output_lang, pair[1])

    **return** (input_variable, target_variable)

**class** **EncoderRNN**(nn.Module):
    **def** __init__(self, input_size, hidden_size, n_layers=1):
        super(EncoderRNN, self).__init__()

        self.input_size  = input_size
        self.hidden_size = hidden_size
        self.n_layers    = n_layers
        self.embedding   = nn.Embedding(input_size, hidden_size)
        self.gru         = nn.GRU(hidden_size, hidden_size, n_layers)

    **def** forward(self, word_inputs, hidden):
        seq_len        = len(word_inputs)
        embedded       = self.embedding(word_inputs).view(seq_len, 1, -1)
        output, hidden = self.gru(embedded, hidden)

        **return** output, hidden

    **def** init_hidden(self):
        hidden = Variable(torch.zeros(self.n_layers, 1, self.hidden_size))

        **if** USE_CUDA:
            hidden = hidden.cuda()

        **return** hidden

**class** **BahdanauAttnDecoderRNN**(nn.Module):
    **def** __init__(self, hidden_size, output_size, n_layers=1, dropout_p=0.1):
        super(AttnDecoderRNN, self).__init__()

        self.hidden_size = hidden_size
        self.output_size = output_size
        self.n_layers    = n_layers
        self.dropout_p   = dropout_p
        self.max_length  = max_length
        self.embedding   = nn.Embedding(output_size, hidden_size)
        self.dropout     = nn.Dropout(dropout_p)
        self.attn        = GeneralAttn(hidden_size)
        self.gru         = nn.GRU(hidden_size * 2, hidden_size, n_layers, dropout=dropout_p)
        self.out         = nn.Linear(hidden_size, output_size)

    **def** forward(self, word_input, last_hidden, encoder_outputs):
        word_embedded  = self.embedding(word_input).view(1, 1, -1)
        word_embedded  = self.dropout(word_embedded)
        attn_weights   = self.attn(last_hidden[-1], encoder_outputs)
        context        = attn_weights.bmm(encoder_outputs.transpose(0, 1))
        rnn_input      = torch.cat((word_embedded, context), 2)
        output, hidden = self.gru(rnn_input, last_hidden)
        output         = output.squeeze(0)
        output         = F.log_softmax(self.out(torch.cat((output, context), 1)))

        **return** output, hidden, attn_weights

**class** **Attn**(nn.Module):
    **def** __init__(self, method, hidden_size, max_length=MAX_LENGTH):
        super(Attn, self).__init__()

        self.method      = method
        self.hidden_size = hidden_size

        **if** self.method == 'general':
            self.attn = nn.Linear(self.hidden_size, hidden_size)

        **elif** self.method == 'concat':
            self.attn  = nn.Linear(self.hidden_size * 2, hidden_size)
            self.other = nn.Parameter(torch.FloatTensor(1, hidden_size))

    **def** forward(self, hidden, encoder_outputs):
        seq_len       = len(encoder_outputs)
        attn_energies = Variable(torch.zeros(seq_len))

        **if** USE_CUDA:
            attn_energies = attn_energies.cuda()

        **for** i in range(seq_len):
            attn_energies[i] = self.score(hidden, encoder_outputs[i])

        **return** F.softmax(attn_energies).unsqueeze(0).unsqueeze(0)

    **def** score(self, hidden, encoder_output):
        **if** self.method == 'dot':
            energy = hidden.dot(encoder_output)
            **return** energy

        **elif** self.method == 'general':
            energy = self.attn(encoder_output)
            energy = hidden.dot(energy)
            **return** energy

        **elif** self.method == 'concat':
            energy = self.attn(torch.cat((hidden, encoder_output), 1))
            energy = self.other.dot(energy)
            **return** energy

**class** **AttnDecoderRNN**(nn.Module):
    **def** __init__(self, attn_model, hidden_size, output_size, n_layers=1, dropout_p=0.1):
        super(AttnDecoderRNN, self).__init__()

        self.attn_model  = attn_model
        self.hidden_size = hidden_size
        self.output_size = output_size
        self.n_layers    = n_layers
        self.dropout_p   = dropout_p
        self.embedding   = nn.Embedding(output_size, hidden_size)
        self.gru         = nn.GRU(hidden_size * 2, hidden_size, n_layers, dropout=dropout_p)
        self.out         = nn.Linear(hidden_size * 2, output_size)

        **if** attn_model != 'none':
            self.attn = Attn(attn_model, hidden_size)

    **def** forward(self, word_input, last_context, last_hidden, encoder_outputs):
        word_embedded      = self.embedding(word_input).view(1, 1, -1)
        rnn_input          = torch.cat((word_embedded, last_context.unsqueeze(0)), 2)
        rnn_output, hidden = self.gru(rnn_input, last_hidden)
        attn_weights       = self.attn(rnn_output.squeeze(0), encoder_outputs)
        context            = attn_weights.bmm(encoder_outputs.transpose(0, 1))
        rnn_output         = rnn_output.squeeze(0)
        context            = context.squeeze(1)
        output             = F.log_softmax(self.out(torch.cat((rnn_output, context), 1)))

        **return** output, context, hidden, attn_weights

encoder_test = EncoderRNN(MAX_LENGTH, MAX_LENGTH, 2 )
decoder_test = AttnDecoderRNN('general', MAX_LENGTH, MAX_LENGTH, 2)

**print**(encoder_test)
**print**(decoder_test)

encoder_hidden = encoder_test.init_hidden()
word_input     = Variable(torch.LongTensor([1, 2, 3]))

**if** USE_CUDA:
    encoder_test.cuda()
    word_input = word_input.cuda()

encoder_outputs, encoder_hidden = encoder_test(word_input, encoder_hidden)

word_inputs     = Variable(torch.LongTensor([1, 2, 3]))
decoder_attns   = torch.zeros(1, 3, 3)
decoder_hidden  = encoder_hidden
decoder_context = Variable(torch.zeros(1, decoder_test.hidden_size))

**if** USE_CUDA:
    decoder_test.cuda()

    word_inputs     = word_inputs.cuda()
    decoder_context = decoder_context.cuda()

**for** i in range(3):
    decoder_output, decoder_context, decoder_hidden, decoder_attn = decoder_test(word_inputs[i], decoder_context, decoder_hidden, encoder_outputs)
    **print**(decoder_output.size(), decoder_hidden.size(), decoder_attn.size())
    decoder_attns[0, i] = decoder_attn.squeeze(0).cpu().data

teacher_forcing_ratio = 0.5
clip                  = 5.0

**def** train(input_variable, target_variable, encoder, decoder, encoder_optimizer, decoder_optimizer, criterion, max_length=MAX_LENGTH):
    encoder_optimizer.zero_grad()
    decoder_optimizer.zero_grad()
    loss = 0

    input_length                    = input_variable.size()[0]
    target_length                   = target_variable.size()[0]
    encoder_hidden                  = encoder.init_hidden()
    encoder_outputs, encoder_hidden = encoder(input_variable, encoder_hidden)
    decoder_input                   = Variable(torch.LongTensor([[SOS_token]]))
    decoder_context                 = Variable(torch.zeros(1, decoder.hidden_size))
    decoder_hidden                  = encoder_hidden

    **if** USE_CUDA:
        decoder_input   = decoder_input.cuda()
        decoder_context = decoder_context.cuda()

    use_teacher_forcing = random.random() < teacher_forcing_ratio

    **if** use_teacher_forcing:
        **for** di in range(target_length):
            decoder_output, decoder_context, decoder_hidden, decoder_attention = decoder(decoder_input, decoder_context, decoder_hidden, encoder_outputs)
            loss += criterion(decoder_output[0], target_variable[di])
            decoder_input = target_variable[di] # Next target is next input

    **else**:
        **for** di in range(target_length):
            decoder_output, decoder_context, decoder_hidden, decoder_attention = decoder(decoder_input, decoder_context, decoder_hidden, encoder_outputs)
            loss += criterion(decoder_output[0], target_variable[di])

            topv, topi = decoder_output.data.topk(1)
            ni         = topi[0][0]

            decoder_input = Variable(torch.LongTensor([[ni]]))

            **if** USE_CUDA:
                decoder_input = decoder_input.cuda()

            **if** ni == EOS_token:
                **break**

    loss.backward()

    torch.nn.utils.clip_grad_norm(encoder.parameters(), clip)
    torch.nn.utils.clip_grad_norm(decoder.parameters(), clip)

    encoder_optimizer.step()
    decoder_optimizer.step()

    **return** loss.data[0] / target_length

**def** as_minutes(s):
    m  = math.floor(s / 60)
    s -= m * 60

    **return** '%dm %ds' % (m, s)

**def** time_since(since, percent):
    now = time.time()
    s   = now - since
    es  = s / (percent)
    rs  = es - s

    **return** '%s (- %s)' % (as_minutes(s), as_minutes(rs))

attn_model  = 'general'
hidden_size = 500
n_layers    = 2
dropout_p   = 0.05

encoder = EncoderRNN(input_lang.n_words, hidden_size, n_layers)
decoder = AttnDecoderRNN(attn_model, hidden_size, output_lang.n_words, n_layers, dropout_p=dropout_p)

**if** USE_CUDA:
    encoder.cuda()
    decoder.cuda()

learning_rate     = 0.0001
encoder_optimizer = optim.Adam(encoder.parameters(), lr=learning_rate)
decoder_optimizer = optim.Adam(decoder.parameters(), lr=learning_rate)
criterion         = nn.NLLLoss()
n_epochs          = 50000
plot_every        = 200
print_every       = 1000
start             = time.time()
plot_losses       = []
print_loss_total  = 0
plot_loss_total   = 0

# Begin!
**for** epoch in range(1, n_epochs + 1):

    # Get training data for this cycle
    training_pair   = variables_from_pair(random.choice(pairs))
    input_variable  = training_pair[0]
    target_variable = training_pair[1]

    # Run the train function
    loss = train(input_variable, target_variable, encoder, decoder, encoder_optimizer, decoder_optimizer, criterion)

    # Keep track of loss
    print_loss_total += loss
    plot_loss_total  += loss

    **if** epoch == 0: **continue**

    **if** epoch % print_every == 0:
        print_loss_avg   = print_loss_total / print_every
        print_loss_total = 0
        print_summary    = '(%d %d%%) %.4f' % (epoch, epoch / n_epochs * 100, print_loss_avg)

        **print**(print_summary)

    **if** epoch % plot_every == 0:
        plot_loss_avg = plot_loss_total / plot_every
        plot_losses.append(plot_loss_avg)
        plot_loss_total = 0

**import** matplotlib.pyplot **as** plt
**import** matplotlib.ticker **as** ticker
**import** numpy **as** np

**def** show_plot(points):
    plt.figure()

    fig, ax = plt.subplots()
    loc     = ticker.MultipleLocator(base=0.2) # put ticks at regular intervals

    ax.yaxis.set_major_locator(loc)
    plt.plot(points)

show_plot(plot_losses)

**def** evaluate(sentence, max_length=MAX_LENGTH):
    input_variable = variable_from_sentence(input_lang, sentence)
    input_length = input_variable.size()[0]

    # Run through encoder
    encoder_hidden = encoder.init_hidden()
    encoder_outputs, encoder_hidden = encoder(input_variable, encoder_hidden)

    # Create starting vectors for decoder
    decoder_input = Variable(torch.LongTensor([[SOS_token]])) # SOS
    decoder_context = Variable(torch.zeros(1, decoder.hidden_size))
    **if** USE_CUDA:
        decoder_input = decoder_input.cuda()
        decoder_context = decoder_context.cuda()

    decoder_hidden = encoder_hidden

    decoded_words = []
    decoder_attentions = torch.zeros(max_length, max_length)

    # Run through decoder
    **for** di in range(max_length):
        decoder_output, decoder_context, decoder_hidden, decoder_attention = decoder(decoder_input, decoder_context, decoder_hidden, encoder_outputs)
        decoder_attentions[di,:decoder_attention.size(2)] += decoder_attention.squeeze(0).squeeze(0).cpu().data

        # Choose top word from output
        topv, topi = decoder_output.data.topk(1)
        ni = topi[0][0]
        **if** ni == EOS_token:
            decoded_words.append('<EOS>')
            **break**
        **else**:
            decoded_words.append(output_lang.index2word[ni])

        # Next input is chosen word
        decoder_input = Variable(torch.LongTensor([[ni]]))
        **if** USE_CUDA:
            decoder_input = decoder_input.cuda()

    **return** decoded_words, decoder_attentions[:di+1, :len(encoder_outputs)]

**def** evaluate_randomly():
    pair = random.choice(pairs)

    output_words, decoder_attn = evaluate(pair[0])
    output_sentence = ' '.join(output_words)

    **print**('>', pair[0])
    **print**('=', pair[1])
    **print**('<', output_sentence)
    **print**('')

evaluate_randomly()

**while** True:
    **try**:
        raw = raw_input(">")
        output_words, attentions = evaluate(raw)
        **print** output_words
    **except**:
        **pass**

**def** show_attention(input_sentence, output_words, attentions):
    # Set up figure with colorbar
    fig = plt.figure()
    ax = fig.add_subplot(111)
    cax = ax.matshow(attentions.numpy(), cmap='bone')
    fig.colorbar(cax)

    # Set up axes
    ax.set_xticklabels([''] + input_sentence.split(' ') + ['<EOS>'], rotation=90)
    ax.set_yticklabels([''] + output_words)

    # Show label at every tick
    ax.xaxis.set_major_locator(ticker.MultipleLocator(1))
    ax.yaxis.set_major_locator(ticker.MultipleLocator(1))

    plt.show()
    plt.close()

**def** evaluate_and_show_attention(input_sentence):
    output_words, attentions = evaluate(input_sentence)
    **print**('input =', input_sentence)
    **print**('output =', ' '.join(output_words))
    show_attention(input_sentence, output_words, attentions)
```

一旦这段代码完成训练，您将看到一个简单的提示，您可以在这里测试结果。在我的笔记本电脑上训练需要相当长的时间，因为我让我的 scraper 脚本运行了很长时间。你收集的训练数据越多，效果就越好，但是你必须自己试验，看看什么对你来说效果最好。

以下是我在笔记本电脑上进行了两个小时的训练后得出的句子结果:

>我需要食物[u ' food]，'【T4]']
>我在哪里可以找到食物[u ' food]，'< EOS > ']
>我想要一些食物>我的胃需要食物
>你好我需要食物[u ' food]，'< EOS > ']
>我想要巴黎的免费食物[u ' food]，'< EOS > '] < EOS > ']
>我要踢一场足球赛
>足球赛【u'football】，< EOS > ']
>我可以换酒店吗【u'change】，<EOS>'】
>换酒店我可以【u'change】，< EOS > ']
>怎么可以

如果根据更多数据进行训练，结果会显著改善。最终，这不是你在笔记本电脑上做的事情，我使用的最终版本是在云中训练的，以便能够在合理的时间框架内完成训练。

幸运的是，那里有几乎无穷无尽的数据供应，您可以向 scraper 脚本添加更多的博客 URL。

# 我们来实验一下！

这将是一个非常有趣的实验，看看我们是否可以颠倒整个过程，看看我们是否可以得出一个完整的句子，只给关键字作为输入，这肯定是我会尝试的事情，我会很快报告。

*原载于*[](http://theapemachine.com/code/keyword-extraction)**。**