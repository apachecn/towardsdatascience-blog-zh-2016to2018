# NumPy 库快速介绍

> 原文：<https://towardsdatascience.com/a-quick-introduction-to-the-numpy-library-6f61b7dee4db?source=collection_archive---------0----------------------->

![](img/f94b8ac0403c471ea0b63579d106d825.png)

在我的上一篇关于熊猫的博客文章之后，我想退一步写一篇关于 T2 熊猫图书馆的文章可能是个好主意。NumPy(Numerical Python 的缩写)是“用 Python 进行科学计算的基础包”,它是 Pandas、Matplotlib 和 Scikit-learn 构建的库。您可能会想“当我可以使用这些库时，使用 NumPy 有什么意义呢？”但是我认为 NumPy 经常被低估，如果使用得当，它可以成为 Python 中数值运算的一个非常强大的工具。

# 安装和入门

默认情况下，NumPy 没有附带 Python，所以需要安装它。正如我为[熊猫](https://medium.com/@adi.bronshtein/a-quick-introduction-to-the-pandas-python-library-f1b678f34673)安装所推荐的，获得 NumPy(以及大量其他软件包)的最简单方法是安装 [Anaconda](https://www.continuum.io/downloads) 。如果你不想安装所有这些软件包而只是安装 NumPy，你可以从[这个页面](https://pypi.python.org/pypi/numpy)下载你的操作系统版本。

下载并安装 NumPy 后，每当您想在 Python IDE ( [集成开发环境](https://en.wikipedia.org/wiki/Integrated_development_environment))中使用它时，如 [Jupyter Notebook](http://jupyter.org/) 或 [Spyder](https://pythonhosted.org/spyder/) (默认情况下，它们都附带了 Anaconda)，您都需要导入它。提醒一下，导入一个库意味着把它加载到内存中，然后你就可以使用它了。要导入 NumPy，您需要编写以下代码:

```
import numpy as np
```

你已经准备好了！通常你会添加第二部分(“作为 np”)，这样你就可以通过写“np.command”来使用 NumPy，而不是每次你想使用它时都必须写“numpy.command”。这不是一个巨大的差异，但嘿，每一个关键的中风计数！请记住，每当你开始一个新的 Jupyter 笔记本，Spyder 文件等，你需要这样做。

# 使用 NumPy

## 创建 NumPy 数组，加载和保存文件

NumPy 使用称为多维**数组**的 Python 对象。数组基本上是值的集合，它们有一个或多个维度。NumPy 数组数据结构也叫 *ndarray* ，是 n 维数组的简称。一维数组称为**向量**，二维数组称为**矩阵**。数据集通常以矩阵的形式构建，例如，用 NumPy 打开比用列表的列表打开要容易得多。

将列表转换成 NumPy 数组非常简单:

```
numpy_array = np.array(list)
```

打印/显示该数组将如下所示:

```
array([[  7.4  ,   0.7  ,   0\.   , ...,   0.56 ,   9.4  ,   5\.   ],
       [  7.8  ,   0.88 ,   0\.   , ...,   0.68 ,   9.8  ,   5\.   ],
       [  7.8  ,   0.76 ,   0.04 , ...,   0.65 ,   9.8  ,   5\.   ],
       ..., 
       [  6.3  ,   0.51 ,   0.13 , ...,   0.75 ,  11\.   ,   6\.   ],
       [  5.9  ,   0.645,   0.12 , ...,   0.71 ,  10.2  ,   5\.   ],
       [  6\.   ,   0.31 ,   0.47 , ...,   0.66 ,  11\.   ,   6\.   ]])
```

另一个选项是使用 [np.genfromtxt()](https://docs.scipy.org/doc/numpy/reference/generated/numpy.genfromtxt.html) 函数打开一个 CSV 文件:

```
numpy_array **=** np**.**genfromtxt("file.csv", delimiter**=**";", skip_header**=**1)
```

括号内的参数是文件名(和路径，如果需要的话)，分隔符设置为“；”为了确保正确解析，可以使用不同的字符进行解析(比如'，')；而 skip_header 设置为‘1’将使 csv 加载到没有标题行的数组中。如果您确实需要标题，可以不包含它(因为缺省值是零)。

还可以使用 np.savetxt()将 NumPy 数组保存到文件中。例如，`np.savetxt('file.txt',arr,delimiter=' ')`将保存到文本文件，而`np.savetxt('file.csv',arr,delimiter=',')`将保存到 CSV 文件。

另一个很酷的特性是能够创建不同的数组，比如随机数组:`np.random.rand(3,4)`将创建一个 0 到 1 之间的 3x4 随机数数组，而`np.random.rand(7,6)*100`将创建一个 0 到 100 之间的 7x6 随机数数组；你也可以用不同的方式定义数组的大小:`np.random.randint(10,size=(3,2))`用 0 到 9 之间的随机数创建一个 3x2 大小的数组。请记住，使用此语法时，最后一位数字(10)不包括在范围内。

也可以创建一个全 0 的数组:`np.zeros(4,3)`(全 0 的 4x3 数组)或 1`np.ones((4))`(1 的 4x1 数组)；您可以使用命令`np.full((3,2),8)`创建一个由 8 个元素组成的 3x2 数组。当然，你可以改变每一个数字来得到你想要的数组。

## 工作和检查阵列

现在您已经加载了数组，您可以通过键入`array.size`来检查它的大小(元素的数量),并通过键入`array.shape`来检查它的形状(尺寸——行和列)。您可以使用`array.dtype`来获取数组的数据类型(浮点、整数等——参见 [NumPy 文档](https://docs.scipy.org/doc/numpy/user/basics.types.html)),如果您需要转换数据类型，您可以使用`array.astype(dtype)`命令。如果您需要将 NumPy 数组转换成 Python 列表，也有一个命令可以完成这个任务:`array.tolist()`。

## 索引和切片

索引和分割 NumPy 数组的工作方式与处理 Python 列表非常相似:`array[5]`将返回第 5 个索引中的元素，`array[2,5]`将返回索引[2][5]中的元素。您也可以选择前五个元素，例如，使用冒号(:)。`array[0:5]`将返回前五个元素(索引 0–4)，而`array[0:5,4]`将返回第 4 列中的前五个元素。您可以使用`array[:2]`来获取从开始到索引 2(不包括索引 2)的元素，或者使用`array[2:]`从第二个索引返回到数组的末尾。`array[:,1]`将返回所有行中索引 1 处的元素。

给 NumPy 数组赋值也非常类似于在 Python 列表中这样做:`array[1]=4`将值 4 赋给索引 1 上的元素；您可以对多个值执行此操作:`array[1,5]=10`或者在赋值时使用切片:`array[:,10]=10`会将整个第 11 列更改为值 10。

## 分类和整形

`array.sort()`可用于对 NumPy 数组进行排序——您可以在括号内传递不同的参数来定义您想要排序的内容(例如，通过使用参数“order = string/list of string”)。参见[文档](https://docs.scipy.org/doc/numpy/reference/generated/numpy.ndarray.sort.html#numpy.ndarray.sort)中的更多示例。`array.sort(axis=0)`将对数组的特定轴——行或列进行排序。`two_d_arr.flatten()`将二维数组展平为一维数组。`array.T`将转置一个数组——意味着列将变成行，反之亦然。`array.reshape(x,y)`会将您的数组重新调整为您用 x 和 y 设置的大小。`array.resize((x,y))`会将数组形状更改为 x 和 y，并用零填充新值。

## 组合和拆分

您可以使用`np.concatenate((array1,array2),axis=0)`来组合两个 NumPy 数组——这将把数组 2 作为行添加到数组 1 的末尾，而`np.concatenate((array1,array2),axis=1)`将把数组 2 作为列添加到数组 1 的末尾。`np.split(array,2)`会将数组拆分成两个子数组，而`np.hsplit(array,5)`会在*第 5 个*索引处水平拆分数组。

## 添加和删除元素

当然，有一些命令可以在 NumPy 数组中添加和删除元素:

*   `np.append(array,values)`将值追加到数组末尾。
*   `np.insert(array, 3, values)`将在索引 3 之前将值插入数组
*   `np.delete(array, 4, axis=0)`将删除数组索引 4 上的行
*   `np.delete(array, 5, axis=1)`将删除数组索引 5 上的列

## 描述统计学

您可以使用 NumPy 方法获取 NumPy 数组的描述性统计信息:

*   `np.mean(array,axis=0)`将返回沿特定轴的平均值(0 或 1)
*   `array.sum()`将返回数组的总和
*   `array.min()`将返回数组的最小值
*   `array.max(axis=0)`将返回特定轴的最大值
*   `np.var(array)`将返回数组的方差
*   `np.std(array,axis=1)`将返回特定轴的标准偏差
*   `array.corrcoef()`将返回数组的相关系数
*   `numpy.median(array)`将返回数组元素的中值

## 和 NumPy 一起做数学

如果没有数字和数学运算，任何 NumPy 教程都是不完整的！让我们复习一下:

`np.add(array ,1)`将把数组中的每个元素加 1，`np.add(array1,array2)`将把数组 2 加到数组 1。对于`np.subtract(), np.multiply(), np.divide() and np.power()`也是如此——所有这些命令都将以与上述完全相同的方式工作。

您还可以让 NumPy 从数组中返回不同的值，比如:

*   `np.sqrt(array)`将返回数组中每个元素的平方根
*   `np.sin(array)`将返回数组中每个元素的正弦值
*   `np.log(array)`将返回数组中每个元素的自然对数
*   `np.abs(arr)`将返回数组中每个元素的绝对值
*   如果数组具有相同的元素和形状，`np.array_equal(arr1,arr2)`将返回`True`

可以对数组中的不同值进行舍入:`np.ceil(array)`将向上舍入到最接近的整数，`np.floor(array)`将向下舍入到最接近的整数，`np.round(array)`将向下舍入到最接近的整数。

这只是你能用 NumPy 做什么的冰山一角！我真的希望这篇博客文章能帮助你看到使用 Python 处理数据的可能性和 NumPy 的强大之处。如果你喜欢这个教程，请随意查看我的[熊猫教程](https://medium.com/@adi.bronshtein/a-quick-introduction-to-the-pandas-python-library-f1b678f34673)(无耻地推销自己:-P)。一如既往的感谢您的阅读！我很感激任何评论、笔记、更正、问题或建议——如果你有什么想让我写的，请不要犹豫让我知道。下一篇博文再见！