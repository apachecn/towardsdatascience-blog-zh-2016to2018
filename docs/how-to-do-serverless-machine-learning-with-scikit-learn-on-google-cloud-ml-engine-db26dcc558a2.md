# 如何使用 scikit 进行无服务器机器学习-在 Google Cloud ML 引擎上学习

> 原文：<https://towardsdatascience.com/how-to-do-serverless-machine-learning-with-scikit-learn-on-google-cloud-ml-engine-db26dcc558a2?source=collection_archive---------9----------------------->

在 Google 云平台上，Cloud ML Engine 提供无服务器的机器学习，用于训练、超参数优化和预测。直到最近，这还只是针对 TensorFlow 的。不过最近，该团队已经为 scikit-learn 实现了所有三种功能。在这篇文章中，我将带它兜一圈。

问题是在给定一些怀孕信息的情况下预测婴儿的体重。这是我用 TensorFlow 端到端解决的一个问题，在我为 GCP NEXT 2018 开发的训练营中。现在让我来谈谈 scikit-learn。跟着我一起看看 GitHub 上的这个 [Jupyter 笔记本。](https://github.com/GoogleCloudPlatform/training-data-analyst/blob/master/blogs/sklearn/babyweight_skl.ipynb)

# 输入数据，从 BigQuery 到 Pandas

输入数据在 BigQuery 中，我将整个数据集的 1/1000 放入熊猫数据帧:

```
**def** query_to_dataframe(query):
  **import** **pandas** **as** **pd**
  **import** **pkgutil**
  privatekey = pkgutil.get_data(KEYDIR, 'privatekey.json')
  **print**(privatekey[:200])
  **return** pd.read_gbq(query,
                     project_id=PROJECT,
                     dialect='standard',
                     private_key=privatekey)

**def** create_dataframes(frac):  
  *# small dataset to fit into memory*
  **if** frac > 0 **and** frac < 1:
    sample = " AND RAND() < {}".format(frac)
  **else**:
    sample = ""

  train_query, eval_query = create_queries()
  train_query = "{} {}".format(train_query, sample)
  eval_query =  "{} {}".format(eval_query, sample)

  train_df = query_to_dataframe(train_query)
  eval_df = query_to_dataframe(eval_query)
  **return** train_df, eval_dftrain_df, eval_df = create_dataframes(0.001)
```

如果我使用较大的虚拟机，就有可能使用完整的数据集，我将在最后一步进行服务培训时这样做。然而，对于开发模型来说，拥有一个小的数据集是很有帮助的。

# 培训 sci kit-学习模型

为了训练模型，我首先编写了一个函数来获取用于训练的 x 和 y(我将它们称为特征和标签):

```
**def** input_fn(indf):
  **import** **copy**
  **import** **pandas** **as** **pd**
  df = copy.deepcopy(indf)

  *# one-hot encode the categorical columns*
  df["plurality"] = df["plurality"].astype(pd.api.types.CategoricalDtype(
                    categories=["Single","Multiple","1","2","3","4","5"]))
  df["is_male"] = df["is_male"].astype(pd.api.types.CategoricalDtype(
                  categories=["Unknown","false","true"]))
  *# features, label*
  label = df['label']
  **del** df['label']
  features = pd.get_dummies(df)
  **return** features, label
```

然后，我创建了一个 RandomForestRegressor，并将 x 和 y 传递给它的 fit()方法:

```
**from** **sklearn.ensemble** **import** RandomForestRegressor
estimator = RandomForestRegressor(max_depth=5, n_estimators=100, random_state=0)
estimator.fit(train_x, train_y)
```

通过在评估数据帧上调用 predict()并计算 RMSE，可以评估模型的性能:

```
**import** **numpy** **as** **np**
eval_x, eval_y = input_fn(eval_df)
eval_pred = estimator.predict(eval_x)
**print**(eval_pred[1000:1005])
**print**(eval_y[1000:1005])
**print**(np.sqrt(np.mean((eval_pred-eval_y)*(eval_pred-eval_y))))
```

# 将教练打包成一个包

代码运行后，我将以下 Jupyter 魔术添加到笔记本的单元格中，将单元格写出到 Python 文件中:

```
*#%writefile -a babyweight/trainer/model.py*
```

我还将许多硬编码的数字(比如随机森林中的树的数量)作为命令行参数。通过让它们成为命令行参数，我可以对它们进行超参数调优。

我在本地测试了这个包，仍然是在 1/1000 的数据集上:

```
%bash
export PYTHONPATH=${PYTHONPATH}:${PWD}/babyweight
python -m trainer.task \
   --bucket=${BUCKET} --frac=0.001 --job-dir=gs://${BUCKET}/babyweight/sklearn --projectId $PROJECT
```

# 无服务器培训

关于 Cloud ML Engine 的培训就像提交 Python 包一样简单:

```
RUNTIME_VERSION="1.8"
PYTHON_VERSION="2.7"
JOB_NAME=babyweight_skl_$(date +"%Y%m**%d**_%H%M%S")
JOB_DIR="gs://$BUCKET/babyweight/sklearn/${JOBNAME}"

gcloud ml-engine jobs submit training $JOB_NAME \
  --job-dir $JOB_DIR \
  --package-path $(pwd)/babyweight/trainer \
  --module-name trainer.task \
  --region us-central1 \
  --runtime-version=$RUNTIME_VERSION \
  --python-version=$PYTHON_VERSION \
  -- \
  --bucket=${BUCKET} --frac=0.1 --projectId $PROJECT
```

这一次，我在全部数据集的 1/10 上进行训练。为了在完整数据集上训练，我需要一台更大的机器。我可以通过更改为自定义层来做到这一点:

```
%writefile largemachine.yaml
trainingInput:
  scaleTier: CUSTOM
  masterType: large_model
```

并传入上面的配置文件:

```
RUNTIME_VERSION="1.8"
PYTHON_VERSION="2.7"
JOB_NAME=babyweight_skl_$(date +"%Y%m**%d**_%H%M%S")
JOB_DIR="gs://$BUCKET/babyweight/sklearn/${JOBNAME}"

gcloud ml-engine jobs submit training $JOB_NAME \
  --job-dir $JOB_DIR \
  --package-path $(pwd)/babyweight/trainer \
  --module-name trainer.task \
  --region us-central1 \
  --runtime-version=$RUNTIME_VERSION \
  --python-version=$PYTHON_VERSION \
  --scale-tier=CUSTOM \
  --config=largemachine.yaml \
  -- \
  --bucket=${BUCKET} --frac=1 --projectId $PROJECT --maxDepth 8 --numTrees 90
```

我如何获得最大深度和最大树数？为此，我做了超参数调整。

# 超参数调谐

对于 ML 引擎中的超参数调整，在评估后写出一个摘要度量。这是代码，假设您有一个名为 rmse 的变量，它保存最终的评估度量:

```
## this is for hyperparameter tuning
hpt = hypertune.HyperTune()
hpt.report_hyperparameter_tuning_metric(
    hyperparameter_metric_tag=’rmse’,
    metric_value=rmse,
    global_step=0)
```

要提交超参数优化作业，请使用要优化的参数编写一个配置文件:

```
%writefile hyperparam.yaml
trainingInput:
  hyperparameters:
    goal: MINIMIZE
    maxTrials: 100
    maxParallelTrials: 5
    hyperparameterMetricTag: rmse
    params:
    - parameterName: maxDepth
      type: INTEGER
      minValue: 2
      maxValue: 8
      scaleType: UNIT_LINEAR_SCALE
    - parameterName: numTrees
      type: INTEGER
      minValue: 50
      maxValue: 150
      scaleType: UNIT_LINEAR_SCALE
```

然后，通过以下方式将其传递给 ML 引擎:

```
--config=hyperparam.yaml
```

试验的输出将开始填充，如果您查看 GCP web 控制台，将会列出具有最低 RMSE 的试验及其运行时参数。

# 部署模型

经过培训后，可以部署 scitkit 学习模型:

```
gcloud alpha ml-engine versions create ${MODEL_VERSION} --model ${MODEL_NAME} --origin ${MODEL_LOCATION} \
    --framework SCIKIT_LEARN --runtime-version 1.8  --python-version=2.7
```

部署的模型有一个端点，您可以访问它来获得预测:

```
**from** **googleapiclient** **import** discovery
**from** **oauth2client.client** **import** GoogleCredentials
**import** **json**

credentials = GoogleCredentials.get_application_default()
api = discovery.build('ml', 'v1', credentials=credentials)

request_data = {'instances':
  *# [u'mother_age', u'gestation_weeks', u'is_male_Unknown', u'is_male_0',*
  *#     u'is_male_1', u'plurality_Single', u'plurality_Multiple',*
  *#     u'plurality_1', u'plurality_2', u'plurality_3', u'plurality_4',*
  *#     u'plurality_5']*
  [[24, 38, 1, 0, 0, 1, 0, 0, 0, 0, 0, 0], 
   [34, 39, 0, 0, 1, 0, 0, 1, 0, 0, 0, 0]]
}

parent = 'projects/**%s**/models/**%s**/versions/**%s**' % (PROJECT, 'babyweight', 'skl')
response = api.projects().predict(body=request_data, name=parent).execute()
**print** "response={0}".format(response)
```

目前，请求必须是文本行的形式，并且必须是预处理的数据。自然，这就对 scikit-learn 模型的可操作性提出了一些警告。

再说一遍，我的[完整代码在 GitHub](https://github.com/GoogleCloudPlatform/training-data-analyst/blob/master/blogs/sklearn/babyweight_skl.ipynb) 上。编码快乐！