# 一个数据科学家的干净代码(？)

> 原文：<https://towardsdatascience.com/clean-code-for-a-data-scientist-f4b760374b74?source=collection_archive---------10----------------------->

![](img/5ef25d113bbe57844579c0ee855eb210.png)

Keep it clean!

“我是一名数据科学家..我不需要写干净的代码，因为我的大部分代码都是一次性的”。“干净的代码和敏捷有利于软件开发..这在我的作品中毫无意义”。我听到上面这些话的次数&甚至不愿意尝试一些关于干净代码的建议，让我困惑。

让我告诉你..你也不需要为软件开发写干净的代码。你也不需要为软件开发实践敏捷。即使没有上述内容，你也可以制作一个完美的软件(维护/修改/扩展会变得困难。但这不是本文的重点)。当你在团队中工作时，你需要遵循干净的代码实践！不管你是在开发一个软件还是一个算法，或者必须尝试多种算法。

干净代码的基本思想是你的团队成员应该能够理解你写的东西。这在数据科学中尤其重要。作为一名科学家，你的实验必须是可重复的。必须可以验证。这意味着你团队中的其他人应该能够理解&重现你的结果。

我们存在于一个团队中。一个人做数据科学家是不可能的。在工业界，大多数时候你会从事应用科学的工作。这意味着你必须理解其他人的问题&他们(团队、业务人员)也需要理解你的解决方案。

作为一名数据科学家，您将与团队的以下成员(不限于)频繁互动:

其他数据科学家/数据分析师(此处可互换使用)—在算法、模型、结果解释、特征工程等方面与其他数据科学家协作。

数据工程师——你不想最终自己处理所有的数据。数据工程师可以优化查询，在最短的时间内获得综合结果。他们需要了解你需要什么数据。

业务分析师(BA)/领域专家—从业务角度来看，您的结果意味着什么？BAs 需要了解您的输入变量意味着什么&您的结果将对业务产生什么影响。从本质上理解这个领域。

既然我们已经确定了协作对于数据科学家的重要性，那么让我们来谈谈通过代码使协作变得有效的方法。这些是唾手可得的果实:

1.**有意义的名字**——你的团队成员不应该问你变量是什么意思。使用能清楚表达意图的名称。名字长是可以的。解决这个问题的方法是使用更好的编辑器(这样你就不必每次都输入完整的变量名)&不要缩短变量名从而使它变得晦涩难懂。还有，**请不要神奇的数字**。

2.**避免心理映射**——你知道你的代码中的哪些参数需要改变以获得想要的结果。其他人没有。如果你必须记住所有需要同时改变的变量，那么你就是在浪费团队的大量时间。有人(包括你)会忘记改变其中一个变量而运行代码。这意味着运行是一种浪费&代码需要重新运行。考虑到一些算法需要的时间，这应该是一个**号**号

3.难道**T5 对同一个东西有**多个名字**。一旦你们团队决定了，就要坚持下去。**

4.使用**商业域名**名称。保持你的创造力来设计特征、分析、创建模型。不要发明名字。

5.**评论**很快就过时了。代码总是说真话。因此，通常不鼓励评论。这同样适用于数据科学代码。但是，我会在这里破例—添加一些评论，表明您使用/不使用某些型号/功能的**意图**。可以评论:“这将需要 5 个小时”或“使用径向基核 SVM 以加快收敛”

6.模块化你的代码。理解一个大斑点并不容易。不管它现在在你脑子里有多好。在以后的时间里，你会花时间去理解它。其他人需要时间来理解它。为什么？因为人们必须仔细阅读每一行代码才能理解它在做什么。当你创建的函数正如它的名字所说的那样，你不需要遍历它里面的每一行。读者仍然会理解正在做什么。

7.**不要重复自己！不要复制和粘贴那些代码行，因为你在别的地方也需要完全相同的东西。在一个函数中提取出来&重用那个函数。这确保了一致性。**

8.单元测试 —是的，它们与数据科学代码非常相关。数据科学代码还是代码！测试您的函数，这些函数根据将要采取的业务决策产生数字，这变得越来越重要。为什么你甚至没有想到测试你的函数会有这么多的反响。你所有的功能都应该被测试，以确保它们做你期望它们做的事情。写一个识别平稳信号的函数？用一个稳定的信号来测试它，确保你的函数返回这样的信号。

9.**格式** —决定你们团队想要遵循的格式。这包括在缩进、目录结构、文件命名约定、输出结果格式等方面使用空格还是制表符。

我完全赞成以一种快速而肮脏的方式尝试各种事物/方法/模式。这对于理解一个人应该在哪个方向投入多少时间非常有效。但是，一旦你最终确定了你的方法，并将其融入到你的工作代码中，一定要清理干净。以上所有的概念都有助于人们作为一个团队工作。我发现它很有用，即使我独自工作。它帮助我在以后的某个时间点理解我的工作。它让我能够解释我的结果&让我能够快速地交叉询问令人惊讶(或震惊)的结果。

《干净的代码》一书很好地解释了上面提到的所有观点。所以，如果你是一名数据科学家&将在一个团队中工作，请阅读&实现干净的代码:)每个科学家都有自己的规则。但是在团队工作的时候，团队是有规则的。