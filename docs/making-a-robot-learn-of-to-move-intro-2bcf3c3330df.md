# 让机器人学会如何移动—简介

> 原文：<https://towardsdatascience.com/making-a-robot-learn-of-to-move-intro-2bcf3c3330df?source=collection_archive---------3----------------------->

## (或者:人工智能遇到机器人的地方)

![](img/324bf2bd93a494f19946f79597f1724d.png)

ICub, a famous humanoid robot developed at Italian Institute of Technology (IIT), Genova, Italy.

当想到人工智能时，许多人脑海中浮现的流行图像通常是某种类人机器人，可能穿着闪亮优雅的白色身体，正在下棋或做其他事情。在许多著名的案例中，科幻故事将人工智能描述为体现在机器人中，或者更广泛地说，体现在合成体中。这允许人工智能与物理世界互动，移动，执行动作，等等。这也意味着机器人**知道如何像人类一样灵巧地移动**。

在这一系列的帖子中，我将简要地谈谈我在过去一个月中一直在从事的一个项目:**如何使用机器学习/人工智能技术来教会一个机器人操纵器移动**，尽可能接近地遵循一个期望的轨迹。

在学术文献中，机器人和人工智能通常被视为两个不同的主题，这是有道理的。**机器人**，在最经典的意义上，处理建模和控制机器人机械手，或移动机器人。在工业环境中，他们经常需要做特定的重复性工作，而且需要非常高的精度。设计实现这些任务的控制架构并不简单，但这些机器人很少被视为智能的，只能完成预编程的任务。另一方面，人工智能是计算机科学的一个分支，研究如何设计模拟智能行为的算法。智能有多种定义，但在本主题中，它通常指基于传感器输入和经验采取行动的能力，以及处理不确定性和意外事件的能力。难怪最近这两个科学领域在有时被称为智能机器人的领域找到了交集。

> 精心设计的机器学习算法可以通过*试错*的方式学习移动。

正如我在引言中简要提到的，机器人机械手通常由精确设计的控制算法移动，这些算法基于对机械手的结构和物理参数的准确了解。这些包括关节之间的角度、链接的长度、质量、摩擦力等等。这些参数给得越精确，控制结构的设计就越精确。但是如果不知道这些参数会发生什么呢？这就是机器学习(ML)的由来。ML 允许计算机直接从数据中学习，并且直观地理解为什么它在这种情况下是有用的。就像人类婴儿一样，机器人**可以通过移动**来学习如何移动。一个精心设计的机器学习算法可以学习以*试错*的方式移动，进行多次尝试并分析其动作的效果。在我们的例子中，动作是施加在关节上的扭矩，效果是这些关节如何运动。

机器人的动力学模型，即描述输入如何影响每个关节运动的数学模型，可能非常复杂，尤其是在处理具有多个自由度(DOF)的机器人时。链接通常是动态耦合的:一个链接的移动、速度和加速度以复杂的方式影响另一个*。*有*惯性效应、离心、科里奥利和重力效应、非线性摩擦*等等。所有这些都很难用数学建模。那么，问题来了:**机器人能学会移动吗**？在接下来的一系列文章中，我将展示我和一位同事一起开发的不同方法、技术和算法。你可以在 GitHub 库中找到为这个项目开发的所有代码。

[*现在读第一部分。*](https://medium.com/@normandipalo/making-a-robot-learn-how-to-move-part-1-evolutionary-algorithms-340f239c9cd2)