# 用动态编程解决问题

> 原文：<https://towardsdatascience.com/solving-problems-with-dynamic-programming-ea4a872dae61?source=collection_archive---------0----------------------->

*此内容最初出现在* [*好奇洞察*](http://www.johnwittenauer.net) 上

动态编程是一种非常有用的解决问题的通用技术，它涉及到将问题分解成更小的重叠子问题，存储从子问题计算出的结果，并在问题的更大块上重用这些结果。动态编程解决方案总是比简单的暴力解决方案更有效。对于包含 [**最优子结构**](https://en.wikipedia.org/wiki/Optimal_substructure) 的问题特别有效。

动态编程以有趣的方式与计算机科学中的许多其他基本概念相关联。例如，递归类似于(但不等同于)动态编程。关键的区别在于，在简单的递归解决方案中，子问题的答案可能会被计算很多次。缓存已经计算过的子问题的答案的递归解决方案被称为 [**记忆化**](https://en.wikipedia.org/wiki/Memoization) ，这基本上是动态编程的逆过程。另一种变化是当子问题实际上根本没有重叠时，在这种情况下，这种技术被称为 [**分治**](https://en.wikipedia.org/wiki/Divide_and_conquer_algorithms) 。最后，动态编程与 [**数学归纳法**](https://en.wikipedia.org/wiki/Mathematical_induction) 的概念联系在一起，可以认为是归纳推理在实践中的具体应用。

虽然动态编程背后的核心思想实际上非常简单，但事实证明，在非平凡的问题上使用它相当具有挑战性，因为如何根据重叠的子问题来构建一个困难的问题通常并不明显。这就是经验和实践派上用场的地方，这也是这篇博文的想法。我们将为几个众所周知的问题构建简单的和“智能的”解决方案，并看看如何使用动态编程解决方案来分解问题。代码是用基本的 python 编写的，没有特殊的依赖性。

## 斐波那契数

首先我们来看一下 [**斐波那契数列**](https://en.wikipedia.org/wiki/Fibonacci_number) 中的数字计算问题。问题的定义非常简单——序列中的每个数字都是序列中前两个数字的和。或者，更正式地说:

*F _ n = F _ n1+F _ N2*，以 *F_0=0* 和 *F_1=1* 作为种子值。

*(注意:Medium 没有能力正确地呈现方程，所以我使用了一个相当简单的解决方案，用斜体显示数学符号……如果真正的意思没有很好地表达出来，我道歉。)*

我们的解决方案将负责计算每个斐波纳契数，直到某个定义的限制。我们将首先实现一个简单的解决方案，从头开始重新计算序列中的每个数字。

```
**def** **fib**(n):  
    **if** n **==** 0:
        **return** 0
    **if** n **==** 1:
        **return** 1 **return** fib(n **-** 1) **+** fib(n **-** 2)**def** **all_fib**(n):  
    fibs **=** []
    **for** i **in** range(n):
        fibs.append(fib(i)) **return** fibs
```

让我们先在一个相当小的数字上试一试。

```
%time print(all_fib(10))[0, 1, 1, 2, 3, 5, 8, 13, 21, 34]
Wall time: 0 ns
```

好吧，可能太琐碎了。让我们试试大一点的…

```
%time print(all_fib(20))[0, 1, 1, 2, 3, 5, 8, 13, 21, 34, 55, 89, 144, 233, 377, 610, 987, 1597, 2584, 4181]
Wall time: 5 ms
```

运行时间现在至少是可测量的，但仍然相当快。让我们再试一次…

```
%time print(all_fib(40))[0, 1, 1, 2, 3, 5, 8, 13, 21, 34, 55, 89, 144, 233, 377, 610, 987, 1597, 2584, 4181, 6765, 10946, 17711, 28657, 46368, 75025, 121393, 196418, 317811, 514229, 832040, 1346269, 2178309, 3524578, 5702887, 9227465, 14930352, 24157817, 39088169, 63245986]
Wall time: 1min 9s
```

升级得很快！显然，这是一个非常糟糕的解决方案。让我们看看应用动态编程时是什么样子的。

```
**def** **all_fib_dp**(n):  
    fibs **=** []
    **for** i **in** range(n):
        **if** i **<** 2:
            fibs.append(i)
        **else**:
            fibs.append(fibs[i **-** 2] **+** fibs[i **-** 1]) **return** fibs
```

这一次，我们保存每次迭代的结果，并计算新的数字，作为先前保存的结果的总和。让我们看看这对函数的性能有什么影响。

```
%time print(all_fib_dp(40))[0, 1, 1, 2, 3, 5, 8, 13, 21, 34, 55, 89, 144, 233, 377, 610, 987, 1597, 2584, 4181, 6765, 10946, 17711, 28657, 46368, 75025, 121393, 196418, 317811, 514229, 832040, 1346269, 2178309, 3524578, 5702887, 9227465, 14930352, 24157817, 39088169, 63245986]
Wall time: 0 ns
```

通过不在每次迭代中计算完整的 recusrive 树，我们基本上已经将前 40 个数字的运行时间从大约 75 秒减少到几乎即时。这也恰好是幼稚递归函数的危险的一个很好的例子。我们新的斐波那契数函数可以计算第一个版本的线性时间与指数时间的附加值。

```
%time print(all_fib_dp(100))[0, 1, 1, 2, 3, 5, 8, 13, 21, 34, 55, 89, 144, 233, 377, 610, 987, 1597, 2584, 4181, 6765, 10946, 17711, 28657, 46368, 75025, 121393, 196418, 317811, 514229, 832040, 1346269, 2178309, 3524578, 5702887, 9227465, 14930352, 24157817, 39088169, 63245986, 102334155, 165580141, 267914296, 433494437, 701408733, 1134903170, 1836311903, 2971215073L, 4807526976L, 7778742049L, 12586269025L, 20365011074L, 32951280099L, 53316291173L, 86267571272L, 139583862445L, 225851433717L, 365435296162L, 591286729879L, 956722026041L, 1548008755920L, 2504730781961L, 4052739537881L, 6557470319842L, 10610209857723L, 17167680177565L, 27777890035288L, 44945570212853L, 72723460248141L, 117669030460994L, 190392490709135L, 308061521170129L, 498454011879264L, 806515533049393L, 1304969544928657L, 2111485077978050L, 3416454622906707L, 5527939700884757L, 8944394323791464L, 14472334024676221L, 23416728348467685L, 37889062373143906L, 61305790721611591L, 99194853094755497L, 160500643816367088L, 259695496911122585L, 420196140727489673L, 679891637638612258L, 1100087778366101931L, 1779979416004714189L, 2880067194370816120L, 4660046610375530309L, 7540113804746346429L, 12200160415121876738L, 19740274219868223167L, 31940434634990099905L, 51680708854858323072L, 83621143489848422977L, 135301852344706746049L, 218922995834555169026L]
Wall time: 0 ns
```

## 最长增长子序列

Fibonacci 问题是一个很好的开始例子，但并没有真正抓住用最优子问题来表示问题的挑战，因为对于 Fibonacci 数来说，答案是非常明显的。让我们在难度上更上一层楼，来看一个叫做 [**最长增长子序列**](https://en.wikipedia.org/wiki/Longest_increasing_subsequence) 的问题。目标是找到给定序列中最长的子序列，使得子序列中的所有元素按升序排序。请注意，元素不需要连续；也就是说，它们不需要彼此相邻出现。例如，在序列[10，22，9，33，21，50，41，60，80]中，最长的递增子序列(LIS)是[10，22，33，50，60，80]。

事实证明，很难用“蛮力”解决这个问题。动态编程解决方案更简洁，也更适合问题定义，所以我们将跳过创建不必要的复杂的简单解决方案，直接跳到 DP 解决方案。

```
**def** **find_lis**(seq):  
    n **=** len(seq)
    max_length **=** 1
    best_seq_end **=** **-**1 # keep a chain of the values of the lis
    prev **=** [0 **for** i **in** range(n)]
    prev[0] **=** **-**1 # the length of the lis at each position
    length **=** [0 **for** i **in** range(n)]
    length[0] **=** 1 **for** i **in** range(1, n):
        length[i] **=** 0
        prev[i] **=** **-**1 # start from index i-1 and work back to 0
        **for** j **in** range(i **-** 1, **-**1, **-**1):
            **if** (length[j] **+** 1) **>** length[i] **and** seq[j] **<** seq[i]:
                # there's a number before position i that increases the lis at i
                length[i] **=** length[j] **+** 1
                prev[i] **=** j **if** length[i] **>** max_length:
            max_length **=** length[i]
            best_seq_end **=** i # recover the subsequence
    lis **=** []
    element **=** best_seq_end
    **while** element **!=** **-**1:
        lis.append(seq[element])
        element **=** prev[element] **return** lis[::**-**1]
```

这里的直觉是，对于给定的索引 *i* ，我们可以通过查看所有索引 *j < i* 和 if *length(j)+1 > i* 和 *seq[j] < seq[i]* 来计算最长递增子序列的长度 *length(i)* (意味着在 jj 位置有一个数字增加了该索引处的最长子序列，使得它现在比最长记录子序列更长乍一看，这有点令人困惑，但是请仔细阅读，并说服自己这个解决方案找到了最佳子序列。“prev”列表保存构成子序列中实际值的元素的索引。

让我们生成一些测试数据并进行测试。

```
**import** numpy **as** np  
seq_small **=** list(np.random.randint(0, 20, 20))  
seq_small[16, 10, 17, 18, 9, 0, 2, 19, 4, 3, 1, 14, 12, 6, 2, 4, 11, 5, 19, 4]
```

现在，我们可以运行一个快速测试，看看它是否适用于一个小序列。

```
%time print(find_lis(seq_small))[0, 1, 2, 4, 5, 19]
Wall time: 0 ns
```

仅根据目测，输出看起来是正确的。让我们看看它在更大的序列上表现如何。

```
seq **=** list(np.random.randint(0, 10000, 10000))  
%time print(find_lis(seq))[29, 94, 125, 159, 262, 271, 274, 345, 375, 421, 524, 536, 668, 689, 694, 755, 763, 774, 788, 854, 916, 1018, 1022, 1098, 1136, 1154, 1172, 1237, 1325, 1361, 1400, 1401, 1406, 1450, 1498, 1633, 1693, 1745, 1765, 1793, 1835, 1949, 1997, 2069, 2072, 2096, 2157, 2336, 2345, 2468, 2519, 2529, 2624, 2630, 2924, 3103, 3291, 3321, 3380, 3546, 3635, 3657, 3668, 3703, 3775, 3836, 3850, 3961, 4002, 4004, 4039, 4060, 4128, 4361, 4377, 4424, 4432, 4460, 4465, 4493, 4540, 4595, 4693, 4732, 4735, 4766, 4831, 4850, 4873, 4908, 4940, 4969, 5013, 5073, 5087, 5139, 5144, 5271, 5280, 5299, 5300, 5355, 5393, 5430, 5536, 5538, 5559, 5565, 5822, 5891, 5895, 5906, 6157, 6199, 6286, 6369, 6431, 6450, 6510, 6533, 6577, 6585, 6683, 6701, 6740, 6745, 6829, 6853, 6863, 6872, 6884, 6923, 6925, 7009, 7019, 7028, 7040, 7170, 7235, 7304, 7356, 7377, 7416, 7490, 7495, 7662, 7676, 7703, 7808, 7925, 7971, 8036, 8073, 8282, 8295, 8332, 8342, 8360, 8429, 8454, 8499, 8557, 8585, 8639, 8649, 8725, 8759, 8831, 8860, 8899, 8969, 9046, 9146, 9161, 9245, 9270, 9374, 9451, 9465, 9515, 9522, 9525, 9527, 9664, 9770, 9781, 9787, 9914, 9993]
Wall time: 4.94 s
```

所以它仍然很快，但是差别是明显的。对于序列中的 10，000 个整数，我们的算法已经需要几秒钟才能完成。事实上，即使这个解决方案使用了动态编程，它的运行时仍然是 O(n2) 。这里的教训是，动态编程并不总是能产生快如闪电的解决方案。同样的问题，应用 DP 也有不同的方法。事实上，这个问题有一个解决方案，它使用二分搜索法树，运行时间为 *O(nlogn)* 时间，比我们刚刚提出的解决方案要好得多。

## 背包问题

[**背包问题**](https://en.wikipedia.org/wiki/Knapsack_problem) 是另一个经典的动态规划练习。这个问题的一般化由来已久，并且有许多变体，除了动态编程之外，实际上还有多种方法可以解决这个问题。尽管如此，这仍然是 DP 练习的一个常见例子。

其核心问题是一个组合优化问题。给定一组项目，每个项目都有一个质量和一个值，确定在不超过总重量限制的情况下产生最高可能值的项目集合。我们将看到的变化通常被称为 0–1 背包问题，它将每种物品的副本数量限制为 0 或 1。更正式地说，给定一组 *n* 个项目，每个项目都有重量 *w_i* 和值 *v_i* 以及最大总重量 *W* ，我们的目标是:

*maxσv _ IX _ I*，其中*σW _ IX _ I≤W*

让我们看看实现是什么样子，然后讨论它为什么工作。

```
**def** **knapsack**(W, w, v):  
    # create a W x n solution matrix to store the sub-problem results
    n **=** len(v)
    S **=** [[0 **for** x **in** range(W)] **for** k **in** range(n)] **for** x **in** range(1, W):
        **for** k **in** range(1, n):
            # using this notation k is the number of items in the solution and x is the max weight of the solution,
            # so the initial assumption is that the optimal solution with k items at weight x is at least as good
            # as the optimal solution with k-1 items for the same max weight
            S[k][x] **=** S[k**-**1][x] # if the current item weighs less than the max weight and the optimal solution including this item is 
            # better than the current optimum, the new optimum is the one resulting from including the current item
            **if** w[k] **<** x **and** S[k**-**1][x**-**w[k]] **+** v[k] **>** S[k][x]:
                S[k][x] **=** S[k**-**1][x**-**w[k]] **+** v[k] **return** S
```

这种算法背后的直觉是，一旦你已经解决了在某个重量 *x < W* 和某个数量的物品 *k < n* 的物品的最佳组合，那么就很容易解决多一个物品或一个更高的最大重量的问题，因为你可以检查通过合并该物品获得的解决方案是否比你已经找到的最佳解决方案更好。那么如何得到初始解呢？继续沿着兔子洞往下走，直到到达 0(在这种情况下，答案是 0)。乍一看，这很难理解，但这正是动态编程的魅力所在。让我们运行一个例子来看看它是什么样子的。我们将从一些随机生成的权重和值开始。

```
w **=** list(np.random.randint(0, 10, 5))  
v **=** list(np.random.randint(0, 100, 5))  
w, v([3, 9, 3, 6, 5], [40, 45, 72, 77, 16])
```

现在，我们可以在一个约束条件下运行该算法，该约束条件是项目的权重之和不能超过 15。

```
knapsack(15, w, v)[[0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0],
 [0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 45, 45, 45, 45, 45],
 [0, 0, 0, 0, 72, 72, 72, 72, 72, 72, 72, 72, 72, 117, 117],
 [0, 0, 0, 0, 72, 72, 72, 77, 77, 77, 149, 149, 149, 149, 149],
 [0, 0, 0, 0, 72, 72, 72, 77, 77, 88, 149, 149, 149, 149, 149]]
```

这里的输出是给定的最大权重(将其视为列索引)和最大项目数(行索引)的最佳值数组。注意输出是如何遵循看起来有点像波前图案的。这似乎是动态编程解决方案中经常出现的现象。右下角的值是我们在给定约束条件下寻找的最大值，也是问题的答案。

我们对动态编程的介绍到此结束！在现实世界中使用这种技术肯定需要大量的练习；动态编程的大多数应用都不是很明显，需要一些技巧才能发现。就我个人而言，这对我来说一点都不自然，甚至学习这些相对简单的例子也需要花很多心思。看起来这类问题在实践中并不经常出现，这可能有一定的道理。然而，我发现，简单地了解动态编程以及它如何适应更一般的问题解决框架，已经使我成为一名更好的工程师，这本身就使得花时间去理解它是值得的。

![](img/404a3450876148a5ae60026f3ca8030b.png)

*对本文的评论，请查看原贴* [*好奇洞察*](http://www.johnwittenauer.net/solving-problems-with-dynamic-programming/)

*关注我的*[*Twitter*](https://twitter.com/jdwittenauer)*获取新帖子更新*