# 如何让 fbprophet 在 AWS Lambda 上工作

> 原文：<https://towardsdatascience.com/how-to-get-fbprophet-work-on-aws-lambda-c3a33a081aaf?source=collection_archive---------11----------------------->

## 解决 fbprophet 无服务器部署的包大小问题

![](img/64b9923cb1893bf9b704e454e2284e78.png)

Adi Goldstein / Unsplash

我假设你正在阅读这篇文章，因为你正在寻找在 AWS Lambda 上使用令人敬畏的 fbprophet(脸书开源预测)库的方法，并且你已经熟悉了完成它的各种问题。我将使用 python 3.6 示例，但是这种方法适用于其他运行时以及其他大型 ML 库。事不宜迟:

1.  本地安装 Docker。另一种方法是运行 EC2 实例，并在实例和本地机器之间复制文件，这对我来说更复杂，也更不可靠。
2.  使用您的代码在工作目录中的相关 lambda 映像上运行一个 shell(看一下 [lambci](https://github.com/lambci/docker-lambda#build-examples) )，它应该在 Lambda 上运行(它有 Lambda 处理程序):

```
docker run --rm -it -v “$PWD”:/var/task lambci/lambda:build-python3.6 bash
```

关于这里使用的命令的解释，请点击 Docker docs 或[这里](https://medium.freecodecamp.org/escaping-lambda-function-hell-using-docker-40b187ec1e48)。

3.创建虚拟环境并安装相关软件包:

```
python3 -m venv venv
. venv/bin/activate
pip install --upgrade pip
pip install fbprophet --no-cache
```

4.你可以检查你的代码是否工作正常:`python lambda_handler.py`

5.将 venv 的大小减少到 250Mb 以下:

```
pip uninstall -y matplolibfind "$VIRTUAL_ENV/lib/python3.6/site-packages" -name "test" | xargs rm -rffind "$VIRTUAL_ENV/lib/python3.6/site-packages" -name "tests" | xargs rm -rfrm -rf "$VIRTUAL_ENV/lib/python3.6/site-packages/pystan/stan/src"rm -rf "$VIRTUAL_ENV/lib/python3.6/site-packages/pystan/stan/lib/stan_math/lib"echo "venv size $(du -sh $VIRTUAL_ENV | cut -f1)"
```

这里发生的事情是，您正在摆脱 matplotlib(如果您不需要绘图)、测试和 pystan 源代码(软件包中最重的部分)。最后一个命令显示 venv 的大小，应该低于 250Mb。

6.确保你的代码在清理后仍然工作:`python lambda_handler.py`

7.创建一个. zip 文件上传到 Lambda:

```
pushd $VIRTUAL_ENV/lib/python3.6/site-packages/zip -r -9 -q /var/task/name_of_your_zip.zip *popdzip -9r name_of_your_zip.zip name_of_your_code_folder/zip -9 name_of_your_zip.zip lambda_handler.py
```

8.现在您的工作目录中有了一个. zip 文件，可以用于 Lambda。你现在可以停止/退出 Docker 了——这个压缩文件仍然在你的文件夹里。

其他资源:

关于为 Lambda 准备 ML 库的精彩帖子:[https://medium . com/@ Mae Bert/machine-learning-on-AWS-Lambda-5dc 57127 aee 1](https://medium.com/@maebert/machine-learning-on-aws-lambda-5dc57127aee1)

为 Lambda 准备 fbprophet 的讨论:[https://gist . github . com/CarstVaartjes/77 DBE 8249d 171 e 592 BD 17847 f 7009272](https://gist.github.com/CarstVaartjes/77dbe8249d171e592bd17847f7009272)

缩小 scipy 的大小:[https://gist . github . com/wrwrwr/4a 38 a 17113 e 3845 f 3385 e 9388 effc 064](https://gist.github.com/wrwrwr/4a38a17113e3845f3385e9388effc064)