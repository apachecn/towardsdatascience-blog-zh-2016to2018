# 自适应元启发式智能粒子(AMI-P)

> 原文：<https://towardsdatascience.com/adaptive-meta-heuristically-intelligent-particle-ami-p-4badd123b3a2?source=collection_archive---------8----------------------->

![](img/742fd34d2b67043dc1e26c4605f3d5a4.png)

Red Stars — Track of best solutions. Blue Star — AMI-Particle

# **AMI 粒子**

*AMI-P 是我新发现的优化方法。AMI 粒子使用单个粒子迭代地搜索全局最优，该单个粒子智能地学习并适应凸和非凸 n 维搜索空间。*

如下所示，AMI-P 在单峰和多峰目标函数中都收敛到全局最优。这项研究部分是因为我发现了一个数学恒等式。这个恒等式通过在目标函数的可行搜索空间内请求自适应 n 维旋转，赋予了 *AMI* 粒子探索和开发的平衡。

*AMI* 粒子的一个独特但值得注意的特征是它不含*导数；因此，AMI-P 是无梯度的*。这允许 AMI 粒子在相对于维度的线性时间*中在更高维度中收敛。*动画性能*部分下面是 100，000- & 1，000，000 维单峰和多峰函数的性能结果。*

*下面显示的所有结果都是使用 Matlab 在 MacBook Pro 上运行 200 次后的最差性能。这些对于应用数学领域来说是非常有希望的结果。数学优化方法是* ***人工智能(神经网络)、力学、分子工程、土木工程、运筹学、地球物理学、控制工程、电气工程以及最后但并非最不重要的经济学和金融学中的关键因素。***

# 粒子动画表演

对于每个测试函数，使用 Matlab 生成二维动画性能图。动画显示了粒子*探索*搜索空间，同时保持*‘吸引’*到它学习*利用*的区域。

*粒子每次收敛到全局最优小于~ 0.1/s；然而，出于演示的目的，我放慢了动画 gif 的速度。*

动画图下方是针对*球体*、 *Rastrigin* 和 *Griewank* 函数的 100，000 维和 1，000，000 维测试中的 AMI-Particle 性能结果。

## **粒子-球函数**

![](img/f957e674bb16cf1694ccaae17ad48b9f.png)

[https://www.sfu.ca/~ssurjano/griewank.html](https://www.sfu.ca/~ssurjano/griewank.html)

![](img/392315aaef910a06b286236639d104bc.png)![](img/ba35b96457d4fddae818b17c2ac2627f.png)

AMI Particle Sphere Test — 2-D where xi ∈ [-100, 100], for all i = 1, …, D (100 epochs)

## AMI 粒子— Rastrigin 函数

![](img/6f869662bba3dd68913720ce9126db36.png)

[https://www.sfu.ca/~ssurjano/rastr.html](https://www.sfu.ca/~ssurjano/rastr.html)

![](img/71fd8bd4e810c1534a64a2f97dda8f22.png)![](img/049c75714ece429237cc47c510b46a1f.png)

AMI Particle Rastrigin Test — 2-D where xi ∈ [-5.12, 5.12], for all i = 1, …, D (100 epochs)

## AMI-Particle — **格里万克函数**

![](img/56f126f54b2f44c337f657a6ea6e5d7a.png)

[https://www.sfu.ca/~ssurjano/griewank.html](https://www.sfu.ca/~ssurjano/griewank.html)

![](img/9de25f174d34e4c66d0e2919d809a579.png)![](img/20948a38e24103143d131abee4784022.png)

AMI Particle Griewank Test — 2-D where xi ∈ [-600, 600], for all i = 1, …, D (250 epochs)

## 粒子——戈尔茨坦价格函数

![](img/821f3301ae1eb51ba93599ac675d8aea.png)

[https://www.sfu.ca/~ssurjano/goldpr.html](https://www.sfu.ca/~ssurjano/goldpr.html)

![](img/4ee601022b999f41af47c88206e32c33.png)![](img/ec6026d213cb4c236a16930ac750cd5e.png)

AMI Particle Goldstein-Price Test — 2-D where xi ∈ [-2, 2], for all i = 1, 2 (300 epochs)

# AMI 粒子——维度测试

## 10 万维收敛测试

![](img/106f62fb5c23bade2299d4dfac10cb2d.png)

AMI-Particle 100,000-d Sphere Test — Worst-Case Performance Results of 200 Runs (~ 3/s)

![](img/7a66b3b3256da720608e9d066c2d477c.png)

AMI-Particle 100,00-d Rastrigin Test — Worst-Case Performance Results of 200 Runs (~ 3/s)

![](img/966fb7eade2c5d6e22f104005509d823.png)

AMI-Particle 100,000-d Griewank Test — Worst-Case Performance Results of 200 Runs (~3/s)

## 1，000，000 维收敛测试

![](img/6e8392a6a20bdb1098d24c06dca127f9.png)

AMI-Particle 1,000,000-d Sphere Test — Worst-Case Performance Results of 200 Runs (~ 9/s)

![](img/c5266293372a9f397481eb438e414ba6.png)

AMI-Particle 1,000,000-d Rastrigin Test — Worst-Case Performance Results of 200 Runs (~ 9/s)

![](img/264d16100bd8126d7b794957b8628a5e.png)

AMI-Particle 1,000,000-d Griewank Test — Worst-Case Performance Results of 200 Runs (~ 9/s)

# 下一步是什么？

随着使用不同的*数学优化*方法解决的过多现实世界问题的增加，*自适应元启发式智能粒子*方法的有希望的结果激励着我继续我的研究。我相信这种方法可以大幅提高性能，并减少*人工智能*和*深度学习中*神经网络*所需的计算资源。*