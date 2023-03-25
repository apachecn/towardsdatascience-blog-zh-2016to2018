# 使用 TensorFlow Estimator API 时如何定制分布式培训

> 原文：<https://towardsdatascience.com/how-to-configure-the-train-and-evaluate-loop-of-the-tensorflow-estimator-api-45c470f6f8d?source=collection_archive---------6----------------------->

TensorFlow 的 Estimator API 提供了一个简单的高级 API 来训练机器学习模型。您可以在估计器上使用 train()、evaluate()或 predict()方法。然而，大多数情况下，培训是在一个循环中以分布式的方式进行的，在培训过程中定期进行评估。为此，您将使用 train_and_evaluate 循环:

```
tf.estimator.train_and_evaluate(estimator, train_spec, eval_spec)
```

在本文中，我将讨论您可能希望在 train_spec 和 eval_spec 中指定哪些内容。总之，这些选项允许 train_and_evaluate 提供一种强大的、可定制的方法来进行分布式培训。

这里有一个完整的 train_and_evaluate(完整代码在 [GitHub](https://github.com/GoogleCloudPlatform/training-data-analyst/blob/master/courses/machine_learning/deepdive/06_structured/babyweight/trainer/model.py#L152) 上)。我将挑选出不同的部分来讨论:

```
def train_and_evaluate(output_dir):
    EVAL_INTERVAL = 300 # seconds run_config = tf.estimator.RunConfig(save_checkpoints_secs = EVAL_INTERVAL,
                                        keep_checkpoint_max = 3)
    estimator = tf.estimator.DNNLinearCombinedRegressor(
        model_dir = output_dir,
        ...
        config = run_config)

    estimator = tf.contrib.estimator.add_metrics(estimator, my_rmse) train_spec = tf.estimator.TrainSpec(
        input_fn = read_dataset('train', tf.estimator.ModeKeys.TRAIN, BATCH_SIZE),
        max_steps = TRAIN_STEPS) exporter = tf.estimator.LatestExporter('exporter', serving_input_fn, exports_to_keep=None) eval_spec = tf.estimator.EvalSpec(
        input_fn = read_dataset('eval', tf.estimator.ModeKeys.EVAL, 2**15),  # no need to batch in eval
        steps = None,
        start_delay_secs = 60, # start evaluating after N seconds
        throttle_secs = EVAL_INTERVAL,  # evaluate every N seconds
        exporters = exporter) tf.estimator.train_and_evaluate(estimator, train_spec, eval_spec)
```

![](img/465b0563cf996f6944b237d3d4a9f0a3.png)

train_and_evaluate() is highly customizable

## 运行配置

[RunConfig](https://www.tensorflow.org/api_docs/python/tf/estimator/RunConfig) 允许您控制检查点被写出的频率。检查点是 Estimator 支持容错的方式。如果训练群集的主要(或主节点)失败，训练将从检查点恢复。检查次数越多，机器故障造成的损失就越少。当然，检查点本身会消耗 CPU 和存储，所以这是一个权衡。在我的示例中，我要求每 300 秒执行一次检查点操作(以限制 CPU 开销)，并且只保存最后 3 个检查点(以限制存储开销):

```
run_config = tf.estimator.RunConfig(save_checkpoints_secs = 300, 
                                    keep_checkpoint_max = 3)
```

您还可以根据训练步骤的数量来指定检查点间隔——最初，这看起来更简单，也更有吸引力。然而，如果您认识到检查点是关于故障恢复的，您将很快认识到按时间指定这是一个更好的选择。Estimator 足够聪明，不会编写检查点，除非训练工作实际上已经取得了进展。

请注意，run_config 是作为参数传递给估计器的:

```
estimator = tf.estimator.DNNLinearCombinedRegressor(
        model_dir = output_dir,
        ...
        config = run_config)
```

## 评估指标

默认情况下，预构建的估计器(如 LinearRegressor 和 DNNClassifier)会预先指定它们将评估的指标。例如，对于 LinearRegressor，这是 average_loss，即均方误差。一个常见的需求是评估其他指标。您可以通过 [add_metrics](https://www.tensorflow.org/api_docs/python/tf/contrib/estimator/add_metrics) 来实现:

```
estimator = tf.contrib.estimator.add_metrics(estimator, my_rmse)
```

这是一种常见的模式——扩展 Estimator 的方法是对其进行包装并添加额外的功能。在这种情况下，有一个贡献函数能够添加指标。my_rmse 是一个函数，它返回此类指标的字典:

```
def my_rmse(labels, predictions):
    pred_values = predictions['predictions']
    return {
      'rmse': tf.metrics.root_mean_squared_error(labels, pred_values)
    }
```

## 训练批量

梯度是一次对一批训练样本进行计算的。该批次的大小由训练输入函数控制:

```
train_spec = tf.estimator.TrainSpec(
  input_fn = read_dataset('train', ModeKeys.TRAIN, 40),
  max_steps = TRAIN_STEPS)
```

您的输入函数可能使用 TensorFlow 数据集 API，其中批处理只是对数据集的一个调用:

```
dataset = dataset.repeat(num_epochs).batch(batch_size)
```

但是，请注意，num_epochs 在分布式训练中的表现不同。训练时间由训练步数控制，而不是由重复输入数据控制。因此，将模式传递给输入函数是很重要的，因为您可能希望随机排列训练示例并无限期地读取训练数据:

```
if mode == tf.estimator.ModeKeys.TRAIN:
   num_epochs = None # indefinitely
   dataset = dataset.shuffle(buffer_size = NWORKERS * batch_size)
```

为什么洗牌？这是因为，在分布式训练中，每个工人在一批上计算梯度，然后梯度更新在所有工人中平均。显然，如果你的所有员工都在处理相同的数据，那就没有意义了。因此，您要求数据集打乱数据，以便每个工人看到不同的批次。

在评估期间，num_epochs 将为 1，但是您可以通过适当地设置评估步骤的数量来评估数据集的一部分(这将在下面介绍)。

## 火车步骤

TrainSpec 的第二个参数表示要训练多少步:

```
train_spec = tf.estimator.TrainSpec(
  input_fn = read_dataset('train', ModeKeys.TRAIN, BATCH_SIZE),
  max_steps = TRAIN_STEPS)
```

记住，训练步骤包括对一批训练样本进行梯度计算。 [TrainSpec](https://www.tensorflow.org/api_docs/python/tf/estimator/TrainSpec) 需要训练模型的最大步数。注意，这是 max_steps，而不是 steps。如果您有一个对应于步骤#1800 的检查点，并且您指定 max_steps=2000，那么训练将在 1800 恢复，并且只进行 200 步！恢复训练的能力是一个重要的属性，但是请确保您完全理解这意味着什么:要从头开始训练，您需要清除输出目录，以便没有预先存在的检查点。

还要注意，这是控制你训练时间的因素——它实际上覆盖了输入函数中的 num_epochs。

## 出口商

检查点不同于导出。检查点是关于故障恢复的，并且包括保存完整的训练状态(权重、全局步数等。).导出是关于创建服务签名。最常见的情况是，您将导出神经网络的预测头或输出节点，但有时，您也会想要导出嵌入节点。

在我的例子中，我在培训结束时导出:

```
exporter = tf.estimator.LatestExporter('exporter', serving_input_fn, exports_to_keep=None)
```

导出的模型将被写入名为“exporter”的目录，并且服务输入函数指定最终用户将被期望向预测服务提供什么。如果要保留与最后 5 个检查点对应的导出，可以指定 exports_to_keep=5。

## 评估批量

训练批次大小大约是计算梯度的样本数。然而，在评估期间，没有计算梯度。在评估期间，成批读取数据以避免过度分配内存缓冲区的唯一原因。因此，请指定一个更大的批量:

```
input_fn = read_dataset('eval', tf.estimator.ModeKeys.EVAL, 2**15)
```

训练批次大小约为 100，而评估批次大小可以设置为一个批次的总内存使用量约为 32MB，显然，确切的数量取决于训练数据在内存中占用的空间。

## 评估步骤

评估时，可以通过在 EvalSpec 中指定 steps=None 对整个数据集进行评估:

```
eval_spec = tf.estimator.EvalSpec(
        input_fn = ...,
        steps = None
```

但是，如果数据集很大，这可能会带来相当大的开销。在培训期间进行评估的唯一原因是为了监控，并且您希望监控在某种程度上是轻量级的。因此，一个好的折衷办法是指定足够大的步骤数，使评估在统计上是稳健的。这取决于您的数据，但 100 往往是一个常见的选择:

```
eval_spec = tf.estimator.EvalSpec(
        input_fn = ...,
        steps = 100
```

因为我们的 batch_size 是 2 * 15(或 32k)，所以 100 个步骤对应于大约 320 万个示例，我们的想法是对这个大数字进行评估已经足够稳定了。

## 评估油门

默认情况下，Estimator 将在每次检查点时进行评估。您可以在评估上指定一个限制，以便更频繁地进行检查点操作(用于故障恢复)，但减少评估次数(用于监控)。这样做的原因是检查点相对较快，但评估可能较慢，尤其是在评估整个数据集时:

```
eval_spec = tf.estimator.EvalSpec(
        input_fn = ...,
        steps = EVAL_STEPS,
        start_delay_secs = 60, # start evaluating after N seconds
        throttle_secs = 600,  # evaluate every N seconds
        exporters = exporter)
```

如您所见，train_and_evaluate 提供了一种强大的、可定制的方法来进行分布式培训。