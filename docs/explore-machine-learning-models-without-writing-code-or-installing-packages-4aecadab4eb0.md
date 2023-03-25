# 探索机器学习模型，无需编写代码或安装软件包

> 原文：<https://towardsdatascience.com/explore-machine-learning-models-without-writing-code-or-installing-packages-4aecadab4eb0?source=collection_archive---------6----------------------->

## 利用 scikit-learn 的模型并通过 API 公开它们的行为

![](img/0a9037daa9dea7e79f5d0e2bcab13d78.png)

“forced perspective photography of bus on road” by [Muneeb Syed](https://unsplash.com/@muneebs88?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

机器学习|人工智能|深度学习——它们是下一件大事，我知道，我们都知道。那为什么要等？当机会就在我们面前时，是什么阻止我们抓住它？也许是因为从日常琐事中抽身出来，做一些新的事情需要一点努力。

早在我开始探索机器学习及其应用的时候，我的很大一部分时间都浪费在猜猜是什么上了。—搞清楚和研究 1。要使用的语言，请按 2。框架选择，3。安装(以正确的方式安装)和设置开发环境。在所有的戏剧之后，我开始实际的发展。

然后出现了构建基于云的应用程序的想法，任何人都可以开始利用机器学习的力量，它会处理其余的事情。当然有很多可用的服务——其中一个是 [BigML](https://bigml.com/) ，但是为了我自己的实践和满足，我开始发展这个想法。

一夜之间成了我梦寐以求的项目；一个独立的 web 应用程序，可以通过 API 学习和测试机器学习模型。没有更多的编码专业知识，安装问题，兼容性问题，需要一个强大的钻机。甚至邮递员也能跑。

## 示例用法

获取当前用户的 JWT

```
POST /api/login/
Content-Type: application/json
{
  "username": "username",
  "password": "password"
}

Response
{
  "token": "abcd12345"
}
```

考虑下面的问题陈述

> 你是一名物理系学生，参加了期末考试，迫不及待地想知道你的最终分数。但是给你打分的老师非常严格。他有一个计算总分的公式但是没人知道(就是 0.5 * Paper_1 + 2 * Paper_2 + Paper_3)。你有一份你朋友的考试成绩和期末成绩的清单，并想计算你的分数。

用给定的输入启动线性回归器

```
POST /api/model/
Content-Type: application/json
Accept: application/json
Authorization: JWT abcd12345

{
  "model_path": "sklearn.linear_model.LinearRegression",
  "action": "new_model",
  "name": "Compute Final Score",
  "input_x": [[95, 87, 69], [99, 48, 54], [85, 57, 98], [90, 95, 91]],
  "input_y": [291, 200, 254, 326]
}

Response
{
  "status": "Trained",
  "model_id": "randommodelid"
}
```

使用这个训练好的模型来预测你的分数

```
POST /api/model/
Content-Type: application/json
Accept: application/json
Authorization: JWT abcd12345

{
  "action": "predict",
  "model_id": "randommodelid",
  "input_x": [[90, 95, 91]]
}

Response
{
  "status": "OK",
  "prediction": [
      326
  ]
}
```

> 这是一个正在开发的活动项目。目前服务于线性回归，SVM 和 KNN。更复杂的型号即将推出…

***更新(2018 . 9 . 30):重构代码，动态获取用户在 API 中提到的模型类。理论上，scikit learn 中的所有模型现在都可以测试。***

`model_path`参数决定调用哪个模型。您可以尝试各种其他方法，包括但不限于:

*   `sklearn.svm.SVR`
*   `sklearn.svm.SVC`
*   `sklearn.linear_model.BayesianRidge`

还有很多很多…

我期待着志同道合的人，就像我一样，谁愿意帮助或有助于该项目。该项目托管在 [Github](https://github.com/ramansah/ml_webapp) 上，我很乐意接受一些拉请求。😃