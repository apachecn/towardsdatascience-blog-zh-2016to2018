# 如何为机器学习设置 Python 环境

> 原文：<https://towardsdatascience.com/how-to-setup-a-python-environment-for-machine-learning-354d6c29a264?source=collection_archive---------8----------------------->

![](img/ae3813504f20b0e0f9a30fc322869f7a.png)

> 想获得灵感？快来加入我的 [**超级行情快讯**](https://www.superquotes.co/?utm_source=mediumtech&utm_medium=web&utm_campaign=sharing) 。😎

为机器学习设置 Python 环境可能是一项棘手的任务。如果你以前从未设置过这样的东西，你可能会花几个小时摆弄不同的命令，试图让它工作。但我们只想直接去 ML！

在本教程中，您将学习如何建立一个*稳定的* Python 机器学习开发环境。您将能够直接进入 ML，再也不用担心安装包了。

# (1)设置 Python 3 和 Pip

第一步是安装 pip，这是一个 Python 包管理器:

```
sudo apt-get install python3-pip
```

使用 pip，我们将能够用一个简单的`pip install *your_package*` *安装在 [*Python 包索引*](https://pypi.org/) 中索引的任何 Python 包。*你很快就会看到我们如何用它来建立我们的虚拟环境。

接下来，当从命令行运行`pip`或`python`命令时，我们将 Python 3 设置为默认。这使得使用 Python 3 更加容易和方便。如果我们不这样做，那么如果我们想使用 Python 3，我们每次都必须记住键入`pip3`和`python3`！

为了强制 Python 3 成为默认版本，我们将修改`~/.bashrc`文件。从命令行执行以下命令来查看该文件:

```
nano ~/.bashrc
```

向下滚动到 **#更多 ls 别名**部分，并添加以下行:

```
alias python='python3'
```

保存文件并重新加载您的更改:

```
source ~/.bashrc
```

嘣！Python 3 现在是您的默认 Python 了！你可以在命令行上用简单的`python *your_program*` 来运行它。

# (2)创建虚拟环境

现在我们将建立一个虚拟环境。在那里，我们将安装机器学习所需的所有 python 包。

我们使用虚拟环境来分离我们的编码设置。想象一下，如果你想在你的电脑上做两个不同的项目，需要不同版本的不同库。将它们都放在同一个工作环境中可能会很麻烦，并且您可能会遇到库版本冲突的问题。你的项目 1 的 ML 代码需要 1.0 版本的`numpy`，但是项目 2 需要 1.15 版本。呀！

虚拟环境允许我们隔离工作区域，以避免这些冲突。

首先，安装相关的软件包:

```
sudo pip install virtualenv virtualenvwrapper
```

一旦我们安装了 virtualenv 和 virtualenvwrapper，我们将再次需要编辑我们的`~/.bashrc`文件。将这三行放在底部，保存它。

```
export WORKON_HOME=$HOME/.virtualenvs
export VIRTUALENVWRAPPER_PYTHON=/usr/bin/python3
source /usr/local/bin/virtualenvwrapper.sh
```

保存文件并重新加载您的更改:

```
source ~/.bashrc
```

太好了！现在我们终于可以像这样创建我们的虚拟环境了:

```
mkvirtualenv ml
```

我们刚刚创建了一个名为`ml`的虚拟环境。要输入，请执行以下操作:

```
workon ml
```

不错！你在`ml`虚拟环境中进行的任何库安装都将被隔离在那里，不会与任何其他环境冲突！因此，无论何时你希望运行依赖于安装在`ml`环境中的库的代码，首先用`workon`命令进入，然后正常运行你的代码。

如果您需要退出 virtualenv，请运行以下命令:

```
deactivate
```

# (3)安装机器学习库

现在我们可以安装我们的 ML 库了！我们将使用最常用的方法:

*   **numpy:** 用于任何矩阵工作，尤其是数学运算
*   **科学技术计算**
*   **pandas:** 数据处理、操作和分析
*   **matplotlib:** 数据可视化
*   **scikit learn:** 机器学习

这里有一个简单的技巧，可以快速安装所有这些库！创建一个`requirements.txt`文件，列出您希望安装的所有软件包，如下所示:

```
numpy
scipy
pandas
matplotlib
scikit-learn
```

完成后，只需执行以下命令:

```
pip install -r requirements.txt
```

瞧啊。Pip 将继续安装文件中列出的所有软件包。

恭喜你，你的环境已经设置好了，你可以开始机器学习了！

# 喜欢学习？

在推特[上关注我，我会在这里发布所有最新最棒的人工智能、技术和科学！也在 LinkedIn](https://twitter.com/GeorgeSeif94) 上与我联系！