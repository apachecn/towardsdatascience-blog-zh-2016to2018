# 用 R 编程的 10 个不太直观的地方

> 原文：<https://towardsdatascience.com/10-not-so-intuitive-things-about-programming-with-r-a4d9d120c42c?source=collection_archive---------10----------------------->

![](img/d128167ba32bd900b69e338b70daa824.png)

# **为什么数据科学要用 R**

![](img/9316fa318e74919d0e655b11886f1a72.png)

传统上，r 被认为是统计学家和某些学术研究者的首选计算语言，因为有大量可用的统计软件包和庞大的社区。可以说，R 的最大优势在于数据操作和统计分析任务的容易实现。此外，ggplot2 之类的包可以生成高质量的可视化效果，使数据分析和报告变得令人愉快，而没有其他脚本语言的陡峭的学习曲线。

也就是说，R 的行为也可能相当古怪。作为三藩市大学 MSDS 项目的一部分，我最近参加了一门关于 R 的探索性数据分析的课程，并以惨痛的代价学会了其中的一些怪癖。我在下面列出了十大怪癖，可能不是很直观。在分析数据和调试代码时，意识到这些怪癖可以节省大量的时间和挫折。

## **1)加载包时使用 require()和 library()**

如上所述，R 的主要优点是有大量的包，可以很容易地检索并加载到活动的 R 会话中。 **require()** 和 **library()** 函数都允许我们将已安装的包加载到活动内存中，但是当加载有问题的包抛出异常时，它们的输出会有细微的差别。当 **library()** 抛出一个 ***错误*** 当包不可加载到活动内存时， **require()** 抛出一个 ***警告*** 并返回一个基于包可用性的逻辑响应。

```
> library(randomForest)
**Error** in library(randomForest) : 
  there is no package called ‘randomForest’

> b <- require(randomForest)
Loading required package: randomForest
**Warning message:**
In library(package, lib.loc = lib.loc, character.only = TRUE, logical.return = TRUE,  :
  there is no package called ‘randomForest’
> print(b)
[1] **FALSE**
```

**require()** 通常可以作为条件检查，查看包是否存在并安装。

## 2)R 中的 NA 与 NULL

`NA`和`NULL`之间的差异可能是一个混淆点，因为人们可能会直觉地认为它们都表示缺失/未定义的值。然而，r 以不同的方式处理这两个保留字。`NA`是一个长度为 1 的 ***逻辑常数，其中包含一个 ***缺失值*** 指示符，而`NULL`表示 ***空对象。*** 通过下面的代码可以直观的解释上述语句:***

```
> v <- c(1,2,3,NA)
> v
[1]  1  2  3 NA> w <- c(1,2,3,NULL)
> w
[1] 1 2 3
```

上面的代码帮助我们理解了`NA`和`NULL`之间的一个关键区别。`NULL`是自身类型的对象，不能强制为任何其他类型。因此，当我们试图将它包含在矢量形式中时，它被忽略了。另一方面，`NA`可以被强制为各种类型，如 **NA_interger、NA_character** 等。但是，如果我们创建一个列表(因为列表可以将不同的类型存储在一起)，那么它们都可以作为一个元素包含在内。

```
> w <- list(1,2,3,NULL)
> w
[[1]]
[1] 1

[[2]]
[1] 2

[[3]]
[1] 3

[[4]]
NULL
```

## 3)使用浮点值的子设置和逻辑向量的循环

要访问向量/矩阵/数组中的特定值，我们可以使用索引值或逻辑向量进行子设置。值得注意的一点是，我们可以使用 floats 和 integer 来调用特定的值，如下所示:

```
> w <- c(1,2,3,4,5)
> w[3.5]
[1] 3
> w[-4.9]
[1] 1 2 3 5
```

我们可以清楚地看到，无论符号如何，在子设置时，指数的浮动部分被忽略。所以 **3.5** 给我们向量中的第 3 个元素，而 **-4.9** 忽略第 4 个元素。在使用逻辑向量进行子集设置时，另一件要注意的事情是向量长度。如果子集向量的长度较小，R 不会抛出错误，而是回收较小的向量，使其具有相同的长度。

```
> w <- c(1,2,3,4,5)
> x <- c(T,F)
> x
[1]  TRUE FALSE
> w[x]
[1] 1 3 5
```

我们可以看到向量`x` 的长度为 2，向量`w`的长度为 5。然而，当我们使用`x`子集时，它被循环到 **c(T，F，T，F，T)** 并且我们在`w`中得到所有的替代值。

![](img/27371056087d925f269b829e28f8f037.png)

## 4)列表中的类型保留与简化

列表能够将不同类型的数据存储在一起，这非常有用。**类型保留**和**类型简化**是两个有趣的概念，当我们试图使用(`[ ]`或`[[ ]]`)提取列表元素时，它们就发挥作用了

```
> a <- list(1,2,3,c(4,5,6))
# a has 1,2,3 and c(4,5,6) as its elements
> a[1]
[[1]]
[1] 1
#Element one as list:Type preservation> a[4]
[[1]]
[1] 4 5 6
#Element 4 as list:Type preservation> a[[4]]
[1] 4 5 6
#Element 4 as vector, type simplification> a[[4]][1]
[1] 4
#First element of 4th element of a, type simplification> a[4][1]
[[1]]
[1] 4 5 6
#[] Outputs the whole vector as we did not simplify the data type to vector from list.
```

从上面的例子中，我们可以看到`[]`有助于 T **类型保存**，输出也是一个列表，类似于初始列表。另一方面，`[[]]`对**进行了类型简化**，并给出了最简单的潜在数据类型。另外，当我们试图访问一个向量的元素时，`[[]]`是很重要的，这个向量包含在一个列表中，如代码的最后一行所示。

## 5)访问矩阵中的列并使用 drop

矩阵运算在实现很多机器学习模型的矢量化和速度方面发挥了很大的作用。很多时候，我们需要对矩阵进行子集化，以访问矩阵中的特定列并执行某些操作。但是，当您尝试从矩阵中提取单个列时，会发生如下所示的奇怪情况:

```
> a <- matrix(c(1:9),ncol=3,nrow=3)
> a
     [,1] [,2] [,3]
[1,]    1    4    7
[2,]    2    5    8
[3,]    3    6    9
> a[,1]
[1] 1 2 3
> class(a[,1])
[1] "integer"
#When you extract a single column, it is converted to a vector> a[,1,drop=F]
     [,1]
[1,]    1
[2,]    2
[3,]    3
> class(a[,1,drop=F])
[1] "matrix"
# drop=F helps us retain the matrix form
```

当我们试图访问一个单独的列时，R 把它转换成向量形式。这有时可能是不必要的，因为您的下游代码可能会由于这种转换而受到影响(*在 R* 中从头实现 k- means 算法时学到了这一点)。为了避免这种情况，我们必须在访问单个列时使用`drop=F`作为参数。

![](img/05854e5930e6cef27d40b0dd8840d144.png)

## 6)访问向量和列表中的越界元素

当我们试图访问一个越界的元素时，这是我们在 R 中能观察到的最不寻常的事情之一。看看下面的代码:

```
> a <- c(1:5)
> a
[1] 1 2 3 4 5
# a has 5 elements as shown above> a[7]
[1] NA
#When we try to access 7th element, we get NA instead of an error> b <- list(c(1,2,3,4))
> b
[[1]]
[1] 1 2 3 4
#b has 1 element which is a vector of 4 elements
> b[2]
[[1]]
NULL
#When we try to access 2nd element, we get NULL instead of an error
```

当我们从一个向量或列表中访问一个越界索引时，我们得到的不是一个错误，而是作为输出的`NA`和`NULL`，这可能会引起关注，有时会使测试/调试变得困难。

![](img/84b694aae966ecd5e7432862bb6b6b29.png)

## 7) if {} else{} vs ifelse()

当我们有了一个向量并想要检查给定的条件时，`if {} else{}`只对向量的第一个元素起作用，并给出一个警告。然而，如果我们想要访问向量中每个元素的条件，我们必须使用`ifelse()`。它比较向量中的每个值并返回一个向量。假设，我们有一个包含 5 个元素的向量，我们想检查它们是偶数还是奇数

```
> a <- c(5:10)
> a
[1]  5  6  7  8  9 10
> if(a%%2==0){x <- "Even"} else{x <- "Odd"}
Warning message:
In if (a%%2 == 0) { :
  the condition has length > 1 and only the first element will be used
> x
[1] "Odd"> y <- ifelse(a%%2,"Even","Odd")
> y
[1] "Even" "Odd"  "Even" "Odd"  "Even" "Odd"
```

我们可以看到`if{} else{}`只比较了`a`的第一个元素，而`ifelse()`达到了要求的结果。

## 8)调用参数数量不足的函数

r 允许调用参数数量不足的函数，只要缺少的参数没有被调用。这是需要记住的非常重要的一点，并且不同于其他编程语言。如果我们明确地希望所有的参数都出现在函数调用中，我们可以在其他选项中使用`force()`。

```
> f <- function(x,y){
           print(x)}
> f(2)
[1] 2
#Function call works with one argument as y is not used.> f(2,3)
[1] 2
#Calling with both arguments> f()
Error in print(x) : argument "x" is missing, with no default
#Since x is called inside function and is missing, we get error#Explicitly checking for both x and y using force()
> f <- function(x,y){force(x,y); print(x)}
> f(2,3)
[1] 2
> f(2)
Error in force(y) : argument "y" is missing, with no default
```

`force()`检查 x 和 y 是否都存在，当缺失时抛出错误。

## 9)功能屏蔽和使用:

很多时候，不同的包有相同名称但不同功能的函数。如果我们想使用特定包中的特定函数，我们可能需要特别指定它。在没有特定调用的情况下，最近加载的包中的函数会屏蔽所有其他同名函数。例如，库`chron`和`tseries`都以`is.weekend()`为其功能。

```
library(chron)
library(tseries)
    ‘tseries’ version: 0.10-45

    ‘tseries’ is a package for time series analysis and computational finance.

    See ‘library(help="tseries")’ for details.

Attaching package: ‘tseries’

**The following object is masked from ‘package:chron’:**

    is.weekendis.weekend(x)
```

当我们调用`is.weekend()`时，来自`tseries`包的函数被使用，因为它是最近加载的包。如果我们特别想使用`chron`，我们需要做以下事情:

```
chron::is.weekend(x)
```

`::`帮助我们指定使用哪个包。`::`也帮助我们不用加载包就可以使用一个函数。然而，每当我们想要使用这个包中的函数时，我们需要使用`::`来专门调用这个函数。

## 10)调用函数时的参数匹配

调用函数时，参数按照以下顺序进行匹配:a)完全名称匹配
b)部分名称匹配
c)位置匹配

```
> f <- function(a1,b1,a2){
    print(paste("a1:",a1,"b1:",b1,"c1:",a2))}> f(a1=2,b1=3,a2=5)
[1] "a1: 2 b1: 3 c1: 5"
#Example of Exact match, each argument has same name > f(b1=3,5,2)
[1] "a1: 5 b1: 3 c1: 2"
#Example of exact match and positional match. Since b1 is matched, 5 and 2 are assigned based on position> f(3,b=5,2)
[1] "a1: 3 b1: 5 c1: 2"
#Partial name match, b matches to b1\. Rest matched based on position > f(3,5,2)
[1] "a1: 3 b1: 5 c1: 2"
> f(3,5,a=2)
Error in f(3, 5, a = 2) : argument 3 matches multiple formal arguments
#Since a matches to both a1 and a2 we get error as R does not know where the value needs to be assigned.
```

# 结束注释:

r 为我们打开了一个无限可能的世界，它蓬勃发展的社区和生态系统使它成为一门值得学习的编程语言。我希望以上几点对任何使用 R 的人都有用，并帮助他们避免许多常见的错误。这篇博客文章是我在旧金山大学教授的探索性数据分析的几个要点。联系我，讨论/编辑任何具体问题。

**关于我:**研究生，旧金山大学数据科学硕士

**领英:**[https://www . LinkedIn . com/in/jyoti-pra kash-maheswari-940 ab 766/](https://www.linkedin.com/in/jyoti-prakash-maheswari-940ab766/)
**GitHub:**[https://github.com/jyotipmahes](https://github.com/jyotipmahes)

## 参考

1.  [Paul interva do 利用 R](https://github.com/paulintrevado) 进行探索性数据分析