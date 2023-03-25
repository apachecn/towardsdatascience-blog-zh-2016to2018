# 假新闻分类器(使用 LSTMs)

> 原文：<https://towardsdatascience.com/fake-news-classifier-e061b339ad6c?source=collection_archive---------3----------------------->

![](img/f16161f2784a3a6d5f2de1a7755370d9.png)

本课是 DeepSchool.io 提供的课程的一部分，我们使用深度学习(递归神经网络)将一组文章分为‘假’和‘真’新闻类。

参见下面的视频或在[https://github.com/sachinruk/deepschool.io](https://github.com/sachinruk/deepschool.io)(第 18 课)获得的代码，了解所用模型的解释。

我们拥有的数据集是一组围绕 2016 年美国大选期间撰写的新闻文章。在第二个模型中，我们利用预先训练好的手套向量来进行迁移学习。如果不使用这些方法，就存在过度拟合的实际风险，如第一个被训练的模型所示。

真的是在学习什么是假新闻吗？不会。如果一个好记者写的内容和假新闻一样，他很可能会把假新闻归类为真新闻。请记住，模型不知道世界的实际状态。事实上，“特朗普总统是有史以来最伟大的总统。”根据我的模型(wtf)，有 73%的概率是一篇真实的新闻文章。

这可能是学习功能，如希拉里被提到了多少次，或者 CNN(电视网络，而不是神经网络类型)被提到了太多。也许网络会询问作者是否来自“信息战争”(由极右翼疯子/精神病患者/讨厌同性恋青蛙的人经营)。

请看[这里](https://blog.kjamistan.com/comparing-scikit-learn-text-classifiers-on-a-fake-news-dataset/)关于如何在 scikit learn 中使用 TfIDf 解决相同问题集的博文。它实际上在 93%的准确率上做得更好(RNN 模型大约是 87%的准确率)。虽然 scikit learn 确实比深度学习表现得更好，但重要的是要理解，你必须在“正常”的机器学习方面进行特征工程。

# 外卖食品

总之，深度学习是避开机器学习的特征工程方面的一个很好的方法。与单词袋方法相比，我们可以通过使用 RNN 来保持句子结构。手套向量的使用是减少我们必须训练的参数数量的重要一步，因此减少了过度拟合的风险。

如果你喜欢我的教程/博客帖子，**可以考虑在[https://www.patreon.com/deepschoolio](https://www.patreon.com/deepschoolio)支持我**，或者订阅我的 YouTube 频道[https://www.youtube.com/user/sachinabey](https://www.youtube.com/user/sachinabey)(或者两者都支持！).**哦还有拍手！:)**