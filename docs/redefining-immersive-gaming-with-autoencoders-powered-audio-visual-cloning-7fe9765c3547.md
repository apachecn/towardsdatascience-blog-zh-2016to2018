# 利用自动编码器支持的视听克隆重新定义沉浸式游戏

> 原文：<https://towardsdatascience.com/redefining-immersive-gaming-with-autoencoders-powered-audio-visual-cloning-7fe9765c3547?source=collection_archive---------8----------------------->

![](img/e5fad03030f57bb8446e5a64c4cb5a3c.png)

My virtual clone appearing in a post-match interview in FIFA 17.

享受电脑游戏的一个重要方面是成为游戏及其故事线的一部分的感觉。沉浸感对激发情绪非常重要，越是感受到这些情绪，玩游戏就越有乐趣。虽然目前的游戏提供了某种程度的沉浸感，具有出色的视觉和声音效果，但这仍然与你自己置身于游戏环境中不一样。这就是我相信在游戏中添加一个你自己的视听克隆作为一个可玩的角色将会把沉浸式游戏带到一个新的水平。一旦你在游戏中与你的这个克隆人建立了联系，故事作者就有可能激发你的各种情绪，从而带来更好的游戏体验。

让自己参与游戏的想法并不新鲜。许多游戏提供了广泛的角色定制，让你控制角色的外观和行为，但这项技术在使角色与你相似的程度方面非常有限。然而，通过深度学习，有可能在这种角色定制的音频和视频方面都有所改善。带着这个想法，我开始创建一个概念，看看当前的深度学习技术如何帮助我们实现这一目标。我试图在奇妙的自动编码器网络的帮助下，在 FIFA 17 的游戏中创建我的音频和视频克隆。

## 视觉克隆

![](img/584ba65ac513e38ec2a2624a7699103b.png)

在我之前的[文章](/using-deep-learning-to-improve-fifa-18-graphics-529ec44ea37e?source=activity---post_recommended_rollup) / [视频](https://www.youtube.com/watch?v=_CW9JVre_t8)中，我展示了视觉克隆是如何利用编码-解码网络实现的，以及它们在生成照片般逼真的人脸方面有多好。我不会再讨论这一部分，所以请随意查看前面提到的文章了解更多细节。我已经用同样的技术在后续的结果中生成了我的脸。

## 音频克隆

深度神经网络的序列到序列学习已被证明在文本到语音转换和机器翻译等任务中非常成功。因此，我想探索在任何书面形式的文本中使用这种技术来创造我的声音的可能性。在我寻找创造我的音频克隆的过程中，我遇到了 [Lyrebird](http://lyrebird.ai) 。

![](img/26cddd2f5e3592c2deb598fc29c66f68.png)

[lyrebird.ai](https://lyrebird.ai/)

这是一个网络应用程序，可以从几分钟的音频中学习你的声音。我录下了我的声音，口述了大约 30 个以书面形式出现的句子，并允许模型进行训练。几分钟之内，网络就可以从我输入的任何文本中生成我的声音。结果虽然不完美，但确实让我吃惊。你可以在我下面嵌入的视频中找到它们。

## 音频克隆-引擎盖下

虽然我不知道 Lyrebird 的技术是如何工作的，但我怀疑它正在使用自动编码器将文本转换为语音，类似于其他克隆算法的工作方式(例如，[百度的 Deep Voice 3](https://github.com/andabi/deep-voice-conversion) )。

![](img/caf3bef62e655a4acafe001adb7bb3dc.png)

Encoder-Decoder Network for text-to-speech conversion

网络的编码器按顺序将文本句子作为输入，并提取出现在该句子中的音素分布的编码。它充当单词的分类器，生成作为类别的音素的概率分布。这个概率分布是由解码器处理的编码，充当音频合成器。这个合成器是根据你录制的声音进行训练的，所以它能够将任何编码转换成你的声音。相当惊人！

## 将这项技术应用到游戏中

最终，我们想要做的是将这项技术很好地应用到游戏中。我试图在游戏《FIFA 17》中创造一个自己的视听克隆体。詹姆斯·罗德里格斯正在接受赛后采访，我用自动编码器网络创建的我的声音替换了球员的脸和声音。这提供了一个很好的概念验证，并给出了我们如何接近完善虚拟克隆的想法，以及游戏开发者如何在不久的将来实时支持这项技术。

你可以在我的 [YouTube 频道](http://youtube.com/c/DeepGamingAI)上找到结果，视频嵌入在下面。

## 游戏中的其他用例

1.  创建游戏中所有玩家/经理的即时克隆，而无需预先录制过场动画。目前，游戏 FIFA 18 只有精选的几个面孔和声音，因为没有从机器上生成它们的技术。这项技术将使游戏开发者很容易在游戏中拥有真实的面孔和声音。
2.  国际足联动态评论。这意味着游戏中的评论员不再年复一年地重复句子。开发者可以简单地改变文本，生成马丁·泰勒和艾伦·史密斯的声音，为游戏的每一次迭代提供新的解说。
3.  在像《孤岛惊魂》这样的游戏中扮演对手。我希望看到我的克隆人扮演对手。不得不做出选择是杀死我的角色还是让它活着，这给了我各种各样的想法，可能是伟大的故事线。

## 结论

虽然这项技术还需要几年时间才能完善或投入生产，但我相信在未来几年内我们可以让它在游戏中实时运行。深度学习在游戏行业的应用是巨大的，机器学习是这个行业的未来，是每个游戏发行商都应该大力投资的事情。