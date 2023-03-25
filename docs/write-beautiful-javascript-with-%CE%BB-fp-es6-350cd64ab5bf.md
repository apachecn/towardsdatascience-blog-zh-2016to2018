# 带有 map、reduce、filter 和 ES6 的 Javascript 数据结构

> 原文：<https://towardsdatascience.com/write-beautiful-javascript-with-%CE%BB-fp-es6-350cd64ab5bf?source=collection_archive---------1----------------------->

## 如果你是一个 javascript 爱好者，并且每天都在使用它，那么你会爱上它的😃

![](img/15a9995f605f7e96711dc09f2331e9a3.png)

Javascript 是一种允许以任何风格编写代码的语言，从命令式到声明式。大多数程序员使用命令式，要么是因为他们来自 OOPs 背景，也许他们热爱它，要么是因为他们不熟悉其他风格。在我们深入到函数式编程(FP)的声明式风格之前，让我们通过看一个例子来理解两者之间的区别(如果你已经知道区别，那么你可以跳过几段)

## 必要的

命令式风格很酷，但是想象一下，如果这里有一个复杂的数学逻辑，那么代码的大小和可读性将会很差。它增加了阅读时的认知负荷，随着时间的推移，在推理和逻辑上更容易动摇。

此外，这段代码片段的主要复杂性源于这样一个事实，即我们不是告诉计算机*我们想要*它做什么，而是指示它*如何做*。

## 宣言的

现在，这看起来非常干净、简短、有表现力、简洁的代码，不容易出错，更容易维护，也更容易调试。我们是在告诉计算机*我们想要*它做什么，而不是*如何去做。*

声明式方法在编译器端很容易优化，副作用也较少。

> 注意:如果你关心上面两个和其他 javascript 函数(map、reduce、filter、find)的性能，那么你应该在这里阅读[获取小数据集，可以在这里查看](https://medium.com/@ideepak.jsd/javascript-performance-test-for-vs-for-each-vs-map-reduce-filter-find-32c1113f19d7)[获取大数据集(100–1000000)](https://github.com/dg92/Performance-Analysis-JS)

让我们从 FP 最常用的 Javascript 函数— `.map()`开始真正的操作。

## 地图

```
// definition 
collection.map((currentValue, index) => {
    // Return element for newArray
});
// example
const arr = [1,2,3,4,5];
const newArray = arr.map(i => i*10);
// return a new array with all value as multiple of 10;
```

**map 作用于一个数组并返回一个数组**。上面的代码片段作用于一个集合，即一个数组，接受一个带有当前迭代值的回调，索引作为参数，并返回一个新的数组。

> 注意:Map 非常适合改变/转换整个数组，而不是在某些情况下中断流程，Map suck 的性能明智，请查看此处的但易于用于小数据集。

## 减少

```
// definition 
collection.reduce((accumulator, item, index) => {
    // logic to perform to get accumulator as a return value
}, initialValue for accumulator);// example
const arr = [1,2,3,4,5];
const total = arr.reduce((acc, item) => acc+= item, 0);
// return a total as 15
```

**Reduce 在一个数组上工作，但是可以返回任何你想让它返回的东西**。顾名思义，它可以简化为任何形式，可以表现为`map`、`find`、`filter`或任何其他 JavaScript 数组函数。上面的代码片段作用于一个数组，并简化为计算数组中项目的总值。

解释上面的例子，在第一次运行 reduces 时，`acc`被赋值为 0，然后`acc+= item`即`acc = acc+item`将计算出为 1 的`0+1`。这个 1 将是下一次迭代的`acc`值，这将一直持续到我们完成所有的数组项。

## 发现

```
// definition 
collection.find((item) => {
    // return first element that satisfy the condition
});// example
const arr = [1,2,8,4,5];
const value = arr.find(i => i%4 == 0);
// return the first value i.e 8 
```

**Find 作用于数组，返回函数中满足条件的第一个元素。**

> 注意:容易，简单但在大数据集上效率不高，为什么？看这里[这里](https://github.com/dg92/Performance-Analysis-JS)

## 过滤器

```
// definition 
collection.filter((currentValue, index) => {
    // logic to filter array on
});
// example
const arr = [1,2,3,4,5];
const newArray = arr.filter(i => i%2 == 0);
// return a new array with value [2, 4]
```

**过滤器作用于一个数组，返回一个已过滤项目的数组。**也就是说，它将删除返回 false 值的项目，只返回从函数返回的 true 值数组。

## 让我们用它们来做一些真实世界的场景+一些 ES6。(让我们在下面的对象键上尝试一些 ARMD)

> 想知道什么是 ARMD 它的添加，阅读，修改，删除，它的酷硬币你自己的行话😄

我们将使用`users`它作为一个数组来进一步举例。

**1。一个**RMD**——给** `**users**`增加一个新元素

```
const newUser = {
    id: 4,
    name: "Denomer Crazy",
    username: "crazy.1",
    email: "[deno@crazy.com](mailto:deno@crazy.com)",
    phone: "",
    website: "crazy.app",
    password: "crazed_checker"
};const newData = [...users, newUser]; // add element at last
or 
const newData = [newUser, ...users]; // add element at first
or 
const newData = users.concat(newUser) // the old way
```

ES6 spread 运算符的使用使得向数组添加元素变得非常容易。我们可以使用 spread 操作符连接两个不同的数组，修改对象的形状，或者添加动态键值对，等等。

```
const hobbies = ['chess', 'pool'];
const newUsers = users.**map**(u => ({...u, hobbies}))
// this will add hobbies to users array and return newUsers array
```

**2。**A**R**MD**——将** `**users**` **的电子邮件地址、电话号码和网址放入新数组**

```
const contactInfo = users.**map**(({email, website, phone}) => ({email, website, phone}));
```

使用 ES6 析构对象键和`map`来获取用户的联系信息数组。

3.AR **M** D **—查找并替换对象**的键值

```
const newUsers = users.**map**(u => u.id == 2? ({...u, name: 'te'}): u);
// this will return newUsers with all user having name 'te'
```

4.手臂**D**—从对象中删除一些关键点

> 注意:我们实际上不会删除键而是返回一个新的对象，如果你想删除键使用 delete 操作符，这里我们考虑的是对象的不变性。

删除键有很多种方法，但我们将着眼于最简单的，单行的。让我们试着从用户中删除网站。

```
const newUsers = users.**map**({id, email, name, username, phone, password} => ({id, email, username, email, phone, password}));
// will return an array with all keys other than website
```

上面的代码似乎很难对大对象进行编码。

```
const newUsers = users.**map**(u => Object.keys(u).**reduce**((newObj, key) => key != 'website' ? { ...newObj, [key]: u[key]} : newObj, {}));
```

我们对用户进行映射，然后对每个用户进行归约并形成一个新对象(`newObj`)，然后检查网站关键字，如果是网站，我们返回之前形成的`newObj`，如果不是，我们进行扩展操作，并将所需的关键字添加到`obj`，最后返回`newObj`。

如果您想加入我的电子邮件列表，请考虑在此输入您的电子邮件地址[](https://goo.gl/forms/MOPINWoY7q1f1APu2)****关注我在**[**medium**](https://medium.com/@ideepak.jsd)**上阅读更多关于 javascript 的文章，并在**[**github**](https://github.com/dg92)**上查看我的疯狂代码**。如果有什么不清楚或者你想指出什么，请在下面评论。**

**你可能也会喜欢我的其他文章**

1.  **[Javascript 执行上下文和提升](https://levelup.gitconnected.com/javascript-execution-context-and-hoisting-c2cc4993e37d)**
2.  **[Javascript —生成器-产出/下一个&异步-等待🤔](https://medium.com/datadriveninvestor/javascript-generator-yield-next-async-await-8442d2c77185)**
3.  **[理解 Javascript 'this '关键字(上下文)](https://medium.com/datadriveninvestor/javascript-context-this-keyword-9a78a19d5786)。**
4.  **[Javascript- Currying VS 部分应用](https://medium.com/datadriveninvestor/javascript-currying-vs-partial-application-4db5b2442be8)**
5.  **[Javascript ES6 —可迭代程序和迭代器](https://medium.com/datadriveninvestor/javascript-es6-iterables-and-iterators-de18b54f4d4)**
6.  **[Javascript 性能测试—针对 vs 针对每个 vs (map，reduce，filter，find)。](https://codeburst.io/write-beautiful-javascript-with-%CE%BB-fp-es6-350cd64ab5bf)**
7.  **[Javascript —代理](https://medium.com/datadriveninvestor/why-to-use-javascript-proxy-5cdc69d943e3)， [Javascript —作用域](https://medium.com/datadriveninvestor/still-confused-in-js-scopes-f7dae62c16ee)**

## **如果这篇文章有帮助，请随意分享并帮助他人找到它！**

****谢谢！****