# 预处理大型数据集:50 万多个实例的在线零售数据

> 原文：<https://towardsdatascience.com/preprocessing-large-datasets-online-retail-data-with-500k-instances-3f24141f511?source=collection_archive---------12----------------------->

几个月前，我有机会参与一个拥有超过 500，00 0 行的庞大数据集的项目！老实说，这是我第一次不得不处理数量惊人的数据，但我认为玩它并探索新的数据挖掘技术会很有趣。在这篇文章中，我将解释我是如何处理这个问题的，并最终得到了一个干净的、便于使用的数据集。还提供了 R 代码。

正在讨论的数据集在 *UCI 机器学习库*的[这里](http://archive.ics.uci.edu/ml/datasets/online+retail)可用。这是一个交易数据集，包含 2010 年 1 月 12 日和 2011 年 9 月 12 日之间英国注册的无店铺在线零售的所有交易。该公司主要销售独特的全场合礼品；这家公司的许多客户都是批发商。属性信息可以在提供的链接中找到。我的最终目标是对这些数据进行购物篮分析，并找出关联规则。然而，第一步是清理数据。

我将数据集保存为一个. csv 文件，并简要查看了所有行。我意识到，由于维度问题，我可能无法注意到最重要的模式和细节。然而，我注意到一些单元格是空的，那些是丢失的值。为了方便起见，它们被重新编码为 NA。我还发现了丢失的值在哪里，以及有多少。

```
dataset = read.csv("OnlineRetail.csv", na.strings = c("","NA"))attach(dataset)#checking if there any missing values, where are they missing, and how many of them are missingany(is.na(dataset))[1] **TRUE**apply(dataset, 2, function(x) any(is.na(x)))InvoiceNo StockCode Description Quantity InvoiceDate UnitPrice CustomerID Country**FALSE** **FALSE** **TRUE** **FALSE** **FALSE** **FALSE** **TRUE** **FALSE**sum(is.na(CustomerID))[1] 135080sum(is.na(Description))[1] 1454
```

*InvoiceNo* 为整数类型；但是，最好是字符类型，这样我们就可以应用字符串函数。在 *InvoiceNo* 和 *Description* 中的 *trim* 功能都删除了前导和尾随空格。

```
dataset$InvoiceNo = as.character(InvoiceNo)
trim = function (x) gsub("^\\s+|\\s+$", "", x)
dataset$InvoiceNo = trim(InvoiceNo)
dataset$Description = trim(as.character(Description))
```

以 C 开头的发票号码实际上是取消的，所以我们可能想去掉它们。我引入了一个名为 *is_C* 的函数，它检查一个字符串是否以 C 开头。我们对原始数据进行子集化，得到 *dataset2* (没有取消)和 *dataset3* (没有空的项目描述)。我们可以看到原始数据集有 541 909 个观察值，现在我们在*数据集 3* 中有 531 167 个观察值。我们刚刚剔除了将近 10 000 个无用的观察值。

```
is_C = function (x) startsWith(x,"C")
dataset2 = dataset[which(!is_C(dataset$InvoiceNo)),] #subsetting
dataset3 = subset(dataset2,!is.na(dataset2$Description)) #subsetting
```

接下来，我又看了一遍这些行，发现许多项目描述没有任何意义。例如，我看到了像“？”这样的符号以及损坏、砸碎、丢失等词语。扔掉这些不需要的东西是个好主意。我想出了一系列有趣的流行语:

```
buzzwords = c("WRONG","LOST", "CRUSHED", "SMASHED", "DAMAGED", "FOUND", "THROWN", "MISSING", "AWAY", "\\?", "CHECK", "POSTAGE", "MANUAL", "CHARGES", "AMAZON", "FEE", "FAULT", "SALES", "ADJUST", "COUNTED", "LABEL", "INCORRECT", "SOLD", "BROKEN", "BARCODE", "CRACKED", "RETURNED", "MAILOUT", "DELIVERY", "MIX UP", "MOULDY", "PUT ASIDE", "ERROR", "DESTROYED", "RUSTY")
```

如果任何项目包含这些单词中的一个，它将被删除。以下功能*是不合需要的*测试一个项目是否“不合需要”。

```
library(stringr)   #function str_detectisUndesirable = function(x){c = **FALSE**   #assume that the string is undesirable (FALSE), and perhaps switch to desirable (TRUE)for (i **in** 1:(length(buzzwords))){
  c = c || ifelse(str_detect(toupper(x),buzzwords[i]),**TRUE**,**FALSE**)
}#now we know whether or not the string is undesirablereturn(c)}
```

现在我们可以子集化 *dataset3* 使得*数量*大于 0(否则没有意义)，项目名称为“合意”。我们得到*数据集 4* ，最后得到*数据集 5* 。 *InvoiceDate* 被转换为 *POSIXct* 对象，订单的确切时间被排除。*描述*列被分解。

```
dataset4 = subset(dataset3, dataset3$Quantity > 0)dataset5 = dataset4[which(!isUndesirable2(as.character(dataset4$Description))),]Time = format(as.POSIXct(strptime(dataset5$InvoiceDate,"%Y-%m-%d %H:%M",tz="")) ,format = "%H:%M:%S")dataset5$InvoiceDate = as.Date(dataset5$InvoiceDate)
dataset5$Description = as.factor(dataset5$Description)
```

就是这样！现在我们有了包含 530 653 个观察值的*数据集 5* ，准备用于购物篮分析。请注意，该数据集在 *CustomerID* 列中仍然有缺失值。然而，这无关紧要，我们为什么要关心客户的 ID 呢？发票号对于发现关联规则和进一步分析更为重要。然而，如果我们想在另一个分析中使用这个数据，我们可以用不同的方式对它进行预处理。

完整的 R 代码，请访问[我的 GitHub 简介。](https://github.com/dinajankovic/Market-Basket-Analysis-Online-Retail)