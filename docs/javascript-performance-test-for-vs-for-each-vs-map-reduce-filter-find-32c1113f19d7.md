# Javascript 性能测试——针对每个 vs 的 vs(映射、归约、过滤、查找)。

> 原文：<https://towardsdatascience.com/javascript-performance-test-for-vs-for-each-vs-map-reduce-filter-find-32c1113f19d7?source=collection_archive---------1----------------------->

![](img/17aed37d40aec439c6636d44068c0a6d.png)

Photo by [Alex Holyoake](https://unsplash.com/@stairhopper?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

我们都知道 for 循环比 each 或 [javascript](https://hackernoon.com/tagged/javascript) 函数要快，因为在 javascript 函数的掩护下，可能会使用 for 循环或其他我不确定的东西。我用一组对象做了一个简单的测试，通过 for [循环](https://hackernoon.com/tagged/loop) / for each / javascript 函数做了一些操作，并观察了执行所花费的时间。

> **这些结果来自小的例子，并且可能根据所执行的操作、执行环境的选择而变化。和虚拟机的选择。**

# 1.减少 for 循环 vs foreach 的 vs

```
// calculated the sum of upVotes
const posts = [ 
  {id: 1, upVotes: 2},
  {id: 2, upVotes: 18}, 
  {id: 3, upVotes: 1}, 
  {id: 4, upVotes: 30}, 
  {id: 5, upVotes: 50} 
];let sum = 0;
console.time('reduce');
sum = posts.reduce((s, p)=> s+=p.upVotes,0);
console.timeEnd('reduce')sum = 0;
console.time('for loop');
for(let i=0; i<posts.length; i++) {
    sum += posts[i].upVotes;
}
console.timeEnd('for loop');sum = 0;
console.time('for each');
posts.forEach(element => {
    sum += element.upVotes;
});console.timeEnd('for each');
```

> **注:下面是结果列表，代码可以在** **这里找到** [**。**](https://github.com/dg92/Performance-analysis-es6)

## 所有的结果都清楚地表明 ***对于循环比对于每一个都更精通于映射/归约/过滤/查找。***

映射/减少/过滤/查找速度慢有很多原因，其中一些原因是

1.  他们有一个回调来执行，所以这是一个开销。
2.  javascript 函数会考虑很多极端情况，比如 getters、稀疏数组以及检查传递的参数是否是数组，这些都会增加开销。

> *我找到了一个*[*lib*](https://github.com/codemix/fast.js/tree/master)*。它重新实现了几个常见的内置原生 JavaScript 函数。*

但是使用方式的选择不仅仅取决于性能，还需要考虑更多因素，其中包括:

1.  代码可读性和可维护性
2.  轻松代码
3.  快速编码
4.  实施与优化
5.  个人选择

就我个人而言，我喜欢地图，减少，过滤，查找，并且我已经用了很长时间了。他们帮助我写出干净、精确、快速且切中要点的代码，这些代码符合我的思维过程。当我别无选择时，我使用 for 循环。

就优化而言，映射/减少/过滤/查找替换应该是最后一个选项，或者不是一个选项，这取决于所需的优化级别。

> 注意:如果你正在使用循环，总是习惯性地使用它们，因为编译器现在足够聪明来正确地优化习惯性循环

更新:[在这里](https://github.com/dg92/Performance-analysis-es6)你可以找到大数据集和繁重计算的结果。

我写了一些关于[overflowjs.com](https://overflowjs.com/)的文章，这篇文章的第 2 部分比较了 ramada 内联缓存热缓存和一些 GC 的东西。所以，别忘了继续关注 overflowjs.com 的

如果您想加入我的电子邮件列表，请考虑在此输入您的电子邮件地址[](https://goo.gl/forms/MOPINWoY7q1f1APu2)****关注我在**[**medium**](https://medium.com/@ideepak.jsd)**上阅读更多关于 javascript 的文章，并在**[**github**](https://github.com/dg92)**上查看我的疯狂代码**。如果有什么不清楚或者你想指出什么，请在下面评论。**

1.  **[Javascript 执行上下文和提升](https://levelup.gitconnected.com/javascript-execution-context-and-hoisting-c2cc4993e37d)**
2.  **[Javascript-生成器-产出/下一个&异步-等待🤔](https://medium.com/datadriveninvestor/javascript-generator-yield-next-async-await-8442d2c77185)**
3.  **[理解 Javascript 'this '关键字(上下文)](https://medium.com/datadriveninvestor/javascript-context-this-keyword-9a78a19d5786)。**
4.  **[Javascript 数据结构与映射、归约、过滤](https://levelup.gitconnected.com/write-beautiful-javascript-with-%CE%BB-fp-es6-350cd64ab5bf)**
5.  **[Javascript- Currying VS 部分应用](https://medium.com/datadriveninvestor/javascript-currying-vs-partial-application-4db5b2442be8)**
6.  **[Javascript ES6 —可迭代程序和迭代器](https://medium.com/datadriveninvestor/javascript-es6-iterables-and-iterators-de18b54f4d4)**
7.  **[Javascript 性能测试—针对 vs 针对每个 vs (map，reduce，filter，find)。](https://codeburst.io/write-beautiful-javascript-with-%CE%BB-fp-es6-350cd64ab5bf)**
8.  **[Javascript —代理](https://medium.com/datadriveninvestor/why-to-use-javascript-proxy-5cdc69d943e3)**
9.  **[Javascript —作用域](https://medium.com/datadriveninvestor/still-confused-in-js-scopes-f7dae62c16ee)**
10.  **[用 Tensorflow-js 进行图像目标检测🤔](https://levelup.gitconnected.com/image-object-detection-with-tensorflow-js-b8861119ed46)**
11.  **Nodejs 应用程序结构——构建高度可扩展的架构。**
12.  **[Node.js 10.0.0，作为后端开发人员/安全爱好者有什么期待？](https://codeburst.io/node-js-10-0-0-what-to-expect-as-a-backend-developer-security-enthusiast-f8680f132320)**
13.  **[图像处理——在 React.js 中制作自定义图像滤镜](https://levelup.gitconnected.com/image-processing-making-custom-image-filters-react-js-part-2-ee2787f56b0d)**
14.  **[谷歌印度面试问题](https://medium.com/@ideepak.jsd/google-india-phone-call-interview-question-software-developer-5a164e97ddeb)**

****谢谢！****