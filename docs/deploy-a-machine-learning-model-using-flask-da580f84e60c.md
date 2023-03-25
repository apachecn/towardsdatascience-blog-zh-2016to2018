# 使用 flask 部署机器学习模型

> 原文：<https://towardsdatascience.com/deploy-a-machine-learning-model-using-flask-da580f84e60c?source=collection_archive---------1----------------------->

作为机器学习的初学者，对于任何人来说，获得关于机器学习和深度学习的所有算法的足够资源可能都很容易，但是当我开始寻找将 ML 模型部署到生产的参考资料时，我没有找到任何可以帮助我部署我的模型的好资源，因为我对这个领域非常陌生。因此，当我成功地使用 Flask 作为 API 部署我的模型时，我决定写一篇文章来帮助其他人简单地部署他们的模型。希望有帮助:)

在本文中，我们将使用简单的线性回归算法和 scikit-learn。为了简单起见，我们将使用 Flask，因为它是一个非常轻量级的 web 框架。我们将创建三个文件，

1.  *model.py*
2.  *server.py*
3.  *request.py*

在一个 *model.py* 文件中，我们将开发并训练我们的模型，在一个 *server.py 中，*我们将编码处理 POST 请求并返回结果，最后在 *request.py* 中，我们将向服务器发送带有特性的请求并接收结果。

# 让我们开始编码部分

1.  **model.py**

正如我上面提到的，在这个文件中我们将开发我们的 ML 模型并训练它。我们将根据员工在该领域的经验来预测他/她的工资。你可以在这里找到数据集。

```
import numpy as np
import pandas as pd
from sklearn.model_selection import train_test_split
from sklearn.linear_model import LinearRegression
import pickle
import requests
import json
```

导入我们将用来开发模型的库。 *numpy* 和 *pandas* 分别操作矩阵和数据，*sk learn . model _ selection*将数据拆分为训练集和测试集， *sklearn.linear_model* 使用 *LinearRegression* 训练我们的模型。 *pickle* 将我们训练好的模型保存到磁盘，*请求*向服务器发送请求， *json* 在我们的终端打印结果。

```
dataset = pd.read_csv('Salary_Data.csv')
X = dataset.iloc[:, :-1].values
y = dataset.iloc[:, 1].values
```

我们已经使用 pandas 导入了数据集，并将要素和标注从数据集中分离出来。

```
X_train, X_test, y_train, y_test = train_test_split(X, y, test_size = 0.33, random_state = 0)
```

在本节中，我们使用来自 *sklearn 的 *train_test_split* 将我们的数据分为训练和测试大小分别为 0.67 和 0.33。*

```
regressor = LinearRegression()
regressor.fit(X_train, y_train)y_pred = regressor.predict(X_test)
```

该对象被实例化为类 *LinearRegression()* 的*回归变量*，并使用 *X_train* 和 *y_train 进行训练。*后者预测结果存储在 *y_pred 中。*

```
pickle.dump(regressor, open('model.pkl','wb'))
```

我们将使用 *pickle* 库将训练好的模型保存到磁盘上。 *Pickle* 用于序列化和反序列化 Python 对象结构。其中 python 对象被转换成字节流。 *dump()* 方法将对象转储到参数中指定的文件中。

在我们的例子中，我们希望保存我们的模型，以便服务器可以使用它。因此，我们将把我们的对象*回归器*保存到名为 *model.pkl.* 的文件中

我们可以通过下面的方法再次加载模型，

```
model = pickle.load(open('model.pkl','rb'))
print(model.predict([[1.8]]))
```

*pickle.load()* 方法加载方法并将反序列化后的字节保存到*模型。*可以使用 *model.predict()进行预测。*

例如，我们可以预测有 1.8 年工作经验的雇员的工资。

在这里，我们的 *model.py* 准备训练并保存模型。 *model.py* 的完整代码如下。

```
# Importing the libraries
import numpy as np
import pandas as pd
from sklearn.model_selection import train_test_split
from sklearn.linear_model import LinearRegression
import pickle
import requests
import json# Importing the dataset
dataset = pd.read_csv('Salary_Data.csv')
X = dataset.iloc[:, :-1].values
y = dataset.iloc[:, 1].values# Splitting the dataset into the Training set and Test set
X_train, X_test, y_train, y_test = train_test_split(X, y, test_size = 1/3, random_state = 0)# Fitting Simple Linear Regression to the Training set
regressor = LinearRegression()
regressor.fit(X_train, y_train)# Predicting the Test set results
y_pred = regressor.predict(X_test)# Saving model to disk
pickle.dump(regressor, open('model.pkl','wb'))# Loading model to compare the results
model = pickle.load(open('model.pkl','rb'))
print(model.predict([[1.8]]))
```

**2。server.py**

在这个文件中，我们将使用 flask web 框架来处理从 *request.py* 获得的 POST 请求。

导入我们将在代码中使用的方法和库。

```
import numpy as np
from flask import Flask, request, jsonify
import pickle
```

这里我们导入了 *numpy* 来创建请求数据的数组， *pickle* 来加载我们训练好的模型进行预测。

在下面的代码中，我们创建了 *Flask()* 的实例，并将模型加载到*模型中。*

```
app = Flask(__name__)model = pickle.load(open('model.pkl','rb'))
```

这里，我们用方法 *predict()绑定了 */api* 。*其中 predict 方法从请求者传递的 json 中获取数据。 *model.predict()* 方法从 json 获取输入，并将其转换为 2D *numpy 数组*结果存储在名为*的变量中，输出*和我们在使用 flaks*json ify()*方法将其转换为 JSON 对象后返回此变量。

```
[@app](http://twitter.com/app).route('/api',methods=['POST'])
def predict():
    data = request.get_json(force=True)
    prediction = model.predict([[np.array(data['exp'])]])
    output = prediction[0]
    return jsonify(output)
```

最后，我们将通过下面的代码运行我们的服务器。这里我使用了端口 5000，并设置了 *debug=True* ，因为如果我们遇到任何错误，我们可以调试并解决它。

```
if __name__ == '__main__':
    app.run(port=5000, debug=True)
```

在这里，我们的服务器已经准备好为请求提供服务。下面是 *server.py.* 的完整代码

```
# Import libraries
import numpy as np
from flask import Flask, request, jsonify
import pickleapp = Flask(__name__)# Load the model
model = pickle.load(open('model.pkl','rb'))@app.route('/api',methods=['POST'])
def predict():
    # Get the data from the POST request.
    data = request.get_json(force=True) # Make prediction using model loaded from disk as per the data.
    prediction = model.predict([[np.array(data['exp'])]]) # Take the first value of prediction
    output = prediction[0] return jsonify(output)if __name__ == '__main__':
    app.run(port=5000, debug=True)
```

3. *request.py*

正如我前面提到的, *request.py* 将向服务器请求预测。

下面是向服务器发出请求的完整代码。

```
import requestsurl = '[http://localhost:5000/api'](http://localhost:5000/api')r = requests.post(url,json={'exp':1.8,})
print(r.json())
```

我们已经使用*请求*库来发出 post 请求。 *requests.post()* 获取 URL 和 post 请求中要传递的数据，从服务器返回的结果存储到变量 *r* 中，并由 *r.json()打印。*

## **结论**

我们创建了三个文件 *model.py、server.py 和 request.py* 来训练和保存模型，处理请求，分别向服务器发出请求。

在对所有这些文件进行编码之后，执行文件的顺序应该是 *model.py* 、 *server.py* (在单独的终端中)以及最后 *request.py* 。

您可以将预测的结果与一个 *model.py* 进行比较，因为我们将结果打印在文件的末尾。

你可以在我的 Github 库 [flask-salary-predictor](https://github.com/vyashemang/flask-salary-predictor/) 中找到所有的代码。

不要犹豫，在下面的评论区说出你的想法。

谢谢:)