# 如何用 R 编写整洁的 SQL 查询

> 原文：<https://towardsdatascience.com/how-to-write-tidy-sql-queries-in-r-d6d6b2a3e17?source=collection_archive---------8----------------------->

如今，我们大多数人都必须与数据库进行交互，而 SQL 是目前最常用的语言。然而，在 R 中使用 SQL 可能会很麻烦。如果您的查询很复杂，您必须将它们编码为文本字符串，这很容易出错，并且面临格式方面的挑战。此外，当您想要构建包含变量的 SQL 查询时，您必须进行替换或粘贴，这有点麻烦。

理想情况下，您希望能够使用 tidy 原则处理您的数据库，利用 tidyverse 的奇迹，最好不要先将整个数据库下载到您的会话中。这就是`dbplyr`的魔力所在。

`dbplyr`充当 SQL 翻译器，允许你使用`tidyverse`来处理数据库。所以现在你可以尽情宣泄了。如果你还没有用过这个，我现在就开始用。编写整洁的数据库查询有许多优点。当你过一段时间再回到你的工作时，你可以更容易地理解你的工作，你可以更清楚地评论，并且它也迫使你去思考你的查询的最有效的结构。

## 在 dbplyr 中工作的基本原则

要在`dbplyr`中工作，您需要像在 R 会话中一样设置数据库连接，我们称之为`myconn`。您将使用`dbplyr::in_schema()`在 R 会话中设置数据库对象。这需要两个参数:第一，您希望在数据库连接中访问的模式，第二，您在该模式中感兴趣的表。下面是一个如何设置的示例:

```
catdata <- dplyr::tbl(
  myconn,
  dbplyr::in_schema("ANIMAL_ANALYSTS", "CAT_TABLE")
)
```

现在`catdata`是一个数据库对象。上面的命令连接到数据库并下载关于字段、数据类型等的最少信息。—足以允许操纵对象，而无需物理下载数据。

现在，您可以像操作 r 中的其他表一样操作`catdata`。例如:

```
weight_by_age <- catdata %>%
  dplyr::group_by(AGE) %>%
  dplyr::summarise(AVG_WT = mean(WT, na.rm = TRUE))
```

所有这些操作都是通过在后台将您的代码翻译成 SQL，而无需实际下载数据。由于数据下载通常是最耗时的步骤，这允许您在提取数据之前考虑您希望在服务器上完成多少工作。

准备拉数据的时候，用`dplyr::collect()`就可以了。这将把后台编译的 SQL 查询发送到数据库并执行它。例如:

```
weight_by_age %>%
  dplyr::rename(`Age of Cat` = AGE,
                `Average Weight` = AVG_WT) %>%
  dplyr::collect() 
```

## dbplyr 中更复杂的 SQL 操作

`dbplyr`非常灵活，我还没有发现我不能用`dbplyr`重写 tidy 的 SQL 查询。

连接通过在数据库对象上使用`dplyr`的连接函数来工作，例如:

```
fullcatdata <- dplyr::left_join(
  catregistrationdetails, 
  catdata, 
  by = "SERIAL_NO"
) %>%
  dplyr::left_join(
    cathealthrecord, 
    by = "SERIAL_NO"
)
```

可以使用`dplyr::mutate()`将新列添加到数据中，甚至可以用于更复杂的连接。例如，如果您的卡特彼勒序列号在一个表中有“CAT-”开头，但在另一个表中没有:

```
 fullcatdata <- catregistrationdetails %>%
  dplyr::mutate(SERIAL_NO = paste0("CAT-", SERIAL_NO)) %>%
  dplyr::left_join(catdata, by = "SERIAL_NO")
```

`dbplyr`巧妙地将 R 函数翻译成 SQL 等价物。您可以使用`dbplyr::translate_sql()`功能来查看它的功能。例如:

```
dbplyr::translate_sql(substr(NAME, -3, -1))
<SQL> substr("NAME", -3, 3)
```

我发现`dbplyr`也让我在反应式环境中更容易编码。如果你要建立一个闪亮的应用程序，根据输入计算猫的平均体重`input$age`:

```
weight <- reactive({
  catdata %>%
  dplyr::filter(AGE == input$age) %>%
  dplyr::select(WT) %>%
  mean(na.rm = TRUE) %>%
  dplyr::collect()
})
```

这些只是`dbplyr`帮助您在 SQL 中更整洁地工作的许多方式中的一部分。我强烈推荐。

*关于* `*dbplyr*` *的更多信息，请点击* [*这里*](https://dbplyr.tidyverse.org/) *。*

最初我是一名纯粹的数学家，后来我成为了一名心理计量学家和数据科学家。我热衷于将所有这些学科的严谨性应用到复杂的人的问题上。我也是一个编码极客和日本 RPG 的超级粉丝。在[*LinkedIn*](https://www.linkedin.com/in/keith-mcnulty/)*或*[*Twitter*](https://twitter.com/dr_keithmcnulty)*上找我。*

![](img/50704f5353ad62709f15c575a4dba836.png)