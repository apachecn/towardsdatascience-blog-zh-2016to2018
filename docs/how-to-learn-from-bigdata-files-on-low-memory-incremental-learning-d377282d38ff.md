# 如何在低内存上处理 BigData 文件？

> 原文：<https://towardsdatascience.com/how-to-learn-from-bigdata-files-on-low-memory-incremental-learning-d377282d38ff?source=collection_archive---------6----------------------->

## 关于如何使用 Pandas/Dask 在 Python 中处理大数据文件的演练

![](img/64d359589eb5eb0f8adfc5ad9de3a039.png)

Photo by [Ruffa Jane Reyes](https://unsplash.com/@opak_02?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

这是我在`Tackle`类别的帖子中的一个，可以在我的 github repo [这里](https://github.com/PuneetGrov3r/MediumPosts/tree/master/Tackle)找到。

**(Edit-31/01/2019)** — [为 BigData 添加了 dask.distributed.LocalCluster 的信息](#2d3d)

**(Edit-12/4/2019)——**添加了关于数据集大小缩减和文件类型使用的新章节【尚未完成，但您仍可从中获得应用思路。]

# 索引

1.  [简介](#dd6b)
2.  [列记忆还原(pd。Series.astype())](#d920) [:未完成]
3.  [文件类型(减少内存使用)](#36ea) [:未完成]
4.  [数据探索](#f270)
5.  [预处理](#d167)
6.  [增量学习(熊猫)](#d418)
7.  [Dask(探索+准备+拟合预测)](#e0ba)
8.  [延伸阅读](#ca96)
9.  [参考文献](#e618)

```
***NOTE:*** This post goes along with ***Jupyter Notebook*** available in my Repo on Github:[[HowToHandleBigData](https://nbviewer.jupyter.org/github/PuneetGrov3r/MediumPosts/blob/master/Tackle/BigData-IncrementalLearningAndDask.ipynb)]  (with dummy data)
and 
Kaggle:[[HowToHandleBigData](https://www.kaggle.com/puneetgrover/learn-from-bigdata-files-on-low-memory)] (with Kaggle [competition](https://www.kaggle.com/c/ga-customer-revenue-prediction) data)
```

# 1.简介 [^](#bd3e)

随着数据量呈指数级增长，我们无法在电脑内存中容纳我们的数据，甚至我们的模型。我们都买不起高端组装台式机。

例如，最近的 kaggle [竞赛](https://www.kaggle.com/c/ga-customer-revenue-prediction)的数据集无法容纳在 kaggle 内核或 Colab 的 17GB 内存中。它有将近 200 万行，最重要的是，一些列有非常大的 JSON 数据作为字符串。我们应该如何解决这个问题？我们可以向谁求助呢？

增量学习和/或 Dask 来拯救！

你可能已经知道神经网络本质上是增量学习器，所以我们可以在那里解决这个问题。许多`sklearn`的模型提供了一种叫做`partial_fit`的方法，使用它我们可以批量建模。一些像`XGBoost`和`LightGBM`这样的 Boosting 库提供了一种渐进学习的方式来处理大数据。

在这里，我们将研究一些 Boosting 算法提供的增量解决方案。然后我们将在同一个数据集上使用`Dask`，并使用其模型进行预测。

# 2.列存储缩减

[^](#bd3e)

![](img/4c2aa5249ea4b031f1b6b829679e0a55.png)

Photo by [Lisa H](https://unsplash.com/@lh_photography?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

```
# I don't know who the original author of this function is,
# but you can use this function to **reduce memory**
# **consumption** **by** **60-70%!****def** *reduce_mem_usage*(df):
    """ 
    iterate through all the columns of a dataframe and 
    modify the data type to reduce memory usage.        
    """
    start_mem = df.memory_usage().sum() / 1024**2
    **print**(('Memory usage of dataframe is {:.2f}' 
                     'MB').format(start_mem))

    **for** col in df.columns:
        col_type = df[col].dtype

        **if** col_type != object:
            c_min = df[col].min()
            c_max = df[col].max()
            **if** str(col_type)[:3] == 'int'**:**
                **if** c_min > np.iinfo(np.int8).min and c_max <\
                  np.iinfo(np.int8).max:
                    df[col] = df[col].astype(np.int8)
                **elif** c_min > np.iinfo(np.int16).min and c_max <\
                   np.iinfo(np.int16).max:
                    df[col] = df[col].astype(np.int16)
                **elif** c_min > np.iinfo(np.int32).min and c_max <\
                   np.iinfo(np.int32).max:
                    df[col] = df[col].astype(np.int32)
                **elif** c_min > np.iinfo(np.int64).min and c_max <\
                   np.iinfo(np.int64).max:
                    df[col] = df[col].astype(np.int64)  
            **else**:
                **if** c_min > np.finfo(np.float16).min and c_max <\
                   np.finfo(np.float16).max:
                    df[col] = df[col].astype(np.float16)
                **elif** c_min > np.finfo(np.float32).min and c_max <\
                   np.finfo(np.float32).max:
                    df[col] = df[col].astype(np.float32)
                **else**:
                    df[col] = df[col].astype(np.float64)
        **else**:
            df[col] = df[col].astype('category') end_mem = df.memory_usage().sum() / 1024**2
    **print**(('Memory usage after optimization is: {:.2f}' 
                              'MB').format(end_mem))
    **print**('Decreased by {:.1f}%'.format(100 * (start_mem - end_mem) 
                                             / start_mem))

    **return** df
```

坦克

# 3.文件类型

[^](#bd3e)

![](img/3f7ecbbae1394563cb7c1fbff4ca9402.png)

Photo by [Kelly Sikkema](https://unsplash.com/@kellysikkema?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

HDF5、拼花地板等。

> *注:如果你不想落入这一节，你可以只看 HDF5。这应该足够了。转到下一部分。*

[分层数据格式—维基百科](https://en.wikipedia.org/wiki/Hierarchical_Data_Format) (HDF5)

[阿帕奇拼花地板](https://www.youtube.com/watch?v=rVC9F1y38oU&list=PLbk_EDDIZpfa-nAodPK_fxwbPNLyixhGQ&index=8&t=0s)

[Hadoop 文件格式:它不再仅仅是 CSV——Kevin Haas](https://community.hitachivantara.com/community/products-and-solutions/pentaho/blog/2017/11/07/hadoop-file-formats-its-not-just-csv-anymore)(不要在阅读 Hadoop 后被吓跑，您可以只使用 Parquet 格式并通过使用`pd.DataFrame.to_parquet`和`pd.DataFrame.read_parquet`方法。)

还有，如果你有兴趣: [Parquet vs Avro](https://www.youtube.com/watch?v=sLuHzdMGFNA) 【无论如何我都会推荐你去看。]

坦克

# 4.数据探索 [^](#bd3e)

![](img/bc39fb8e2b834318c4fda11866ead6d8.png)

Photo by [Panos Sakalakis](https://unsplash.com/@meymigrou?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

首先，我们将通过使用以下命令查看前几行来了解我们的数据是什么样子的:

```
part = pd.read_csv("train.csv.zip", nrows=10)
part.head()
```

通过这个，你将有关于不同的列是如何构成的，如何处理每个列等的基本信息。列出不同类型的栏目，如`numerical_columns`、`obj_columns`、`dictionary_columns`等。它将保存所有相应的列。

现在，为了研究数据，我们将像这样逐列进行:

```
**#** For dictionary columns you can do:# 'idx' is index of corresponding column in DataFrame.
# You can find it by using np.where(col==df.columns)for col in dictionary_columns:
    df = pd.read_csv("train.csv.zip", usecols = [idx], converters={col: json.loads})
    column_as_df = json_normalize(df[col])
    # ... plot each column ...
    # ... check if you want to drop any column ... 
```

您可以将所有列名作为键的字典和应用于它的方法保存在一个列表中，作为该列的管道。你也可以把你的字典保存起来，以备将来使用:

```
with open("preprocessing_pipeline.pickle", "wb") as fle:
  pickle.dump(preprocessing_pipeline, fle)
```

如果其中一列对您的内存来说太大，实际上我上面提到的 kaggle 竞赛中的一行就是这种情况。你甚至可以逐步打开一列，并可以做一些基本的事情，如计算平均值，标准差等。手动。或者你也可以用`Dask`来代替它，用和`pandas`几乎一样的 API 来计算它们。参见最后一节`Dask`。

# 5.预处理 [^](#bd3e)

![](img/ea0733752a594d5dc78342bf5b09eae5.png)

Photo by [rawpixel](https://unsplash.com/@rawpixel?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

对于预处理数据，我们将使用我们之前创建的字典，它包含关于我们希望保留哪些列(作为键)以及对每列应用什么方法(作为值)的信息，以创建一个方法。

在增量学习过程中，每一批数据都会调用这个方法。

现在这里要注意的一件事是我们安装了方法(比如 *LabelEncoder* 的、*scalar*的等等。)在探索整个数据列的过程中，我们将在这里的每个增量步骤中使用它来转换数据。因为，在每一批中，可能会有一些数据丢失，如果我们使用了不同的*标签编码器*、*标量*等。对于每一批，这些方法不会给出相同类别的相同结果。这就是为什么我们在探索过程中已经安装了整个列。

以下是预处理数据的方法:

```
def preprocess(df):
  df.reset_index(drop=True, inplace=True)

  # For dict columns:
  for col in dict_columns:
    col_df = json_normalize(df[col])
    # json.loads during pd.read_csv to convert string to dict.                                
    col_df.columns = [f"{col}.{subcolumn}" for subcolumn in col_df.columns]
    # Select all columns which we selected before.
    selected_columns = [c for c in dictionary.keys() if c in col_df.columns()]
    to_drop = [c for c in col_df.columns if not in selected_columns]

    # Drop all previously unselected columns.
    col_df = col_df.drop(to_drop, axis=1)                                       

    df = df.drop(col, axis=1).merge(col_df, right_index=True, left_index=True)

  # And so on...

  # And then apply all Scalars, LabelEncoder's to all columns selected...

  return df
```

# 6.增量学习 [^](#bd3e)

![](img/69316806a29405a548bc0c99caefa83d.png)

Photo by [Bruno Nascimento](https://unsplash.com/@bruno_nascimento?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

要使用`pandas`增量读取数据文件，您必须使用一个参数`chunksize`，该参数指定一次要读/写的行数。

```
incremental_dataframe = pd.read_csv("train.csv",
                        chunksize=100000) **#** Number of lines to read.# This method will return a sequential file reader (TextFileReader)
# reading 'chunksize' lines every time. To read file from 
# starting again, you will have to call this method again.
```

然后你可以使用`XGBoost` [](#8633)或`LightGBM`对你的数据进行增量训练。对于`LightGBM`,你必须向它的`.train`方法传递一个参数`keep_training_booster=True`,向`XGBoost`的`.train`方法传递三个参数。

```
**#** First one necessary for incremental learning:
lgb_params = {
  'keep_training_booster': True,
  'objective': 'regression',
  'verbosity': 100,
}**#** First three are for incremental learning:
xgb_params = {
  'update':'refresh',
  'process_type': 'update',
  'refresh_leaf': True,
  'silent': False,
  }
```

在每一步中，我们将保存我们的估计值，然后在下一步中将其作为参数传递。

```
**#** For saving regressor for next use.
lgb_estimator = None
xgb_estimator = Nonefor df in incremental_dataframe:
  df = preprocess(df)

  xtrain, ytrain, xvalid, yvalid = # Split data as you like

  lgb_estimator = lgb.train(lgb_params,
                         **#** Pass partially trained model:
                         init_model=lgb_estimator,
                         train_set=lgb.Dataset(xtrain, ytrain),
                         valid_sets=lgb.Dataset(xvalid, yvalid),
                         num_boost_round=10)

  xgb_model = xgb.train(xgb_params, 
                        dtrain=xgb.DMatrix(xtrain, ytrain),
                        evals=(xgb.DMatrix(xvalid, yvalid),"Valid"),
                        **#** Pass partially trained model:
                        xgb_model = xgb_estimator)

  del df, xtrain, ytrain, xvalid, yvalid
  gc.collect()
```

`CatBoost`的增量学习法正在进行中。[](#6942)

为了加快速度，如果您的块仍然足够大，您可以使用`Python`的`multiprocessing`库函数来并行化您的预处理方法，如下所示:

```
n_jobs = 4
for df in incremental_dataframe:
  p = Pool(n_jobs)
  f_ = p.map(preprocess, np.array_split(df, n_jobs))
  f_ = pd.concat(f_, axis=0, ignore_index=True)
  p.close()
  p.join()

  # And then your model training ...
```

关于`Python`中并行编程的介绍，请在这里阅读我的帖子。

# 7.达斯克 [^](#bd3e)

![](img/d79afb98d3f16cda5ffb456632bf2b79.png)

Photo by [Nick Fewings](https://unsplash.com/@jannerboy62?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

> Dask 有助于以顺序并行的方式利用大数据资源。

关于`Dask`的介绍，请在这里阅读我的帖子。

您可以在这里以类似的方式应用来自`pandas` API 的函数。您可以检查是否有这样的空值:

```
df.isnull().sum().compute()
```

要缩放列，可以将它们转换为数组，并以类似的方式使用其 Scaler 函数:

```
rsc = dask_ml.preprocessing.RobustScaler()
result = rsc.fit_transform(X[:,i].reshape(-1, 1)) # for ith column
```

处理 JSON 或任何其他半结构化数据，如日志文件等。你可以使用`Dask`的`Bag`容器提供的函数。

```
df[key] = df[dict_col].to_bag().pluck(key).to_dataframe().iloc[:,0]
```

在预处理之后，你可以使用`Dask`的一个模型来训练你的数据。

完整代码请阅读 Jupyter 笔记本[中的`Dask`部分，点击这里](https://nbviewer.jupyter.org/github/PuneetGrov3r/MediumPosts/blob/master/Tackle/BigData-IncrementalLearningAndDask.ipynb#Method-2:-Using-Dask:)。

```
**Note:** You should only use Dask in case of Big Data, where it is not able to fit in your memory. Otherwise in-memory learning with pandas and sklearn will be lot faster.**Note: (Local Cluster)**
You can perform almost any BigData related query/tasks with the help of LocalCluster. You can, specifically, use 'memory_limit' parameter to constrict Dask's memory usage to a specific amount. 
Also, at times you might notice that **Dask** is exceeding memory use, even though it is dividing tasks. It could be happening to you because of the function you are trying to use on your **dataset** wants most of your data for processing, and multiprocessing can make things worse as all workers might try to copy **dataset** to memory. This can happen in aggregating cases.In these cases you can use **Dask.distributed.LocalCluster** parameters and pass them to **Client**() to make a **LocalCluster** using cores of your Local machines.**from** dask.distributed **import** Client, LocalCluster
client = **Client**(n_workers=1, threads_per_worker=1, processes=False,
                memory_limit='25GB', scheduler_port=0, 
                silence_logs=False, diagnostics_port=0)
client'scheduler_port=0' and 'diagnostics_port=0' will choose random port number for this particular client. With 'processes=False' **dask**'s client won't copy dataset, which would have happened for every process you might have made.
You can tune your client as per your needs or limitations, and for more info you can look into parameters of **LocalCluster.** You can also use multiple clients on same machine at different ports.
```

# 8.延伸阅读 [^](#bd3e)

1.  [https://www . ka ggle . com/mlisovyi/bigdata-dask-pandas-flat-JSON-trim-data-upd](https://www.kaggle.com/mlisovyi/bigdata-dask-pandas-flat-json-trim-data-upd)
2.  [*https://github . com/dmlc/xgboost/issues/3055 # issue comment-359505122*](https://github.com/dmlc/xgboost/issues/3055#issuecomment-359505122)
3.  [https://www . ka ggle . com/ogrellier/create-extracted-JSON-fields-dataset](https://www.kaggle.com/ogrellier/create-extracted-json-fields-dataset)
4.  [https://github . com/Microsoft/light GBM/blob/master/examples/python-guide/advanced _ example . py](https://github.com/Microsoft/LightGBM/blob/master/examples/python-guide/advanced_example.py)
5.  [将数据帧内存大小减少约 65% | Kaggle](https://www.kaggle.com/arjanso/reducing-dataframe-memory-size-by-65)
6.  [机器学习处理大数据文件的 7 种方法——机器学习掌握](https://machinelearningmastery.com/large-data-files-machine-learning/)

# 9.参考文献 [^](#bd3e)

1.  [https://www . ka ggle . com/mlisovyi/bigdata-dask-pandas-flat-JSON-trim-data-upd](https://www.kaggle.com/mlisovyi/bigdata-dask-pandas-flat-json-trim-data-upd)
2.  [https://www . ka ggle . com/ogrellier/create-extracted-JSON-fields-dataset](https://www.kaggle.com/ogrellier/create-extracted-json-fields-dataset)
3.  [https://gist . github . com/goraj/6 df 8 f 22 a 49534 e 042804 a 299d 81 bf2d 6](https://gist.github.com/goraj/6df8f22a49534e042804a299d81bf2d6)
4.  [https://github.com/dmlc/xgboost/issues/3055](https://github.com/dmlc/xgboost/issues/3055)
5.  [https://github.com/catboost/catboost/issues/464](https://github.com/catboost/catboost/issues/464)
6.  https://github.com/Microsoft/LightGBM/issues/987
7.  [https://gist . github . com/ylogx/53 fef 94 cc 61d 6a 3 e 9 B3 EB 900482 f 41 e 0](https://gist.github.com/ylogx/53fef94cc61d6a3e9b3eb900482f41e0)

```
Suggestions and reviews are welcome.
Thank you for reading!
```

签名:

![](img/ca01c1d315400c09978fb5e62da01d87.png)