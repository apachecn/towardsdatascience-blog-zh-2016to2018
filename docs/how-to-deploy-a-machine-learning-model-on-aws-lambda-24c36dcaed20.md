# 如何在 AWS Lambda 上部署机器学习模型

> 原文：<https://towardsdatascience.com/how-to-deploy-a-machine-learning-model-on-aws-lambda-24c36dcaed20?source=collection_archive---------0----------------------->

![](img/d7930344e8dd5c427ecfe11b931965ab.png)

在本教程中，我将带您在 AWS Lambda 上部署一个机器学习模型。我们的模型也可以通过使用 Amazon API Gateway 的 API 进行访问。最终，我们将得到一个真正无服务器系统的完美配方。让我们直入主题吧。

## **将你的模型上传到亚马逊 S3**

第一步是将你的模型上传到亚马逊 S3。如果你不熟悉亚马逊 S3，请访问此链接开始:[http://docs . AWS . Amazon . com/amazons 3/latest/UG/uploadingobjectsintoamazons 3 . html](http://docs.aws.amazon.com/AmazonS3/latest/UG/UploadingObjectsintoAmazonS3.html)

## **创建虚拟环境**

当部署到 AWS Lambda 时，您需要上传一个包含代码和依赖项的虚拟环境。在项目文件夹中，编写以下几行。为了避免冲突，你应该**而不是**将你的虚拟环境命名为你的项目文件夹。

```
pip install virtualenv
virtualenv your_virtual_environment_name
```

要启动您的虚拟环境:

```
source your_virtual_environment_name/bin/activate
```

## **创建烧瓶 API**

我们需要一个基本的 [Flask](http://flask.pocoo.org/) 应用程序来处理我们的请求。该应用程序将从亚马逊 S3 加载我们的模型，并返回预测。首先创建一个名为 predictions.py 的文件，然后添加下面的代码。确保在进入下一步之前修改 *BUCKET_NAME* 和 *MODEL_FILE_NAME* 。

## 从亚马逊 S3 加载我们的模型

使用 [Boto](https://github.com/boto/boto) 库，我们加载之前保存在亚马逊 S3 上的模型，并保存到磁盘以备后用。

## 创建预测

最后但并非最不重要的是，我们准备得到一些预测。predict 方法接收发送给 API 的数据作为参数。您将需要添加一些代码来处理数据，以匹配您的模型。

最终文件应该是这样的

## 测试 API

*   要运行您的应用程序，您必须创建一个环境变量来告诉 [Flask](http://flask.pocoo.org/) 要执行哪些文件:

```
export FLASK_APP=predictions.py
```

在将代码部署到 AWS 之前，您应该确保代码在本地工作。

安装[烧瓶](http://flask.pocoo.org/)，然后运行第二个命令来启动你的应用程序:

```
pip install Flask
flask run
```

打开终端，测试你的预测:

```
curl -d '{"payload": "Insert the data needed for your model to make predictions"}' http://127.0.0.1:5000/
```

当一切正常时，你可以进入下一步。

## 部署到 AWS Lambda

我们将使用一个名为 [*Zappa*](https://github.com/Miserlou/Zappa) 的库，而不是手动创建和配置 AWS Lambda 和 API 网关。

> Zappa 使在 AWS Lambda + API Gateway 上构建和部署所有 Python WSGI 应用程序变得非常容易。

**重要提示:在部署之前，确保配置您的 AWS 凭证。更多信息请访问:**[http://docs . AWS . Amazon . com/CLI/latest/user guide/CLI-chap-getting-started . html](http://docs.aws.amazon.com/cli/latest/userguide/cli-chap-getting-started.html)

在您的虚拟环境中，安装所需的软件包:

```
pip install zappa sklearn boto numpy scipy
```

然后，初始化 [Zappa](https://github.com/Miserlou/Zappa) 。当被问及 *app 函数名*时，写下 *predictions.app.* (或者，你的 [Flask](http://flask.pocoo.org/) app +的名字。app)

```
zappa init
```

AWS Lambda 要求您的环境最大大小为 50mb，但我们的打包环境大约为 100mb。幸运的是，Lambda 可以从亚马逊 S3 加载代码，而不会有太大的性能损失(只有几毫秒)。

要激活此功能，您必须在 zappa_settings.json 中添加一个新行

```
"slim_handler": true
```

您现在可以开始部署了。在初始化 Zappa 时，使用带有您选择的环境名称的 deploy 命令。

```
zappa deploy your-environment-name
```

完成后，您将在最后一条消息中看到您的 API URL。它将看起来像“https://abcdefg . execute-API . us-east-1 . Amazon AWS . com/your-env-name”

您可以使用 cURL 测试您的部署:

```
curl -d '{"payload": "Insert the data needed for your model to make predictions"}' https://your-api-url.com/your-env
```

恭喜你！您已经在无服务器基础设施上部署了您的模型。

如果你喜欢这篇文章，请一定留下你的关注。我也有一些关于机器学习的很棒的文章即将发表，敬请关注！

你也可以在推特上联系到我。