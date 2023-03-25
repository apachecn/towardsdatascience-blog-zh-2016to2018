# Clojure 中的 Spark 机器学习

> 原文：<https://towardsdatascience.com/spark-machine-learning-in-clojure-1d5a2c97924a?source=collection_archive---------6----------------------->

虽然 Spark 是在 Scala 中实现的，但是我们可以使用 Spark 平台来完成机器学习任务，这要感谢 [Sparkling 库](https://github.com/gorillalabs/sparkling)。

在这张工作表中，我们将演示在机器学习的分类任务中使用 Sparkling。让我们首先加载依赖项

```
(ns itchy-garden
  (:require [sparkling.conf :as conf]
            [sparkling.core :as s]
            [sparkling.ml.core :as m]
            [sparkling.ml.classification :as cl]
            [sparkling.ml.transform :as xf]
            [sparkling.ml.validation :as v]) (:import [org.apache.spark.api.java JavaSparkContext]
           [org.apache.spark.sql DataFrame SQLContext]
           [org.apache.spark.ml.classification NaiveBayes LogisticRegression
            DecisionTreeClassifier RandomForestClassifier GBTClassifier ]
           [java.io File])) ;;outputnil
```

让我们加载一个 libsvm 格式的数据集。这包括大约 3k 个实例，其中每个实例都有一个值为 0 或 1 的目标变量，以及多个连续值的特征。

请注意，Spark 的 ML 库实现仅适用于*正标签*，标签在 1，-1 范围内的数据集不能开箱即用，标签需要更改为正。

```
(defn download-dataset
  []
  (let [svm-dataset-path
        "http://www.csie.ntu.edu.tw/~cjlin/libsvmtools/datasets/binary/svmguide1"
        tmpfile (.getPath (File/createTempFile "svmguide" "svm"))
        _ (spit tmpfile (slurp svm-dataset-path))]
    tmpfile))(def dataset-path (download-dataset))
```

# 交互效度分析

我们将在这个数据集上训练一个二进制分类器，并在 areaUnderROC 曲线度量上使用交叉验证来评估它的性能。

这个 API 使用中间件模式(受[环](https://github.com/ring-clojure/ring/wiki/Concepts)的启发)来指定这些工件:

*   处理函数进行交叉验证
*   add-dataset 函数采用一个函数，该函数返回 Spark 数据帧中的数据集
*   估计量是所使用的分类器或回归量的类型。在这种情况下，我们将使用逻辑回归分类器。
*   这种情况下的评估器是二元分类评估器。如果我们有一个多类分类问题，一个多类评估将适用。分类的默认度量是 ROC 曲线下的面积。

*run-pipeline* 函数将执行处理程序并返回交叉验证的 areaUnderROC 指标。

```
(let [cvhandler (-> m/cv-handler
                    (m/add-dataset (partial m/load-libsvm-dataset dataset-path))
                    (m/add-estimator cl/logistic-regression)
                    (m/add-evaluator v/binary-classification-evaluator))]
  (m/run-pipeline cvhandler));;output: areaUnderROC score(0.989250155930387)
```

前面的示例使用了逻辑回归的默认选项以及赋值器。在下一个示例中，我们将指定

*   分类器的超参数(弹性网参数)
*   评估者的不同评估指标(精确召回曲线下的面积)

```
(let [cvhandler (-> m/cv-handler
                    (m/add-dataset (partial m/load-libsvm-dataset dataset-path))
                    (m/add-estimator cl/logistic-regression {:elastic-net-param 0.01})
                    (m/add-evaluator v/binary-classification-evaluator {:metric-name "areaUnderPR"} ))]
  (m/run-pipeline cvhandler));;output: area under Precision Recall(0.9941958048724657)
```

我们还可以更改交叉验证处理程序本身的参数。让我们将折叠次数更改为 5 次(从默认的 3 次)

```
(let [cvhandler (-> (partial m/cv-handler {:num-folds 5})
                    (m/add-dataset (partial m/load-libsvm-dataset dataset-path))
                    (m/add-estimator cl/logistic-regression)
                    (m/add-evaluator v/binary-classification-evaluator))]
  (m/run-pipeline cvhandler));;output(0.9894297378637298)
```

# 训练-验证分割

除了使用交叉验证，我们还可以使用单一折叠，并根据百分比分割训练测试集。

我们将使用训练验证处理程序，而不是交叉验证处理程序，而其他参数保持不变。

```
(let [tvhandler (-> m/tv-handler
                    (m/add-dataset (partial m/load-libsvm-dataset dataset-path))
                    (m/add-estimator cl/logistic-regression)
                    (m/add-evaluator v/binary-classification-evaluator))]
  (m/run-pipeline tvhandler));;output (0.9919497271895795)
```

让我们指定一个不同的训练与验证比率，其中训练集是 60%，其余的是验证集。

```
(let [tvhandler (-> (partial m/tv-handler {:train-ratio 0.6} )
                    (m/add-dataset (partial m/load-libsvm-dataset dataset-path))
                    (m/add-estimator cl/logistic-regression)
                    (m/add-evaluator v/binary-classification-evaluator))]
  (m/run-pipeline tvhandler));;output(0.9897219862173144)
```

# 网格搜索

通常分类器使用多个超参数，我们希望找到给出最佳分类分数的超参数的值。SparkML 为此提供了网格搜索功能。参见[链接](http://scikit-learn.org/stable/modules/grid_search.html)了解 Scikit-learn 中的网格搜索

我们将指定一系列*正则化*值，并为每个值找到交叉验证的分数。

```
(defn addregularization
  "sets the regularization parameters to search over"
  [regparam est]
  (v/param-grid [[(.regParam est) (double-array regparam)]]))(let [cvhandler (-> m/cv-handler
                    ;;search for the best value for the regularization parameter 
                    (m/add-grid-search (partial addregularization [0.1 0.05 0.01]))
                    (m/add-evaluator v/binary-classification-evaluator)
                    (m/add-estimator cl/logistic-regression)
                    (m/add-dataset (partial m/load-libsvm-dataset dataset-path)))]
  (m/run-pipeline cvhandler));;output(0.9730089312309409 0.9773629797090069 0.9853807421182689)
```

这 3 个分数对应于正则化的三个值，我们可以看到最后一个值(0.01)返回了 AucROC 度量上的最佳交叉验证分数。