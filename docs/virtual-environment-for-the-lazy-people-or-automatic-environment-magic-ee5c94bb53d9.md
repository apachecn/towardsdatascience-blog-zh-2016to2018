# “懒惰”人的虚拟环境，或自动环境魔术。

> 原文：<https://towardsdatascience.com/virtual-environment-for-the-lazy-people-or-automatic-environment-magic-ee5c94bb53d9?source=collection_archive---------6----------------------->

![](img/41df929dde00156c7c7de142febd7391.png)

当您努力开发或部署大量不同的 python 项目时，您通常会使用一个 **virtualenv** 来分离和隔离 Python 环境。这绝对是一个好的实践，不仅对于开发人员，而且对于在生产环境中进行部署的人员也是如此。隔离环境使您的系统保持安全和清晰，也给了您很大的灵活性。

但是，当您更改目录时，经常会忘记激活所需的虚拟环境，或者当您离开时，又忘记禁用它。我发现自己经常这样做，所以我开始思考如何让这个例行程序自动化。嗯，我不是唯一一个有这种想法的人，善良的人们已经采取了必要的措施来解决这个问题。

我想向您介绍几个实用程序，它们可以让您更轻松地配置和自动更改每个目录的不同环境。这个实用程序像标准 bash cd 命令的包装器一样工作。每次当你“光盘”在一些目录，其中包含一些具体的。env 文件，它们为您激活先前配置的环境。还有，你出门的时候，他们会自动停用。还不错。

经过一番搜索，我停在了两个最有趣的项目上， [**Direnv**](https://direnv.net/) 和 [**Autoenv**](https://github.com/kennethreitz/autoenv) 。你可以选择任何一个，但我更喜欢 Direnv，因为它是一个编译程序，所以我希望它能快一点。

Direnv 易于安装，因为它像一个包一样出现在最流行“apt”和“yum”发行版中，你也可以从源代码安装它几秒钟。

安装 direnv 后，您需要在~/中添加一个字符串。bashrc 文件:

```
eval "$(direnv hook bash)"
```

我们已经准备好了一些例子:

```
$ mkdir /home/projects && cd /home/projects 
$ mkdir project1 project2
```

然后我们需要在两个目录中创建一个. envrc 文件，在 direnv 在目录中找到这个文件后，它将准备/启用新的环境并导出所有列出的本地变量。

出于检查的原因，我们将为每个项目目录声明不同的 python 版本。

```
$ echo "layout python" > project1/.envrc
$ echo "layout python3" > project2/.envrc
```

出于安全原因，您需要批准新的或更改的。envrc 文件首先，direnv 会要求您确认。

```
$ cd project1/ 
direnv: error .envrc is blocked. Run `direnv allow` to approve its content.$ direnv allowdirenv: loading .envrc 
Running virtualenv with interpreter /usr/bin/python 
New python executable in /home/projects/project1/.direnv/python-2.7.13/bin/python 
Installing setuptools, pkg_resources, pip, wheel...done. 
direnv: export +VIRTUAL_ENV ~PATH
```

在您批准之后，direnv 将继续创建新的 python 2 环境。让我们对 project2 目录执行相同的步骤。

```
$ cd .. 
direnv: unloading$ cd project2/ 
direnv: error .envrc is blocked. Run `direnv allow` to approve its content.$ direnv allowdirenv: loading .envrc 
Already using interpreter /usr/bin/python3 
Using base prefix '/usr' 
New python executable in /home/projects/project2/.direnv/python-3.5.3/bin/python3 
Also creating executable in /home/projects/project2/.direnv/python-3.5.3/bin/python 
Installing setuptools, pkg_resources, pip, wheel...direnv: ([direnv export bash]) is taking a while to execute. Use CTRL-C to give up. 
done. 
direnv: export +VIRTUAL_ENV ~PATH
```

正如你现在看到的，我们得到了 python 3，让我们“cd”到每个目录，并尝试安装不同的包或库。

```
$ cd project1/ 
direnv: loading .envrc 
direnv: export +VIRTUAL_ENV ~PATH$ pip install ansible skip skip skip... :) Successfully installed MarkupSafe-1.0 PyYAML-3.13 ansible-2.7.1 asn1crypto-0.24.0 bcrypt-3.1.4 cffi-1.11.5 cryptography-2.3.1 enum34-1.1.6 idna-2.7 i
paddress-1.0.22 jinja2-2.10 paramiko-2.4.2 pyasn1-0.4.4 pycparser-2.19 pynacl-1.3.0 six-1.11.0$ ansible --version 
ansible 2.7.1 
  config file = None 
  configured module search path = [u'/root/.ansible/plugins/modules', u'/usr/share/ansible/plugins/modules'] 
  ansible python module location = /home/projects/project1/.direnv/python-2.7.13/local/lib/python2.7/site-packages/ansible 
  executable location = /home/projects/project1/.direnv/python-2.7.13/bin/ansible 
  python version = 2.7.13 (default, Sep 26 2018, 18:42:22) [GCC 6.3.0 20170516]$ which ansible 
/home/projects/project1/.direnv/python-2.7.13/bin/ansible$ cd ..
direnv: unloading$ ansible  
-bash: ansible: command not found$ cd project1 
direnv: loading .envrc 
direnv: export +VIRTUAL_ENV ~PATH$ ansible
Usage: ansible <host-pattern> [options] 
bla bla bla...
```

正如你所看到的，我们刚刚在 project1 目录中安装了一个 Ansible，在我们离开这个目录之后，所有的环境都被卸载，所以在 project1 目录之外就没有 Ansible 了。同样，当我们返回时，我们将得到可工作的 Ansible 包。

这真的很方便，当您创建一个专用的 Ansible 部署服务器时，将使用这个 Ansible 安装的人可以直接“cd”到所需的目录来获得工作项目，以及所有导出的本地环境和变量。事实上，你可以在这个服务器上有几个不同的 Ansible 项目，有不同的参数和所有你需要的正确配置的环境，只需“cd”到所需的目录。有用，不是吗？

此外，我们可以在。例如，envrc 文件:

```
$ echo "export FOO=Hello" >> .envrc  
direnv: error .envrc is blocked. Run `direnv allow` to approve its content. $ direnv allow
direnv: loading .envrc 
direnv: export +FOO +VIRTUAL_ENV ~PATH $ echo $FOO 
Hello
```

您可以在项目主页上找到关于安装和使用的所有附加信息。希望它对你也很方便有用。

祝你好运。