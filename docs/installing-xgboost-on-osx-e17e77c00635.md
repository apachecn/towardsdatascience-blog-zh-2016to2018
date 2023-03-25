# 在 OSX 上安装 XGBoost

> 原文：<https://towardsdatascience.com/installing-xgboost-on-osx-e17e77c00635?source=collection_archive---------1----------------------->

我们本周的项目是研究一个古老的卡格尔竞赛([https://www.kaggle.com/c/predict-west-nile-virus](https://www.kaggle.com/c/predict-west-nile-virus))。XGBoost 在过去的 kaggle 比赛中被许多获胜者使用，所以我们非常希望它能提高我们的成绩。虽然这并没有提高我们的成绩，但我确实在这个过程中学到了很多。

我找到的下载 XGBoost for Mac 的最好的文章是通过 IBM 网站(讽刺的是，不是？).甚至这篇文章也有很多地方需要进行故障诊断，所以我想介绍一下我遇到错误的地方，但这绝对是一个好的起点。如果 IBM 的文章对您有用，就不需要继续读下去了，但是如果没有，我希望我能回答您的一些问题。

 [## 在 Mac OSX 上安装 XGBoost(最好保密的是优化)

### OSX 比 Windows 好得多，不是吗？这是一个普遍的智慧，当我…

www.ibm.com](https://www.ibm.com/developerworks/community/blogs/jfp/entry/Installing_XGBoost_on_Mac_OSX?lang=en) 

1.  在你的终端上，如果你还没有安装家酿软件的话。"/usr/bin/ruby-e " $(curl-fsSL[https://raw . githubusercontent . com/home brew/install/master/install](https://raw.githubusercontent.com/Homebrew/install/master/install))" "如果在这一步遇到错误，请参考本网站"[https://computers . tuts plus . com/tutorials/home brew-demystified-OS-xs-ultimate-package-manager-MAC-44884](https://computers.tutsplus.com/tutorials/homebrew-demystified-os-xs-ultimate-package-manager--mac-44884)"。
2.  同样在终端中，您应该“brew install gcc—without-multi lib”，without multilib 应该有助于解决我们稍后可能会遇到的多线程错误。这可能需要一些时间，但是如果这不起作用，请尝试“sudo apt-get install gcc
    sudo apt-get install g++”
3.  cd 到你想放目录的任何地方，我简单地把我的放在我的根目录，但是确保这不是在另一个目录里！(我们不需要任何怀孕的回复)
4.  使用这一行“git clone-recursive[https://github.com/dmlc/xgboost](https://github.com/dmlc/xgboost)”将存储库克隆到您希望您的目录所在的任何位置
5.  cd 放到名为 xgboost 的新目录中
6.  打开 make/config.mk。如果您的计算机已正确格式化，只需键入“打开 make/config.mk”就可以了。如果没有，您可以使用 GUI 在您喜欢的文本编辑器或其他命令中打开它。取消对这两行代码的注释:“export CC = gcc
    export CXX = g++”。根据您安装的 gcc 版本，您可能需要将它们更改为“export CC = gcc-6
    export CXX = g++-6”。请确保保存此内容，然后继续下一步。
7.  cd 进入 xgboost(如果您还没有在那里)，然后“cp make/config.mk。
    make -j4
8.  最后一步:“CD python-package；sudo python setup.py 安装

要检查这是否成功，您可以打开一个 jupyter 笔记本或者在您的终端中键入“pip freeze ”,并确保 xgboost 有一个版本。如果你想在 jupyter 笔记本中使用它，你应该打开一个新的终端窗口，并启动一个新的内核。你可以通过“import xgboost”来检查这个过程是否成功，只要它不抛出错误，你就可以使用它。

如果这些步骤不起作用，请尝试“brew install gcc5”，然后将步骤 6 更改为“export CC = gcc-5
export CXX = g++-5”

如果最后一步不起作用，就用这三个代替:“sudo。/build . sh
CD python-package
sudo/home/username/anaconda/bin/python setup . py install "

令人沮丧的答案是，如果这不起作用，卸载并再次尝试稍微改变步骤，并尝试准确地遵循 IBM 的文章。

[](https://github.com/dmlc/xgboost/issues/463#issuecomment-147365960) [## pip 安装失败问题#463 dmlc/xgboost

### xgboost -可扩展、可移植和分布式梯度增强(GBDT、GBRT 或 GBM)库，用于 Python、R、Java、Scala…

github.com](https://github.com/dmlc/xgboost/issues/463#issuecomment-147365960)