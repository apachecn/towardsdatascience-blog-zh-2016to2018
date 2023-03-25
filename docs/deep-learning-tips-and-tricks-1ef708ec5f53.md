# 深度学习技巧和诀窍

> 原文：<https://towardsdatascience.com/deep-learning-tips-and-tricks-1ef708ec5f53?source=collection_archive---------3----------------------->

以下是我与同龄人和学生就如何优化深度模型进行的对话、信息和辩论的精华集合。如果你有有效的方法，请分享出来！！

# 首先，为什么要调整模型？

像卷积神经网络(CNN)这样的深度学习模型有大量的参数；我们实际上可以称之为超参数，因为它们在模型中没有被优化。你可以搜索这些超参数的最佳值，但是你需要大量的硬件和时间。那么，一个真正的数据科学家会满足于猜测这些基本参数吗？

改进您的模型的最好方法之一是建立在对您的领域进行了深入研究的专家的设计和架构之上，这些专家通常拥有强大的硬件。优雅地，他们经常开源最终的建模架构和基本原理。

# 深度学习技术

这里有一些方法可以帮助你通过预先训练的模型来提高你的健身时间和准确性:

1.  ***研究理想的预训架构:*** 了解迁移学习的[好处](https://www.analyticsvidhya.com/blog/2017/06/transfer-learning-the-art-of-fine-tuning-a-pre-trained-model/)，或者浏览一些[强大的 CNN 架构](http://www.vlfeat.org/matconvnet/pretrained/)。考虑那些看起来不明显的领域，但是共享潜在的潜在特征。
2.  ***使用较小的学习率:*** 由于预训练的权重通常比随机初始化的权重好，所以修改要更细腻！你在这里的选择取决于学习环境和预训练的进展情况，但是检查跨时代的错误，以了解你离收敛有多近。
3.  ***使用退出:*** 与回归模型的脊和套索正则化一样，所有模型都没有优化的 *alpha* 或*退出*。这是一个超参数，取决于您的具体问题，必须进行测试。从更大的变化开始——像`np.logspace()`可以提供的数量级的更宽的网格搜索跨度——然后像上面的学习率一样下降。
4.  ***限制权重大小:*** 我们可以限制某些层的权重的最大范数(绝对值)，以便推广我们的模型
5.  ***不要碰第一层:*** 神经网络的第一个隐藏层往往会捕捉通用的和可解释的特征，如形状、曲线或跨领域的相互作用。我们应该经常不去管这些，而把重点放在优化更远的元潜在水平上。这可能意味着添加隐藏层，这样我们就不会匆忙的过程！
6.  ***修改输出层:*** 用一个新的激活函数和适合你的域的输出大小替换模型默认值。但是，不要把自己局限在最明显的解决方案上。虽然 MNIST 可能看起来想要 10 个输出类，但一些数字有常见的变化，允许 12-16 个类可以更好地解决这些变化并提高模型性能！正如上面的提示一样，随着我们接近输出，深度学习模型应该越来越多地修改和定制。

# 喀拉斯的技术

以下是如何在 MNIST 的 Keras 中修改辍学和限制体重的方法:

```
# dropout in input and hidden layers
# weight constraint imposed on hidden layers
# ensures the max norm of the weights does not exceed 5model = Sequential()model.add(Dropout(0.2, input_shape=(784,))) # dropout on the inputs
# this helps mimic noise or missing datamodel.add(Dense(128, input_dim=784, kernel_initializer='normal', activation='relu', kernel_constraint=maxnorm(5)))model.add(Dropout(0.5))model.add(Dense(128, kernel_initializer='normal', activation='tanh', kernel_constraint=maxnorm(5)))model.add(Dropout(0.5))model.add(Dense(1, kernel_initializer='normal', activation='sigmoid'))
```

## 辍学最佳实践:

*   使用 20–50%的小压降，建议输入[为 20%。太低，你的影响可以忽略不计；太高，你吃不饱。](https://chrisalbon.com/deep_learning/keras/adding_dropout/)
*   在输入图层和隐藏图层上使用 dropout。这已经被证明可以提高深度学习的性能。
*   使用具有衰减和大动量的大学习率。
*   约束你的体重！大的学习率会导致爆炸梯度。对网络权重施加约束条件(例如大小为 5 的最大范数正则化)已被证明可以改善结果。
*   使用更大的网络。当在更大的网络上使用 dropout 时，您可能会获得更好的性能，从而为模型提供更多学习独立表示的机会。

以下是一个在 Keras 中对 MNIST 的 14 个类进行最终图层修改的示例:

```
**from** keras.layers.core **import** Activation, Densemodel**.**layers**.**pop() # defaults to last
model**.**outputs **=** [model**.**layers[**-**1]**.**output]
model**.**layers[**-**1]**.**outbound_nodes **=** []
model**.**add(Dense(14, activation**=**'softmax')) 
```

以及如何在前五层冻结权重的示例:

```
**for** layer **in** model**.**layers[:5]:
    layer**.**trainable **=** False
```

或者，我们可以将该层的学习速率设置为零，或者使用每个参数的自适应学习算法，如 [Adadelta 或 Adam](https://keras.io/optimizers/) 。这有点复杂，最好在其他平台上实现，比如 Caffe。

# 预训练网络的图库:

## **Keras**

*   [卡格尔列表](https://www.kaggle.com/gaborfodor/keras-pretrained-models)
*   [Keras 应用](https://keras.io/applications/)
*   [OpenCV 示例](https://www.learnopencv.com/keras-tutorial-fine-tuning-using-pre-trained-models/)

## **张量流**

*   [VGG16](https://github.com/ry/tensorflow-vgg16)
*   [盗梦空间 V3](https://github.com/tensorflow/models/blob/master/inception/README.md#how-to-fine-tune-a-pre-trained-model-on-a-new-task)
*   [ResNet](https://github.com/ry/tensorflow-resnet)

## **火炬**

*   [LoadCaffe](https://github.com/szagoruyko/loadcaffe)

## **咖啡馆**

*   [模型动物园](https://github.com/BVLC/caffe/wiki/Model-Zoo)

# [在 Jupyter 中查看你的张量图](http://nbviewer.jupyter.org/github/tensorflow/tensorflow/blob/master/tensorflow/examples/tutorials/deepdream/deepdream.ipynb)

获得模型外观的视觉概念通常是很重要的。如果你在 Keras 中工作，抽象是很好的，但是不允许你深入模型的各个部分进行更深入的分析。幸运的是，下面的代码让我们可以直接用 Python 可视化我们的模型:

```
# From: [http://nbviewer.jupyter.org/github/tensorflow/tensorflow/blob/master/tensorflow/examples/tutorials/deepdream/deepdream.ipynb](http://nbviewer.jupyter.org/github/tensorflow/tensorflow/blob/master/tensorflow/examples/tutorials/deepdream/deepdream.ipynb)
# Helper functions for TF Graph visualization
from IPython.display import clear_output, Image, display, HTML
def strip_consts(graph_def, max_const_size=32):
    """Strip large constant values from graph_def."""
    strip_def = tf.GraphDef()
    for n0 in graph_def.node:
        n = strip_def.node.add() 
        n.MergeFrom(n0)
        if n.op == 'Const':
            tensor = n.attr['value'].tensor
            size = len(tensor.tensor_content)
            if size > max_const_size:
                tensor.tensor_content = bytes("<stripped %d bytes>"%size, 'utf-8')
    return strip_def

def rename_nodes(graph_def, rename_func):
    res_def = tf.GraphDef()
    for n0 in graph_def.node:
        n = res_def.node.add() 
        n.MergeFrom(n0)
        n.name = rename_func(n.name)
        for i, s in enumerate(n.input):
            n.input[i] = rename_func(s) if s[0]!='^' else '^'+rename_func(s[1:])
    return res_def

def show_graph(graph_def, max_const_size=32):
    """Visualize TensorFlow graph."""
    if hasattr(graph_def, 'as_graph_def'):
        graph_def = graph_def.as_graph_def()
    strip_def = strip_consts(graph_def, max_const_size=max_const_size)
    code = """
        <script>
          function load() {{
            document.getElementById("{id}").pbtxt = {data};
          }}
        </script>
        <link rel="import" href="https://tensorboard.appspot.com/tf-graph-basic.build.html" onload=load()>
        <div style="height:600px">
          <tf-graph-basic id="{id}"></tf-graph-basic>
        </div>
    """.format(data=repr(str(strip_def)), id='graph'+str(np.random.rand()))

    iframe = """
        <iframe seamless style="width:800px;height:620px;border:0" srcdoc="{}"></iframe>
    """.format(code.replace('"', '&quot;'))
    display(HTML(iframe))# Visualizing the network graph. Be sure expand the "mixed" nodes to see their 
# internal structure. We are going to visualize "Conv2D" nodes.
graph_def = tf.get_default_graph().as_graph_def()
tmp_def = rename_nodes(graph_def, lambda s:"/".join(s.split('_',1)))
show_graph(tmp_def)
```

# 用 Keras 可视化你的模型

这将[绘制出模型](https://keras.io/visualization/)的图形，并将其保存为 png 文件:

```
**from** keras.utils **import** plot_model
plot_model(model, to_file='model.png')
```

`plot`采用两个可选参数:

*   `show_shapes`(默认为假)控制是否在图形中显示输出形状。
*   `show_layer_names`(默认为真)控制图层名称是否显示在图形中。

您也可以直接获得`pydot.Graph`对象并自己渲染它，例如在 ipython 笔记本中显示它:

```
**from** IPython.display **import** SVG
**from** keras.utils.visualize_util **import** model_to_dotSVG(model_to_dot(model).create(prog='dot', format='svg'))
```

希望这个合集对你的机器学习项目有所帮助！请在下面的评论中告诉我你是如何优化你的深度学习模型的，并在 [Twitter](https://twitter.com/ultimetis) 和 [LinkedIn](https://www.linkedin.com/in/jbalaban/) 上与我联系！