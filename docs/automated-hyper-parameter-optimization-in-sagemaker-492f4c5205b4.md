# SageMaker 中的自动化超参数优化

> 原文：<https://towardsdatascience.com/automated-hyper-parameter-optimization-in-sagemaker-492f4c5205b4?source=collection_archive---------9----------------------->

![](img/02f600ed4014f9d331c46ed2168a6cf1.png)

*注:代码示例在* [*我的博客*](https://blog.zakjost.com/post/sagemaker-hpo/) *上呈现比在介质上好得多。*

# 什么是超参数优化(HPO)？

因此，您已经构建了您的模型，并获得了合理的结果，现在准备尽可能多地挤出性能。一种可能是进行[网格搜索](https://en.wikipedia.org/wiki/Hyperparameter_optimization#Grid_search)，在那里你尝试超参数的每一种可能的组合，并选择最好的一个。如果您的选择数量相对较少，这种方法很有效，但是如果您有大量的超参数，并且有些是可能跨越几个数量级的连续值，那该怎么办呢？[随机搜索](https://en.wikipedia.org/wiki/Random_search)可以很好地探索参数空间，而无需致力于探索*所有的参数空间*，但是在黑暗中随机探索是我们能做的最好的事情吗？

当然不是。[贝叶斯优化](https://docs.aws.amazon.com/sagemaker/latest/dg/automatic-model-tuning-how-it-works.html)是一种在进行顺序决策时优化功能的技术。在这种情况下，我们试图通过选择超参数值来最大化性能。这种顺序决策框架意味着，您为下一步选择的超参数将受到所有先前尝试的性能的影响。贝叶斯优化做出关于如何平衡探索参数空间的新区域和利用已知表现良好的区域的原则性决定。这就是说，使用贝叶斯优化通常比网格搜索和随机搜索更有效。

# 怎么做

好消息是 SageMaker 使这变得非常容易，因为该平台负责以下事项:

1.  实施贝叶斯优化算法来处理分类、整数和浮点超参数
2.  在给定一组来自 HPO 服务的超参数的情况下，协调模型的训练和评估
3.  整合培训作业和 HPO 服务，后者会传达选定的超参数值，并在作业完成后报告绩效

# 先决条件

下面的代码将假设我们正在使用 TensorFlow 估计器模型，但是与 HPO 相关的部分应该扩展到任何 [SageMaker 估计器](https://sagemaker.readthedocs.io/en/latest/estimators.html)。要以本示例所示的方式运行代码，您需要以下内容:

*   对 SageMaker 的工作原理有所了解。如果你想要一些这样的例子，在[这个回购](https://github.com/awslabs/amazon-sagemaker-examples)中有几个官方笔记本例子。你可能会发现 [TensorFlow HPO 的例子](https://github.com/awslabs/amazon-sagemaker-examples/tree/master/hyperparameter_tuning/tensorflow_mnist)特别相关。
*   有 SageMaker 的 Python [SDK](https://github.com/aws/sagemaker-python-sdk)
*   已经配置了必要的 API 权限，或者正在 SageMaker 笔记本实例中运行

# 步骤 1-创建评估者

使用 SageMaker 运行 HPO 的一个关键要求是，您的模型需要:

1.  预计超参数将从 SageMaker 传递
2.  将性能指标写入日志

对于内置算法，这已经为您完成了。在使用 SageMaker 构建任意张量流模型的情况下，这意味着在`model.py`文件中正确配置东西，也称为“入口点”。这是 SageMaker 用来构建 TensorFlow 模型的文件，它希望定义符合特定输入/输出模式的某些函数。(有关您需要指定的功能的更多详细信息，请参见 [TensorFlow 自述文件](https://github.com/aws/sagemaker-python-sdk/blob/master/src/sagemaker/tensorflow/README.rst)。)

# 让您的模型准备好接受 SageMaker 的超参数

为了动态地指定参数值，您的模型代码需要接受、解析和利用它们。在 TensorFlow 中，您允许 SageMaker 通过向需要在入口点文件中指定的函数添加`hyperparameters`参数来指定超参数。例如，对于`model_fn`中需要的超参数:

```
DEFAULT_LEARNING_RATE = 1e-3
def model_fn(features, labels, mode, hyperparameters=None):
    if hyperparameters is None:
        hyperparameters = dict()
    # Extract parameters
    learning_rate = hyperparameters.get('learning_rate', DEFAULT_LEARNING_RATE)
    ...
```

您可能还希望在`train_input_fn`中有一个超参数，例如指定训练时期的数量:

```
def train_input_fn(training_dir, hyperparameters=None):
    # Extract parameters
    if not hyperparameters:
        hyperparameters = dict() epochs = hyperparameters.get('epochs', None)
    ...
```

如果指定了参数，这些示例提取参数，如果没有指定，则使用默认值。

# 将性能指标写入日志

将性能指标写入日志的第二个要求是 SageMaker 的一个实现细节:它通过从训练日志文本中提取模型性能来获得运行的模型性能。这些是发送回 HPO 引擎的值。

对于 TensorFlow，默认情况下，`EstimatorSpec`中指定的指标会写入日志。例如，这段代码是我的`model_fn`的一部分:

```
def model_fn(features, labels, model, hyperparameters=None)
    ...
    if mode == tf.estimator.ModeKeys.EVAL:
        eval_metric_ops = {
            "roc_auc": tf.metrics.auc(
                labels, 
                predictions,
                summation_method='careful_interpolation'),
            "pr_auc": tf.metrics.auc(
                labels, 
                predictions, 
                summation_method='careful_interpolation', 
                curve='PR'),
        }
    else:
        # e.g. in "training" mode
        eval_metric_ops = {} return tf.estimator.EstimatorSpec(
        mode=mode,
        loss=loss,
        train_op=train_op,
        eval_metric_ops=eval_metric_ops,
    )
```

在训练期间，模型将定期停止并评估测试集(这个过程的细节可以由您配置)。这些事件的日志如下所示:

```
*2018–10–02 17:23:40,657 INFO — tensorflow — Saving dict for global step 101: global_step = 101, loss = 0.45420808, pr_auc = 0.36799875, roc_auc = 0.6891242*
```

这是 SageMaker 将用来衡量任何特定培训工作的表现。

# 构建评估器

评估员通常用于开始一项培训工作。这使您能够告诉 SageMaker 在哪里存储输出，哪些实例用于训练…等等。既然入口点文件中的函数已经正确配置为接受超参数并将性能指标写入日志，您就可以创建 TensorFlow 估计器了:

```
from sagemaker.tensorflow import TensorFlow# The parameters that are constant and will not be tuned
shared_hyperparameters = {
    'number_layers': 5,
}tf_estimator = TensorFlow(
    entry_point='my/tensorflow/model.py',
    role='<sagemaker_role_arn>',
    train_instance_count=1,
    train_instance_type='ml.p3.2xlarge',
    training_steps=10000,
    hyperparameters=shared_hyperparameters,
)
```

# 步骤 2 —定义绩效指标

在这一步，我们需要告诉 SageMaker *如何*从日志中提取性能信息。这是通过指定一个[正则表达式](https://regexr.com/)并将其分配给一个度量名称来实现的。尽管您可以指定多个表达式(AWS CloudWatch 会自动收集这些表达式以便于绘图/监控)，但需要挑出其中一个作为 HPO 的优化目标。您还需要指定是最大化还是最小化数量。注意，虽然正则表达式可能匹配多个日志条目，但是日志中的最后一个实例作为最终性能值返回。

```
objective_metric_name = 'PR-AUC'
objective_type = 'Maximize'
metric_definitions = [
    {'Name': 'ROC-AUC', 'Regex': 'roc_auc = ([0-9\\.]+)'},
    {'Name': 'PR-AUC', 'Regex': 'pr_auc = ([0-9\\.]+)'}
]
```

# 步骤 3-定义超参数搜索空间

我们现在需要指定我们的超参数被称为什么，它们是什么类型(连续、整数或分类)，以及它们的可能值是什么。下面是一个例子:

```
from sagemaker.tuner import (
    IntegerParameter, CategoricalParameter, 
    ContinuousParameter, HyperparameterTuner)hyperparameter_ranges = {
    "learning_rate": ContinuousParameter(1e-5, 1e-1),
    "number_nodes": IntegerParameter(32, 512),
    "optimizer": CategoricalParameter(['Adam', 'SGD'])
}
```

# 步骤 4-指定优化迭代的次数

最后，我们需要决定如何运行 HPO 作业。如果您并行运行许多作业，那么您可以同时探索大部分空间。然而，如果您只是并行运行一百万个作业，那么您实际上是在进行随机搜索。贝叶斯优化的*顺序*本质允许之前运行的结果通知未来的运行。

因此，我们需要决定总共运行多少个作业，以及在任何给定时间并行运行多少个作业。例如，我们可能运行 100 个作业，并行运行 5 个。总共有 20 次连续迭代，每次探索 5 个点。这里的选择将取决于参数空间的大小和预算。

现在你有了要求 SageMaker 管理 HPO 所需的一切:

```
tuner = HyperparameterTuner(
    tf_estimator,
    objective_metric_name,
    hyperparameter_ranges,
    metric_definitions,
    max_jobs=100,
    max_parallel_jobs=5,
    objective_type=objective_type
)# The data configuration
channel = {
    'training': 's3://<bucket_name>/my/training_file.csv',
    'test': 's3://<bucket_name>/my/test_file.csv',
}tuner.fit(inputs=channel)
```

# 最终性能

您可以使用 [sdk](https://sagemaker.readthedocs.io/en/latest/tuner.html) 来 pinging SageMaker 以获得状态报告，或者从 HPO 运行中获得最佳作业的统计数据。您还可以从 AWS SageMaker 控制台完成这项工作，它很好地显示了所有作业的性能摘要以及 HPO 作业配置。

如上所述，你可以在控制台中进入 CloudWatch，点击`Browse Metrics`，找到你在第二步`metric_definitions`的`Name`字段中定义的指标。

完成所有工作后，您只需发出以下命令即可部署最佳模型:

```
tuner.deploy(
    initial_instance_count=1,
    instance_type='ml.p3.2xlarge'
)
```

*你可以在【blog.zakjost.com】[](http://blog.zakjost.com)*找到我所有的内容并订阅**