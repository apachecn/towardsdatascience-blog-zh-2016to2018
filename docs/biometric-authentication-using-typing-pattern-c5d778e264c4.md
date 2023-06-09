# 使用打字模式的生物认证

> 原文：<https://towardsdatascience.com/biometric-authentication-using-typing-pattern-c5d778e264c4?source=collection_archive---------1----------------------->

我最近构建了一个应用程序，使用应用于击键生物识别的机器学习算法来演示基于打字模式的身份验证。这里有详细流程的 prezi。

早在 2003 年，我就致力于这个确切的想法，作为我本科最后一年项目的一部分。那是神经网络流行的最初几天，当今世界没有多少先进的工具可用。我们炮制了一个基于 C++前端和基于 Matlab 的神经网络(NN)感知器模型与几个隐藏层的解决方案。我们的方法是训练单个神经网络模型来识别多个个体的分型模式。我们能够达到 90%的准确率——好到足以毕业:-)。

# 使用开源工具进行机器学习

快进 10 年，你会有一些非常强大的开源工具和库来完成捕捉击键生物特征、创建和训练神经网络模型的重任。这个领域本身有了一个全新的名字——机器学习。我目前破解生物认证的方法是基于 Keras 库，Keras 库本身是基于谷歌开源的 TensorFlow 库。前端登录屏幕是基于[登录流](https://github.com/mxstbr/login-flow)由马克斯斯托伊伯基于反应样板。它通过 [Rx.js](https://github.com/ReactiveX/rxjs) 库使用 reactor 模式实现来捕获击键间隔。登录流程也得到了增强，可以利用[电子邮件验证](https://github.com/whitef0x0/node-email-verification)。后端是带有 MongoDB 的 NodeJS 服务器，用于存储用户凭证。 [ZeroRPC](http://www.zerorpc.io/) 用于调用为运行 NN 算法而定制的 python 函数。

# 初速电流状态

这里使用的机器学习算法非常简单。而且，学习是基于在注册过程中提供的单个正样本。在这种约束下，应用程序仍然能够以非常高的准确度预测正确的身份验证。

# 次状态

我在考虑添加一个应用程序的激励系统，让用户有动力打破对方的认证，这反过来又为训练提供了负面的样本。让我用一个例子来解释这一点:

用户集:U1、U2、U3

用户模型的正面训练样本(Px ):

U1: [P11，P12]

U2: [P21，P22]

U3: [P31，P32]

在激励系统中，U1 被要求破解 U2 的认证。这是完全匿名的，U1 不知道他/她正在破坏谁的认证。这给出了 U2 的负样本(Nx)。

U2: [P21，P22，N23]

那么激励在哪里呢？用户破坏他人身份验证的贡献越大，负面样本收集的身份验证弹出窗口就越多。越多的阴性样本意味着认证过程越准确。经过一段时间后，整个系统的精确度会提高。

# 今后

有相当多的公司已经在这个领域做了大量的工作。 [TypingDNA](http://typingdna.com/) 提供先进的击键动力学服务，其准确性令人惊讶。然而，他们确实让你开始输入至少 90 个字符。我建议你看看那里的产品，也试试[的快速演示](http://typingdna.com/demo-api.html)。这是对未来的一瞥。

# 乐观

最后乐观地说，有一天我们都将摆脱记住密码的麻烦，认证将像在纸上签名一样简单。

# **尝试/反馈/分享/重复**

欢迎合作！

欢迎分享！