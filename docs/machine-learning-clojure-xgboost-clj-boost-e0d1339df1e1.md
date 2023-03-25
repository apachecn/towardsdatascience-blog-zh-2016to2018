# 使用 XGBoost 和 clj-boost 在 Clojure 中进行机器学习

> 原文：<https://towardsdatascience.com/machine-learning-clojure-xgboost-clj-boost-e0d1339df1e1?source=collection_archive---------13----------------------->

## 机器学习+ REPL = ❤

> Clojure 这是一个 [LISP](https://en.wikipedia.org/wiki/Lisp_(programming_language)) 。所以作为一个 LISP，它有很多括号。既然我们已经把那件事一吐为快，我们可以继续谈论更严肃的事情了。

![](img/966a41195961c40fd4ae0cd32e856b5d.png)

*Obligatory xkcd comic*

# 为什么是 Clojure？

你可能从来没有听说过 **Clojure** ，更不用说数据科学和机器学习了。那么，为什么您会对使用它来做这些事情感兴趣呢？我来告诉你为什么:要让要紧的事(数据)**一流**！

在 Clojure 中我们不处理类、对象等等，一切都只是数据。而那个数据是**不可变的**。这意味着，如果您搞砸了您的转换，数据将是好的，您不必从头再来。

前一个只是我想到的原因之一，另一个可能是 [JVM](https://en.wikipedia.org/wiki/Java_virtual_machine) 。是的，我们都在某种程度上讨厌它，但不要搞错:它自 1991 年开发以来，一直是**生产级**。这就是为什么全世界的企业仍然在 JVM 上运行和开发软件。考虑到这一点，Clojure 甚至可以被最保守的公司所接受，因为从根本上说，它是他们已经知道的东西。

我想说的第二点是 REPL。这不像通常的语言外壳(例如 Python REPL ),通常非常基础和烦人，但是它有**超能力**！去现场看看该服务在生产中发生了什么会很好吗？[搞定！](https://github.com/nrepl/drawbridge)你想要一个既能用于生产又能用于实验的绘图服务吗？[搞定！](https://github.com/metasoarous/oz)您是否碰巧有嵌套的数据结构，并且可视化地探索它们以更好地理解它们会很好？[搞定！](https://github.com/eggsyntax/datawalk)

这意味着你并不真的需要像 jupyter 笔记本这样的东西，尽管如果你在这样的环境中感觉更舒服，有一些选项可以无缝地工作: [clojupyter](https://github.com/clojupyter/clojupyter) 是用于 Jupyter 笔记本的 Clojure 内核，而[大猩猩 REPL](http://gorilla-repl.org/) 是本地 Clojure 笔记本解决方案。

# 数据读取

如果你从未看过 Clojure 代码，或者如果你刚刚开始，我的建议是看一看[在 Y 分钟内学会 clo jure](https://learnxinyminutes.com/docs/clojure/)以便能够跟上这一点，那么当我开始学习时，我最喜欢的来源之一是[clo jure for the Brave and True](https://www.braveclojure.com/)。无论如何，我会仔细解释代码中的每一步，这样每个人都可以很容易地理解。

这只是一个介绍，所以我们将使用臭名昭著的[虹膜数据集](https://archive.ics.uci.edu/ml/datasets/iris)。如果你还没有 [Leiningen](https://leiningen.org/) 获取并安装它，它真的很容易使用，并且是 Clojure 事实上的构建工具。现在，您可以通过启动以下命令从命令行创建一个新的项目框架:

```
lein new clj-boost-demo
cd clj-boost-demo
```

您应该具有如下所示的目录结构:

```
.
├── CHANGELOG.md
├── doc
│   └── intro.md
├── LICENSE
├── project.clj
├── README.md
├── resources
├── src
│   └── clj_boost_demo
│       └── core.clj
└── test
└── clj_boost_demo
└── core_test.clj
```

我们最关心的文件是`project.clj`，它是 Leiningen 正确准备和构建项目的基础，`src`是我们放置应用程序或库的代码的地方，`resources`是我们放置 Iris csv 文件的地方，您可以从这里的[获得](https://raw.githubusercontent.com/uiuc-cse/data-fa14/gh-pages/data/iris.csv)。现在我们可以定义`project.clj`文件，这是 Clojure 的另一个“明智”之处:你必须**显式声明**库和版本，这总是一件好事。

```
(defproject clj-boost-demo "0.1.0-SNAPSHOT"
  :description "FIXME: write description"
  :url "http://example.com/FIXME"
  :license {:name "Eclipse Public License"
            :url "http://www.eclipse.org/legal/epl-v10.html"}
  :dependencies [[org.clojure/clojure "1.9.0"]
                 [org.clojure/data.csv "0.1.4"]
                 [clj-boost "0.0.3"]])
```

如果我们保存`project.clj`并在 shell 中启动`lein run`，Leiningen 将获得所有需要的依赖项并启动一个 REPL。我们可以通过打开和修改`src/clj-boost-demo/core.clj`文件进入数据加载和传输。我们会在那里找到一些占位符代码，我们可以去掉它，开始写我们的代码。

在 Clojure 中，我们使用名称空间，通常一个文件包含一个定义导入的名称空间:

```
(ns clj-boost-demo.core
  (:require [clj-boost.core :as boost]
            [clojure.java.io :as io]
            [clojure.data.csv :as csv]))(def iris-path "resources/iris.csv")
```

定义一个新的名称空间，从名称空间定义导入库是一个好的实践，就像我们在这里做的一样。`:require`这是一个**关键字**，它本身就是 Clojure 中的一个类型，我们稍后会看到它们为什么重要，`[clj-boost.core :as boost]`意味着我们想要使用`clj-boost`库中的`core`名称空间，但是我们想要用名称`boost`引用它下面的所有名称。如果你懂 Python，这和做`import library as lbr`是一样的。

使用`def`,我们在当前名称空间中全局创建新的**变量**。在这种情况下，我们指向一个表示数据集所在路径的字符串。通常在 Clojure 中我们不会定义很多全局名，除了函数名。事实上,`iris-path`将是我们在本演示中使用的唯一全球名称！

为了读取 Iris csv，我们使用以下代码:

```
(defn generate-iris
  [iris-path]
  (with-open [reader (io/reader iris-path)]
    (into []
          (comp (drop 1) (map #(split-at 4 %)))
          (csv/read-csv reader))))
```

这段代码定义了(`defn`)一个名为`generate-iris`的函数，它将 Iris csv 的路径作为参数。然后我们在给定的路径下打开一个到文件的连接，当我们完成时这个连接将被关闭(`with-open`)。当你在一个函数调用之后看到一个**向量**，它带有一个符号，后面跟着一些代码——`[reader (io/reader iris-path)]`——这就是局部绑定。

本地绑定有助于避免混淆全局名称空间，并将代码执行分成微小的部分。在这种情况下，我们使用一个 Java 阅读器，确切地说是一个`BufferedReader`，来打开和读取文件。如您所见，我们通过执行`io/reader`来使用导入的`clojure.java.io`名称空间，而`name/`是访问驻留在导入的名称空间中的名称的语法。

下面的代码可能看起来有点深奥，但这只是习惯问题。让我们从 REPL 开始分解所有的步骤。

```
(with-open [reader (io/reader iris-path)] 
  (csv/read-csv reader)) 
;IOException Stream closed java.io.BufferedReader.ensureOpen
```

上面的代码抛出一个错误，那是因为`csv/read-csv`是**懒**。默认懒惰是 Clojure 的另一个特性:大部分 Clojure 函数不会返回任何东西，直到你需要那些值。如果你考虑一下，这很好:如果我们有一个非常大的文件，我们不必将它全部加载到内存中来处理它，但是我们可以逐行读取和处理它，同时将结果写入另一个文件。

为了使函数变得更有吸引力，我们可以使用`doall`:

```
(with-open [reader (io/reader iris-path)] 
  (doall (csv/read-csv reader))) 
;(["sepal_length" "sepal_width" "petal_length" "petal_width" "species"] 
; ["5.1" "3.5" "1.4" "0.2" "setosa"] 
; ["4.9" "3" "1.4" "0.2" "setosa"]...)
```

结果是包含字符串的**向量**的**序列**。序列是 Clojure 中最重要的数据结构之一:它们是懒惰的、不可变的，可以由任何其他 Clojure 数据结构产生。更多关于他们的信息，请查看官方 Clojure 文件。**向量**非常类似于 Python 列表，区别在于它们是持久的。

为了更好地理解**不变性**和**持久性**让我们尝试一个小实验:

```
(def a [1 2 3]) 
(println a) 
;[1 2 3] (conj a 4) 
;[1 2 3 4] (println a) 
;[1 2 3]
```

正如你所看到的，我们创建了一个向量`a`，然后`conj`(向向量追加元素)4 到`a`，结果是一个全新的数据结构，事实上`a`仍然有初始值。

# 数据处理

在这种情况下，我们不再关心标题，因为`"species"`列是我们想要预测的类，也是最后一个，所以我们可以立即开始处理原始数据。为了简化这个过程，我们定义了一个在 REPL 中使用的`demo-reader`函数，并开始处理结果数据:

```
(defn demo-reader [] 
  (with-open [reader (io/reader iris-path)] 
    (doall (csv/read-csv reader))))
```

然后，我们将使用**线程宏**来逐步试验和应用转换:

```
(->> (demo-reader) (take 3)) 
;(["sepal_length" "sepal_width" "petal_length" "petal_width" "species"] 
; ["5.1" "3.5" "1.4" "0.2" "setosa"] 
; ["4.9" "3" "1.4" "0.2" "setosa"])
```

让我们将一个值作为后续函数的最后一个参数:你肯定还记得在学校时他们教过你，要求解 f(g(x))，你应该从求解 g(x)= x’开始，然后 f(x’)= x”。线程宏它只是语法糖，使代码更可读。

这里有一个简单的例子:

```
(inc 1) 
;2 (dec (inc 1)) 
;1 (-> 1 inc dec) 
;1
```

我们`inc`将`1`加 1，然后`(dec (inc 1))`表示`inc`加`1`，然后`dec`将结果-2-加 1，得到 1。基本上，我们从右到左阅读来理解函数的应用顺序。使用`(-> 1 inc dec)`我们可以从左至右阅读操作。有关**线程宏**的更多信息，请查看[官方 Clojure 文档](https://clojure.org/guides/threading_macros)。

回到`(->> (demo-reader) (take 3))`正如你看到的结果，我们只从 csv 中获得前 3 个向量。`take`完全按照您的想法去做:它懒洋洋地从给定的集合中获取`n`值。这在实验时非常有用，否则我们将不得不处理整个序列。

要从序列中删除标题，我们可以从结果中`drop`第一行:

```
(->> (demo-reader) (take 3) (drop 1)) 
;(["5.1" "3.5" "1.4" "0.2" "setosa"] 
; ["4.9" "3" "1.4" "0.2" "setosa"])
```

现在，因为我们想把 X 值从 Y 值(我们想预测的类)中分离出来，所以最好一次完成。如果您来自 Python 或其他类似 C 语言的语言，您可能会尝试使用循环来做这件事，但是在 Clojure 中，我们做的事情不同。

使用`map`,我们可以对集合中的所有值应用一个函数:

```
(map inc [1 2 3]) 
;(2 3 4)
```

在这种情况下，我们想要分割值，猜猜看，有一个`split-at`函数在等着我们！

```
(split-at 2 [1 2 3])
*;[(1 2) (3)]*
```

这正是我们所需要的，所以我们将定义一个匿名函数并在我们的向量上使用它:

```
(->> (demo-reader) 
     (take 3) 
     (drop 1) 
     (map #(split-at 4 %))) 
;([("5.1" "3.5" "1.4" "0.2") ("setosa")] 
; [("4.9" "3" "1.4" "0.2") ("setosa")])
```

为了定义命名函数，我们使用`defn`，它是一个**宏**，让我们避免每次都输入`(def my-func (fn [arg] "do something"))`，所以通过简单的`(fn [arg] "I have no name")`，我们得到一个匿名函数。

`#(split-at 4 %)`是另一个和`(fn [x] (split-at 4 x))`解析一样的简写，所以只是省打字的一种方式。

现在让我们通过使用**传感器**将所有东西放在一起。如同带有传感器的线程宏一样，我们将**的**函数组合在一起，但是传感器返回一个只传递一次数据的函数。如果它们一开始看起来有点晦涩，不要担心，我也花了一些时间来理解这个概念，我建议你仔细阅读这个写得非常好的关于传感器的系列文章。

```
(def xf (comp (drop 1) (map #(split-at 4 %)))) (into [] xf [[1 2 3 4 5] [1 2 3 4 5]]) 
;[[(1 2 3 4) (5)]]
```

使用`comp`我们将函数组合在一起，只返回一个函数，而使用`into`我们循环遍历一个集合，并将结果放入作为第一个参数给出的集合中。我喜欢这样来思考这个过程:这就像我们从一个集合中把**个值拉到另一个集合中，但是当我们这么做的时候，我们对所有的值都应用了一个函数`xf`。**

结果是我们开始的`generate-iris`函数:

```
(defn generate-iris
  [iris-path]
  (with-open [reader (io/reader iris-path)]
    (into []
          (comp (drop 1) (map #(split-at 4 %)))
          (csv/read-csv reader))))
```

现在我们想从这个`([("5.1" "3.5" "1.4" "0.2") ("setosa")] [("4.9" "3" "1.4" "0.2") ("setosa")])`转到我们能以更简单的方式处理的东西:`([5.1 3.5 1.4 0.2 0] [4.9 3.0 1.4 0.2 0])`。基本上，我们将字符串解析成数字，并将类( *setosa* 、 *virginica* 和 *versicolor* )转换成整数。

让我们从抽象出所需的转换开始:

```
(defn parse-float
  [s]
  (Float/parseFloat s))(->> (generate-iris iris-path) 
     (take 2) 
     (map first)) ; first returns the first element of a collection ;(("5.1" "3.5" "1.4" "0.2") ("4.9" "3" "1.4" "0.2")) (->> (generate-iris iris-path) 
     (take 2) 
     (map first) 
     (map #(map parse-float %))) 
; ((5.1 3.5 1.4 0.2) (4.9 3.0 1.4 0.2)) (->> (generate-iris iris-path) 
     (take 2) 
     (map first) 
     (map #(map parse-float %)) 
     (map vec)) 
; ([5.1 3.5 1.4 0.2] [4.9 3.0 1.4 0.2])
```

通过这些变换，我们可以为我们的模型构建 X。让我们建造一个名为**的变压器**:

```
(def transform-x
  (comp
   (map first)
   (map #(map parse-float %))
   (map vec)))
```

取而代之的是 Y:

```
(->> (generate-iris iris-path) 
     (take 2) 
     (map last)) ; last takes the last item from a collection 
;(("setosa") ("setosa")) (let [l (first '("setosa"))] 
  (case l "setosa" 0 "versicolor" 1 "virginica" 2)) 
;0
```

停一下，用`let`我们可以创建本地绑定，比如给只存在于本地空间而非全局的数据或函数命名。`case`这是避免嵌套`(if condition "this" "else this")`的一种方式。我们说的是:如果`l`是`=`到`"setosa"`那么返回`0`，如果是`=`到`"versicolor"`返回`1`，而如果是`=`到`"virginica"`返回 2。

这样，如果给定值与三种情况都不匹配，我们就会得到一个错误。这也有助于检查数据质量。

```
(->> (generate-iris iris-path) 
     (take 2) 
     (map last) 
     (map (fn [label] 
       (let [l (first label)] 
         (case l "setosa" 0 "versicolor" 1 "virginica" 2))))) 
;(0 0)(def transform-y
  (comp
   (map last)
   (map (fn [label]
          (let [l (first label)]
            (case l
              "setosa"     0
              "versicolor" 1
              "virginica"  2))))))(defn munge-data
  [iris-data]
  (let [x (into [] transform-x iris-data)
        y (into [] transform-y iris-data)]
    (map conj x y)))
```

# 列车-测试分离

当进行机器学习时，要做的最重要的事情之一是在一个**训练**和一个**测试**集合上分割数据。一个好的分割需要随机采样，所以我们将从头实现一个非常简单的采样器。

```
(defn train-test-split
  [n dataset]
  (let [shuffled (shuffle dataset)]
    (split-at n shuffled)))
```

`train-test-split`在训练集中获取一个集合和一些你想要的实例。`shuffle`函数只是简单地打乱了集合，所以我们每次都会得到一个随机的结果，并且可以很容易地避免重复。

如果您有一个相当大的数据集，这种解决方案并不是最佳的，在这种情况下，您可能想要看一下[采样](https://github.com/bigmlcom/sampling)，这是一个非常好的库，可以处理关于采样的所有事情。

```
(defn train-set
  [split-set]
  (let [set (first split-set)]
    {:x (mapv drop-last set)
     :y (mapv last set)}))

(defn test-set
  [split-set]
  (let [set (last split-set)]
    {:x (mapv drop-last set)
     :y (mapv last set)}))
```

利用上述函数，我们生成训练集和测试集，作为带有 2 个**键** : `:x`和`:y`的 2 个**映射**。Clojure 中的地图是一等公民，具有非常好的属性:

```
; Create a map with mixed types 
{:a 1 "key" "string" 2 1/3}

; Maps are functions 
({:a 1} :a) 
;1 ; Keywords are functions as well 
(:a {:a 1}) 
;1 ; Access other kinds of keys by using the map... 
({1 2} 1) 
;2 ; ...or the get function 
(get {"key" 1} "key") 
;1
```

地图远不止这些，如果你不知道它们，你应该看看这里的。

# 训练和预测

**XGBoost** 是一个集合模型，它使用梯度增强来最小化损失函数。如果你不能理解这些单词，我的建议是查看[这个非常好的算法解释](https://medium.com/@samudralaajit/unveiling-mathematics-behind-xgboost-c7f1b8201e2a)(有图片和公式)。

[**clj-boost**](https://gitlab.com/alanmarazzi/clj-boost) 给你一个 Clojure 接口到底层 Java 实现的库，这样我们可以避免 Java 互操作，得到同样的结果。为了训练一个模型，我们必须从数据中创建一个 *DMatrix* ,我们希望将这些数据提供给算法进行学习。这不是我的选择，我可以将数据转换隐藏在 API 实现的后面，但是有一个问题:一旦你将数据放入一个 *DMatrix* 中，你就不能再触摸或查看它们了。

```
(defn train-model
  [train-set]
  (let [data   (boost/dmatrix train-set)
        params {:params         {:eta       0.00001
                                 :objective "multi:softmax"
                                 :num_class 3}
                :rounds         2
                :watches        {:train data}
                :early-stopping 10}]
    (boost/fit data params)))
```

用`dmatrix`我们序列化我们的训练集，有各种方法生成一个 *DMatrix* ，所以我建议你看一下[文档](https://cljdoc.org/d/clj-boost/clj-boost/CURRENT/api/clj-boost.core#dmatrix)或[自述文件](https://gitlab.com/alanmarazzi/clj-boost/tree/dev#dmatrix)。`:params`映射的作用就像是 *XGBoost* 的配置，这是我们可以用它做什么的一个非常小的例子，要知道所有可能的选项总是参考[官方文档](https://xgboost.readthedocs.io/en/latest/parameter.html)。

这里我们说 *XGBoost* 应该以 0.00001 的*学习率* — `:eta`进行训练，因为我们正在对 3 个类进行分类—*setosa、versicolor* 和*virginica—*我们将`:objective`设置为 *multi:softmax* ，并告诉 *XGBoost* 我们与`:num_class`有多少个类。

*XGBoost* 将进行 2 次`:rounds`的增强，将通过将*训练集*传递给`:watches`图来评估其准确性(这不是一个好的实践，但这只是一个例子),如果准确性将开始连续 10 次迭代增加，它将由于`:early-stopping`参数而停止训练。

对定义的*数据*和*参数*调用`fit`从头训练一个 *XGBoost* 模型，并返回一个 *Booster* 实例。我们可以使用 *Booster* 进行预测，我们可以将它持久化到磁盘上，或者将其作为基线提供给另一个 *XGBoost* 模型。

```
(defn predict-model
  [model test-set]
  (boost/predict model (boost/dmatrix test-set)))

(defn accuracy
  [predicted real]
  (let [right (map #(compare %1 %2) predicted real)]
    (/ (count (filter zero? right))
       (count real))))
```

虽然我们通过*训练装置*本身来检查训练性能，但我们将在之前准备的*测试装置*上检查准确性。`predict`需要一个*模型*和数据作为我们想要预测的 *DMatrix* 并返回一个预测向量。

让我们把一切都打包成一个`-main`函数，这样我们就可以从 REPL 和命令行运行整个分析。

```
(defn -main
  []
  (let [split-set    (->> iris-path
                          generate-iris
                          munge-data
                          (train-test-split 120))
        [train test] (map #(% split-set) [train-set test-set])
        model     (train-model train)
        result    (predict-model model test)]
    (println "Prediction:" (mapv int result))
    (println "Real:      " (:y test))
    (println "Accuracy:  " (accuracy result (:y test)))))
```

我们生成*分裂集*，然后我们使用一个小技巧:我们将多个函数映射到一个集合上，而不是相反。然后我们训练 XGBoost 模型并得到预测。

```
(-main) 
Prediction: [1 1 2 0 2 2 2 2 2 1 1 0 1 2 0 1 1 1 0 1 0 2 1 1 0 0 1 2 1 1] 
Real: [1 1 2 0 2 2 2 2 2 1 1 0 1 2 0 1 1 1 0 1 0 2 1 1 0 0 1 2 2 1]
Accuracy: 29/30
```

# 灵活性与生产

您可能没有注意到，尽管我们编写的代码足够简单灵活，可以用于分析和实验，但这也是**生产就绪**代码。如果您在命令行中从项目的根目录执行`lein run`，您将获得与从 REPL 执行`(-main)`相同的结果。因此，向程序添加功能是微不足道的，例如，当程序发生变化时，您可能希望向它输入新数据，并且您希望重新训练您的模型。

如果我们现在在 Jupyter 笔记本上用 Python 做类似的事情，我们可能会到处都是任务，命令式代码必须从头开始重写，以使它在某种程度上为生产做好准备，我不会谈论这样一个事实，即如果数据管理性能可能是一个问题，通过组成转换器，我们几乎可以获得**免费**的并行化。

现在你可以用 [clj-boost](https://gitlab.com/alanmarazzi/clj-boost) 玩一会儿了，不要忘了还有[文档](https://cljdoc.org/d/clj-boost/clj-boost/CURRENT/doc/readme)可用，这仍然是一项正在进行的工作，所以如果有问题、想法、改进方法或者只是你正在使用它并且你对它感到满意，请让我知道。

你可以在 [clj-boost repo](https://gitlab.com/alanmarazzi/clj-boost/blob/master/demo/tutorial.clj) 上找到完整的脚本，不要忘记 clj-boost 是自由/开源软件，任何形式的贡献都是可以接受的！

这篇文章最初发表在 [rDisorder](https://www.rdisorder.eu/2018/12/03/machine-learning-clojure-xgboost/) 上(请特别查看更好的代码片段)