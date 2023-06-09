# 用 Python 构建预测 API(第 3 部分):自动化测试

> 原文：<https://towardsdatascience.com/building-prediction-apis-in-python-part-3-automated-testing-a7cfa1fa7e9d?source=collection_archive---------1----------------------->

![](img/d89fe699a106d362b82188e3e15091f5.png)

在上一篇文章中，我们改进了预测 API 的错误处理，并考虑了我们应该对哪些记录进行评分的微妙决定。在这篇文章中，我们将看看如何使用 pytest 测试我们的 API。

[](https://medium.com/@chris.moradi/building-prediction-apis-in-python-part-1-series-introduction-basic-example-fe89e12ffbd3) [## 用 Python 构建预测 API(第 1 部分):系列介绍&基本示例

### 好吧，你已经训练了一个模型，但是现在呢？如果没有人会使用，所有的工作都是没有意义的。在某些应用中…

medium.com](https://medium.com/@chris.moradi/building-prediction-apis-in-python-part-1-series-introduction-basic-example-fe89e12ffbd3) 

和往常一样，我们将使用 Python 3，并且我将假设您要么正在使用 Anaconda，要么已经设置了一个安装了这些包的环境:flask、scikit-learn 和 pytest。

*注意:我将在这篇文章中浏览代码片段，但是在上下文中查看完整的文件可能会有所帮助。你可以在 GitHub* *上找到* [*的完整例子。*](https://github.com/cmoradi/prediction-apis/tree/master/part03-testing/code)

# 测试案例

虽然自动化测试是现代软件开发中的核心实践，但它尚未被许多数据科学家完全接受。简单地说，这是在主代码基础上运行测试的附加代码。测试框架，比如 pytest，使得定义和执行一套测试变得很容易。随着新功能的实现或现有代码的重构，这些测试帮助开发人员确认现有功能没有被破坏或定位已经引入的错误。

以下是一些不使用我的(尖刻的)回答编写测试的常见借口:

*   *代码很少，所以不需要测试。*当您需要扩展当前功能时会发生什么？每个增强可能都很小，但是从长远来看，您可能会得到一个庞大的未经测试的代码库。如果目前代码很少，编写测试应该很容易，那么就去做吧！
*   *当我写原始代码*时，我已经根据多个测试用例检查了每个功能。太好了！构建测试的一个挑战是提出好的测试用例，而你已经做到了。每当你做一个小的改变时，有能力运行所有那些测试用例不是很棒吗？
*   *我写代码不出错*。当然啦！你不需要测试用例来测试你的代码，但是当其他人加入到项目中并开始修改你漂亮、完美的代码时会发生什么呢？我们怎么知道他们是否打碎了什么东西？你愿意详细检查他们所做的每一项承诺吗？你对在你的余生中维护这个代码感到兴奋吗？

在进入这篇文章的内容之前，我想指出我将是一个伪君子。我们将专注于测试 API。在这个过程中，我们还需要修改构建模型的代码。敏锐的读者会注意到，我没有为模型构建管道编写任何测试。如果有帮助的话，我真的很抱歉。

# 为更好的测试进行调整

在我们开始编写实际的测试之前，我想改变一下我们正在生成的 API 响应。目前，我们只发送回预测的类(虹膜类型)。虽然这是我们的预测 API 的用户所需要的，但它并没有提供大量的信息供我们测试。预测类被选为具有最高模型分数的类(虹膜类型)。由于这种阈值处理，即使潜在的分数有些不同，预测的类别也可以是相同的。这类似于一个函数，它执行复杂而精确的计算，但返回一个舍入到最接近的整数的值。即使许多输入的返回整数值与预期值匹配，基础计算也可能不正确。理想情况下，我们希望在计算的精确结果被阈值化或舍入之前验证它们是否正确。

为了给我们自己提供更多的数据来验证我们对模型的评分是正确的，我们将改变 API 响应来包含每个类的概率。这是通过调用`MODEL.predict_proba()`而不是`MODEL.predict()`来完成的。然后我们使用`argmax()`来获得最大值的索引，这给了我们预测的类。我们将通过`probabilities`将原始类别概率返回给用户(参见下面的示例响应)。

Updated API to return class probabilities

然后我们可以运行 API ( `python predict_api.py`)并通过`requests`进行测试调用:

# API 的基本测试

我们将从一个简单的例子开始，在这个例子中，我们只对如上所示的相同例子进行评分，但是我们将使用 pytest 来完成这个任务。我们首先需要创建一个测试文件:`test_predict_api.py`。现在，我们可以把它放在与我们的 API 文件相同的目录中:`predict_api.py`。*注意:pytest 能够自动定位测试文件和函数，但是您需要协助它这样做。默认情况下，它将检查任何文件名以“test_”前缀开头的文件，并运行任何以“test_”开头的测试函数。*

当我们构建测试时，我们通常遵循这种模式:

1.  设置(可选):测试或环境的初始化。示例:初始化数据库，创建将在测试中使用的类的实例，初始化系统的状态，等等
2.  运行代码:在预定义的测试用例上或在预定义的环境中运行主代码库(测试中的代码)中的一些代码。这可能包括:调用一个函数/方法，创建一个类的实例，初始化一个资源，调用一个 API 等等
3.  验证结果:通过使用`assert`语句检查代码的效果是否符合预期:函数调用的返回值是否正确，异常是否被适当地提出，系统是否已经改变到正确的状态等等…
4.  拆除(可选):测试运行后进行清理，将环境恢复到默认状态。

以下是本示例如何与这些步骤保持一致:

1.  设置:实例化一个`test_client`(如下所述)，它将允许我们模拟对 API 的调用。
2.  运行代码:用一组预定义的特性调用 `/predict`端点。
3.  验证结果:我们得到一个带有 200 的`status_code`的响应，内容是带有正确格式和值的 JSON。
4.  拆:这个有些含蓄。我们将在`test_client`的“设置”过程中使用上下文管理器。退出时，客户端将被清理。

如上所述，我们使用的`test_client`是 Flask 的一个特性，它允许我们在不运行服务器的情况下模拟调用。这里，我们使用上下文管理器创建一个新的客户机。由此，我们发出一个 GET 请求。`query_string`关键字参数提供的功能类似于`params`在`requests.get()`中的工作方式；它允许我们传递用于创建查询字符串的数据。

从响应中，我们检查我们是否收到了“200 OK”状态，最后我们检查响应的有效负载是否与我们预期的相匹配。既然它是以`bytes`的形式出现，我们可以用`json.loads()`把它转换成一个`dict`。

我们现在可以在命令行使用`pytest`来执行测试。

# 总是先失败

每当你编写自动化测试时，**验证你确实在测试一些东西是很重要的——你的测试可能会失败**。虽然这是一个显而易见的说法，但一个常见的缺陷是编写的测试实际上并不测试任何东西。当它们通过时，开发人员认为被测试的代码是正确的。然而，测试通过了，因为测试写得很差。

防止这种错误的一种方法是使用测试驱动开发(TDD)。我们不会对此进行深入探讨，但这是一个发展过程，其中:

1.  您首先为一个新特性编写一个测试。
2.  您确认测试失败。
3.  您编写代码来实现新功能。
4.  您验证测试现在通过了。

如果你之前没有尝试过 TDD，我绝对推荐。这需要纪律，尤其是在开始的时候，还需要其他开发者和利益相关者的支持。然而，在实现新特性时，对过程的投入会带来更少的错误和更低的压力。要了解更多，请阅读 Harry Percival 的优秀著作，[用 Python 进行测试驱动的开发](https://www.obeythetestinggoat.com/pages/book.html)，这本书是他在网上免费提供的。

如果您不支持 TDD，我用来验证每个测试是否确实在测试的懒惰方法是将我的 assert 表达式更改为显式失败。例如，我们将把`assert response.status_code == 200`改为`assert response.status_code != 200`。如果您进行了此更改并重新运行测试，您应该会收到类似于以下内容的失败消息:

如果您打算使用这种方法，请注意 pytest 只会报告第一个发生的`AssertionError`。所以，**你必须分别改变每个断言，然后重新测试**。

# 更多测试

我们现在有一个对我们的 API 的测试调用，它正在工作。我们如何扩展这个来测试具有不同特征值和不同预期结果(标签和概率)的多个调用？一个快捷的选择是使用我们在模型构建期间创建的测试数据集。但是，我们需要获得类概率和预测标签，以用作每个输入记录的预期结果。

需要注意的一点是**我们测试的是 API 平台，而不是模型本身**。基本上，这意味着我们不关心模型是否做出了错误的预测；我们只想验证 API 平台上的模型输出是否与构建/离线/开发环境中的模型输出相匹配。我们还需要测试特性的准备(例如，均值插补)是否在 API 平台上正确完成，但我们将在本文的下一部分讨论这个问题。

由于我们的测试数据集可能会随着模型的每个新版本而改变，我们应该将这些数据的生成合并到我们的模型构建中。我对我们的模型构建脚本做了一些轻微的重构(还需要更多),并添加了在模型构建后生成测试数据集的代码。我们将把我们的测试用例存储在一个 JSON 文件中，每个测试用例的结构如下:

下面是我们的模型构建代码的修改版本，它合并了测试数据集的生成:

在顶部，有一个名为`prep_test_cases()`的函数，它只是将每个测试的特性、分类概率和预测标签重新格式化为我们的测试用例格式。

现在我们已经生成了测试数据，我们需要添加一个新的测试来对该文件中的所有记录进行评分，并检查响应:

因为我们以一种清晰的方式构建了测试数据，其中每个测试用例都有特性(API 输入)以及预期的响应(API 输出)，所以测试代码相当简单。这种方法的一个缺点是类别概率是浮动的，我们正在对这些值进行精确的比较。通常，在比较浮点值时允许有一定的容差，这样非常接近的值就被认为是等价的。为了处理这个问题，我们需要解析预期的响应，并在比较`probabilities`中的每个值时使用`pytest.approx()`。它不需要太多的代码，但是我认为这会使这个讨论有点混乱，所以我省略了实现。

# 处理缺失值

我们的 API 配置为使用均值插补来替换错误值或缺失值，但我们的测试数据集不包含任何缺失值的记录。然而，这不是一个问题，因为我们可以使用我们已经拥有的数据来模拟这些数据。我们只需要用某个特性的平均值替换现有的值，并重新对记录进行评分。对于我们的模型构建脚本，我们将在原始测试数据生成代码之后添加以下内容:

我们可能不需要这么彻底，但是对于每一条记录，我们都在测试可能缺失的每一个特征组合。我们为每条记录创建了两个版本:一个有缺失值的`None` ,另一个有平均值的估算值。第一个将作为`features`存储在测试用例中(在`None`有价值的特性被删除之后)。第二个将被评分，以获得我们期望看到的 API 返回的预测概率。为了去掉 `None`值的特性，我们必须在`prep_test_cases`函数中对`feat_dict`的创建做一个小小的改变。下面是修改后的函数:

我们也需要改变我们的测试来使用这个新文件。虽然我们可以复制最后一个测试函数`test_api()`并替换文件名`testdata_iris_v1.0.json`，但这会导致重复的代码。因为我们需要测试函数除了文件名之外完全相同，所以更好的方法是使用 pytest 的`parametrize`功能。我们只需添加一个装饰器，允许我们为测试函数指定参数，并为每个值重新运行测试。在这种情况下，我们将传入文件名:

# 测试错误

在[上一篇关于错误处理的文章](https://medium.com/@chris.moradi/building-prediction-apis-in-python-part-2-basic-error-handling-3ab87b7a93)中，我提到我们可以更好地选择我们愿意评分的记录，但是我没有提供这样的例子。这里我们将调整我们的 API 来看一个简单的例子，在这个例子中，我们将拒绝缺少`petal_width`数据的请求。我们将对所有其他记录进行评分，如果需要的话，使用平均插补。

稍微扯点题外话，我是怎么选的`petal_width`？嗯，如果我们查看特征重要性(使用`model.feature_importances_`，我们会看到第四个特征(`petal_width`)具有最高值，归一化得分为 0.51。因为这是我们模型中最重要的特性，所以拒绝缺少这个特性的记录是最有意义的。

实现这一点的一个简单方法是删除`petal_width`的默认值，然后在它丢失的情况下处理它。几乎所有的代码都保持不变，但是我在这里把它包括进来是为了便于理解。

我们可以做一个快速测试，以确保它适用于一个简单的情况:

太好了！有用！现在，我们只需要将它添加到我们的测试套件中。

为了简单起见，我将跳过如何修改我们旧的缺失值测试，只实现处理`petal_width`的缺失值或错误值的新测试。基本上，我从`missing_grps`中移除了所有包含 3 的元组(索引为`petal_width`)以及所有特性都缺失的测试。

对于我们的新测试，我们可以使用相同的 JSON 格式。这将是一个更干净的实现。为了清楚起见，我将在一个单独的函数中实现这些测试，这个函数有两个针对`petal_width`的测试用例:特性丢失和特性有一个错误的值(“垃圾”)。

我们可以重新运行我们的测试，并验证这些通过。当然，我们还应该尝试将`==`改为`!=`，以验证它们在每种情况下都失败。同样，这将有助于确保我们正在测试我们实际认为我们正在测试的东西。

# 识别问题

现在我们有了测试，我们可能想知道它们是否真的能捕捉到我们代码中的错误。也许，当您创建这些测试并在您的 API 代码上运行它们时，您已经发现了一些问题。如果没有，您可以做一些简单的更改，这些更改会导致一个或多个测试失败(分别进行这些操作):

*   在 API 代码中，将`sepal_length`的`default`(平均插补)值从 5.8 更改为 5.3。
*   在 API 中，放回`petal_width`的平均值插补。这将允许 API 对缺少`petal_width`的记录进行评分。当`petal_width`丢失时，您应该在期望 API 返回“400 错误请求”的测试中看到失败。
*   在 API 中，更改当`petal_width`丢失时发送的错误消息的文本。
*   我们还可以模拟有人意外修改模型并试图部署它的情况。为了测试这一点，我们可以构建您的模型的替代版本，部署它，但是使用原始模型的测试数据文件(JSON)。实现这一点的快速方法是在训练/测试集分割中对`random_state`使用不同的值(例如`random_state=30`)。记得把`joblib.dump()`中的模型输出文件名改成别的(例如*' iris-RF-altmodel . pkl '*)；您需要在 API 中修改`MODEL`来引用这个文件。此外，确保您不要执行生成测试数据文件的代码，因为这些代码会基于替代模型重新构建它们。当您重新运行您的测试时，您可能会在所有测试中看到失败，除了那些当`petal_width`丢失或无效时拒绝请求的测试。如果您的测试仍然通过，尝试另一个`random_state`,因为模型可能是等价的，因为训练集可能保持不变或者变化不足以改变模型。

我们的测试肯定能发现问题，但是我们能发现所有的问题吗？简单的答案是，我们可能没有捕捉到一切。在创建这个帖子的时候，我试着将`sepal_width`的平均插补(默认值)从 3.0 改为 3.1。当我重新测试时，他们都通过了。也许这没什么大不了的；也许我们的模型对平均值附近`sepal_width`的微小变化并不敏感。这是最不重要的特征。然而，我们将我们的测试集用于测试用例，这些数据点不一定落在不同类的边界附近。如果我们有更多的测试用例或者更好的测试用例，我们可能已经能够捕捉到这种类型的 bug。

# 包扎

我们已经看到自动化测试可以帮助我们找到代码中的错误。虽然我们从测试单个 API 调用开始，但我们能够快速转向运行大量测试用例的框架，并且它只需要添加一点额外的代码。

测试是一个重要的话题，所以我们可能会在以后的文章中再次讨论这个话题。这里有一些我们没有涉及的领域的快速预览，但将来可能会涉及:

*   测试速度:当你改变代码时，经常运行测试是有益的。这使得在重构现有代码或添加新功能时更容易及早发现错误。如果测试需要一段时间来运行，开发人员就不太可能这样做。一种方法是将快速运行的测试与耗时较长的测试分开。然后，开发人员可以在进行增量更改时运行快速测试套件，并在将更改集成回主存储库之前运行完整套件。
*   嘲讽和修补:我们发现对`sepal_width`的缺省值(平均插补值)的微小改变不会导致我们的测试失败。如果这是一个需求，我们可以在评分过程中使用补丁拦截对`model.predict_proba()`的调用，以验证正确的值被替换。
*   Fixtures:这是 pytest 的一个特性，您可以创建、配置和销毁资源，以便建立一个干净且一致的环境，每个测试都可以在其中运行。如果你熟悉许多单元测试框架中的“安装”和“拆卸”, fixtures 就是这种思想的延伸。
*   子系统的集成:目前，我们只有模型和 API。在随后的文章中，我们将看看如何添加一个数据库后端和一些其他服务。我们如何测试这些？我们如何测试整个系统？
*   持续集成工具:这些工具有助于更容易地将代码集成到共享存储库中。通过使它变得更容易，我们希望它能更经常地、更小规模地完成。这些工具的一个共同特征是，它们会在每次提交拉请求时自动运行测试套件，并且通过/失败的结果会提供给评审者。
*   测试覆盖率:我们测试了我们代码的每一行吗？我们可以创建一个测试覆盖报告来帮助我们了解哪些代码行在测试期间运行了，哪些没有运行。这不会告诉我们是否已经处理了所有可能的情况，但是它可以给我们信息，让我们知道我们的测试套件在哪里不足。
*   高级测试方法:我们不太可能涵盖这些主题，但是我想提到它们。使用基于属性的测试(参见[假设](https://github.com/HypothesisWorks/hypothesis-python)，您创建参数化测试，框架为您生成一组广泛的测试用例。这可以导致更全面的测试，而不需要你想出所有的边缘情况。突变测试(见[宇宙射线](https://github.com/sixty-north/cosmic-ray))采用了一种非常不同的方法。它与您现有的测试用例一起工作，并实际上以某种小的方式(突变)修改您的源代码(测试中的代码)，以查看您现有的测试是否失败。如果所有的测试仍然通过，你的测试代码是不完整的，因为它不能找到由变异引入的错误。

## 脚注

1.  不确定是否有人真的会这么说，但有时你会遇到这样想的人。
2.  你可能在想，*“我的用户不需要知道每个类的底层分数；他们只需要预测。那么，为什么我只是为了测试而改变我的回答呢？”*好问题！我们采用这种方法是出于方便和清晰的考虑。在实际实现中，您使用一个标志来指定是否返回基础分数，并且可能将此功能限制于某些用户。我们还可以使用模仿和修补来访问分数，而无需修改响应来包含模型分数。
3.  如果你在运行`pytest`时遇到问题，请尝试以下选项:`py.test`或`python -m pytest`。