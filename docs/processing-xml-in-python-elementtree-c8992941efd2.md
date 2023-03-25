# 用 Python 处理 XML—element tree

> 原文：<https://towardsdatascience.com/processing-xml-in-python-elementtree-c8992941efd2?source=collection_archive---------0----------------------->

## 初学者指南

![](img/662f0a1c298d7845f4618bd665e70cd4.png)

“shallow focus photography of spider web” by [Robert Anasch](https://unsplash.com/@diesektion?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

了解如何使用 Python ElementTree 包解析、浏览、修改和填充 XML 文件，用于循环和 XPath 表达式。作为一名数据科学家，您会发现理解 XML 对于 web 抓取和解析结构化文档的一般实践都是非常有用的

**可扩展标记语言** (XML)是一种标记语言，它通过定义一组机器可读和人类可读格式的规则来编码文档。从 SGML(标准通用标记语言)扩展而来，它让我们描述文档的结构。在 XML 中，我们可以定义自定义标签。我们也可以使用 XML 作为交换信息的标准格式。

*   XML 文档有几个部分，叫做 ***元素*** ，由一个开始**和一个结束*标签*** 定义。标签是一种标记结构，以`<`开始，以`>`结束。开始标记和结束标记之间的字符(如果有的话)是元素的内容。元素可以包含标记，包括其他元素，这些元素称为“子元素”。
*   最大的顶级元素称为 ***根*** ，它包含所有其他元素。
*   属性是存在于开始标记或空元素标记中的名称-值对。XML 属性只能有一个值，并且每个属性在每个元素中最多只能出现一次。

这里有一张`movies.xml`的快照，我们将在本教程中使用:

```
<?xml version="1.0"?>
<collection>
    <genre category="Action">
        <decade years="1980s">
            <movie favorite="True" title="Indiana Jones: The raiders of the lost Ark">
                <format multiple="No">DVD</format>
                <year>1981</year>
                <rating>PG</rating>
                <description>
                'Archaeologist and adventurer Indiana Jones 
                is hired by the U.S. government to find the Ark of  the Covenant before the Nazis.'
                </description>
            </movie>
               <movie favorite="True" title="THE KARATE KID">
               <format multiple="Yes">DVD,Online</format>
               <year>1984</year>
               <rating>PG</rating>
               <description>None provided.</description>
            </movie>
            <movie favorite="False" title="Back 2 the Future">
               <format multiple="False">Blu-ray</format>
               <year>1985</year>
               <rating>PG</rating>
               <description>Marty McFly</description>
            </movie>
        </decade>
        <decade years="1990s">
            <movie favorite="False" title="X-Men">
               <format multiple="Yes">dvd, digital</format>
               <year>2000</year>
               <rating>PG-13</rating>
               <description>Two mutants come to a private academy for their kind whose resident superhero team must oppose a terrorist organization with similar powers.</description>
            </movie>
            <movie favorite="True" title="Batman Returns">
               <format multiple="No">VHS</format>
               <year>1992</year>
               <rating>PG13</rating>
               <description>NA.</description>
            </movie>
               <movie favorite="False" title="Reservoir Dogs">
               <format multiple="No">Online</format>
               <year>1992</year>
               <rating>R</rating>
               <description>WhAtEvER I Want!!!?!</description>
            </movie>
        </decade>    
    </genre>

    <genre category="Thriller">
        <decade years="1970s">
            <movie favorite="False" title="ALIEN">
                <format multiple="Yes">DVD</format>
                <year>1979</year>
                <rating>R</rating>
                <description>"""""""""</description>
            </movie>
        </decade>
        <decade years="1980s">
            <movie favorite="True" title="Ferris Bueller's Day Off">
                <format multiple="No">DVD</format>
                <year>1986</year>
                <rating>PG13</rating>
                <description>Funny movie on funny guy </description>
            </movie>
            <movie favorite="FALSE" title="American Psycho">
                <format multiple="No">blue-ray</format>
                <year>2000</year>
                <rating>Unrated</rating>
                <description>psychopathic Bateman</description>
            </movie>
        </decade>
    </genre>
```

# 元素树简介

XML 树结构使得导航、修改和移除在程序上相对简单。Python 有一个内置的库， **ElementTree** ，它有读取和操作 XML(和其他类似结构的文件)的函数。

一、导入`ElementTree`。使用`ET`的化名是很常见的做法:

```
**import xml.etree.ElementTree as ET**
```

## 解析 XML 数据

在提供的 XML 文件中，有一个描述电影的基本集合。唯一的问题是数据很乱！这个收藏有很多不同的管理者，每个人都有自己的方式将数据输入文件。本教程的主要目标是用 Python 阅读和理解文件，然后解决问题。

首先你需要用`ElementTree`读入文件。

```
**tree = ET.parse('movies.xml')
root = tree.getroot()**
```

现在已经初始化了树，您应该查看 XML 并打印出值，以便理解树是如何构造的。

```
**root.tag**'collection'
```

在顶层，您会看到这个 XML 植根于`collection`标签。

```
**root.attrib**{}
```

## 对于循环

通过使用一个简单的“for”循环，可以很容易地迭代根中的子元素(通常称为“子元素”)。

```
**for child in root:
    print(child.tag, child.attrib)**genre {'category': 'Action'}
genre {'category': 'Thriller'}
genre {'category': 'Comedy'}
```

现在你知道根`collection`的孩子都是`genre`。为了指定流派，XML 使用属性`category`。根据`genre`元素，有动作片、惊悚片和喜剧片。

通常，了解整个树中的所有元素是很有帮助的。一个有用的功能是`root.iter()`。

```
**[elem.tag for elem in root.iter()]**['collection',
 'genre',
 'decade',
 'movie',
 'format',
 'year',
 'rating',
 'description',
 'movie',
 .
 .
 .
 .
 'movie',
 'format',
 'year',
 'rating',
 'description']
```

有一种查看整个文档的有用方法。如果将根传递给`.tostring()`方法，就可以返回整个文档。在 ElementTree 中，这个方法采用了一种稍微奇怪的形式。

由于 ElementTree 是一个功能强大的库，它不仅可以解释 XML，还必须指定显示为字符串的文档的编码和解码。

您可以扩展`iter()`函数的用途，以帮助查找感兴趣的特定元素。`root.iter()`将列出根下与指定元素匹配的所有子元素。这里，您将列出树中`movie`元素的所有属性:

```
**for movie in root.iter('movie'):
    print(movie.attrib)**{'favorite': 'True', 'title': 'Indiana Jones: The raiders of the lost Ark'}
{'favorite': 'True', 'title': 'THE KARATE KID'}
{'favorite': 'False', 'title': 'Back 2 the Future'}
{'favorite': 'False', 'title': 'X-Men'}
{'favorite': 'True', 'title': 'Batman Returns'}
{'favorite': 'False', 'title': 'Reservoir Dogs'}
{'favorite': 'False', 'title': 'ALIEN'}
{'favorite': 'True', 'title': "Ferris Bueller's Day Off"}
{'favorite': 'FALSE', 'title': 'American Psycho'}
{'favorite': 'False', 'title': 'Batman: The Movie'}
{'favorite': 'True', 'title': 'Easy A'}
{'favorite': 'True', 'title': 'Dinner for SCHMUCKS'}
{'favorite': 'False', 'title': 'Ghostbusters'}
{'favorite': 'True', 'title': 'Robin Hood: Prince of Thieves'}
```

## XPath 表达式

很多时候元素没有属性，它们只有文本内容。使用属性`.text`，可以打印出这个内容。

现在，打印出电影的所有描述。

```
**for description in root.iter('description'):
    print(description.text)**'Archaeologist and adventurer Indiana Jones is hired by the U.S. government to find the Ark of the Covenant before the Nazis.'None provided.
Marty McFly
Two mutants come to a private academy for their kind whose resident superhero team must oppose a terrorist organization with similar powers.
NA.
WhAtEvER I Want!!!?!
"""""""""
Funny movie about a funny guy
psychopathic Bateman
What a joke!
Emma Stone = Hester Prynne
Tim (Rudd) is a rising executive who “succeeds” in finding the perfect guest, IRS employee Barry (Carell), for his boss’ monthly event, a so-called “dinner for idiots,” which offers certain 
advantages to the exec who shows up with the biggest buffoon.Who ya gonna call?
Robin Hood slaying
```

打印出 XML 很有帮助，但是 XPath 是一种查询语言，用于快速方便地搜索 XML。然而，理解 XPath 对于扫描和填充 XML 至关重要。`ElementTree`有一个`.findall()`函数，它将遍历被引用元素的直接子元素。

在这里，您将搜索 1992 年上映的电影树:

```
**for movie in root.findall("./genre/decade/movie/[year='1992']"):
    print(movie.attrib)**{'favorite': 'True', 'title': 'Batman Returns'}
{'favorite': 'False', 'title': 'Reservoir Dogs'}
```

函数`.findall()`总是从指定的元素开始。这种类型的功能对于“查找和替换”来说非常强大。你甚至可以搜索属性！

现在，只打印多种格式的电影(一个属性)。

```
**for movie in root.findall("./genre/decade/movie/format/[@multiple='Yes']"):
    print(movie.attrib)**{'multiple': 'Yes'}
{'multiple': 'Yes'}
{'multiple': 'Yes'}
{'multiple': 'Yes'}
{'multiple': 'Yes'}
```

集思广益，为什么在这种情况下，print 语句返回`multiple`的“是”值。想想“for”循环是怎么定义的。

**提示**:在 XPath 内部使用`'...'`返回当前元素的父元素。

```
**for movie in root.findall("./genre/decade/movie/format[@multiple='Yes']..."):
    print(movie.attrib)**{'favorite': 'True', 'title': 'THE KARATE KID'}
{'favorite': 'False', 'title': 'X-Men'}
{'favorite': 'False', 'title': 'ALIEN'}
{'favorite': 'False', 'title': 'Batman: The Movie'}
{'favorite': 'True', 'title': 'Dinner for SCHMUCKS'}
```

## 修改 XML

早些时候，电影的名字是绝对的混乱。现在，再把它们打印出来:

```
**for movie in root.iter('movie'):
    print(movie.attrib)**{'favorite': 'True', 'title': 'Indiana Jones: The raiders of the lost Ark'}
{'favorite': 'True', 'title': 'THE KARATE KID'}
{'favorite': 'False', 'title': 'Back 2 the Future'}
{'favorite': 'False', 'title': 'X-Men'}
{'favorite': 'True', 'title': 'Batman Returns'}
{'favorite': 'False', 'title': 'Reservoir Dogs'}
{'favorite': 'False', 'title': 'ALIEN'}
{'favorite': 'True', 'title': "Ferris Bueller's Day Off"}
{'favorite': 'FALSE', 'title': 'American Psycho'}
{'favorite': 'False', 'title': 'Batman: The Movie'}
{'favorite': 'True', 'title': 'Easy A'}
{'favorite': 'True', 'title': 'Dinner for SCHMUCKS'}
{'favorite': 'False', 'title': 'Ghostbusters'}
{'favorite': 'True', 'title': 'Robin Hood: Prince of Thieves'}
```

在未来修正后面 2 中的 2。这应该是一个查找和替换的问题。编写代码以找到标题“Back 2 the Future”并将其保存为变量:

```
**b2tf = root.find("./genre/decade/movie[@title='Back 2 the Future']")
print(b2tf)**<Element 'movie' at 0x10ce00ef8>
```

注意，使用`.find()`方法返回树的一个元素。很多时候，编辑元素中的内容更有用。

将 Back 2 未来元素变量的`title`属性修改为“回到未来”。然后，打印出变量的属性以查看您的更改。通过访问元素的属性，然后给它分配一个新值，可以很容易地做到这一点:

```
**b2tf.attrib["title"] = "Back to the Future"
print(b2tf.attrib)**{'favorite': 'False', 'title': 'Back to the Future'}
```

将您的更改写回 XML，以便它们在文档中永久固定。再次打印出您的电影属性，以确保您的更改生效。使用`.write()`方法来做这件事:

```
**tree.write("movies.xml")****tree = ET.parse('movies.xml')
root = tree.getroot()****for movie in root.iter('movie'):
    print(movie.attrib)**{'favorite': 'True', 'title': 'Indiana Jones: The raiders of the lost Ark'}
{'favorite': 'True', 'title': 'THE KARATE KID'}
{'favorite': 'False', 'title': 'Back to the Future'}
{'favorite': 'False', 'title': 'X-Men'}
{'favorite': 'True', 'title': 'Batman Returns'}
{'favorite': 'False', 'title': 'Reservoir Dogs'}
{'favorite': 'False', 'title': 'ALIEN'}
{'favorite': 'True', 'title': "Ferris Bueller's Day Off"}
{'favorite': 'FALSE', 'title': 'American Psycho'}
{'favorite': 'False', 'title': 'Batman: The Movie'}
{'favorite': 'True', 'title': 'Easy A'}
{'favorite': 'True', 'title': 'Dinner for SCHMUCKS'}
{'favorite': 'False', 'title': 'Ghostbusters'}
{'favorite': 'True', 'title': 'Robin Hood: Prince of Thieves'}
```

## 修复属性

`multiple`属性有些地方不正确。使用`ElementTree`根据电影的格式来固定指示器。首先，打印`format`属性和文本，看看哪些部分需要修复。

```
**for form in root.findall("./genre/decade/movie/format"):
    print(form.attrib, form.text)**{'multiple': 'No'} DVD
{'multiple': 'Yes'} DVD,Online
{'multiple': 'False'} Blu-ray
{'multiple': 'Yes'} dvd, digital
{'multiple': 'No'} VHS
{'multiple': 'No'} Online
{'multiple': 'Yes'} DVD
{'multiple': 'No'} DVD
{'multiple': 'No'} blue-ray
{'multiple': 'Yes'} DVD,VHS
{'multiple': 'No'} DVD
{'multiple': 'Yes'} DVD,digital,Netflix
{'multiple': 'No'} Online,VHS
{'multiple': 'No'} Blu_Ray
```

这个标签需要做一些工作。

您可以使用正则表达式来查找逗号——这将告诉您`multiple`属性应该是“是”还是“否”。使用`.set()`方法可以很容易地添加和修改属性。

```
**import re****for form in root.findall("./genre/decade/movie/format"):
    # Search for the commas in the format text
    match = re.search(',',form.text)
    if match:
        form.set('multiple','Yes')
    else:
        form.set('multiple','No')****# Write out the tree to the file again
tree.write("movies.xml")****tree = ET.parse('movies.xml')
root = tree.getroot()****for form in root.findall("./genre/decade/movie/format"):
    print(form.attrib, form.text)**{'multiple': 'No'} DVD
{'multiple': 'Yes'} DVD,Online
{'multiple': 'No'} Blu-ray
{'multiple': 'Yes'} dvd, digital
{'multiple': 'No'} VHS
{'multiple': 'No'} Online
{'multiple': 'No'} DVD
{'multiple': 'No'} DVD
{'multiple': 'No'} blue-ray
{'multiple': 'Yes'} DVD,VHS
{'multiple': 'No'} DVD
{'multiple': 'Yes'} DVD,digital,Netflix
{'multiple': 'Yes'} Online,VHS
{'multiple': 'No'} Blu_Ray
```

## 移动元素

一些数据被放在了错误的年代。使用你所学到的关于 XML 和`ElementTree`的知识来查找和修复十年数据错误。

在整个文档中打印出`decade`标签和`year`标签将会很有用。

```
**for decade in root.findall("./genre/decade"):
    print(decade.attrib)
    for year in decade.findall("./movie/year"):
        print(year.text)**{'years': '1980s'}
1981 
1984 
1985 
{'years': '1990s'}
2000 
1992 
1992  
{'years': '1970s'}
1979 
{'years': '1980s'}
1986 
2000 
{'years': '1960s'}
1966 
{'years': '2010s'}
2010 
2011 
{'years': '1980s'}
1984 
{'years': '1990s'}
1991
```

这两年是错误的十年，是 2000 年代的电影。使用 XPath 表达式找出这些电影是什么。

```
**for movie in root.findall("./genre/decade/movie/[year='2000']"):
    print(movie.attrib)**{'favorite': 'False', 'title': 'X-Men'}
{'favorite': 'FALSE', 'title': 'American Psycho'}
```

为了转移 x 战警的数据，你必须在动作片中添加一个新的十年标签，即 2000 年代。可以使用`.SubElement()`方法将这个标签添加到 XML 的末尾。

```
**action = root.find("./genre[@category='Action']")
new_dec = ET.SubElement(action, 'decade')
new_dec.attrib["years"] = '2000s'**
```

现在分别使用`.append()`和`.remove()`将 x 战警电影添加到 2000 年代，并将其从 20 世纪 90 年代删除。

```
**xmen = root.find("./genre/decade/movie[@title='X-Men']")
dec2000s = root.find("./genre[@category='Action']/decade[@years='2000s']")
dec2000s.append(xmen)
dec1990s = root.find("./genre[@category='Action']/decade[@years='1990s']")
dec1990s.remove(xmen)**
```

## 构建 XML 文档

很好，所以你可以把整部电影推进到一个新的十年。将您的更改保存回 XML。

```
**tree.write("movies.xml")****tree = ET.parse('movies.xml')
root = tree.getroot()****print(ET.tostring(root, encoding='utf8').decode('utf8'))**
```

# 结论

*ElementTree* 是一个重要的 Python 库，允许您解析和导航 XML 文档。使用 ElementTree 将 XML 文档分解成一个易于使用的树结构。如果有疑问，打印出来(`print(ET.tostring(root, encoding='utf8').decode('utf8'))`)——使用这个有用的 print 语句一次性查看整个 XML 文档。

# *参考文献*

*   斯蒂芬·豪森发布的原始帖子:[数据营](https://www.datacamp.com/community/tutorials/python-xml-elementtree#intro)
*   Python 3 文档:[元素树](https://docs.python.org/3.5/library/xml.etree.elementtree.html)
*   维基百科: [XML](https://en.wikipedia.org/wiki/XML)