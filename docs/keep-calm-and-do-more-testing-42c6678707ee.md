# 保持冷静，多做测试！

> 原文：<https://towardsdatascience.com/keep-calm-and-do-more-testing-42c6678707ee?source=collection_archive---------15----------------------->

作为数据科学家，我们都很熟悉流程、包或应用程序中断时会发生什么。我们饶有兴趣地深入其中，试图诊断意外错误发生在哪里。原始数据中是否发生了意想不到的事情？我们的代码没有预料到特定的输入排列吗？

由于令人沮丧的调试和重写，修复问题的过程可能从简单的调整到几天的揪头发和敲打头部。

最近，我开始接触 R 语言的测试，对它的直观性感到惊喜。当我们开发一个过程、包或应用程序时，在工作流程中增加测试并不需要花费太多的精力，但是在调试方面的好处是巨大的。

![](img/73500c9c2c4708b00fca4fdb0a8b99fe.png)

Debugging can be a nightmare — do yourself a favor and set up a testing workflow

## R 中的测试基础设施

R 中支持平滑、自动化测试的关键包是`testthat`。在您的 R 项目中，您可以通过一个简单的命令来初始化测试基础设施

```
devtools::use_testthat()
```

这个命令将在您的项目中创建一个名为`tests/testthat`的目录，您可以在其中放置执行测试的 R 脚本。

如果您的测试需要在本地设置环境变量，例如数据库凭证，您可以通过使用

```
usethis::edit_r_environ("project")
```

这将在 RStudio 中打开一个可编辑的文件，您可以在其中输入特定项目的变量名和值。

为了编写测试本身，您可以创建包含测试代码的 R 脚本，并将它们放在新的`tests/testthat`子目录中。这些脚本就像任何其他的 R 代码，除了它们使用方便的包装和为测试设计的`testthat`函数。关键函数是`testthat::test_that()` ，它有两个参数:

1.  `desc`，字符串形式的测试描述，如`"Test that I have all the cat data I expect"`
2.  `code`，代码执行您需要的计算，然后测试这些计算是否返回预期值，包含在`{}`中。

通常情况下,`code`会以某种形式的比较函数结束。`testthat`包含多个比较功能，例如:

*   `testthat::expect_equal()`测试两个参数是否相等。
*   `testthat::expect_gte()`测试第一个参数是否大于或等于第二个参数
*   `testthat::expect_s3_class()`测试第一个参数是否具有第二个参数中给出的 S3 类
*   `testthat::expect_true()`测试包含在参数中的语句评估为`TRUE`

一旦您编写了测试脚本，您就可以使用简单的命令`devtools::test()`自动运行所有的测试并查看性能摘要。

## 测试代码的示例

这里有几个非常简单的例子。假设我们有一个进程，它通过`mtcars`数据集中的`cyl`生成平均值`mpg`，并将其写入名为`mpg_by_cyl`的数据库表中。

现在，作为一名优秀的 R 公民，您非常了解您的`mtcars`数据集，并且您知道有三个`cyl`值，因此您会期望您的`mpg_by_cyl`数据库表有三行。因此，在您的测试脚本中，您可以从数据库中获取`mpg_by_cyl`,并且您可以编写以下简单的测试:

```
testthat::test_that("mpg_by_cyl has expected number of rows", {

    mpg_by_cyl %>%
      nrow() %>% 
      testthat::expect_equal(3)})
```

当您运行您的测试时，如果`mpg_by_cyl`确实有三行，您将从`devtools::test()`那里得到一个好的`Woot!`或者类似的鼓励话语。如果它没有像预期的那样返回，您将会被警告失败的条件，并得到一条像`Nobody`s perfect!`这样的安慰信息，然后您就会知道与`mpg_by_cyl`相关的事情没有像预期的那样发生。

这是另一个例子。假设您有一个脚本，它生成一个对象`date`，该对象包含一个表示格式`"%d %B %Y"`的最近日期的字符串，例如`"14 December 2017"`，您的流程将这个字符串写入某个数据库。您希望测试是否确实生成了有意义的日期并将其写入数据库。一种方法是检查字符串是否包含`" 20"`,这是您对任何最近日期的预期格式。因此，您可以让您的测试文件从它被写入的数据库中获取`date`，然后编写下面的测试:

```
testthat::test_that("date contains a recent date", {

  testthat::expect_true(grepl(" 20", date))})
```

这将评估写入数据库的内容是否包含预期的字符串，如果失败，您就知道这个过程出错了。(当然，这并不能完全验证预期日期已被写入，但数据科学家将确定如何编写特定的测试，以提供他们所需的确定程度)。

就个人而言，我发现 R 中的测试基础设施非常直观，我建议您养成为所有的包、过程和应用程序编写测试的习惯。你可以以后再谢我！

*关于 R 中测试的更多信息，查看 Hadley Wickham 的开卷* [*这里*](http://r-pkgs.had.co.nz/tests.html) *。*

最初我是一名纯粹的数学家，后来我成为了一名心理计量学家和数据科学家。我热衷于将所有这些学科的严谨性应用到复杂的人的问题上。我也是一个编码极客和日本 RPG 的超级粉丝。在[*LinkedIn*](https://www.linkedin.com/in/keith-mcnulty/)*或*[*Twitter*](https://twitter.com/dr_keithmcnulty)*上找我。*