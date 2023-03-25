# 为圣保罗地铁运营构建数据集

> 原文：<https://towardsdatascience.com/building-a-dataset-for-the-s%C3%A3o-paulo-subway-operation-2d8c5a430688?source=collection_archive---------11----------------------->

![](img/338cf4ca48c345c302e8fa8370d390b4.png)

Photo by [Rafael De Nadai](https://unsplash.com/photos/bC5bAMUvaOs?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) on [Unsplash](https://unsplash.com/@rafaelnadai?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

你坐过拥挤的地铁吗？我知道大多数大城市都会出现这种情况，但在圣保罗，情况可能是这样的:

![](img/4324bd49fb69f91e437d8eb38b268dd9.png)

This is what a crowded subway station may look like here in SP

现在，作为一名巴西人和一名*保利斯塔诺*，我能做些什么呢？(这是葡萄牙语中“住在圣保罗的人”的意思)

Metr 地铁公司的[透明](https://transparencia.metrosp.com.br/)网页提供了关于乘客需求的数据，但粒度较低，对每条地铁线工作日和周末的平均条目进行了分组。这还不错，但我们可以做得更好。

![](img/f3df9b64daec0803615381559f695572.png)

The [transparency](https://transparencia.metrosp.com.br/) webpage for the Metrô subway company provides data on the passenger demand but with low granularity

我知道 ViaQuatro 公司的网页提供了所有地铁线路的(几乎)实时运营状态，该公司与政府有一个地铁线路运营联合体:

![](img/a0e1162bf0e216eede12b00e88b637f9.png)

The ViaQuatro company webpage provides operation status for all of the subway lines, as highlighted in red

每天乘坐 SP 地铁的人直观地知道，一个 ***正常*** 运行状态不足以推断车站不会拥挤，但是一个 ***减速*** 或 ***瘫痪*** 状态可能与第一张图中所示的混乱有关。

不幸的是，这些数据没有被存储。至少对我们这些普通公民来说不是这样，所以我们可以对它进行访问、监控、评估、展示等等。我们为什么不使用 Python 来解决这个问题呢？

本文剩余部分显示的步骤是一个教导性的，尽管是项目的完全功能性演示。万一你是开发者或者想直奔最终结果，可以随意探索 [*Github 资源库*](http://github.com/douglasnavarro/sp-subway-scraper) *。*

所以…

# 我们的目标是以非侵入性的方式持续存储圣保罗地铁线的运营状态历史，并使任何公民都可以方便地使用。

# 为了达到这个目标，我们将编写一个能够请求 ViaQuatro 网页，提取操作状态和时间戳并存储它的自治 web scraper。

为此，我们将需要`requests`包来下载网页，并需要`BeautifulSoup`来轻松处理网页:

```
>>> import requests
>>> from bs4 import BeautifulSoup
```

好了，要下载这个页面，我们需要做的就是

`>>> vq_home_request = requests.get('http://www.viaquatro.com.br')`

如果请求成功，我们就有了 HTML 代码

```
>> vq_home_content = vq_home_request.text 
>> vq_home_content'\r\n<!DOCTYPE HTML>\r\n<html lang="pt-BR">\r\n<head>\r\n\t<meta charset="UTF-8">\r\n    **<title>ViaQuatro | Seja Bem-vindo</title>**\r\n\t<meta name="description" content="ViaQuatro - Informa&#231;&#245;es sobre rotas, tarifas, esta&#231;&#245;es e muito mais." /> \r\n\t<meta name="keywords" content="" />\r\n    <meta http-equiv="X-UA-Compatible" content="IE=edge" >\r\n\t<script src="/Content/js/3rd/modernizr.custom.97134.js"></script>\r\n    <!-- Use Zepto for best performance on WebKit based browser -->\r\n\t\t<!-- <script src="js/zepto/zepto.js" type="text/javascript" charset="utf-8"></script> -->\r\n\t<link rel="stylesheet" type="text/css" href="/Content/css/default.css" media="all" /> **<!-- pra ser carregado em todas as páginas -->**\r\n
(...)
```

![](img/6f96a2a721fc157e59ece7736cf44539.png)

We do have the HTML inside our environment!

现在让我们使用 *BeautifulSoup* 来浏览 DOM 并提取标题页面，以检查我们是否在 BeautifulSoup 对象中组织了 HTML 元素:

```
>>> soup = BeautifulSoup(vq_home_content, 'html.parser')
>>> soup.title
<title>ViaQuatro | Seja Bem-vindo</title>
```

现在到了困难的部分——提取真正重要的东西。使用开发工具检查 HTML，我们看到显示地铁线路当前状态的“操作”面板由一个`<section class="operacao">`元素组成。里面有我们需要的所有数据。因此，让我们把它拉出来，这样我们就可以对它进行以下查询:

`>> operation_column = soup.find(class_= "operacao")`

![](img/aa306cce9c428d2460ab4b6c14df5f6f.png)

We need to inspect the page code in order to find out what element contains the data we want

让我们也使用 [*字典理解*](https://www.python.org/dev/peps/pep-0274/) *:* 初始化一个字典来存储行状态

```
>> lines_metro = ['azul', 'verde', 'vermelha', 'amarela', 'lilás', 'prata']>> lines_cptm  = ['rubi', 'diamante', 'esmeralda', 'turquesa', 'coral', 'safira', 'jade']>> all_lines = lines_metro + lines_cptm>> **extracted_status = {line:'' for line in all_lines}**>> extracted_status
{'amarela': '', 'vermelha': '', 'diamante': '', 'turquesa': '', 'coral': '', 'prata': '', 'lilás': '', 'esmeralda': '', 'safira': '', 'rubi': '', 'verde': '', 'azul': '', 'jade': ''}
```

继续我们的调查，我们看到 Amarela(黄色)地铁线的状态包含在一个带有名为*状态的css 类的 *span* 标签中。*

```
>> status_amarela = operation_column.find(class_="status").text
>> extracted_status['amarela'] = status_amarela
```

![](img/7f8a325b8e2cfbf12bb3b9920626767f.png)

The operation status for the Amarela (yellow) subway line is wrapped around a ***span*** tag with acss class called ***status***

对于所有其他线路，数据被分成三个标签`<div class="linhas">`，每个标签代表一家参与城市铁路系统的公司。在每一个*div*中，我们都有一个*无序列表*标签，其中每一个*条目*都包含行号、其名称和当前在**另一个看起来像`<div class="info">`的 div** 中的操作状态。

为了提取所有这些内容，我们将遍历`<div class="linhas">`div，然后遍历每个`<div class="info">` div，以获得我们真正想要的内容:操作状态和行名:

![](img/084d6c2efc88600e9fb83c022cf44ec1.png)

we will iterate over the `<div class="linhas">` divs and then iterate over each `<div class="info">` div to get what we really want

```
>> lines_containers = operation_column.find_all(class_ = "linhas")>> for container in lines_containers:
       line_info_divs = container.find_all(class_ = "info")
       for div in line_info_divs:
           line_title  = ''
           line_status = ''
           spans = div.find_all("span")
           line_title = spans[0].text.lower()
           line_status = spans[1].text.lower()
           extracted_status[line_title] = line_status>> extracted_status
{'safira': 'normal', 'prata': 'normal', 'rubi': 'normal', 'azul': 'normal', 'turquesa': 'normal', 'amarela': 'Operaç\xe3o Normal', 'lilás': 'normal', 'esmeralda': 'normal', 'coral': 'normal', 'verde': 'normal', 'diamante': 'normal', 'jade': 'normal', 'vermelha': 'normal'}
```

现在我们有了字典中所有行的操作状态！另一个重要信息是该数据的时间戳。它显示在“操作”面板中，就在标题的右边。这是页面中唯一的`<time>`元素。让我们把它拉出来:

```
>> time_data = soup.find('time').text
>> time_data
'18/08/2018 23:11'
```

**终于，我们有了所有的数据！我承认我需要一点时间来欣赏这一点。**

![](img/b749ed30cfbe74df068ae116f10904f8.png)

Web scraping is so cool! [Source](https://giphy.com/gifs/curbyourenthusiasm-l1J9yTco40EU5JzTW)

此时，我们可以简单地将`extracted_status`和`time_data`写入本地系统中的一个文件，或者利用互联网的魔力将其存储在任何地方。

但是让我们更进一步，使用 **Google Sheets** ,因为它非常受欢迎，非开发人员也可以访问，并且提供了一个有良好文档记录的 API。最重要的是， [gspread 包](https://github.com/burnash/gspread)为我们包装了所有的请求，并且 [oauth2client](https://github.com/google/oauth2client) 使用我们的 API 凭证处理令牌获取过程。

我知道有很多隐藏的东西，但是能够用这么少的代码做这么多的事情是用 Python 这样的语言编程的美妙之处之一。

简单地说，我们需要做的是:

*   获取我们的谷歌证书，以便以后使用它们进行授权(这是手动的，只需要做一次)
*   通过我们的代码使用凭据获得授权
*   通过我们的代码将数据写入公共电子表格

要获得证书，我们可以按照 gspread 文档提供的这个逐步指南[进行操作。在这个过程结束时，您得到的是一个类似如下的`client_secret.json`文件:](https://gspread.readthedocs.io/en/latest/oauth2.html)

```
{
    "private_key_id": "2cd … ba4",
    "private_key": "-----BEGIN PRIVATE KEY-----\nNrDyLw … jINQh/9\n-----END PRIVATE KEY-----\n",
    "client_email": "473000000000-yoursisdifferent@developer.gserviceaccount.com",
    "client_id": "473 … hd.apps.googleusercontent.com",
    "type": "service_account"
}
```

这很重要:我们需要与我们的 API 用户共享我们将要使用的电子表格。否则，在尝试写入数据时，我们会收到拒绝访问错误。这个用户由 json 文件中的 *client_email* 字段表示。

![](img/91ae4079c183073d0d8e8568edd1e312.png)

This is important: we need to share the spreadsheet we will be using with our API user

回到代码，让我们导入 *gspread* 和 *oauth2client* 并从。json 文件到我们的环境中:

```
>> import gspread
>> from oauth2client.service_account import ServiceAccountCredentials>> scope = ['https://spreadsheets.google.com/feeds',
            'https://www.googleapis.com/auth/drive']
>> creds = ServiceAccountCredentials.from_json_keyfile_name(
           'client_secret.json',
           scope)
```

现在让我们使用我们的凭据获得授权，并初始化电子表格。要初始化电子表格，我们需要它的 ID，它显示在我们用来在浏览器中访问它的 URL 中:

https://docs.google.com/spreadsheets/d/**1 wiki 6 sucqaauf 9 qojog _ tme OSA _ lm _ kjm 83 qwxf 9 dsg**

```
>> client = gspread.authorize(creds)>> SPREADSHEET_ID = "1WiKE6SUCqAaUF9qOJOg_tMeOSA_lm_kjm83qwXF9dSg">> data_sheet = client.open_by_key(SPREADSHEET_ID).worksheet("data")
```

注意，我们不直接使用电子表格对象，而是使用其中的工作表，因此我们需要指定它的名称。

让我们写我们的数据！ *append_row* 方法确实如其名，我们只需要传递一个列表，其中每个元素将被写入该行的不同列。

```
>> for line in all_lines:
    data_sheet.append_row([time_data, line, extracted_status[line]])
```

![](img/37bfca3d15c01aa2f6f2a41423b300dc.png)

We can actually see the data being stored in real time!

为了拥有一个不会让你的电脑一直开着的自主刮刀，我们必须组织这段代码，并让它在云服务上运行。针对异常的一些保护也很重要，但是这些超出了本文的范围。如果你想了解更多的细节，可以随意看看 github 的回购。

我将在以后的文章中分析这些数据。我们看看会有什么结果！

【编辑】保罗[做了一个很棒的分析！看这里](https://github.com/pmhaddad)！