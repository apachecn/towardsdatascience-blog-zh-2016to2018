# 使用 GANS 进行半监督学习

> 原文：<https://towardsdatascience.com/using-gans-for-semi-supervised-learning-df1bfc289601?source=collection_archive---------3----------------------->

在监督学习中，我们有一个输入 x 和类别标签 y 的训练集。我们训练一个以 x 为输入，y 为输出的模型。

在半监督学习中，我们的目标仍然是训练一个以 x 为输入，生成 y 为输出的模型。然而，并非所有的训练示例都有标签 y。我们需要开发一种算法，通过研究标记的(x，y)对和未标记的 x 示例，能够更好地进行分类。

我们将使用 GAN 鉴别器作为 n+1 类鉴别器。它将识别 n 个不同类别的标记数据，以及来自生成器的第(n+1)类伪图像。鉴别器将在真实的标记图像、真实的未标记图像和虚假图像上进行训练。通过利用三个数据源而不是一个数据源，它将比只利用一个数据源训练的传统分类器更好地推广到测试集。

## 发电机:

输出与真实图像尺寸相同的假图像的典型发生器。

需要注意的事项:

*   由于输出激活是 tanh(值范围从-1 到+1)，这意味着当我们训练鉴别器时，我们需要将真实图像缩放到相同的范围。
*   我们对生成器的每一层使用标准卷积转置、批量范数和泄漏 relu。

```
def generator(z, output_dim, reuse=False, alpha=0.2, training=True, size_mult=128):
    with tf.variable_scope('generator', reuse=reuse):
        # First fully connected layer
        x1 = tf.layers.dense(z, 4 * 4 * size_mult * 4)
        # Reshape it to start the convolutional stack
        x1 = tf.reshape(x1, (-1, 4, 4, size_mult * 4))
        x1 = tf.layers.batch_normalization(x1, training=training)
        x1 = tf.maximum(alpha * x1, x1)

        x2 = tf.layers.conv2d_transpose(x1, size_mult * 2, 5, strides=2, padding='same')
        x2 = tf.layers.batch_normalization(x2, training=training)
        x2 = tf.maximum(alpha * x2, x2)

        x3 = tf.layers.conv2d_transpose(x2, size_mult, 5, strides=2, padding='same')
        x3 = tf.layers.batch_normalization(x3, training=training)
        x3 = tf.maximum(alpha * x3, x3)

        # Output layer
        logits = tf.layers.conv2d_transpose(x3, output_dim, 5, strides=2, padding='same')

        out = tf.tanh(logits)

        return out
```

**鉴别器:**

由于无监督的组件，鉴别器比生成器更复杂。

需要注意的事项:

*   积极和广泛使用辍学。这是因为我们在一小组带标签的训练样本上有一个深度卷积层的大锤(注意，大多数训练样本是无标签的)，这使得很难不对训练数据进行过度拟合，并教导鉴别器更好地进行概括。
*   我们不在鉴别器的第一层使用批量标准化，因为鉴别器从真实和虚假数据中查看数据的分布是很重要的。
*   我们在鉴别器的最后一层没有使用批处理规范化。Bn 会将每个特征的平均值设置为 Bnμ参数。该层用于特征匹配损失，仅当鉴别器对数据运行时的平均值与鉴别器对生成器样本运行时的平均值不同时，该层才起作用。
*   与标准 GAN 鉴别器一样，我们对每层使用卷积步长、批量范数和泄漏 relu，并且我们使用卷积步长而不是最大池。
*   对于最后一层，我们使用全局平均池来获取特征，而不是密集层。
*   class_logits =不同类上的 softmax 分布。
*   gan_logits 使得 P(输入是真实的还是虚假的)= sigmoid(gan_logits)

```
def discriminator(x, reuse=False, alpha=0.2, drop_rate=0., num_classes=10, size_mult=64):
    with tf.variable_scope('discriminator', reuse=reuse):
        x = tf.layers.dropout(x, rate=drop_rate/2.5)

        # Input layer is 32x32x3
        x1 = tf.layers.conv2d(x, size_mult, 3, strides=2, padding='same')
        relu1 = tf.maximum(alpha * x1, x1)
        relu1 = tf.layers.dropout(relu1, rate=drop_rate)

        x2 = tf.layers.conv2d(relu1, size_mult, 3, strides=2, padding='same')
        bn2 = tf.layers.batch_normalization(x2, training=True)
        relu2 = tf.maximum(alpha * x2, x2)

        x3 = tf.layers.conv2d(relu2, size_mult, 3, strides=2, padding='same')
        bn3 = tf.layers.batch_normalization(x3, training=True)
        relu3 = tf.maximum(alpha * bn3, bn3)
        relu3 = tf.layers.dropout(relu3, rate=drop_rate)

        x4 = tf.layers.conv2d(relu3, 2 * size_mult, 3, strides=1, padding='same')
        bn4 = tf.layers.batch_normalization(x4, training=True)
        relu4 = tf.maximum(alpha * bn4, bn4)

        x5 = tf.layers.conv2d(relu4, 2 * size_mult, 3, strides=1, padding='same')
        bn5 = tf.layers.batch_normalization(x5, training=True)
        relu5 = tf.maximum(alpha * bn5, bn5)

        x6 = tf.layers.conv2d(relu5, 2 * size_mult, 3, strides=2, padding='same')
        bn6 = tf.layers.batch_normalization(x6, training=True)
        relu6 = tf.maximum(alpha * bn6, bn6)
        relu6 = tf.layers.dropout(relu6, rate=drop_rate)

        x7 = tf.layers.conv2d(relu5, 2 * size_mult, 3, strides=1, padding='valid')
        # Don't use bn on this layer, because bn would set the mean of each feature
        # to the bn mu parameter.
        # This layer is used for the feature matching loss, which only works if
        # the means can be different when the discriminator is run on the data than
        # when the discriminator is run on the generator samples.
        relu7 = tf.maximum(alpha * x7, x7)

        # Flatten it by global average pooling
        features = tf.reduce_mean(relu7, (1, 2))

        # Set class_logits to be the inputs to a softmax distribution over the different classes
        class_logits = tf.layers.dense(features, num_classes + extra_class)

        # Set gan_logits such that P(input is real | input) = sigmoid(gan_logits).
        # Keep in mind that class_logits gives you the probability distribution over all the real
        # classes and the fake class. You need to work out how to transform this multiclass softmax
        # distribution into a binary real-vs-fake decision that can be described with a sigmoid.
        # Numerical stability is very important.
        # You'll probably need to use this numerical stability trick:
        # log sum_i exp a_i = m + log sum_i exp(a_i - m).
        # This is numerically stable when m = max_i a_i.
        # (It helps to think about what goes wrong when...
        #   1\. One value of a_i is very large
        #   2\. All the values of a_i are very negative
        # This trick and this value of m fix both those cases, but the naive implementation and
        # other values of m encounter various problems)

        if extra_class:
            real_class_logits, fake_class_logits = tf.split(class_logits, [num_classes, 1], 1)
            assert fake_class_logits.get_shape()[1] == 1, fake_class_logits.get_shape()
            fake_class_logits = tf.squeeze(fake_class_logits)
        else:
            real_class_logits = class_logits
            fake_class_logits = 0.

        mx = tf.reduce_max(real_class_logits, 1, keep_dims=True)
        stable_real_class_logits = real_class_logits - mx gan_logits = tf.log(tf.reduce_sum(tf.exp(stable_real_class_logits), 1)) + tf.squeeze(mx) - fake_class_logits

        out = tf.nn.softmax(class_logits)

        return out, class_logits, gan_logits, features
```

## 模型损失函数和优化器:

**d_loss** :鉴频器的损耗为:

1.  GAN 问题的损失，其中我们最小化二进制真假分类问题的交叉熵:d_loss_real 和 d_loss_fake
2.  真实监督数据(带标签)多分类问题的损失:d_loss_class。对于 d_loss_class，我们仅考虑迷你批次中具有标签的样本，但是迷你批次中可能没有样本具有标签，因此以下表达式将为 0/1，并且 d_loss_class 的损失为 0。" TF . reduce _ sum(label _ mask * class _ cross _ entropy)/TF . maximum(1。，tf.reduce_sum(label_mask))"

发电机的损耗是由 Tim Salimans 在 OpenAI 发明的“特征匹配”损耗。这种损失包括最小化数据上的预期特征和生成样本上的预期特征之间的绝对差异。这种损失对于半监督学习比传统的 GAN 损失更有效。随着时间的推移，这将迫使生成器生成与真实数据相似的样本。

> 请注意，使用生成器的特征损失，生成器样本不会像在传统 DCGAN 中生成的样本一样好，但是这里的目标是使用小的标记数据集和大的未标记数据集来实现多类分类的高鉴别器准确度。

```
def model_loss(input_real, input_z, output_dim, y, num_classes, label_mask, alpha=0.2, drop_rate=0.):
    """
    Get the loss for the discriminator and generator
    :param input_real: Images from the real dataset
    :param input_z: Z input
    :param output_dim: The number of channels in the output image
    :param y: Integer class labels
    :param num_classes: The number of classes
    :param alpha: The slope of the left half of leaky ReLU activation
    :param drop_rate: The probability of dropping a hidden unit
    :return: A tuple of (discriminator loss, generator loss)
    """

    # These numbers multiply the size of each layer of the generator and the discriminator,
    # respectively. You can reduce them to run your code faster for debugging purposes.
    g_size_mult = 32
    d_size_mult = 64

    # Here we run the generator and the discriminator
    g_model = generator(input_z, output_dim, alpha=alpha, size_mult=g_size_mult)
    d_on_data = discriminator(input_real, alpha=alpha, drop_rate=drop_rate, size_mult=d_size_mult)
    d_model_real, class_logits_on_data, gan_logits_on_data, data_features = d_on_data
    d_on_samples = discriminator(g_model, reuse=True, alpha=alpha, drop_rate=drop_rate, size_mult=d_size_mult)
    d_model_fake, class_logits_on_samples, gan_logits_on_samples, sample_features = d_on_samples

    # Here we compute `d_loss`, the loss for the discriminator.
    # This should combine two different losses:
    #  1\. The loss for the GAN problem, where we minimize the cross-entropy for the binary
    #     real-vs-fake classification problem.
    #  2\. The loss for the SVHN digit classification problem, where we minimize the cross-entropy
    #     for the multi-class softmax. For this one we use the labels. Don't forget to ignore
    #     use `label_mask` to ignore the examples that we are pretending are unlabeled for the
    #     semi-supervised learning problem.
    d_loss_real = tf.reduce_mean(
        tf.nn.sigmoid_cross_entropy_with_logits(logits=gan_logits_on_data,
                                                labels=tf.ones_like(gan_logits_on_data)))
    d_loss_fake = tf.reduce_mean(
        tf.nn.sigmoid_cross_entropy_with_logits(logits=gan_logits_on_samples,
                                                labels=tf.zeros_like(gan_logits_on_samples)))
    y = tf.squeeze(y)
    class_cross_entropy = tf.nn.softmax_cross_entropy_with_logits(logits=class_logits_on_data,
                                                                  labels=tf.one_hot(y, num_classes + extra_class,
                                                                                    dtype=tf.float32))
    class_cross_entropy = tf.squeeze(class_cross_entropy)
    label_mask = tf.squeeze(tf.to_float(label_mask))
    d_loss_class = tf.reduce_sum(label_mask * class_cross_entropy) / tf.maximum(1., tf.reduce_sum(label_mask))
    d_loss = d_loss_class + d_loss_real + d_loss_fake

    # Here we set `g_loss` to the "feature matching" loss invented by Tim Salimans at OpenAI.
    # This loss consists of minimizing the absolute difference between the expected features
    # on the data and the expected features on the generated samples.
    # This loss works better for semi-supervised learning than the tradition GAN losses.
    data_moments = tf.reduce_mean(data_features, axis=0)
    sample_moments = tf.reduce_mean(sample_features, axis=0)
    g_loss = tf.reduce_mean(tf.abs(data_moments - sample_moments))pred_class = tf.cast(tf.argmax(class_logits_on_data, 1), tf.int32)
    eq = tf.equal(tf.squeeze(y), pred_class)
    correct = tf.reduce_sum(tf.to_float(eq))
    masked_correct = tf.reduce_sum(label_mask * tf.to_float(eq))

    return d_loss, g_loss, correct, masked_correct, g_model
```

[https://github . com/mchablani/deep-learning/blob/master/semi-supervised/semi-supervised _ learning _ 2 _ solution . ipynb](https://github.com/mchablani/deep-learning/blob/master/semi-supervised/semi-supervised_learning_2_solution.ipynb)

演职员表:来自讲稿:【https://classroom.udacity.com/nanodegrees/nd101/syllabus 