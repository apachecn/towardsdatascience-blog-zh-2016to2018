# 如何在一小时内开发、部署和扩展一个数据科学算法！

> 原文：<https://towardsdatascience.com/how-to-develop-deploy-and-scale-a-data-science-algorithm-in-one-hour-6c43f2b78f1d?source=collection_archive---------3----------------------->

![](img/a8d4887aebe1098e43c034a042e20043.png)

这样你就有了一张数据表。您希望使用所有其他列中的值来预测一列中的值。您需要训练、测试、部署和生产(即，使其在 API 背后可用)一种算法，该算法将根据输入特征准确预测输出特征。我们开始吧。

在我们开始之前，你需要一个 [AWS](https://aws.amazon.com/) 账户。不要问问题，直接报名，现收现付，很便宜。其次，您还需要访问 Linux 终端。我用的是[云 9](https://c9.io) ，它太棒了，而且有一个免费选项。最后，用你的 AWS 证书在你的 Linux 终端上安装并设置[无服务器框架](https://serverless.com/)，这是开源的好日子。

第一步:去 [S3](https://aws.amazon.com/s3/) 上传你的 CSV 格式的数据。

第二步:在 AWS 上进入[亚马逊机器学习](https://aws.amazon.com/machine-learning/)，点击‘新建...’然后是“数据源和 ML 模型”。遵循要求的步骤(步骤不多，应该很简单，只需使用默认选项)。一旦完成，亚马逊机器学习将构建、训练和测试算法。

第三步:每个人都知道好的模型需要好的数据，“垃圾进垃圾出”等等。现在请评估您的模型，如这里的[所解释的](http://docs.aws.amazon.com/machine-learning/latest/dg/evaluating_models.html)。

步骤 4:现在让我们部署模型。按照这些步骤[为您的模型](http://docs.aws.amazon.com/machine-learning/latest/dg/requesting-real-time-predictions.html#creating-a-real-time-endpoint)创建一个实时端点(阅读文档并自己按照这些步骤来做确实是一个很好的实践)。

第五步:接下来是有趣的部分。我们将使用 [AWS Lambda](https://aws.amazon.com/lambda/) 和 [Amazon API Gateway](https://aws.amazon.com/api-gateway/) 来处理对算法的请求。最棒的是，你甚至不会知道你正在使用它们！我将更详细地介绍这些步骤:

步骤 5a:在您的 Linux 终端中，键入:

```
serverless create --template aws-nodejs --path my-first-algorithm
```

步骤 5b:在新创建的文件夹中，将其粘贴到“serverless.yml”文件中:

```
service: my-first-algorithmprovider:
  name: aws
  runtime: nodejs4.3iamRoleStatements:
  - Effect: Allow
    Action:
      - machinelearning:Predict
    Resource: "*"functions:
  model:
    handler: handler.model
    events:
    - http:
        path: model
        method: post
        cors: true
```

步骤 5b:将下面的代码粘贴到您的“handler.js”文件中，插入您在上面的步骤 4 中找到的正确的“MLModelId”和“PredictEndpoint”(我的示例只有一个输入特征，如果您的特定模型需要，请重命名并添加额外的特征):

```
// load aws sdk
const AWS = require('aws-sdk');// load AWS MachineLearning
var machinelearning = new AWS.MachineLearning({apiVersion: '2014-12-12'});module.exports.model = (event, context, callback) => { var data = JSON.parse(event.body);
  var inputFeature1Data = data.inputFeature1; var params = {
    MLModelId: **INSERT-THIS-YOURSELF**,
    PredictEndpoint: **INSERT-THIS-YOURSELF**,
    Record: {
      inputFeature1: inputFeature1Data
    }
  }; machinelearning.predict(params, function(err, data) {
    if (err) {
        const response = {
          headers: {'Access-Control-Allow-Origin': '*'},
          statusCode: err.statusCode,
          body: JSON.stringify(err)
        };
        callback(null, response);
    } else {
        const response = {
          headers: {'Access-Control-Allow-Origin': '*'},
          statusCode: 200,
          body: JSON.stringify(data)
        };
        callback(null, response);
    }
  });};
```

步骤 5c:确保您仍然在 Linux 终端中新创建的文件夹中，并键入`serverless deploy`，等待服务部署，一旦您看到“端点”url，您就可以开始了。

您可以通过将下面的代码粘贴到您的 Linux 终端来测试该模型的工作情况(将“endpoint-url”替换为步骤 5c 中的 url 端点) :

`curl -X POST endpoint-url --data '{"inputFeature1":7}'`

现在，您已经有了一个在生产中完全部署和可用的算法。它可以扩展到处理所有请求，并且完全由 AWS 管理，您不需要做任何维护，太棒了！这个工作流程很神奇，尤其是如果你想建立一个概念验证算法的话。添加 API 密钥或 JSON web 令牌验证将提供必要的安全性，只允许特定用户访问您的算法。

我希望您对此感兴趣，我确实认为这是数据科学工作流程中的一个范式转变。

附:这是我第一次在网上发帖，请评论但不要太苛刻:)