# 用于数据分析的 Numpy 简介

> 原文：<https://towardsdatascience.com/introduction-to-numpy-for-data-analysis-3aaded0d0996?source=collection_archive---------3----------------------->

## 查看更新的 DevOps 课程。

## 课程注册链接:

[](https://www.101daysofdevops.com/register/) [## 创建一个帐户

### 编辑描述

www.101daysofdevops.com](https://www.101daysofdevops.com/register/) 

**课程链接:**

[](https://www.101daysofdevops.com/courses/101-days-of-devops/) [## 101 天的开发周期

### http://100daysofdevops.com/day-100-100-days-of-devops/好消息是从话题开始包括(请让我知道…

www.101daysofdevops.com](https://www.101daysofdevops.com/courses/101-days-of-devops/) 

**YouTube 链接:**

[](https://www.youtube.com/user/laprashant/videos) [## 普拉尚·拉赫拉

### 主要想法是通过视频分享我的 Linux 知识。我的 Linkedin 个人资料…

www.youtube.com](https://www.youtube.com/user/laprashant/videos) 

*NumPy 是 Python 的线性代数库，这也是 PyData 生态系统中所有库都依赖 NumPy 作为主要构建模块的重要原因。*

***安装 Numpy***

```
*# pip2 install numpy**Collecting numpy**Using cached numpy-1.12.1-cp27-cp27mu-manylinux1_x86_64.whl**Installing collected packages: numpy**Successfully installed numpy-1.12.1*
```

*强烈建议使用 Anaconda 发行版安装 Python，以确保所有底层依赖项(* ***如线性代数库*** *)都与 conda 安装同步。*

*如果你有康达安装*[【https://www.continuum.io/downloads】T21](https://www.continuum.io/downloads)

```
*conda install numpy*
```

*Numpy 数组是我们使用 Numpy 的主要原因，它们有两种类型*

*   ***向量(一维数组)***
*   ***矩阵(二维数组)***

```
*# 1-D Array**>>> test = [1,2,3]**>>> import numpy as np**# We got the array
>>> np.array(test)**array([1, 2, 3])**>>> arr = np.array(test)*
```

*我们来看看* ***二维数组***

```
*>>> test1 = [[1,2,3],[4,5,6],[7,8,9]]**>>> test1**[[1, 2, 3], [4, 5, 6], [7, 8, 9]]**>>> np.array(test1)**array([[1, 2, 3],**[4, 5, 6],**[7, 8, 9]])*
```

*但是生成 NumPy 数组最常见的方法是使用****range****函数(类似于 Python 中的 range)*

```
*#Similar to range(start,stop,step),stop not included and indexing start with zero 
>>> np.arange(0,10)**array([0, 1, 2, 3, 4, 5, 6, 7, 8, 9])*
```

*但是如果我们在寻找特定类型的数组*

```
*>>> np.zeros(3)**array([ 0., 0., 0.])**#We are passing Tuple where* ***first value represent row*** *and* ***second represent column****>>> np.zeros((3,2))**array([[ 0., 0.],**[ 0., 0.],**[ 0., 0.]])*
```

*类似的还有那些*

```
*>>> np.ones(2)**array([ 1., 1.])**>>> np.ones((2,2))**array([[ 1., 1.],**[ 1., 1.]])*
```

*现在我们来看看****linspace***

```
*#It will give 9 evenly spaced point between 0 and 3(It return 1D vector)
>>> np.linspace(0,3,9)**array([ 0\. , 0.375, 0.75 , 1.125, 1.5 , 1.875, 2.25 , 2.625, 3\. ])**>>> np.linspace(0,10,3)**array([  0.,   5.,  10.])*
```

*让我们创建* ***单位矩阵*** *(二维方阵，其中行数等于列数，对角线为 1)*

![](img/3b1cd8f07a390570130cb00d4d403834.png)

*创建数组* ***随机数***

```
*#1-D, it create random sample uniformly distributed between 0 to 1
>>> np.random.rand(3)**array([ 0.87169008, 0.51446765, 0.65027072])**#2-D
>>> np.random.rand(3,3)**array([[ 0.4217015 , 0.86314141, 0.14976093],**[ 0.4348433 , 0.68860693, 0.88575823],**[ 0.56613179, 0.56030069, 0.51783999]])*
```

*现在如果我想要* ***随机整数***

```
*#This will give random integer between 1 and 50
>>> np.random.randint(1,50)**27**#In case if we need 10 random integer
>>> np.random.randint(1,50,10)**array([39, 34, 30, 21, 18, 30,  3,  6, 37, 11])*
```

*我们可以* ***重塑*** *我们现有的阵列*

```
*>>> np.arange(25)**array([ 0, 1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12, 13, 14, 15, 16,**17, 18, 19, 20, 21, 22, 23, 24])**>>> arr = np.arange(25)**>>> arr**array([ 0, 1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12, 13, 14, 15, 16,**17, 18, 19, 20, 21, 22, 23, 24])**>>> arr.reshape(5,5)**array([[ 0, 1, 2, 3, 4],**[ 5, 6, 7, 8, 9],**[10, 11, 12, 13, 14],**[15, 16, 17, 18, 19],**[20, 21, 22, 23, 24]])*
```

*让我们来看看其他一些方法*

```
*>>> np.random.randint(0,50,10)**array([10, 40, 18, 30, 6, 40, 49, 23, 3, 18])**>>> ranint = np.random.randint(0,50,10)**>>> ranint**array([18, 49, 6, 28, 30, 10, 46, 11, 40, 16])**#It will return* ***max value*** *of the array
>>> ranint.max()**49****#Minimum value*** *>>> ranint.min()**6**>>> ranint**array([18, 49,  6, 28, 30, 10, 46, 11, 40, 16])**#To find out the position
>>> ranint.argmin()**2**>>> ranint.argmax()**1*
```

*找出一个数组的形状*

```
*>>> arr.shape**(25,)**>>> arr.reshape(5,5)**array([[ 0, 1, 2, 3, 4],**[ 5, 6, 7, 8, 9],**[10, 11, 12, 13, 14],**[15, 16, 17, 18, 19],**[20, 21, 22, 23, 24]])**>>> arr = arr.reshape(5,5)**>>> arr.shape**(5, 5)*
```

*找出* ***的数据类型***

```
*>>> arr.dtype**dtype(‘int64’)*
```

*NumPy 情况下的分度*

```
*>>> import numpy as np**>>> arr =np.arange(0,11)**>>> arr**array([ 0, 1, 2, 3, 4, 5, 6, 7, 8, 9, 10])**>>> arr[0]**0**>>> arr[0:4]**array([0, 1, 2, 3])*
```

*Numpy 数组与 Python 列表的不同之处在于它们的广播能力*

```
*>>> arr[:] = 20**>>> arr**array([20, 20, 20, 20, 20, 20, 20, 20, 20, 20, 20])**# Now let's try to slice this array**>>> arr1 = arr[0:5]**>>> arr1**array([0, 1, 2, 3, 4])**>>> arr1[:] = 50**>>> arr1**array([50, 50, 50, 50, 50])**#But as you can see the side effect it change the original array too(i.e data is not copied it's just the view of original array)* ***>>> arr******array([50, 50, 50, 50, 50,  5,  6,  7,  8,  9, 10])****#If we want to avoid this feature, we can copy the array and then perform broadcast on the top of it**>>> arr2 = arr.copy()**>>> arr2**array([50, 50, 50, 50, 50,  5,  6,  7,  8,  9, 10])**>>> arr2[6:10] = 100**>>> arr2**array([ 50,  50,  50,  50,  50,   5, 100, 100, 100, 100,  10])**>>> arr**array([50, 50, 50, 50, 50,  5,  6,  7,  8,  9, 10])*
```

*索引* ***二维数组(矩阵)***

```
*>>> arr = ([1,2,3],[4,5,6],[7,8,9])
>>> arr**([1, 2, 3], [4, 5, 6], [7, 8, 9])**>>> arr1 = np.array(arr)**>>> arr1**array([[1, 2, 3],**[4, 5, 6],**[7, 8, 9]])**>>> arr[1]**[4, 5, 6]**# To grab 5(Indexing Start with zero)**>>> arr1[1][1]**5**#Much shortcut method
>>> arr1[1,1]**5*
```

*从二维数组中抓取元素*

```
*>>> arr**array([[1, 2, 3],**[4, 5, 6],**[7, 8, 9]])**#This will grab everything from Row 1 except last element(2) and staring from element 1 upto the end from Row 2* *>>> arr[:2,1:]**array([[2, 3],**[5, 6]])*
```

***条件选择*** *:这样会返回布尔值*

```
*>>> arr**array([ 1, 2, 3, 4, 5, 6, 7, 8, 9, 10])**>>> arr > 5**array([False, False, False, False, False, True, True, True, True, True], dtype=bool)**# We can save this value to an array and perform boolean selection**>>> my_arr = arr > 5**>>> my_arr**array([False, False, False, False, False,  True,  True,  True,  True,  True], dtype=bool)**>>> arr[my_arr]**array([ 6,  7,  8,  9, 10])****#OR much easier way****>>> arr[arr > 5]**array([ 6,  7,  8,  9, 10])**>>> arr[arr < 5]**array([1, 2, 3, 4])*
```

***操作***

```
*# It's the same operation as we are doing with Normal Python**>>> arr = np.arange(0,10)**>>> arr**array([0, 1, 2, 3, 4, 5, 6, 7, 8, 9])**>>> arr**array([0, 1, 2, 3, 4, 5, 6, 7, 8, 9])****#Addition*** *>>> arr + arr**array([ 0, 2, 4, 6, 8, 10, 12, 14, 16, 18])****#Substraction*** *>>> arr — arr**array([0, 0, 0, 0, 0, 0, 0, 0, 0, 0])****#Multiplication*** *>>> arr * arr**array([ 0, 1, 4, 9, 16, 25, 36, 49, 64, 81])****#Broadcast(It add's/substract/multiply 100 to each element)*** *>>> arr + 100**array([100, 101, 102, 103, 104, 105, 106, 107, 108, 109])**>>> arr - 100**array([-100,  -99,  -98,  -97,  -96,  -95,  -94,  -93,  -92,  -91])**>>> arr * 100**array([  0, 100, 200, 300, 400, 500, 600, 700, 800, 900])*
```

*在 Python 的情况下，如果我们试图用零除一，我们将得到被零除的异常*

```
*>>> 0/0**Traceback (most recent call last):**File "<stdin>", line 1, in <module>**ZeroDivisionError: division by zero****OR****>>> 1/0**Traceback (most recent call last):**File “<stdin>”, line 1, in <module>**ZeroDivisionError: division by zero*
```

*在 Numpy 的情况下，如果我们试图除以零，我们不会得到任何异常，但它会返回****nan****(不是一个数字)*

```
*#Not giving you 
>>> arr/arr**__main__:1: RuntimeWarning: invalid value encountered in true_divide**array([* ***nan****, 1., 1., 1., 1., 1., 1., 1., 1., 1.])*
```

*如果被零除，它将返回* ***无穷大***

```
*>>> 1/arr**array([* ***inf****, 1\. , 0.5 , 0.33333333, 0.25 ,**0.2 , 0.16666667, 0.14285714, 0.125 , 0.11111111])*
```

***万能数组功能***

```
*#Square root
>>> np.sqrt(arr)**array([ 0\. , 1\. , 1.41421356, 1.73205081, 2\. ,**2.23606798, 2.44948974, 2.64575131, 2.82842712, 3\. ])**#Exponential
>>> np.exp(arr)**array([ 1.00000000e+00, 2.71828183e+00, 7.38905610e+00,**2.00855369e+01, 5.45981500e+01, 1.48413159e+02,**4.03428793e+02, 1.09663316e+03, 2.98095799e+03,**8.10308393e+03])**#Maximum
>>> np.max(arr)**9**#Minimum
>>> np.min(arr)**0**#Logarithmic
>>> np.log(arr)**__main__:1: RuntimeWarning: divide by zero encountered in log**array([       -inf,  0\.        ,  0.69314718,  1.09861229,  1.38629436,**1.60943791,  1.79175947,  1.94591015,  2.07944154,  2.19722458])*
```