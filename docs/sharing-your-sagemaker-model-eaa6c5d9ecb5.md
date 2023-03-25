# 分享你的 SageMaker 模型

> 原文：<https://towardsdatascience.com/sharing-your-sagemaker-model-eaa6c5d9ecb5?source=collection_archive---------8----------------------->

![](img/66cbf2826079e9938587c9e31e85abb2.png)

*本文原载于*【blog.zakjost.com】

*AWS 最近发布了 [SageMaker](https://aws.amazon.com/sagemaker/) ，它可以帮助你从零开始构建模型，到部署和扩展这些模型以用于生产。本文将假设您已经在 SageMaker 中运行了一个模型，并希望授予对另一个 AWS 帐户的访问权限，以便使用您的模型进行预测。*

# *从父帐户向子帐户授予(有限)权限*

*在较高层次上，您需要创建一个有权调用端点的角色，然后授予子帐户对此角色的访问权限。为此，您需要孩子帐户的 12 位帐户 id。然后，按照以下步骤操作:*

*   *登录父帐户控制台并导航到 IAM 服务*
*   *创建名为“SageMakerExternalInvokeEndpoint”的新策略。粘贴以下策略。请注意括号中您需要输入的数据。*

```
*{ "Version": "2012-10-17", "Statement": [ { "Action": ["sagemaker:InvokeEndpoint"], "Effect": "Allow", "Resource": "arn:aws:sagemaker:*:<parent   account_id>:endpoint/<endpoint name>” } ]}*
```

*   *单击“角色”并创建“SageMakerExternal”角色*
*   *选择“另一个 AWS 帐户”并粘贴子帐户 id*
*   *附加新的“SageMakerExternalInvokeEndpoint”策略(单击“策略类型”箭头“客户管理”，然后选择“SageMakerExternalInvokeEndpoint”)*
*   *单击新角色并复制角色 arn，这将在后面的步骤中使用。*

# *向子帐户授予“承担角色”策略*

*本部分将授予承担刚刚由子帐户创建的角色的能力。您将需要上述最后一步中的角色 arn。*

*   *使用子帐户登录到控制台-> IAM*
*   *如果您没有群，请创建一个。我把我的叫做“SagemakerTester”。如果您有现有用户，您可以将他们添加到组中。*
*   *单击“权限”选项卡，展开“内联策略”箭头，然后创建组策略。添加此策略:*

```
*{“Version”: “2012–10–17”, “Statement”: { “Effect”: “Allow”, “Action”: “sts:AssumeRole”, “Resource”: “<SageMakerExternal role arn copied above>“ }}*
```

# *使用 Boto3 中的假定角色*

*现在权限已经配置好了，您需要能够在尝试访问端点时承担这个角色。如果您使用的是 Boto3，代码将如下所示:*

```
*# Get temporary assumed role credentialsclient = boto3.client('sts') #assumes you’ve configured boto with normal child account credentialsresponse = client.assume_role( RoleArn="<SageMakerExternal role arn copied above>", RoleSessionName="testSessionName", # this is arbitrary ExternalId=‘<parent account_id>’ )# Create boto client using temp credentialsclient = boto3.client( 'sagemaker-runtime', aws_access_key_id=response["Credentials"]["AccessKeyId"], aws_secret_access_key=response["Credentials"]["SecretAccessKey"], aws_session_token=response["Credentials"]["SessionToken"],)# Invoke endpointclient.invoke_endpoint( EndpointName=<endpoint_name>, Body=<data>)*
```