# 带有 Keras + OpenAI 的强化学习:DQNs

> 原文：<https://towardsdatascience.com/reinforcement-learning-w-keras-openai-dqns-1eed3a5338c?source=collection_archive---------0----------------------->

**快速回顾**

上次在我们的 Keras/OpenAI 教程中，我们讨论了一个将深度学习应用于强化学习上下文的非常基本的例子。回想起来，这真是一场不可思议的演出！如果你查看训练数据，随机机会模型通常只能执行中值 60 步。然而，通过对这些看起来非常普通的数据进行训练，我们能够“击败”环境(即获得> 200 步的性能)。这怎么可能呢？

对此我们可以有一个直观的感受。让我们想象一下我们用作训练数据的完全随机序列。任何两个序列都极不可能彼此高度重叠，因为它们完全是随机生成的。然而，有一些**关键特征在成功的试验之间是共同的，例如当杆子向右倾斜时，推车向右推，反之亦然。因此，通过在所有这些试验数据上训练我们的神经网络，我们提取了促成它们成功的共有模式，并能够消除导致它们独立失败的细节。**

也就是说，我们本周考虑的环境比上周要困难得多:登山车。

**更复杂的环境**

尽管看起来我们应该能够应用与上周应用的技术相同的技术，但这里有一个关键特征使得这样做不可能:我们不能生成训练数据。与非常简单的横翻筋斗的例子不同，采取随机的动作通常只会导致我们在山脚下结束试验。也就是说，我们有几个试验结果都是一样的，最后都是 200。这对于用作训练数据实际上是无用的。想象一下，你在一个班级里，不管你在试卷上写了什么答案，你都得了零分！你将如何从这些经历中学习？

![](img/c06b5dedd75d15a67c90a1df638891c1.png)

Random inputs for the “MountainCar-v0” environment does not produce any output that is worthwhile or useful to train on

与此一致，我们必须找到一种方法来逐步改善以前的试验。为此，我们使用一个最基本的强化学习的垫脚石:Q-learning！

**DQN 理论背景**

Q-learning(顺便说一下，它不代表任何东西)的核心是创建一个“虚拟表”，说明在给定环境的当前状态下，每个可能的行为会获得多少奖励。让我们一步一步地分解它:

![](img/df2fae5885f57264063fc38c9a98033a.png)

You can imagine the DQN network as internally maintaining a spreadsheet of the values of each of the possible actions that can be taken given the current environment state

我们所说的“虚拟表”是什么意思想象一下，对于输入空间的每一种可能的配置，您有一个表，为您可以采取的每一种可能的行动分配一个分数。如果这是神奇的可能，那么对你来说“战胜”环境将是极其容易的:简单地选择得分最高的行动！关于这个分数有两点需要注意。首先，这个分数通常被称为“Q 分数”，这就是整个算法名称的来源。第二，与任何其他分数一样，这些 Q 分数在它们的模拟环境之外没有意义。也就是说，它们没有**的绝对意义**，但这完全没问题，因为我们只需要它来做比较。

那么，为什么我们需要虚拟表用于*的每个*输入配置呢？为什么我们不能用一张桌子来统治所有人呢？原因是这样做没有意义:这就等于说，在谷底时采取的最佳行动正是你站在左斜坡最高点时应该采取的行动。

现在，我所描述的(为每个输入配置维护一个虚拟表)的主要问题是这是不可能的:我们有一个连续(无限)的输入空间！我们可以通过离散化输入空间来解决这个问题，但这似乎是一个非常粗糙的解决方案，我们在未来的情况下会反复遇到这个问题。那么，我们如何解决这个问题呢？通过将神经网络应用于这种情况:这就是 DQN 的 D 的来源！

**DQN 代理**

因此，我们现在已经把问题简化为找到一种方法，在给定当前状态的情况下，给不同的动作分配 Q 分数。当使用任何神经网络时，这是一个非常自然的第一个问题的答案:我们的模型的输入和输出是什么？对于这个模型，您需要理解的数学范围是下面的等式(不要担心，我们会分解它):

![](img/a7b5c1da724108a7f5bc77314ed1316a.png)

如前所述，q 代表给定当前状态(s)和所采取的行动(a)时由我们的模型估计的值。然而，目标是确定一个状态的总值。我这么说是什么意思？*的总体*值是**，既是**你将获得的即时回报**又是**你在该职位上未来将获得的预期回报。也就是说，我们要考虑一个事实，即一个职位的价值往往不仅反映其眼前的收益，还反映其所能实现的未来收益(该死，深)。在任何情况下，我们都会对未来的回报进行贴现，因为如果我比较两种情况，在这两种情况中，我期望得到 100 美元，其中一种是在未来，我将总是选择目前的交易，因为未来的交易在我做交易和我收到钱之间可能会发生变化。伽马因子反映了该州预期未来回报的折余值。

就是这样:这就是我们需要的所有数学知识！是时候开始写代码了！

**DQN 代理实现**

深度 Q 网络围绕着持续学习，这意味着我们不会简单地积累一堆试验/训练数据并将其输入模型。相反，我们通过运行试验来创建训练数据，并在运行试验后直接将这些信息输入其中。如果这一切现在看起来有些模糊，不要担心:是时候看看关于这一点的一些代码了。代码主要围绕着定义一个 DQN 类，算法的所有逻辑都将在这个类中实现，并且我们为实际的训练公开一组简单的函数。

**DQN 超参数**

首先，我们将讨论一些与 dqn 相关的参数。它们中的大多数是大多数神经网络实现的标准:

```
class DQN:
    def __init__(self, env):
        self.env     = env
        self.memory  = deque(maxlen=2000)

        self.gamma = 0.95
        self.epsilon = 1.0
        self.epsilon_min = 0.01
        self.epsilon_decay = 0.995
        self.learning_rate = 0.01
```

让我们一步一步地看完这些。第一个是简单的环境，当我们需要在创建模型时引用形状时，我们提供它是为了方便。“记忆”是 DQNs 的关键组成部分:如前所述，试验用于持续训练模型。然而，我们不是在试验出现时进行训练，而是将它们添加到记忆中，并在记忆的随机样本上进行训练。为什么要这样做，而不只是将最近的 *x* 次试验作为我们的“样本”进行训练原因有些微妙。想象一下，我们只是将最近的试验作为样本进行训练:在这种情况下，我们的结果将只学习它最近的行动，这可能与未来的预测没有直接关系。特别是在这种环境下，如果我们沿着斜坡的右侧向下移动，那么对最近试验的训练将需要对您向右上坡时的数据进行训练。但是，这与确定在你即将面临的爬上左山坡的场景中采取什么行动完全没有关系。因此，通过采取随机样本，我们不会偏向我们的训练集，而是理想地了解如何同样好地扩展我们会遇到的所有环境。

因此，我们现在讨论模型的超参数:伽马、ε/ε衰减和学习速率。第一个是未来回报折旧因素(<1) discussed in the earlier equation, and the last is the standard learning rate parameter, so I won’t discuss that here. The second, however, is an interesting facet of RL that deserves a moment to discuss. In any sort of learning experience, we always have the choice between exploration vs. exploitation. This isn’t limited to computer science or academics: we do this on a day to day basis!

Consider the restaurants in your local neighborhood. When was the last time you went to a new one? Probably a long time ago. That corresponds to your shift from *探索*到*开发*):你不去寻找新的更好的机会，而是满足于你在过去的经历中找到的最好的机会，并从那里最大化你的效用。相比之下，当你搬进你的房子时:那时，你不知道什么餐馆是好的或不好的，所以被诱惑去探索你的选择。换句话说，学习有一个明显的趋势:在你没有意识到的时候探索你所有的选择，一旦你对其中一些有了看法，就逐渐转向利用。同样，我们希望我们的模型能够捕捉到这种自然的学习模式，而 epsilon 就扮演了这一角色。

ε表示我们将致力于探索的时间比例。也就是说，试验的一小部分*selfε*,我们将简单地采取随机行动，而不是我们预测在那种情况下最好的行动。如前所述，在我们对该问题形成稳定的估值之前，我们希望在开始时更多地这样做，因此在开始时将ε初始化为接近 1.0，并在每个连续的时间步将其衰减某个分数< 1。

**DQN 车型**

在上面的 DQN 初始化中，有一个关键的东西被排除在外:用于预测的实际模型！就像在我们最初的 Keras RL 教程中一样，我们直接以数字向量的形式给出输入和输出。因此，在我们的网络中，除了完全连接的层之外，没有必要采用更复杂的层。具体来说，我们将模型定义为:

```
def create_model(self):
        model   = Sequential()
        state_shape  = self.env.observation_space.shape
        model.add(Dense(24, input_dim=state_shape[0], 
            activation="relu"))
        model.add(Dense(48, activation="relu"))
        model.add(Dense(24, activation="relu"))
        model.add(Dense(self.env.action_space.n))
        model.compile(loss="mean_squared_error",
            optimizer=Adam(lr=self.learning_rate))
        return model
```

并使用它来定义模型和目标模型(解释如下):

```
def __init__(self, env):
        self.env     = env
        self.memory  = deque(maxlen=2000)

        self.gamma = 0.95
        self.epsilon = 1.0
        self.epsilon_min = 0.01
        self.epsilon_decay = 0.995
        self.learning_rate = 0.01
        self.tau = .05 self.model = self.create_model()
        # "hack" implemented by DeepMind to improve convergence
        self.target_model = self.create_model()
```

事实上，有两个*独立的*模型，一个用于预测，一个用于跟踪“目标值”,这绝对是违反直觉的。明确地说，模型( *self.model* )的角色是对要采取的行动进行实际预测，而目标模型( *self.target_model* )跟踪我们*希望*我们的模型采取的行动。

为什么不采用一种模式来实现这两种功能呢？毕竟，如果有东西在预测要采取的行动，难道不应该隐含地决定我们*想要*我们的模型采取什么模型吗？这实际上是 DeepMind 在深度学习中开发的那些“奇怪的技巧”之一，用于在 DQN 算法中实现收敛。如果你使用一个单一的模型，它可以(并且经常)在简单的环境中收敛(比如横竿)。但是，它在这些更复杂的环境中不收敛的原因是因为我们如何训练模型:如前所述，我们正在“动态地”训练它

因此，我们在每个时间步进行训练，如果我们使用单一网络，也将在每个时间步改变“目标”。想想那会有多混乱！这就像一个老师告诉你去完成 pg。6，当你读完一半的时候，她把它改成了 pg。当你完成一半的时候，她让你做 pg。21!因此，这由于缺乏使用优化器的明确方向而导致缺乏收敛，即梯度变化太快而无法稳定收敛。因此，作为补偿，我们有一个变化较慢的网络来跟踪我们的最终目标，并努力实现这些目标。

**DQN 培训**

训练包括三个主要步骤:记忆、学习和重新定位目标。第一个基本上只是随着我们经历更多的试验而增加记忆:

```
def remember(self, state, action, reward, new_state, done):
        self.memory.append([state, action, reward, new_state, done])
```

这里没有什么值得注意的，除了我们必须存储 *done* 阶段，以便稍后更新奖励函数。转到 DQN 的主体，我们有火车功能。这是我们利用我们储存的记忆，并从我们过去看到的东西中积极学习的地方。我们首先从整个内存中抽取一个样本。从那以后，我们对每个样品进行不同的处理。正如我们在前面的等式中看到的，我们希望将 *Q* 函数更新为**当前奖励**和预期未来奖励(按 gamma 折旧)之和。在我们处于试验末期的情况下，没有这样的未来奖励，所以这种状态的全部价值只是我们当前收到的奖励。然而，在非终结状态下，我们希望看到如果我们能够采取任何可能的行动，我们将获得的最大回报是什么，从中我们得到:

```
def replay(self):
        batch_size = 32
        if len(self.memory) < batch_size: 
            return samples = random.sample(self.memory, batch_size)
        for sample in samples:
            state, action, reward, new_state, done = sample
            target = self.target_model.predict(state)
            if done:
                target[0][action] = reward
            else:
                Q_future = max(
                    self.target_model.predict(new_state)[0])
                target[0][action] = reward + Q_future * self.gamma
            self.model.fit(state, target, epochs=1, verbose=0)
```

最后，我们必须重新定位我们的目标，简单地将权重从主模型复制到目标模型中。然而，与主训练方法不同，此目标更新的调用频率较低:

```
def target_train(self):
        weights = self.model.get_weights()
        target_weights = self.target_model.get_weights()
        for i in range(len(target_weights)):
            target_weights[i] = weights[i]
        self.target_model.set_weights(target_weights)
```

**DQN 行动**

最后一步是简单地让 DQN 实际执行期望的动作，该动作基于给定的ε参数在采取随机动作和基于过去训练的动作之间交替，如下所示:

```
def act(self, state):
        self.epsilon *= self.epsilon_decay
        self.epsilon = max(self.epsilon_min, self.epsilon)
        if np.random.random() < self.epsilon:
            return self.env.action_space.sample()
        return np.argmax(self.model.predict(state)[0])
```

**培训代理**

现在，我们开发的复杂代理自然会对代理进行培训。我们必须实例化它，将我们遇到的经验输入它，训练代理，并更新目标网络:

```
def main():
    env     = gym.make("MountainCar-v0")
    gamma   = 0.9
    epsilon = .95 trials  = 100
    trial_len = 500 updateTargetNetwork = 1000
    dqn_agent = DQN(env=env)
    steps = []
    for trial in range(trials):
        cur_state = env.reset().reshape(1,2)
        for step in range(trial_len):
            action = dqn_agent.act(cur_state)
            env.render()
            new_state, reward, done, _ = env.step(action) reward = reward if not done else -20
            print(reward)
            new_state = new_state.reshape(1,2)
            dqn_agent.remember(cur_state, action, 
                reward, new_state, done)

            dqn_agent.replay()
            dqn_agent.target_train() cur_state = new_state
            if done:
                break
        if step >= 199:
            print("Failed to complete trial")
        else:
            print("Completed in {} trials".format(trial))
            break
```

**完整代码**

这里是使用 DQN 在“MountainCar-v0”环境下进行训练的完整代码！

敬请关注下一期 Keras+OpenAI 教程！

# 评论并点击下面❤️以示支持！