# TensorFlow 服务客户。让它更瘦更快！

> 原文：<https://towardsdatascience.com/tensorflow-serving-client-make-it-slimmer-and-faster-b3e5f71208fb?source=collection_archive---------7----------------------->

![](img/be89cea52cb769775adda9ebaa5fa185.png)

[TensorFlow Serving](https://www.tensorflow.org/serving/) 提供了一种在生产中部署和服务模型的简洁方式。我之前已经描述过部署过程[这里](/how-to-deploy-machine-learning-models-with-tensorflow-part-3-into-the-cloud-7115ff774bb6)。不幸的是，有两个问题我很久以后才注意到，感谢宝贵的评论。首先，单次预测花费太多时间。第二，实际上，没有必要在客户端使用 TensorFlow。

瓶颈是对 TensorFlow 的调用，它创建了一个张量 protobuf(完整的代码可以在[这里](https://github.com/Vetal1977/tf_serving_example/blob/master/svnh_semi_supervised_client.py)找到):

```
tf.contrib.util.make_tensor_proto(data, shape=[1])
```

所有其他事情都不依赖于 TensorFlow，仅仅需要从 protobufs 生成 Python 文件。

# 解决方案

我介绍了一个专门针对图像预测的解决方案，而不改变原始的 protobufs。我发现了一篇很棒的博文,作者走得更远，复制并修改了原始的 protobufs。这允许将代码减少到最少，如果我只需要预测的话，我肯定会这么做。

## 从 protobufs 生成 Python 代码

我们必须创建一个在 TensorFlow protobuf 文件中描述的具有特定类型和形状的 *TensorProto* 对象。我们可以按原样使用我们的图像数据数组。

更具体地说，这意味着我们必须从 TensorFlow 核心 protobufs 生成 Python 文件，并直接使用它们，而不是任何包装器。我已经生成了它们，并放入我的库[这里](https://github.com/Vetal1977/tf_serving_example/tree/master/tensorflow/core)。如果您愿意，您可以自己动手(假设您克隆了示例项目):

```
# 1
cd <tensorflow serving source folder># 2
python -m grpc.tools.protoc ./tensorflow/tensorflow/core/framework/*.proto --python_out=<path to the project> --grpc_python_out=<path to the project> --proto_path=.# 3
python -m grpc.tools.protoc ./tensorflow/tensorflow/core/example/*.proto --python_out=<path to the project> --grpc_python_out=<path to the project> --proto_path=.# 4
python -m grpc.tools.protoc ./tensorflow/tensorflow/core/protobuf/*.proto --python_out=<path to the project> --grpc_python_out=<path to the project> --proto_path=.
```

不幸的是，由于依赖关系，您需要所有这些，或者您必须调整 protobufs 以满足您的需要，并消除不需要的依赖关系。

## 替换张量流代码

现在我们可以替换一个“标准的”张量原型创造

```
tf.contrib.util.make_tensor_proto(data, shape=[1])
```

包含以下内容:

```
dims = [tensor_shape_pb2.TensorShapeProto.Dim(size=1)]
tensor_shape_proto = tensor_shape_pb2.TensorShapeProto(dim=dims)
tensor_proto = tensor_pb2.TensorProto(
    dtype=types_pb2.DT_STRING,
    tensor_shape=tensor_shape_proto,
    string_val=[data])
```

这是怎么回事？首先，我们创建一个与我们的数据相匹配的 dimension 对象——我们只有一个图像要预测。接下来，我们使用创建的维度对象初始化适当的张量形状。最后，我们用张量形状和我们的数据创建所需的类型为 *string* 的张量 protobuf。我们必须将数据放入*字符串值*中，因为我们有一个*字符串*的张量。你可以在生成的*tensor flow/core/framework/types _ Pb2 . py*文件中找到其他可用的类型。

现在，我们可以将张量 protobuf 设置为请求输入:

```
request.inputs['images'].CopyFrom(tensor_proto)
```

你可以在这里找到完整的代码[。](https://github.com/Vetal1977/tf_serving_example/blob/master/svnh_semi_supervised_client_no_tf.py)

## 表演

你也可以显著提高表现。如果您喜欢在客户端继续使用 TensorFlow 框架，请进行以下更改:在开始时导入 *make_tensor_proto* ，稍后调用它。

```
...
from tensorflow.contrib.util import make_tensor_proto
...request.inputs['images'].CopyFrom(make_tensor_proto(data, shape=[1]))
```

如果您不再在客户端使用 TensorFlow，性能会自动提高。在我的系统上，现在单个图像预测需要 3 毫秒，而不是 300 毫秒。

# 摘要

TensorFlow 服务客户端不需要 TensorFlow 框架来发出请求。您唯一需要做的就是正确初始化发送给服务器的 gRPC 请求。您可以使用原始 TensorFlow Core protobufs 和以下 Python 文件并创建适当的 tensor protobuf 对象来实现这一点。如果您希望客户端中的 TensorFlow 尽可能少，您需要调整原始 protobufs 以满足您的特定需求。