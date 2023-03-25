# 在 Python 中导入数据

> 原文：<https://towardsdatascience.com/importing-data-in-python-f6602add57b7?source=collection_archive---------15----------------------->

![](img/7836b437e8227b803f50744df9999bf3.png)

## 关于导入不同数据的不同方法的总结很少

最近，我在 DataCamp 完成了两门关于 Python 中数据导入的课程，我对可以用来获取数据的资源数量感到非常惊讶。在这里，我想总结所有这些方法，同时敏锐我的知识。我也认为对其他人来说这也是有用的。那么，我们开始吧。

有各种各样的文件可以用作数据源:

*   平面文件— csv、txt、tsv 等。
*   腌制的文件
*   excel 电子表格
*   SAS 和 Stata 文件
*   HDF5
*   矩阵实验室
*   SQL 数据库
*   网页
*   蜜蜂

## 平面文件

平面文件——txt、CSV——很容易，使用 numpy 或 pandas 导入它们的方法很少。

*numpy.recfromcsv* —加载存储在逗号分隔文件中的 ASCII 数据。返回的数组是记录数组(如果 usemask=False，请参见 recarray)或被屏蔽的记录数组(如果 usemask=True，请参见 ma.mrecords.MaskedRecords)。

```
data = np.recfromcsv(file)
```

*numpy.loadtxt* —该函数旨在成为简单格式文件的快速阅读器。 *genfromtxt* 函数提供了更复杂的处理，例如，具有缺失值的行。

```
data = np.loadtxt('file.csv', delimiter=',', skiprows=1, usecols=[0,2])
```

*numpy.genfromtxt* —从文本文件中加载数据，缺失值按指定处理。更复杂的函数，有许多参数来控制您的导入。

```
data = np.genfromtxt('titanic.csv', delimiter=',', names=True, dtype=None)
```

有了 pandas，这就更容易了——一行代码就可以把你的文件放在一个数据帧中。还支持可选的迭代或者将文件分成块。

```
data = pd.read_csv(file, nrows=5, header=None, sep='\t', comment='#', na_values='Nothing')
```

## 泡菜

泡菜是什么鬼东西？它用于序列化和反序列化 Python 对象结构。python 中的任何对象都可以被腌制，以便保存在磁盘上。pickle 所做的是在将对象写入文件之前先“序列化”对象。Pickling 是一种转换 python 对象(list、dict 等)的方法。)转换成字符流。这个想法是，这个字符流包含在另一个 python 脚本中重建对象所需的所有信息。下面的代码将打印一个在某处创建并存储在文件中的字典——很酷，不是吗？

```
import pickle 
with open('data.pkl', 'rb') as file: 
       d = pickle.load(file)print(d)
```

## 擅长

pandas.read_excel 将 excel 表格读入 pandas 数据框架，并有许多自定义数据导入功能，这是前所未有的愉悦(听起来像电视广告:D)。但这是真的——这个函数的文档非常清楚，您实际上可以对这个 Excel 文件做任何您想做的事情。

```
df = pd.read_excel('file.xlsx', sheet_name='sheet1')
```

## SAS 和 Stata

SAS 代表统计分析软件。SAS 数据集包含组织为观察值(行)和变量(列)表的数据值。要打开这种类型的文件并从中导入数据，下面的代码示例会有所帮助:

```
from sas7bdat import SAS7BDAT
with SAS7BDAT('some_data.sas7bdat') as file: 
     df_sas = file.to_data_frame()
```

Stata 是一个强大的统计软件，使用户能够分析、管理和生成数据的图形可视化。它主要由经济学、生物医学和政治学领域的研究人员用来检查数据模式。数据存储在。dta 文件，最好的导入方式是 *pandas.read_stata*

```
df = pd.read_stata('file.dta')
```

## HDF5

分层数据格式(HDF)是一组文件格式(HDF4、HDF5)，旨在存储和组织大量数据。HDF5 是一种独特的技术套件，能够管理极其庞大和复杂的数据集合。HDF5 简化了文件结构，仅包含两种主要类型的对象:

*   数据集，是同质类型的多维数组
*   组，是可以容纳数据集和其他组的容器结构

这就产生了真正的分层的、类似文件系统的数据格式。事实上，甚至可以使用类似 POSIX 的语法 */path/to/resource* 来访问 HDF5 文件中的资源。元数据以用户定义的命名属性的形式存储在组和数据集上。然后可以使用数据集、组和属性构建更复杂的表示图像和表格的存储 API。

为了导入 HDF5 文件，我们需要 *h5py* 库。下面的代码样本让一切变得更简单，对我来说完全可以理解。

```
import h5py # Load file: 
data = h5py.File('file.hdf5', 'r') # Print the keys of the file 
for key in data.keys(): 
    print(key)# Now when we know the keys we can get the HDF5 group
group = data['group_name'] # Going one level deeper, check out keys of group 
for key in group.keys(): 
    print(key)# And so on and so on
```

## 矩阵实验室

很多人用 MATLAB 工作并将数据存储在。mat 文件。那些文件是什么？这些文件包含 MATLAB 工作空间中分配给它们的变量和对象的列表。不足为奇的是，它在 Python 中作为字典导入，其中的键是 MATLAB 变量和值——分配给这些变量的对象。为了读写 MATLAB 文件*使用了 scipy.io* 包。

```
import scipy.io 
mat = scipy.io.loadmat('some_project.mat')
print(mat.keys())
```

## 关系数据库

使用驱动程序连接到数据库，我们可以直接从那里获取数据。通常它意味着:创建连接、连接、运行查询、获取数据、关闭连接。一步一步来做是可能的，但是在熊猫身上，我们有一个很棒的功能来为我们做这件事，所以为什么要自寻烦恼呢？它只需要一个可以用 *sqlalchemy* 包创建的连接。下面是连接到 sqlite 数据库引擎并从中获取数据的示例:

```
from sqlalchemy import create_engine 
import pandas as pd 
# Create engine 
engine = create_engine('sqlite:///localdb.sqlite')# Execute query and store records in DataFrame 
df = pd.read_sql_query("select * from table", engine)
```

## 来自网络的数据

应该就此写一篇单独的文章，但我将强调几件事，至少知道从哪里开始。首先，如果我们有一个文件的直接 url，我们可以使用标准的*pandas . read _ CSV/pandas . read _ excel*函数在参数“file=”中指定它

```
df = pd.read_csv('https://www.example.com/data.csv', sep=';')
```

除此之外，要从 web 获取数据，我们需要使用 HTTP 协议，尤其是 get 方法(有很多这样的方法，但是对于导入，我们不需要更多)。包*请求*在这方面做得非常出色。要从 *requests.get* 收到的响应中访问文本，我们只需使用 method .text。

```
import requests 
r = requests.get('http://www.example.com/some_html_page') print(r.text)
```

r.text 会给我们一个包含所有 html 标签的网页——不是很有用，不是吗？但是有趣的事情开始了。我们有一个 *BeautifulSoup* 包，它可以解析 HTML 并提取我们需要的信息，在本例中是所有超链接(继续前面的例子):

```
from bs4 import BeautifulSoup
html_doc = r.text # Create a BeautifulSoup object from the HTML 
soup = BeautifulSoup(html_doc)# Find all 'a' tags (which define hyperlinks) 
a_tags = soup.find_all('a') # Print the URLs to the shell 
for link in a_tags: 
    print(link.get('href'))
```

## 应用程序接口

在计算机编程中，应用编程接口(API)是一组子例程定义、通信协议和用于构建软件的工具。一般来说，它是各种组件之间的一组明确定义的通信方法。有许多不同的 API，首先要做的是检查文档，但事实是几乎所有的 API 都以 JSON 格式返回数据。我们必须能够捕捉到结果。并且再次打包*的请求*会帮我们处理。(我们必须发送 HTTP GET 请求来从 API 获取数据)。

```
import requests 
r = requests.get('https://www.example.com/some_endpoint') # Decode the JSON data into a dictionary: 
json_data = r.json()# Print each key-value pair in json_data 
for k in json_data.keys(): 
    print(k + ': ', json_data[k])
```

正如我们所见，数据无处不在，我们必须知道获取数据的所有方法。至此，我的简短总结到此结束。希望它不仅对我有用。