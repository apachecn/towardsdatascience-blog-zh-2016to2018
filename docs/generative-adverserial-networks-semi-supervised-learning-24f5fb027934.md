# 生成广告序列网络和半监督学习

> 原文：<https://towardsdatascience.com/generative-adverserial-networks-semi-supervised-learning-24f5fb027934?source=collection_archive---------5----------------------->

# 介绍

我将直接进入我们上次在高层[解释过的内容。该代码也可以在](http://jakublangr.com/gans-tutorial.html) [GitHub](https://github.com/jakubLangr/Gans-Semi-Supervised/blob/master/gans_semi_supervised_learning.ipynb) 和 [Medium](https://medium.com/@james.langr) 上获得。这部分与 Jupyter 笔记本相同，只是缺少代码输出。

![](img/e5a042ea0c2a9866c3e87925dbd752ad.png)

Just a sanity check that my synthesising of faces works as expected.

# 生成广告序列网络和半监督学习

# 作者:雅各布·朗格

这段代码是为我写的，用来试验人工智能的一些最新进展。我特别选择了半监督学习和生成式逆向网络(GANs)来推动自己。其中一些代码是作为深度学习课程的[创造性应用的作业完成的，这对我学习现代人工智能有极大的帮助。一些广泛的框架来自于](https://www.kadenze.com/courses/creative-applications-of-deep-learning-with-tensorflow-iv) [Parag Mital](https://www.linkedin.com/in/pkmital) 对课程最后部分的预编码设置和解释，但他的代码的这种用法是完全新颖的，需要大量的工程、缝合和抽象。在这个 Jupyter 笔记本中，我做了以下事情:

1.  导入所有必要的依赖项(以及一些我在开发过程中使用但在最终版本中没有使用的依赖项)
2.  使用 GAN 方法生成合成图像。+更具体地说，[这种最近非常流行的无监督技术](http://blog.aylien.com/introduction-generative-adversarial-networks-code-tensorflow/)可以通过竞争另一个网络来欺骗对方(稍后解释)，来学习[名人数据集](http://jakublangr.com/mmlab.ie.cuhk.edu.hk/projects/CelebA.html)上构成人脸的更高表示(以及潜在空间中的许多属性)+或者，人们可以将这种方法视为使用自动编码器风格的基因生成模型，该模型试图基于播种因子生成新的示例。
3.  这种播种因子或“潜在特征空间”总是对生成模型的某些方面进行编码，一旦理解，就可以用于可预测地操纵生成图像的性质——例如秃顶、性别或微笑。
4.  因此，我们可以产生几乎无限的新例子，因为我们知道如何操纵潜在空间，我们可以知道它们的标签。在这个例子中，我们创建了 40，000 张男人和女人的脸，现在可以用于进一步的训练
5.  然后，我们在合成数据上训练下一层分类器，用于男性或女性面部的二元分类。然而，我们不是从头开始训练一个新的分类器，而是使用一种使用牛津大学的`Visual Geometry Group`或`vgg16`预训练网络的`transfer learning`方法来获得更高的准确性，而不必在大规模集群上训练数天。
6.  我们使用不同的`vgg16`名人面孔预测(确切地说是`2623`)并在带有标签的合成例子上训练一个简单的完全连接的两层神经网络。(这取代了典型的迁移学习方法，即删除最后一层并在其上进行训练。在这里，我们简单地将其分为 2 个步骤)
7.  使用 100 个手工标记(由我)的例子来评估新分类器的准确性。

# 动机

这真的很令人兴奋，因为它允许我们在几乎没有标记数据的情况下训练分类器，只要我们有许多未标记的数据，[这是一个非常有前途的策略，特别是对于拥有较小数据集的较小公司](http://jakublangr.com/ai-2016-review.html)。

# 术语的简要定义:

半监督学习:基本上是在训练过程中使用未标记数据和标记数据

生成性广告系列网络:详细解释如下

代码是在`Tensorflow 1.0.0`做的。

```
# First check the Python version
import sys
if sys.version_info < (3,4):
    print('You are running an older version of Python!\n\n',
          'You should consider updating to Python 3.4.0 or',
          'higher as the libraries built for this course',
          'have only been tested in Python 3.4 and higher.\n')
    print('Try installing the Python 3.5 version of anaconda'
          'and then restart `jupyter notebook`:\n',
          'https://www.continuum.io/downloads\n\n')# Now get necessary libraries
try:
    import os
    import pandas as pd
    import pickle
    import tflearn
    import pickle
    from joblib import Parallel, delayed
    import random
    import multiprocessing
    import numpy as np
    import matplotlib.pyplot as plt
    from skimage.transform import resize
    from skimage import data
    from scipy.misc import imresize
    from scipy.ndimage.filters import gaussian_filter
    import IPython.display as ipyd
    import tensorflow as tf
    from libs import utils, datasets, dataset_utils, nb_utils
except ImportError as e:
    print(e)
    print("Make sure you have started notebook in the same directory",
          "as the provided zip file which includes the 'libs' folder",
          "and the file 'utils.py' inside of it.  You will NOT be able",
          "to complete this assignment unless you restart jupyter",
          "notebook inside the directory created by extracting",
          "the zip file or cloning the github repo.")
    print(e)# We'll tell matplotlib to inline any drawn figures like so:
%matplotlib inline
plt.style.use('ggplot')
```

# 生成对抗网络(GAN) /深度卷积网络(DCGAN)

# 介绍

回想一下，生成性对抗网络是两个网络，一个生成器和一个鉴别器。“生成器”获取一个特征向量，并将该特征向量解码成图像。鉴别器与自动编码器的编码器完全一样，只是它在最终层中只能有 1 个值。我们用一个 sigmoid 把这个值压扁在 0 和 1 之间，然后把它的意思解读为:1，你给我的图像是真的，或者 0，你给我的图像是生成器生成的，是假的！所以鉴别器就像一个编码器，它拍摄图像，然后进行测谎。你在给我灌输谎言吗？还是形象真实？

以自动编码器为例。损失函数部分作用于输入空间。它说，每像素，我的重建和输入图像有什么不同？每像素的 l2 损失。回想一下，当时我们认为这不是最好的主意，因为每像素的差异并不代表我们对图像的感知。考虑这一点的一种方法是，如果我们有相同的图像，并将其平移几个像素。我们无法分辨这种差异，但两幅图像之间的每像素差异可能非常大。

GAN 不使用每像素差异。相反，它训练一个距离函数:鉴别器。鉴别器接受两个图像，真实图像和生成的图像，并学习相似图像应该是什么样子！这真的是这个网络令人惊奇的部分，并且为无监督学习开辟了一些非常令人兴奋的潜在未来方向。另一个也学习距离功能的网络被称为暹罗网络。我们在本课程中没有涉及到这个网络，但它常用于面部验证，或断言两张脸是否相同。

众所周知，GAN 网络的训练是一个巨大的痛苦！因此，我们实际上不会训练它。相反，我们将讨论这个基本网络的扩展，称为 VAEGAN(变分自动编码器 GAN)。现在，让我们继续创建 GAN。

让我们首先创建两个网络:鉴别器和生成器。我们将首先构建一个通用编码器，用于我们的鉴别器。我们想要的是使用我们的编码器的每个层的维度列表对输入占位符进行编码。在卷积网络的情况下，我们的维度列表应该对应于输出滤波器的数量。我们还需要指定每层卷积网络的内核高度和宽度。

我们首先需要一个占位符。这将是输入鉴别器的“真实”图像，鉴别器将把该图像编码成单个值，0 或 1，表示“是，这是真实的”,或者“否，这不是真实的”。

该描述由 Parag 在麻省理工学院许可下善意提供。

```
net = CV.get_celeb_vaegan_model()
```

我们将加载包含在这个字典中的 graph_def。它遵循与`inception`、`vgg16`和`i2v`预训练网络相同的思想。它是一个定义了关键字`graph_def`的字典，具有图的预训练网络。它还包括`labels`和一个`preprocess`键。我们将不得不做一件额外的事情，那就是关闭来自变化层的随机采样。这并不是真正必要的，但将确保我们每次使用网络时得到相同的结果。我们将使用`input_map`参数来做到这一点。不要担心这没有任何意义，因为我们没有覆盖任何深度的变化层。要知道，这是从网络中移除一个随机过程，因此它是完全确定的。如果我们没有这样做，我们每次使用网络时都会得到稍微不同的结果(这可能是您所希望的)。

现在让我们得到网络的相关部分:`X`，网络的输入图像，`Z`，输入图像的编码，`G`，解码图像。在许多方面，这就像我们在上面学到的自动编码器，除了不是`Y`作为输出，我们有来自我们的生成器的`G`！我们训练它的方式非常不同:我们在生成器和鉴别器之间使用对抗过程，并使用鉴别器自己的距离度量来帮助训练网络，而不是像素到像素的差异。

```
X = g.get_tensor_by_name('net/x:0')
Z = g.get_tensor_by_name('net/encoder/variational/z:0')
G = g.get_tensor_by_name('net/generator/x_tilde:0')
```

让我们来看看一些数据:

```
files = sorted(datasets.CELEB())
img_i = 20
img = plt.imread(files[img_i])
plt.imshow(img)
```

探索名人网络属性

现在，让我们尝试探索数据集的属性。我们没有用任何受监督的标签来训练网络，但名人网络数据集对其 200，000 张图像中的每张图像都有 40 个属性。这些已经被解析并存储在网络字典中:

找出某个属性的潜在编码名人数据集包括其 200，000+图像的每个属性。这允许我们将一些我们知道具有特定属性的图像(例如“微笑”)输入编码器。我们存储它们的编码，并保留编码值的这种分布。然后，我们可以查看任何其他图像，看看它是如何编码的，并通过添加我们微笑图像的编码来稍微改变编码！结果应该是我们的形象，但更多的微笑。这太疯狂了，我们将看看如何做到这一点。首先让我们检查我们的潜在空间:潜在特征算法现在让我们试着写一个通用函数来执行我们刚刚做的所有事情，这样我们就可以用许多不同的特征来做这件事。然后我们将尝试把它们结合起来，合成出具有我们想要的特征的人…

```
def get_features_for(label='Bald', has_label=True, n_imgs=50):
    # Helper function to obtain labels and then preprocessing and returning
    # a vector for the seeding function for GAN
    # basically figures out the embedding for a particular attribute
    label_i = net['labels'].index(label)
    label_idxs = np.where(net['attributes'][:, label_i] == has_label)[0]
    label_idxs = np.random.permutation(label_idxs)[:n_imgs]
    imgs = [plt.imread(files[img_i])[..., :3]
            for img_i in label_idxs]
    preprocessed = np.array([CV.preprocess(img_i) for img_i in imgs])
    zs = sess.run(Z, feed_dict={X: preprocessed})
    return np.mean(zs, 0)
```

现在我们使用代码在“男性”和“非男性”(女性)图像之间创建一个插值。因为我们只使用了两个端点，所以我们得到了两个图像:一个 100%的男人和一个 100%的女人(请注意，我们也可以通过对两个种子向量进行加权平均来得到中间的任何图像)。

```
def gan_generate_data(num_iter=20000,imgs=15):
    # generates 2*(number of iter) images 
    # adding random number of pictures for each synthesis (to increase variation)
    # returns list of [Male, Female] * num_iter images
    generated_images = [] for i in range(num_iter): n_imgs = random.choice(range(imgs-10, imgs+10)) z1 = get_features_for('Male', True, n_imgs=n_imgs)
        z2 = get_features_for('Male', False, n_imgs=n_imgs) notmale_vector = z2 - z1
        amt = np.linspace(0, 1, 2)
        zs = np.array([z1 + notmale_vector*amt_i for amt_i in amt])
        g = sess.run(G, feed_dict={Z: zs}) generated_images.append(g[0])
        generated_images.append(g[1]) if i%1000==0:
            print('Iteration number : {}'.format(i)) return generated_imagesgenerated_data = gan_generate_data()
```

很好，我们有数据可以使用，它保存在一个 pickle 文件中，所以我们不必重新创建它。现在，让我们只添加一个热编码标签(我们已经以可预测的方式完成了此操作，即 male (0)总是第一个)。我们可以对它进行感官检查，得到整个样品的形状。

```
labels = [0,1] * 20000
generated_data = np.array(generated_data)
generated_data.shape
```

# 延长

现在让我们进入迁移学习部分。首先，我们必须离开网络。

```
from libs import vgg16, inception, i2vnet = vgg16.get_vgg_face_model()
```

# 迁移学习

这里我们得到了`vgg16`网络，我们已经在前面加载了它，并使用它来为它自己的一个预训练类生成预测。然而，由于我们想要预测不同的任务，我们然后使用`transferred_predictions`函数来获得 2623 个不同类别的预测，然后使用它作为下一个分类器的输入，来训练它识别性别。

为了有效地做到这一点，我们必须首先做一些图像处理，这是我们在`transferred_df`做的。

```
def transferred_predictions(img):
    # gets an image (`np.array`) as an input outputs net's final layer predictions 
    results = [] # Grab the tensor defining the input to the network
    x = g.get_tensor_by_name(names[0] + ":0") # And grab the tensor defining the softmax layer of the network
    softmax = g.get_tensor_by_name(names[-2] + ":0") with tf.Session(graph=g) as sess, g.device('/cpu:0'):
        # Remember from the lecture that we have to set the dropout
        # "keep probability" to 1.0.
        res = softmax.eval(feed_dict={x: img } ) # , Not using droput here
                    # 'net/dropout_1/random_uniform:0': [[1.0] * 4096],
                    # 'net/dropout/random_uniform:0': [[1.0] * 4096]})
        test_array = res.argsort()[-5:][::-1].flatten()
        results = ([(res.flatten()[int(idx)], 
                net['labels'][int(idx)])
               for idx in test_array ]) result = pd.DataFrame(results, columns=['score','label']) # .sort(columns='score') results.append(result.score) return resultsdef transferred_df(generated_data):
    # does the preprocessing of the `list` of generated_data and outputs `list` of predictions
    results = [] for i in range(len(generated_data)):
        img = imresize(generated_data[i], size=(224,224,3))
        img = net['preprocess'](img)[np.newaxis]
        result = transferred_predictions(img)
        results.append(result) if i%1000==0:
            print("Current image id {}".format(i)) return results def parallel_transfer_eval(generated_data):
    # returns parallely executed `transferred_df` using first split (fs), second (ss) and third (ts) as divisors
    pool = multiprocessing.Pool(4)
    fs = int(len(generated_data)/4)
    ss = int(2*len(generated_data)/4)
    ts = int(3*len(generated_data)/4)
    target = generated_data[:fs], generated_data[fs:ss], generated_data[ss:ts],generated_data[ts:]
    results = pool.map(transferred_df, zip(target))
    # results = Parallel(n_jobs=4)(delayed(transferred_df)(img) for img in generated_data) return results
```

# 利用迁移学习

现在我们在典型的[迁移学习](http://cs231n.github.io/transfer-learning/)范例中使用`vgg16`做出的预测。在这里，我们只是采用最后一层预测，重塑特征，并将其提供给下一层分类器(有时也通过移除最后(几个)完全连接的层来完成)，并对整个网络进行训练。这里我们只是在最后一层创建一个新的。实践支持这两种方法。

```
from sklearn.cross_validation import train_test_split# train-test for proper evaluation
train_X, test_X, train_y, test_y = train_test_split(X, y )tflearn.init_graph(num_cores=8, gpu_memory_fraction=0.5)# set up the network
net = tflearn.input_data(shape=[None, 2623])
net = tflearn.fully_connected(net, 2, activation='softmax')
net = tflearn.regression(net, optimizer='adam', loss='categorical_crossentropy')# train
model = tflearn.DNN(net)
model.fit(generated_data, labels, validation_set=train_X) from sklearn.ensemble import RandomForestClassifier
from sklearn.preprocessing import LabelEncoder# reshape labels so that they match what the network expects
labels = ['Male', 'Female'] * 10000
encoder = LabelEncoder()
encoder.fit(labels)
labels = encoder.transform(labels)
labels = np_utils.to_categorical(labels)
labels.shapetest_imgs = np.array([CV.preprocess(plt.imread(file)) for file in files[:100]])
test_imgs.shape
```

我们已经完成了这一点，因为我们已经对生成的和手工标记的图像(测试)进行了评分！然而，这只是我们旅程中的第一步，因为现在我们必须将`vgg16`生成的分数转移到新的分类器上(转移学习中的最后一位，通常通过删除最后一层并使用新的最终层重新运行网络来简化，但这里是为了训练目的而明确完成的。)

# 训练和评估新的分类器

为了简单起见，我们将只使用`tflearn`分类器，这样我们就可以更容易地使用迁移学习来完成前面所有工作的复杂性:1 .我们训练(基于合成数据和因此完全可预测的标签)2。我们对(我的)手举例子进行评价

```
from __future__ import absolute_import
from __future__ import division
from __future__ import print_function labels = [0,1] * 10000feature_columns = [tf.contrib.layers.real_valued_column("", dimension=2623)] classifier = tf.contrib.learn.DNNClassifier(feature_columns=feature_columns,
                                            hidden_units=[2623,512],
                                            gradient_clip_norm=.01,
                                            optimizer=tf.train.AdamOptimizer(learning_rate=0.1),
                                            n_classes=2)
                                            # model_dir='./model')# Fit model.
classifier.fit(x=array,
               y=labels,
               batch_size=256,
               steps=10000) # Evaluate accuracy.
test_labels = np.array([0, 0, 1, 0, 0, 0, 1, 1, 0, 0, 0, 1, 1, 0, 1, 1, 0, 0, 0, 1, 1, 0, 1,
       0, 1, 0, 0, 0, 0, 1, 0, 1, 1, 0, 0, 1, 1, 1, 0, 0, 1, 0, 0, 0, 0, 0,
       0, 0, 1, 1, 1, 1, 1, 1, 0, 1, 0, 0, 0, 0, 1, 1, 0, 0, 1, 1, 0, 1, 1,
       1, 1, 0, 1, 0, 0, 1, 1, 0, 0, 1, 1, 1, 1, 0, 0, 0, 0, 0, 0, 0, 1, 1,
       0, 0, 0, 0, 0, 0, 0, 0])# test_array = np.array([ [res[0] for res in result] for result in test_array ])accuracy_score = classifier.evaluate(x=test_array,
                                     y=test_labels)["accuracy"]
print('Accuracy: {0:f}'.format(accuracy_score))
```

# 一般讨论

结果并不是那么出色，然而，我认为这是一个迷人的研究领域，很可能会成为人工智能未来的最大领域之一:但我们仍然比随机(持续)要好，如果我在这方面花更多时间，可能会变得更好。

此外，在许多行业应用中，只需稍加修改，这些代码就可以进行微调和重用:

a) 3D 对象生成

b) [Pix2Pix 应用](https://www.youtube.com/watch?v=u7kQ5lNfUfg)设法根据风格创建新图像，或者只是从卫星图像生成地图。这里的可能性实际上是无限的。

c)灌制老电影。仅举几个例子。

感谢您的阅读，如果您对此感兴趣，请浏览[我的网站](http://jakublangr.com)了解更多信息！

由于我通常会在我的网站上发布文章一周后在 Medium 上发布，所以我会为那些坚持到现在的人增加一点奖励，并提到我的下一篇文章将是关于区块链的，并评估它将如何改变金融世界。

**2018 年 10 月 8 日更新:我正在与人合著一本关于 GANs 的书** [**，你可以在这里找到。**](http://bit.ly/gan-book) **如需更多更新，您可以关注我@langrjakub，**因为我通常不会更新旧文章，但如果您感兴趣，请在 Twitter 上与我联系。

# 发布在[技术](http://jakublangr.com/category/technical.html)

[python](http://jakublangr.com/tag/python.html)[AI](http://jakublangr.com/tag/ai.html)[半监督学习](http://jakublangr.com/tag/semi-supervised-learning.html) [GANs](http://jakublangr.com/tag/gans.html) [创成式](http://jakublangr.com/tag/generative.html) [Adverserial](http://jakublangr.com/tag/adverserial.html) [神经网络](http://jakublangr.com/tag/neural-networks.html) [代码](http://jakublangr.com/tag/code.html)