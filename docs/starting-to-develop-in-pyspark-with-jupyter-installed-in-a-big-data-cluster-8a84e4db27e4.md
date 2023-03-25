# 开始在 PySpark 中开发，Jupyter 安装在一个大数据集群中

> 原文：<https://towardsdatascience.com/starting-to-develop-in-pyspark-with-jupyter-installed-in-a-big-data-cluster-8a84e4db27e4?source=collection_archive---------12----------------------->

众所周知，像 [Jupyter、](http://jupyter.org/) [Apache Zeppelin](https://zeppelin.apache.org/) 或最近推出的[云数据实验室](https://cloud.google.com/datalab/)和 [Jupyter 实验室](https://jupyterlab.readthedocs.io/en/stable/)这样的数据科学工具对于日常工作来说是必不可少的，那么如何将轻松开发模型的能力与大数据集群的计算能力结合起来呢？在本文中，我将分享在 GCP 的一个数据处理集群中开始使用 PySpark 的 Jupyter 笔记本的简单步骤。

![](img/316d58d22c2ceeb6183eb88fde35cc5c.png)

Final goal

![](img/b0b846b8db321140a2fa08f24ddf65ec.png)

**先决条件**

1.拥有一个谷歌云账户(只需登录你的 Gmail，就能自动获得一年 300 美元的信用额度)[1]

2.用您喜欢的名称创建一个新项目

![](img/85a05bf2f99be13b3eae0e70ffd76684.png)

**步骤**

1.  为了使部署更容易，我将使用一个[测试功能](https://cloud.google.com/dataproc/docs/tutorials/jupyter-notebook)，它只能在通过 Google Cloud Shell 创建数据处理集群时应用。对于我们的集群，我们需要定义许多特性，比如工作人员的数量、master 的高可用性、RAM 和硬盘驱动器的数量等。为了简单起见，我建议通过 UI 模拟集群的创建。首先我们需要启用 Dataproc(图 1 和图 2)。

![](img/eadb2b57e8d3cdd1690b21c24ab0e15d.png)

Figure 1 Enable Dataproc API I

![](img/dadb0ed66e1be43b8c64b06d2ab3b224.png)

Figure 2 Enable Dataproc API II

2.使用您自己的集群大小获得模拟创建过程的等效命令行*。我将设定基本规格:*

*   区域:全球
*   集群模式:标准
*   主节点:2 个 vCPUs、7.5GB 内存和 300 磁盘大小
*   工作节点:2 个 CPU、7.5GB 内存和 200 个磁盘大小

![](img/7084b867913a0ae2ca98b8905dcc3b61.png)

Simulate creating a cluster through UI

![](img/35102bc521d4b0cea9718ab981967995.png)

Basic specs

> **重要提示:您应该点击高级选项，将图像更改为 1.3 Debian 9，以使 beta 参数生效。**

![](img/038674c3327f4dc2ec04f4f0d81880b1.png)

To access click Advance options

![](img/d5d6a6bc7d9ae2ac92dd9c86ab64ee70.png)

Change to 1.3 Debian 9

3.获取等效命令行

![](img/5cfa1c88b663315181dae73f86566d94.png)

Click in command line

![](img/1c304471d5ca1f94345e489fd82c7499.png)

Copy the gcloud command

4.关闭模拟并单击以激活云壳

![](img/9558264578789417e1e39066a5e9af73.png)

Activate Cloud Shell

5.修改您的命令添加并运行(可能需要几分钟)

```
--optional-components=ANACONDA,JUPYTER
```

变化

***g cloud data proc****星团*至 ***gcloud beta dataproc 星团***

奔跑

*   **重要信息更改项目 id ctic-227716，**在控制台中用黄色书写的名称，同时将 cluster-ctic 重命名为您喜欢的名称(记住使用小写)

```
gcloud beta dataproc clusters create cluster-ctic --subnet default --zone us-west1-a --master-machine-type n1-standard-2 --master-boot-disk-size 300 --num-workers 2 --worker-machine-type n1-standard-2 --worker-boot-disk-size 200 --optional-components=ANACONDA,JUPYTER --image-version 1.3-deb9 --project ctic-227716
```

![](img/b87fd0545cf5dfac91c60a8f28ae2422.png)

running in shell

![](img/905b87c075e1bc20c628970684b8cca4.png)

cluster created

6.允许 Jupyter 端口的传入流量，在登录页面中搜索防火墙规则并创建一个规则。

![](img/b38752a3ae486ac145656c181b099693.png)

search Firewall rules VPC network

![](img/0419602abd47b6bc59971d38480d9d94.png)

click on create a rule

7.定义防火墙规则开放端口 8123 并保存。

![](img/d385d68f1282f6123af67e80bb1965a8.png)

parameters

![](img/74edafbffa6334d0ace5a027d2bf024f.png)

Rule working

8.检查您的所有节点是否都允许 http 和 https 流量，并添加在每个节点中创建的防火墙标记。

![](img/dc3d8e6b8344b9f37823271ae329bff9.png)

9.输入您的 Jupyter 笔记本！(您需要您的主 IP 并添加 jupyter 默认端口，例如[http://30.195 . XXX . xx:8123](http://30.195.xxx.xx:8123))

![](img/9a6076dcd7d644474f9c958bed835165.png)

get master´s IP

10.让我们创建我们的第一个 Pyspark 笔记本

![](img/243e4392db7b3d02a981a07cdb55ab4f.png)

create the first Pyspark notebook

11.验证运行良好

![](img/316d58d22c2ceeb6183eb88fde35cc5c.png)

**奖励:检查火花 UI**

*   要访问 Spark UI，您需要添加另一个防火墙规则，如步骤 7。打开端口 8088、4040、9870 和 4041。

![](img/7ae9304cf0c55c3adb09d4eb3994d750.png)

Create Spark UI rule

*   点击我们第一个笔记本中的 Spark UI 链接，您将看到一个 ERR_NAME_NOT_RESOLVED 错误，只需将 URL 替换为主 IP 即可

> 例如[http://3x . XXX . xx . x:8088/proxy/application _ 1542773664669 _ 0001](http://3x.xxx.xx.x:8088/proxy/application_1542773664669_0001)

![](img/09b483d4a141e800b5a50e544b50daaa.png)

Spark UI

**结论**

在本文中，我尝试在 Data Proc 集群中部署 Jupyter，使得在真实集群中使用 PySpark 更加方便。如果您对下一篇文章有任何问题或建议，请随意。

下一篇文章再见！快乐学习！

PS 如果你有任何问题，或者想要澄清一些事情，你可以在 [Twitter](https://twitter.com/thony_ac77) 和 [LinkedIn 上找到我。](https://www.linkedin.com/in/antoniocachuan/)如果你想了解 Apache Arrow 和 Apache Spark，我有一篇文章[**用一些例子温和地介绍了 Apache Arrow 与 Apache Spark 和 Pandas**](/a-gentle-introduction-to-apache-arrow-with-apache-spark-and-pandas-bb19ffe0ddae) 此外，今年还出版了一本了解 Apache Spark、Spark 的最佳书籍:[权威指南](https://amzn.to/2NQxTmZ)