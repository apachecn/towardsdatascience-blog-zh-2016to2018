# 用 purrr 实现 R 语言中的函数式编程

> 原文：<https://towardsdatascience.com/functional-programming-in-r-with-purrr-469e597d0229?source=collection_archive---------0----------------------->

![](img/e4a2976cfcdccad5f2c7b2cd37c95d63.png)

Want replication without repetition? Use purrr!

当您第一次开始使用 R 时，您可能正在编写简单的代码。

```
print("Hello world!")5 * 6x <- c(1, 2, 3, 4, 5)
```

这很好，你正在学习 R 中的字符串、数学和向量！

然后你开始做一些基本的分析。你想看看你是否能找到一些数字的平均值。

```
employee <- c('John Doe','Peter Gynn','Jolie Hope')
salary <- c(21000, 23400, 26800)
startdate <- as.Date(c('2010-11-1','2008-3-25','2007-3-14'))# form dataframe and take mean of salary column
employ_data <- data.frame(employee, salary, startdate)
mean(employ_data$salary)# output
[1] 23733.33
```

最终，您有希望接触到`tidyverse`，并且您会发现这个“为数据科学设计的 R 包的固执己见的集合”是如何使 R 中的数据分析更容易和更可读的！

```
mtcars %>% 
  group_by(cyl) %>% 
  summarize(mean(mpg))# A tibble: 3 x 2
    cyl `mean(mpg)`
  <dbl>       <dbl>
1  4.00        26.7
2  6.00        19.7
3  8.00        15.1
```

一切都很顺利！在这一点上，你很可能已经取代了 Excel，还有 SPSS 或其他一些统计软件套件！但是你会遇到一个问题，你需要重复使用一个函数。

您可以使用类似下面的代码来计算一些**因变量**和一组**自变量**的单向方差分析:

```
aov_mpg <- aov(mpg ~ factor(cyl), data = mtcars)
summary(aov_mpg)aov_disp <- aov(disp ~ factor(cyll), data = mtcars)
summary(aov_disp)aov_hp <- aov(hp ~ factor(cyl), data = mrcars)
summry(aov_hpp)aov_wt <- aov(wt ~ factor(cyl), datas = mtcars)
summary(aov_wt)
```

但是您复制粘贴了 3 次代码，糟糕的是，您犯了一些小的拼写错误，从而引发了一个错误！(以上代码导致错误！)

此外，如果您意识到您实际上想要运行这些 ANOVAs 来计算齿轮数而不是气缸数，会怎么样呢？你必须回去把`factor(cyl)`呼叫改成`factor(gear)` 4x！这不是很有效率，而且你更有可能以错误结束，因为你必须多次输入所有的内容！

再举个例子怎么样。

让我们根据**气缸**的数量，计算**重量**和**每加仑行驶里程**之间的线性关系的 **R 平方**值。

我在下面写了代码，为来自`mtcars`数据集的 4 缸汽车做了这些。这是一个最坏的情况，你知道一些`dplyr`代码(`dplyr::filter`)，但是不习惯使用管道。这很好，你完成了你的目标，但大量的编码！你必须为 6 缸和 8 缸汽车复制这个代码，甚至更多的代码…

```
# create df for 4 cylinder cars
cyl_4 <- filter(mtcars, cyl == 4)# create a linear model on 4 cyl cars
lm_4 <- lm(mpg ~ wt, data = cyl_4)# get the summ
lm_4_summary <- summary(lm_4)# get the r.squared value
lm_4_r_squared <- lm_4_summary["r.squared"]# check the value
lm_4cyl_r_squared# output
$r.squared
[1] 0.5086326
```

或者，你可以对管道做同样的事情。输入要少得多，但是对所有 3 个子集都这样做意味着我们必须多次复制粘贴，所以如果你最终想除了 mpg ~ wt 之外还作为 mpg ~ disp 的线性模型这样做，你就必须再复制代码 3 次，再修改 3 次。这可能看起来没什么大不了的，但是一旦你开始按比例增加代码(比如 10+倍或 100+倍，等等)，最终将是一件大事。

```
lm_4cyl_rsquared <- mtcars %>% 
  filter(cyl == 4) %>%
  lm(mpg ~ wt, data = .) %>% 
  summary() %>% 
  .$"r.squared"# output
[1] 0.5086326
```

为了解决这个最小化 ***重复*** 并进一步*复制的问题，我们可以直接进入`purrr`！要阅读更多关于`purrr`的内容，Hadley Wickham 推荐了“R for Data Science”中的[迭代](http://r4ds.had.co.nz/iteration.html)章节，或者你也可以看看`purrr` [文档](https://cran.r-project.org/web/packages/purrr/purrr.pdf)。最后，珍妮·布莱恩在这里有一个很棒的咕噜咕噜教程。你可以单独加载`purrr`，但是它也作为`tidyverse`库的一部分被加载。*

*![](img/632cd243f2dd2ef32a644ca390916cfc.png)*

*I used to be all meep — meep — PANIC about purrr!!*

*![](img/fe0ea48dd9a0cb86f1f1185a2f8bf4c6.png)*

*now I’m all like
map %>%
map %>%
PARTY!*

*`purrr`允许您将 ***映射*** 函数到数据。恰当地说，`purrr`中的基本功能叫做`map()`！map 函数通过对每个元素应用一个函数并返回一个与输入长度相同的向量来转换它们的输入。*

***map()的基本参数有:** `.x` —列表或原子向量(逻辑、整数、双精度/数字和字符)
`.f` —函数、公式或原子向量*

*基本上`map()`采用一个函数(`.f`)并将其应用于数据(`.x`)。*

*回到我们从线性模型中获取 R 平方的例子，我们使用下面的代码和`purrr`。*

```
*mtcars %>%
  split(.$cyl) %>%
  map(~ lm(mpg ~ wt, data = .)) %>%
  map(summary) %>%
  map_dbl("r.squared")# output
4         6         8 
0.5086326 0.4645102 0.4229655*
```

*这将在 5 行代码中根据气缸数量生成我们所有 3 个线性模型的输出！这就是`purrr`的妙处，函数的高效缩放！*

*让我们来分解我们的线性模型 R 平方码。*

*我们采用 mtcars 数据集，根据气缸数量将其分成数据子集，将 *mpg* by *wt* 的线性模型应用于每个数据子集，应用汇总函数，然后得出 r.squared 值。然而，尽管`purrr`是可读的，我们还是需要了解一些使用它的习惯。*

```
*mtcars %>%
  split(.$cyl) %>%
  map(~ lm(mpg ~ wt, data = .)) %>%
  map(summary) %>%
  map_dbl("r.squared")# output
4         6         8 
0.5086326 0.4645102 0.4229655*
```

*对于我们这里的代码，你可能已经注意到我们有一个“.”在代码中放置了两次。这是数据的占位符，我们可以在下面看到。的“.”指示左侧数据，在本例中为`mtcars`。我们的 split 调用将`mtcars`数据帧分成 3 个数据帧，每个数据帧存储在一个列表中。这可能看起来很奇怪，但是它允许`map`循环遍历我们的 3 个数据帧，并在其中的每一个上单独复制`lm()`函数。*

```
*# piped
mtcars %>% 
  split(.$cyl)# base R
split(mtcars, mtcars$cyl)# output
List of 3
 $ 4:'data.frame': 11 obs. of  11 variables:
  ..$ mpg : num [1:11] 22.8 24.4 22.8 32.4 30.4 33.9 21.5 27.3 26...
  ..$ cyl : num [1:11] 4 4 4 4 4 4 4 4 4 4 ...
  ..$ disp: num [1:11] 108 146.7 140.8 78.7 75.7 ...
  ..$ hp  : num [1:11] 93 62 95 66 52 65 97 66 91 113 ...
  ..$ drat: num [1:11] 3.85 3.69 3.92 4.08 4.93 4.22 3.7 4.08 ...
  ..$ wt  : num [1:11] 2.32 3.19 3.15 2.2 1.61 ...
  ..$ qsec: num [1:11] 18.6 20 22.9 19.5 18.5 ...
  ..$ vs  : num [1:11] 1 1 1 1 1 1 1 1 0 1 ...
  ..$ am  : num [1:11] 1 0 0 1 1 1 0 1 1 1 ...
  ..$ gear: num [1:11] 4 4 4 4 4 4 3 4 5 5 ...
  ..$ carb: num [1:11] 1 2 2 1 2 1 1 1 2 2 ...
 $ 6:'data.frame': 7 obs. of  11 variables:
  ..$ mpg : num [1:7] 21 21 21.4 18.1 19.2 17.8 19.7
  ..$ cyl : num [1:7] 6 6 6 6 6 6 6
  ..$ disp: num [1:7] 160 160 258 225 168 ...
  ..$ hp  : num [1:7] 110 110 110 105 123 123 175
  ..$ drat: num [1:7] 3.9 3.9 3.08 2.76 3.92 3.92 3.62
  ..$ wt  : num [1:7] 2.62 2.88 3.21 3.46 3.44 ...
  ..$ qsec: num [1:7] 16.5 17 19.4 20.2 18.3 ...
  ..$ vs  : num [1:7] 0 0 1 1 1 1 0
  ..$ am  : num [1:7] 1 1 0 0 0 0 1
  ..$ gear: num [1:7] 4 4 3 3 4 4 5
  ..$ carb: num [1:7] 4 4 1 1 4 4 6
 $ 8:'data.frame': 14 obs. of  11 variables:
  ..$ mpg : num [1:14] 18.7 14.3 16.4 17.3 15.2 10.4 10.4 14.7...
  ..$ cyl : num [1:14] 8 8 8 8 8 8 8 8 8 8 ...
  ..$ disp: num [1:14] 360 360 276 276 276 ...
  ..$ hp  : num [1:14] 175 245 180 180 180 205 215 230 150 150 ...
  ..$ drat: num [1:14] 3.15 3.21 3.07 3.07 3.07 2.93 3 3.23 2.76...
  ..$ wt  : num [1:14] 3.44 3.57 4.07 3.73 3.78 ...
  ..$ qsec: num [1:14] 17 15.8 17.4 17.6 18 ...
  ..$ vs  : num [1:14] 0 0 0 0 0 0 0 0 0 0 ...
  ..$ am  : num [1:14] 0 0 0 0 0 0 0 0 0 0 ...
  ..$ gear: num [1:14] 3 3 3 3 3 3 3 3 3 3 ...
  ..$ carb: num [1:14] 2 4 3 3 3 4 4 4 2 2 ...*
```

*类似地，这个“.”in 或 first map 调用是数据的占位符，但在这种情况下，它将循环遍历由前面的管道生成的 3 个数据帧的列表。你可以看到我们得到了一个 3 个`lm()`输出的列表，我们需要`map`一个对每个输出的总结调用来访问 R 平方。*

```
*mtcars %>%
  split(.$cyl) %>%
  map(~ lm(mpg ~ wt, data = .))# output
$`4`Call:
lm(formula = mpg ~ wt, data = .)Coefficients:
(Intercept)           wt  
     39.571       -5.647$`6`Call:
lm(formula = mpg ~ wt, data = .)Coefficients:
(Intercept)           wt  
      28.41        -2.78$`8`Call:
lm(formula = mpg ~ wt, data = .)Coefficients:
(Intercept)           wt  
     23.868       -2.192*
```

*接下来，我们将总结函数映射到每个列表项，以获得具有 R 平方值的更清晰的输出。我们现在有了统计输出的其余部分，包括 p 值和 R 平方。*

```
*mtcars %>%
  split(.$cyl) %>%
  map(~ lm(mpg ~ wt, data = .)) %>%
  map(summary)# output
$`4`Call:
lm(formula = mpg ~ wt, data = .)Residuals:
    Min      1Q  Median      3Q     Max 
-4.1513 -1.9795 -0.6272  1.9299  5.2523Coefficients:
            Estimate Std. Error t value Pr(>|t|)    
(Intercept)   39.571      4.347   9.104 7.77e-06 ***
wt            -5.647      1.850  -3.052   0.0137 *  
---
Signif. codes:  0 ‘***’ 0.001 ‘**’ 0.01 ‘*’ 0.05 ‘.’ 0.1 ‘ ’ 1Residual standard error: 3.332 on 9 degrees of freedom
Multiple R-squared:  0.5086, Adjusted R-squared:  0.454 
F-statistic: 9.316 on 1 and 9 DF,  p-value: 0.01374$`6`Call:
lm(formula = mpg ~ wt, data = .)Residuals:
     Mazda RX4  Mazda RX4 Wag Hornet 4 Drive        Valiant       Merc 280 
       -0.1250         0.5840         1.9292        -0.6897         0.3547 
     Merc 280C   Ferrari Dino 
       -1.0453        -1.0080Coefficients:
            Estimate Std. Error t value Pr(>|t|)   
(Intercept)   28.409      4.184   6.789  0.00105 **
wt            -2.780      1.335  -2.083  0.09176 . 
---
Signif. codes:  0 ‘***’ 0.001 ‘**’ 0.01 ‘*’ 0.05 ‘.’ 0.1 ‘ ’ 1Residual standard error: 1.165 on 5 degrees of freedom
Multiple R-squared:  0.4645, Adjusted R-squared:  0.3574 
F-statistic: 4.337 on 1 and 5 DF,  p-value: 0.09176$`8`Call:
lm(formula = mpg ~ wt, data = .)Residuals:
    Min      1Q  Median      3Q     Max 
-2.1491 -1.4664 -0.8458  1.5711  3.7619Coefficients:
            Estimate Std. Error t value Pr(>|t|)    
(Intercept)  23.8680     3.0055   7.942 4.05e-06 ***
wt           -2.1924     0.7392  -2.966   0.0118 *  
---
Signif. codes:  0 ‘***’ 0.001 ‘**’ 0.01 ‘*’ 0.05 ‘.’ 0.1 ‘ ’ 1Residual standard error: 2.024 on 12 degrees of freedom
Multiple R-squared:  0.423, Adjusted R-squared:  0.3749 
F-statistic: 8.796 on 1 and 12 DF,  p-value: 0.01179*
```

*我们上一张地图有点不同。你可以看到我们这次使用了`map_dbl`。这表明我们希望我们的输出是 dbl 或数字结果。我们得到了很好的命名数字！*

```
*mtcars %>%
  split(.$cyl) %>%
  map(~ lm(mpg ~ wt, data = .)) %>%
  map(summary) %>%
  map_dbl("r.squared")# output
        4         6         8 
0.5086326 0.4645102 0.4229655*
```

*如果我们没有指明`map_dbl`，而是使用了`map`，我们将得到一个相同结果的列表。*

```
*mtcars %>%
  split(.$cyl) %>% 
  map(~ lm(mpg ~ wt, data = .)) %>%
  map(summary) %>%
  map("r.squared")# output
$`4`
[1] 0.5086326$`6`
[1] 0.4645102$`8`
[1] 0.4229655*
```

*您也可以使用`map_dfr`，它将输出绑定到数据帧的行中。*

```
*mtcars %>%
  split(.$cyl) %>% 
  map(~ lm(mpg ~ wt, data = .)) %>%
  map(summary) %>%
  map_dfr("r.squared")# output
# A tibble: 1 x 3
    `4`   `6`   `8`
  <dbl> <dbl> <dbl>
1 0.509 0.465 0.423*
```

*`purrr`和`purrr`中的其他函数有无限的应用，这极大地增强了 r 中函数式编程的能力。我希望本指南能激励您将`purrr`添加到工具箱中，并探索这个有用的`tidyverse`包！*

*作为`purrr`更多应用的一个简短的引子，我将留给你这个例子。我在开始时提到了计算多个变量的 ANOVAs。自己分解一下这个例子，看看大家怎么看！(您可以将这段代码复制粘贴到 R 中，但是需要先加载`tidyverse`和`broom`包)。*

```
*mtcars %>%
  mutate(cyl = factor(cyl),
         am = factor(am)) %>%
  select(mpg, disp, hp) %>%
  map(~ aov(.x ~ cyl * am, data = mtcars)) %>%
  map_dfr(~ broom::tidy(.), .id = 'source') %>%
  mutate(p.value = round(p.value, 5))*
```

*最后，我要感谢几个#r4ds slack 社区成员，他们帮助我理解了 purrr: Frank Farach，Michael Kuehn 和 Kent Johnson。*

*如果你有兴趣加入这个由[杰西·梅根](https://medium.com/u/56211c3483cf?source=post_page-----469e597d0229--------------------------------)领导的社区，在这里查看她的帖子[，愿原力与你同在！](https://medium.com/@kierisi/r4ds-the-next-iteration-d51e0a1b0b82)*

*![](img/58b1853ac21c84c986f931ffb8360c00.png)*