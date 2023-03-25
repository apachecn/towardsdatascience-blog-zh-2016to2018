# 为什么我放弃了基于 Tensorflow 队列的数据输入管道

> 原文：<https://towardsdatascience.com/why-am-i-chucking-the-tensorflow-queue-based-data-input-pipeline-5b4427735a0d?source=collection_archive---------4----------------------->

![](img/760cffb733b05e490cb07ffbab4aa47b.png)

Post 假设对 Tensorflow 有一些基本的了解。主要是我在抱怨输入管道的问题😆

我最近在使用基于 Tensorflow 数据输入管道的 [**旧**](https://www.tensorflow.org/programmers_guide/threading_and_queues) 队列时遇到了困难。没什么大不了的，但从设计的角度来看，这似乎不太对。Tensorflow 团队确实意识到了其旧设计的问题，并提出了一个问题( [Issue #7951](https://github.com/tensorflow/tensorflow/issues/7951) )以进行彻底的设计更改(事实上，问题描述中说的是“从头开始”)。当我开始学习/使用输入管道时，新的设计还没有出现(尽管这个问题已经提出来了)。).鉴于我已经在学习和实验方面投入了一些时间，我想继续使用旧的设计。

我决定用在我的 [TrafficSignSense](https://bitbucket.org/hawk23/trafficsignsense/src) (嗯，现在报废了)项目中。我创建了一个抽象，因为基于队列的管道本身非常分散。我决定将它放在“创建-启动-清理”抽象中(供参考):

回到这个问题，train 程序看起来像这样:

当我需要定期验证我的模型时，问题就出现了。

```
eval_list = [train_optimizer, merged_summary, model_loss,              train_accuracy, valid_accuracy]        
for i in range(NUM_EPOCHS):            
    _, summary, loss, accuracy_t, accuracy_v = sess.run(eval_list)                       
    train_writer.add_summary(summary)
```

当我们试图评估`valid_accuracy`时，毫不奇怪你得到的是错误的结果。它基本上是随机选择权重。请注意，我正在重新创建模型(`model_v = model.CTrafficModel(batch_valid_input, NUM_CLASSES`)，这正是主要问题所在。我必须为验证数据重新创建模型，因为这是我使用输入管道输入数据的唯一方式。我“也许”可以用检查站来解决这个问题。但是，这似乎不对。我认为检查点是训练验证过程的输出，在推理时间或训练恢复的情况下使用。训练和验证是紧密耦合的，我不喜欢为了验证而每隔几个迭代就抛弃权重并重新加载的想法。我也不喜欢仅仅因为数据改变就重新创建模型的想法(假设模型保持不变)。

以上只是输入管道的旧设计的几个问题。归档的问题描述了许多其他相关的问题。我最终决定永远放弃它。

## 那么，接下来呢？

作为替代，Tensorflow 拥有`Dataset`和`Iterator`API，可以轻松创建输入管道(他们是这么说的)。我还没看完。Tensorflow 也有这些`Estimator`和`Experiment`高级抽象。我打算和`Dataset-Iterator`一起使用。希望他们让手头的任务变得容易！！！