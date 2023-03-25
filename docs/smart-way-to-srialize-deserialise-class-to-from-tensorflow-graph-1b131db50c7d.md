# 将类序列化/反序列化到 Tensorflow 图或从 tensor flow 图序列化/反序列化类的智能方法

> 原文：<https://towardsdatascience.com/smart-way-to-srialize-deserialise-class-to-from-tensorflow-graph-1b131db50c7d?source=collection_archive---------6----------------------->

![](img/b4ec07d753b603a8e96d66fba8f66632.png)

Photo by [John Fowler](https://unsplash.com/photos/RHaF3u3YjTM?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) on [Unsplash](https://unsplash.com/collections/2313048/astres?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

*自动将您的字段与张量流图绑定*

将类字段自动绑定到图中的 tensorflow 变量并恢复它们，而不用手动从图中获取每个变量，这是不是很酷？

*本文的代码可以在* [*这里找到，*](https://github.com/FrancescoSaverioZuppichini/TFGraphConvertible) *一个笔记本版本可以在* [*这里找到*](https://github.com/FrancescoSaverioZuppichini/TFGraphConvertible/blob/master/example.ipynb)

想象你有一个`Model`类

通常，你首先**建造**你的模型，然后**训练**它。之后，你想从保存的图中得到旧的变量，而不是从头开始重建整个模型。

```
<tf.Variable 'variable:0' shape=(1,) dtype=int32_ref>
```

现在，假设我们刚刚训练了我们的模型，我们想要存储它。通常的模式是

现在你想执行**推理**，也就是通过加载存储的图来取回你的东西。在我们的例子中，我们想要名为`variable`的变量

```
INFO:tensorflow:Restoring parameters from /tmp/model.ckpt
```

现在我们可以从图中找回我们的`variable`

```
name: "variable" op: "VariableV2" attr { key: "container" value { s: "" } } attr { key: "dtype" value { type: DT_INT32 } } attr { key: "shape" value { shape { dim { size: 1 } } } } attr { key: "shared_name" value { s: "" } }
```

但是，如果我们想再次使用我们的`model`类呢？如果我们现在试着打电话给`model.variable`,我们没有收到

```
None
```

一个解决方案是**重新构建**整个模型，然后恢复图形

```
INFO:tensorflow:Restoring parameters from /tmp/model.ckpt <tf.Variable 'variable:0' shape=(1,) dtype=int32_ref>
```

你已经可以看到这是浪费时间。我们可以通过以下方式将`model.variable`直接绑定到正确的图节点

```
name: "variable" op: "VariableV2" attr { key: "container" value { s: "" } } attr { key: "dtype" value { type: DT_INT32 } } attr { key: "shape" value { shape { dim { size: 1 } } } } attr { key: "shared_name" value { s: "" } }
```

现在想象一下，我们有一个非常大的嵌套变量模型。为了正确恢复模型中的每个变量指针，您需要:

*   命名每个变量
*   从图表中获取变量

如果我们可以自动检索在模型类中设置为字段的所有变量，会不会很酷？

# TFGraphConvertible

我创建了一个类，叫做`TFGraphConvertible`。你可以使用`TFGraphConvertible`来自动**序列化**和**反序列化**一个类。

让我们重新创建我们的模型

它公开了两个方法:`to_graph`和`from_graph`

# 序列化-到图形

为了使**序列化一个类**，你可以调用 **to_graph** 方法来创建一个字段名字典- > tensorflow 变量名。你需要传递一个`fields`参数，一个我们想要序列化的字段的字典。在我们的情况下，我们可以通过所有的测试。

```
{'variable': 'variable_2:0'}
```

它将创建一个字典，将所有字段作为键，将相应的 tensorflow 变量名作为值

# 反序列化-从图形

为了**反序列化一个类**，您可以调用 **from_graph** 方法，该方法获取之前创建的字典并将每个类字段绑定到正确的 tensorflow 变量

```
None <tf.Tensor 'variable_2:0' shape=(1,) dtype=int32_ref>
```

现在你有你的`model`回来了！

# 完整示例

我们来看一个更有趣的例子！我们将为 MNIST 数据集训练/恢复一个模型

我们去拿数据集吧！

```
Using TensorFlow backend.
```

现在是时候训练它了

```
0.125
0.46875
0.8125
0.953125
0.828125
0.890625
0.796875
0.9375
0.953125
0.921875
```

完美！让我们将序列化模型存储在内存中

```
{'x': 'ExpandDims:0', 
'y': 'one_hot:0', 
'forward_raw': 'dense_1/BiasAdd:0', 
'accuracy': 'Mean:0', 
'loss': 'Mean_1:0', 
'train_step': 'Adam'}
```

然后我们重置图表并重新创建模型

```
INFO:tensorflow:Restoring parameters from /tmp/model.ckpt
```

当然，我们的变量在`mnist_model`不存在

```
--------------------------------------------------------------------------- AttributeError Traceback (most recent call last) <ipython-input-21-9def5e0d8f6c> in <module>() ----> 1 mnist_model.accuracy AttributeError: 'MNISTModel' object has no attribute 'accuracy'
```

让我们通过调用`from_graph`方法来重新创建它们。

```
<tf.Tensor 'Mean:0' shape=() dtype=float32>
```

现在`mnist_model`已经准备好了，让我们看看测试集的准确性

```
INFO:tensorflow:Restoring parameters from /tmp/model.ckpt 
1.0
```

# 结论

在本教程中，我们已经看到了如何序列化一个类并将每个字段绑定回张量流图中正确的张量。请注意，您可以将`serialized_model`存储为`.json`格式，并从任何地方直接加载。这样，您可以通过使用面向对象编程直接创建您的模型，并检索其中的所有变量，而不必重新构建它们。

感谢您的阅读

弗朗西斯科·萨维里奥·祖皮奇尼

*原载于*[*gist.github.com*](https://gist.github.com/002b66e4b384fe31bd005a00845f5fa3)*。*