# 将计算机视觉数据集纳入单一格式:迈向一致性

> 原文：<https://towardsdatascience.com/bringing-computer-vision-datasets-to-a-single-format-step-towards-consistency-870b8323bcf0?source=collection_archive---------7----------------------->

![](img/1b88b97c5972207d5dbdb8e1fe9f2acf.png)

当你有一个好的工作算法，并且你想在一些数据集上测试你的杰作时，几乎总是要在数据的实际加载和预处理上花费相当多的时间。如果我们能够以一种单一的格式和一致的方式访问数据(例如，总是将训练图像存储在关键字“train/image”下)，那就太好了。

在这里，我将分享一个由我编写的 [github repo](https://github.com/thushv89/PreprocessingBenchmarkDatasets) ，它将几个流行的数据集转换为 HDF5 格式。当前支持以下数据集。

*   [ILSVRC ImageNet](http://www.image-net.org/challenges/LSVRC/)
*   [CIFAR 10 和 CIFAR 100](https://www.cs.toronto.edu/~kriz/cifar.html) 数据集
*   数据集

# 这段代码是做什么的？

所以这个仓库做了很多事情。首先让我告诉你组织。代码库非常简单。每个数据集都有一个文件来预处理数据并另存为 HDF5(例如，对于 Imagenet，我们有`preprocess_imagenet.py`、CIFAR-10 和 CIFAR-100，我们有`preprocess_cifar.py`，对于 SVHN，我们有`preprocess_svhn.py`)。实际上，每个文件都执行以下操作:

*   将原始数据载入内存
*   执行所需的任何整形，以使数据达到适当的维度(例如，cifar 数据集将图像作为向量给出，因此需要将其转换为三维矩阵)
*   创建一个 HDF5 文件来保存数据
*   使用 Python 多重处理库，并根据用户规范处理每个图像

下面我将告诉 ImageNet 文件是做什么的。这是最复杂的文件，其他的都很简单。

这里我讨论一下`preprocess_imagenet.py`文件的作用。这基本上将 ImageNet 数据的子集保存为 HDF5 文件。该子集是属于许多自然类别(例如植物、猫)和人工类别(例如椅子、桌子)的数据。此外，您可以在保存数据时规范化数据。

一旦运行脚本，`save_imagenet_as_hdf5(...)`函数就会接管。该函数首先在有效的数据集文件名和标签之间创建一个映射(即`build_or_retrieve_valid_filename_to_synset_id_mapping(...)`)。接下来，它用`write_art_nat_ordered_class_descriptions(...)`或`retrieve_art_nat_ordered_class_descriptions(...)`隔离与 ImageNet 数据集(1000 个类)的分类问题相关的类。然后我们使用`write_selected_art_nat_synset_ids_and_descriptions(...)`方法将选择的人工和自然类信息写入一个 xml 文件。

接下来，我们扫描训练数据中的所有子目录，并将所有相关数据点放入内存。接下来，我们创建 HDF 文件来保存数据。这是通过`save_train_data_in_filenames(...)`功能完成的。数据将保存在以下注册表项下:

*   `/train/images/`
*   `/train/images/`
*   `/valid/images/`
*   `/valid/images/`

# 稍后访问和加载数据

您以后可以通过以下方式访问这些保存的数据:

```
dataset_file = h5py.File(“data” + os.sep + “filename.hdf5”, “r”)train_dataset, train_labels = dataset_file[‘/train/images’], dataset_file[‘/train/labels’]test_dataset, test_labels = dataset_file[‘/test/images’], dataset_file[‘/test/labels’]
```

# 编码和进一步阅读

代码可以在这里[获得](https://github.com/thushv89/PreprocessingBenchmarkDatasets)，你可以在我的[博客文章](http://www.thushv.com/computer_vision/bringing-computer-vision-datasets-to-a-single-format-step-towards-consistency/)中查看关于代码做什么以及如何运行的完整描述。

**注意:**如果您在运行代码时看到任何问题或错误，请通过评论或在 Github 页面上打开问题让我知道。这将有助于我改进代码，消除任何讨厌的错误。

干杯！