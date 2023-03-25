# 如何扩展固定张量流估计量

> 原文：<https://towardsdatascience.com/how-to-extend-a-canned-tensorflow-estimator-to-add-more-evaluation-metrics-and-to-pass-through-ddf66cd3047d?source=collection_archive---------6----------------------->

## 或者在使用固定评估器时，如何添加更多的评估指标并传递实例键

像 DNNRegressor 和 DNNLinearCombinedRegressor 这样的预构建的(“封装的”)估算器使编写 TensorFlow 程序变得简单。例如，下面是一个完整的 TensorFlow 程序，用于从内存中的 Pandas 数据帧开始训练 TensorFlow 模型:

```
import tensorflow as tffeatcols = [
  tf.feature_column.numeric_column("sq_footage")
]
model = tf.estimator.LinearRegressor(featcols, './model_trained')def train_input_fn(df, num_epochs):  # a Pandas dataframe
  return tf.estimator.inputs.pandas_input_fn(
    x = df,
    y = df['price'],
    num_epochs=num_epochs)model.train(train_input_fn(df,100), steps=1000)
```

如果你想像 scikit-learn 一样使用 TensorFlow，代码行数也差不多。

## 稳健估计量

然而，实际上，你想要比这更健壮。你想要读取不完全适合内存的分片文件，批处理它们，等等。这涉及到在您的[输入函数](https://github.com/GoogleCloudPlatform/training-data-analyst/blob/master/courses/machine_learning/deepdive/03_tensorflow/taxifare/trainer/model.py#L37)中使用 TensorFlow 中的数据集 API 的更多代码。当你训练的时候，你需要时不时的检查和评估。所以，你不要只叫 train，你要叫 [train_and_evaluate](https://cloud.google.com/blog/big-data/2018/02/easy-distributed-training-with-tensorflow-using-tfestimatortrain-and-evaluate-on-cloud-ml-engine) 。

预构建的评估者非常简单，但也非常固执——他们选择一种特定的做事方式，并给你一堆现成的东西。如果你想要更大的灵活性？好吧，写一个自定义估算器([见例](https://medium.com/google-cloud/how-to-do-time-series-prediction-using-rnns-and-tensorflow-and-cloud-ml-engine-2ad2eeb189e8))！

但两者之间存在灰色地带。您可以使用固定的估计器，并且仍然可以获得一定的灵活性。怎么会？使用 [extenders.py](https://github.com/tensorflow/tensorflow/blob/master/tensorflow/contrib/estimator/python/estimator/extenders.py) 中的功能。在这篇博文中，我将向您展示两种这样的能力(顺便提一下，这是两个非常[常见的](https://github.com/GoogleCloudPlatform/cloudml-samples/issues/67)问题)。

![](img/4db89018f68f40442934184c9a206024.png)

You can extend a canned estimator too!

## 如何添加额外的评估指标

当您使用 DNNRegressor 时，作为评估循环一部分的度量只有 average_loss，在本例中恰好是 RMSE。但是如果您想要更多的度量标准呢？简单地用 *add_metrics* 包装估算器，如下所示:

```
def my_rmse(labels, predictions):
    pred_values = predictions['predictions']
    return {'rmse': tf.metrics.root_mean_squared_error(labels, pred_values)}def train_and_evaluate(output_dir):

    estimator = tf.estimator.DNNLinearCombinedRegressor(
        ...)

    estimator = tf.contrib.estimator.add_metrics(estimator, my_rmse)  
    ... tf.estimator.train_and_evaluate(estimator, train_spec, eval_spec)
```

让我们稍微解释一下:

1.  编写一个函数，根据标签和预测值计算感兴趣的指标，并返回一个字典。
2.  将该函数与您的原始估算器一起传递给 add_metrics
3.  从该点开始使用返回的估计量

注意，上面的 *my_rmse* 方法接收标签和整个预测张量。这样，如果您正在进行分类，并且您的一些指标需要预测的类，而其他指标需要预测的概率，您可以计算您想要的。

在这里看到一个[完整的例子](https://github.com/GoogleCloudPlatform/training-data-analyst/blob/master/courses/machine_learning/deepdive/07_structured/babyweight/trainer/model.py#L127)。

## 如何将键和输入特征转发到输出

由固定估计器返回的预测 dict 中只有预测(咄！).但是，在很多情况下，您希望预测也包含一些输入。例如，您可能有一个与每一行相关联的唯一键，并且您希望该键成为预测的一部分，以便您知道预测对应于哪一行。或者，您可能希望根据特定的输入值来计算评估(例如，根据婴儿是早产还是足月来计算婴儿体重的 RMSE)。

为此，用 forward_features 包装估计器，如下所示:

```
estimator = tf.contrib.estimator.forward_features(estimator, KEY_COLUMN)
```

你可以传入一个单独的字符串(就像我上面做的那样)或者一个字符串列表。理想情况下，这就是你所需要的。然而，有两个警告。

第一个警告是，上面的键列被添加到预测的输出中，但没有添加到导出签名中，因此当模型在内存中时它可以工作，但如果在训练后恢复，它就不能工作(这没有意义:我已经向 TensorFlow repo 提交了一个 pull 请求来解决这个问题)。在那之前，你需要这个[解决方案](https://github.com/GoogleCloudPlatform/training-data-analyst/blob/master/courses/machine_learning/deepdive/07_structured/babyweight/trainer/model.py#L132):

```
def forward_key_to_export(estimator):
    estimator = tf.contrib.estimator.forward_features(estimator, KEY_COLUMN)
    # return estimator## This shouldn't be necessary (I've filed CL/187793590 to update extenders.py with this code)
    config = estimator.config
    def model_fn2(features, labels, mode):
      estimatorSpec = estimator._call_model_fn(features, labels, mode, config=config)
      if estimatorSpec.export_outputs:
        for ekey in ['predict', 'serving_default']:
          estimatorSpec.export_outputs[ekey] = \
            tf.estimator.export.PredictOutput(estimatorSpec.predictions)
      return estimatorSpec
    return tf.estimator.Estimator(model_fn=model_fn2, config=config)
    ##
```

第二个警告是，你不能既传递一个特征*又*把它作为模型的输入。要么消费输入，要么传递输入。这里的解决方法是使用 tf.identity 在输入函数中复制这样的列:

```
features['gw'] = tf.identity(features['gestation_weeks'])
```

希望这有所帮助！