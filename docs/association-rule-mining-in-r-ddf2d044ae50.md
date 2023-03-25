# R 中的关联规则挖掘

> 原文：<https://towardsdatascience.com/association-rule-mining-in-r-ddf2d044ae50?source=collection_archive---------1----------------------->

![](img/3f6bad6da227da51ac3cc5705aec39cf.png)

关联规则挖掘(也称为关联规则学习)是一种常用技术，用于发现许多变量之间的关联。杂货店、电子商务网站和任何拥有大型事务数据库的人都经常使用它。我们在日常生活中遇到的一个最常见的例子是——当你在他们的网站上订购一些东西时，亚马逊知道你还想买什么。同样的想法也适用于 Spotify 他们知道你下一首想听的歌。所有这些都在某种程度上结合了数据挖掘概念和关联规则挖掘算法。

M 市场篮子分析类似于 ARM。购物篮分析是一种建模技术，其理论基础是，如果你购买了某一组商品，你就更有可能(或更不可能)购买另一组商品。例如，如果你在一家英国酒吧，你买了一品脱啤酒而没有买酒吧餐，你比没有买啤酒的人更有可能同时买薯片。

> *基于强规则的概念，Rakesh Agrawal、Tomasz Imieliński 和 Arun Swami 引入了关联规则，用于在超市销售点(POS)系统记录的大规模交易数据中发现产品之间的规律性。*

**本文解释了关联规则挖掘的概念以及如何在 R** 中使用这种技术

为了在 R 中执行关联规则挖掘，我们使用 R 中的`arules`和`arulesViz`包。

> [*Michael hassler 等人*](https://cran.r-project.org/web/packages/arules/vignettes/arules.pdf) *创作并维护了两个非常有用的关联规则挖掘 R 包:arules 包和 arulesViz 包。*

如果您的系统中没有安装这些软件包，请使用以下命令来安装它们。

```
> install.packages("arules")
> install.packages("arulesViz")
```

# 数据

我使用的是与`arules`软件包捆绑在一起的 AdultUCI 数据集。

```
> data(“Groceries”)
```

让我们先检查食品杂货数据。

```
> class(Groceries)
      [1] "transactions"
      attr(,"package")
      [1] "arules"
```

这是一个事务性数据集。

```
> inspect(head(Groceries, 2))
          items                                                   
      [1] {citrus fruit,semi-finished bread,margarine,ready soups}
      [2] {tropical fruit,yogurt,coffee}
```

从上面的输出中可以观察到前两个事务以及每个事务中涉及的项目。

# 生成规则

有三个参数控制要生成的规则数量*，即。* **支持和信心**。另一个参数 **Lift** 是使用支持度和置信度生成的，并且是过滤生成的规则的主要参数之一。

*   ***支持*** 是项集在数据集中出现频率的指示。只考虑上面输出中的两个事务。项目*柑橘类水果*的支持度为 1/2，因为它只出现在两个交易中的 1 个中。
*   ***置信度*** 是规则被发现为真的频率的指示。在生成规则之后，我们将讨论更多关于置信度的内容。

让我们使用*先验*算法找出规则。

```
> grocery_rules <- apriori(Groceries, parameter = list(support = 0.01, confidence = 0.5))AprioriParameter specification:
      confidence minval smax arem  aval originalSupport maxtime support minlen maxlen target   ext
        0.5    0.1    1 none FALSE            TRUE       5    0.01      1     10  rules FALSEAlgorithmic control:
      filter tree heap memopt load sort verbose
      0.1 TRUE TRUE  FALSE TRUE    2    TRUEAbsolute minimum support count: 98set item appearances ...[0 item(s)] done [0.00s].
      set transactions ...[169 item(s), 9835 transaction(s)] done [0.00s].
      sorting and recoding items ... [88 item(s)] done [0.00s].
      creating transaction tree ... done [0.00s].
      checking subsets of size 1 2 3 4 done [0.00s].
      writing ... [15 rule(s)] done [0.00s].
      creating S4 object  ... done [0.00s].
```

Apriori 算法用给定的约束生成了 15 条规则。让我们深入到输出的参数规范部分。

*   ***minval*** 是一个项集要成为规则的一部分所应满足的支持度的最小值。
*   ***smax*** 是一个项集的最大支持值。
*   ***arem*** 是一个附加的规则评估参数。在上面的代码中，我们使用支持度和置信度限制了规则的数量。在函数中使用 **arem** 参数有几种其他的方法来约束规则，我们将在本文后面讨论更多。
*   ***aval*** 是表示是否返回用 arem 选择的附加规则评估度量的逻辑。
*   ***原始支持*** 传统的支持度值在计算支持度时只考虑 LHS 和 RHS 两个项目。如果您想仅使用 LHS 项目进行计算，则需要将其设置为 FALSE。
*   ***maxtime*** 是允许检查子集的最大时间量。
*   ***minlen*** 是规则中要求的最小项数。
*   ***maxlen*** 是规则中可以出现的最大项数。

```
> inspect(head(sort(rules, by = "confidence"), 3))
      lhs                                 rhs                support    confidence lift     count
      [1] {citrus fruit,root vegetables}   => {other vegetables} 0.01037112 0.5862069  3.029608 102  
      [2] {tropical fruit,root vegetables} => {other vegetables} 0.01230300 0.5845411  3.020999 121  
      [3] {curd,yogurt}                    => {whole milk}       0.01006609 0.5823529  2.279125  99
```

上面显示了按置信度排序的前 3 条规则。

# 限制生成的规则数量

在许多情况下，您希望限制生成的规则数量。例如，您可以在回归/分类中使用关联规则作为预测器。您可以生成规则，将规则的右侧作为您的响应，并将生成的规则用作建模特征。在这种情况下，您不希望使用所有生成的规则作为预测器，因为许多规则实际上是更大规则的子集，因此您希望消除它们。下面的代码片段显示了如何生成其 RHS 是预定义的规则。

```
wholemilk_rules <- **apriori**(data=Groceries, parameter=**list** (supp=0.001,conf = 0.08), appearance = **list** (rhs="whole milk"))# The above code shows what products are bought before buying "whole milk" and will generate rules that lead to buying "whole milk".
```

您可以通过调整一些参数来限制规则的数量。虽然参数调整取决于您正在处理的数据类型，但最常见的方法包括更改支持度、置信度和其他参数，如 minlen、maxlen 等。

```
> grocery_rules_increased_support <- apriori(Groceries, parameter = list(support = 0.02, confidence = 0.5))# This generates only one rule in the output.
```

如果你想让**变得更强**规则，你必须增加信心。如果您想要**更长的**规则，请增加 maxlen 参数。如果您想消除**较短的**嵌线，减小 minlen 参数。

有时，您可能想找到包含最大数量项目的规则，并删除作为较长规则子集的较短规则。下面的代码删除了这些多余的规则。

```
> subsets <- which(colSums(is.subset(grocery_rules, groery_rules)) > 1)
> grocery_rules <- grocery_rules[-subsets]
```

让我们看看之前描述过的 **arem** 参数。规则生成后，根据参数 *arem* 的值进一步评估。 *arem* 参数取值如下: *none* ， *diff* ， *quot* ， *aimp* ， *info* ， *chi2* 。

```
# This gives more than 1,500,000 rules
> rules <- apriori(Groceries, parameter = list(supp = 0.0001, conf = 0.5))# This gives 982,000 rules.
> rules_chi2 <- apriori(Groceries, parameter = list(supp = 0.0001, conf = 0.5, arem = "chi2"))
```

# 将数据帧转换成事务数据

使用与`arules`包捆绑的`AdultUCI`数据集。

```
> data("AdultUCI")
> class(AdultUCI)
      "data.frame"
```

当您查看 AdultUCI 数据帧的结构时，您会发现有几列是数字。事务性数据集的每个事务都包含该事务中涉及的项的列表。当我们将数据帧转换成事务性数据集时，该数据帧的每一行都将成为一个事务。每一列都将成为一个项目。但是，如果列的值是数字，则不能使用它，因为该列可以接受无限个值。因此，在将 dataframe 转换为事务性数据集之前，我们必须确保将每一列转换为因子或逻辑，以确保该列仅从固定集中取值。

```
> str(AdultUCI)
'data.frame':	48842 obs. of  15 variables:
 $ age           : int  39 50 38 53 28 37 49 52 31 42 ...
 $ workclass     : Factor w/ 8 levels "Federal-gov",..: 7 6 4 4 4 4 
 $ fnlwgt        : int  77516 83311 215646 234721 338409 284582 
 $ education     : Ord.factor w/ 16 levels "Preschool"<"1st-4th"<..: 
 $ education-num : int  13 13 9 7 13 14 5 9 14 13 ...
 $ marital-status: Factor w/ 7 levels "Divorced","Married-AF-Spouse"
 $ occupation    : Factor w/ 14 levels "Adm-clerical",..: 1 4 6 6 10 
 $ relationship  : Factor w/ 6 levels "Husband","Not-in-family",..: 
 $ race          : Factor w/ 5 levels "Amer-Indian-Eskimo",..: 5 5 5 
 $ sex           : Factor w/ 2 levels "Female","Male": 2 2 2 2 1 1 1 
 $ capital-gain  : int  2174 0 0 0 0 0 0 0 14084 5178 ...
 $ capital-loss  : int  0 0 0 0 0 0 0 0 0 0 ...
 $ hours-per-week: int  40 13 40 40 40 40 16 45 50 40 ...
 $ native-country: Factor w/ 41 levels "Cambodia","Canada",..: 39 39 
 $ income        : Ord.factor w/ 2 levels "small"<"large": 1 1 1 1 1 
```

在`AdultUCI`数据帧中，第 1、3、5、11、12、13 列是整数。所以把每一列都转换成因子。

```
> AdultUCI <- lapply(AdultUCI, function(x){as.factor(x)})> str(AdultUCI)
List of 15
 $ age           : Factor w/ 74 levels "17","18","19",..: 23 34 22 
 $ workclass     : Factor w/ 8 levels "Federal-gov",..: 7 6 4 4 4 4 
 $ fnlwgt        : Factor w/ 28523 levels "12285","13492",..: 3462 
 $ education     : Ord.factor w/ 16 levels "Preschool"<"1st-4th"<..: 
 $ education-num : Factor w/ 16 levels "1","2","3","4",..: 13 13 9 7 
 $ marital-status: Factor w/ 7 levels "Divorced","Married-AF-
 $ occupation    : Factor w/ 14 levels "Adm-clerical",..: 1 4 6 6 10 
 $ relationship  : Factor w/ 6 levels "Husband","Not-in-family",..: 
 $ race          : Factor w/ 5 levels "Amer-Indian-Eskimo",..: 5 5 5 
 $ sex           : Factor w/ 2 levels "Female","Male": 2 2 2 2 1 1 
 $ capital-gain  : Factor w/ 123 levels "0","114","401",..: 28 1 1 1
 $ capital-loss  : Factor w/ 99 levels "0","155","213",..: 1 1 1 1 1
 $ hours-per-week: Factor w/ 96 levels "1","2","3","4",..: 40 13 40
 $ native-country: Factor w/ 41 levels "Cambodia","Canada",..: 39 39
 $ income        : Ord.factor w/ 2 levels "small"<"large": 1 1 1 1 1 
```

现在`AdultUCI` dataframe 可以使用下面的代码片段转换成事务性数据集。

```
> transactional_data <- as(AdultUCI, "transactions")
```

这篇关于关联规则挖掘的文章到此结束。