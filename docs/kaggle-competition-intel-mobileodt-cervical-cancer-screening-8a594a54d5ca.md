# Kaggle 竞赛:英特尔和 MobileODT 宫颈癌筛查

> 原文：<https://towardsdatascience.com/kaggle-competition-intel-mobileodt-cervical-cancer-screening-8a594a54d5ca?source=collection_archive---------1----------------------->

我开始看 Kaggle 比赛来练习我的机器学习技能。当前正在进行的竞赛之一被框架化为图像分类问题。英特尔与 MobileODT 合作，开始了一场 [Kaggle 比赛，以开发一种基于图像识别女性子宫颈类型的算法](https://www.kaggle.com/c/intel-mobileodt-cervical-cancer-screening)。

训练集包含分成三种类型的 1481 幅图像。Kagglers 可以使用 6734 额外的图像。其中一些来自重复的病人。一些附加图像质量较低。比赛的两个阶段的测试集是可用的，kagglers 必须提交一组预测概率，3 个类别中的每一个，测试集的每个图像。总奖金为 100，000 美元。

我试图以一种天真的方式来解决这个问题:只需获得一个预训练的 Inception V3 图像分类模型，并在这个数据集上对其进行微调。

**数据**

Philipp Schmidt 出版了[子宫颈 EDA](https://www.kaggle.com/philschmidt/intel-mobileodt-cervical-cancer-screening/cervix-eda/notebook) 笔记本:研究数据集的基本属性。

我加载了所有带标签的图像，并将它们的大小调整为 224x224 的形状，这在 Inception V3 中使用。以 80/20 的比例混合并分成训练集和开发集。

**型号**

使用 Keras 加载在 ImageNet 数据集上预先训练的 Inception V3 模型和权重。移除了顶部分类层，在顶部添加了一个新的带有 dropout 的密集层和一个 softmax 层。我冻结了所有的初始层，首先训练新的密集层。然后《盗梦空间》的最后两个卷积块被解冻，我也对它们进行了微调。该模型在 80%的标记数据上进行训练，在 20%上进行验证。

**结果**

不太好。验证损失不会低于 0.95。该模型过拟合很快。我在验证集上得到 54.5%的准确率。

我的代码在[这里](https://github.com/surmenok/Kaggle-MobileODT/blob/master/MobileODT_CNN.ipynb)可用。

正如你在关于 Kaggle ( [1](https://www.kaggle.com/c/intel-mobileodt-cervical-cancer-screening/discussion/30401) 、 [2](https://www.kaggle.com/c/intel-mobileodt-cervical-cancer-screening/discussion/30312) 、 [3](https://www.kaggle.com/c/intel-mobileodt-cervical-cancer-screening/discussion/30342) 的讨论中所看到的，一个未经训练的人很难对这些图像进行分类。参见 visoft 的[如何(人工)识别子宫颈类型的简短教程](https://www.kaggle.com/c/intel-mobileodt-cervical-cancer-screening/discussion/30471)。

低画质更难。另一个挑战是数据集的规模较小。

看起来最好的前进方式是将问题一分为二:图像分割以在图像中找到子宫颈，然后进行图像分类。图像分割问题需要人工审查训练样本，以找到包围盒。[光照校正](https://www.kaggle.com/c/intel-mobileodt-cervical-cancer-screening/discussion/31163)可以另当别论去尝试。数据扩充(旋转、翻转)有助于增加训练样本的数量。

在这一步，我想我对处理图像分类问题有了很好的感觉。这个对我来说太难了，我现在无法竞争，计算机视觉也不是我关注的领域。我去查查其他比赛。这个看起来很有趣:[两个适马连接:租赁列表查询](https://www.kaggle.com/c/two-sigma-connect-rental-listing-inquiries)。这是一个分类问题。数据集包括结构化数据、文本和图像。

*文章原载于*[*http://Pavel . surmenok . com/2017/04/09/ka ggle-competition-Intel-mobile ODT-宫颈癌-筛查/*](http://pavel.surmenok.com/2017/04/09/kaggle-competition-intel-mobileodt-cervical-cancer-screening/)