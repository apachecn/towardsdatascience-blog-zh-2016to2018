# Jupyter 笔记本的版本控制

> 原文：<https://towardsdatascience.com/version-control-for-jupyter-notebook-3e6cef13392d?source=collection_archive---------1----------------------->

![](img/52e23d1cc644d0fd4f498672a5939f56.png)

[Credit](https://pixabay.com/photos/illustration-red-panda-the-zoo-2246804/)

Jonathan Whitmore 的 Jupyter 笔记本数据科学最佳实践非常有用，我一直强烈鼓励团队成员至少采用其中的一部分，特别是 post-save 挂钩和笔记本命名约定。

[](http://jupyter.org/) [## Jupyter 项目

### Jupyter 笔记本是一个基于网络的交互式计算平台。该笔记本结合了现场代码，方程式…

jupyter.org](http://jupyter.org/) 

## 保存后挂钩

这样做的目的是在保存笔记本时自动保存一份 Python 脚本和 HTML 格式的副本。

我曾经在提交到版本控制之前清除笔记本中的所有输出，以使变更历史更加清晰，但是这产生了两个问题:

1.  如果笔记本的数据处理部分需要 10 分钟以上的时间来运行，*这将会产生一种强烈的激励，让审查者跳过实际运行笔记本*，而只是通读代码，并假设它做了它看起来要做的事情。这是假设训练好的模型以某种方式被序列化以供审阅者加载，否则可能需要花费数小时在审阅者的系统上训练。预训练模型的分布带来了下一个问题:
2.  没有简单的方法将数据和模型文件与笔记本一起分发。对于数据科学家来说，存储和传输千兆字节的数据不是那么容易的，而*让数据工程师专门为此在内部构建一些东西可能不划算*。对于“大数据”场景，您不希望将 Spark 访问凭据保存在笔记本电脑中(非常危险)，因此您需要找到一些安全管理集群访问的方法。(有一个叫做[数据版本控制](https://dataversioncontrol.com/)的新框架，看起来很有前途。但你需要向谷歌或亚马逊支付云存储费用。)

总而言之，解决这些问题最直接的方法就是**只需将笔记本的输出纳入版本控制**。(尽管我建议在提交之前重启内核并重新运行整个笔记本。)

将笔记本转换为 Python 脚本和 HTML 文件。在评审过程中使用 Python 脚本进行代码差异。HTML 文件最适合作为跨团队沟通电子邮件的附件。(就我个人而言，只有当项目的积极参与者没有真正使用 Jupyter 时，我才会将 HTML 文件添加到版本控制中。)

以下要点基本上是从 [Jupyter 笔记本数据科学最佳实践](https://svds.com/jupyter-notebook-best-practices-for-data-science/)中直接复制的。它是根据最新的 Jupyter 测试的，放在这里供快速参考:

使用上述脚本有两种方式:

1.  放在`~/.jupyter/jupyter_notebook_config.py`的顶部。(如果还没有，运行`jupyter notebook --generate-config`创建它)
2.  如果你想要不同项目的不同配置，使用`export JUPYTER_CONFIG_DIR=~/.jupyter_profile2`来指定配置文件夹。
3.  将其作为`jupyter_notebook_config.py`放在项目文件夹的根目录下(在那里运行`jupyter notebook`命令)。

## 还有呢！

如果使用得当，Jupyter notebook 可以成为一个非常有效的工具，提高团队协作的效率。请查看 [Jupyter 笔记本数据科学最佳实践](https://svds.com/jupyter-notebook-best-practices-for-data-science/)了解更多信息。

[](https://svds.com/jupyter-notebook-best-practices-for-data-science/) [## Jupyter 笔记本电脑数据科学最佳实践-硅谷数据科学

### Jupyter 笔记本是一个神奇的工具，可以用在许多不同的方面。因为它的灵活性，工作…

svds.com](https://svds.com/jupyter-notebook-best-practices-for-data-science/) 

## 20190313 更新

Github 上有一个新工具可以直接区分 Jupyter 笔记本:

[](https://www.reviewnb.com/) [## 点评 nb:GitHub 的 Jupyter 笔记本差异

### Jupyter 笔记本的 HTML 渲染差异。告别凌乱的 JSON diff。通过审阅评论在笔记本上协作…

www.reviewnb.com](https://www.reviewnb.com/) 

我还没有找到时间真正尝试它，但我听说过它的好处: