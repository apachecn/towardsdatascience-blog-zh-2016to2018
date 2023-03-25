# 了解卡尔曼滤波器

> 原文：<https://towardsdatascience.com/make-sense-of-kalman-filter-c59fe5f8202f?source=collection_archive---------3----------------------->

*这份材料来自我在 Udacity 自动驾驶汽车项目中获得的知识。特别感谢巴斯蒂安·特龙和奔驰工程师的讲座。*

![](img/3f356d0d0a2ad85c3c85503279ceb93b.png)

Source: [https://www.codeproject.com/Articles/865935/Object-Tracking-Kalman-Filter-with-Ease](https://www.codeproject.com/Articles/865935/Object-Tracking-Kalman-Filter-with-Ease)

# **什么是卡尔曼滤波器？**

长回答:看看这个维基百科[链接](https://en.wikipedia.org/wiki/Kalman_filter)。想研究一下吗？这个[环节](https://in.udacity.com/course/artificial-intelligence-for-robotics--cs373/)。

我的(简短且更普通的用户友好的)回答:
-预测位置和速度，有一些不确定性。
-然后测量实验位置和速度，有些不确定性。
-最后，通过将我们的预测与测量信息相结合，增加我们预测的确定性。

# **好奇部分 1:为什么我们必须预测然后测量和更新？**

确实如此。乍一看，这个过程似乎有点丰富。做出所有的预测，然后回头来衡量我们刚刚预测的东西，这有什么意义呢？直接投入测量，生活就不能更简单吗？

那样生活会简单得多。然而，在我揭示为什么的秘密之前，我必须问你一些事情。

> 想象你在路上开车，少管闲事，尽量保持 30 英里的时速。然后，在你右边车道上的人发出信号让你超车。因为你是个好人(有着美丽的灵魂)，所以你可以接受。这里不谈任何技术问题，在这种情况下，你会怎么做，让那个人安全地走到你前面？
> 
> 如果那是我，我可以用 2 秒钟做一个完整的观察 ***汽车在哪里&相对于我的速度*** *它的速度是多少。我的直觉可以预测它下一步要做什么。那样的话，我要么减速让道，要么加速让道。取决于我的车和另一辆车的位置，我会根据我的最佳判断让另一辆车在不造成任何事故的情况下超车。*
> 
> 假设那辆车成功地变道到了我的前面，我仍然不断地观察那辆车并调整我的速度，这样我的车就能一直保持在安全区域。如果车开慢了，我预测下一秒车还是会慢，我会在后面保持慢速。但是，如果它突然开得很快，我可以加快一点速度(只要在限速之下)，更新我的信念。我在那里所做的是一个连续的**预测**和**更新**的过程。

所以，如果你以我为例，我想我已经给了你卡尔曼滤波器背后的逻辑。但是卡尔曼滤波器没有被发现适用于人类(或者是？>:)，它已被用于机器上的各种工程应用，因为机器不具备像人或动物那样根据环境变化进行逻辑思维的能力。

# 好奇第二部分:为什么我们要把预测和测量结合起来？难道测量本身还不够精确到可以更新吗？

又来了。认为单独使用传感器信息进行更新是正确的做法是有道理的。然而，测量也会有不确定性(等等…什么？？？).例如，激光雷达用于检测位置，并且它位于汽车外部，可能存在环境影响(雨、雾、灰尘等)，这使得测量不太确定。

不要盲目相信一方是一个好习惯，在某个地方总是有不确定的空间。通过组合信息，我们有更多的信息来降低不确定性/协方差，并给出更可靠的最终输出。

# 结论

传感器就像我们的眼睛，鼻子，耳朵。它们感知事物，并将信息传回大脑进行处理。对于自动驾驶汽车，我们有摄像头，激光雷达和雷达。相机获取图像，使深度学习能够识别物体，甚至预测信息。激光雷达和雷达感测速度和位置，以便机器可以根据卡尔曼滤波器应用做出相应的移动决定。

**希望你喜欢这篇短文。如果你喜欢你所读的，请点击* ❤ *分享。我感谢任何反馈。下次见！**