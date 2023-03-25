# 如何下载您所有的原始 fitbit 数据

> 原文：<https://towardsdatascience.com/how-to-download-all-of-your-raw-fitbit-data-d5bcf139d7ed?source=collection_archive---------7----------------------->

出于好奇，有些人会想挖掘自己所有的原始 fitbit 数据。在本文中，我将讨论如何实际检索原始 fitbit 数据，假设他们有兴趣并且知道如何这样做。

# 2019 年更新

FitBit 现在提供一次性下载你所有的数据！从网页浏览器进入你的 fitbit [账户设置，将一个大的 zip 文件](https://help.fitbit.com/articles/en_US/Help_article/1133)放入队列。您将收到一封电子邮件，内容如下:

> 所需操作:确认您的 Fitbit 数据导出请求

按照那里的指示，你就完成了！下面的原始博客帖子仍然具有编程价值，FitBit 限制了您重新下载所有数据的 zip 文件的频率，因此仍然有理由使用我在这里描述的应用程序编程接口方法。感谢媒体用户[侏儒](https://medium.com/@nathan.f.rasmussen)在八月份对这个选项的评论。该功能至少从 2019 年 4 月开始出现。

## 附文

在这种情况下，短语“原始数据”不是指加速度计信号，如你慢跑时的手速，这些信号经过瞬时片内处理，以识别每个原子脚步或楼梯攀爬。蓝牙带宽和电池寿命方面的考虑限制了你的 fitbit 设备只能将聚合数据传输到你的手机、平板电脑或电脑上。

在这种情况下，原始数据是指可用的最细粒度形式的数据，即 fitbit 生成并上传到云中某处持续更新的数据库的所谓数据耗尽。目前，最精细的数据形式是你每分钟的步数、心率和睡眠时间序列。

FitBit(该公司，不同于 *fitbit* ，追踪器/应用)声明“你的数据属于你”:任何人都可以在任何时候访问他或她自己的数据。尽管如此，在实践中检索数据需要克服一些重大的技术障碍。在过去的两年里，[我开发并应用了一个 Python 框架](https://github.com/gully/ForTheBit)来查询我的 fitbit 数据并将其编译成易于使用的格式，以便进行交互式数据科学和个人生理学研究。这是我发现的。

# 以编程方式访问 fitbit 数据

*查看博客顶部今年的更新，您现在可以一次性下载您所有的 fitbit 数据！*

> 据我所知，没有办法一次性下载你所有的 fitbit 数据。—这篇博文，2018

FitBit 在其[开发者网站](https://dev.fitbit.com/)上提供[应用编程接口](https://en.wikipedia.org/wiki/Application_programming_interface)(API)，其中[门户网站](https://dev.fitbit.com/build/reference/web-api/)包含关于如何获取数据的适度高级指令。登录到 fitbit 帐户后，您可以阅读文档以了解系统如何工作。你将不得不用 RESTful HTTP 请求编写查询。如果您以前没有这样做过，这需要一点时间来适应，但是有一个例子可以说明这一点。如果您想获得从 2018 年 9 月 21 日开始的所有“日内心率数据”，您应该编写这个长 HTTP 查询字符串:

```
[https://api.fitbit.com/1/user/-/activities/heart/date/2018-09-21/1d/1min.json](https://api.fitbit.com/1/user/-/activities/heart/date/2018-09-21/1d/1min.json)
```

…但是你还没有完成。你需要说明你想如何处理这些数据——获取、更改或删除——并且你需要证明你有权这么做。有史以来最受欢迎的 Python 包之一， [requests](http://docs.python-requests.org/en/master/) ，如果您了解 Python，它允许您以编程方式执行这些命令。每一种现代计算语言都有一个等同于请求的语言，但是我将主要关注 Python。在 Python 中，您可以:

```
import requests
response = requests.get(query_str, headers=secret_header)
```

其中`query_str`是上面的查询字符串，`secret_header`包含关于您的授权的信息。

## 批准

只有你能看到你的“日内”数据 FitBit 采用这个名称来表示你的心率和步数的每分钟时间序列。FitBit 使用标准的加密安全认证程序来验证你的身份，它们使检索当天数据变得特别困难:你需要注册一个免费的个人应用程序来获得 API 客户端证书。这篇 [2016 的博文](http://shishu.info/2016/06/how-to-download-your-fitbit-second-level-data-without-coding/)描述了如何让你的`secret_token`成为标题。一旦你有了令牌，你就可以写:

```
secret_header = {‘Authorization’: ‘Bearer {}’.format(secret_token)}
```

注意，这个秘密令牌不应该公开共享——毕竟，它是秘密的。

## 保存和检查数据

您得到的响应对象包含`json`格式的数据，它有多层原始数据和元数据。出于备份目的，您可以将未删节的数据保存在本地(推荐):

```
import json
with open(path, ‘w’) as f:
     json.dump(response.json(), f)
```

…其中,`path`是一个描述性的文件名，您可以编造，例如`HR_20180921_1d_1min.json`。当您准备好分析您的数据时，您可以将笨拙的 json 格式打开成更方便的`pandas` `DataFrame`:

```
import pandas as pd
with open(path, ‘r’) as f:
     json_data = json.load(f)
df = pd.DataFrame(json_data[‘activities-heart-intraday’][‘dataset’])
```

现在，您可以开始检查、合并和转换数据，以制作一些令人惊叹的图表。根据我的经验，我不得不做一些标准的清理操作，比如将索引设置为日期和时间，并将“值”重命名为更具信息性的名称，比如“heart_rate”或简单的“HR”。数据清理、合并和分析的主题是另一篇文章的主题。

![](img/3016c4369877f605e496867f7db8300d.png)

My intraday heart rate data for January 1, 2017, from midnight to midnight.

## 纵向扩展和横向扩展

此时，下一步是获取**所有数据**。你可能想写一个`for`-循环所有可用的日期和所有可用的数据类型。你将遇到的主要问题是，FitBit 只允许你每小时请求 150 包数据。这种看似反常的速率限制可能令人沮丧，但在公共 API 中是一种常见的做法——我们将不得不绕过节流。在你的 for 循环中插入下面的小技巧，在计算机等待下一个小时的时候显示一个实时的进度条:

```
from tqdm import tqdm
import timeif response.status_code == 429:
     wait = int(response.headers[‘Fitbit-Rate-Limit-Reset’])+30 
     for seconds in tqdm(range(wait)):
         time.sleep(1)
```

如果你有一个启用心率的 fitbit(如 *Charge 2* 或 *Alta HR* ，那么你将可以访问至少三个当天数据流:1) **步数**，2) **心率**，以及 3) **睡眠**。如果你有，比如说，2 年的这些数据，你最终会得到**成千上万的文件** :
`2 years * 365 days/year * 3 streams/day * 1 file/stream = 2190 files`

…假设您将每天和每个流的完整 json 数据保存到自己的文件中。你一次只能下载 150 个文件，所以你要花 15 个小时才能得到所有 2 年的数据。FitBit 文档确实一次提供了更多天的查询，所以如果您不耐烦等待 15 个小时以上，您可以尝试这些替代方法。

## 辅助数据

你可能还想要其他类型的数据，比如健身活动日志、可选的 GPS 地图文件、体重日志(假设你有 fitbit *Aria* ，或者你手动记录你的体重)，等等。我不会详细介绍如何下载这些文件，但是这些方法非常相似，并且在 FitBit Web API 上有很好的记录。

# 回顾和展望

普通 fitbit 用户能够完成所有这些步骤的现实程度如何？不太可能。然而，普通的 fitbit 用户可能会从非常好的 fitbit 应用程序和网络仪表盘中获得大多数可用的见解。原始的 fitbit 日内数据很吸引人，但从中收集见解需要一些相当先进的数据科学技能，或者至少需要大量的奉献。