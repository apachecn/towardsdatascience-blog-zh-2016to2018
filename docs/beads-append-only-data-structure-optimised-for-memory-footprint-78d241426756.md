# Beads —仅附加针对内存占用优化的数据结构

> 原文：<https://towardsdatascience.com/beads-append-only-data-structure-optimised-for-memory-footprint-78d241426756?source=collection_archive---------8----------------------->

![](img/13aea698b4451f1002f9a75d250809e3.png)

我必须承认，我对数据表示产生了一种不健康的迷恋。我认为它是基本的构建模块，在计算机科学中没有得到足够的重视。

在这篇博文中，我想介绍一种数据结构，它主要专注于以非常紧凑的方式表示数据，因此提供了最小的内存占用和良好的[数据局部性](https://en.wikipedia.org/wiki/Locality_of_reference)。

> 小免责声明:beads 数据结构的参考实现是用 Swift 编写的，所以简短的代码片段会在 Swift 中，但不用担心，我让它对于不熟悉 Swift 编程语言的人来说是易于理解的。

当我们谈论[数据结构](https://en.wikipedia.org/wiki/Data_structure)时，我们关注运行时特征和大 O 符号。然而，我没有看到很多关于内存占用的讨论。比如:[链表](https://en.wikipedia.org/wiki/Linked_list)是一种广为人知的数据结构，以 O(1) prepend 特性而闻名。然而，我从来没有读到过这样的讨论——一个 64 位架构上的指针如何有 8 字节宽，这意味着一个布尔值列表将为每个列表元素占用 16 字节。与[位数组](https://en.wikipedia.org/wiki/Bit_array)相比，布尔值链表需要 **128x** 更多的空间。不要让我从[数据位置](https://en.wikipedia.org/wiki/Locality_of_reference)开始。

eads 被设计成一个序列，它在追加时压缩值。Append 是在常数时间内执行的，它涉及一些计算，但是这些并不是很昂贵，并且基于复杂性理论，我们可以假设运行时间特性为 O(1)。

让我们举一个实际的例子。假设我们有一个数字序列`[1, 5, 6, 7]`，我们希望将它存储在一个数据结构中。最简单的就是把它存储在一个数组里。如果数字是正数并且小于 256，我们可以将它们存储在一个`[UInt8](https://developer.apple.com/documentation/swift/uint8)`数组中。这意味着每个条目将只占用 1 个字节。

如果我们需要在数组中存储一个负数或者更大的数，会发生什么呢？在这种情况下，我们需要确定最大的可能值，并根据这种特殊情况选择一个数字类型。然而，预测潜在价值的大小非常困难，甚至是不可能的。这就是为什么我们通常不麻烦，我们定义一个整数数组`[Int]`，其中`[Int](https://developer.apple.com/documentation/swift/int)`在 64 位架构上是 64 位宽。这意味着我们每个条目占用 8 个字节，尽管这是完全不必要的。

珠子做了一些不同的事情。Beads 不仅存储值，还存储类型。一个类型存储在 4 位中，这意味着我们可以在一个字节中存储 2 个类型标签。让我们把我们的数字系列`[1, 5, 6, 7]`储存在珠子里:

`[{u8/u8}, 1, 5, {u8/u8}, 6, 7]`

我们为每两个条目引入一个标签条目`{/}`，这意味着条目的数量增长到 150%。因此，如果我们可以将我们的数字序列存储为一个`[UInt8]`，我们就是在浪费空间。然而，如果我们将序列存储为`[Int]`。该序列如下所示:

`[1, 0, 0, 0, 0, 0, 0, 0, 5, 0, 0, 0, 0, 0, 0, 0, 6, 0, 0, 0, 0, 0, 0, 0, 7, 0, 0, 0, 0, 0, 0, 0,]`

每个数字占用 8 个字节。我们假设我们在一个小端架构上。不需要的字节等于`0`。如果我们比较珠子和一组`Int`——珠子只占大约 19%的空间。

如果序列是稀疏的，这种影响会变得更大——意味着我们有很多`nil`值。

例如:`[5, nil, nil, nil, nil, 2]`=>6 字节

变成:`[{u8/nil}, 5, {nil/nil}, {nil/u8}, 2]`=>5 字节

当一个`nil`值被附加到珠子上时，只有标签被添加，因为标签只占用一半的字节，我们有 50%的好处。所以在`[UInt8]`的情况下，我们*损失* 50%，`[UInt8?]` *随着每个`nil`值获得* 50%。也就是说，如果我们有相同数量的数字和`nil`值，我们就是 100%。如果`nil`值的数量更大，我们向 50%收敛。如果更小，我们就向 150%收敛。

## 追加是如何工作的？

珠子公开了各种数字类型的附加方法。当使用类型为`Int`的数字调用 append 时，我们检查这个数字是否可以用更小的类型来表示。最简单但不一定是最有效的解决方案是双重造型和比较:

```
let value: Int = 42
let u8Value = UInt8(value)
if value = Int(u8Value) {
  // store value as u8
}
```

我们把初始值转换成一个更小的数字表示，然后再把它转换回来，看看它是否等于初始值。在这种情况下，我们可以采用较小的表示，并将其存储在底层缓冲区中。

该解决方案甚至适用于[浮点数](https://en.wikipedia.org/wiki/Floating-point_arithmetic)。浮点通常以 4 字节(单精度)或 8 字节(双精度)存储。如果传递给 append 的值是一个`[Double](https://developer.apple.com/documentation/swift/double)`，但实际上是一个小整数(例如`42.0`)，我们可以节省高达 81%的空间。

## 弦乐呢？

如果我们有一个只和数字一起工作的数据结构，这将是非常有限的。存储一个字符串序列是一个非常普通和重要的任务。

如果我们考虑一下，字符串只是一系列数字，它们对应于特定的字符编码。如今最流行的字符编码是 [UTF-8](https://en.wikipedia.org/wiki/UTF-8) 。它将文本转换成一系列的`UInt8`数字，或者如果你愿意的话，转换成`[Data](https://developer.apple.com/documentation/foundation/data)`。珠子可以附加一系列的`UInt8`数字，或者`Data`作为一个带有一对类型标签的珠子。这里有一个例子:

`"Maxim" => [{data/u8}, 5, [77, 97, 120, 105, 109]]`

字符串`"Maxim"`用下面的标签对`{data/u8}`表示。它表示下一个条目是以 u8 类型的大小为前缀的数据。这样，迭代器能够将下一个条目解释为大小`5`，并且知道接下来的 5 个条目`[77, 97, 120, 105, 109]`属于一起。数据内部的 5 个条目形成字符串`"Maxim"`的 [ASCII](https://en.wikipedia.org/wiki/ASCII) /UTF-8 表示的信息没有存储在任何地方，并且被认为是隐含的知识。

这里有一个更复杂的例子，我们存储了一个带有`nil`值的字符串序列:

```
["Max", "Maxim", "", nil, "Alex"]
=>
[{data/u8}, 3, [77, 97, 120], {data/u8}, 5, [77, 97, 120, 105, 109], {data/u8}, 0, {nil/data}, {u8/-}, 4, [65, 108, 101, 120]]
```

让我们把这个例子分解一下。

`"Max"`和`"Maxim"`并不奇怪，它们存储在`{data\u8}`标签中，后面是代表字符串的大小和数字。

空字符串(`""`)却很有趣。标签字节仍然是`{data/u8}`，但是后面只跟着`0`。这些信息足以存储一个空字符串。

接下来是`nil`，在这种情况下，我们甚至不需要标签对。一个`nil`是一个`nil`，可以用一个标签来表示，只占用标签字节的一半。

下一个条目是`"Alex"`，不出所料，它必须由一个`{data/u8}`标签对来表示，但是由于我们在前面的字节中仍有空间仅被`nil`占用，因此该标签对将分布在两个字节中。这就是导致`{nil/data}, {u8/-}, 4, [65, 108, 101, 120]`的原因。

## 零重量表示的压缩数据

如你所见，beads 非常努力地尽可能少地浪费内存。这是实现这个目标的另一个窍门。

如果我们将字符串存储在 [UTF-16](https://en.wikipedia.org/wiki/UTF-16) 中，让我们看看我们的字符串序列会是什么样子。这种格式也很普遍，但是特别是对于简单的 ASCII 字符来说更浪费。这是我们之前在 UTF-16 中的例子:

```
["Max", "Maxim", "", nil, "Alex"]
=>
[{data/u8}, 8, [255, 254, 77, 0, 97, 0, 120, 0], {data/u8}, 12, [255, 254, 77, 0, 97, 0, 120, 0, 105, 0, 109, 0], {data/u8}, 0, {nil/data}, {u8/-}, 10, [255, 254, 65, 0, 108, 0, 101, 0, 120, 0]]
```

现在`"Max"`表示为`{data/u8}, 8, [255, 254, 77, 0, 97, 0, 120, 0]`。标签字节是相同的，但是代表字符串的数字序列的长度是 8，而不是 UTF-8 中的 3。这是因为在 UTF-16 编码中，我们将每个字符存储在 2 个字节中，我们需要在序列前加上所谓的 [BOM](https://en.wikipedia.org/wiki/Byte_order_mark) ，它可以识别数字是根据[大端还是小端](https://en.wikipedia.org/wiki/Endianness)存储的。

当我们存储使用许多简单 ASCII 字符的语言(如英语)编写的文本时，我们最终会有许多`0`值。这就是我所说的“ ***零重表象*** ”。在这种情况下，珠子提供一种特殊的珠子类型，称为紧凑数据(简称为`c_data`)。让我们直接看看它的运行情况:

```
["Max", "Maxim", "", nil, "Alex"]
=>
[{c_data/u8}, 6, [{01010111}, 255, 254, 77, 97, 120], {c_data/u8}, 9, [{01010111}, 255, 254, 77, 97, 120, {00000101}, 105, 109], {c_data/u8}, 0, {nil/c_data}, {u8/-}, 8, [{01010111}, 255, 254, 65, 108, 101, {00000001}, 120]]
```

使用`c_data`,我们能够将`"Max"`减少到 75%——从 8 字节减少到 6 字节。

这是通过以下技术实现的:

我们创造了一串珠子。如果你仔细想想，`[255, 254, 77, 0, 97, 0, 120, 0]`可以被看作是一个 beed 本身。不过我们知道数字都是`UInt8`，所以可以用标志字节来存放别的东西。我们使用标志字节来存储下一个字节是否等于`0`。这样我们减少了:

`[255, 254, 77, 0, 97, 0, 120, 0]`

to ( *位屏蔽应从右向左读*)

`[{01010111}, 255, 254, 77, 97, 120]`

使序列缩短两个字节。

现在，记住我们将`[1, 5, 6, 7]`存储为`[Int]`的例子

```
[1, 0, 0, 0, 0, 0, 0, 0, 5, 0, 0, 0, 0, 0, 0, 0, 6, 0, 0, 0, 0, 0, 0, 0, 7, 0, 0, 0, 0, 0, 0, 0,]
```

这也是一个 ***零重的表象。*** 如果我们将其存储为`c_data`会怎么样？

```
[{c_data/u8}, 8, [{00000001}, 1, {00000001}, 5, {00000001}, 6,{00000001}, 7]]
```

我们以(8 + 2)字节结束，这比 32 字节好，但仍然比 6 字节差，我们能够通过直接珠子追加来实现。

## 让我们进一步了解珠子的想法

我们讨论了添加数字和字符串，但是对于更复杂的数据类型呢？这一次，如果一个数据类型可以转换成`Data`或`[UInt8]`，我们可以像附加字符串一样附加它。然而，我们可以做得更好。假设我们想要创建一个三维向量的珠子:

```
struct Vector3 {
  let x: Double
  let y: Double
  ley z: Double
}
```

我们可以说，我们可以将每一个`Vector3`转换成它自己的珠子数据结构，然后将它们连接在一起。

让我们看看:

`Vector(x:2, y: 4, z:0)`可转换为`[{u8/u8}, 2, 4, {u8/-}, 0]`

以及`Vector(x:5, y: 8, z:257)`至`[{u8/u8}, 5, 8, {u16/-}, 1, 1]`

所以如果我们有多个向量

```
[
  Vector(x:2, y: 4, z:0), Vector(x:5, y: 8, z:257)
]
```

我们可以把它们连接到下面的珠子上

```
[{u8/u8}, 2, 4, {u8/-}, 0, {u8/u8}, 5, 8, {u16/-}, 1, 1]
```

这里有一个有趣的小细节。`Vector3`有 3 个属性，这意味着第二个标签的第二部分是一个 ***跳过*** 标记`-`。如果我们想快速连接两颗珠子。我们保留 skip 标记，并用第二个珠子扩展第一个珠子的底层缓冲区。然而，如果我们不想浪费内存，我们可以将第二个珠子的成员一次附加到第一个值上，从而产生以下珠子:

```
[{u8/u8}, 2, 4, {u8/u8}, 0, 5, {u8/u16}, 8, 1, 1]
```

意思是——如果我们想为每秒连接的珠子节省一个字节，我们可以做到！*(我不确定我们是否应该这样做)*

在这一点上，我想强调一下，beads 是一个短暂的数据结构，也就是说，它不是持久的。这是一个有意识的选择，因为目标是拥有一个最小内存占用的数据结构。

## 我们如何从珠子中读取数据？

正如我之前提到的，珠子基本上是一个序列。在 Swift 中有一个叫做`[Sequence](https://developer.apple.com/documentation/swift/sequence)`的协议，它对实现类型强加了一个要求:**它必须提供一个迭代器**。

大多数其他语言都提供了类似的协议:

*   `[Iterable](https://docs.oracle.com/javase/8/docs/api/java/lang/Iterable.html)`在 Java 中
*   `[IEnumerable](https://msdn.microsoft.com/en-us/library/system.collections.ienumerable(v=vs.110).aspx)`在 C#中
*   `[IntoIterator](https://doc.rust-lang.org/std/iter/trait.IntoIterator.html)`在生锈
*   等等…

Beads 是一个`Sequence`，迭代器提供对下一个 beads 的访问。那么作为用户，我们可以检查这个珠子是否是一个`nil`值，或者要求将珠子解释为`int`、`uint`、`double`或`data`。如果给定的珠子不能按要求解释，我们得到一个`nil`值。因为我们可以明确地检查一个值不是`nil`，所以我们可以识别一个问题。

下面是一个简单的单元测试，我们将值附加到珠子上，然后将珠子映射到一个新的数组中，并检查它是否等于初始值:

```
func testSequenceU8AndI8ToInt() {
  var beads = BeadsSequence()
  let values = [
    1, 0, nil, 250, nil, 5, -5, -127
  ]
  for v in values {
    beads.append(v)
  }
  XCTAssertEqual(beads.map { $0.int }, values)
}
```

## 复杂数据类型呢？

为了让处理复杂数据类型变得愉快，我们引入了一个`BeadsConvertible`协议:

```
public protocol BeadsConvertible {
  static func from(
    sequenceIterator: BeadsSequence.BeadsIterator
  ) -> BeadsSequence.FailableResult<Self>?
  var numberOfBeads: Int { get }
  func toBeads() -> BeadsSequence
}
```

当一个类型实现这个协议时，它允许我们将这个类型的实例转换成珠子，反之亦然。

这是一个单元测试，它执行从`[CGPoint](https://developer.apple.com/documentation/coregraphics/cgpoint)`阵列到珠子的往返行程:

```
func testCGPoint() {
  var sequence = BeadsSequence()
  let points = [
    CGPoint(x: 2, y: 4),
    CGPoint(x: 2.5, y: 4.5),
    CGPoint(x: 2.1, y: 4.1)
  ]
  for point in points {
    sequence.append(point.toBeads())
  } let pointsArray = sequence.beadsConvertibleSequence(
      for: CGPoint.self
  ).map { try?$0.get() } XCTAssertEqual(points, pointsArray)
}
```

## 观点

Beads 是一种专门的数据结构，在内存占用和数据局部性很重要的用例中非常有用。它还被设计成数据序列化格式的平台。它有它的局限性，例如，它只能附加，但这也是它能做好一件事的原因。

我很想听听你们的想法、关切和建议。

我会尽快把链接添加到 github 库。

## 2018 年 8 月 26 日更新:

我很高兴地宣布，珠数据结构是在 Github:

[](https://github.com/beads-data-structure) [## 珠子数据结构

### GitHub 是人们构建软件的地方。超过 2800 万人使用 GitHub 来发现、分享和贡献超过…

github.com](https://github.com/beads-data-structure) 

## 2019 年 5 月 31 日更新:

由于珠子用于 Dart，我们引入了另一种(最后一种可能的)珠子类型— **TinyData。**

当存储的字节数组小于 16 字节时，使用微小数据。在这种情况下，数据的长度可以存储为一个标签(4 位):

`"Maxim" => [{t_data/5}, [77, 97, 120, 105, 109]]`

这样，我们为每个项目节省了一个字节。

目前只能在 dart 存储库中找到微小数据的实现:

[](https://github.com/beads-data-structure/beads_dart) [## 珠子-数据-结构/珠子 _ 镖

### Dart 的珠子数据结构。通过在…上创建帐户，为 beads-data-structure/beads _ dart 开发做出贡献

github.com](https://github.com/beads-data-structure/beads_dart) 

其他实现也将随之而来。