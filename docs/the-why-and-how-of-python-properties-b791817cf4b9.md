# Python 属性的原因和方式

> 原文：<https://towardsdatascience.com/the-why-and-how-of-python-properties-b791817cf4b9?source=collection_archive---------3----------------------->

![](img/1184164f7e6f471ed675b6ff0c851855.png)

[https://cdn.programiz.com/sites/tutorial2program/files/python-property.jpg](https://cdn.programiz.com/sites/tutorial2program/files/python-property.jpg)

## **一个现实世界的问题**

大约一年前，我们在我目前的工作中遇到了一个有趣的障碍:我们的计算机视觉管道消耗了太多的内存。拥有 10gb 可用内存和一个相当大的交换分区的机器正在猛烈地颠簸。经过一些调查，我们意识到这是我们在流水线开始时将一个任务的所有相关输入图像加载到内存中的结果。当流水线第一次组装时，每个作业的最大图像数量约为 20 个，这并不是一个问题，但这个数字突然开始增长。我们在一个有 210 个图像的作业中发现了这个问题，一个 52 兆的图像占用了 11 兆的内存。

太好了。我们遇到了一个问题。解决这一难题的简单方法是只在需要的时候将图像加载到内存中，而不要保留对它的不必要的引用。这是一个不错的解决方案(尽管它本身有一些问题是可以解决的，但我不会在本文中深入讨论)，但这似乎需要大量的重构。为了理解为什么，请允许我向您展示一些(非常简单的)示例代码:

```
# The class that holds onto the image
class ImageHolder:
    def __init__(self, image_path):
        self.image = load_image_from_path(image_path) holders = []
# The bit of code that loads the images
for image_path in large_list_of_paths:
    holders.append(ImageHolder(image_path)) # Various parts of the pipeline that do something with the images
for holder in holders:
    do_something(holder.image)
```

## **现实世界的解决方案**

在大多数其他语言(如 C++)中，最简单的解决方案是:

```
class ImageHolder:
    def __init__(self, image_path):
        self.image_path = image_path def get_image(self):
        return load_image_from_path(self.image_path)
```

但是这产生了一个问题:我们现在必须确保所有对`ImageHolder.image`的引用都变成了`ImageHolder.get_image()`。我们还需要确保程序员记得更改未来的代码(尽管有些人会认为我们应该从一开始就这样做，如果属性不存在，我也会同意)。虽然这两个任务都不是特别重要的任务，但是它们仍然很烦人，而且它们扰乱了代码本身的语义(也就是说，`operate_on(holder.image)`比`operate_on(holder.get_image())`读起来要好得多)。谢天谢地，在 python 中，我们可以这样做:

```
class ImageHolder:
    def __init__(self, image_path):
        self.image_path = image_path@property
def image(self):
        return load_image_from_path(self.image_path)
```

现在`ImageHolder.image`(看起来*不像*函数调用)返回图像，就好像它是一个指向图像的字段。**这个**就是 python 属性的魔力。他们将我们从最初糟糕的设计中解救出来(从一开始就没有设置 getters 和 setters ),同时使我们的代码看起来不那么笨拙。

## **另一个不太“现实”的例子**

当然，使用这样的抽象还有无数的其他原因，它确实有助于保持对象中的*状态*的总大小较小，并提高可维护性。考虑一个`Circle`类的简单实现:

```
PI = 3.141
class Circle:
    def init(self, radius):
        self.radius = radius
        self.area = PI * self.radius ** 2
        self.circumference = 2 * PI * self.radius
```

并按以下方式使用它:

```
c = Circle(1)
print c.area  # prints 3.141
print c.circumference  # prints 6.282c.radius = 2
print c.area  # prints 3.141 which is incorrect
print c.circumference  # prints 6.282 which is incorrect
```

一旦我们更改了`radius`,`area`和`circumference`字段在更新之前都是无用的(我们可以在这里使用 setters 来缓解这种情况，但那是另一篇文章了)。与下面的实现进行比较:

```
PI = 3.141
class Circle:
    def init(self, radius):
        self.radius = radius def area(self):
        return PI * self.radius ** 2 def circumference(self):    
        return 2 * PI * self.radius
```

再次对其运行相同的测试，我们得到:

```
c = Circle(1)
print c.area  # prints 3.141
print c.circumference  # prints 6.282c.radius = 2
print c.area  # prints 12.564 which is correct!
print c.circumference  # prints 12.564 which is correct!
```

只有`radius`成为该对象的*状态*才有意义，因为其他两个值都可以从中导出。Python 属性允许我们封装这种现实，而不会使语法看起来笨拙。

## **结论**

我想展示 Python 属性在现实环境中的用处。我希望这篇文章能做到这一点。关于属性(以及设置器)更详细的讨论可以在[这里](https://www.programiz.com/python-programming/property)找到。我将在另一篇文章中讨论 setters，在那篇文章中，我将解释如何更新一个没有加载到内存中的图像(以及其他内容)。