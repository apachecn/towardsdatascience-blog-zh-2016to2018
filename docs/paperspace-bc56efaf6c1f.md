# 如何使用 Paperspace 来训练你的深层神经网络

> 原文：<https://towardsdatascience.com/paperspace-bc56efaf6c1f?source=collection_archive---------18----------------------->

![](img/712f52d1bf96676b871c85682ee3498c.png)

[rawpixel](https://unsplash.com/@rawpixel?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

计算资源是深度学习应用的主要瓶颈之一。这不仅适用于业务用例，也适用于自学。GPU 是一项昂贵的投资，尤其是当你只在某些时候需要它们的时候。云服务为这个问题提供了解决方案:**只在你需要的时候租用资源**。这篇博文向你展示了如何开始使用这些服务中最用户友好的:Paperspace。

> **免责声明:**我没有从 Paperspace 获得任何形式的赞助或报酬。当我在 fast.ai 上开始为程序员开设 [*深度学习*](http://course.fast.ai/) 课程时，我偶然发现了他们，并一直呆在那里。我还决定省去推荐链接，以保持分离的干净。

云服务提供商有很多，Azure、AWS 和 Google Cloud 是其中的三大领导者。那么为什么要使用 Paperspace 呢？我更喜欢它们，因为它们在机器学习方面有清晰和专注的用户体验。特别是对于三大市场领导者来说，机器学习是复杂机器的一个齿轮，涵盖了人们可以从云服务中期望的一切。这种复杂程度符合拥有建立和维护所有依赖关系的资源的公司和其他组织的需要。然而，Paperspace 在运行所需的时间上击败了巨头。我还发现监控我的账单更容易了。

在这篇文章中，我将向你展示如何立即开始使用 Paperspace。明确地说，Paperspace 提供的产品比我在这里描述的更多。如果你想更多地了解他们的产品组合，可以去他们的网站上查看完整的产品列表。

在我开始之前，有一个一般性的评论:**在本地机器上尽可能多地测试你的代码**。使用云服务需要花钱，当你专注于一项具有挑战性的任务时，很容易忘记这一点。

![](img/6d738dadcdc36271f38b7e4e8458e84d.png)

[YIFEI CHEN](https://unsplash.com/@imchenyf?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

首先，你必须[注册](https://www.paperspace.com/account/signup)这项服务。这里有一个提示:学习 fast.ai 课程的学生会得到一个促销代码，价值 15 美元。这相当于大约 30 个小时的 GPU 使用时间，所以我建议你开始学习 fast.ai 的优秀课程，并获得额外的好处。

注册后，您就可以访问您的控制台。有三种产品可供选择:渐变、核心和 API。对于快速访问 GPU 资源，我更喜欢**渐变**。更确切地说，我用笔记本来做这件事。

当你点击 ***创建笔记本+*** 时，会有一系列免费的容器供你选择。如有必要，您还可以配置新的容器。对于快速分析或实验， **Jupyter 笔记本数据科学堆栈**是完美的选择。你可以在这里找到规格。它允许您同时使用笔记本电脑和控制台，因此通过额外的软件包来扩展功能是没有问题的。

在创建笔记本实例的第二步中，您必须决定要使用的机器。例如，我使用最便宜的 CPU 选项来下载数据，并对我的深度学习模型进行测试。这个选项叫做 C2，每小时只需 0.009 美元。**也就是 4 天半的计算能力，1 美元**。

由于集成存储，从廉价的 CPU 解决方案开始是可能的。有 **200GB 的永久存储**可用，它通过`storage`文件夹自动连接到每个新实例。这种配置可以让你先用便宜的 CPU 准备好一切。

一旦你确定一切正常，你就可以开始一个新的 JupyterHub 环境。这一次，我选择了 GPU 选项。最便宜的选项从每小时 0.51 美元开始，一直涨到每小时 1.72 美元。也有更强大的机器可用(最高成本为每小时 20.99 美元)，但你需要升级你的帐户才能做到这一点。我使用 Paperspace 来支持我在深度学习领域的个人发展。为此，0.51 美元的期权总是足够了。

对结果满意后，您应该**将您使用的笔记本**(原始文件和 HTML 副本)**和训练好的模型**导出到您选择的备份位置或版本控制系统。

![](img/de3d0da9f9d488cfdb72936b2740975e.png)

[rawpixel](https://unsplash.com/@rawpixel?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

这些信息已经足够开始您的 Paperspace 之旅。我算了一下上一个深度学习项目的成本。总之，我花了大约三美元(大部分花在模特培训上)。然而，更重要的是，**我花了 99%的时间解决实际问题**，只有 1%的时间用于设置。

**感谢阅读！如果你喜欢这篇文章，留下一些吧👏🏻并在 LinkedIn 或 Twitter 上分享。请在评论和** [**推特**](https://twitter.com/TimoBohm) **上告诉我你的想法。再次感谢，继续学习！**