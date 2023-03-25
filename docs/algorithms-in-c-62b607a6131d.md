# C++中的算法

> 原文：<https://towardsdatascience.com/algorithms-in-c-62b607a6131d?source=collection_archive---------4----------------------->

## 完全搜索，贪婪，分治，动态规划

# 介绍

本文旨在向读者介绍四种主要的算法范式:*完全搜索*、*贪婪算法*、*分而治之、*和*动态规划*。许多算法问题可以映射到这四个类别中的一个，掌握每一个将使你成为一个更好的程序员。

本文是从竞技编程的角度来写的。在参考资料部分，您可以找到一些资源来帮助您入门，或者通过编码竞赛来提高您的编程技能。

![](img/24d40856b5a86779fd230481f8a7ca10.png)

# 完整搜索

完全搜索(又名蛮力或递归回溯)是一种通过遍历整个搜索空间寻找解决方案来解决问题的方法。在搜索过程中，我们可以删除搜索空间中我们确信不会导致所需解决方案的部分。在编程竞赛中，完全搜索可能会导致超时，然而，这是一个解决小输入问题的好策略。

## 完整搜索示例:8 皇后问题

我们的目标是在棋盘上放置 8 个皇后，这样就不会有两个皇后互相攻击。在最天真的解决方案中，我们需要列举 64 种选择 8 ~ 4B 的可能性。一个更好的天真的解决方案是认识到我们可以将每个皇后放在一个单独的列中，这导致了 8⁸~17M 可能性。我们可以做得更好，把每个皇后放在一个单独的列和一个单独的行，结果是 8！约 40K 有效行排列。在下面的实现中，我们假设每个皇后占据不同的列，并且我们为 8 个皇后中的每一个计算有效的行号。

```
#include <cstdlib>
#include <cstdio>
#include <cstring>
using namespace std; //row[8]: row # for each queen
//TC: traceback counter
//(a, b): 1st queen placement at (r=a, c=b)
int row[8], TC, a, b, line_counter; bool place(int r, int c)
{   
    // check previously placed queens 
    for (int prev = 0; prev < c; prev++) 
    { 
        // check if same row or same diagonal
        if (row[prev] == r || (abs(row[prev] — r) == abs(prev — c)))
            return false; 
    }
    return true;
}void backtrack(int c)
{
    // candidate solution; (a, b) has 1 initial queen
    if (c == 8 && row[b] == a) 
    { 
        printf(“%2d %d”, ++line_counter, row[0] + 1); 
        for (int j=1; j < 8; j++) {printf(“ %d”, row[j] + 1);}    
        printf(“\n”);
    } //try all possible rows 
    for (int r = 0; r < 8; r++)
    {
        if (place(r, c))
        {
            row[c] = r; // place a queen at this col and row   
            backtrack(c + 1); //increment col and recurse
        }
    }
}int main() 
{
     scanf(“%d”, &TC); 
     while (TC--) 
     { 
        scanf(“%d %d”, &a, &b); a--; b--; //0-based indexing    
        memset(row, 0, sizeof(row)); line_counter = 0;
        printf(“SOLN COLUMN\n”);
        printf(“ # 1 2 3 4 5 6 7 8\n\n”);
        backtrack(0); //generate all possible 8! candidate solutions    
        if (TC) printf(“\n”);
     }
     return 0;
}
```

对于 TC=8 和(a，b) = (1，1)的初始皇后位置，上述代码产生以下输出:

```
SOLN       COLUMN
 #    1 2 3 4 5 6 7 8 1    1 5 8 6 3 7 2 4
 2    1 6 8 3 7 4 2 5
 3    1 7 4 6 8 2 5 3
 4    1 7 5 8 2 4 6 3
```

这表明在给定初始皇后位置(r=1，c=1)的情况下，有四种可能的布局。注意，与迭代解决方案相比，递归的使用允许更容易地修剪搜索空间。

# 贪婪算法

贪婪算法在每一步采取局部最优选择，希望最终达到全局最优解。贪婪算法通常依赖于贪婪启发式算法，人们经常可以找到贪婪算法不能达到全局最优的例子。

## 贪婪的例子:分数背包

贪婪背包问题包括选择将什么物品放置在有限容量 W 的背包中，以便最大化背包物品的总价值，其中每个物品具有相关联的重量和值。我们可以将贪婪试探法定义为物品价值与物品重量的比率，即我们希望贪婪地选择同时具有高价值和低重量的物品，并基于该标准对物品进行分类。在分数背包问题中，我们被允许取一个物品的分数(相对于 0-1 背包)。

```
#include <iostream>
#include <algorithm>
using namespace std; struct Item{
    int value, weight;
    Item(int value, int weight) : value(value), weight(weight) {}
}; bool cmp(struct Item a, struct Item b){ 
    double r1 = (double) a.value / a.weight; 
    double r2 = (double) b.value / b.weight; 
    return r1 > r2;
} double fractional_knapsack(int W, struct Item arr[], int n)
{
    sort(arr, arr + n, cmp);     int cur_weight = 0; double tot_value = 0.0;
    for (int i=0; i < n; ++i) 
    { 
        if (cur_weight + arr[i].weight <= W) 
        {
            cur_weight += arr[i].weight;
            tot_value += arr[i].value;
        }   
        else 
        {   //add a fraction of the next item
            int rem_weight = W — cur_weight;
            tot_value += arr[i].value * 
                        ((double) rem_weight / arr[i].weight);                     
            break;
        }
    } 
    return tot_value;
}
int main()
{ 
    int W = 50; // total knapsack weight
    Item arr[] = {{60, 10}, {100, 20}, {120, 30}}; //{value, weight}
    int n = sizeof(arr) / sizeof(arr[0]); 
    cout << “greedy fractional knapsack” << endl; 
    cout << “maximum value: “ << fractional_knapsack(W, arr, n);
    cout << endl; 
    return 0;
}
```

由于排序是开销最大的操作，该算法的运行时间为 O(n log n)。给定三个项目的(值，重量)对:{(60，10)，(100，20)，(120，30)}，并且总容量 W=50，上面的代码产生以下输出:

```
greedy fractional knapsack
maximum value: 240
```

我们可以看到，输入项目按价值/成本的递减比率排序，在贪婪地选择项目 1 和 2 后，我们取项目 3 的 2/3 分数，总价值为 60+100+(2/3)120 = 240。

# 各个击破

分而治之(D&C)是一种技术，它将一个问题分成更小的、*独立的*子问题，然后组合每个子问题的解决方案。

分而治之技术的例子包括排序算法，例如快速排序、合并排序和堆排序以及二分搜索法。

## D&C 的例子:二分搜索法

二分搜索法的经典用法是在一个*排序的*数组中搜索一个值。首先，我们检查数组的中间，看看是否包含我们要找的东西。如果是这样，或者没有其他项目需要考虑，我们就停止。否则，我们决定答案是在中间元素的左边还是右边，并继续搜索。由于每次检查后搜索空间的大小减半，因此算法的复杂度为 O(log n)。

```
#include <algorithm>
#include <vector>
#include <iostream>
using namespace std;int bsearch(const vector<int> &arr, int l, int r, int q)
{ 
    while (l <= r) 
    {
        int mid = l + (r-l)/2;
        if (arr[mid] == q) return mid; 

        if (q < arr[mid]) { r = mid — 1; } 
        else              { l = mid + 1; }
    }
    return -1; //not found
}int main()
{
    int query = 10; 
    int arr[] = {2, 4, 6, 8, 10, 12};
    int N = sizeof(arr)/sizeof(arr[0]);
    vector<int> v(arr, arr + N); 

    //sort input array
    sort(v.begin(), v.end()); int idx;
    idx = bsearch(v, 0, v.size(), query);
    if (idx != -1)
        cout << "custom binary_search: found at index " << idx;    
    else 
        cout << "custom binary_search: not found"; return 0;
}
```

上面的代码产生以下输出:

```
custom binary_search: found at index 4
```

注意如果没有找到查询元素，但是我们希望找到不小于查询的第一个条目或者大于查询的第一个条目，我们可以使用 STL lower_bound 和 upper_bound。

# 动态规划

动态编程(DP)是一种技术，它将一个问题分成更小的*重叠的*子问题，计算每个子问题的解决方案，并将其存储在 DP 表中。从差压表中读取最终解。

掌握动态编程的关键技能是确定问题状态(DP 表的条目)以及状态之间的关系或转换的能力。然后，在定义了基本情况和递归关系之后，可以用自顶向下或自底向上的方式填充 DP 表。

在自顶向下的 DP 中，根据需要递归地填充表格，从顶部开始，向下到更小的子问题。在自底向上的 DP 中，从最小的子问题开始迭代地填充表格，并使用它们的解决方案来构建和达到更大的子问题的解决方案。在这两种情况下，如果已经遇到了子问题，那么只需在表中查找它的解决方案(而不是从头开始重新计算解决方案)。这大大降低了计算成本。

## DP 示例:二项式系数

我们用二项式系数的例子来说明自顶向下和自底向上 DP 的使用。下面的代码基于带有重叠子问题的二项式系数的递归。设 C(n，k)表示 n 选择 k，那么，我们有:

```
Base case: C(n,0) = C(n,n) = 1
Recursion: C(n,k) = C(n-1, k-1) + C(n-1, k)
```

请注意，我们有多个重叠的子问题。例如，对于 C(n=5，k=2 ),递归树如下:

```
 C(5, 2)
                      /                       \
             C(4, 1)                            C(4, 2)
            /      \                        /           \
       C(3, 0)   C(3, 1)             C(3, 1)             C(3, 2)
                 /    \             /     \             /     \
           C(2, 0)  C(2, 1)      C(2, 0) C(2, 1)    C(2, 1)  C(2, 2)
                   /      \              /   \        /    \
               C(1, 0)  C(1, 1)    C(1, 0)  C(1, 1) C(1, 0)  C(1, 1)
```

我们可以如下实现自顶向下和自底向上的 DP:

```
#include <iostream>
#include <cstring>
using namespace std; #define V 8
int memo[V][V]; //DP table int min(int a, int b) {return (a < b) ? a : b;} void print_table(int memo[V][V])
{
    for (int i = 0; i < V; ++i) 
    {
        for (int j = 0; j < V; ++j)
        {
            printf(" %2d", memo[i][j]);        
        }
        printf("\n");
    }
} int binomial_coeffs1(int n, int k)
{ 
    // top-down DP 
    if (k == 0 || k == n) return 1;  
    if (memo[n][k] != -1) return memo[n][k];
    return memo[n][k] = binomial_coeffs1(n-1, k-1) +      
                        binomial_coeffs1(n-1, k);
}int binomial_coeffs2(int n, int k)
{    
    // bottom-up DP
    for (int i = 0; i <= n; ++i)  
    {        
        for (int j = 0; j <= min(i, k); ++j)
        {            
            if (j == 0 || j == i) 
            {                
                memo[i][j] = 1;
            }  
            else
            {
                memo[i][j] = memo[i-1][j-1] + memo[i-1][j];                  
            }
        } 
    }
    return memo[n][k];
}  
int main()
{
    int n = 5, k = 2;
    printf("Top-down DP:\n");
    memset(memo, -1, sizeof(memo));
    int nCk1 = binomial_coeffs1(n, k);
    print_table(memo);
    printf("C(n=%d, k=%d): %d\n", n, k, nCk1);

    printf("Bottom-up DP:\n");
    memset(memo, -1, sizeof(memo));
    int nCk2 = binomial_coeffs2(n, k);
    print_table(memo);
    printf("C(n=%d, k=%d): %d\n", n, k, nCk2);

    return 0;
}
```

对于 C(n=5，k=2)，上面的代码产生以下输出:

```
Top-down DP:
 -1 -1 -1 -1 -1 -1 -1 -1
 -1 -1 -1 -1 -1 -1 -1 -1
 -1  2 -1 -1 -1 -1 -1 -1
 -1  3  3 -1 -1 -1 -1 -1
 -1  4  6 -1 -1 -1 -1 -1
 -1 -1 10 -1 -1 -1 -1 -1
 -1 -1 -1 -1 -1 -1 -1 -1
 -1 -1 -1 -1 -1 -1 -1 -1
C(n=5, k=2): 10Bottom-up DP:
  1 -1 -1 -1 -1 -1 -1 -1
  1  1 -1 -1 -1 -1 -1 -1
  1  2  1 -1 -1 -1 -1 -1
  1  3  3 -1 -1 -1 -1 -1
  1  4  6 -1 -1 -1 -1 -1
  1  5 10 -1 -1 -1 -1 -1
 -1 -1 -1 -1 -1 -1 -1 -1
 -1 -1 -1 -1 -1 -1 -1 -1
C(n=5, k=2): 10
```

时间复杂度为 O(n * k)，空间复杂度为 O(n * k)。在自顶向下 DP 的情况下，子问题的解决方案根据需要存储(存储),而在自底向上 DP 中，整个表是从基本情况开始计算的。注意:出于打印目的，选择了较小的 DP 表格尺寸(V=8 ),建议使用更大的表格尺寸。

# 密码

所有代码可在:[https://github.com/vsmolyakov/cpp](https://github.com/vsmolyakov/cpp)获得

要编译 C++代码，您可以运行以下命令:

```
>> g++ <filename.cpp> --std=c++11 -Wall -o test
>> ./test
```

# 结论

有很多学习算法的好资源。我强烈推荐 Steven Halim 关于竞争性编程的书[1]。除了经典的算法设计手册[2]和 CLRS [3]。有许多伟大的编码挑战网站，其中一些在[4]中提到。此外，[5]有一个奇妙的算法集合和易于理解的实现。如果您正在构建自己的库或参加编程竞赛，这是一个很好的资源。

希望这篇文章对你有所帮助。编码快乐！！

# 参考

1.  Steven Halim，“竞争性编程”，第 3 版，2013 年
2.  史蒂文·斯基埃纳，《算法设计手册》，施普林格，2011 年
3.  托马斯·科尔曼等人，《算法导论》，麻省理工学院出版社，2009 年
4.  [https://medium . freecodecamp . org/the-10-most-popular-coding-challenge-websites-of-2016-fb8a 5672d 22 f](https://medium.freecodecamp.org/the-10-most-popular-coding-challenge-websites-of-2016-fb8a5672d22f)
5.  【https://www.geeksforgeeks.org/ 