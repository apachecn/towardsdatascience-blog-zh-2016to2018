# 自动化枯燥的任务:企业软件中的聊天机器人

> 原文：<https://towardsdatascience.com/automate-the-boring-task-chatbots-in-enterprise-software-dc83cc8f09d8?source=collection_archive---------6----------------------->

这篇文章结合了我个人网站[这里](http://nahidalam.com/automate-boring-task-1/)和[这里](http://nahidalam.com/automate-boring-task-2/)的两篇博文。

![](img/d9f5ea8de652fa0ecd2c42c74211e08c.png)

由于对任何形式的炒作都持怀疑态度，我对聊天机器人并不感兴趣。在投入另一场硅谷炒作之前，我需要理解它的价值。当我在 2017 年初加入思科超创新生活实验室(CHILL)时，机会来了。思科在企业软件领域与许多其他企业客户合作。这意味着他们拥有庞大的基础设施、工具集和客户支持流程，这些都是自动化的成熟产品。

虽然我不能谈论我在这里工作的项目，但它激起了我对聊天机器人、自然语言处理(NLP)和整体对话人工智能空间的兴趣。虽然之前我已经创建了[基于规则的家庭自动化系统](https://twitter.com/litehouseio)和[语言转换软件](http://www.banglasms.com/)，但这一次是我第一次使用实际的自然语言处理技术涉足企业规模的对话式人工智能。

我开始研究聊天机器人框架。回到 2017 年 5 月，思科收购了一家名为 [MindMeld](https://mindmeld.com/) 的对话式人工智能初创公司。我开始使用 MindMeld NLP APIs 和它的对话式 AI 框架，更深入地研究了诸如 ***实体*** 、 ***意图*** 等概念。我继续深入研究，发现了[rasa . AI](https://rasa.ai/)——另一个企业级对话式人工智能框架。而且这次是开源的！很少有朋友还提到过[chatter bot](http://chatterbot.readthedocs.io/en/stable/)——一个开源的会话式对话引擎。随着我不断探索，我在消费者和企业领域遇到了许多其他对话式人工智能产品。由于我在 Backstage Capital 的 EIR 演出，我还发现了一些机器人分析产品。

在这一系列帖子中，我将深入探讨聊天机器人框架、对话式人工智能产品、机器人分析产品以及更多我感兴趣的产品。

# 在对话式人工智能框架中

![](img/99f7051f1071e8ee0264514f7080d483.png)

为了构建一个聊天机器人，我最终研究了不同的对话机器人框架。 [Botkit](https://www.botkit.ai/) 和 [Api.ai](https://dialogflow.com/) (现在的 Dialogflow)就是两个明显的例子。它们都支持思科的协作平台 Cisco Spark，也可以通过各自的 REST APIs 从 webapp 中使用。但很快我意识到了它们的局限性。

僵尸工具是一个基于规则的框架。在基于规则的框架中，开发人员需要创建交互逻辑——如何解释传入的聊天消息，以及典型的响应集是什么。所以本质上，僵尸工具本身并不提供任何机器学习能力。你必须不断地为简单的应用程序创建规则。这些类型框架适用于快速原型开发，但不适用于大型可伸缩系统。截至 2017 年 5 月，僵尸工具[增加了将僵尸工具工作室与微软自然语言处理引擎 Microsoft LUIS 集成的功能](https://blog.howdy.ai/helping-your-bot-grow-a-brain-4d08d14bec19)。Botkit 和 LUIS 的整合服务于我的目的，即；使用真正的自然语言处理引擎，但这导致了下一个问题——使用基于云的 NLP 服务。

Api.ai，微软 LUIS，亚马逊 Lex 等。是基于云的对话式人工智能 NLP 服务。他们很快就能上手，不用太担心 NLP 或机器学习的细节。但是有几个原因我不想和他们一起去。首先，我不想把我的数据上传到那些云服务上。第二，我希望能够控制我在聊天机器人中使用的自然语言处理技术，最后，我希望能够更多地控制我的机器人的功能，而不是基于云的自然语言处理服务提供的常见功能。

# **那么我在找什么呢**

简而言之，我在聊天机器人框架中寻找以下特征

*   不是基于规则或基于云的 NLP
*   有 web/REST API 支持，所以我可以从 webApp 上使用它，而不仅仅是在第三方平台上，如 Slack，脸书等。
*   支持 Python，但 Node.js 也很酷
*   允许我自定义
*   巨大的优势——开源

MindMeld 服务于上述大部分目的，我在思科的工作中使用它。但我也想看看外面还有什么。我偶然发现了[rasa . ai](https://rasa.ai/)——一个面向企业的开源对话式 AI。Rasa 似乎与 MindMeld 非常相似——尽管我需要更深入地了解 Rasa 的 NLP 功能。Rasa 让我着迷的是，它们是企业级开源解决方案——开发者的天堂！

我还遇到了聊天机器人。它似乎是一个基于规则的引擎，基于这个[聊天机器人平台对照表](https://chatbotsjournal.com/25-chatbot-platforms-a-comparative-table-aeefc932eaff)。虽然 Chatterbot 有能力添加您自己的语料库来训练机器人。需要仔细观察才能决定它的 NLP 能力。

在那之前，敬请期待！

**注:**

我是旧金山的一名软件工程师，从事聊天机器人、自然语言处理方面的工作，最近获得了深度学习奖学金，成为杰瑞米·霍华德 fast.ai 课程的一部分。

这一系列帖子的名字是受了这本书的启发。特别感谢 [Anastasia](https://www.linkedin.com/in/anastasiastarpehellis/) 和 [Meighan](https://www.linkedin.com/in/meighanbyron/) 对这篇博客的快速回顾。感谢 [Karthik](https://www.linkedin.com/in/karthikraghunathan/) 分享他对 MindMeld 平台的深刻见解。