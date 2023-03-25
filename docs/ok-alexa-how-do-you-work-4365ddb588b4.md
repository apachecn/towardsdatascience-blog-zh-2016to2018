# 好的，Alexa…你是怎么工作的？

> 原文：<https://towardsdatascience.com/ok-alexa-how-do-you-work-4365ddb588b4?source=collection_archive---------2----------------------->

![](img/39e4af6ce002eeabcd029eeac510818d.png)

> “我有许多由许多人制造的组件。”—亚马逊回声
> 
> “我可以帮你做很多事情。例如，你可以说这样的话……要查看更多示例，请查看 Google Home 应用程序。”—谷歌主页

这不是我想要的答案。

## 好的，杰瑞，声控扬声器是如何工作的？

我最近偶然发现了 YouTube 上风险投资家 a16z ( [安德里森·霍洛维茨](https://a16z.com/))的频道，并立即被他们的视频内容吸引住了，尤其是他们的动画短片[系列](https://www.youtube.com/playlist?list=PLM4u6XbiXf5rcgvD82RkfxIBy1jsn_C_6)。在我的最爱中，普通合伙人[维贾伊·潘德](https://twitter.com/vijaypande)、[本尼迪克特·埃文斯](https://twitter.com/BenedictEvans)和[克里斯·迪克森](https://twitter.com/cdixon)分别将机器学习、S 曲线和创新者困境的复杂概念，分解成易于消化、易于理解的 5 分钟视频。

受到他们例子的启发，我决定创作自己的动画短片，尽可能清晰简洁地总结我的热情和研究领域——语音计算。是动画 TL；我写的文章的博士，如果你愿意的话。利用类似的手绘视觉效果和彩色动画，我想创造一个简短的抓住本质

1.  为什么语音是下一个大的平台转移
2.  以语音为中心的设备如何工作
3.  我们如何衡量音频传感器的质量，以及如何增强方向性
4.  亚马逊 Echo 和谷歌 Home 中已经包含的定向麦克风还有待改进

语音计算已经在我们中间了。事实上，百度首席科学家吴恩达预测，到 2020 年，50%的搜索将基于语音或图像。同样，Juniper Research 的一份报告发现，到同一年，语音智能扬声器将进入 55%的美国家庭。随着我们一点点接近那个未来，我很高兴看到初创公司继续 1)为亚马逊 Echo 和谷歌 Home 生态系统开发服务，2)寻找在多个行业使用语音的新机会，以及 3)建立支持语音的基础设施——为日益增长的语音技术热潮提供支持。

如果您喜欢该视频，请点赞、评论并分享。我很想听听你对你希望我深入研究的其他领域的看法。

此外，我强烈推荐去看看并订阅 a16z 的 YouTube 频道。除了短片之外，它还提供了很棒的内容，如他们一年一度的 a16z 峰会的视频记录和 [Frank Chen](https://twitter.com/withfries2) 对所有前沿技术的特殊[走查](https://www.youtube.com/playlist?list=PLM4u6XbiXf5pIWcUyOX3WkAb5Gv5kKwMY)。