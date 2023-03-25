# 让 R 和 D3.js 在 R markdown 中正常运行

> 原文：<https://towardsdatascience.com/getting-r-and-d3-js-to-play-nicely-in-r-markdown-270e302a52d3?source=collection_archive---------0----------------------->

![](img/b78e950531d12bb3bd7d725f20429f78.png)

因为已经有很多优秀的资源是由更有资格的人制作的，所以这并不是 JavaScript、D3.js、R 或 R markdown 的全面教程。相反，它的目的是展示在一个文件中使用所有这些文件的经验。

为了使本教程简单，让我们假设数据已经完全科学化，剩下的就是创建 D3 可视化。请看一下 R 附带的原始样本数据集

```
head(cars)##   speed dist
## 1     4    2
## 2     4   10
## 3     7    4
## 4     7   22
## 5     8   16
## 6     9   10
```

在语言之间传递数据不像引用包含它们的变量的名称那么简单。如果您尝试在上面的 R 块后面添加 JavaScript 代码`console.log(cars);`，它将返回一个`ReferenceError`，因为`cars`尚未在 JavaScript 中定义。

您可以将数据写入一个新文件，然后使用 D3 中适当的加载方法读回该文件。但是，可以将数据直接传递给。Rmd 文件。

```
cat(
  paste(
  '<script>
    var data = ',cars,';
  </script>'
  , sep="")
)## 
## <script>
##   var data = c(4, 4, 7, ... 24, 24, 25);
## </script>
## <script>
##   var data = c(2, 10, 4, ... 93, 120, 85);
## </script>
```

上面的代码确实会将数据传递到我们的 JavaScript 空间；然而，我们给自己制造了两个新问题:

*   数据中的每一列都作为 R 向量被传递，JavaScript 会将其解释为对某个函数`c()`的调用，每个值都作为参数被传递。
*   这些列被逐个传递给 JavaScript 变量`data`，用`dist`列中的值覆盖`speed`列中的值。

要同时解决这两个问题，可以利用 D3 的假设，即它处理的数据是 JSON 格式的。r 的`jsonlite`库是轻量级的，有一个简单的`toJSON()`方法，非常适合我们的目的。

```
library("jsonlite")
cat(
  paste(
  '<script>
    var data = ',toJSON(cars),';
  </script>'
  , sep="")
)## <script>
##   var data = [{"speed":4,"dist":2},
##               {"speed":4,"dist":10},
##               {"speed":7,"dist":4},
##                 ...
##               {"speed":24,"dist":93},
##               {"speed":24,"dist":120},
##               {"speed":25,"dist":85}];
## </script>
```

**** *您必须在代码块顶部的花括号中包含 results="asis "选项，以便通过***** 传递数据

现在 JavaScript 有了数据，而且是它可以处理的格式。是时候开始 D3ing 了！

有两种方法可以加载 D3 库。在可视化之前的任何时候，将`<script src="https://d3js.org/d3.v4.min.js"></script>`直接包含在您的降价文件中。或者在与您相同的目录中创建一个. html 文件，其中只包含相同的脚本标记。Rmd 文件和[包含它](http://rmarkdown.rstudio.com/html_document_format.html#includes)

你现在可以在 R markdown 中自由地想象 D3.js，直到你满意为止。只需用 JavaScript 代码块添加代码，直接在 markdown 中的脚本标记之间，或者在用脚本标记链接的单独文件中。

我应该加一句警告。如果你是那种只从 https://bl.ocks.org T2 复制/粘贴的人，这没什么不好意思的——我个人不知道有谁使用 D3 不这么做——但是你必须记住他们的数据总是通过 D3 的加载方法从外部来源读入的。您必须删除这些代码行，并在 D3 的`.data()`方法中代入您的数据变量。此外，一定要检查 D3 代码，更新对列名的所有数据引用。

[查看 GitHub 上的代码](https://github.com/becausealice2/D3-in-Rmd)