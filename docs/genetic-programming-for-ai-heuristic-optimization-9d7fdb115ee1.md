# 遗传编程在人工智能启发式优化中的应用

> 原文：<https://towardsdatascience.com/genetic-programming-for-ai-heuristic-optimization-9d7fdb115ee1?source=collection_archive---------7----------------------->

![](img/9ef9b3f7d48cee69124600d8c218f606.png)

Photo by [James Harrison](https://unsplash.com/@jstrippa?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 介绍

我对基因编程的兴趣始于 2015 年，当时我研究了迭代[最后通牒游戏](https://en.wikipedia.org/wiki/Ultimatum_game)。最近，我一直在使用遗传算法来优化工作中风险管理系统的参数。在这篇短文中，我将讨论构建你自己的遗传算法的高级思想和必要成分。我还将给出一个简单的遗传算法的实现，该算法使用 alpha beta 剪枝和迭代深化的 minimax 来优化一般游戏 AI 代理的启发式函数。最后，我将讨论人工智能中遗传编程的几个缺点。

# 这是什么？

遗传编程(GP)是一种进化算法，可以计算人类不知道如何直接解决的一般问题的解决方案。机器的任务是从问题的高级实现中生成一个工作的计算机程序。这个想法是随机生成数以千计的计算机程序，并使用达尔文自然选择来进化这些程序，直到种群收敛到全局最大值/最小值。它通常用于机器学习领域，以确定数据中特征之间的关系。GP 还被应用于各种金融领域:建立自动交易策略、风险管理系统和信用卡检测。其他感兴趣的领域包括量子计算、电路和天线的设计。我最近还听说遗传算法被用来调试大规模程序中的代码。

# 遗传程序的组成部分

所有的进化算法都必须有一个繁殖、变异和进化的机制。此外，如果您希望使用真正的遗传编程，您必须定义参数的遗传表示。例如，浮点数据类型的简单表示是二进制字符串表示。然后，人们可以将进化应用于字符串的每一位。关于育种，交叉是最流行的方法，通过这种方法，二进制数据的基因组以随机方式组合。定义健壮的繁殖和变异机制对于确保解收敛到全局最大值/最小值是至关重要的。

# 玩游戏的例子

假设我们的任务是构建一个人工智能系统来玩一个普通的游戏(例如国际象棋)。如果我们应用最小最大和阿尔法贝塔剪枝的标准方法，我们的任务将是生成一个启发式函数来确定一个移动的相对“强度”。假设我们选择以下形式的东西:

```
V(w) = w * white_pieces — (1-w)*black_pieces where w in [0,1].
```

遗传编程的任务是为我们的人工智能代理选择参数 w。请注意，在下面的代码中，我将使用我的 AlphaBetaPlayer 类来表示群体中的个体。遗传得分函数被定义为上面的简单试探法。

我们的首要任务是定义一个单独的代理，如下所示:

```
def individual(weight):
    # creates an individual 
    return AlphaBetaPlayer(score_fn=genetic, weight=weight)
```

初始种群只是一组随机权重的个体。我们可以如下实现它:

```
def population(count):
    # creates a list of individuals 
    return [ individual(random.uniform(0,1)) for x in range(count) ]
```

我们现在可以将育种过程定义为在母亲体重和父亲体重之间计算一个随机数，并稍微增加范围以保持更多的遗传多样性。我们不允许母亲和父亲是同一个人。

```
def breed(mother, father):
    if mother != father:   
        if father.w > mother.w:
            childWeight = random.uniform(mother.w*.95, father.w*1.05)
        else:
            childWeight = random.uniform(father.w*.95, mother.w*1.05)
        if childWeight < 0:
            childWeight = 0
        if childWeight > 1:
            childWeight = 1
        child = individual(childWeight)
        return child
    else:
        print('Cannot breed with itself: Error: Mother == Father')
```

请记住，这里选择的育种过程是完全随意的，可以有不同的定义。比如我们可以考虑取母亲和父亲的算术平均值甚至几何平均值。你应该尽你所知去模仿达尔文进化论。再次重申我上面的评论，最健壮的方法是使用某种类型的二进制字符串表示，并对基因型应用真交换。在这个简短的例子中不讨论这个问题。我们以显而易见的方式调整琐碎的边界情况。

我们的下一个任务是定义突变过程。一个简单的实现是将权重向上或向下移动一个随机的量。我们做了一些更聪明的事情来确保基因多样性。

```
def mutate_agent(agent):
    if agent.w < 0.5:
        newWeight = (1-agent.w) + random.uniform(-0.5, 0.1)
    else:
        newWeight = (1-agent.w) + random.uniform(-0.1, 0.5) 
    if newWeight < 0:
        newWeight = 0
    if newWeight > 1:
        newWeight = 1
    mutated_agent = individual(newWeight)
    return mutated_agent
```

特别是，我们将概率分布转换为移动权重，这些权重要么接近 0，要么接近 1，分别接近 1 或 0。同样，我们以明显的方式调整琐碎的边界情况。

我们最后的任务是结合以上的功能来进化当前的种群。我们的方法包括让代理在一定数量的游戏中互相竞争，然后选择前 20%的玩家在下一轮进化中使用。我们还随机选择一些表现较差的代理，以确保遗传多样性。我们有效地杀死了剩下的特工。最后，我们使用我们的育种过程创建孩子，并使用我们上面讨论的突变函数突变 5%的人口。函数中的参数定义了要进行的游戏的数量、要保留的双亲的百分比、表现最差者的随机选择大小和变异率。

```
def evolve(pop, gamesFactor=2, retain=0.2, random_select=0.05, mutate=0.01):
    # Determine the parents to breed from the population
    agent_score = {}
    numGames = len(pop) * gamesFactor
    bar = progressbar.ProgressBar()for game in bar(range(numGames)):
        competitors = random.sample(pop, 2)
        game = Board(competitors[0], competitors[1])
        winner, history, outcome = game.play()
        competitors.remove(winner)
        loser = competitors[0]
        if winner not in agent_score.keys():
            agent_score[winner] = 1
        else:
            agent_score[winner] += 1
        if loser not in agent_score.keys():
            agent_score[loser] = -1
        else:
            agent_score[loser] -= 1        

    top_performers_size = int(retain * len(pop))
    bottom_performers_size = len(pop) - top_performers_size
    rand_select_size = int(len(pop) * random_select)
    top_perfomers = heapq.nlargest(top_performers_size, agent_score, key=agent_score.get)
    bottom_performers = heapq.nsmallest(bottom_performers_size, agent_score, key=agent_score.get)
    parents = top_perfomers + random.sample(bottom_performers, rand_select_size)
    random.shuffle(parents)# Create children
    numChildren = len(pop) - len(parents)

    children = []
    for i in range(numChildren):
        par = random.sample(parents, 2)
        father = par[0]
        mother = par[1] 
        child = breed(mother, father)
        children.append(child)

    new_pop = parents + childrenmutated_pop = []
    # Randomly mutate some of the new population
    for agent in new_pop:
        if mutate > random.uniform(0,1):
            print('Mutate')
            mutated_agent = mutate_agent(agent)
            mutated_pop.append(mutated_agent)
        else:
            mutated_pop.append(agent)
    return mutated_pop
```

将所有东西结合到我们的主程序中，我们可以不断地进化种群，直到我们收敛到一个全局最大值/最小值。这个模拟花了 17 个小时在我的机器上运行。

```
if __name__ == "__main__":
    pop_count = 100
    evolution_cyles = 12
    pop = population(pop_count)
    history = []
    for i in range(evolution_cyles):
        print(i)
        pop = evolve(pop, gamesFactor=10, retain=0.2, random_select=0.05, mutate=0.05)
        best_weights = [i.w for i in pop]
        print(stats.describe(best_weights))
        history.append(best_weights)

    print('Evolution Results:')
    [stats.describe(x) for x in history]
```

在一天结束的时候，你会对观察经过适当数量的进化周期后的最终种群感兴趣。描述性统计应该有一个小的标准差和一个紧密的四分位数范围。你应该看到所有的代理都有相似的权重。对于下棋代理，遗传算法给出的最优权重约为 0.3452。

# 遗传编程的缺点

遗传编程的一个简单缺点是计算成本。在达到收敛之前，需要进化许多种群。另一个缺点是解决方案不能保证收敛到全局最大值/最小值。通常也很难确定育种和突变程序。最后，大量的参数通常需要大量的进化周期。玩游戏的启发式函数通常被设计来平衡复杂性和对游戏的洞察力。将启发式算法设计得尽可能简单通常是很重要的。出于这个原因，遗传编程对于玩人工智能游戏的代理人来说非常有效。

# 参考

[1][https://Zhang GW . WordPress . com/2009/11/08/a-simple-genetic-programming-in-python-4/](https://zhanggw.wordpress.com/2009/11/08/a-simple-genetic-programming-in-python-4/)

[2][https://lethain . com/genetic-algorithms-cool-name-damn-simple/](https://lethain.com/genetic-algorithms-cool-name-damn-simple/)

[3]https://en.wikipedia.org/wiki/Genetic_programming

[http://www.genetic-programming.org/](http://www.genetic-programming.org/)

[5]本·兰德在 2015 年夏天