# 决策树——鸟瞰图和实现

> 原文：<https://towardsdatascience.com/decision-trees-a-birds-eye-view-and-an-implementation-c91754f0dcd0?source=collection_archive---------6----------------------->

*本文正被移至我的* [*子栈发布*](https://vishalramesh.substack.com/) *。这里* *可以免费阅读文章* [*。这篇文章将于 2022 年 5 月 18 日被删除。*](https://vishalramesh.substack.com/p/decision-trees-a-birds-eye-view-and-an-implementation-c91754f0dcd0?s=w)

# 这篇文章的目的是什么？

在该数据集中实现了以下内容

*   理解决策树的定义

履行

*   加载数据
*   使用相关矩阵和配对图可视化数据
*   构建决策树分类器
*   使用混淆矩阵确定模型的准确性
*   将决策树可视化为流程图

# 什么是决策树？

> 决策树是一种类似流程图的结构，其中每个内部节点代表对一个属性的“测试”(例如，掷硬币是正面还是反面)，每个分支代表测试的结果，每个叶节点代表一个类标签(在计算所有属性后做出的决定)。
> 
> (来源:[维基百科](https://en.wikipedia.org/wiki/Decision_tree#Overview))

更简单地说，决策树检查一个属性或一组属性是否满足条件，并根据检查结果执行后续检查。该树根据这些检查将数据分成不同的部分。

# 履行

*本文其余部分已移至出版物* [*机器学习——科学、工程和 Ops*](https://vishalramesh.substack.com/) *。你可以在这里* *免费阅读整篇文章* [*。*](https://vishalramesh.substack.com/i/52376063/importing-the-necessary-libraries)