# 使用 Google Apps 脚本从您的 Google Analytics 帐户获取数据

> 原文：<https://towardsdatascience.com/using-google-apps-script-to-fetch-data-from-your-google-analytics-account-6f5fb800e98d?source=collection_archive---------5----------------------->

## 只需点击一个按钮，所有的谷歌分析数据都可以在谷歌工作表中找到，这怎么样？

![](img/2eebd5b0d23593c988c4fc08cde7b9ce.png)

Photo by [Myriam Jessier](https://unsplash.com/@mjessier?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) on [Unsplash](https://unsplash.com/?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

谷歌分析无需介绍。绝对是互联网上最受欢迎的网络分析服务，帮助企业收集和分析数据，并使用它们来发展和增强他们的服务或产品。访问 Google Analytics 数据的最常见方式是通过主仪表板和预定义的报告。为了处理数据并提取更有价值的见解，您可以创建标准或[自定义报告](https://support.google.com/analytics/answer/1033013?hl=en)来呈现您需要的数据，然后通常将它们导出到电子表格中。方法是使用 GA 控制台中提供的导出功能(PDF、Google Sheets、Excel 或 CSV 格式),然后将文件导入 Google 电子表格或任何其他电子表格软件。

![](img/8b21bfb1339ff0272043ba2c930c257b.png)

Image by Author

但是，如果您需要对多个文件这样做，或者在一段时间内定期这样做(例如，生成定期报告)，该怎么办呢？这很痛苦，不是吗？好吧，**谷歌应用程序脚本来拯救！**💪💪

## Google 企业应用套件脚本

那么，什么是 Google Apps 脚本呢？ [Google Apps Script](https://developers.google.com/apps-script/) 是一种 JavaScript 云脚本语言，提供了跨 G-Suite 产品和第三方服务自动执行任务以及构建 web 应用的简单方法。简而言之，使用 JavaScript，我们可以增加我们最喜欢的、已经很强大的谷歌应用程序的功能，比如电子表格、文档、日历、邮件等等。在我们的案例中，我们将关注 Google 电子表格和 Google Apps 脚本之间的集成。要访问脚本编辑器，我们可以在其中构建我们的应用程序，我们可以创建一个新的电子表格，并从顶部菜单中单击**工具>脚本编辑器**。

![](img/658d44cc28ec5ce2c19be1b7c1ecd885.png)

Image by Author

## 连接 Google 电子表格和 Google APIs

上述选项将使用脚本编辑器控制台打开一个新标签。使用这个控制台，我们可以构建我们的应用程序，创建脚本文件，连接资源等。在我们的例子中，我们将使用 Google Analytics API 作为资源来获取我们需要的数据。要启用 Google 电子表格和 Google 分析 API 之间的连接，我们必须遵循以下步骤:

1.  单击顶部菜单上的“资源”选项。
2.  点击“高级谷歌服务…”选项。
3.  为我们的项目命名(在这种情况下，我们将命名为“测试项目”)。
4.  搜索 **Google Analytics API** ，点击开关将其打开。

![](img/70a687ffc355a5966a4b59ff9c700c59.png)

Image by Author

5.先别急着关窗。为了获得完全访问权限，我们还必须在 Google API 控制台中启用我们选择的服务。如果我们点击提供的链接，我们将被直接重定向到 GCP 控制台，通过搜索顶部搜索栏上的“分析 API ”,我们将找到我们选择的服务。

![](img/a25ecaaa281d09cc0fa87363fe63c7e7.png)

Image by Author

6.点击启用，你就可以开始了！👏

## 查询浏览器

在处理数据和报告时，最重要的是我们的数据要一致和可信。建立别人对你数据的信任需要很长时间，但失去信任只需一分钟。在这个过程中我们最好的朋友是谷歌的一个非常有用的工具，叫做[查询浏览器](https://ga-dev-tools.appspot.com/query-explorer/)。Query Explorer 通过使用各种参数进行查询和测试响应来帮助我们处理 Google Analytics 数据。我们可以选择想要查询数据的帐户—属性—视图，只需添加参数(开始日期、结束日期、指标、维度、筛选器、细分等)。然后，我们可以下载 TSV 格式的数据(制表符分隔的文件)，或将直接 API 查询 URI 保存到该报告中。通过 Google Apps 脚本查询数据是模拟查询浏览器数据的一种方式，就我个人而言，我总是让查询浏览器在一个选项卡中打开来验证查询。

## 构建脚本

说够了。让我们开始行动吧。在我们创建了项目并启用了分析 API 之后，下一步是实际构建脚本来获取我们需要的数据。我们将需要 2 个标签不断在点击之外。

1.  [查询浏览器](https://ga-dev-tools.appspot.com/query-explorer/)(构建和验证查询)
2.  [Google Apps 脚本电子表格参考](https://developers.google.com/apps-script/reference/spreadsheet/spreadsheet-app)(一个非常详细且写得很好的文档)

为了从谷歌分析中查询数据，我们需要下面的函数。该函数接受 5 个参数作为输入，并返回一个报告作为输出。

```
function gaGet(tableId, startDate, endDate, metrics, options)
```

这些论点是:

1.  **tableId:** 实际上是我们要查询的视图的 Id。在查询资源管理器中，当我们选择适当的帐户-属性-视图时，会自动设置表 id(格式:“ga:xxxxxxx”)。
2.  **startDate:** 查询的开始日期(格式:“yyyy-MM-dd”或 NdaysAgo)。
3.  **结束日期:**查询的结束日期(格式:“yyyy-MM-dd”或 NdaysAgo)。
4.  **指标:**保存查询指标的表(例如 ga:用户或 ga:会话)
5.  **选项:**包含所有其他必要信息的对象(如维度、过滤器、分段、排序等)

**注意:* [*关于这些参数的完整参考，你可以在这里查阅文档。*](https://developers.google.com/analytics/devguides/reporting/core/v3/reference)

上面脚本中我们最应该关心的核心命令是:

```
return Analytics.Data.Ga.get(tableId, startDate, endDate, metrics, options);
```

这是实际查询来自 Google Analytics 的数据并返回报告的命令。代码片段的所有其他部分只是为了处理任何可能的错误。

## 用例

因此，在我们的用例中，我们将获取一些核心指标，如浏览量、平均。我们网站前 5 页的页面时间和跳出率，并写在我们的电子表格中。为此，您需要复制下面的脚本，并将其粘贴到应用程序脚本控制台。

**注意:您需要为工作表(不是电子表格)的名称、要查询的表 id(您可以通过选择“帐户”—“属性”—“视图”)以及查询的开始和结束日期指定您自己的值。*

粘贴了上面的脚本并添加了自己的参数后，选择**运行>运行函数> main** 。如果需要，进行身份验证，瞧！您的数据可以在电子表格中找到。在第一列中，我们看到了页面，接下来的三列分别是页面浏览量、页面平均停留时间和跳出率，正如我们在“指标”数组中声明的那样。

# 结论

在这篇小文章中，我们看到了一种从 Google Analytics 查询数据并使用应用程序脚本将其写入 Google 电子表格的简单方法。当然，应用程序脚本的能力是无限的，它能做的远不止获取简单的数据。把手弄脏，当你越来越习惯使用 Apps Script 时，你会意识到你能实现多少，你能自动化多少任务和报告。

页（page 的缩写）s:该指南已更新，以适应最近的变化。

> 如果您需要帮助或希望优化您的跟踪和分析设置[，请联系我们](https://www.amplifyd.co.uk/)！我们支持所有领域和规模的客户取得巨大成功，我们能够展示高质量的结果。我们的程序和经验确保您能够找到、跟踪、测试和分析所有重要的部分，并对您的业务绩效有一个全面的了解。