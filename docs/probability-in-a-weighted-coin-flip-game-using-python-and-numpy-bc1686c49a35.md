# 使用 Python 和 Numpy 的加权抛硬币游戏中的概率

> 原文：<https://towardsdatascience.com/probability-in-a-weighted-coin-flip-game-using-python-and-numpy-bc1686c49a35?source=collection_archive---------0----------------------->

本周一个简短有趣的问题:

两个玩家正在玩一个游戏，从玩家 1 开始，他们掷一枚*不一定是*公平的硬币。第一个抛头的人获胜。一枚硬币正面朝上的概率是 *p* 。参与人 1 赢得比赛的概率是多少？

![](img/5e365c56839b51f984f3680ac67227f2.png)

Photo by [Andriyko Podilnyk](https://unsplash.com/@yirage?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

我们可以用 python 中的一个简单函数来探讨这个问题。让我们编写一个接受两个参数的函数:1。)要玩的游戏的数量，以及 2。)抛硬币导致正面的概率(设置为默认值 0.5 或 50%)。

我们姑且称这个函数为“P1 _ 赢 _ 概率 _ 加权 _ 硬币 _ 游戏”

我们可以用下面的代码模拟这个游戏:

```
import numpy as npdef P1_win_prob_weighted_coin_game(num_games, prob_heads=.5): 
  player_one_wins = 0 
  for n in range(0,num_games): 
    num_flips = 0 
    win = 0
    while win == 0: 
      turn = np.random.uniform(0,1) 
      num_flips += 1 
      if turn <= prob_heads: 
        if num_flips % 2 != 0: 
          player_one_wins += 1 
        win += 1 
  return float(player_one_wins)/float(num_games)
```

现在，让我们一步一步地简单介绍一下。

```
import numpy as npdef P1_win_prob_weighted_coin_game(num_games, prob_heads = 0.5): 
  player_one_wins = 0
```

我们从导入 numpy 开始，因为我们可以利用它的随机选择功能来模拟这个游戏的抛硬币机制。

我们定义函数的名称，并指定两个参数。我们将默认正面概率设置为 0.5，以防我们只是想使用公平硬币指定一些游戏来查看结果。显然，我们可以在这里随意输入自定义概率。

我们还为玩家 1 赢得的游戏数量设置了一个全局计数器，每轮模拟从 0 开始。

```
 for n in range(0,num_games): 
      num_flips = 0 
      win = 0
      while win == 0: 
        turn = np.random.uniform(0,1) 
        num_flips += 1
```

现在，对于指定模拟次数(例如，range(0，num_games ))的每场游戏，我们将从零开始计算掷硬币的次数。这既能记录游戏进行了多长时间，也能帮助我们在一次正面朝上的情况下确定谁是赢家。

我们还指定了一个获胜条件“win ”,它将帮助我们知道游戏何时完成。当 win 仍然等于零时，玩家继续掷硬币。每“转”一次构成硬币的一次翻转，它是在 0 和 1 之间随机选择的十进制值。每走一个回合，我们的 num_flips 计数器就加一。

```
 if turn <= prob_heads: 
          if num_flips % 2 != 0: 
            player_one_wins += 1 
          win += 1
```

如果随机选择的“回合”值小于或等于我们设置的“prob_heads”值，我们认为这是一个获胜条件。

我们可以这样理解:如果翻转头部的概率是 0.6，那么 0 和 1 之间的 60%的值可以被解释为头部翻转(例如，0.0 和 0.6 之间的所有值)。

一旦满足获胜条件，我们就检查硬币被抛了多少次。因为参与人 1 总是第一个掷硬币，我们知道奇数次掷硬币意味着参与人 1 是最后一个走的，因此是赢家。如果玩家 1 赢了游戏，我们在函数的顶部给我们的玩家 1 赢计数器加 1。我们还将“win”的值更改为 1.0，这将中断当前游戏，并在预设的模拟次数内开始下一个游戏。

```
 return float(player_one_wins)/float(num_games)
```

一旦所有模拟运行完毕，该函数将返回玩家 1 赢得的游戏数除以所玩游戏的总数。这将表示为十进制的获胜百分比。

好吧！现在我们已经写好了函数，让我们用一枚硬币玩 50，000 次游戏，看看会有什么结果:

```
P1_win_prob_weighted_coin_game(50000)0.66848
```

在超过 50，000 场比赛中，我们看到一号玩家有明显的优势。事实上，玩家 1 有大约 2/3 的机会因为先抛硬币而赢得游戏，即使使用公平硬币。

如果我们调整硬币正面朝上的概率呢？如果硬币有 75%的机会正面朝上呢？

```
P1_win_prob_weighted_coin_game(50000, .75)0.80066
```

在 75%的抛硬币中，一个有重量的硬币朝上，玩家 1 有 80%的机会获胜。

如果我们真的降低头部出现的可能性呢？比方说，如果正面只出现 1%的几率呢？

```
P1_win_prob_weighted_coin_game(50000, .01)0.50498
```

只有当我们大幅度降低正面朝上的可能性时，两个玩家才有大致相同的获胜机会。

有趣的东西！你可以随意摆弄这个函数，看看不同的模拟次数和正面概率如何影响玩家 1 获胜的可能性。

感谢阅读！