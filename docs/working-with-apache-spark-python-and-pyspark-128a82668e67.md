# Apache Spark、Python 和 PySpark 入门

> 原文：<https://towardsdatascience.com/working-with-apache-spark-python-and-pyspark-128a82668e67?source=collection_archive---------4----------------------->

![](img/c5c8b07a38ddc2ed6903adc6976c5332.png)

Image Source: www.[spark.apache.org](https://spark.apache.org/)

本文是 Apache Spark 单节点安装的快速指南，以及如何使用 Spark python 库 PySpark。

# 1.环境

*   Hadoop 版本:3.1.0
*   阿帕奇卡夫卡版本:1.1.1
*   操作系统:Ubuntu 16.04
*   Java 版本:Java 8

# 2.先决条件

Apache Spark 需要 Java。要确保安装了 Java，首先更新操作系统，然后尝试安装它:

```
sudo apt-get updatesudo apt-get –y upgradesudo add-apt-repository -y ppa:webupd8team/javasudo apt-get install oracle-java8-installer
```

# 3.安装 Apache Spark

# 3.1.下载并安装 Spark

首先，我们需要为 apache Spark 创建一个目录。

```
sudo mkdir /opt/spark
```

然后，我们需要下载 apache spark 二进制包。

```
**wget “**http://www-eu.apache.org/dist/spark/spark-2.3.1/spark-2.3.1-bin-hadoop2.7.tgz**”**
```

接下来，我们需要将 apache spark 文件解压缩到/opt/spark 目录中

```
sudo tar -xzvf spark-2.3.1-bin-hadoop2.7.tgz --directory=/opt/spark -- strip 1
```

# 3.2.配置 Apache Spark

当 Spark 启动作业时，它会将其 jar 文件传输到 HDFS，这样任何正在工作的机器都可以使用这些文件。这些文件对于较小的作业来说是一笔很大的开销，所以我把它们打包，复制到 HDFS，并告诉 Spark 它不再需要复制它们了。

```
jar cv0f ~/spark-libs.jar -C /opt/spark/jars/ .hdfs dfs -mkdir /spark-libshdfs dfs -put ~/spark-libs.jar /spark-libs/
```

复制文件后，我们必须告诉 spark 忽略从 Spark 默认配置文件中复制 jar 文件:

```
sudo gedit /opt/spark/conf/spark-defaults.conf
```

添加以下几行:

```
spark.master spark://localhost:7077spark.yarn.preserve.staging.files truespark.yarn.archive hdfs:///spark-libs/spark-libs.jar
```

在本文中，我们将配置 Apache Spark 在单个节点上运行，因此它将只是 localhost:

```
sudo gedit /opt/spark/conf/slaves
```

确保它只包含值 localhost

在运行服务之前，我们必须打开。使用 gedit 的 bashrc 文件

```
sudo gedit ~/.bashrc
```

并添加以下几行

```
export SPARK_HOME=/opt/sparkexport SPARK_CONF_DIR=/opt/spark/confexport SPARK_MASTER_HOST=localhost
```

现在，我们必须运行 Apache Spark 服务:

```
sudo /opt/spark/sbin/start-master.shsudo /opt/spark/sbin/start-slaves.sh
```

# 4.安装 Python

# 4.1.获取最新的 Python 版本

Ubuntu 16.04 预装了 Python 3 和 Python 2。为了确保我们的版本是最新的，我们必须用 apt-get 更新和升级系统(在先决条件一节中提到):

```
sudo apt-get updatesudo apt-get -y upgrade
```

我们可以通过键入以下命令来检查系统中安装的 Python 3 的版本:

```
python3 –V
```

它必须返回 python 版本(例如:Python 3.5.2)

# 4.2.安装 Python 工具

要管理 Python 的软件包，我们必须安装 pip 实用程序:

```
sudo apt-get install -y python3-pip
```

还需要安装一些软件包和开发工具，以确保我们的编程环境有一个健壮的设置。

```
sudo apt-get install build-essential libssl-dev libffi-dev python-dev
```

# 4.3.营造环境

我们需要首先安装 venv 模块，它允许我们创建虚拟环境:

```
sudo apt-get install -y python3-venv
```

接下来，我们必须为我们的环境创建一个目录

```
mkdir testenv
```

现在，我们必须转到这个目录并创建环境(所有环境文件都将创建在一个名为 my_env 的目录中):

```
cd testenvpython3 -m venv my_env
```

我们完成了检查使用 ls my_env 创建的环境文件

要使用此环境，您需要激活它:

```
source my_env/bin/activate
```

# 5.使用 PySpark

# 5.1.配置

首先我们需要打开。bashrc 文件

```
sudo gedit ~/.bashrc
```

并添加以下几行:

```
export PYTHONPATH=/usr/lib/python3.5export PYSPARK_SUBMIT_ARGS=” -- master local[*] pyspark-shell”export PYSPARK_PYTHON=/usr/bin/python3.5
```

# 5.2.FindSpark 库

如果我们在机器上安装了 Apache Spark，我们就不需要在开发环境中安装 pyspark 库。我们需要安装 findspark 库，它负责定位随 apache Spark 一起安装的 pyspark 库。

```
pip3 install findspark
```

在每个 python 脚本文件中，我们必须添加以下行:

```
import findsparkfindspark.init()
```

# 5.3.PySpark 示例

# 5.3.1.阅读 HDFS 的作品

下面的脚本将读取存储在 hdfs 中的文件

```
import findsparkfindspark.init()from pyspark.sql import SparkSessionsparkSession = SparkSession.builder.appName(“example-pyspark-hdfs”).getOrCreate()df_load = sparkSession.read.csv(‘hdfs://localhost:9000/myfiles/myfilename’)df_load.show()
```

# 5.3.2.阅读阿帕奇·卡夫卡《消费者》

我们首先必须将 spark-streaming-Kafka-0–8-assembly _ 2.11–2 . 3 . 1 . jar 库添加到我们的 Apache spark jars 目录/opt/spark/jars 中。我们可以从 mvn 存储库下载:

-[https://mvn repository . com/artifact/org . Apache . spark/spark-streaming-Kafka-0-8-assembly _ 2.11/2 . 3 . 1](https://mvnrepository.com/artifact/org.apache.spark/spark-streaming-kafka-0-8-assembly_2.11/2.3.1)

以下代码从 Kafka 主题消费者那里读取消息，并逐行打印出来:

```
import findsparkfindspark.init()from kafka import KafkaConsumerfrom pyspark import SparkContextfrom pyspark.streaming import StreamingContextfrom pyspark.streaming.kafka import KafkaUtilsKAFKA_TOPIC = ‘KafkaTopicName’KAFKA_BROKERS = ‘localhost:9092’ZOOKEEPER = ‘localhost:2181’sc = SparkContext(‘local[*]’,’test’)ssc = StreamingContext(sc, 60)kafkaStream = KafkaUtils.createStream(ssc, ZOOKEEPER, ‘spark-streaming’, {KAFKA_TOPIC:1})lines = kafkaStream.map(lambda x: x[1])lines.pprint()ssc.start()ssc.awaitTermination()
```

# 6.文献学

[1] M. Litwintschik，“Hadoop 3 单节点安装指南”，2018 年 3 月 19 日。【在线】。可用:[http://tech . marksblogg . com/Hadoop-3-single-node-install-guide . html](http://tech.marksblogg.com/hadoop-3-single-node-install-guide.html.)【2018 年 6 月 1 日访问】。

[2] L. Tagiaferri，“如何在 Ubuntu 16.04 上安装 Python 3 并设置本地编程环境”，2017 年 12 月 20 日。【在线】。可用:[https://www . digital ocean . com/community/tutorials/how-to-install-python-3-and-set-up-a-local-programming-environment-on-Ubuntu-16-04。](https://www.digitalocean.com/community/tutorials/how-to-install-python-3-and-set-up-a-local-programming-environment-on-ubuntu-16-04.)【2018 年 08 月 01 日接入】。

[3]《Apache Spark 官方文档》，[在线]。可用:[https://spark.apache.org/docs/latest/.](https://spark.apache.org/docs/latest/.)【2018 年 8 月 5 日访问】。

[4]《堆栈溢出问答》[在线]。可用:[https://stackoverflow.com/.](https://stackoverflow.com/.)【2018 年 6 月 1 日获取】。

[5] A. GUPTA，“PySpark 中数据帧操作的完整指南”，2016 年 10 月 23 日。【在线】。可用:[https://www . analyticsvidhya . com/blog/2016/10/spark-data frame-and-operations/。](https://www.analyticsvidhya.com/blog/2016/10/spark-dataframe-and-operations/.)【2018 年 8 月 14 日进入】。