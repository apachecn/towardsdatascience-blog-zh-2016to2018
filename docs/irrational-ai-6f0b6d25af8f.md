# 非理性人工智能

> 原文：<https://towardsdatascience.com/irrational-ai-6f0b6d25af8f?source=collection_archive---------10----------------------->

机器是理性的，人类是非理性的，对吧？不对。人类和机器都是非理性的，原因惊人地相似。在这篇博客中，我会告诉你为什么，以及为什么它不一定是一件坏事。

**首先，我说的理性是什么意思？**

基于数据的逻辑决策是理性决策。理想情况下，为了做出最合理的决定，你需要一个完美的数据集。一个完整、准确和公正的数据集。您希望逻辑地处理这些数据，用每条新数据更新每个结果的概率。

**那么，为什么人类会有偏见呢？**

![](img/92c67b652f19b1894df9a2356bd761f7.png)

我们人类的大脑进化了几千年，来吃饭、睡觉、繁殖和保护自己免受威胁，偶尔需要使用一些更高级的推理来从别人那里偷食物并保护自己的食物不被抢走。社会、语言和复杂的发明在我们的进化中是最近的事情，期望我们的大脑如此快速地适应抽象或高度数学化的思想来做出理性的决定是不公平的。

做出合理的决策需要准确和完整的数据集，以及纯粹的数学决策。我们的大脑无法做到这一点。为了利用他们可用的处理能力，并且在我们的行动所要求的时间限制内，我们已经进化到进行大量的近似——这就是我们发现偏差的地方。

即使我们有一个完美的数据集，人类也很难做出理性的决定。想象一下，你进行了一次广谱血液检测，你的医生告诉你，你的一种非常罕见的疾病检测呈阳性，这种疾病的发病率为 1/1000。测试本身有 97%的准确率。那么，你得这种病的可能性有多大？

许多人会立即跳到 97%的测试准确率，并认为这是 97%的可能性。事实上，这种可能性只有 3%。原因是它实际上只影响 1/1000 人，如果你测试 1000 人，你会期望 3%的人得到假阳性，因为测试是 3%不准确的。因此，我们将有 30 个测试呈阳性，但实际上只有一个人患有这种疾病，因此，在一次测试后，你患这种疾病的可能性大约为 3%。

Eliezer Yudkowski 举了一个很好的例子来说明时间限制是如何发挥作用的——他谈到了一只老虎。当我们看到老虎时，我们不会想‘嗯，那个动物是黄色的，有条纹的。其他黄色和条纹的生物在过去被描述为老虎，我被告知老虎有很高的概率“啊啊啊啊啊啊啊啊”。相反，我们看到黄色的闪光，粗略的形状粗略的形状，我们的大脑完成了模式，我们跑了。

正如埃利泽所说，偏见不是我们在纯粹理性的头脑上加上的东西，它是我们整个决策过程。

**那么，为什么 AI 是非理性的？**

理论上，人工智能具有理性决策的潜力。在人脑有我们可能永远无法克服的硬件限制的地方，给计算机增加更多的处理能力是相对容易的。一旦我们发现了正确的算法来做出决定(我并不是说人类擅长做出公平公正的算法，但科学方法应该会给我们带来一些希望)，我们就可以确信自动化系统将永远遵循这些指令和规则，每次都不会失败。

然而实际上，人工智能不太可能是理性的。理性所需要的完整、准确和公正的数据集？它不存在。巨大的处理能力，可以根据该数据集或我们能想到的最佳数据集进行接收和决策？它是昂贵的。我们实现任何接近理性人工智能或自动化系统的唯一方式是通过蛮力，而蛮力将是缓慢且非常昂贵的。

企业首先受利润驱动。如果人工智能或自动化正在被考虑取代历史上由人工操作的流程，那么采用该技术的商业案例将从根本上关乎它如何提高利润。即使是决心做出理性和无偏见的人工智能的善意的人，也必须说服他们的同行，整理出一个看起来很可能类似于“消费者在道德产品上的支出正在增加，通过确保我们的自动化促进平等(说真的，阅读弗吉尼亚·尤班克斯的《自动化不平等》)我们可以增加销售和利润”的商业案例。如果我发现世界上有些企业愿意违法，我不会感到惊讶，因为他们的违法行为使他们获得的利润超过了罚款。无论哪种方式，对盈利能力的争论只会增加一个理性的或无偏见的产品成功的机会。

如果我们开始超越更标准的自动化，走向人工通用智能，我们知道我们面前的项目将是富有挑战性和昂贵的。一个经常被提出的通往 AGI 的路径依赖于“加速回报法则”你从建立一个狭窄的智能开始。在某项特定任务中，可能比人做得更好或至少更划算的东西。使用它带来的力量——无论是狭义人工智能能够进行的推理类型，还是你可以从中获得的资源，来构建第二个更高级的人工智能。冲洗和重复，直到你有自己的 AGI。这里的问题是，我们将被鼓励在这些阶段做出非理性的人工智能。由于我们在早期阶段受到资源、数据和智能的限制，开发人员将需要走捷径，找到有时优雅、有时不那么优雅的概括问题的方法，以进入下一次迭代。如果我们看到加速回报定律导致人工智能快速而艰难的起飞，那么当我们意识到它不再受我们控制，不是一个真正理性的存在时，可能为时已晚。

一个简单的解决方案是让企业放慢脚步，但这个问题类似于囚徒困境，特别是如果我们开始考虑人工智能的话。对社会最有益的结果是开发安全的、价值一致的人工智能。我可能可以控制我正在开发的系统，但我对我的同行或竞争对手正在做的事情几乎没有控制权。如果我不能信任他们的行为，我可能会放松我的安全标准，决定即使我像我的竞争对手一样偷工减料，我相信我更值得信赖，可能会偷工减料。否则，我可能会采取稍微更具破坏性的方法，假设一个无法控制的 AGI 将导致人类的末日，如果有人比我更近，他们比我更强大，如果我不能打败他们，我也可能加入他们。

我们真的想要一个真正理性的人工智能吗？

如果我们主要着眼于人工通用智能(AGI)和人工超级智能(ASI)，我们会听到围绕价值调整、控制和伦理的对话。所有这些都是远离理性和偏向的一步，这是一件好事。

第一，价值对齐。这里我们的意思是明确地将人工智能的目标和价值观与人类的目标和价值观相一致。我们今天可能无法准确知道这些价值是什么，或者它们是否会在今后的任何时间内保持不变，但它确实积极地推动了对人类所重视的东西的偏见，而不是地球上其他生物所重视的东西，或者对人工智能本身更直接有益的东西。这并不意味着人工智能会忽视宇宙中的所有其他生命，因为人类可能会珍视其他生命(无论我们表现得多么糟糕！).

伦理学也很有趣。我不认为人工智能能够以某种方式发现任何普遍的“先验”伦理。相反，我认为道德是人类创造并赋予其意义的东西，它是一群人对与错的平均标准。由于这个原因，随着我们的需求和优先事项的改变，人们认为道德的东西在整个人类历史中不断发展。

那么，为什么要自动化呢？

仅仅因为这个系统不完美，并不意味着它没有用。让我们不要忘记人类也是有偏见的。如果我们要建立一个比人类快 10 倍的系统，并且减少 95%的偏差输出，那么我们就减少了世界上偏差的数量和比例——我们只需要在这里做一个假设，我们可以盈利；否则我们不可能卖出很多。

同样重要的是，我们要摆脱责备他人的欲望。如果我们今天能够推出一款与人类司机一样安全的无人驾驶汽车，如果在转换的第一天有 3000 人死亡，公众仍会强烈抗议。即使这个数字是 2000 人，即使第一天就有 100 人死亡，人们的反应无疑也会是负面的。事实上，人类每天在道路上造成 3000 多人死亡。如果我们的动机确实是减少错误输出和限制死亡，我们应该自动化。

我们不要忘记不到理性推理所能带来的效用。创造有用的输出，而不是完美的输出，但为时已晚。我们必须有意识地实现自动化，找到我们既能减少有偏差的产出又能提高盈利能力的实例，并在我们被完全推向速度、远离公平而违背自己最佳判断的情况下保持谨慎。