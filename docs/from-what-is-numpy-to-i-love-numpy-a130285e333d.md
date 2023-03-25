# 来自《什么是 numpy？🤔到“我爱 numpy！💪🏼"

> 原文：<https://towardsdatascience.com/from-what-is-numpy-to-i-love-numpy-a130285e333d?source=collection_archive---------2----------------------->

![](img/3dcbce1aeac0da8b49f4f027f1708b08.png)

Numpy 或“数字 python”是用于科学计算的核心 Python 库，Numpy 也很快，因为它与 C 库绑定。

常规 python 列表与 Numpy 数组:

> 这种差异主要是由于“间接性”——Python 列表是指向 Python 对象的指针数组，每个指针至少有 4 个字节，即使是最小的 Python 对象也有 16 个字节(4 个用于类型指针，4 个用于引用计数，4 个用于值——内存分配器四舍五入到 16)。NumPy 数组是一个统一值的数组，单精度数字每个占用 4 个字节，双精度数字占用 8 个字节。不太灵活，但是您为标准 Python 列表的灵活性付出了很多！这就是 numpy 数组比 Python list 更紧凑的原因。

来源:[😊](http://stackoverflow.com/questions/993984/why-numpy-instead-of-python-lists)

目录:

1.  装置
2.  基本数字阵列
3.  另一个很酷的内置方法

# **安装:👨🏼‍💻**

有两种安装方式:使用 pip 或 anaconda。如果你没有安装 anaconda，我会使用 pip，但是我推荐使用 [anaconda](https://www.continuum.io/downloads) 🐍相反。

```
conda install numpy
```

或者

```
pip install numpy
```

# **基本 Numpy 数组:👓**

Numpy 数组有两种不同的类型:vetrices 和 matrices。Vetrices 是严格的一维数组，而 Matrices 是二维的(注意:二维数组可以包含一行和一列)。在这一节中，我想介绍 Numpy 数组和几个函数的基本实现。

```
import numpy as np
```

这就是 numpy 显示二维数组的方式。在这种情况下，上面显示的二维数组的形状是(3，3)。3 个用于其行，3 个用于其列。

*   shape: shape 是一个描述符，用于从给定的 np 数组中返回元组中的行数和列数。
*   整形:为你的 np 数组分配一个新形状的函数

> 注意:当你试图改变二维数组的形状时，请注意数组中有多少个元素。

**举例:**

当一个(3，3) 2d 数组总共有 9 个元素时，如果将其列整形为(3，5)或任何大于 3 的数，这是不可能的。但是在另一方面，仍然有可能重新成形为(9，1)，九行和一列。

```
numpy_array.reshape(3, 5)
```

会给你:

```
Traceback (most recent call last):
  File "practice_one_numpy.py", line 9, in <module>
    two_darray.reshape(3, 5)
ValueError: cannot reshape array of size 9 into shape (3,5)
```

# 内置方法🐍：

这里有几个很酷而且可能是有用的数字内置方法😉。

*   arange:返回具有给定间隔的均匀分布的值
*   眼睛:创建一个单位矩阵
*   random.rand:创建一个给定形状的从 0 到 1 的随机数数组。

如果您想了解更多关于 numpy 的信息，请查看此资源。🐍。谢谢你😁