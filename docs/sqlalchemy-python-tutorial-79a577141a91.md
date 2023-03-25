# SQLAlchemy-Python 教程

> 原文：<https://towardsdatascience.com/sqlalchemy-python-tutorial-79a577141a91?source=collection_archive---------0----------------------->

我们经常遇到作为关系数据库的数据。为了使用它们，我们通常需要编写原始的 SQL 查询，将它们传递给数据库引擎，并将返回的结果作为普通的记录数组进行解析。

![](img/285c31f6086fb575feb6ebc687b6f090.png)

SQLAlchemy 提供了一种很好的与数据库交互的“Pythonic 式”方法。因此，您可以利用 SQLAlchemy 的 Pythonic 框架来简化您的工作流并更高效地查询数据，而不是处理传统 SQL(如 MySQL、PostgreSQL 或 Oracle)的特定方言之间的差异。

## 关于数据科学的其他故事可以在[这里](https://medium.com/hacking-datascience)找到

## 安装软件包

```
pip install sqlalchemy
```

## 连接到数据库

为了开始与数据库交互，我们首先需要建立一个连接。

```
import sqlalchemy as db
engine = db.create_engine('dialect+driver://user:pass**@host**:port/db')
```

连接到各种数据库的一些例子可以在 [**这里**](http://docs.sqlalchemy.org/en/latest/core/engines.html#postgresql) 找到

## 查看表格详细信息

SQLAlchemy 可以用来通过反射从数据库中自动加载表。反射是读取数据库并基于该信息构建元数据的过程。

**示例**

## 询问

`Table`和`MetaData`已经被导入。元数据以`metadata.`的形式提供

ResultProxy: *由* `*.execute()*` *方法返回的对象。可以通过多种方式使用它来获取查询返回的数据。*

ResultSet: *在 ResultProxy 上使用诸如* `*.fetchall()*` *之类的获取方法时，查询中要求的实际数据。*

**处理大型 ResultSet**

我们使用`.fetchmany()`来加载最佳行数，并克服大数据集情况下的内存问题

```
while flag:
    partial_results = ResultProxy.fetchmany(50)
    if(partial_results == []): 
	flag = False
    //
	code
   //
ResultProxy.close()
```

**转换为数据帧**

```
df = pd.DataFrame(ResultSet)
df.columns = ResultSet[0].keys()
```

**过滤数据**

让我们看一些原始 SQLite 查询和使用 SQLAlchemy 的查询的例子。

**其中**

```
**SQL :**
SELECT * FROM census 
WHERE sex = F**SQLAlchemy :** db.select([census]).*where*(census.columns.sex == 'F')
```

**中的**

```
**SQL :**
SELECT state, sex
FROM census
WHERE state IN (Texas, New York)**SQLAlchemy :**
db.select([census.columns.state, census.columns.sex]).where(census.columns.state.*in_*(['Texas', 'New York']))
```

**与，或，非**

```
**SQL :**
SELECT * FROM census
WHERE state = 'California' AND NOT sex = 'M'**SQLAlchemy :**
db.select([census]).where(db.*and_*(census.columns.state == 'California', census.columns.sex != 'M'))
```

**排序依据**

```
**SQL :**
SELECT * FROM census
ORDER BY State DESC, pop2000**SQLAlchemy :**
db.select([census]).*order_by*(db.desc(census.columns.state), census.columns.pop2000)
```

**功能**

```
**SQL :**
SELECT SUM(pop2008)
FROM census**SQLAlchemy :**
db.select([db.*func.sum*(census.columns.pop2008)])
```

其他功能包括*平均值、计数、最小值、最大值* …

**分组依据**

```
**SQL :**
SELECT SUM(pop2008) as pop2008, sex
FROM census**SQLAlchemy :**
db.select([db.func.sum(census.columns.pop2008).label('pop2008'), census.columns.sex]).*group_by*(census.columns.sex)
```

**截然不同的**

```
**SQL :**
SELECT DISTINCT state
FROM census**SQLAlchemy :**
db.select([census.columns.state.*distinct*()])
```

**案例&投**

`case()`表达式接受匹配的条件列表和条件匹配时返回的列，如果没有匹配的条件，则接受一个`else_`。

`cast()`将表达式转换成特定类型的函数

**举例**

当结果只包含单个值时，我们对结果使用`.scalar`

**加入**

如果您有两个已经建立了关系的表，那么只需将每个表中需要的列添加到 select 语句中，就可以自动使用这种关系。

```
select([census.columns.pop2008, state_fact.columns.abbreviation])
```

**例子**

## 创建数据并将其插入表格

通过将不存在的数据库传递给引擎，sqlalchemy 会自动创建一个新的数据库。

## 更新数据库中的数据

```
db.update(table_name).values(attribute = new_value).where(condition)
```

## 删除表格

```
db.delete(table_name).where(condition)
```

## 放下一张桌子

```
table_name.drop(engine) #drops a single tablemetadata.drop_all(engine) #drops all the tables in the database
```

这个故事的 IPython 笔记本和其他资产可以在 [**这里**](https://github.com/vinaykudari/hacking-datascience/tree/master/notebooks/sqlalchemy) 找到

再见😃

**我的其他故事**

*   [在 15 行代码下构建任意深度学习分类器](/build-any-deep-learning-image-classifier-under-15-lines-of-code-using-fastai-v2-123c81c13b)
*   [如何保护自己的互联网身份？](https://medium.com/@vinaykudari/protecting-your-internet-identity-4d8ef368caa7)