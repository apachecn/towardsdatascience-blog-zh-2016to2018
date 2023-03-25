# 从范数到正交性:带有直观例子的机器学习基础数学第 2/3 部分

> 原文：<https://towardsdatascience.com/from-norm-to-orthogonality-fundamental-mathematics-for-machine-learning-with-intuitive-examples-57bb898e69f2?source=collection_archive---------9----------------------->

为了理解机器学习算法的数学，特别是深度学习算法，从基础到更高级建立数学概念是必不可少的。不幸的是，数学理论在许多情况下太难/抽象/枯燥，难以消化。想象你正在吃一个比萨饼，喝一杯可乐总是更容易和更有趣。

这篇文章的目的是**为基础数学理论提供直观的例子**使学习体验更加愉快和难忘，那就是鸡翅配啤酒，薯条配番茄酱，里脊配葡萄酒。

包含 3 门课程的机器学习基础数学课程组织如下:

[**从标量到张量**:带有直观例子的机器学习基础数学**第 1/3 部分**](https://medium.com/@alina.li.zhang/from-scalar-to-tensor-fundamental-mathematics-for-machine-learning-with-intuitive-examples-part-163727dfea8d)

*   什么是标量、矢量、矩阵和张量？
*   标量、向量和矩阵之间的加法
*   标量、向量和矩阵之间的乘法
*   单位矩阵和逆矩阵
*   对角矩阵和对称矩阵

**从范数到正交**:带有直观示例的机器学习基础数学**第 2/3 部分**

*   向量的 1-范数，2-范数，最大范数
*   正交和标准正交向量
*   正交矩阵

[**从特征分解到行列式**:带有直观例子的机器学习基础数学**第 3/3 部分**](https://medium.com/@alina.li.zhang/from-eigendecomposition-to-determinant-fundamental-mathematics-for-machine-learning-with-1b6b449a82c6)

*   矩阵的特征分解:特征值和特征向量
*   跟踪运算符
*   方阵的行列式

在本文中，我们将通过直观的例子，从范数到正交性讨论第 2/3 部分**。**

## 向量的 1-范数，2-范数，最大范数

如何度量一个矢量的大小？一种方法是使用范数函数:

![](img/67d6f1cbedd5c392b7f35d9b2d4aea5d.png)

*   1-范数:在机器学习应用中，当 0 和非 0 元素之间的差异很重要时，通常使用 1-范数。

![](img/42b1a2331e40afc12a9fe217ff6dfc55.png)

例如，向量 ***v*** 的 1 范数可以计算为:

![](img/3a9668b94baf057fc8854981ed464b27.png)

*   **2-范数**:通称欧氏范数，是原点到向量 ***x*** 所标识的点的欧氏距离。

![](img/983c3932620d43677317b6fe120fb43d.png)

Photo credit to [wikipedia](https://en.wikipedia.org/wiki/Euclidean_distance)

通常使用平方 2-范数而不是 2-范数本身来度量向量的大小。原因是平方 2 范数可以计算为:

![](img/280157ae066f66c17dd693ca159b3150.png)

这比计算 2-范数本身更方便。下面的例子说明了如何计算向量 ***v:*** 的 2-范数

![](img/8f96bd6d10f8e06b520b6bde347a0c57.png)

*   **最大范数**:向量中元素的最大绝对值，可以写成:

![](img/e90cca17642a12fcfb22cfbe8c55aac5.png)

下面的例子显示了向量 ***v:*** 的最大范数的计算

![](img/efe9a3742a4b74a597194609eac57913.png)

## 正交和标准正交向量

向量 ***u*** 和向量 ***v*** 彼此正交**当且仅当它们的点积为 0:**

![](img/b6d51adf1dfbf4a796af38d61cae1330.png)

例如，在三维欧几里得空间中，

![](img/6f5a3ee391cfb632b8fac823149d9432.png)

在几何学中，两个正交向量在欧几里得空间中相互垂直:

![](img/ca5effcffc10499cb60b83db7ac1f071.png)![](img/00dbc7b1b6b1e773df66ebe15315ceba.png)

Photo credit to [ubisafe](https://ubisafe.org/orthogonal-vector.html)

向量 u 和向量 v 是一对**正交的**向量的意思是:

![](img/40aa4adbdef62020235be9f8f785c6d1.png)

它可以扩展到 3-D 欧几里得空间中的下列方程:

![](img/aefd1b0b3d5d99c3d1d121d5e5c2ba6b.png)

举个例子，

![](img/55839688e62acec7f6963dc5bfc91a7d.png)![](img/3d39baca3b0b39995c84eeef36e6460d.png)

因此，我们说，向量 ***u*** 和向量 ***v*** 是正交的。

## 正交矩阵

正交矩阵是行和列正交的正方形矩阵:

![](img/608e22beaea04626532f7fdc8aa06f37.png)

例如，以下矩阵是正交的，因为:

![](img/087b20144d810327f0c35bd5d89b077f.png)

这意味着如果一个矩阵的转置等于其逆矩阵，则该矩阵是正交的:

![](img/48c740bfd40c62c2a18c5be1b096ea80.png)

因此，正交矩阵是机器学习中感兴趣的，因为矩阵的逆的计算非常便宜。我们需要注意的是，正交矩阵中的行和列不仅是正交的，而且是正交的。

恭喜你！你已经用直观的例子完成了机器学习基础数学的三分之二。你能做到的！

## 下一步: [**从特征分解到行列式**:带有直观例子的机器学习基础数学**第 3/3 部分**](https://medium.com/@alina.li.zhang/from-eigendecomposition-to-determinant-fundamental-mathematics-for-machine-learning-with-1b6b449a82c6)