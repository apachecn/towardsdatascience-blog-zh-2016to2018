# 使用蒙特卡洛树搜索你的梦幻足球选秀

> 原文：<https://towardsdatascience.com/using-monte-carlo-tree-search-for-your-fantasy-football-draft-6509b78a1c20?source=collection_archive---------9----------------------->

*本帖中的代码也可作为* [*Jupyter 笔记本*](https://github.com/ykeuter/ffl/blob/master/notebooks/mcts.ipynb) *。*

离下一个美式足球赛季开始还有两个月，这意味着世界各地的梦幻足球运动员正在为他们即将到来的联赛选秀做准备。在这篇文章中，我们将使用蒙特卡罗树搜索算法来优化我们在一个典型的蛇草案中的下一个选择。如果你不熟悉梦幻足球或者蛇选秀，你可以在 [ESPN 的梦幻足球 101](http://www.espn.com/fantasy/football/story/_/id/19522393/fantasy-football-espn-fantasy-football-101) 找到很好的介绍。我们将看到一个由十支球队组成的标准 PPR 得分联盟，但是这些设置应该很容易改变以适应你自己的联盟。我们将主要关注草案逻辑的 Python 3 实现，将算法的细节留给文本中的参考，同时仍然提供代码。

![](img/20b4384dfc420d57c02249b06009b8a2.png)

Photo by [Karri Terra](https://pixy.org/author/Karri_Terra/) on [Pixy](https://pixy.org/)

## 草案

选秀是你的联赛赛季的开始。每个竞争者挑选真实世界的足球运动员来组成他的初始名单。糟糕的选秀真的会毁了你的赛季，所以很多研究都花在为你的球队挑选合适的球员上。在大多数联赛中，选择是按照蛇的顺序依次进行的(1 比 10，然后 10 比 1，等等)。).这意味着在正确的时间填补正确的位置是你战略的重要组成部分。如果你的大部分竞争对手都是从挑选外接球手开始的，你应该从众还是逆势而为？这正是我们想用算法回答的问题。

但首先是逻辑草案。我们需要一个对象来描述我们草稿的确切状态。在每个回合中，我们需要知道到目前为止每个花名册选择了哪些足球运动员，哪些足球运动员仍然可用(自由球员)，接下来回合的顺序是什么，以及哪个竞争者最后移动(因此算法可以从该竞争者的角度评估最后移动)。

```
class DraftState:
    def __init__(self, rosters, turns, freeagents, playerjm=None):
        self.rosters = rosters
        self.freeagents = freeagents
        self.turns = turns
        self.playerJustMoved = playerjm
```

让我们看看如何准确地评估一个草案状态。我们的名单包括一名四分卫(QB)，两名跑卫(RB)，两名外接员(WR)，一名紧逼端(TE)，一名灵活端(RB，WR 或 TE)，一名防守端(D)，一名踢球者(K)和七名替补。首先，让我们假设我们可以根据每个足球运动员的赛季总幻想点数来评估他的价值。然后，我们可以根据一些权衡来评估阵容中的每个位置。重量是至关重要的，因为我们不能在赛季的每一周都使用相同的球员阵容，因为赛季期间的周、伤病或战略决策。

根据一些实验，下面的重量看起来还可以。

由于草案的逻辑都是关于将足球运动员转移到联盟的名单上，所以包含一个`NflPlayer`类也是有意义的。

```
class NflPlayer:
    def __init__(self, name, team, position, points):
        self.name = name
        self.team = team
        self.position = position
        self.points = points
```

现在可以通过将每个球员(从第一个选秀权开始向下移动)映射到其位置的最高可用权重来评估名册。具体来说，如果出现平局，我们将降低灵活头寸的优先级。如果最后的任何权重没有映射到名册上的球员，则相应位置的前三名自由球员的平均值将映射到该权重——有点像对该位置进行分流。代码如下所示。

```
import numpy as npdef GetResult(self, playerjm):
    """ Get the game result from the viewpoint of playerjm.
    """
    if playerjm is None: return 0

    pos_wgts = {
        ("QB"): [.6, .4],
        ("WR"): [.7, .7, .4, .2],
        ("RB"): [.7, .7, .4, .2],
        ("TE"): [.6, .4],
        ("RB", "WR", "TE"): [.6, .4],
        ("D"): [.6, .3, .1],
        ("K"): [.5, .2, .2, .1]
    } result = 0
    # map the drafted players to the weights
    for p in self.rosters[playerjm]:
        max_wgt, _, max_pos, old_wgts = max(
            ((wgts[0], -len(lineup_pos), lineup_pos, wgts) for lineup_pos, wgts in pos_wgts.items()
                if p.position in lineup_pos),
            default=(0, 0, (), []))
        if max_wgt > 0:
            result += max_wgt * p.points
            old_wgts.pop(0)
            if not old_wgts:
                pos_wgts.pop(max_pos)

    # map the remaining weights to the top three free agents
    for pos, wgts in pos_wgts.items():
        result += np.mean([p.points for p in self.freeagents if p.position in pos][:3]) * sum(wgts) return resultDraftState.GetResult = GetResult
```

注意，这个方法包含了很多假设。最值得注意的是，它纯粹基于季节预测，忽略了诸如(共)方差或(强度)时间表之类的东西。此外，它只是孤立地看待每个花名册的价值，而不是相互比较。这些假设应该在以后研究。

让我们继续下一个选秀逻辑——在给定选秀状态的情况下，找出当前竞争对手的可用移动。理论上，你可以选择任何可用的自由球员，但幸运的是，我们可以大大简化事情。因为我们的估价只是基于每个球员的赛季预测，所以选择一个预测比另一个相同位置的自由球员低的自由球员是没有意义的。这将可用的移动限制为选择一个位置(然后选择该位置上最有价值的自由球员)。此外，由于估值中使用的权重，例如起草三个四分卫是没有意义的(第三个四分卫不会增加任何价值)。实际上，以下每个位置的限制似乎是合理的。

这导致了下面的代码。

```
def GetMoves(self):
    """ Get all possible moves from this state.
    """
    pos_max = {"QB": 2, "WR": 6, "RB": 6, "TE": 2, "D": 2, "K": 1} if len(self.turns) == 0: return [] roster_positions = np.array([p.position for p in  self.rosters[self.turns[0]]], dtype=str)
    moves = [pos for pos, max_ in pos_max.items() if np.sum(roster_positions == pos) < max_]
    return movesDraftState.GetMoves = GetMoves
```

最后一部分逻辑涉及到在每次选择后更新草稿状态。代码很简单。

```
def DoMove(self, move):
    """ Update a state by carrying out the given move.
        Must update playerJustMoved.
    """
    player = next(p for p in self.freeagents if p.position == move)
    self.freeagents.remove(player)
    rosterId = self.turns.pop(0)
    self.rosters[rosterId].append(player)
    self.playerJustMoved = rosterId

DraftState.DoMove = DoMove
```

最后，我们的算法需要一个`Clone`方法，这样多个模拟运行就不会相互干扰。

```
def Clone(self):
    """ Create a deep clone of this game state.
    """
    rosters = list(map(lambda r: r[:], self.rosters))
    st = DraftState(rosters, self.turns[:], self.freeagents[:],
            self.playerJustMoved)
    return stDraftState.Clone = Clone
```

## 该算法

蒙特卡洛树搜索是一种启发式搜索算法，用于在基于回合的游戏中寻找最佳的下一步棋，在这种游戏中，几乎不可能考虑所有可能的棋步(及其最终结果)。它被成功地用于玩游戏，如国际象棋和扑克，最近是谷歌战胜世界冠军围棋手的一个非常重要的部分。因为我们的蛇形选秀也是回合制的，每回合都有很多可能的走法，所以 MCTS 看起来是击败我们竞争对手的绝佳选择。该算法背后的基本思想是模拟大量的游戏——首先通过选择随机移动，然后通过专注于最有希望的移动来收敛到最佳移动。它平衡了探索(寻找更有前途的步骤)和开发(关注最有前途的步骤)之间的权衡。

我们下面使用的实现来自 [MCTS 研究中心](http://mcts.ai)，在那里你也可以找到对算法的很好的介绍。另一个精彩的介绍请看一下[杰夫·布拉德伯里的博客文章](https://jeffbradberry.com/posts/2015/09/intro-to-monte-carlo-tree-search/)。

```
# This is a very simple implementation of the UCT Monte Carlo Tree Search algorithm in Python 2.7.
# The function UCT(rootstate, itermax, verbose = False) is towards the bottom of the code.
# It aims to have the clearest and simplest possible code, and for the sake of clarity, the code
# is orders of magnitude less efficient than it could be made, particularly by using a 
# state.GetRandomMove() or state.DoRandomRollout() function.
# 
# Written by Peter Cowling, Ed Powley, Daniel Whitehouse (University of York, UK) September 2012.
# 
# Licence is granted to freely use and distribute for any sensible/legal purpose so long as this comment
# remains in any distributed code.
# 
# For more information about Monte Carlo Tree Search check out our web site at [www.mcts.ai](http://www.mcts.ai)from math import *
import randomclass Node:
    """ A node in the game tree. Note wins is always from the viewpoint of playerJustMoved.
        Crashes if state not specified.
    """
    def __init__(self, move = None, parent = None, state = None):
        self.move = move # the move that got us to this node - "None" for the root node
        self.parentNode = parent # "None" for the root node
        self.childNodes = []
        self.wins = 0
        self.visits = 0
        self.untriedMoves = state.GetMoves() # future child nodes
        self.playerJustMoved = state.playerJustMoved # the only part of the state that the Node needs later

    def UCTSelectChild(self):
        """ Use the UCB1 formula to select a child node. Often a constant UCTK is applied so we have
            lambda c: c.wins/c.visits + UCTK * sqrt(2*log(self.visits)/c.visits to vary the amount of
            exploration versus exploitation.
        """
        UCTK = 200
        s = sorted(self.childNodes, key = lambda c: c.wins/c.visits + UCTK * sqrt(2*log(self.visits)/c.visits))[-1]
        return s

    def AddChild(self, m, s):
        """ Remove m from untriedMoves and add a new child node for this move.
            Return the added child node
        """
        n = Node(move = m, parent = self, state = s)
        self.untriedMoves.remove(m)
        self.childNodes.append(n)
        return n

    def Update(self, result):
        """ Update this node - one additional visit and result additional wins. result must be from the viewpoint of playerJustmoved.
        """
        self.visits += 1
        self.wins += resultdef UCT(rootstate, itermax, verbose = False):
    """ Conduct a UCT search for itermax iterations starting from rootstate.
        Return the best move from the rootstate.
    """ rootnode = Node(state = rootstate) for i in range(itermax):
        node = rootnode
        state = rootstate.Clone() # Select
        while node.untriedMoves == [] and node.childNodes != []: # node is fully expanded and non-terminal
            node = node.UCTSelectChild()
            state.DoMove(node.move) # Expand
        if node.untriedMoves != []: # if we can expand (i.e. state/node is non-terminal)
            m = random.choice(node.untriedMoves) 
            state.DoMove(m)
            node = node.AddChild(m,state) # add child and descend tree # Rollout - this can often be made orders of magnitude quicker using a state.GetRandomMove() function
        while state.GetMoves() != []: # while state is non-terminal
            state.DoMove(random.choice(state.GetMoves())) # Backpropagate
        while node != None: # backpropagate from the expanded node and work back to the root node
            node.Update(state.GetResult(node.playerJustMoved)) # state is terminal. Update node with result from POV of node.playerJustMoved
            node = node.parentNode return sorted(rootnode.childNodes, key = lambda c: c.visits)[-1].move # return the move that was most visited
```

从字面上看，这段代码唯一的变化是在`UCTSelectChild`中，我们应用了`UCTK`来更好地权衡勘探和开发。这是必要的，因为原始代码是基于 0(输)和 1(赢)之间的比赛结果，而我们的结果是一个团队的总预测分数，这是完全不同的尺度。值 200 是根据经验设置的。

## 运行代码

我们现在应该有了运行草案的所有组件。唯一缺少的是季节预测。你可以找到许多网站提供这些预测，但我解决了 ESPN 的预测，因为这是我的联盟主办。我创建了一个 [CSV](https://github.com/ykeuter/ffl/blob/master/notebooks/nfl_players.csv) ，对 2018 年的预测如下。

我们可以运行一个完整的草案，其中每个竞争者草案，他的团队使用蒙特卡罗树搜索算法。让我们试一试。

```
import pandas as pdnfl_players = pd.read_csv("nfl_players.csv", index_col=0)
freeagents = [NflPlayer(*p) for p in nfl_players.itertuples(index=False, name=None)]num_competitors = 10
rosters = [[] for _ in range(num_competitors)] # empty rosters to start withnum_rounds = 16
turns = []
# generate turns by snake order
for i in range(num_rounds):
    turns += reversed(range(num_competitors)) if i % 2 else range(num_competitors)

state = DraftState(rosters, turns, freeagents)
iterations = 1000
while state.GetMoves() != []:
    move = UCT(state, iterations)
    print(move, end=".")
    state.DoMove(move)
```

到目前为止，一切顺利。虽然代码相当慢，但一切似乎都运行良好。速度还不是我们关注的焦点，需要对实现做进一步的研究。但是现在我认为结果看起来很有希望，我们可以对算法有一个很好的总体想法。需要注意的一点是，上面的代码每轮使用 1000 次迭代，目前看来还可以，但这也需要进一步研究。

各参赛队的最终结果如下所示。

## 后续步骤

现在我们有了一个应用于梦幻足球选秀的蒙特卡洛搜索树算法的工作版本，我们应该花一些时间来分析性能。我会在我的下一篇文章中尝试一下，所以请关注我的博客…

感谢阅读！请提供反馈。