# 最后，通过制作一个来学习如何使用命令行应用程序！

> 原文：<https://towardsdatascience.com/finally-learn-how-to-use-command-line-apps-by-making-one-bd5cf21a15cd?source=collection_archive---------8----------------------->

冒着疏远很多读者的风险…我是和 GUI 一起长大的，所以我从来不需要学习终端的方式！在当今友好用户界面的社会中，这是被社会所接受的，除非你从事软件工程……哎呀！

![](img/d8a25f71c5053af8c2905f4db79e70cf.png)

Manager — “Oh this is easy, just use this command line app”, Me — ”Yes…the command line… I’ll use that…”

# 找回我的软件工程街头信誉

我已经通过从 Stack-Overflow 复制和粘贴完整的命令行操作走了很远，但还没有足够舒适地“正确”使用命令行应用程序。最终让我喜欢上它的是当我偶然发现一个非常方便的 python 库，叫做[**arg parse**](https://docs.python.org/3/library/argparse.html)**，它允许你为你的 python 脚本构建一个漂亮健壮的命令行界面。**

**[本教程](https://docs.python.org/3.5/howto/argparse.html)对如何使用 **argparse、**进行了深入的解释，但我还是要回顾一下关键的开眼器 **:****

> **[argparse](https://docs.python.org/3/library/argparse.html) 是标准 python 库的一部分，所以打开你的代码编辑器，跟着做(你不需要安装任何东西)！**

# ****帮助****

**每个命令行 app 最有用的部分！**

```
# inside a file called my_app.py**import** **argparse**
parser = argparse.ArgumentParser(description="*Nice little CL app!*")
parser.parse_args()
```

**上面的代码什么也不会做，除了默认情况下你有**帮助**标志！**

**在命令行中，您可以运行:**

```
python my_app.py --help
```

**或者**

```
python my_app.py -h
```

**您将得到如下输出:**

```
usage: my_app.py [-h] **Nice little CL app!** optional arguments:-h, --help  **show this help message and exit**
```

**看起来很酷，对吧？但是等一下，当你使用 **argparse** 到添加更多功能(见下文)时，这个**帮助**输出将自动填充你如何使用你的应用程序的所有指令！**

# **必需的参数**

**假设你想让你的应用程序接受一些变量，我们只需使用`parser.add_argument()`函数，并给我们的参数一些标签(在这种情况下是“名称”):**

```
import argparse
parser = argparse.ArgumentParser(**description**="*Nice little CL app!*") parser.add_argument("***name***", **help**="*Just your name, nothing special*") args = parser.parse_args()
print("*Your name is what?* " + args**.name**)
```

**注意我们是如何添加**帮助**文本的！现在，当我们运行`python my_app.y -h`时，我们会获得所有应用程序的详细信息:**

```
usage: my_app.py [-h] name **Nice little CL app!** positional arguments:name        **Just your name, nothing special** optional arguments:-h, --help  **show this help message and exit**
```

**很酷，但是让我们用`python my_app.py`来运行我们的应用程序**

```
usage: my_app.py [-h] namemy_app.py: error: too few arguments
```

**没错！自动输入检查！**

**现在让我们运行`python my_app.py "Slim Shady"`**

```
Your name is what? Slim Shady
```

**相当光滑！**

# **可选参数**

**也许你想让某人告诉你更多关于他们的事情？在使用`parser.add_argument()`函数时添加双破折号将使该参数可选！**

```
import argparse
parser = argparse.ArgumentParser(**description**=”*Nice little CL app!*”)
parser.add_argument(“*name*”, **help**=”*Just your name, nothing special*”) parser.add_argument("***--****profession*”, **help**=”Y*our nobel profession*”) args = parser.parse_args()
print(“*Your name is what?* “ + args.name)if args.profession:
    print(“*What is your profession!? a* “ + args.profession)
```

**如果要为该参数传递一个变量，只需在要传递的变量前指定双虚线参数名称:**

```
python my_app.py "Slim Shady" **--profession "gift wrapper"**
```

**这给了你:**

```
Your name is what? Slim ShadyWhat is your profession!? a gift wrapper
```

**或者可以不用，毕竟是可选的！**

```
python my_app.py "Slim Shady"
```

**仍然给你:**

```
Your name is what? Slim Shady
```

**并且在运行时再次神奇`python my_app.py -h`:**

```
usage: my_app.py [-h] [--profession PROFESSION] name **Nice little CL app!** positional arguments:name                      **Just your name, nothing special**optional arguments:-h, --help                **show this help message and exit**--profession PROFESSION   **Your nobel profession**
```

# **旗帜**

**也许你只是想让一些很酷的事情发生。将`action="*store_true*"`添加到您的`parser.add_argument()`函数中，您就有了一个**标志**参数:**

```
import argparseparser = argparse.ArgumentParser(**description**="*Nice little CL app!*")
parser.add_argument("*name*", **help**="*Just your name, nothing special*")
parser.add_argument("*--profession*", **help**="*Your nobel profession*") parser.add_argument("*--cool*", **action**="*store_true*", **help**="*Add a little cool*") args = parser.parse_args()
print("*Your name is what?* " + args.name)cool_addition = " *and dragon tamer*" if args.cool else ""if args.profession:
    print("*What is your profession!? a* " + args.profession + cool_addition)
```

**它非常简洁，您只需将**标志**的名称放入您的命令中，如下所示:**

```
python my_app.py "Slim Shady" --profession "gift wrapper" **--cool**
```

**转眼间。**

```
Your name is what? Slim ShadyWhat is your profession!? a gift wrapper and dragon tamer
```

**记住，你不必使用它，它只是一面旗帜:**

```
python my_app.py "Slim Shady" --profession "gift wrapper"
```

**还是会给:**

```
Your name is what? Slim ShadyWhat is your profession!? a gift wrapper
```

**看看**帮助**命令`python my_app.py -h`:**

```
usage: my_app.py [-h] [--profession PROFESSION] [--cool] name **Nice little CL app!** positional arguments:name                      **Just your name, nothing special**optional arguments:-h, --help                **show this help message and exit**--profession PROFESSION   **Your nobel profession**--cool                    **Add a little cool**
```

**我只是假设从现在开始你和我一样对此感到满意。**

# **简易格式**

**揭开了困扰我很久的神秘的一个角色的争论。仅仅通过在你的`parser.add_argument()`函数中添加一个以单个破折号为前缀的单个字母，你就可以得到相同参数的超短版本:**

```
import argparseparser = argparse.ArgumentParser(**description**="*Nice little CL app!*")
parser.add_argument("*name*", **help**="*Just your name, nothing special*")parser.add_argument("-p", "*--profession*", **help**="*Your nobel profession*")parser.add_argument("-c", "*--cool*", **action**="*store_true*", **help**="*Add a little cool*")args = parser.parse_args()
print("*Your name is what?* " + args.name)
cool_addition = " *and dragon tamer*" if args.cool else ""
if args.profession:
    print("*What is your profession!? a* " + args.profession + cool_addition)
```

**这样就不用输入:**

```
python my_app.py "Slim Shady" --profession "gift wrapper" --cool
```

**您只需键入:**

```
python my_app.py "Slim Shady" -p "gift wrapper" -c
```

**您将得到相同的输出:**

```
Your name is what? Slim ShadyWhat is your profession!? a gift wrapper and dragon tamer
```

**这反映在帮助文本中(`python my_app.py -h`):**

```
usage: my_app.py [-h] [--profession PROFESSION] [--cool] name**Nice little CL app!**positional arguments:name                           **Just your name, nothing special**optional arguments:-h, --help                      **show this help message and exit**-p PROFESSION, --profession PROFESSION   **Your nobel profession**-c, --cool                      **Add a little cool**
```

**![](img/88452351c6c1961f86e6e844f5f862c0.png)**

**a perfect little command line app!**

**你现在有了一个完美的小命令行应用程序，希望你能更舒服地找到命令行应用程序！**

**只要记住`--help`！**

**[](https://github.com/ZackAkil/super-simple-command-line-app) [## ZackAkil/超级简单命令行应用程序

### 超级简单的命令行应用程序代码“终于学会如何使用命令行应用程序...通过制造一个！”

github.com](https://github.com/ZackAkil/super-simple-command-line-app)**