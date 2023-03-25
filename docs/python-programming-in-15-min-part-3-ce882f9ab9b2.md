# 15 分钟 Python 编程第 3 部分

> 原文：<https://towardsdatascience.com/python-programming-in-15-min-part-3-ce882f9ab9b2?source=collection_archive---------5----------------------->

异常、模块、包

![](img/89dce43a946f36cefe4a029a64246cab.png)

# **错误和异常**

Python 中最常见的观点是，它处理所有异常错误。一个**异常**是一个错误或其他异常情况发生的信号。有几个内置的异常，它指示某些条件，如 IndentationError:意外缩进，ZeroDivisionError:被零除。您还可以定义例外情况。

程序是易受影响的。如果代码总是返回有效的结果就好了，但有时无法计算出正确的结果。

例如，不能将一个数除以零，也不能访问负项列表中的第三个元素。

到目前为止，错误消息还没有被提及，但是如果您尝试过这些例子，您可能已经看到了一些。(至少)有两种不同的错误:

1.**语法错误**
2。**异常**

## **语法错误**

语法错误，也称为解析错误，可能是您在学习 Python 时最常见的抱怨。语法错误几乎总是致命的，也就是说，几乎没有办法成功执行一段包含语法错误的代码。

示例:

```
>>> print("Hello
File "<stdin>", line 1
print("Hello
            ^
SyntaxError: EOL while scanning string literal
```

该错误是由箭头前面的标记引起的。在本例中，在 print()函数中检测到错误，因为括号没有闭合。

```
>>> while True print("Hello World !")
File "<stdin>", line 1
while True print("Hello World !")
          ^
SyntaxError: invalid syntax
```

由于 while 循环的条件后缺少冒号“:”，因此遇到了语法错误。

## **异常情况**

当程序中发生异常情况时，就会发生异常。例如，如果您要读取一个不存在的文件，或者在程序运行时不小心删除了该文件，该怎么办？这种情况使用异常来处理。

类似地，如果你的程序有一些错误的语句呢？
这是由 Python 处理的，它告诉你有一个错误。

示例:

考虑一个简单的打印函数调用。如果我们把 print 这个词拼错成了 Print 呢？注意这里的大写。在这种情况下，Python 会引发语法错误。

```
>>> Print("Hello there !")
Traceback (most recent call last):
File "<stdin>", line 1, in <module>
NameError: name 'Print' is not defined
```

观察器:引发了一个 NameError，并且还打印出了检测到错误的位置。

> 现在让我们看看 Python 中几种错误

当一个数被零除时。

```
>>> 2/0
Traceback (most recent call last):
File "<stdin>", line 1, in <module>
ZeroDivisionError: division by zero
```

IndexError:当索引超出范围时。

```
>>> list = [1,2,3]
>>> list[4]
Traceback (most recent call last):
File "<stdin>", line 1, in <module>
IndexError: list index out of range
```

TypeError:当操作或函数应用于不适当类型的对象时引发

```
>>> '2' + 2
Traceback (most recent call last):
File "<stdin>", line 1, in <module>
TypeError: must be str, not int
```

KeyError:当字典使用不当时会发生。

```
>>> dict = {'a' : 'Stark', 'b': 'Steve'}
>>> dict['c']
Traceback (most recent call last):
File "<stdin>", line 1, in <module>
KeyError: 'c'
```

# **异常处理**

像许多其他编程语言一样，Python 也有异常处理。我们可以使用 try except for 语句处理异常。我们基本上将常规语句放在 try 块中，并将所有错误处理程序放在 except 块中。

示例:

handling_exception.py

python3 handling_exception.py

```
You can't divide by zero.
```

## **捕捉 Python 中的特定异常**

try 子句可以有任意数量的 except 子句来以不同的方式处理它们，但是在出现异常的情况下，只会执行一个子句。

我们可以使用一组值在 except 子句中指定多个异常。下面是一个伪代码示例。

```
try:
# do something
    passexcept ValueError:
# handle ValueError exception
    passexcept (TypeError, ZeroDivisionError):
# handle multiple exceptions
# TypeError and ZeroDivisionError
    passexcept:
# handle all other exceptions
    pass
```

## **引发异常**

在 Python 编程中，当运行时出现相应的错误时，会引发异常，但是我们可以使用关键字 raise 强制引发异常。

示例:引发键盘中断

```
>>> raise KeyboardInterrupt
Traceback (most recent call last):
File "<stdin>", line 1, in <module>
KeyboardInterrupt
>>>
```

提高内存错误

```
>>> raise MemoryError(“Argument”)
Traceback (most recent call last):
File “<stdin>”, line 1, in <module>
MemoryError: Argument
```

让我们提出一个值错误并排除这个错误。

提升 _ 错误. py

python3 提升 _error.py

```
Enter a negative integer: 5
That is not a negative number!
```

## **试试……终于**

Python 中的 try 语句可以有一个可选的 finally 子句。这个子句无论如何都要执行，一般用来释放外部资源。

文件处理. py

在这里，它尝试打开当前目录中的 text.txt 文件，否则将引发一个 FileNotFoundError 错误。

# **模块**

Python 附带了数百个模块，可以做各种各样事情。也可以从互联网上下载第三方模块。

Python 包括一组称为标准库的模块，例如，math，cmath，它包含实数和复数的数学函数，但还有更多。

使用 import 语句导入模块。

```
import module_name
```

现在让我们导入几个模块并在其中运行函数。

```
>>> import math
>>> math.pi
3.141592653589793
>>> math.sin(0)
0.0
>>> math.cos(45)
0.5253219888177297
```

这里，我们导入了数学模块，并使用 sin 和 cos 函数来返回值。

```
>>> import time
>>> print(time.asctime())
Thu Jul 27 01:47:01 2017
```

在本例中，我们导入了时间模块，并从该模块调用了 **asctime** 函数，该函数以字符串形式返回当前时间。

> 还有一种导入方式是使用 import 语句。

```
>>> from time import asctime
>>> asctime()
'Thu Jul 27 01:49:10 2017'
```

这里，我们只从时间模块中导入了 asctime 函数。

# **套餐**

考虑一个声音包，组织你的 Python 代码的方式创造了令人敬畏的包。

```
sound/                        **Top-level package**
    __init__.py               Initialise the sound packageformats/                      **Sub-package** for file format
    __init__.py
    wavread.py
    wavwrite.py
    aiffread.py
    ...effects/                      **Sub-package** for sound effects
    __init__.py
    echo.py
    surround.py
    reverse.py
    ...filters/                      **Sub-package** for filters
    __init__.py
    equalizer.py
    vocoder.py
    karaoke.py
    ...
```

## **第三方包**

Python 拥有最大的创建 Python 包的社区。在[https://pypi.python.org/pypi](https://pypi.python.org/pypi)有超过 100，000 种包装可供选择。

Python 包是所有模块的集合，这些模块恰当地连接成一个形式，并且是分布式的 PyPi，Python 包索引维护可用 Python 包的列表。现在，当您完成 pip 设置后，进入命令提示符或终端，并说

```
pip install <package-name>
```

运行此命令后，包将被安装到您的 python 库中。您可以将包导入到您的程序中。

**恭喜**您已经完成了基本的 Python 编程！

向所有阅读并支持这篇报道的人致敬。

第 1 部分和第 2 部分使用以下链接。

[](https://medium.com/towards-data-science/python-programming-in-15-min-part-1-3ad2d773834c) [## 15 分钟 Python 编程第 1 部分

### 关于 Python

medium.com](https://medium.com/towards-data-science/python-programming-in-15-min-part-1-3ad2d773834c) [](https://medium.com/towards-data-science/python-programming-in-15-min-part-2-480f78713544) [## 15 分钟 Python 编程第 2 部分

### 控制流程、功能、面向对象。

medium.com](https://medium.com/towards-data-science/python-programming-in-15-min-part-2-480f78713544) 

写作方式和例子的灵感来自《Python 编程的现代方法》一书。

启动 GitHub 回购，传播爱。

[](https://github.com/vamsi/python-programming-modern-approach) [## vamsi/python-编程-现代方法

### python 编程-现代方法-Python 编程工具包:现代方法书。参考书中的步骤…

github.com](https://github.com/vamsi/python-programming-modern-approach) 

感谢 [Richa Kulkarni](https://medium.com/u/ca469a170fd2?source=post_page-----ce882f9ab9b2--------------------------------) 对故事的少许贡献。

感谢阅读。如果你觉得这个故事有帮助，请点击下面的💚去传播爱。