# 面向数据科学家和其他非 Web 开发人员的完整堆栈基础知识，第 2 部分:jQuery、D3、AJAX 和 Flask

> 原文：<https://towardsdatascience.com/full-stack-basics-for-data-scientists-and-other-non-web-developers-part-2-jquery-d3-ajax-and-90f629c6b952?source=collection_archive---------5----------------------->

这是关于基本的全栈 web 开发工具的 2 部分系列的第 2 部分，数据科学家需要这些工具来在 web 上构建基本的交互式可视化。第一部分涵盖了 HTML 和 CSS。在这一部分中，我们将实现我们的承诺，教你如何制作*交互式*可视化，包括目前最流行的 web 编程语言 JavaScript，两个最流行的库 jQuery 和 D3.js，以及一个优秀的 Python 后端框架 Flask。

# Java Script 语言

基本上，HTML 用于在页面上放置元素，CSS 用于设置这些元素的样式，包括将它们放置在页面上相对于彼此的特定位置，Javascript 用于在元素被放置在页面上后对其进行操作。例如，D3.js 只是一堆处理特定 HTML 元素类的代码——即，[支持向量图形](https://en.wikipedia.org/wiki/Scalable_Vector_Graphics) (SVG)元素，如矩形和圆形。

Javascript 可以用来以你能想到的所有方式操纵元素。它可以改变元素的样式，或者直接改变样式，或者给元素分配一个不同的 CSS 类。它可以在页面上移动元素。

这些都可以用普通的 Javascript 来完成。然而，我们甚至不会在这里深入讨论如何做到这一点，因为最好的做法是使用目前最流行的 Javascript 库[、](https://en.wikipedia.org/wiki/JQuery) [jQuery](https://jquery.com) 来完成这样的任务。

# jQuery

jQuery 允许您使用`$`语法轻松地选择页面上的元素，特别是如果它们有一个指定的“id”或“class”的话。您可以使用与 CSS 类似的语法选择特定类的所有元素，例如，`$(".fruit")`选择所有具有类`fruit`的元素，`$("#banana")`将只选择具有 *id* 的`banana`的元素。

一旦我们做到了这一点，就有了相对直观、简单的语法来操作这些元素。例如，假设我们已经选择了页面上 class `fruit`的所有元素；默认情况下，jQuery 会将它们读入一个数组，所以我们可以使用下面的`$`和`each`语法遍历它们并更改它们的属性:

```
var bananas = $(".banana");
bananas.each(function() {
  $(this).text("My text now says 'banana'");
  $(this).addClass("fruit");
}
```

你明白了。我们将在下面的函数中广泛使用这种模式。

还有另外两个基础工具，每个数据科学家都应该知道，它们被用来构建这个数独应用`[D3.JS](https://d3js.org)`和`[AJAX](http://adaptivepath.org/ideas/ajax-new-approach-web-applications/)`。

# D3。射流研究…

你可能在网上看过非常酷的`[D3.JS](https://bl.ocks.org/mbostock)`[可视化](https://bl.ocks.org/mbostock)。我认为这些展示了`D3.JS`的力量是很棒的，但是华而不实的演示从根本上来说是为了记忆和让正在发生的事情看起来像魔术，而不是照亮表面下正在发生的事情。事实上，你说“他们是怎么做到的？”是好设计的标志。这是相反的——我想让你确切地知道我是怎么做的！

所以，华丽的 D3 可视化掩盖了 D3 在做什么。 D3 到底在做什么？D3 是一个 Javascript 库。基本上，它选择和操作页面上的 HTML 元素——就是这样。它使用的语法虽然需要时间来适应，但却非常有表现力和简洁。在我看来，初学者纠结 D3 的很大一部分原因。他们的头脑中没有这样的思维模式:

*   一个页面上存在 HTML 元素——即使这些是像`<svg>`或`<circle>`这样的元素(通常是 D3 操作的元素),它们仍然是 HTML 元素。
*   *D3。JS 只是使用 Javascript 来操作这些元素*——仅此而已。

那是 D3。我们一会儿会谈到它是如何在数独应用程序中使用的。首先:什么是 AJAX，我们为什么要了解它？

# 创建交互式、快速动态网页应用的网页开发技术

[AJAX](https://en.wikipedia.org/wiki/Ajax_(programming)) 代表异步 Javascript 和 XML。它用于以下在现代 web 上非常常见的用例:当一个页面加载时，它最初只是加载向用户显示“初始”页面所需的所有 HTML 和 CSS 想象一下脸书在你的新闻提要上加载前几个条目。但是，一旦在页面上执行了操作(例如，用户向下滚动或单击按钮)，您可能希望用新信息更新页面上的内容。此外，这些信息可能必须来自外部来源，比如服务器。为了做到这一点，Javascript 有一个名为 [AJAX](http://api.jquery.com/jquery.ajax/) 的内置函数，它可以向外部服务器或函数发出请求，获取所需的信息，然后根据这些信息更新页面上的内容，完成所有这些——这是关键部分— *,而无需重新加载页面*。AJAX 是你可以向下滚动脸书新闻提要的原因，脸书不必把你带到一个包含“第二页”信息的新网站——它发出一个 AJAX 请求，得到的响应只是被添加到当前页面。

最后，让我们介绍一个更接近核心数据科学工具包的工具:Flask。

# 瓶

对于这个项目来说，Flask 允许您定义不同的路径，将 Javascript 数据路由到实际操作这些数据的 Python 函数。因此，如果您想拥有一个接受一个数字并将其乘以 2 的 Python 函数，您可以定义一个`multiply`路由，在该路由后面有一个 Python 中的`multiply_by_two`函数，并让 Javascript 调用该路由(使用 AJAX，我们将会看到)。或者，如果你想有一个函数，例如，接受一个数独板，并从板上消除一个数字作为候选，你也可以这样做。理论上，我们可以用 Javascript 完成所有这些工作。但是，因为 Python 是数据科学的标准语言，也因为它是一种比 Javascript 更简洁的通用语言——以及(真正的原因)Peter Norvig 用 Python 写了他的文章“[如何解决每一个数独](http://norvig.com/sudoku.html)”——我们将使用 Python 编写数独函数，并使用 Flask 和 AJAX 在两者之间建立接口。

# 这些工具如何组合在一起

数独应用的代码是[这里是](https://github.com/SethHWeidman/Sudoku_solver)。我们现在将介绍各种功能及其作用。

当用户导航到数独应用程序的[时，他们正在前往](http://sethweidman.com/app/sudoku/)[烧瓶应用程序](https://github.com/SethHWeidman/Sudoku_solver/blob/master/main.py)中的路线`[/](https://github.com/SethHWeidman/Sudoku_solver/blob/master/main.py#L28)`。

当用户点击[“逐步求解”](https://github.com/SethHWeidman/Sudoku_solver/blob/ac6c36495657ead14dd7fa69c0d26a969b986923/templates/index.html#L307)并点击[“开始”](https://github.com/SethHWeidman/Sudoku_solver/blob/ac6c36495657ead14dd7fa69c0d26a969b986923/templates/index.html#L309)时，它调用 Python 路由`[sudoku_action](https://github.com/SethHWeidman/Sudoku_solver/blob/master/main.py#L44)`。Flask 知道如何将单选按钮的值作为一个`request`对象读入数据，该对象的值是一个可以通过`request.form`访问的字典。特别是，它可以计算出使用`[request.form['type']](https://github.com/SethHWeidman/Sudoku_solver/blob/ac6c36495657ead14dd7fa69c0d26a969b986923/main.py#L56)`选择了哪个单选按钮。如果它检测到“逐步求解”已被选中，它将简单地呈现 HTML 模板`[sudoku_action.html](https://github.com/SethHWeidman/Sudoku_solver/blob/master/templates/sudoku_action.html)`。

该模板将立即启动`callFlaskUpdateOne`功能:

```
function callFlaskUpdateOne() {      
  $.ajax({        
    url: {{ url_for('update_one')}},
    data: extractData(),
    type: 'POST',
    success: function(response) {
      updateClasses(response['values']);  
      displayMessage(response['message'])
      highlightCell(response['peer']);          
      callFlaskUpdateOne();        
    },        
    error: function(error) {
      console.log(error);
    }    
  })  
};
```

这个函数使用 jQuery 中特定的`ajax`函数。作为参数，该函数需要

*   一个`url`，定义在哪里进行函数调用。
*   `data`，告诉它向那个函数发送什么数据。
*   请求的`type`，对于我们的目的来说是一个`POST`请求。

所以:启动这个函数首先调用函数`update_one`的 Python route。将发送到该函数的数据在`extractData()` 函数中。这个函数的目标是从实际的网页中获取关于数独板当前状态的所有数据。这样做的方式是我们到目前为止所学概念的直接应用，它利用了这样一个事实，即我们可以将关于数独板的实际数据存储在 Python 和 Javascript 都能理解的结构中。

Python 和 JavaScript 都“理解”的数独棋盘结构就是这个结构

```
{‘A1’: ‘123456789’, 'A2': '1245', 'A3': 8, ...} 
```

也就是说，它是一本有 81 个词条的词典。这些物品的 81 个键是字母 A-I 和数字 1-9 的 81 种组合。作为值，它们是包含所有尚未从方块中消除的数字的字符串。此外，每个`[cell](https://github.com/SethHWeidman/Sudoku_solver/blob/master/templates/sudoku_action.html#L11)`都被赋予了一个职业`[sudoku-solving](https://github.com/SethHWeidman/Sudoku_solver/blob/master/templates/sudoku_action.html#L11)`和一个`[name](https://github.com/SethHWeidman/Sudoku_solver/blob/master/templates/sudoku_action.html#L11)` [属性](https://github.com/SethHWeidman/Sudoku_solver/blob/master/templates/sudoku_action.html#L11)，对应于前面提到的名字字母组合之一，比如`D5`。

因此，在任何给定的时间，我们都可以使用 jQuery 提取具有该类名的所有 HTML *元素*，然后简单地遍历每个元素，并将“data”中的元素赋值为将 HTML 元素的“name”作为其键，将 HTML 元素的 *text* 作为其值。

```
function extractData() {
      output_dict = {};
      var numbers = $(".sudoku-solving");
      numbers.each(function() {
         output_dict[$(this).attr('name')] = $(this).text();
      })
      return output_dict;
    };
```

注意，在以`output_dict`开始的行中，我们使用`[]`符号在字典中查找元素，就像我们在 Python 中一样。

然后，我们用`ajax`函数中的[行](https://github.com/SethHWeidman/Sudoku_solver/blob/ac6c36495657ead14dd7fa69c0d26a969b986923/templates/sudoku_action.html#L393)和`data: extractData()`将数据发送给 Python。这是一个很好的例子，说明了 HTML、CSS、jQuery 和 AJAX 是如何交互的，我们将在这个应用中介绍的其他功能也是根据类似的原理运行的。

一旦 Python 接收到数据，它[就以相同的形式将数据发送回 Javascript](https://github.com/SethHWeidman/Sudoku_solver/blob/master/main.py#L74) ，除了 Python Sudoku 代码已经修改了数据，以便从棋盘上的一个单元格中删除一个数字作为候选。它还向 Javascript 发回一条将显示在屏幕上的“消息”,以及一个表示已经去掉数字的单元格的“对等体”:

```
return jsonify(values=new_values, message=message, peer=peer)
```

(阅读[经典的 Peter Norvig 数独文章](http://norvig.com/sudoku.html)并查看`[sudoku.py](https://github.com/SethHWeidman/Sudoku_solver/blob/master/python/sudoku.py)`中的`[eliminate_one](https://github.com/SethHWeidman/Sudoku_solver/blob/master/python/sudoku.py#L96)`功能)

此响应由`ajax`功能:`success: function(response)`接收；收到此响应后，将运行几个 JavaScript 函数:

```
updateClasses(response['values']);          displayNewData(response['values']);          displayMessage(response['message'])          highlightCell(response['peer']);
```

让我们一个接一个地看一下:

## 更新类

第一个功能是`updateClasses`。这个函数循环遍历每个数独`cells`。对于这些单元中的每一个，它查看从 Python 接收回的数据，看它接收到的数据的长度是否为 1，这意味着单元被“求解”。如果是这样，它将移除一个名为`[sudoku-not-solved](https://github.com/SethHWeidman/Sudoku_solver/blob/master/static/stylesheets/style.css#L18)`的 CSS 类，该类对单元格中尚未求解的数字进行样式化(例如，定位和调整大小)。移除了该类之后，它添加了一个新的 CSS 类`[sudoku_number](https://github.com/SethHWeidman/Sudoku_solver/blob/master/static/stylesheets/style.css#L2)`,其效果是增加该单元格中剩余数字的大小，并使其在父单元格中居中:

```
function updateClasses(input_sudoku) {
  var numbers = $(".sudoku-solving");
  numbers.each(function() {
    var candidate_numbers = input_sudoku[$(this).attr("name")];
    if (candidate_numbers.length == 1) {
       $(this).text(candidate_numbers);               $(this).addClass("sudoku_number"); 
       $(this).removeClass("sudoku-not-solved"); 
      }    
    });  
  };
```

## 显示新数据

一旦更新了类，就调用“displayNewData”函数。这个函数简单地遍历页面上的所有元素，并根据 Python 接收到的数据更新它们的文本。

```
function displayNewData(update_sudoku) {     
  var numbers = $(".sudoku-solving");
  numbers.each(function() {       
    $(this).text(update_sudoku[$(this).attr('name')]);
    });
  };
```

## 显示消息

现在我们到了涉及 D3 的部分。在将原始的数独放到页面上时，一个`<svg>`元素被附加到数独板的底部，宽度为 500，高度为 100。

```
var message_area = d3.selectAll(".update_message")
  .append("svg")
  .attr("width", 500)
  .attr("height", 100);
```

完成 AJAX 请求后，JavaScript 会收到一条消息。使用 D3，我们将消息附加到 SVG 对象，并将其放在对象底部附近的`y=80`处，并使其透明。然后，我们使用 d3 做一个过渡，将文本向上移动到`y=50`并移除透明度，这样它就好像“出现”在屏幕上。然后，我们暂停 500 毫秒，然后我们从屏幕上移除对象，再用半秒钟将文本移回`y=80`，并使其再次透明。以下是所有这些的代码:

```
function displayMessage(message) {    
var variable_message = message_area.append("text")
  .text(message)
  .attr("x", 100)
  .attr("y", 80)
  .attr("fill", "red")
  .attr("opacity", 0)
  .transition()
  .duration(500)
  .attr("y", 50)
  .attr("opacity", 1)
  .transition()
  .duration(500)
  .transition()
  .duration(500)
  .attr("y", 80)
  .attr("opacity", 0);
};
```

该函数突出显示一个特定的单元格，即 Python 代码返回的单元格。首先，它使用 Python 返回的函数名选择单元格。然后，它使用 JavaScript 函数`setTimeout`暂停 2 秒钟，然后从单元格中删除这个类。这比我想象的要简单:CSS 中的`[highlighted-cell](https://github.com/SethHWeidman/Sudoku_solver/blob/ac6c36495657ead14dd7fa69c0d26a969b986923/static/stylesheets/style.css#L89)` [类](https://github.com/SethHWeidman/Sudoku_solver/blob/ac6c36495657ead14dd7fa69c0d26a969b986923/static/stylesheets/style.css#L89)处理得很好。下面是 Javascript 代码:

```
function highlightCell(cell) { $('#'.concat(cell)).addClass('highlighted-cell')
  setTimeout(function() {
    $('#'.concat(cell)).removeClass('highlighted-cell');
  }, 2000)
};
```

## 把这些放在一起

所以，每三秒钟，我们做以下事情:

1.  使用 jQuery 从页面中提取数据。
2.  调用 Python 来获取“新数据”以显示在板上。
3.  通过更新页面上事物的类别，使页面上任何有一个数字的单元格“变大”。
4.  实际显示新数据。
5.  在屏幕底部的 SVG 上显示消息。
6.  突出显示已更新的单元格。

## 看到它的实际应用

导航到[sethweidman.com/app/sudoku](http://sethweidman.com/app/sudoku)。这个应用程序预装了我在谷歌上找到的第一个“简单数独”。点击“运行数独”来看看它的行动！

# 结论

即使您自己不是 web 开发人员——我想大多数人都不是——我也希望这个由两部分组成的系列文章能让您了解 web 开发的基本工具是如何工作的，这样您至少可以更自信地参与到团队关于应该构建什么以及应该如何构建的讨论中。对于正在阅读这篇文章的数据科学家来说:我相信，如果你花时间学习这些工具，你会成为更有效的数据科学家，这样你就可以构建可视化，更好地向你的老板、客户甚至同事说明你正在做什么——毕竟，我们在 Metis 教授这些工具是有原因的！