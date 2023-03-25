# 将 Bash 用于数据管道

> 原文：<https://towardsdatascience.com/using-bash-for-data-pipelines-cf05af6ded6f?source=collection_archive---------12----------------------->

![](img/d5495fd7cbfbf14873e926c537dd18d8.png)

作为一名数据科学家，使用 bash 脚本创建数据管道非常有用。这些脚本的可能性几乎是无限的，但是在这里，我将通过一个非常基本的 bash 脚本来下载数据并计算数据集中的行数和列数。

一旦你掌握了使用 bash 脚本的诀窍，你就可以拥有创建物联网设备的基础，以及更多，因为这一切都与 [*Raspberry Pi*](https://www.raspberrypi.org/) *一起工作。*有一个很酷的项目，你可以用它来使用 [*twitter api*](https://developer.twitter.com/en/docs.html) 下载你所有的 twitter 消息，然后预测 twitter 上某个用户的消息是否是垃圾消息。它可以在你房间的树莓 Pi 服务器上运行！不过这有点超出了本教程的范围，所以我们将从查看旧金山的 [*汽车速度数据集开始！*](https://catalog.data.gov/dataset/san-francisco-speed-limit-compliance)

此外，获取实时数据的能力、复制结果的能力对于数据科学来说是非常必要的。在本教程中，我将向您展示如何使用 bash 和 Unix 命令创建数据管道，然后您可以作为数据科学家在工作中使用这些命令

## 先决条件

1.  熟悉命令行(知道如何创建目录、更改目录和创建新文件)
2.  Linux 或苹果电脑
3.  互联网连接

## 目标:创建一个 bash 脚本来完成以下任务

*   从在线资源下载数据
*   计算数据中的行数
*   记录数据中列的名称
*   遍历多个文件并给出所有这些信息

## 第 1 部分:下载数据文件

对于本教程的第一部分，假设我们正在使用 [**旧金山限速数据**](https://catalog.data.gov/dataset/san-francisco-speed-limit-compliance/resource/1407d038-e0f4-439b-9370-64d70253396b) ，我们想通过命令行创建我们的整个管道。我们需要开始为这个作品创建一个文件夹，我称之为`cars_pipeline`

要在 Unix 中做到这一点，您需要在命令行上执行以下命令:

*   制作目录

`mkdir cars_pipeline`

*   搬进新主任

`cd cars_pipeline`

一旦我们进入这里，我们将创建一个新的 bash 脚本，我称之为:`download_data.sh`你可以随意命名，只要它以扩展名`.sh`结尾

*   创建文件的命令是:

`touch download_data.sh`

现在我们已经创建了这个文件，我们将把数据下载到我们正在工作的文件夹中。为此，我们将使用名为 nano 的文本编辑器。

*   为了在 nano 中打开我们的文件，我们在命令行中执行以下命令:

`nano download_data.sh`

一旦打开它，您将通过粘贴以下代码来创建您的第一个 bash 脚本:

*   除了 *#，带#的文本都是注释！/bin/bash* ，它被称为 **shebang** ，是每个 bash 脚本所必需的

```
#!/bin/bash # Command below is to download the data
curl -o speed.csv https://data.sfgov.org/api/views/wytw-dqq4/rows.csv?accessType=DOWNLOAD
```

要保存:

*   控制+ o
*   进入
*   控制+ x

现在我们已经保存了文件，让我们来看看这个用来下载数据的大命令:

`curl -o speed.csv https://data.sfgov.org/api/views/wytw-dqq4/rows.csv?accessType=DOWNLOAD`

*   `curl`是下载命令
*   `-o`是输出的 a 标志
*   `speed.csv`是输出名称
*   `[https://data.sfgov.org/api/views/wytw-dqq4/rows.csv?accessType=DOWNLOAD](https://data.sfgov.org/api/views/wytw-dqq4/rows.csv?accessType=DOWNLOADis)`是下载数据的网址

现在，要下载，我们只需在终端中使用以下命令运行 bash 脚本:

`bash download_data.sh`

如果您现在查看该文件夹，您将看到您已经下载了文件 cars.csv！

如果您对此有任何疑问，请询问，您可以随时通过替换 url 来下载不同的文件。同样，您可以向脚本传递一个命令行参数，这样您就可以使用该脚本下载您想要的任何数据。

这个脚本非常简单，所以让我们转到一个稍微难一点的脚本，它会告诉我们数据集中的行数和列数。

## **第 2 部分:为所需信息解析文件**

在这里，我们将创建一个更复杂的脚本，这将更深入。我将仔细检查每一行，解释它是做什么的，然后在最后把它们作为一个脚本放在一起。

我将第二个脚本命名为:`process_data.sh`,您可以随意命名，只要它以扩展名`.sh`结尾。

您将使用与上面相同的过程来编辑和保存您的 bash 脚本。您可以跟随文章，或者向下滚动并使用 nano 复制整个脚本。

第一行:shebang

*   在[计算](https://en.wikipedia.org/wiki/Computing)中，a **shebang** 是由字符[数字符号](https://en.wikipedia.org/wiki/Number_sign)和[感叹号](https://en.wikipedia.org/wiki/Exclamation_mark) (#！)在一个[脚本](https://en.wikipedia.org/wiki/Script_(computing))的开头。

`#!/bin/bash`

我们将把文件名作为命令行参数传入，并用我们的结果保存一个文本文件。因此，我们将使用一个名为`echo`的命令，它打印出跟随其后的值。为了访问参数，我们将列举它们，第一个是`$1`，第二个是`$2`，依此类推…

我们希望它保存文件的名称，因此我们将使用命令:

`echo "The name of the file is:" $1`

现在，我们将使用以下命令来计算 csv 中的行数:

```
lines=$(wc -l < $1)echo "The file has" $lines "lines"
```

*   我们创建了变量 lines 来计算我们作为命令行参数传入的文件中的行数！更多关于 wc 命令 [**的信息，请点击**](https://en.wikipedia.org/wiki/Wc_(Unix))
*   然后我们打印出文件的行数！

接下来，我们将从 csv 文件中获取列名！为此，我们将在 bash 脚本中使用以下命令。

`colnames=$(head -n 1 < $1)`

这将创建一个变量，其中只包含 csv 的第一行！将所有这些放在一起(以及我添加的一些内容，以便日期自动填充到文本文件中)，我们得到以下脚本:

```
#!/bin/bashecho "Data Processed by Elliott Saslow"DATE=`date +%Y-%m-%d`echo "Date is: "$DATEecho ""echo "The name of the file is:" $1echo ""lines=$(wc -l < $1)echo ""echo "The file has" $lines "lines"echo ""colnames=$(head -n 1 < $1)echo ""echo "Column names are: "echo ""echo $colnames
```

现在，要运行该脚本，我们像上面使用 nano 一样保存它，并在命令行中使用以下命令运行它:

`bash process_data.sh speed.csv > text.txt`

该命令执行以下操作:

*   调用脚本
*   传递我们正在查看的文件(`speed.csv`)
*   将输出传递给一个名为`text.txt`的文本文件

如果您运行了这个程序，并且正确地完成了其他所有工作，那么您的文件夹中就会有一个文本文件，它包含质量控制检查的开始，您可以将它用于您的数据管道！

请在评论中告诉我你在哪里遇到了困难，以及我可以如何帮助你！

干杯