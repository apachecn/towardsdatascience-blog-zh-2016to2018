# 机器学习中的 Python 迭代器介绍

> 原文：<https://towardsdatascience.com/introduction-to-iterators-and-generators-in-python-for-data-science-and-machine-learning-bce75e136360?source=collection_archive---------8----------------------->

![](img/e998308e96d5e46b4ac318616c380bd9.png)

[Ricardo Gomez Angel](https://unsplash.com/@ripato?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

从事机器学习和人工智能的人非常依赖 Python 中的迭代器(和生成器)。它们是语言的核心概念，在机器学习包中使用很多，如 *scikit-learn* 或 *Keras* 。理解它们是如何工作的以及为什么 Pythonistas 们使用它们会给你带来两个好处:

1.  你可以更有效地使用它们。
2.  你可以更快地理解与他们相关的问题。这种理解将为您节省大量的代码编写和调试时间。

我将分两步给你必要的背景知识。首先，我向您介绍一些基本概念，包括一些简单的例子。其次，我将向您介绍来自 *scikit-learn* 的两个示例性实现。在这一步中，我将向您展示源代码的简化版本。这种方法将让您了解开发人员如何将这些抽象概念转化为现实生活中的工具。

# 迭代器的一般概念

## 什么是迭代器？

迭代器是 Python 对象，一次返回一个元素。每次你向迭代器请求下一个项时，它都会调用它的`__next__`方法。如果有另一个值可用，迭代器将返回它。如果没有，它会引发一个`StopIteration`异常。

这种行为(只在被要求时返回下一个元素)有两个主要优点:

1.  迭代器需要更少的内存空间。他们记住了上一个值和到达下一个值的规则，而不是记住一个(可能很长的)序列的每一个元素。
2.  迭代器不检查它们产生的序列有多长。例如，他们不需要知道一个文件有多少行或者一个文件夹中有多少个文件来遍历它们。

重要的一点是:不要混淆迭代器和可迭代对象。Iterables 是可以通过使用它们的`__iter__`方法创建迭代器的对象。这就是 Python 在 for-loops 中做的事情:

```
for classifier in ["XGBoost", "Logistic Regression", "SVM"]:
    if classifier == "XGBoost":
        print(f"{classifier} is the best! Maybe.")
    else:
        print(f"{classifier} is fun. Somehow.")
```

在内部，Python 调用在`in`之后提交的任何东西的`__iter__`方法。然后在 while 循环中对创建的迭代器使用`next`，直到引发`StopIteration`异常。你可以在这里找到完整的解释和[这里](http://treyhunner.com/2016/12/python-iterator-protocol-how-for-loops-work/)。在 Python 中可以构建多种类型的 for 循环的原因是它们有一个实现的`__iter__`方法。

Iterables 很方便，但是它们没有“纯”迭代器的优点。例如，你转换成迭代器的链表已经占用了内存。如果可以的话，尽量直接使用迭代器而不是可迭代的。

## 什么是发电机？

生成器是构建迭代器的一种简化方式。不是实现*迭代协议*，生成器是用`yield`语句返回值的函数。`yield`和`return`的区别在于，生成器会跟踪它们的局部变量。每次生成器到达`yield`时，都会返回值。然后，它会记住函数中变量的当前状态，并等待下一次调用。

# 机器学习包中的生成器

我想向您展示两个例子在 *scikit-learn* 中的实现。不过在 *Keras* 或者 *Tensorflow* 中也有例子。如果你想加深理解，我鼓励你去寻找它们。

## 示例 1: KFold

机器学习的主要挑战之一是避免过度拟合。许多算法都有超参数或修改来解决这个问题，但最重要的概念是交叉验证。对于一个很好的概述，我推荐这篇文章:

[](/train-test-split-and-cross-validation-in-python-80b61beca4b6) [## 在 Python 中训练/测试分割和交叉验证

### 大家好！在我上一篇关于 Python 中线性回归的文章之后，我认为写一篇文章是很自然的…

towardsdatascience.com](/train-test-split-and-cross-validation-in-python-80b61beca4b6) 

在这里，我们正在寻找 K 倍交叉验证。其想法是将数据集分割成多个折叠，并使用每个折叠一次来测试在剩余折叠上训练的模型。例如，如果您使用三个折叠，模型将在其中两个上训练，并使用一个进行测试。

这里使用生成器的原因是，您不必一开始就记住所有的索引组合。知道在相应的回合中测试哪些指数就足够了。其余的将用于培训。

让我们看一下[scikit-learn 的`KFold` 的源代码](https://github.com/scikit-learn/scikit-learn/blob/f0ab589f/sklearn/model_selection/_split.py#L357)(第 357 到 434 行)来看看生成器是如何实现的:

```
class KFold(...):
    def __init__(...):
        ...
    def _iter_test_indices(...):
        ...
        current = 0
        for fold_size in fold_sizes:
            start, stop = current, current + fold_size
            yield indices[start:stop]
            current = stop
```

靠近类定义的末尾有一个`yield`语句，表示一个生成器函数。方法的名称`_iter_test_indices` 表明生成器只产生测试文件夹的索引。正如我之前所写的，训练将在所有其他指数上进行，因此没有必要显式计算它们。

第一次调用生成器时，它从索引 0 ( `current=0`)开始，加上折叠的大小，得到这个折叠的最后一个索引。然后产生这些指数。下一次，在运行另一轮 for 循环之前，它查找它的局部变量并将`current`设置为来自上一次迭代的`stop`的值。

## 示例 2:梯度增强分类器

梯度提升背后的基本思想是组合足够多的弱学习者来获得强模型。与并行训练许多学习者的 bagging 方法(如 random forest)相反，boosting 以迭代方式工作。它训练一个弱学习者，评估它的主要弱点，并试图在下一轮中弥补它们。

这种算法的迭代特性迫切需要生成器。让我们[检查一下`GradientBoostingClassifier`的源代码](https://github.com/scikit-learn/scikit-learn/blob/121dd5a/sklearn/ensemble/gradient_boosting.py#L1693)，更具体地说是第 1693 到 2156 行。为了简单起见，我们只关注`GradientBoostingClassfier`类中的一个特定生成器(第 2151 行):

```
def staged_predict_proba(...):
    ...
    for score in self._staged_decision_function(X):
          yield self.loss_._score_to_proba(score)
```

我在这里省略了很多代码，但基本思想保持不变。梯度增强阶段是迭代过程中的一个步骤。这个生成器在每一步都监视测试集上的错误。这里使用生成器的原因是，您不必定义该模型的特定实例中有多少个阶段。

# 摘要

当然，迭代器、可迭代对象和生成器还有很多。我最喜欢的关于迭代器和其他 Python 对象之间关系的博文是 Datacamp 的这篇:

[](https://www.datacamp.com/community/tutorials/python-iterator-tutorial) [## Python 迭代器教程

### 迭代器是 Python 无处不在的灵魂。它们无处不在，你一定在某个节目中见过它们…

www.datacamp.com](https://www.datacamp.com/community/tutorials/python-iterator-tutorial) 

我希望这篇博文能帮助你掌握基本思想。我还希望源代码示例能够缩小基本概念和实际实现之间的差距。

**感谢阅读！如果你喜欢这篇文章，留下一些吧👏🏻并分享一下！让我知道你在评论和** [**推特**](https://twitter.com/TimoBohm) **上的想法。我也很乐意连接上**[**LinkedIn**](https://www.linkedin.com/in/timo-boehm-datascience/)**。再次感谢，继续学习！**