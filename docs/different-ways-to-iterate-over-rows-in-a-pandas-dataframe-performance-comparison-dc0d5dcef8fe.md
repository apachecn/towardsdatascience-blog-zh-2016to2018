# 在 Pandas 数据框架中迭代行的不同方法——性能比较

> 原文：<https://towardsdatascience.com/different-ways-to-iterate-over-rows-in-a-pandas-dataframe-performance-comparison-dc0d5dcef8fe?source=collection_archive---------1----------------------->

**为什么我对迭代的性能感兴趣**

如果你是一个像我一样经常处理结构化数据集的 python 用户，你可能会出于很多原因经常使用 pandas。该软件包包含大量有用的功能，这些功能已经针对其用途和目的进行了优化(例如描述性统计、分组汇总)。大多数时候，这些功能足以满足您的需求。但是当涉及到时间序列数据时，我经常需要在我的 python 代码中遍历数据框并执行特别的滑动窗口计算。

这让我思考——迭代熊猫数据框最省时的方式是什么？

让我们使用这个简单的问题来比较各种迭代方法的性能:

```
N = 1000
repeats = 100
a = np.repeat(1000, N)
pd_dataset = pd.DataFrame({'a': a})# Goal is compute the column b where b(i) = a(i) + 1
b = a + 1
```

这实际上可以通过对整列应用一个操作符来快速解决，从而生成如上所示的新列。操作在这里并不重要，我们的想法是，如果列操作不可行，比较获得相同结果的替代方法。在这个练习中，我实现了 6 个备选方法，并对它们进行了比较。

我从[这篇博文](https://medium.com/pythonhive/python-decorator-to-measure-the-execution-time-of-methods-fa04cb6bb36d)中借用了 timeit 方法，通过用@ timeit 注释方法来测量每个方法的执行时间。这是我修改过的 timeit 方法——它将执行时间附加到一个作为名为 *log_time* 的输入变量传入的列表中:

```
# Credits to [Fahim Sakri](https://medium.com/@fahimsakri?source=post_header_lockup)def timeit(method):
    def timed(*args, **kw):
        ts = time.time()
        result = method(*args, **kw)
        te = time.time()
        kw['log_time'].append(int((te - ts) * 1000))
        return result
    return timed
```

让我们将 a + 1 操作封装到它自己的方法中，这样它就可以计时了。以列操作为例，下面是使用给定方法获得平均执行时间的初始代码:

```
**def my_compute**(x):
    return x + 1

@timeit
**def use_column**(dataset, **kwargs):dataset['b'] = my_compute(dataset.a)**def time_this**(func, method_name):
    *""" Execute the given function 100 times and measure the execution time for each run.
        Returns a dictionary containing the statistics based on the execution times
    """* N = 1000
    repeats = 100
    a = np.repeat(1000, N)
    pd_dataset = pd.DataFrame({'a': a})

    timing = []
    for i in range(repeats):
        func(pd_dataset.copy(), log_time=timing)
    return {'method': method_name, 'average': np.average(timing), 'min': np.min(timing), 'max': np.max(timing)} results = pd.DataFrame()
results = results.append([time_this(use_for_loop_loc,'use_for_loop_loc')])
```

**迭代行的方法**

总之，我比较了 8 种基于现有列生成新的值列的方法(需要对整个列/值数组进行一次迭代)。这些都是在一个 python 文件中实现的。

前两种是在 dataframe 列上应用列方式函数的方法:

1.  **使用 _ 列**:使用熊猫列操作
2.  **使用 _ 熊猫 _ 应用**:使用熊猫[应用](https://pandas.pydata.org/pandas-docs/stable/generated/pandas.DataFrame.apply.html)功能

接下来是在 for 循环中使用 [pandas 索引方法](https://pandas.pydata.org/pandas-docs/stable/indexing.html)访问变量的三种不同方法:

3 **。use_for_loop_loc** :使用熊猫 [loc](https://pandas.pydata.org/pandas-docs/stable/generated/pandas.DataFrame.loc.html) 功能

4. **use_for_loop_at:** 使用 pandas [at](https://pandas.pydata.org/pandas-docs/stable/generated/pandas.DataFrame.at.html) 函数(访问单个值的函数)

5. **use_for_loop_iat:** 使用 pandas [iat](https://pandas.pydata.org/pandas-docs/stable/generated/pandas.DataFrame.iat.html) 函数(访问单个值的函数)

还有其他不使用 pandas 索引的方法:

6. **use_numpy_for_loop** :从 column 中获取底层 numpy 数组，迭代、计算并将值作为新列赋给 dataframe

7. **use_iterrows** :使用 pandas [iterrows](https://pandas.pydata.org/pandas-docs/stable/generated/pandas.DataFrame.iterrows.html) 函数获取要迭代的 iterables

8. **use_zip** :使用 python 内置的 [zip](https://docs.python.org/3.3/library/functions.html#zip) 函数进行迭代，将结果存储在一个 numpy 数组中，然后在完成后将值作为一个新列赋给 dataframe

以下是每种方法的平均执行持续时间(秒)，使用不同的数据集大小(N=1000、10000、10000)重复测试:

```
N = 1000
               method  average  min  max
          use_column     0.00    0    0
     use_panda_apply     1.95    1    9
    use_for_loop_loc   409.69  398  474
     use_for_loop_at    13.01   12   14
    use_for_loop_iat    13.39   13   15
  use_numpy_for_loop     9.59    9   11
  use_panda_iterrows    58.31   57   60
             use_zip     0.02    0    1N = 10000 method  average   min   max
          use_column     0.06     0     1
     use_panda_apply     1.01     1     2
    use_for_loop_loc  4220.30  4182  4418
     use_for_loop_at   127.79   126   130
    use_for_loop_iat   134.03   132   138
  use_numpy_for_loop    94.64    93    96
  use_panda_iterrows   574.54   564   579
             use_zip     3.00     3     3N = 100000 method   average    min    max
          use_column      0.43      0      1
     use_panda_apply      6.01      4     10
    use_for_loop_loc  54340.47  53415  55124
     use_for_loop_at   1263.17   1238   1431
    use_for_loop_iat   1326.79   1313   1365
  use_numpy_for_loop    939.02    926    956
  use_panda_iterrows   5676.55   5609   6184
             use_zip     28.00     27     29
```

我的预期和结果是正确的:

*   列操作和应用都相对较快
*   使用 at()和 iat()选择比 loc()快
*   在 numpy 数组上基于位置的索引比在 pandas 数据帧上基于位置的索引快

令人惊讶的是:

*   对于小数据集，zip()相对较快——对于 N < 1000 的数据集，甚至比 apply()更快
*   iat()和 at()索引比 loc()快 30 倍
*   即使使用索引进行访问，loc()也比预期慢

基于这些结果，从现在开始我会做些什么:

*   如果我不能使用列操作或 apply()实现我想要的，我将使用 zip()代替(而不是 iterrows()！)
*   我将避免使用 loc()来更新或访问单个值，而是使用 iat()和 at()
*   考虑将基础值提取为 numpy 数组，然后执行处理/分析