# 熊猫清洗数据介绍(私人慕尼黑 Housemarket 数据集)

> 原文：<https://towardsdatascience.com/introduction-to-cleaning-data-with-pandas-private-munich-housemarket-data-set-be5fc558c9e3?source=collection_archive---------4----------------------->

一开始我就开始用 python 做数据分析之外的其他事情。我是一名技术 SEO 顾问，想要使用一个爬虫(或构建一个)，但不知道任何编程语言，python 看起来是开始的完美语言…
现在，我更喜欢 python，因为它的多功能性可以做我刚才提到的事情，还可以轻松实现数据分析。

如果我们能把这两个世界融合在一起会怎么样？当我手腕受伤时，我创作了我的第一个爬虫，它并不漂亮，它是用一只手写的，但是很有效。
现在我正在看数据，我想这将是一个很好的简单的例子，说明如何使用 python(主要是 Pandas)来使用您收集的数据，或者您可以使用的数据。

# 慕尼黑的房产市场

我现在住在慕尼黑，我真的很喜欢这个城市，在我职业生涯的初期，我去过很多地方(纽约/旧金山/台湾/巴黎)，我很快想到我需要一些数据来帮助寻找我的家。如果我想在某个地方购买，你需要支付的不是一个小数目，很可能是一大笔钱，所以你需要用一些确凿的数据来支撑你的愿望。

我用我的小爬虫独自收集慕尼黑周围房产市场的数据。它仍然是一个手动程序，因为当我创建它时，我不知道也没有时间让它成为一个全天候运行的云原生应用程序。

我可能会尝试改进它，使它成为云原生的，这样我就可以监控它。如果我成功了，这将是一篇博文。我在[上的博客数据分析](https://datanalyst.info?camp=rf~medium~pandas-intro-1)。
与此同时，我一次又一次地运行这个爬虫，有些人可能会说是以一种随机的方式；) .

慕尼黑的房市贵，不是一点点，是真的贵。
所以我在做我的爬虫，以便找到一个符合我预算的家。大部分是公寓(因为房子不在我的预算之内),位于慕尼黑地区。

所以从这个分析的偏见开始，这里有一些:

*   我寻找“最多 4 个房间的公寓”
*   我寻找“高达 60 万€”的公寓(大约 69 万美元)
*   我找的是“离慕尼黑中心最多 20 公里”。
*   我看了公寓是在一楼还是在楼上，但我没有看具体的楼层。

我将我的数据保存为 csv 格式，你可以在我的 github 账户上找到:[https://github.com/pitchmuc/munich_housemarket.git](https://github.com/pitchmuc/munich_housemarket.git)

# 了解熊猫的数据

当我有数据集要查看时，我首先做两件事:

1.  我观察数据集的大小。
2.  我看数据集的类型。

对我来说，数据集不是很大(150 kb)，所以我的电脑处理起来没有问题。如果它再大一点，我会把它分割成最大 100MB 的块，这样我就可以轻松地播放和测试我的脚本了。然后我会生成一个健壮的脚本来处理您的所有数据。
当您对您的完整数据集(即:5GB)执行分析时，您希望在花费数小时运行您的分析并发现它没有达到您想要的效果之前，确保这是可行的。

您必须处理的数据类型可以是:

*   clean:你只需要导入它
*   半干净:您必须导入它，并处理一些丢失的数据。
*   一点也不干净:你必须清理一下，导入它并处理掉丢失的数据。

在我们的例子中，我已经设法获得了相当干净到半干净的数据，但是仍然需要进行一些清理。
在下面的步骤中，你将看到如何用 python 熊猫实现基本的清洁:

导入您想要使用的库。在我们的例子中，你想要熊猫和熊猫。

```
import pandas as pd #naming convention 
import numpy as np #naming convention
```

在您的环境中导入数据集。
在我的例子中，我使用的是 ipython(或者叫做 jupyter notebook)。我真的很喜欢这种探索的环境。当事情变得严重时，我通常会去找 Spyder。

```
df = pd.read_csv('data_immoscout.csv',delimiter='\t') # df is going to be our dataframe. (place where we store all the data). I used tab as delimiter.
```

看看你掌握的数据。使用。head()函数将给出你的数据的预览。

```
df.head() ## use df.head(20) to get 20 lines
```

在 df.head()之上，您还可以尝试查看数据被处理的确切类型。
为了做到这一点，你可以使用

```
df.dtypes ##it will give you something like this : 
index0 int64 
terrace object 
bedrooms object 
construction_year object 
date_month int64 
date_year int64 
date_year_month object 
floor object 
rooms object 
price_room object 
price_surface object 
price object 
surface object 
zip int64 
dtype: object
```

查看您拥有的列的类型非常重要，因为这通常会让您了解数据存储的类型。或者应该存储数据的类型。

```
df.columns # will give you something like : Index(['index0', 'terrace', 'bedrooms', 'construction_year', 'date_month', 'date_year', 'date_year_month', 'floor', 'rooms', 'price_room', 'price_surface', 'price', 'scout_id', 'surface'], dtype='object')
```

由此我可以给你一些关于这些数据的解释，但是通过 dtypes 和 head()来看这些数据实际上应该已经给了你这样的想法:

*   index0:这是表格的索引
*   露台:有没有露台
*   卧室:有多少间卧室
*   建造年份:什么时候建造的
*   date_month:一年中实现爬网的月份
*   date_year:实现爬网的年份
*   date_year_month:实现爬网的月份和年份
*   地板:到底是不是底楼？
*   房间:房间数量
*   价格 _ 房间数:公寓价格除以房间数
*   价格 _ 表面:公寓价格除以表面
*   价格:公寓的价格
*   表面:平面的表面
*   zip:公寓的邮政编码

当你开始处理这些数据时，你要做的第一件事是确保它们在系统中被正确识别。
什么叫“正确的方式”？
正确的做法是:数字看成数字，字符串看成字符串，日期看成日期，NaN 看成 NaN。

**什么是南？**
南代表“不是一个数”。了解这一点非常重要，因为这是我们在最终处理它们之前想要清理的数据类型。

正如你可能已经看到的，有几行实际上写的是 NAN。是公认的南吗？为了查看你有多少 NaN，你可以使用这个非常有用的函数:

```
df.isna().sum() ## The result will look like this :index0               0
terrace              0
bedrooms             0
construction_year    0
date_month           0
date_year            0
date_year_month      0
floor                0
rooms                0
price_room           0
price_surface        0
price                0
surface              0
zip                  0
dtype: int64
```

这意味着在您当前的数据集(或数据帧)中没有可识别的 NaN。
‘NAN’只是我实际运行爬虫时设置的一个字符串。

然后，您将用 NaN 的正确定义替换“NAN”字符串。为了做到这一点，我们将使用 numpy 库，您只需要这样使用它:

```
df.replace('NAN',np.NaN,inplace=True)When we have done that, we will then see different result on the previous method : df.isna().sum() ## The result will look like this :index0                 0
terrace                0
bedrooms             319
construction_year     49
date_month             0
date_year              0
date_year_month        0
floor                113
rooms                 34
price_room            35
price_surface        212
price                  1
surface              211
zip                    0
dtype: int64
```

在这里，我们开始了解这些数据的实际情况。
你看，我们缺少很多卧室信息的数据。
在接下来的步骤中，我们的任务是尽可能减少 NaN。
我们将尝试假设一些不同的策略来填充这里的 NaN。

如果你想看到你的数据集的整体质量，你实际上可以使用一些计算。
像这样:

```
(df.isnull().sum() / len(df))*100 ## result will look like this :index0                0.000000
terrace               0.000000
bedrooms             36.708861
construction_year     5.638665
date_month            0.000000
date_year             0.000000
date_year_month       0.000000
floor                13.003452
rooms                 3.912543
price_room            4.027618
price_surface        24.395857
price                 0.115075
surface              24.280783
zip                   0.000000
dtype: float64
```

所以我提供的数据并不完美，但你可能会看到很多比这更糟糕的数据。
了解数据:36.7%的行没有卧室信息。

真正令人不安的是，我们遗漏了一个特定数据点的价格信息。
价格真的很难预测，但让我们看看是什么数据，我们可能会有所发现:

```
df[df['price'].isna()].T ##the result will look like this :367index0 4terrace yesbedrooms NaNconstruction_year 2016 date_month 8date_year 2016date_year_month 2016_08floor NaNrooms 5price_room NaNprice_surface NaNprice NaNsurface 209zip 81479
```

你会发现我们遗漏了价格和卧室数量。
我们有两种选择:

*   用我们掌握的信息预估价格(5 室，209 平米，建造年份:2016 年)
*   从数据集中移除此线

在我们的案例中，2016 年建造的 200 平方米可能会超出我的数据集限制(<600 K€). I will then remove the line from the data set.

From my extract, I saw that the index of that line is 367.
我可以通过以下简单操作删除该线:

```
df.drop(df.index[367],inplace=True)
```

但是如果你有不止一个索引，你怎么管理呢？

你只需要大声说出你的病情指数:

```
df.drop(df[df['price'].isna()].index,inplace = True)
```

由于我们正在删除无用的信息，我们可以借此机会删除 index0 列，因为您可能已经看到 pandas 会自动为您的数据帧生成一个索引。
为了完全删除一列，你需要实现这个动作:

```
df.drop(['index0'],axis=1,inplace=True) #axis = 1 make sure that you are deleting column
```

现在我们想让熊猫识别正确的数据类型。
拥有正确的数据类型将允许我们实现对数值类型(int 或 float)的数值运算。
如果我们有正确的数据类型集，我们将能够以更好的方式处理 NaN。

我建议总是将数值设置为 int。
为什么 **int** ？因为它更容易处理和解释，但你可以试着让一切都浮动起来。
浮点数可以表示整数(2.0 == 2)但整数不能表示某些浮点数(2.1！= 2)

一个简单的循环可能是这样的:

```
for col in list(df.columns): # for each column
   try:
       df[col] = df[col].astype(int) #try to set it as int
   except:
       df[col] = df[col].astype(str) # if not sucessful try to set it as string
```

当您实现这个操作时，您可以看到并不是所有的列都被更改为数字。
这主要是因为某些列不是整数而是浮点数。(所以一个小数)

你可以实现另一个循环:

```
for col in list(df.columns):
    if df[col].dtypes == object:
        try:
            df[col] = df[col].astype(float)
        except:
            df[col] = df[col].astype(str)Then by doing a simple overview, your data start to look like something you can work :df.dtypes # it will show something like this :
```

Out[32]:

```
terrace               object
bedrooms             float64
construction_year    float64
date_month             int32
date_year              int32
date_year_month        int32
floor                 object
rooms                float64
price_room           float64
price_surface        float64
price                  int32
surface              float64
zip                    int32
dtype: object
```

# 填充 NaN

现在我们已经准备好处理不同类型的数据，我们将使用我们的大脑来处理不同的数据类型并填充 NaN 值。
如前所述，在进行任何分析之前，您可能想要感觉缺失值的最大值，以便计算最大值。

**填写楼层栏**你应该还记得，这一栏告诉我们这是不是底层。
通过这个简单的方法，你可以看到不同的值和它们出现的次数。

```
df['floor'].value_counts() ### Should show something like this :up floor 687 
nan 112 
ground floor 69 
Name: floor, dtype: int64
```

您需要做的是计算楼上和楼下之间的分布，并将该分布应用于其余数据。你可以很容易地计算出底层公寓的数量约占本栏的 10%。
因此，我们需要用“底层”值替换 na 的 1/10。

我们可以通过简单地创建一个函数来实现:

```
def fill10pct(): 
    if np.random.random_integers(0,9) == 0: 
        return 'ground floor' 
    else: 
        return 'up floor'
```

然后，您需要将它应用到您的行:

```
for index, value in df.iterrows(): 
    if df.loc[index,'floor']=='nan': 
        df.loc[index,'floor'] = fill10pct()
```

您可以运行 df['floor']。value_counts()来检查是否保持了分布。

填充房间现在我们将尝试填充房间。
我们将在这里尝试一种不同的技术。我们有一些信息可以帮助我们确定总共有多少个房间。
有卧室信息，所以在德国的邮政公寓中，卧室是唯一从房间数中分开统计的房间。
也就是:三室是指两个卧室，一个大房间，一个厨房和一个卫生间。
所以我们可以说房间的数量是卧室的数量+1，但是如果我们没有卧室的数量呢？那么，为了简单起见，我们可以说房间的数量是 2。这是我能拿到的最低工资。为此我们将创造我们的 2 个条件(有或没有若干卧室)

```
conditions = [ (df['rooms'].isnull()) & (df['bedrooms'].isnull()), (df['rooms'].isnull()) & (df['bedrooms'].notnull())] 
choices = [2,df['bedrooms']+1]
df['rooms'] = np.select(conditions, choices, default=2)
```

我们将使用 numpy select 函数来决定应用哪个选项

```
df['rooms'] = np.select(conditions, choices, default=2)
```

当你知道怎么做时，这很容易。:)它太简单了，所以我们将把它做得更健壮一点，并在其中集成曲面。我们会说，如果表面大于 75 平方米，我们将设置房间数为 3。

```
conditions =[ 
(df['rooms'].isnull()) & (df['bedrooms'].isnull()) & (df['surface'].isnull()), 
(df['rooms'].isnull()) & (df['bedrooms'].isnull()) & (df['surface']>75), 
(df['rooms'].isnull()) & (df['bedrooms'].notnull()), ] choices = [2,3,df['bedrooms']+1] 
df['rooms'] = np.select(conditions,choices,default=2)
```

**填充卧室**
填充卧室其实是相反的逻辑。如果你有房间的数量，你实际上可以猜出卧室的数量。
这一次，我们将使用从 numpy 中选择的方法:

```
df['bedrooms'] = np.where(df['bedrooms'].isnull(), df['rooms']-1, df['bedrooms'])
```

**填充表面**
对于表面，我们缺少 211 个数据点。我们可以有相同的策略，而不是房间的数量。外推现有公寓的表面以填充表面的缺失值。如果我们能得到 2、3 和 4 房间公寓的平均面积，我们就能给这些房间分配平均值。为了实现这一点，我们将使用熊猫最重要的功能之一。**组由**组成。

```
df.groupby(['rooms'])['surface'].mean() ##it should give you something like :
2.0 89.988924 
3.0 91.100000 
4.0 100.40000
```

有趣的是，你的 2 室和 3 室公寓的平均面积没有太大差别。很可能我们的数据没有那么干净，一些三室公寓被当成了两室公寓。

```
conditions = [
(df['rooms']==2 & df['surface'].isnull()), 
(df['rooms']==3 & df['surface'].isnull()), 
(df['rooms']==4 & df['surface'].isnull()) ] 
choices = [90,91.1,100] 
df['surface'] = np.select(conditions,choices,default=90.5)#default in between 2 and 3 rooms
```

**填写建造年份**
在这个例子中，这是相当困难的，因为建造年份可能是随机的。你无法根据之前的数据猜测建造年份。
在这种情况下，为了不虚报数据，我会选择用这个维度的含义来填补空白。
这是另一种你可以在熊猫身上快速使用的方法:

```
df['construction_year'].fillna(df['construction_year'].mean(),inplace=True)
```

**填充剩余的…**
正如你在做 df.isnull()时可能注意到的。sum()其他一些列有 NaN，但它们实际上是其他列的计算结果。所以你只需要用你的主列重新计算，所有的 NaN 就会消失。

如果您正在使用 Python 和 Pandas 进行数据分析，我希望这篇关于如何清理数据的教程能够对您有所帮助。
这是处理数据的一个非常重要的部分，如果您计划进行机器学习，清理数据并从 NaN 数据点中创造价值是机器学习最重要的方面之一。

如标题所示，我们将有第二篇文章，在那里我们实际分析数据，我们可能会尝试做一些可视化。

不要犹豫，评论并给出你的提示来分析这个数据集。
如上所述，数据集(干净的和不干净的)和 Jupyter 笔记本都在我的 Github 账户上:[https://github.com/pitchmuc/munich_housemarket](https://github.com/pitchmuc/munich_housemarket)