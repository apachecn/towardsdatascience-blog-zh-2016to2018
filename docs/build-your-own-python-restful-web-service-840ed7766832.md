# 构建自己的 Python RESTful Web 服务

> 原文：<https://towardsdatascience.com/build-your-own-python-restful-web-service-840ed7766832?source=collection_archive---------0----------------------->

![](img/c34a84c252101556beb2df22e2567ad8.png)

“An overhead shot of strawberry and blackberry pies next to a white coffee pot and a bunch of flowers” by [Brooke Lark](https://unsplash.com/@brookelark?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

在这篇博客中，我将解释我是如何创建一个 RESTful web 服务来充当数据工程服务的。web 服务是通过一个 **Docker** 容器中的 python web framework**CherryPy**提供的。以下是如何使用 3 个文件和不到 30 行代码实现这一目标的分步说明。

第一步是通过安装 [Docker](https://www.docker.com/get-docker) 、 [Python 3](https://www.python.org/downloads/) 和以下 Python 库来设置开发环境:

*   pandas——用于对数据集执行聚合
*   CherryPy -服务于 web 服务的 web 框架

这些库可以使用 *pip* 命令安装:

```
>> pip install pandas
>> pip install CherryPy
```

让我们首先编写一个简单的数据处理器 *myprocessor.py* :

```
class MyProcessor:         def run(self, df):        
        return df.agg(['mean', 'min', 'max'])
```

该处理器计算输入数据帧所有列的平均值、最小值和最大值。

在同一个目录中，我们创建一个 web 服务脚本 *ws.py* ，它包含请求处理程序的定义:

```
**import** cherrypy
**import** pandas **as** pd
**import** myprocessorp = myprocessor.MyProcessor()

**class MyWebService**(object):

   @cherrypy.expose
   @cherrypy.tools.json_out()
   @cherrypy.tools.json_in()
   **def process**(self):
      data = cherrypy.request.json
      df = pd.DataFrame(data)
      output = p.run(df)
      **return** output.to_json()**if** __name__ == '__main__':
      ...
```

该处理程序接受指向 */process* 端点的请求，并期望输入数据在请求体中表示为 JSON 字符串。处理的结果将作为 JSON 字符串返回——使用 panda *to_json()* 方法方便地进行转换。

接下来是在 *ws.py* 的主要部分启动 web 服务的代码:

```
**if** __name__ == '__main__':
   config = {'server.socket_host': '0.0.0.0'}
   cherrypy.config.update(config)
   cherrypy.quickstart(MyWebService())
```

myprocessor.py 和 ws.py 都是 web 服务所需要的。我们现在可以使用以下 python 命令启动 web 服务:

```
>> python ws.py
[24/Jul/2018:11:13:53] ENGINE Listening for SIGTERM.
[24/Jul/2018:11:13:53] ENGINE Listening for SIGHUP.
[24/Jul/2018:11:13:53] ENGINE Listening for SIGUSR1.
[24/Jul/2018:11:13:53] ENGINE Bus STARTING[24/Jul/2018:11:13:53] ENGINE Started monitor thread 'Autoreloader'.
[24/Jul/2018:11:13:53] ENGINE Started monitor thread '_TimeoutMonitor'.
[24/Jul/2018:11:13:53] ENGINE Serving on [http://0.0.0.0:8080](http://0.0.0.0:8080)
[24/Jul/2018:11:13:53] ENGINE Bus STARTED
```

哒哒！这里是我们的网络服务运行在任何时候。

在 Linux 系统中，我们可以使用 *curl* 发送这个 POST 请求来验证 REST API 是否正常工作:

```
>> curl -d '{"num1" : [1, 2, 3], "num2":[4, 5, 6]}' -H "Content-Type: application/json" -X POST [http://localhost:8080/process](http://localhost:8080/process)"{\"num1\":{\"mean\":2.0,\"min\":1.0,\"max\":3.0},\"num2\":{\"mean\":5.0,\"min\":4.0,\"max\":6.0}}"
```

预期结果是一个数据集，包括发送到 web 的两组数字的平均值、最小值和最大值。

**码头集装箱**

接下来，我们需要编写一个 *Dockerfile —* 这是一个文本文件，用于设置 web 服务将要运行的环境。如果你以前没有使用过 Docker，我建议至少阅读一下 [Docker 入门文档](https://docs.docker.com/get-started/)，这样你就可以熟悉 Docker 的概念。文件中的第一行声明使用*python:3 . 6 . 4-slim-Jessie*作为基础映像:

```
from python:3.6.4-slim-jessie
```

这个是[官方 Python Linux 映像](https://hub.docker.com/_/python/)之一，预装了 Python 3 的普通版本，还有许多其他映像可以从 Docker 存储库中选择，但这是一个精简版本，足以运行一个简单的服务。就像我们必须安装额外的 python 包来本地运行 web 服务一样，在构建 docker 映像时也需要这样做:

```
RUN pip install pandas
RUN pip install CherryPy
```

接下来，构建过程需要将 web 服务文件复制到默认的工作目录中:

```
COPY myprocessor.py .
COPY ws.py .
```

容器需要公开端口 8080 以允许访问:

```
EXPOSE 8080
```

*docker 文件*中的最后一条语句定义了容器启动时要运行的命令:

```
ENTRYPOINT ["python", "ws.py"]
```

这是完成的*docker 文件*的样子，它应该与之前编写的 python 脚本驻留在同一个目录中:

```
from python:3.6.4-slim-jessieRUN pip install pandas
RUN pip install CherryPy
COPY myprocessor.py .
COPY ws.py .
EXPOSE 8080
ENTRYPOINT ["python", "ws.py"]
```

为了执行 docker build 命令，我们停留在创建了 *Dockerfile* 的目录中。为 docker 映像分配一个标记是一个很好的做法，这个标记可以在以后引用，因此我们使用带有-t 标记的 build 命令来构建映像:

```
>> docker build -t python-ws .
```

我们现在已经准备好了一个 docker 图像，让我们试一试:

```
>> docker run -p 8080:8080 python-ws
```

该命令以附加模式启动容器，因此 Cherrypy 日志将立即出现在控制台中。同样，之前用于测试的相同 curl 命令也应该工作，因为主机(localhost)现在将定向到端口 8080 的请求转发到正在运行的 docker 容器。

我已经演示了如何构建一个简单的 RESTful web 服务，但是这个服务还远远不能用于生产。如果 web 服务托管在公共云中，应采取额外的安全措施，例如 HTTPS 连接、某种形式的身份验证。如何启用这些安全特性的示例代码可以在我的 [GitHub 项目](https://github.com/hungapl/python-ws)中找到。Cherrypy 教程页面提供了许多关于如何定制 web 服务的例子。