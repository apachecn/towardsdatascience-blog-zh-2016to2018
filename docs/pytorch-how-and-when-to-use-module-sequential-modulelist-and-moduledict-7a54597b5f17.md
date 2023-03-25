# Pytorch:如何以及何时使用模块、顺序、模块列表和模块指令

> 原文：<https://towardsdatascience.com/pytorch-how-and-when-to-use-module-sequential-modulelist-and-moduledict-7a54597b5f17?source=collection_archive---------3----------------------->

![](img/f4cb6bdd9cde347a31e6e675fd3b4b28.png)

Photo by [Markus Spiske](https://unsplash.com/photos/C0koz3G1I4I?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) on [Unsplash](https://unsplash.com/search/photos/lego?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

*嘿，我在*[*LinkedIn*](https://www.linkedin.com/in/francesco-saverio-zuppichini-94659a150/)*过来打个招呼👋*

*在 Pytorch 1.7 更新*

你可以在这里找到代码

[Pytorch](https://pytorch.org/) 是一个开源的深度学习框架，提供了一种创建 ML 模型的智能方式。即使文档做得很好，我仍然发现大多数人仍然能够写出糟糕的、没有条理的 PyTorch 代码。

今天我们来看看 PyTorch 的三个主要构建模块是如何使用的:`Module, Sequential and ModuleList`。我们将从一个例子开始，并不断改进。

这四个类都包含在`torch.nn`中

# 模块:主要构建模块

[模块](https://pytorch.org/docs/stable/nn.html?highlight=module)是主要的构建模块，它定义了所有神经网络的基类，您必须对其进行子类化。

让我们创建一个经典的 CNN 分类器作为示例:

```
MyCNNClassifier( (conv1): Conv2d(1, 32, kernel_size=(3, 3), stride=(1, 1), padding=(1, 1)) (bn1): BatchNorm2d(32, eps=1e-05, momentum=0.1, affine=True, track_running_stats=True) (conv2): Conv2d(32, 64, kernel_size=(3, 3), stride=(1, 1), padding=(1, 1)) (bn2): BatchNorm2d(32, eps=1e-05, momentum=0.1, affine=True, track_running_stats=True) (fc1): Linear(in_features=25088, out_features=1024, bias=True) (fc2): Linear(in_features=1024, out_features=10, bias=True) )
```

这是一个非常简单的分类器，编码部分使用两层 3x3 convs + batchnorm + relu，解码部分使用两个线性层。如果您不熟悉 PyTorch，您可能以前见过这种类型的编码，但是有两个问题。

如果我们想增加一层，我们必须再次在`__init__`和`forward`函数中写很多代码。此外，如果我们有一些想要在另一个模型中使用的公共块，例如 3x3 conv + batchnorm + relu，我们必须重新编写它。

# 连续:堆叠和合并层

[顺序](https://pytorch.org/docs/stable/nn.html?highlight=sequential#torch.nn.Sequential)是模块的容器，可以堆叠在一起，同时运行。

你可以注意到我们已经把所有东西都储存到`self`里了。我们可以使用`Sequential`来改进我们的代码。

```
MyCNNClassifier( (conv_block1): Sequential( (0): Conv2d(1, 32, kernel_size=(3, 3), stride=(1, 1), padding=(1, 1)) (1): BatchNorm2d(32, eps=1e-05, momentum=0.1, affine=True, track_running_stats=True) (2): ReLU() ) (conv_block2): Sequential( (0): Conv2d(32, 64, kernel_size=(3, 3), stride=(1, 1), padding=(1, 1)) (1): BatchNorm2d(64, eps=1e-05, momentum=0.1, affine=True, track_running_stats=True) (2): ReLU() ) (decoder): Sequential( (0): Linear(in_features=25088, out_features=1024, bias=True) (1): Sigmoid() (2): Linear(in_features=1024, out_features=10, bias=True) ) )
```

好多了 uhu？

你有没有注意到`conv_block1`和`conv_block2`看起来几乎一样？我们可以创建一个重述`nn.Sequential`的函数来简化代码！

然后我们可以在模块中调用这个函数

```
MyCNNClassifier( (conv_block1): Sequential( (0): Conv2d(1, 32, kernel_size=(3, 3), stride=(1, 1), padding=(1, 1)) (1): BatchNorm2d(32, eps=1e-05, momentum=0.1, affine=True, track_running_stats=True) (2): ReLU() ) (conv_block2): Sequential( (0): Conv2d(32, 64, kernel_size=(3, 3), stride=(1, 1), padding=(1, 1)) (1): BatchNorm2d(64, eps=1e-05, momentum=0.1, affine=True, track_running_stats=True) (2): ReLU() ) (decoder): Sequential( (0): Linear(in_features=25088, out_features=1024, bias=True) (1): Sigmoid() (2): Linear(in_features=1024, out_features=10, bias=True) ) )
```

更干净！还是`conv_block1`和`conv_block2`差不多！我们可以用`nn.Sequential`合并它们

```
MyCNNClassifier( (encoder): Sequential( (0): Sequential( (0): Conv2d(1, 32, kernel_size=(3, 3), stride=(1, 1), padding=(1, 1)) (1): BatchNorm2d(32, eps=1e-05, momentum=0.1, affine=True, track_running_stats=True) (2): ReLU() ) (1): Sequential( (0): Conv2d(32, 64, kernel_size=(3, 3), stride=(1, 1), padding=(1, 1)) (1): BatchNorm2d(64, eps=1e-05, momentum=0.1, affine=True, track_running_stats=True) (2): ReLU() ) ) (decoder): Sequential( (0): Linear(in_features=25088, out_features=1024, bias=True) (1): Sigmoid() (2): Linear(in_features=1024, out_features=10, bias=True) ) )
```

`self.encoder`现占据展位`conv_block`。我们已经为我们的模型解耦了逻辑，使它更容易阅读和重用。我们的`conv_block`功能可以导入到另一个模型中使用。

# 动态连续:一次创建多个层

如果我们可以在`self.encoder`中添加一个新的层，硬编码会不方便:

如果我们可以将大小定义为一个数组并自动创建所有的层，而不需要写入每一层，这是不是很好？幸运的是，我们可以创建一个数组并将其传递给`Sequential`

```
MyCNNClassifier( (encoder): Sequential( (0): Sequential( (0): Conv2d(1, 32, kernel_size=(3, 3), stride=(1, 1), padding=(1, 1)) (1): BatchNorm2d(32, eps=1e-05, momentum=0.1, affine=True, track_running_stats=True) (2): ReLU() ) (1): Sequential( (0): Conv2d(32, 64, kernel_size=(3, 3), stride=(1, 1), padding=(1, 1)) (1): BatchNorm2d(64, eps=1e-05, momentum=0.1, affine=True, track_running_stats=True) (2): ReLU() ) ) (decoder): Sequential( (0): Linear(in_features=25088, out_features=1024, bias=True) (1): Sigmoid() (2): Linear(in_features=1024, out_features=10, bias=True) ) )
```

我们来分解一下。我们创建了一个数组`self.enc_sizes`来保存编码器的大小。然后我们通过迭代大小创建一个数组`conv_blocks`。因为我们必须给 booth 一个尺寸，并给每一层一个特大号，所以我们通过将数组移动一位来实现数组本身。

为了清楚起见，看一下下面的例子:

```
1 32 32 64
```

然后，由于`Sequential`不接受列表，我们通过使用`*`操作符来分解它。

Tada！现在，如果我们只想添加一个尺寸，我们可以很容易地在列表中添加一个新的数字。将大小作为参数是一种常见的做法。

```
MyCNNClassifier( (encoder): Sequential( (0): Sequential( (0): Conv2d(1, 32, kernel_size=(3, 3), stride=(1, 1), padding=(1, 1)) (1): BatchNorm2d(32, eps=1e-05, momentum=0.1, affine=True, track_running_stats=True) (2): ReLU() ) (1): Sequential( (0): Conv2d(32, 64, kernel_size=(3, 3), stride=(1, 1), padding=(1, 1)) (1): BatchNorm2d(64, eps=1e-05, momentum=0.1, affine=True, track_running_stats=True) (2): ReLU() ) (2): Sequential( (0): Conv2d(64, 128, kernel_size=(3, 3), stride=(1, 1), padding=(1, 1)) (1): BatchNorm2d(128, eps=1e-05, momentum=0.1, affine=True, track_running_stats=True) (2): ReLU() ) ) (decoder): Sequential( (0): Linear(in_features=25088, out_features=1024, bias=True) (1): Sigmoid() (2): Linear(in_features=1024, out_features=10, bias=True) ) )
```

我们可以对解码器部分做同样的事情

```
MyCNNClassifier( (encoder): Sequential( (0): Sequential( (0): Conv2d(1, 32, kernel_size=(3, 3), stride=(1, 1), padding=(1, 1)) (1): BatchNorm2d(32, eps=1e-05, momentum=0.1, affine=True, track_running_stats=True) (2): ReLU() ) (1): Sequential( (0): Conv2d(32, 64, kernel_size=(3, 3), stride=(1, 1), padding=(1, 1)) (1): BatchNorm2d(64, eps=1e-05, momentum=0.1, affine=True, track_running_stats=True) (2): ReLU() ) ) (decoder): Sequential( (0): Sequential( (0): Linear(in_features=25088, out_features=1024, bias=True) (1): Sigmoid() ) (1): Sequential( (0): Linear(in_features=1024, out_features=512, bias=True) (1): Sigmoid() ) ) (last): Linear(in_features=512, out_features=10, bias=True) )
```

我们遵循相同的模式，我们为解码部分创建一个新的块，linear + sigmoid，并传递一个包含大小的数组。我们必须添加一个`self.last`,因为我们不想激活输出

现在，我们甚至可以将我们的模型一分为二！编码器+解码器

```
MyCNNClassifier( (encoder): MyEncoder( (conv_blokcs): Sequential( (0): Sequential( (0): Conv2d(1, 32, kernel_size=(3, 3), stride=(1, 1), padding=(1, 1)) (1): BatchNorm2d(32, eps=1e-05, momentum=0.1, affine=True, track_running_stats=True) (2): ReLU() ) (1): Sequential( (0): Conv2d(32, 64, kernel_size=(3, 3), stride=(1, 1), padding=(1, 1)) (1): BatchNorm2d(64, eps=1e-05, momentum=0.1, affine=True, track_running_stats=True) (2): ReLU() ) ) ) (decoder): MyDecoder( (dec_blocks): Sequential( (0): Sequential( (0): Linear(in_features=1024, out_features=512, bias=True) (1): Sigmoid() ) ) (last): Linear(in_features=512, out_features=10, bias=True) ) )
```

注意`MyEncoder`和`MyDecoder`也可以是返回`nn.Sequential`的函数。我更喜欢对模型使用第一种模式，对构建模块使用第二种模式。

通过将我们的模块分成子模块，共享代码、**调试代码、T23 测试代码变得更加容易。**

# ModuleList:当我们需要迭代时

`ModuleList`允许您将`Module`存储为列表。当你需要遍历层并存储/使用一些信息时，比如在 U-net 中，它会很有用。

`Sequential`之间的主要区别是`ModuleList`没有`forward`方法，所以内层没有连接。假设我们需要解码器中每个层的每个输出，我们可以通过以下方式存储它:

```
torch.Size([4, 16]) torch.Size([4, 32]) [None, None]
```

# ModuleDict:当我们需要选择时

如果我们想在我们的`conv_block`中切换到`LearkyRelu`呢？我们可以使用`ModuleDict`创建一个`Module`的字典，并在需要时动态切换`Module`

```
Sequential( (0): Conv2d(1, 32, kernel_size=(3, 3), stride=(1, 1), padding=(1, 1)) (1): BatchNorm2d(32, eps=1e-05, momentum=0.1, affine=True, track_running_stats=True) (2): LeakyReLU(negative_slope=0.01) ) Sequential( (0): Conv2d(1, 32, kernel_size=(3, 3), stride=(1, 1), padding=(1, 1)) (1): BatchNorm2d(32, eps=1e-05, momentum=0.1, affine=True, track_running_stats=True) (2): ReLU() )
```

# 最终实施

让我们把一切都结束吧！

```
MyCNNClassifier( (encoder): MyEncoder( (conv_blokcs): Sequential( (0): Sequential( (0): Conv2d(1, 32, kernel_size=(3, 3), stride=(1, 1), padding=(1, 1)) (1): BatchNorm2d(32, eps=1e-05, momentum=0.1, affine=True, track_running_stats=True) (2): LeakyReLU(negative_slope=0.01) ) (1): Sequential( (0): Conv2d(32, 64, kernel_size=(3, 3), stride=(1, 1), padding=(1, 1)) (1): BatchNorm2d(64, eps=1e-05, momentum=0.1, affine=True, track_running_stats=True) (2): LeakyReLU(negative_slope=0.01) ) ) ) (decoder): MyDecoder( (dec_blocks): Sequential( (0): Sequential( (0): Linear(in_features=1024, out_features=512, bias=True) (1): Sigmoid() ) ) (last): Linear(in_features=512, out_features=10, bias=True) ) )
```

# 结论

你可以在这里找到代码

综上所述。

*   当你有一个由多个小块组成大块时，使用`Module`
*   当你想从层中创建一个小块时，使用`Sequential`
*   当你需要迭代一些层或构建块并做一些事情时，使用`ModuleList`
*   当需要参数化模型的某些模块时，例如激活功能，使用`ModuleDict`

那都是乡亲们！

感谢您的阅读

*原载于*[*gist.github.com*](https://gist.github.com/7b228760eefe8e77fb0a37b5783a379c)*。*