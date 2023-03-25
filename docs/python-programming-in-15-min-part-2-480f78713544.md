# 15 分钟 Python 编程第 2 部分

> 原文：<https://towardsdatascience.com/python-programming-in-15-min-part-2-480f78713544?source=collection_archive---------3----------------------->

控制流程、功能、面向对象。

![](img/609892ba12ff6c0687e538f508a17473.png)

# **控制流程**

**for 循环**

Python 中的 for 循环能够遍历任何序列的项，比如 list 或 string。

它以任何有序的顺序遍历项目，即字符串、列表、元组、字典的键和其他可迭代的术语。python for 循环以关键字 ***for*** 开始，后面是一个变量属性，它将保存后面的序列对象的值。

*for 循环的语法*

```
for variable in sequence:
    statement(s)
```

示例:

colors.py

python3 colors.py

```
Red
Black
Blue
White
Pink
```

数学 _ 表格. py

在这种情况下，我们只打印语句块中的值。

python3 数学 _ 表格. py

```
1 times 5 is 5
2 times 5 is 10
3 times 5 is 15
4 times 5 is 20
5 times 5 is 25
```

**While 循环**

只要给定的条件为真，Python 编程语言中的 while 循环语句就会重复执行目标语句。与 for 循环不同，while 循环不会运行 n 次，而是直到满足定义的条件。

*While 循环的语法*

```
while condition:
    statement(s)
```

示例:

倒计时 _ 定时器. py

## **范围功能**

Python 中的 range 函数生成一个数字列表，可用于循环迭代和其他少数情况。

现在让我们看看范围函数是如何工作的

```
range(n)
range(begin, end)
```

range(n)生成从 1 开始到(n-1)结束的整数。

range(begin，end)生成从 begin 开始到 end-1 结束的整数。

$python3

>>>范围(8)

范围(0，8)

示例:

打印值域函数迭代的数字的平方。

方形. py

python3 广场. py

```
1
4
9
16
25
36
```

# **功能**

函数是执行特定任务的可重用代码块。它是一个小的计算单元，可以接受参数，也可以返回值。

> 注意:函数体必须像“if”语句一样缩进。

**声明函数**

关键字***def****引入了一个函数定义，后跟函数名和带括号的形参列表。构成函数体的语句从下一行开始，必须缩进。*

*句法上，*

```
*def function_name(formal parameters):
    statement(s)*
```

> *注意:声明一个函数并不运行这个函数。为了运行，您必须使用函数名调用函数。*

*例子*

*greet.py*

*python3 greet.py*

```
*Hello ! Welcome to the party.*
```

*在这个程序中，我们创建了一个名为 greet 的函数，没有空括号之类的参数。该函数被定义为打印一个名为“Hello！欢迎参加聚会。”*

*所以调用函数只是打印给定的字符串。*

***带参数的函数。***

*函数可以接受参数，这些参数是你提供给函数的值，这样函数就可以利用这些参数做一些事情。参数在函数定义中的一对括号内指定，用逗号分隔。*

*示例:*

**greet_names.py**

*python3 greet_names.py*

```
*Hello Flash
Hello Arrow
Hello Bat*
```

**factorial.py**

*python3 factorial.py*

```
*The factorial of number is 120*
```

*参见递归，变量范围。*

# ***面向对象 Python 简介***

*在我们迄今为止编写的所有程序中，我们都是围绕函数来设计程序的，即操作数据的语句块。这被称为面向过程的编程方式。还有另一种组织你的程序的方法，就是把数据和功能结合起来，把它包装在一个叫做类的东西里面。这被称为面向对象编程范例。*

*快速浏览一下**面向对象术语**的基础知识。*

***类**:类是创建单个对象的蓝图。*

***对象**:一个真实世界的实体，有状态和行为。*

*让我们用类中的类方法创建一个类 Person。*

```
*class Person():
    def say(self):
        print("Hello")*
```

*现在让我们为这个类创建一个对象实例。*

```
*class Person():
    def say(self):
        print("Hello") jackman = Person()
jackman.say()*
```

*进一步扩展这个情节让我们创建两个带参数的方法，hello 和 bye。*

*methods_examples.py*

*python3 method_examples.py*

```
*Hello Lee How are you ?
Nice Meeting You Edison*
```

> ***注意:** Self 是所有实例方法的默认参数*

*这需要我们为每个类方法重复实例变量，而不是用实例变量创建一个对象。现在是时候使用构造函数了。*

# ***构造函数***

*Python 中的构造函数是在一个特殊的方法 __init__ 下编写的。*

*现在让我们为一个对象写一个构造函数。在这个例子中，让我们用实例变量 name 和 year_of_birth 创建一个对象。在类中编写构造函数的过程消除了每个实例方法中实例变量的重复。*

**constructors.py**

*python3 构造函数. py*

```
*Name of the person is Vihar
Your are 19 Years Old*
```

# *类和对象示例:*

*银行 _ 账户. py*

*python3 银行账户. py*

```
*100
50
40
90*
```

*使用第 3 部分的以下链接*

*[](https://medium.com/towards-data-science/python-programming-in-15-min-part-3-ce882f9ab9b2) [## 15 分钟 Python 编程第 3 部分

### 异常、模块、包

medium.com](https://medium.com/towards-data-science/python-programming-in-15-min-part-3-ce882f9ab9b2) 

如果您错过了《15 分钟》第 1 部分中的 Python 编程，请浏览链接。

[](https://medium.com/towards-data-science/python-programming-in-15-min-part-1-3ad2d773834c) [## 15 分钟 Python 编程第 1 部分

### 关于 Python

medium.com](https://medium.com/towards-data-science/python-programming-in-15-min-part-1-3ad2d773834c) 

参考:Python 编程的现代方法——瓦姆西·鞍马，皮尔森印度公司。

[https://github.com/vamsi/python-programming-modern-approach](https://github.com/vamsi/python-programming-modern-approach)

感谢 [Sai Tejaswie](https://medium.com/u/6f32f4e13a1f?source=post_page-----480f78713544--------------------------------) 对故事的少许贡献。

请继续关注第三部分。*