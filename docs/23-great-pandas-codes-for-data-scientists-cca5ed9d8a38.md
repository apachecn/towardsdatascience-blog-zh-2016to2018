# 数据科学家的 23 只大熊猫代码

> 原文：<https://towardsdatascience.com/23-great-pandas-codes-for-data-scientists-cca5ed9d8a38?source=collection_archive---------6----------------------->

![](img/7e3521500e74b2399fe2530265e244a9.png)

> 想获得灵感？快来加入我的 [**超级行情快讯**](https://www.superquotes.co/?utm_source=mediumtech&utm_medium=web&utm_campaign=sharing) 。😎

这里有 23 个熊猫代码，供数据科学家帮助更好地理解你的数据！

# 基本数据集信息

## (1)读入 CSV 数据集

```
pd.DataFrame.from_csv(“csv_file”) 
```

运筹学

```
pd.read_csv(“csv_file”)
```

## (2)读入 Excel 数据集

```
pd.read_excel("excel_file")
```

## (3)将您的数据框直接写入 csv

逗号分隔，不带索引

```
df.to_csv("data.csv", sep=",", index=False)
```

## (4)基本数据集特征信息

```
df.info()
```

## (5)基本数据集统计

```
print(df.describe())
```

## (6)打印表格中的数据框

```
print(tabulate(print_table, headers=headers))
```

其中“print_table”是列表的列表,“headers”是字符串头的列表

## (7)列出列名

```
df.columns
```

# 基本数据处理

## (8)丢弃丢失的数据

```
df.dropna(axis=0, how='any')
```

返回给定轴上的标签被忽略的对象，其中部分或全部数据交替丢失

## (9)替换丢失的数据

```
df.replace(to_replace=None, value=None)
```

用“值”替换“to_replace”中给定的值。

## (10)检查 nan

```
pd.isnull(object)
```

检测缺失值(数值数组中的 NaN，对象数组中的 None/NaN)

## (11)删除一个特征

```
df.drop('feature_variable_name', axis=1)
```

对于行，轴为 0；对于列，轴为 1

## (12)将对象类型转换为浮点型

```
pd.to_numeric(df["feature_name"], errors='coerce')
```

将对象类型转换为数字，以便能够执行计算(如果它们是字符串)

## (13)将数据帧转换为 numpy 数组

```
df.as_matrix()
```

## (14)获得数据帧的前“n”行

```
df.head(n)
```

## (15)按特征名称获取数据

```
df.loc[feature_name]
```

# 对数据帧进行操作

## (16)将函数应用于数据帧

这将使数据框的“高度”列中的所有值乘以 2

```
df["height"].apply(**lambda** height: 2 * height)
```

运筹学

```
def multiply(x):
    return x * 2df["height"].apply(multiply)
```

## (17)重命名列

这里，我们将数据框的第 3 列重命名为“大小”

```
df.rename(columns = {df.columns[2]:'size'}, inplace=True)
```

## (18)获取列的唯一条目

在这里，我们将获得列“name”的唯一条目

```
df["name"].unique()
```

## (19)访问子数据帧

在这里，我们将从数据框中选择“名称”和“大小”列

```
new_df = df[["name", "size"]]
```

## (20)关于您的数据的摘要信息

```
**# Sum of values in a data frame** df.sum()**# Lowest value of a data frame** df.min()**# Highest value** df.max()**# Index of the lowest value** df.idxmin()**# Index of the highest value** df.idxmax()**# Statistical summary of the data frame, with quartiles, median, etc.** df.describe()**# Average values** df.mean()**# Median values** df.median()**# Correlation between columns** df.corr()**# To get these values for only one column, just select it like this#** df["size"].median()
```

## (21)整理你的数据

```
df.sort_values(ascending = False)
```

## (22)布尔索引

这里我们将过滤名为“size”的数据列，只显示等于 5 的值

```
df[df["size"] == 5]
```

## (23)选择值

让我们选择“大小”列的第一行

```
df.loc([0], ['size'])
```

# 喜欢学习？

在[推特](https://twitter.com/GeorgeSeif94)上关注我，我会在那里发布所有最新最棒的人工智能、技术和科学！也在 LinkedIn 上与我联系！