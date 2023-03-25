# 用 Python 为你的 Mac 构建一个简单的语音助手

> 原文：<https://towardsdatascience.com/building-a-simple-voice-assistant-for-your-mac-in-python-62247543b626?source=collection_archive---------2----------------------->

## 最基本的 python 语音助手，可以帮你打开电脑应用或者搜索互联网

像 Siri 或 Cortana 这样的语音助手可以为你做一些很酷很有用的事情:打开应用程序、搜索信息、发送消息等等。虽然用别人给你的很酷，但是自己做更有趣。在这篇文章中，我将向你展示如何*开始*构建一个语音助手框架，它可以做完全开发的工具在 Mac 上所能做的 1%的事情——抱歉，我不会对任何其他操作系统做太多介绍。

![](img/a66be1a282024ce354cd298e59121429.png)

Ironic Cortana Reference Since We’re on Mac — [Source](https://www.google.com/url?sa=i&source=images&cd=&cad=rja&uact=8&ved=2ahUKEwjCxbv8vd7cAhWlT98KHT9CDd4QjRx6BAgBEAU&url=https%3A%2F%2Fwww.windowscentral.com%2Fai-bots-and-canvases-conversation-part-ii-microsofts-long-road-ai-and-bots&psig=AOvVaw04TjHeK41lJOaXvCPwCiLh&ust=1533853026653434)

就像我上一篇关于使用你自己的短信创建聊天机器人的博文一样，这不会是一个完全成熟的想法，也不会让你在制作中走得太远。老实说，我花了比编码更长的时间来寻找/制作这个帖子的 gif。

然而，如果你想知道如何用 python 创建自己的语音助手，并且喜欢瞎折腾的话，这篇博文应该会给你一个很好的起点。还有一些其他的帖子有着几乎相同的基本概念——但是在 TDS 上没有看到任何类似的东西，我想这到底是怎么回事🤷🏻我可以写得更好，我的会更有趣。

在这篇博文中，我将展示两种不同的语音激活功能:

1.  打开应用程序
2.  搜索互联网

# 设置和安装

我们首先需要两个特别的包裹。第一个是使用麦克风— [这篇文章](https://realpython.com/python-speech-recognition/)很好地介绍了 python 中的语音识别。第二个和我过去的帖子一样，用 [elasticsearch](https://www.elastic.co/) 做查询-查询匹配，返回一个响应。

在 Mac 上，用自制软件安装`portaudio`和`elasticsearch`。家酿是一个伟大的 macOS 软件包管理器，可以在这里下载。然后在终端运行中:

```
brew install portaudio
brew install elasticsearch
```

我将使用`pip`进行 python 包管理。我们将使用的`speech_recognition`包将语音翻译成文本，而`pyaudio`是连接到你的计算机的麦克风所必需的。我们还将使用 elasticsearch Python 客户端来简化我们的工作，我正在使用 Python 3。

```
pip install SpeechRecognition
pip install pyaudio
pip install elasticsearch
```

# Python 中的语音识别

这个`speech_recognition`包很棒，因为正如前面的文章中所说的,“SpeechRecognition 库充当了几个流行的语音 API 的包装器，因此非常灵活。其中之一——Google Web Speech API——支持一个默认的 API 键，该键被硬编码到 SpeechRecognition 库中。

![](img/77d83b7e3d8bd82144b9ca0f755971f7.png)

[Source](https://www.google.com/url?sa=i&source=images&cd=&cad=rja&uact=8&ved=2ahUKEwiS_5eM4NncAhVmkuAKHZU5DQYQjRx6BAgBEAU&url=https%3A%2F%2Fgifer.com%2Fen%2F7VTb&psig=AOvVaw1vHDMJQ-SJ6JyAq5lX6htl&ust=1533690378156041)

超级方便，如果你不想为任何其他供应商建立一个帐户。在不到 10 行代码中，这个包应该让我们连接到我们内置的计算机麦克风，并转录我们的声音！

```
import speech_recognition as srr = sr.Recognizer()
mic = sr.Microphone()with mic as source:
    r.adjust_for_ambient_noise(source)
    audio = r.listen(source)
    transcript = r.recognize_google(audio)
    print(transcript)
```

这段代码应该运行到它听到什么为止，然后把你说的话(或者它认为你说的话)打印出来。超级简单。关于这些类以及它们如何工作的更多信息，请参考前面提到的帖子。

# 给你的电脑一个声音

在 Python 中有多种方法可以做到这一点，但是最简单的方法就是使用 macOS 自带的方法。使用子进程包，我们可以很容易地让我们的计算机说话:

```
import subprocessdef say(text):
    subprocess.call(['say', text])say("yo whatup dog")
```

# 打开电脑上的应用程序

所以现在你可以把声音转换成文字，让你的电脑说话，世界就是你的了，生病了。现在我们可以决定如何使用输入。对于一个简单的想法，我的第一反应是用它来打开应用程序。显然，它一点也不酷，但作为一个例子，它工作得很好，我将使用的一般结构可以应用于更广泛的功能。因此，让我们首先获得我们所有应用程序的列表。

![](img/4dec5e4b400572e7b8e474d9c36e4aec.png)

在 Mac 上，我想打开的所有应用程序都在`/Applications`目录中，并且大部分都有一个`.app`扩展名，所以我们可以用如下方式获得它们的名称:

```
d = '/Applications'
apps = list(map(lambda x: x.split('.app')[0], os.listdir(d)))
```

列出目录并删除扩展名。酷毙了。

打开应用程序也很容易。这里有一个使用`os`包和 Spotify 的简单例子。

```
import osapp = 'Spotify'
os.system('open ' +d+'/%s.app' %app.replace(' ','\ '))
```

![](img/738a7e45741b02e3fb6aa7468af07b13.png)

`.replace`只是为了在较长的应用程序名称中适当地转义空格，但如果你安装了 Spotify，并且在你的应用程序中，它应该会打开。

# 在 Elasticsearch 中索引您的数据

将语音脚本与功能绑定在一起的显而易见的解决方案是某种精确匹配，如果`transcript == 'open spotify'`那么这样做…我可能不必解释这是如何不可扩展的。此外，使用像 elastic 这样的酷框架会更好。此外，如果没有任何实际的数据或算法，它就不是一篇数据科学博客。

来自我在 chatbots 上的上一篇博文——“elastic search 是一个可扩展的搜索平台，它使用类似于 TF-IDF 或词频逆文档频率的算法。本质上，它是一个简单的功能，经常在搜索/相似性空间中使用，根据关键字定位文档…有关它的基本概述，请查看[维基百科](https://en.wikipedia.org/wiki/Tf%E2%80%93idf)。”

![](img/7c0b6438a35e01dd1ebb2c330a38811b.png)

[Source](https://www.elastic.co/products/elasticsearch)

基本上，我们会将所有语音命令(类似于“打开 spotify”)上传到 elasticsearch，并将它们映射到我们想要运行的系统命令(实际的编程命令)。为此，我们可以快速创建一个字典列表。

```
import osd = '/Applications'
records = []
apps = os.listdir(d)for app in apps:
    record = {}
    record['voice_command'] = 'open ' + app.split('.app')[0]
    record['sys_command'] = 'open ' + d +'/%s' %app.replace(' ','\ ')
    records.append(record)
```

现在，我们已经将所有的语音命令和系统命令相互映射。接下来，打开一个终端，启动 elasticsearch。如果你是通过自制软件安装的，它会在你的路径中，所以只要输入`elasticsearch`，它就会在默认端口 9200 上启动。

接下来，我们可以索引我们的数据，使其可搜索。我们启动 python 客户机，将数据放入名为`voice_assistant`的新索引中。

```
from elasticsearch import Elasticsearch
from elasticsearch.helpers import bulkes = Elasticsearch(['localhost:9200'])
bulk(es, records, index='voice_assistant', doc_type='text', raise_on_error=True)
```

现在你的数据都可以在索引`voice_assistant`中搜索到了。我们从下面的搜索函数中得到的`response`变量是由被称为[查询 DSL](https://www.elastic.co/guide/en/elasticsearch/reference/current/_introducing_the_query_language.html) 的语言生成的。我们基本上是在语音命令上做一个简单的 TF-IDF 搜索，有些模糊——考虑到拼写错误/糟糕的转录。

```
def search_es(query):
    res = es.search(index="voice_assistant", doc_type="text", body={                     
    "query" :{
        "match": {
            "voice_command": {
                "query": query,
                "fuzziness": 2
            }
            }
        },
    }) return res['hits']['hits'][0]['_source']['sys_command']
```

现在尝试一下吧！`search_es('open spotify')`应该返回我们将传递给`os.system`也就是`open /Applications/Spotify.app`的命令。因为我们将`fuzziness`参数设置为 2，所以`search_es('open spofity')`也将返回相同的内容，即使有拼写错误。

尽管这是一个很小的功能，但这里的关键是这个原则可以应用于许多不同的功能——Q & A、其他命令等等

# 在语音助手中将所有这些结合在一起

现在，我们可以将语音转换成文本，并对所有命令进行适当的索引，我们可以使用`search_es`函数来访问这些命令。剩下的就是把它们绑在一起了！让我们创建一个寻找用户输入的脚本，当它得到用户输入时，它在我们的索引中搜索该命令，然后尝试运行它。唯一缺少的是一个短语来触发我们的聊天机器人，所以让我们再定义一个助手函数，这样我们就有一个关键字短语来激活它，我选择我的只是“嘿伙计”。

```
def activate(phrase='hey dude'):
    try:
        with mic as source:
            r.adjust_for_ambient_noise(source)
            audio = r.listen(source)
            transcript = r.recognize_google(audio)
            if transcript.lower() == phrase:
                return True
            else:
                return False
```

使用这个触发器，我们现在可以更有选择地激活我们的实际搜索功能。在我们之前定义的函数的帮助下，我们可以将代码封装在一个无限循环中，这样它就会一直运行，等待我们激活它。我让我的助手在运行任何东西之前问我它能有什么帮助，显然你可以随意用你的名字替换“凯尔”。

```
import speech_recognition as sr
import os 
from elasticsearch import Elasticsearch##start python client 
es = Elasticsearch(['localhost:9200'])##initialize speech recognizer and mic
r = sr.Recognizer()
mic = sr.Microphone()while True:   
    if activate() == True:
        try:
            say("Hey Kyle, how can I help you today?")
            with mic as source:
                print("Say Something!")
                r.adjust_for_ambient_noise(source)
                audio = r.listen(source)
                transcript = r.recognize_google(audio)
                sys_command = search_es(transcript)
                os.system(sys_command)
                say("I opened that application for you")
        except:
            pass
    else:
        pass
```

需要注意的一点是——因为我们这里有所有的`except`语句，一旦你启动了它，你就不能用键盘来中断它，你必须停止整个进程，或者如果你在 jupyter 中重启内核——请随意构建更合适的错误处理。

运行它，开始不分青红皂白地打开应用程序，因为你可以！很酷。只要确保你已经启动了`elasticsearch`，并且定义了你所有的助手函数。

# 额外收获:用 Selenium 搜索谷歌的语音助手

打开应用程序很酷，但我肯定不会真的用它…让我的电脑自动搜索我说的任何东西是这个助手更好的用途。你或许可以使用`requests`和`BeautifulSoup`库对这部分进行更多的定制，但是如果你只是让`selenium`打开一个 Chrome/Firefox/随便什么窗口为你搜索，那就真的没有意义了。

![](img/152cf669b7e98fb897e55bc3adaf3ce1.png)

## 安装 selenium 和 web 驱动程序

只需使用`pip`安装必要的 python 包，并确保您已经安装了想要使用的 [webdriver](https://docs.seleniumhq.org/projects/webdriver/) 。我会用 chrome 的那个。

`pip install selenium`

## 从 python 搜索 google

用`selenium`从谷歌搜索超级简单。我使用 Chrome 作为我的首选浏览器，所以我从下面的`webdriver.Chrome()`开始使用 selenium。如果你用别的东西，请随意替换它。

```
from selenium import webdriver
from selenium.webdriver.common.keys import Keysdef search_google(query):
    browser = webdriver.Chrome()
    browser.get('[http://www.google.com'](http://www.google.com'))
    search = browser.find_element_by_name('q')
    search.send_keys(query)
    search.send_keys(Keys.RETURN)
```

就是这样。Selenium 在一个窗口中打开谷歌，找到搜索栏，为我们键入内容，然后单击搜索按钮。您可以快速测试它，以确保它正常工作:

```
search_google('how to build a voice assistant in python')
```

## 谷歌搜索的语音助手

不像以前，我们没有一个弹性搜索索引来查找我们的信息——我们的搜索更加动态，我们必须做一些更简单的文本预处理。假设我们的查询将包含短语“search google for ”,我们可以在那里分割字符串，并将字符串的其余部分传递给我们的函数。

```
while True:
    if activate() == True:
        try:
            say("Hey Kyle, how can I help you today?")
            with mic as source:
                print('Say Something!')
                r.adjust_for_ambient_noise(source)
                audio = r.listen(source)
                transcript = r.recognize_google(audio) phrase = 'search google for ' if phrase in transcript.lower():
                    search = transcript.lower().split(phrase)[-1]
                    search_google(search)
                    say("I got these results for you")
                else:
                    say('I only search google, sorry fam')
        except:
            pass
    else:
        pass
```

# 未来方向

酷，现在你也有一个语音助手不断听你搜索互联网或打开应用程序。你还可以做更多的事情:在同一个助手中把它们绑在一起(我不喜欢),整合新闻/天气 API，也许自动化电子邮件内容，在 elasticsearch 中放入你自己的自定义短语，为激活建立一个更复杂的模型，等等……但这只是一个不错的开始。希望你喜欢它！