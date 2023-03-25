# Python 中优化的 I/O 操作

> 原文：<https://towardsdatascience.com/optimized-i-o-operations-in-python-194f856210e0?source=collection_archive---------2----------------------->

## 使用 Python 的数据科学堆栈加速分析的输入/输出技巧

![](img/63befdc50b7d03828b9aecc55cc90530.png)

在处理数据密集型应用程序时，我经常面临输入/输出(I/O)挑战，这是每个性能关键型应用程序的瓶颈。随着存储数据量的增加，有必要将数据存储在磁盘中，以通过将数据从磁盘加载到 RAM 来弥补 RAM 的不足，反之亦然。因此，在处理金融数据或任何科学数据时，I/O 操作本质上是非常重要的任务。

通过这篇文章，我试图揭示一些图书馆和他们的商业技巧。Python 有内置的功能，可以用来将对象存储在磁盘上，并从磁盘读取到 RAM 中。此外，Python 在处理文本文件和 SQL 数据库时非常健壮。Pandas 库提供了大量的类和方法来读写各种格式的文件。

我们将在这里研究以下领域的数据存储和检索方法:

1.  使用 Pickle 模块的序列化存储
2.  对文本数据的 I/O 操作
3.  SQL 数据库
4.  具有 PyTables 的 I/O

在 Python 语言中优化 I/O 操作时要考虑的两个主要因素是效率(性能)和灵活性。让我们直入主题:

# 使用 Pickle 模块的序列化存储

Python 语言中有许多模块，可以在大规模部署环境中轻松使用。

## 使用 pickle 模块读写文件

您需要将数据存储在您的磁盘上，以便以后共享、记录或使用。我们有 pickle 模块，它序列化 python 对象以快速进行读写操作。

```
# On running the above code snippet, you'll see:CPU times: user 40.9 ms, sys: 14 ms, total: 54.9 ms
Wall time: 54.5 ms
```

随机浮点构建一个 9MB 的文件，该文件被序列化为字节流并在 54.9 毫秒内写入磁盘。您将使用 pickle 模块的 ***dump*** 和 ***load*** 函数分别写入和读取文件。要断言序列化和反序列化的数据，可以使用 Numpy 的 *allclose* 方法。你可以这样做:

```
np.allclose(np.array(a1), np.array(a2))# here a2 is the deserialized object after reading the same file
# using the load function.
```

因此，pickle 模块存储了 python 列表、dict 等。在将它们转换成磁盘上的字符流之后。这里的关键是这个字节流包含了在另一个 python 脚本中重建对象所必需的信息。

# 对文本数据的 I/O 操作

Python 一直是最受欢迎的语言，尤其是在处理文本文件时，因为它具有处理文本数据的健壮性和易用性。有几个选项来操作字符串对象和一般的文本文件。

要编写 CSV(逗号分隔值)，我们可以使用 write 和 readline 方法:

```
csv_file.write(header)# time is time array and data is the dummy numpy array
**for** time, (a, b, c, d, e) **in** zip(time, data): 
  s = '%s,%f,%f,%f,%f,%f**\n**' % (time, a, b, c, d, e)                csv_file.write(s)csv_file.close()# to read the file, we can use readlines function
content = csv_file.readlines()
```

虽然 python 提供了处理文本文件的方法，但是我们有 pandas 库，它可以读写各种数据格式，并且更好更容易获得。

无论是 CSV(逗号分隔值)、SQL(结构化查询语言)、XLS/XLSX(Microsoft Excel 文件)、JSON(Javascript 对象表示法)还是 HTML(超文本标记语言)。

熊猫让整个 CSV 文件的读写过程变得更加方便、简洁、快捷。

```
%time data.to_csv(filename + '.csv')# CPU times: user 5.59 s, sys: 137 ms, total: 5.69 s# And to read the files back from the diskpd.read_csv(<path to the CSV file>)
```

# SQL 数据库

Python 附带了对 SQL 数据库 SQLite3 的支持。使用 python，我们可以处理几乎任何类型的数据库(SQL 或 NoSQL)。

SQL 查询被写成字符串对象，其中语法和数据类型取决于所使用的数据库。说明了在 SQLite 数据库中通过 python 创建 Todo 表:

```
import sqlite3 as sq# query string to create the tablequery = 'CREATE TABLE TODO_NUMBER (Num1 real, Num2 real, Num3 real)'
con = sq.connect(path + 'todo.db')
con.execute(query)
con.commit()
```

让我们尝试在创建的数据库中插入一些数据，

```
data = np.random.standard_normal((1000000, 3))
%%time
con.executemany('INSERT INTO TODO_NUMBER VALUES (?, ?, ?, ?, ?)', data)
con.commit()# Time taken: CPU times: user 10.3 s, sys: 316 ms, total: 10.6 s
Wall time: 11 s
```

将 100 万行写入数据库是一项有点繁重且耗时的任务。读取数据库要快得多:

```
con.execute('SELECT * FROM TODO_NUMBER').fetchall() 
```

如果在数据库中处理大量的数字和数组，可以利用 Numpy 数组将数据直接读入 numpy ndarray。

```
np_query = 'SELECT * FROM TODO_NUMBER WHERE Num1 > 0 AND Num2 < 0'
res = np.array(con.execute(np_query).fetchall()).round(3)
```

这是一个非常好的技巧，可以毫不费力地读取和绘制查询结果。为了使读取更加高效和优化，我们应该使用 pandas 读取整个表和查询结果。当整个表被加载到内存中时，分析和处理变得更快。这是通过使用子库 *pandas.io.sql* 实现的

```
import pandas.io.sql as pds
data_df = pds.read_sql('SELECT * FROM TODO_NUMBERS', con)
```

该表现在被加载到内存中，这使得处理速度更快。使用 SQLite3 需要几秒钟的 SQL 查询在内存中使用 pandas 时在几毫秒内完成:

```
%time data_df[(data_df['Num1'] > 0) & (data_df['Num2'] < 0)].head()# CPU times: user 50 ms, sys: 0 ns, total: 50 ms# Wall time: 49.9 ms
```

我们可以用 pandas 掌握更多复杂的查询，它会比 SQL 更快地产生结果，但它不能取代 SQL。鉴于 pandas 能够复制 SQL 查询，我们可以使用 pandas 的内存处理显著加快分析速度。

> 这里需要注意的一点是，pandas 并不是用来取代 SQL 数据库的，目前它也不能取代 SQL 数据库。Pandas 不支持关系数据结构。

# 具有 PyTables 的 I/O

PyTables 是针对 HDF5 数据库/文件标准的 Python 绑定。它是专门为增强 I/O 操作的性能和充分利用可用硬件而设计和开发的。它在加速分析和更快生成输出方面做得非常好。PyTables 数据库可以容纳许多表，它支持压缩和索引，还支持对表的重要查询。

PyTables 具有基于文件的数据库格式。让我们看一下表格的工作原理，

这将为我们创建一个带有指定数据类型的必需字段的表。现在让我们填充数据库，我们必须创建一些随机值，并将它们逐行写入表中，如下所示:

但是同样，我们有一种更优化和 Pythonic 化的方法来达到相同的结果，即利用 NumPy 结构化数组:

```
dty = np.dtype([('Num1', 'i4'), ('Num2', '<i4')])sarray = np.zeros(len(ran_int), dtype=dty)
```

现在我们已经在表中设置了完整的数据，这一切都归结为表的创建，如下所示:

```
%%time
h5.create_table('/', 'ints_from_array', sarray,title='Integers', expectedrows=rows, filters=filters)
```

这种方法速度更快，而且我们用更少的代码行完成了同样的结果。我们可以使用以下命令删除重复的表:

```
h5.remove_node(‘/’, ‘ints_from_array’)
```

pandas 和 PyTables 都能够处理复杂的类似 SQL 的查询、索引和选择。就 I/O 操作而言，它们都经过了速度设计和优化。

使用 PyTables 的一个主要优点是它的压缩操作方式。它使用压缩不仅可以节省磁盘空间，还可以提高 I/O 操作的性能。

# 结论

一般来说，金融或科学中的许多企业级应用领域只需基于阵列的数据建模就能取得成功。在大多数情况下，结合使用 NumPy 和 PyTables I/O 功能可以显著提高性能。事实证明，基于 HDF5 的商店是所有方法的独特补充。

如果你有复杂的数据结构，表现出单个对象/表之间的许多关系，关系数据库有它的优点。这在某些情况下可能证明是合理的，在这些情况下，与纯基于 NumPy ndarray 或基于 pandas DataFrame 的方法相比，存在性能劣势。

# Harshit 的数据科学

通过这个渠道，我计划推出几个涵盖整个数据科学领域的系列。以下是你应该订阅[频道](https://www.youtube.com/channel/UCH-xwLTKQaABNs2QmGxK2bQ)的原因:

*   该系列将涵盖每个主题和副主题的所有必需/要求的高质量教程。
*   解释了为什么我们在 ML 和深度学习中做这些事情的数学和推导。
*   与谷歌、微软、亚马逊等公司的数据科学家和工程师以及大数据驱动型公司的首席执行官的播客。
*   项目和说明，以实现迄今为止所学的主题。

你可以在 [LinkedIn](https://www.linkedin.com/in/tyagiharshit/) 、 [Twitter](https://twitter.com/tyagi_harshit24) 或 [Instagram](https://www.instagram.com/upgradewithharshit/?hl=en) 上与我联系(在那里我谈论健康和福祉。)

注意:在这些黑暗的时期，自我隔离为自我提升腾出了一些空间，我们可以利用这些空间来发展新的技能、爱好，并帮助未来的自己。