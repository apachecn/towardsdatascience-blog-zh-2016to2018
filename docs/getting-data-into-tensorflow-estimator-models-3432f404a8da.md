# 将数据导入张量流估算模型

> 原文：<https://towardsdatascience.com/getting-data-into-tensorflow-estimator-models-3432f404a8da?source=collection_archive---------2----------------------->

![](img/83a4e1a483e4eb3730e828e691adabdd.png)

Photo by [Mathew Schwartz](https://unsplash.com/@cadop?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

机器学习是关于你的数据的数量和质量。上述数据通常可从多种来源获得:

*   文本文件(CSV、TSV、Excel)
*   数据库
*   流媒体源

文本文件是由从其他来源提取数据的某个人或某些人提供的，但他们希望您不必亲自提取数据。数据可以在一个或多个文件中，有或没有标题。

张量流估计器使用输入函数。输入函数的签名返回一组要素和标注。特性是特性名称和数值数组的字典。标签是一组值。需要进行一些管理，比如对数据进行洗牌，然后分批返回。你采取的方法决定了你需要付出多少努力。

让我们从简单的选项开始。如果你把数据放在一个文件中，你可以把它完全读入内存(所谓的玩具例子)，而且这个文件是以文本分隔的格式(CSV，TSV 等)，所需的工作量是最小的。通常情况下，你可以用 numpy 或 pandas 来读取你的文件。

提醒一下，在使用`tf.estimator` API 的时候，需要在训练的时候传入一个输入函数。这是用于训练的函数签名:

```
train(
    input_fn,
    hooks=None,
    steps=None,
    max_steps=None,
    saving_listeners=None
)
```

我们的重点是`input_fn`！我们将使用流行的波士顿住房数据，这些数据在[这里](https://www.kaggle.com/c/boston-housing)托管。

如果你有 numpy 格式的数据，你可以使用`tf.estimator.inputs.numpy_input_function`来获取你的数据。首先，您需要为您的特征定义一个字典:

```
# extract numpy data from a DataFrame
crim = train_df['crim'].values
zn = train_df['zn'].values
indus = train_df['indus'].values
chas = train_df['chas'].values
nox = train_df['nox'].values
rm = train_df['rm'].values
age = train_df['age'].values
dis = train_df['dis'].values
rad = train_df['rad'].values
tax = train_df['tax'].values
ptratio = train_df['ptratio'].values
black = train_df['black'].values
lstat = train_df['lstat'].values
medv = train_df['medv'].values# create a dictionary
x_dict = {
    'crim': crim,
    'zn': zn,
    'indus': indus,
    'chas': chas,
    'nox': nox,
    'rm': rm,
    'age': age,
    'dis': dis,
    'rad': rad,
    'tax': tax,
    'ptratio': ptratio,
    'black': black,
    'lstat': lstat
}
```

有了字典，我们可以继续定义输入函数。

```
def np_training_input_fn(x, y):
  return tf.estimator.inputs.numpy_input_fn(
      x= x,
      y= y,
      batch_size= 32,
      num_epochs= 5, # this way you can leave out steps from training
      shuffle= True,
      queue_capacity= 5000
  )
```

在我们的函数中，我们传入 x 和 y，x 是我们的字典，y 是我们的标签。我们还可以传入我们的批量大小、时期数以及是否打乱数据。请注意，你总是想打乱你的数据。批量大小是一个超级参数，你应该凭经验归档。epochs 的数量是您想要查看数据的次数。对于训练，设置任何数字。对于测试，将其设置为 1。

在创建评估器之前，您需要特性列。

```
feature_cols = [tf.feature_column.numeric_column(k) for k in x_dict.keys()]lin_model = tf.estimator.LinearRegressor(feature_columns=feature_cols)lin_model.train(np_training_input_fn(x_dict, medv), steps=10)
```

您可以省略步骤，以便训练使用在训练输入函数中指定的时段，或者指定用于训练的步骤数。numpy 输入到此为止。

对于数据帧，您将继续定义输入函数如下:

```
def pd_input_fn(df, y_label):
  return tf.estimator.inputs.pandas_input_fn(
      x=df,
      y=df[y_label],
      batch_size = 32,
      num_epochs = 5,
      shuffle = True,
      queue_capacity = 1000,
      num_threads = 1
  )
```

请注意，在上面的方法中，我们继续传入数据帧，并在其中添加标签。如果标签不在您传递给`x`的内容中，您将得到一个错误。你把一个系列传给`y`。其他参数与处理 numpy 时相同。

该模型在未来会得到同样的对待。创建模型并指定特征列。然后，您继续训练模式。

```
lin_model = tf.estimator.LinearRegressor(feature_columns=feature_cols)lin_model.train(pd_input_fn(train_df, 'medv'), steps=10)
```

当您可以将数据读入内存时，一切都很好。但是，当你不能的时候会发生什么。当你的训练数据集是 100GB 时会发生什么？

好消息是，这样的数据集通常由分布式系统生成，因此您的文件将是`sharded`。这意味着数据将被存储在不同的文件名为`data-0001-of-1000`的文件中。

如果你从未处理过大数据，你的第一个想法可能是使用`glob`。不要这样做，除非你知道你正在处理一个玩具的例子。你会耗尽你的记忆，训练也会停止。

这些类型的文件通常没有头文件，这是一件好事。首先，您将定义一个列名列表，这些列名应该按照您的列在文件中存在的顺序排列。其次，定义一个标签列。最后，定义一个缺省值列表，以便在读取过程中遇到缺失值时可以处理它们。

```
CSV_COLUMNS = ['medv', 'crim', 'zn', 'lstat', 'tax', 'rad', 'chas', 'nox', 'indus', 'ptratio', 'age', 'black', 'rm', 'dis']
LABEL_COLUMN = 'medv'
DEFAULTS = [[0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0], [0.0]]
```

接下来，我们定义一个函数来读入文本数据，并返回我们的格式，就像我们前面的函数处理它们一样。函数创建方式的一个优点是它可以处理通配符，比如`data-*`。

```
**def** read_dataset(filename, mode, batch_size = 512):
  **def** _input_fn():
    **def** decode_csv(value_column):
      columns = tf.decode_csv(value_column, record_defaults = DEFAULTS)
      features = dict(zip(CSV_COLUMNS, columns))
      label = features.pop(LABEL_COLUMN)
      **return** features, label

    *# Create list of files that match pattern*
    file_list = tf.gfile.Glob(filename)

    *# Create dataset from file list*
    dataset = tf.data.TextLineDataset(file_list).map(decode_csv)
    **if** mode == tf.estimator.ModeKeys.TRAIN:
        num_epochs = None *# indefinitely*
        dataset = dataset.shuffle(buffer_size = 10 * batch_size)
    **else**:
        num_epochs = 1 *# end-of-input after this*

    dataset = dataset.repeat(num_epochs).batch(batch_size)
    **return** dataset.make_one_shot_iterator().get_next()
  **return** _input_fn
```

该函数接受三个参数:一个匹配多个文件的模式、一个模式(训练或评估)和一个批处理大小。注意`read_dataset`返回一个函数。我们称这个函数为`_input_fn`。在这个函数中，我们有一个名为`decode_csv`的函数，它将创建一个字典，提取一个序列，并以我们在本文开头提到的元组格式返回两者。

其次，我们的函数使用`glob`创建一个文件名列表。是的，`glob`仍然被使用，但是我们不把结果传递给一个`pandas.read_csv()`。反而遇到了`tf.data.TextLineDataset()`。它有三个参数:文件名列表、压缩格式(none、ZLIB 或 GZIP)和缓冲区大小。`read_csv`和`TextLineDataset`的主要区别在于，前者将内容读入内存(我们可以批量读取)，而后者返回一个`Iterator`。

因此，我们的函数通过调用`map`函数，传入`decode_csv`，使用`TextLineDataset`创建一个数据集。它做的下一件事是检查我们是否处于训练模式。如果不是，我们的历元数被设置为 1。如果是的话，它会被设置为我们想要的任意多个纪元。我们的训练数据集也被打乱了。然后，我们的数据集被设置为重复我们想要的历元数，并根据我们的批量大小进行配置。

最后，我们返回一个一次性迭代器，并调用`get_next()`。所有这些工作都由我们前面看到的函数在幕后处理。我们可以使用以下方法创建我们的训练、评估和测试输入函数:

```
**def** get_train():
  **return** read_dataset('./train-.*', mode = tf.estimator.ModeKeys.TRAIN)

**def** get_valid():
  **return** read_dataset('./valid.csv', mode = tf.estimator.ModeKeys.EVAL)

**def** get_test():
  **return** read_dataset('./test.csv', mode = tf.estimator.ModeKeys.EVAL)
```

剩下的过程和我们看到的完全一样。我们可以像往常一样创建我们的估计器并训练它。

对于真正的项目，你将从阅读一个使用熊猫和`tf.estimator.inputs`的训练文件开始。但是，要在培训中使用所有文件，您需要使用`tf.data.TextLineDataset`。

快乐编码。