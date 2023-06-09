# 为什么使用上下文管理器是更好的选择？

> 原文：<https://towardsdatascience.com/https-medium-com-sinister-why-using-a-context-manager-is-a-better-choice-55ccfcecddb8?source=collection_archive---------3----------------------->

每当我用 python 写几行代码时，脑子里总会冒出一个问题，有没有更好的方法来写这段代码！更好的方法不是在算法方法的意义上，虽然它很重要，实际上它是最重要的，像减少时间或空间的复杂性或两者都有！与其他人解决同样问题的方式相比，这是让你的方法占上风的原因。但是紧接着的下一个重要问题是，你是否按照它应该的方式写了它？

长话短说，有没有更 pythonic 化(当然也更好)的方法来编写同样的代码？

假设您应该将一些 json 数据写入一个文件。

显而易见的方法是打开文件，将 json 数据写入文件，然后关闭它。这种简单直观的方法只有在没有例外的情况下才是好的。

比方说，第 16 行产生了一个错误，可能是`data`不是一个有效的 json。在这种情况下，进一步的执行会停止，程序会立即停止。这里你应该注意到一件事，`close()`在第 18 行没有被调用。因此，文件描述符已经泄漏，数据很有可能会被破坏。现在，这是一个可怕的问题，尤其是如果您正在处理的文件对您所在的系统或业务组织至关重要。

那么，您应该如何解决这个问题呢？一个可能的解决方案是使用 try-except 块，它不仅可以捕获错误，而且在任何一种情况下，我们都有能力显式关闭文件。

然而，将代码放入 try-except 块大多数时候看起来很难看，更好更漂亮的替代方法是使用上下文管理器。

非常简单的语法如下

```
with context_manager as something:
     # do anything with something
     pass
```

这相当于 try-finally 块

```
something = expression
something.__enter__()
try:
    # do anything with something
    pass
finally:
    something.__exit__()
```

我们用`with`语句重写的函数现在看起来更干净了

我们不需要显式关闭文件，所有这些都由`with`语句负责。当然，在引擎盖下会发生一些事情。为了更详细地理解它们，让我们创建一个我们自己的上下文管理器。

我们的自定义上下文管理器是一个具有(特别是)以下两种方法的类:

*   `__enter__()`
*   `__exit__()`

在调用上下文管理器的过程中调用`__enter__()`，当`with`块的执行完成时，即在块的末尾，调用`__exit__()`方法来包装代码。所以任何你认为应该在`with`块的初始化阶段执行的东西都应该放在`__enter__()`的体内。`__exit__()`应该包含您希望在`with`块末尾执行的所有清理代码。即使在运行时遇到错误，也会执行`__exit__()`主体中的代码。这确保了清理动作被保证执行，就像在前面的情况中关闭文件一样，即使在调用`json.loads()`或`json.dump()`的过程中有错误。

需要记住的一点是，`with`块中定义的变量有局部作用域，但是上下文管理器本身没有局部作用域(w.r.t. `with`块)，因此可以重用。

上下文管理器可以用于其他目的，而不是简单的文件 I/O，比如打开和关闭套接字，在测试期间实现设置和拆除功能，以及超出我知识范围的事情。

我希望你喜欢读这篇文章，如果你有什么建议，请在评论中告诉我。最后，如果你觉得这篇文章值得你花时间，请点击推荐按钮，我认为如果你不动它，跟随 *的* [*按钮会嫉妒的。感谢阅读，希望你学到了新的东西；——)*](https://medium.com/@sinister)