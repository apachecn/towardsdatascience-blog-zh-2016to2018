# 如何使用 Apache Livy 在 AWS EMR 中运行 spark 批处理作业

> 原文：<https://towardsdatascience.com/how-to-do-better-deployments-of-spark-batch-jobs-to-aws-emr-using-apache-livy-adc2417f0d8b?source=collection_archive---------7----------------------->

![](img/c0d341142899e1d62d34e971b5b5e101.png)

在本文中，我们将讨论在 [Apache Livy](https://livy.incubator.apache.org) 的帮助下，使用 rest 接口在 [AWS EMR](https://aws.amazon.com/emr/) 上运行 spark 作业。

我们将完成以下步骤:

*   创建一个简单的批处理作业，从 Cassandra 读取数据并将结果写入 S3 的 parquet
*   建造罐子并把它存放在 S3
*   提交作业并等待它通过 livy 完成

*什么是阿帕奇李维？*

> Apache Livy 是一个通过 REST 接口与 Spark 集群轻松交互的服务。它支持 Spark 作业或 Spark 代码片段的轻松提交、同步或异步结果检索，以及 Spark 上下文管理，所有这些都通过一个简单的 REST 接口或 RPC 客户端库实现。Apache Livy 还简化了 Spark 和应用服务器之间的交互，从而支持将 Spark 用于交互式 web/移动应用程序。

*你为什么会使用它？*

Apache livy 让我们的生活更轻松。我们不需要使用 EMR 步骤或 ssh 进入集群并运行 spark submit。我们只是使用了一个很好的 REST 接口。

让我们看看它的实际效果。

## 步骤 1:构建简单的批处理作业

批处理作业将是一个 scala 应用程序。

在我们的应用程序中，我们首先构建 spark 会话，并确保我们可以连接到 Cassandra 集群:

```
val sparkConf = new SparkConf()sparkConf.set("spark.cassandra.connection.host", "YOUR_CASSANDRA_HOST")
sparkConf.set("spark.cassandra.connection.port", "YOUR_CASSANDRA_PORT")val spark: SparkSession = SparkSession.builder()
      .appName("RunBatchJob")
      .config(sparkConf)
      .getOrCreate()
```

我们还需要确保能够访问我们要写入数据的 S3 存储桶:

```
spark.sparkContext.hadoopConfiguration.set("fs.s3a.acl.default", "BucketOwnerFullControl")
```

我们可以从数据库中读取输入数据:

```
val input = spark.read
      .format("org.apache.spark.sql.cassandra")
      .options(Map("table" -> "my_table", "keyspace" -> "my_schema"))
      .load()
```

Cassandra 桌子的结构非常简单:

```
CREATE TABLE my_schema.my_table (
    id1 text PRIMARY KEY,
    id2 text,
    id3 text,
    id4 text
);
```

最后，我们只需将数据帧写入 S3 的一个拼花文件，同时删除重复的行:

```
input
      .dropDuplicates
      .write
      .mode("append")
      .parquet("s3a://your_bucket/your_preffix/")
```

## 步骤 2:将 jar 部署到 S3

好，现在我们有了一个应用程序。我们需要把它装进一个[大罐子](https://stackoverflow.com/questions/19150811/what-is-a-fat-jar)里，然后复制到 S3。这样，远程访问 jar(从 livy)就更容易了。

使用 [gradle](https://gradle.org) 管理我们的应用程序的依赖关系。在教程的最后，我会发布 github repo 的链接，在那里你可以找到 gradle 文件的完整代码和细节。我不会在这里讨论细节，因为这超出了本教程的范围。

要创建 fat jar，我们需要在应用程序的根目录下运行:

```
gradle shadowJar
```

生成后，我们可以使用 [aws cli](https://aws.amazon.com/cli/) 将其复制到 S3:

```
aws s3 cp build/libs/spark_batch_job-1.0-SNAPSHOT-shadow.jar s3://your_bucket/your_prefix/
```

一旦这样做了，我们终于可以从使用 livy 开始我们的 spark 工作了。

## 步骤 3:通过 Livy 提交作业

我们将使用一个简单的 python 脚本来运行我们的命令。主要功能非常简单:

```
def run_spark_job(master_dns):
    response = spark_submit(master_dns)
    track_statement_progress(master_dns, response)
```

它将首先提交作业，然后等待它完成。 *track_statement_progress* 步骤对于检测我们的作业是否成功运行非常有用。 *master_dns* 是 EMR 集群的地址。让我们更深入地研究我们各自的方法。

*spark_submit* 功能:

```
host = '[http://'](/') + master_dns + ':8999'data = {'className': "com.app.RunBatchJob", "conf":{"spark.hadoop.fs.s3a.impl":"org.apache.hadoop.fs.s3a.S3AFileSystem"}, 'file': "s3a://your_bucket/spark_batch_job-1.0-SNAPSHOT-shadow.jar"}headers = {'Content-Type': 'application/json'}response = requests.post(host + '/batches', data=json.dumps(data), headers=headers)
```

这只是使用请求库完成的一个 *post* 。在 EMR 上，livy 服务器运行在端口 **8999 上。**我们请求中的数据本质上是我们将提供给 *spark-submit* 命令的参数。我们需要使用 **/batches** 端点。这将告诉 livy 我们将提交一个批处理作业。

以及*跟踪声明进度*函数。这将每 10 秒轮询一次 livy 服务器，并检查应用程序的状态是否为*成功。*如果是，则作业已成功完成:

```
statement_status = ''
host = '[http://'](/') + master_dns + ':8999'
session_url = host + response_headers['location'].split('/statements', 1)[0]while statement_status != 'success':
        statement_url = host + response_headers['location']
        statement_response = requests.get(statement_url, headers={'Content-Type': 'application/json'})
        statement_status = statement_response.json()['state']
        logging.info('Statement status: ' + statement_status) lines = requests.get(session_url + '/log', headers={'Content-Type': 'application/json'}).json()['log']
        for line in lines:
            logging.info(line) if statement_status == 'dead':
            raise ValueError('Exception in the app caused it to be dead: ' + statement_status) if 'progress' in statement_response.json():
            logging.info('Progress: ' + str(statement_response.json()['progress']))
        time.sleep(10)
```

这是一个较长的函数，所以我将尝试一步一步地解释它。对于运行时间较长的作业，livy 会更改 url，因此我们需要存储最新的一个:

```
statement_url = host + response_headers['location']
```

然后，我们获取集群的当前状态并记录下来:

```
statement_response = requests.get(statement_url, headers={'Content-Type': 'application/json'})
statement_status = statement_response.json()['state']
logging.info('Statement status: ' + statement_status)
```

我们也正在获取应用程序的日志并显示它们。这对调试错误很有用:

```
lines = requests.get(session_url + '/log', headers={'Content-Type': 'application/json'}).json()['log']
for line in lines:
    logging.info(line)
```

如果我们有一个严重的异常，那么应用程序将会死亡。我们需要标记它并抛出一个异常:

```
if statement_status == 'dead':
    raise ValueError('Exception in the app caused it to be dead: ' + statement_status)
```

最后一步，我们记录进度并等待 10 秒钟，直到我们再次检查应用程序的状态:

```
if 'progress' in statement_response.json():
    logging.info('Progress: ' + str(statement_response.json()['progress']))
time.sleep(10)
```

这就是全部。代码在 github 上，可以从[这里](https://github.com/BogdanCojocar/medium-articles/tree/master/livy_batch_emr)访问。