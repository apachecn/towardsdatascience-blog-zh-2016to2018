# 定制 Keras 生成器

> 原文：<https://towardsdatascience.com/writing-custom-keras-generators-fe815d992c5a?source=collection_archive---------0----------------------->

## 定制您的数据生成器，加快培训速度

使用 *Keras* 生成器的想法是在训练过程中即时获得成批的输入和相应的输出，例如读入 100 幅图像，获得相应的 100 个标签向量，然后将这组输入馈送到 gpu 进行训练步骤。

![](img/d324547e180a0cbd017336237ff3240b.png)

**Keras** : keras.io

我面临的问题是标准的 Keras 生成器的内存需求。内存生成器创建原始数据的副本，并且必须将`dtype`从`uint8`转换为`float64`。另一方面，从目录中读取的 *Keras* 生成器期望每个类中的图像都在一个独立的目录中(在多标签问题、分割问题等中是不可能的)。)

因此，我喜欢将批处理生成器分为 4 个步骤:

```
1\. Get input            : input_path -> image
2\. Get output           : input_path -> label
3\. Pre-process input    : image -> pre-processing step -> image
4\. Get generator output : *(* *batch_input, batch_labels )*
```

***步骤 1 :*** 定义一个函数来获取输入(可以是子集化一个 numpy 数组，pandas dataframe，从磁盘读入等等。) :

```
from skimage.io import imreaddef get_input(path):

    img = imread( path )

    return( img )
```

***第二步:*** 定义一个函数来获取输出:

```
import numpy as np
import pandas as pddef get_output( path, label_file = None ):

    img_id = path.split('/')[-1].split('.')[0]
    labels = label_file.loc[img_id].values

    return(labels)
```

***第三步:*** 定义一个函数对输入进行预处理:

```
def preprocess_input( image ):

    --- Rescale Image
    --- Rotate Image
    --- Resize Image
    --- Flip Image
    --- PCA etc.

    return( image )
```

***第四步:*** 把所有的东西放在一起定义你的生成器:

```
def image_generator(files, label_file, batch_size = 64):

    while True: # Select files (paths/indices) for the batch
          batch_paths  = np.random.choice(a    = files, 
                                          size = batch_size)
          batch_input  = []
          batch_output = [] 

          # Read in each input, perform preprocessing and get labels for input_path in batch_paths: input = get_input(input_path )
              output = get_output(input_path,label_file=label_file )

              input = preprocess_input(image=input)
              batch_input += [ input ]
              batch_output += [ output ] # Return a tuple of (input, output) to feed the network batch_x = np.array( batch_input )
          batch_y = np.array( batch_output )

          yield( batch_x, batch_y )
```

现在，您可以添加特定数据集定义的预处理功能，将输出作为图像掩模(分割问题、定位问题等)。).

在这之后，使用这个定制生成器与使用一个预定义的*Keras*imagedata generator 并简单地将生成器对象传递给`model.fit_generator().`是一样的