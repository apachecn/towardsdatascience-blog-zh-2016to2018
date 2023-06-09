# 我和 JSON 的恋情

> 原文：<https://towardsdatascience.com/my-love-affair-with-json-edaca39e8320?source=collection_archive---------0----------------------->

![](img/d3cedbcc654f68d4f1738b1ac092f9cf.png)

Photo by [Glenn Carstens-Peters](https://unsplash.com/photos/npxXWgQ33ZQ?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) on [Unsplash](https://unsplash.com/photos/0m-eVEm7mfo?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

对我来说， [JSON](https://www.json.org/) (JavaScript 对象符号)是一种令人愉快的数据共享/传输格式。使用和阅读都很简单。处理起来非常快。它也是更容易阅读的 [YAML](http://yaml.org/) 的子集。

我已经发展了个人的最佳实践，用于处理以 JSON 为核心的数据，我将在下面分享。这些最佳实践是我从他人那里学到的，也是我在处理生物制药数据时的个人经验，这些数据虽然一般不是很大，但往往是非常异构的，数据格式也不是很稳定。我不想建议您对所有事情都使用 JSON。例如，出于性能原因，有时需要二进制格式。

对于大多数语言来说，它的序列化和反序列化速度非常快，这意味着它可以轻松快速地发送到另一个进程，保存到磁盘/文件和检索。您可以将数据对象作为 JSON 保存到磁盘，然后将该文件发送给使用不同语言的其他人，通常他们选择的编程语言中有一个库/包，可以轻松地将它从 JSON 数据对象反序列化为数据结构变量。

> 序列化意味着转换为磁盘上的文件，例如，将变量(数据对象、列表或字典/哈希)从应用程序转储到磁盘。反序列化则相反，将文件读入应用程序变量。
> 
> javascript 对象、python 字典、perl 散列或关联数组基本上是相同的概念，尽管我用 javascript 对象扩展了一下。任何使用键、值格式的数组，如 fruit_colors['apple'] = 'red '。此后，我将把这个数据结构称为散列。

# 最佳实践 1——JSON 适用于所有事情

尽可能使用 JSON 在应用程序和数据科学家之间共享结构化数据。为了以最大的通用性轻松做到这一点，在使用 ISO 日期格式保存到 JSON 之前，请始终将日期转换为字符串。我没有在我的 JSON 包中添加定制的 JSON 格式化程序来处理日期和其他非 JSON 的简单格式(例如，字符串、数字、数组、散列)，我通常只是将其转换为字符串表示，并在反序列化时将其从字符串转换回日期。

```
JSON hash and array example:{
  "key": "value",
  "example_array": [
    "one",
    "two"
  ]
}
```

我通常将 JSON 序列化到带有缩进和换行的数据文件中，以使其更容易阅读，但您不必这样做。您可以将所有内容都放在一行中，没有多余的空格或换行符，以使其更小，尽管我发现这通常没有什么影响，因为如果 JSON 数据非常大，在存储或传输到另一个服务器时，您总是希望对其进行压缩。*对于 REST API 服务器传输 JSON，你总要在 webserver 上开启* [*压缩*](https://www.nginx.com/resources/admin-guide/compression-and-decompression/) *。*

# 最佳实践 2 — JSONSchema

使用 [JSONSchema](http://json-schema.org/) 来描述你的数据集。即使您只是自己使用它，它也可以帮助您思考您存储的内容，并提供描述属性，以便于记录您的格式。这是描述易于共享的 JSON 数据的最佳标准方式，大多数编程语言都可以用它来验证数据集。

在 JSONSchema 中，并不是数据集验证的每个方面都可以被捕获，有些东西需要放到代码中，但是它可以捕获数据结构定义的大多数方面。

> 我将经常构造我的 JSONSchema 定义，以使用 JSON 数组**和** JSONLines(下面解释)格式——这里的例子[是](https://github.com/belbio/schemas/blob/master/schemas/nanopub_bel-1.0.0.yaml)。

如果您认为 JSONSchema 文件具有更广泛的公共利益，那么 [JSON Schema Store](http://schemastore.org/json/) 是发布 JSON Schema 文件的好地方。

> 我已经开始在我的 JSON 数据中添加 JSONSchema 定义的 url，这样我就有了用于该数据的模式定义的引用——特别是如果它是一个长期演化的数据集。

# 最佳实践 3——YAML 是

使用 [YAML](http://yaml.org/) 进行 JSONSchema 定义。是的，JSONSchema 定义的 YAML。一般来说，要使用 JSONSchema 验证 JSON，您必须首先将 JSONSchema 定义文件加载/反序列化到您的应用程序中进行验证，因此使用什么格式并不重要。YAML 允许评论(JSON 不允许)，对人类来说更容易阅读。

```
# You can have comments in the YAML file like this
key: value
example_array:
  - one
  - two
```

有一个很好的 python 包叫做 [json2yaml](https://github.com/drbild/json2yaml) ，它很好地完成了 json 和 yaml 文件*之间的来回转换，同时保持了散列的顺序*。如果您将 YAML/JSON 用于配置或者有标准的数据顺序，那么保持它的自然顺序会很方便。大多数 JSON/YAML 转换器将按字母数字顺序对数据进行排序。当来回转换时，你会丢失 YAML 的评论，所以我倾向于使用 YAML 作为真实的原始来源，并根据需要转换成 JSON。

我更喜欢将 YAML 用于人类可读的、可维护的配置文件。这种模式的一个例子是使用 YAML 编写 [BEL](http://bel.bio) 语言规范文件，然后将它转换成一个增强的 JSON 文件，为了方便起见，还以编程方式生成了附加部分。

# 最佳实践 4 — YAML 第一

除非您已经检查了 YAML 解析器的性能，否则不要将 YAML 用于与性能相关的任务。我没有意识到 Python 中的 YAML 解析器比 JSON 慢这么多。在 Python 3.4+中， [JSON 的执行速度几乎与任何内置序列化格式(例如 pickle)](https://stackoverflow.com/a/26860404/886938) 一样快。

读入大约 1000 行长的 BEL 规范 YAML 文件大约需要 150 毫秒。读入该文件的增强 JSON 版本大约需要 6 毫秒，这个版本大约长了 50%。YAML 是一种比 JSON 更复杂的解析结构，而且它也没有那么受欢迎，所以性能调整较少。

因此，YAML 非常适合在需要大量人工交互和编辑的地方使用，并且不会造成瓶颈，因为它必须在高吞吐量的数据管道中反复解析。

# 最佳实践 5 — JSONLines

[JSONLines](http://jsonlines.org/) 是一种将较小的 JSON 数据一起存储到单个数据集的好方法，当您想要减少处理的内存开销或能够处理流数据时。JSONLines 的概念是数据文件的每一行都是一个单独的 JSON 对象，如下所示。

```
JSONLines - each line below is a separate JSON object{"key": "value1", "key2": "key2value"}
{"key": "value2", "example_list": [1, 3]}
[1, 5, 10]
```

我处理术语数据集中的术语(BEL 名称空间),这些术语可能有数百万个条目。如果我使用一个像下面这样的顶级数组，我必须将所有这些项读入内存来处理它们。因为我一次只需要处理一个术语，所以我可以使用 JSONLines 格式一次读取一行(一次读取一个术语),并使我的内存开销降低几百万倍。

```
JSON array of objects:[
  {
    "key": "value1"
  },
  {
    "key": "value2"
  }
]
```

如果我在一个 JSONLines 文件中有多种类型的数据对象，我将添加一个顶级键来标识每种类型。一个例子是:

```
{"metadata": {"author": "William Hayes", "date": "20180101", ...}
{"nanopub": {"id": 1, "citation": {...}, "assertions": {...} }
{"nanopub": {"id": 2, "citation": {...}, "assertions": {...} }
```

处理 JSONLines (*)的一些小技巧。jsonl)文件。我将较大的 JSONLines 文件进行了 gzip 压缩，并使用了基于以下内容的自定义 bash 函数:

```
gunzip -c $1 | jq . | more;
```

要 gunzip 到 STDOUT，使用 [jq](https://stedolan.github.io/jq/) 使用缩进/换行格式化每个 line/JSON 对象，并一次查看一页。我个人发现 gzip/gunzip 在速度/压缩方面是一个很好的平衡。Bzip2 压缩效果更好，但速度慢得多。你的需要可能与我的不同。

# 最后的想法

谢谢你，谢谢你，谢谢你道格拉斯·克洛克福特指定 JSON 并帮助它取得今天的成功。我经历过 CSV、ASN.1、SGML、XML 和各种风格的定制数据格式，无法在这里充分表达我对 JSON 的热爱。各种语言的 JSON 解析器、JSONSchema、YAML 等的创建者对 API、数据科学和信息学产生了巨大的影响，但这种影响还没有得到充分的认识。