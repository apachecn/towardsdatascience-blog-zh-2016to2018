# 数据有不同的形状和大小

> 原文：<https://towardsdatascience.com/data-comes-in-different-shapes-and-sizes-ac5b411456c4?source=collection_archive---------2----------------------->

![](img/2d8393b5493fc6386df273d2096e507a.png)

## 让我们来谈谈表格、树和图形

[表格](https://en.wikipedia.org/wiki/Table_(information)) —数据的表格化表示，可能是最古老的数据存储方式。也是最简单的。我们定义了行和列，其中一列代表一个属性，一行代表一个由属性组合而成的条目。

[CSV](https://en.wikipedia.org/wiki/Comma-separated_values) 是表格数据的简单表示。下面你可以看到一个代表我最亲近的家人的 CSV:

```
Maxim,June 12,Berlin
Efim,November 24,Essen
Margarita,August 20,Bochum
Issai,May 9,Bochum
```

每行代表一个家庭成员，家庭成员由以下属性表示:

*   名字
*   生日
*   城市

这些属性很好地代表了一个人，但是它们没有说明这些人之间的关系。如果我们添加另外三个属性来建立它们之间的关系会怎么样:

*   父亲
*   母亲
*   同科

在这种情况下，CSV 可能如下所示:

```
Maxim,June 12,Berlin,3,2,1
Efim,November 24,Essen,3,2,0
Margarita,August 20,Bochum,,,
Issai,May 9,Bochum,,,
```

这些关系被描述为行的索引(索引从 0 开始)。

所以马克西姆的父亲是伊萨伊，马克西姆的母亲是玛格丽塔，马克西姆的兄弟姐妹是埃菲姆。在第二行，我们看到 Maxim 是 Efims 的兄弟姐妹(这并不奇怪),他们有相同的父母。Margarita 和 Issai 没有提到父亲、母亲和兄弟姐妹，不是因为他们没有，而是因为这些人没有列在这个数据集中。

## 如果马克西姆有多个兄弟姐妹会怎么样？

在 CSV 和一般表格数据表示的情况下，这是非常不幸的。然而，我们有两个选择:

1.  为值数组创建自定义语法。例如，使用`+`字符分隔多个行索引`1+10+12`。
2.  有第二个表，表示关系的[邻接矩阵](https://en.wikipedia.org/wiki/Adjacency_matrix)。对于 CSV 格式，每个文件只能有一个表，所以我们需要创建另一个文件。

我们了解到，表格数据表示适用于一对一和多对一的关系，但是如果我们需要一对多的关系，就会变得棘手。

## 树呢？

[树](https://en.wikipedia.org/wiki/Tree_(data_structure))中的[节点](https://en.wikipedia.org/wiki/Node_(computer_science))(如果它不是叶子)与其子节点有一对多的关系。让我们试着用 [XML](https://en.wikipedia.org/wiki/XML) 来表示相同的数据:

```
<person name="Maxim" birthday="June 12" city="Berlin">
 <father>
  <person name="Issai" birthday="May 9" city="Bochum"/>
 </father>
 <mother>
  <person name="Margarita" birthday="August 20" city="Bochum"/>
 </mother>
 <siblings>
  <person name="Efim" birthday="November 24" city="Essen">
   <!-- 😔 we have to repeat father and mother now -->
   <father>
    <person name="Issai" birthday="May 9" city="Bochum"/>
   </father>
   <mother>
    <person name="Margarita" birthday="August 20" city="Bochum"/>
   </mother>
   <siblings>
    <!-- 😨 OMG we have a cycle, abort!!! -->
   </siblings>
  </person>
 </siblings>
</person>
```

一棵树必须有一个根元素。在我们的第一次尝试中，我们改变了数据集的语义，并决定将其表示为 Maxim 的“家谱”。然而，这也强调了我们的数据对于这种表示来说太复杂了。当我们添加 Efim 作为兄弟姐妹时，我们必须复制 Issai 和 Margarita。然后我们意识到 Efim 和 Maxim 建立了一个参考循环。这使得纯粹的[层次化](https://en.wikipedia.org/wiki/Hierarchy)表示成为不可能。

好，让我们尝试使用带有显式引用的 XML:

```
<people>
 <person id="0" name="Maxim" birthday="June 12" city="Berlin">
  <father ref="3"/>
  <mother ref="2"/>
  <sibling ref="1">
  <!-- could have another sibling tag here
  <sibling ref="123">
  -->
 </person>
 <person id="1" name="Efim" birthday="November 24" city="Essen">
  <father ref="3"/>
  <mother ref="2"/>
  <sibling ref="0">
 </person>
 <person id="2" name="Margarita" birthday="August 20" city="Bochum"/>
 <person id="3" name="Issai" birthday="May 9" city="Bochum"/>
</people>
```

现在我们可以在一个`person`节点中拥有多个`sibling`节点。然而，与 CSV 相比，我们有一个小缺点。CSV 中的引用是行的索引。在 XML 中，一个节点可能会根据解析器和 XML producer 的实现来改变它的索引，因此有必要在每个 person 节点上有一个显式的`id`属性，在`father` `mother` `sibling`节点上有一个`ref`属性，它们反映了 person 的 id。当我们写数据时，这不是一个很大的不便，但当我们读数据时，这是一个更大的不便。为了遵循参考，用户将不得不建立某种类型的[查找表](https://en.wikipedia.org/wiki/Lookup_table)，其中可以通过`id`找到一个人。

## JSON 能帮我们解决这个问题吗？

[JSON](https://en.wikipedia.org/wiki/JSON) 也是一个树形结构，但是如果 XML 是非常同构的——我们只能通过元素和内容来描述数据。JSON 是异构的——我们有数组、对象、字符串、数字、布尔文字和`null`供我们使用。在 JSON 中，我们可以如下表示相同的数据集:

```
[
 {
  "name":"Maxim",
  "birthday":"June 12",
  "city":"Berlin",
  "father":3,
  "mother":2,
  "siblings":[1]
 },{
  "name":"Efim",
  "birthday":"November 24",
  "city":"Essen",
  "father":3,
  "mother":2,
  "siblings":[0]
 },{
  "name":"Margarita",
  "birthday":"August 20",
  "city":"Bochum"
 },{
  "name":"Issai",
  "birthday":"May 9",
  "city":"Bochum"
 }
]
```

我们说根元素是一个数组。它有三个子对象，其中`siblings` property 是一个人员索引数组。数组中的元素有一个稳定的索引，所以我们不需要显式的`id`属性。我们还避免构建显式的查找表，因为数组可以通过索引来访问。

## 有些事我们还没谈过。

CSV、XML 和 JSON 都是基于文本的格式。为了处理数据集并遵循引用，我们需要解析数据集并将其转换成某种内存模型。如果我们有一个大的数据集，这可能会变得非常不方便。

## 文本与二进制

当我们从文本表示切换到二进制表示时，我们可以选择/构建一种允许随机值访问的格式。在这种情况下，引用可以由我们所引用条目的偏移量来表示。

偏移量可以是绝对的，也可以是相对的。绝对偏移量对于读、写和验证操作来说更简单。一个相对偏移量，可以帮助你减少二进制文件的大小，用户可以将多个缓冲区合并到一个文件中。如果你有兴趣，可以看看下面这篇文章:[flatbufferswift](https://github.com/mzaks/FlatBuffersSwift)是如何解决这种技术的:

 [## 使用 JSON 时，性能并不是唯一损失的东西

### 这个来自 RSAnimate 的短片告诉我们，我不可能在 100 篇文章中做到这一点。

medium.com](https://medium.com/@icex33/performance-is-not-the-only-thing-you-lose-while-using-json-d7fc788c3056) 

## 感谢您抽出宝贵的时间，如果您喜欢这篇文章，请鼓掌。