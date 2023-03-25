# 如何构建能够自我更新的闪亮应用

> 原文：<https://towardsdatascience.com/how-to-build-r-shiny-apps-that-update-themselves-6ce2d9606e90?source=collection_archive---------9----------------------->

R 生态系统现在是一个非常好的地方。特别是，R 中的基础设施允许人们通过闪亮的应用程序与数据进行交互，这为减少数据分析师的工作量提供了不可思议的潜力。

现在有可能构建闪亮的应用程序，可以定期更新自己，引入更新的数据，以便人们总是看到最新的分析。因此，通过一个预先开发 sprint，分析师可以将特定主题的持续分析工作量减少近 100%。

## 你需要什么

1.  一个项目，在该项目中，您定期以相同的格式交付报表，并且数据库中的所有数据都可用。
2.  R，R 降价和 R 闪亮技能
3.  访问包含闪亮服务器的运行 R 的 Linux 服务器，或者如果你真的幸运，访问 RStudio Connect 发布平台，这使得整个过程变得更加容易。

## **第一步:根据本地数据源构建你的应用**

使用本地数据源，以`.csv`、`.feather`文件、`.RData`文件甚至是`SQLLite`本地数据库的形式，编写一个闪亮的应用程序，使其成功部署在您的本地机器上。确保它按预期运行。

## **步骤 2:编写一个可以在服务器上运行的 ETL 脚本或文档**

用 R 编写一个脚本，从远程数据库中提取原始数据，将其转换成您闪亮的应用程序所需的形式。然后，您需要将这些数据写入某个地方，以便您的闪亮应用程序可以远程访问这些数据。选项包括:

*   在新表中写回源数据库(如果您有权限这样做)
*   写入托管您的闪亮服务器的 Linux 服务器的文件系统
*   如果你使用 RStudio Connect，在`/var/cache/data`写入它的缓存。

## **步骤 3:将您的脚本部署到您的 Linux 服务器或 RStudio Connect 上**

您需要在 Linux 服务器上的 R 中设置数据库连接。[这里的](https://db.rstudio.com/best-practices/drivers/)是一个很好的指南。然后，您需要编辑您的脚本来指向这些连接。

如果您将输出写入 Linux 文件系统，您将需要确保它位于 Shiny 服务器可以访问的目录中。

如果您在 RStudio Connect 中操作，您可以将您的脚本包装到一个 R Markdown 文档中，并发布它，包括源代码，这样每当文档被刷新时，代码就会执行，从而执行一个新的 ETL。

## **步骤 4:将你的闪亮应用部署到闪亮服务器或 RStudio Connect**

您需要编辑您的应用程序代码，以指向远程数据源，无论它是数据库还是文件系统中的某个地方。然后，您可以将应用程序文件加载到 Linux 服务器上的子目录`shiny-server`中。应用的 URL 将是 shiny 服务器的 URL，后跟子目录名称，例如`[https://my.shiny.server/myappname](https://my.shiny.server/myappname.)` [。](https://my.shiny.server/myappname.)

如果在 RStudio Connect 中工作，您只需正常发布并在 RStudio Connect 中自定义 URL。

## **第五步:设置数据自动刷新**

如果您的脚本位于您的 linux 服务器上，您可以使用各种调度工具来设置脚本在期望的时间间隔自动运行。`cron`是一种简单的方法。在您的 Linux 用户文件夹中找到您的`crontab`文件，并适当地编辑它。例如，要设置您的脚本在每个星期天的凌晨 2 点运行，编辑您的`crontab`文件，如下所示:

```
0 2 * * 0 Rscript /mypath/myscript
```

这将自动以所需的频率运行您的脚本，提取原始数据并用新数据覆盖您的应用程序访问的文件。

如果您使用的是 RStudio Connect，您可以为包含脚本的 R Markdown 文档设置，以便使用文档的“设置”菜单刷新到计划。

所以你有它。一个完全自动化的应用程序，可以自我刷新。想想如果你能设置好这个，你能节省多少时间。不客气

*你可以在这里* *了解更多关于 Shiny Server 和 RStudio Connect* [*。*](https://www.rstudio.com/products/shiny/shiny-server/)

最初我是一名纯粹的数学家，后来我成为了一名心理计量学家和数据科学家。我热衷于将所有这些学科的严谨性应用到复杂的人的问题上。我也是一个编码极客和日本 RPG 的超级粉丝。在[*LinkedIn*](https://www.linkedin.com/in/keith-mcnulty/)*或*[*Twitter*](https://twitter.com/dr_keithmcnulty)*上找我。*

![](img/44b6ffcfa0f505bdcf8a8aab3402098b.png)