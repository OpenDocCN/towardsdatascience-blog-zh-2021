# 从设计到部署:机器学习应用的整个生命周期

> 原文：<https://towardsdatascience.com/from-design-to-deploy-the-whole-lifepath-of-a-machine-learning-app-e9e0357525cc?source=collection_archive---------29----------------------->

## 设计、开发、部署和维护机器学习应用程序的逐步描述。通过一个例子。用代码。

你为什么要读一篇 25 分钟的阅读时间中等的帖子？好吧，在这里我试图浓缩一个机器学习项目的完整路径，从数据分析到在 AWS EC2 上的部署。

# 介绍

在纯真年代，在学习完我们的第一门 ML 课程后，我们都认为要成为一名数据科学家，在笔记本上工作就足够了。

一旦我们离开幼儿园，我们就知道这与事实相去甚远。

如今，除了机器学习算法的知识(或者更常见的图书馆用法)，数据科学家还必须具备许多其他技能。

这篇文章旨在通过一个完整的机器学习任务示例(我将在模型部分保持简单),从设计到部署和维护。

我这样做是因为我和许多数据科学家谈过话，也和他们一起工作过，他们在职业生涯中并不年轻，我对这个角色有很大的困惑。数据科学是一项伟大的活动，但通常数据科学家的代码很难投入生产(甚至很难阅读)，因为它们是在没有考虑模型的真实用途的情况下编写的。
我发现这是一个尊重的问题——对于在数据科学家之后从事模型工作的所有可怜虫来说——提供一个可靠而简单的模型部署。
此外，如今 docker 是所有数据科学工作机会中高度要求的技能，所以为什么不花点时间看看 docker 是如何成为机器学习应用程序的呢？

![](img/0e1f52f859a61d553ec7ee8fc5af6c4c.png)

用来说明机器学习项目生命周期的无处不在的图像。图片由【[https://www.jeremyjordan.me/ml-projects-guide/](https://www.jeremyjordan.me/ml-projects-guide/)提供

上图不错，但是我觉得可能显得有点太抽象了。我会用我糟糕的绘图能力重新设计如下。

![](img/13e754adef2f6752502babc027fee720.png)

简化的模型生命周期。作者画得不好的图像。

上面的图片也是我们在这篇文章中要遵循的模式。

# 问题定义

具体来说，让我们从定义我们的问题开始。想象一下，你在一家咨询公司工作(就像我一样)，你的销售同事来找你说“嘿，伙计，一家房地产代理公司想要一个房价评估员，我们能做吗？”。
我希望你的回答是“没问题”，那么我们可以开始了。

在深入研究这个问题之前，让我先观察一下。应该有一个更早的(也是重要的)步骤:数据收集(可以是抓取、客户端数据库查询等。)，这在这里将被忽略，因为这会导致我们偏离轨道。

# 0.初步分析

在这一部分之下，通常是数据收集、标记、数据分析、一些更好地理解我们正在处理的问题的可视化、数据清理等。

由于这些过程受要处理的具体数据的影响很大，我们尽可能保持简单。我们将简要描述探索性数据分析，仅此而已。

## 数据收集

我知道，我写了我会忽略这一部分。我没有撒谎。
在这里，我想象客户给了我们一个标签很好的数据集(这个[是](https://www.cs.toronto.edu/~delve/data/boston/bostonDetail.html))。在真实(艰难)的生活中，你通常会花很多时间自己标记数据，但我们现在仍然生活在童话中。

因此，让我们从简单地将数据导入熊猫数据框架开始。

`print(df.head())`命令打印以下内容

![](img/c7d70cae6057074f866d61b7cf6070fc.png)

可以查看打印数量`boston_dataset.DESCR`以获得功能的简要描述。

```
**CRIM**: Per capita crime rate by town
**ZN**: Proportion of residential land zoned for lots over 25,000 sq. ft
**INDUS**: Proportion of non-retail business acres per town
**CHAS**: Charles River dummy variable (= 1 if tract bounds river; 0 otherwise)
**NOX**: Nitric oxide concentration (parts per 10 million)
**RM**: Average number of rooms per dwelling
**AGE**: Proportion of owner-occupied units built prior to 1940
**DIS**: Weighted distances to five Boston employment centers
**RAD**: Index of accessibility to radial highways
**TAX**: Full-value property tax rate per $10,000
**PTRATIO**: Pupil-teacher ratio by town
**B**: 1000(Bk — 0.63)², where Bk is the proportion of [people of African American descent] by town
**LSTAT**: Percentage of lower status of the population
**MEDV**: Median value of owner-occupied homes in $1000s
```

最后一个是我们的目标，它不包括在数据帧中。的确，它可以通过`boston_dataset.target`进入。

## 探索性数据分析

这是你发现和熟悉你的数据的部分。

首先，查看数据中是否有缺失值是一个很好的做法。我们使用`isnull()`来计算每个特性缺失值的数量。在这种情况下，您可能不会惊讶地发现没有丢失值。
现在，我们将使用一些可视化方法来描述数据的分布，并理解目标变量和特征变量之间的关系。
我们发现这个分布或多或少是正态的，很少有异常值。

![](img/5d0ce18340bba236741126da7172a1e8.png)

为了更好地研究特征变量，一种选择是相关矩阵或相关图。因为我们有相当多的相关矩阵，我们更喜欢相关矩阵，更容易可视化。

![](img/d4d22560c05931ace01b4240ab4bd17a.png)

我们可以注意到的是:

*   一些特征之间以及与目标变量之间都有很强的正相关性。如果我们想要一个线性模型，那么我们合理地选择这样的特征。通过查看相关矩阵，我们可以看到`RM`与目标有很强的正相关性(0.7)，而`LSTAT`与目标有很强的负相关性(-0.74)。
*   对于线性回归模型，检查多重共线性可能很重要。在我们的例子中，例如，`RAD`和`TAX`是真正相关的(直观上它们可以交换而不会丢失太多信息)。
*   为了捕捉更高的相关阶，可以研究特征平方的相关矩阵，或者三次幂的相关矩阵，等等。
*   人们可以用其他一些更系统的方法(如 t-SNE、主成分分析等)来减少特征。)并研究简化特征与目标变量的相关性。

基于以上观察，我们可以将`RM`和`LSTAT`作为我们的特征。然而，由于这不是本帖的兴趣所在，我们将忽略这一点，使用所有的功能。

## 为模型准备数据

我们可以使用流行的 sklearn 方法将数据收集到列车测试分割中。

强烈建议采取最后一个步骤:立即清理您的数据管道。为此，我们将编写一组简单的函数。关键词是“*模块化*”，因为你想在其他项目中重用这个代码，你也可能想保持这个模型，也许添加/删除功能。

注意我们是如何使用字典为`import_data`函数输入参数的。这是因为我们想通过 HTTP 协议用`json`或`yml`文件来提供这样的功能。

因为我已经记住了我想要使用的模型，所以我进一步考虑数据标准化。在向神经网络输入分布在不同区间(大小和中心点)的数据时，可能会出现一些问题。原则上，一个训练有素的模型应该适应这种差异，并对它们进行适当的编码，然而，这需要大量的数据，并且在故事的结尾，使模型更难训练，更不稳健，最终不精确。
处理此类数据的普遍最佳实践是进行特征式归一化:对于输入数据(输入数据矩阵中的一列)中的每个特征，我们将减去特征的平均值并除以标准偏差，因此特征将集中在 0 附近，并具有单位标准偏差。

因此，我们将上面的函数修改如下，

# 1.模型结构

在这个阶段，我们设计模型并训练它，以便评估它的性能。通常，这部分是在笔记本上完成的，不用太担心命名约定、函数、OOP、注释等。然而，我的建议(也是在这里)是尽可能的干净，因为这可以节省你数小时的时间将代码转换成可读的东西。

## 算法设计

之前执行的数据分析应驱动该器件。例如，基于决策树的算法或 XGBoost 将允许您获得很好的结果。

通常这部分是由试错法构成的。通常，您不希望在整个数据集上训练您的候选模型(当您有大量数据时)，因此一种解决方案可能是对您的训练数据进行随机采样，并训练一组模型来比较它们的性能，并根据您选择的指标选择最佳模型。

此外，为问题选择正确的度量标准至关重要。当然，这取决于问题，但是，这里我们可以陈述一个好的指标必须满足的三个属性:

1.  必须是**单编号**。你想做简单的比较。
2.  它必须在某些数值性能方面满足**。*即*要测量算法数值性能。**
3.  **必须是**优化**。*即*它必须测量算法结果。**

**典型的例子是:我们挑选具有最高*F1*-得分和推理时间低于 10 毫秒的模型。**

**当然，这是初步分析。通常，在选择“正确的”之前，您可能会经历几个不同型号的完整培训。
在这里，我们假装我们已经正确地做了所有的事情，我们参考这篇伟大的文章来了解细节。**

**<https://medium.com/structuring-your-machine-learning-projects/satisficing-and-optimizing-metric-24372e0a73c>  

假设根据我们的分析，我们选择了一个神经网络模型。同样，我的建议是写你的笔记本单元格时要时刻记住你要在生产中转换这些代码，所以要干净！

你可以开始识别一种模式。配置字典将是一个 json(或 yml，或其他类似格式)文件。

![](img/4becd373eba9bb157c1ff5f3d41b28f4.png)

一旦模型被定义和编译(如果你是 PyTorch 的人，不需要编译)，我们就准备在我们的数据上训练它。

## 首次培训

让我们根据训练数据拟合模型，并在测试中测量性能。严格地说，我们还应该有一个验证集，但是我们以后会有时间来担心这个问题。在这一阶段，我们只想以一个工作模型和它的性能度量来结束。这是过度限制的*快速而肮脏的*方法，与你的代码有多乱没有关系。

## 模型评估

查看学习曲线总是有用的(在回归情况下甚至更有用)。

![](img/a449dd760f5e8de3ac382a6e457dd52b.png)![](img/fdf5b4485af66862697e457aeafd02df.png)

我们可以看到我们的模型仍然在最小化损失函数。这将建议增加纪元的数量。
我们也有替代方案，*即*增加我们网络的模型化能力，即修改其架构、更多隐藏单元和更多隐藏层。我们将在下一部分尝试这两种方法。

总之，一个细心的读者可能已经注意到，测试损失似乎比培训损失要低。这是因为 Keras 计算两种损失的方式略有不同。详情参考本帖。

<https://www.pyimagesearch.com/2019/10/14/why-is-my-validation-loss-lower-than-my-training-loss/#:~:text=The%20second%20reason%20you%20may,is%20measured%20after%20each%20epoch>  

## 超参数选择和优化

在上一节中，我们最后给出了两个可能的选项来改进我们的模型。

我们可以尝试两种方法，看看哪种解决方案在测试集上给出了最好的结果。我们实际上使用我们的测试集作为验证集。

之前的车型配置给了我们上面的剧情。让我们保持一切不变，只是将纪元的数量增加到 100。
唯一要做的就是修改`config`变量，如下所示。

```
config = {
           'data': boston_dataset,
           'train_test_ratio': 0.2,
           'model_config': {
               'model_name': 'house_pricing_model',
               'layers': {
                        'first_layer': 12, 
                        'second_layer': 5, 
                        'output_layer': 1
                       },
                'activations': ['relu', 'relu', None],
                'loss_function': 'mse',
                'optimiser': 'adam',
                'metrics': ['mae']
                },
            'training_config': {
                'batch_size': 10,
                'epochs': 100
            }
           }
```

其他一切保持不变，我们可以重新初始化我们的模型，并再次运行训练函数。我们得到，

![](img/bb9acf7450c7a7ec33181dc3766502ff.png)

而学习曲线看起来像

![](img/bb9abd2fa04d69b185f16093fed382a5.png)![](img/ddcf124701e4882b303a0bf2d4e24eee.png)

伟大的结果！我们看不到过度拟合问题，损失稳定在 85/90 左右。

为了给误差值赋予一些意义，我们必须查看目标变量。`df.target.describe()`(以及我们之前绘制的直方图)给了我们想要的信息。

![](img/0b885b5b937789d8165b906701e91355.png)

我们可以看到大约 2.3 的平均绝对误差相当于我们数据平均值的大约 10%的误差。

*注意*:为了使我们的误差估计更加稳健，我们应该进行 k 倍交叉验证，并取所有倍的平均值作为平均绝对误差值。为了可读性，我们将忽略这一点。

让我们尝试另一个选项，并修改隐藏单位和隐藏层的数量。

```
config = {
           'data': boston_dataset,
           'train_test_ratio': 0.2,
           'model_config': {
               'model_name': 'house_pricing_model',
               'layers': {
                        'first_layer': 64, 
                        'second_layer': 64,
                        'third_layer': 32,
                        'output_layer': 1
                       },
                'activations': ['relu', 'relu', 'relu', None], # Regression problem: no activation in the last layer.
                'loss_function': 'mse',
                'optimiser': 'adam',
                'metrics': ['mae']
                },
            'training_config': {
                'batch_size': 10,
                'epochs': 100
            }
           }
```

我们加了一层，增加了每层的隐藏单元数量。我们用这样的配置来训练吧。

![](img/b9ab7b5437d21ec4d3ee29f737a698ab.png)

数字略有提高！我们来看看学习曲线。

![](img/c58b88af7b9f05ca8c3d74e9f51f88c1.png)![](img/89f8a1e28e7a2d6cd8b201ab57276015.png)

它们很好，但我们开始看到任何数据科学家的克星:过度拟合。大约在 15/20 时期，训练损失系统地低于测试损失，并且这种差异随着时期而增长。
我们可以减少历元的数量，或者插入一个丢弃层来减少过度拟合。以此类推，直到我们对结果满意为止。

出于我们的目的，我们将在这里停下来，选择以下配置。

```
config = {
           'data': boston_dataset,
           'train_test_ratio': 0.2,
           'model_config': {
               'model_name': 'house_pricing_model',
               'layers': {
                        'first_layer': 64, 
                        'second_layer': 64,
                        'third_layer': 32,
                        'output_layer': 1
                       },
                'activations': ['relu', 'relu', 'relu', None], 
                'loss_function': 'mse',
                'optimiser': 'adam',
                'metrics': ['mae']
                },
            'training_config': {
                'batch_size': 10,
                'epochs': 17
            }
           }
```

**Optuna**

另一个更有效的选择是用库寻找最佳超参数配置。Optuna 就是这样一个库，最近他们还推出了一个寻找最佳模型架构的实验特性。我的建议是尝试一下。

<https://medium.com/optuna/using-optuna-to-optimize-tensorflow-hyperparameters-57b6d4d316a2>  

超参数优化是一个非常有趣的话题，它本身需要一系列的帖子，但是对于本文，让我们按照我们的指导示例，继续构建培训管道。

## 管道建设

因为我想把重点放在服务部分，这里我将保持培训管道简单，但是，你可以让这成为你想要的复杂。例如，您可以构建另一个应用程序来构建和训练您的模型。
严格地说，除了保存模型文件并导出之外，还可以在笔记本中训练模型。这是可以做到的，但是，我建议不要这样做，因为笔记本没有版本控制系统，你无法检查你是否引入了一些不兼容的问题，等等。

这里我们将采用一个折中的解决方案:我们将创建一个脚本，将一个配置文件作为输入(在模型的后续版本中您应该修改的唯一一个文件),并返回一个保存的模型文件。
因为我们在书写笔记本的单元格时投入了一些精力，所以脚本很容易创建。

我们将利用一个辅助库，因为我们的脚本将由命令行界面执行，并且我们希望将配置文件路径作为参数传递，而不接触脚本代码。

这个脚本非常简短，非常具有示意性。

注意，我们导入了一个`utils`模块，它是我们上面定义的函数的集合。还要注意，这不是最干净的方式，我们可以(通常我们应该)模块化更多，甚至更好，为我们的模型创建收集方法的类。

作为奖励，你可以将这个训练脚本转换成一个命令，*也就是*你会变成

```
python3 -m estimator train --conf config.json
```

到…里面

```
estimator train --conf config.json
```

要做到这一点，你可以阅读这个不错的中型职位。

<https://christopherdoucette.medium.com/turning-python-scripts-into-cli-commands-aecf56dfda18>  

`config.json`文件包含定义和训练我们模型的所有信息。

## 模特培训

最后，我们准备对我们选择的模型进行适当的训练。一旦模型被拟合，我们保存它以便导出，并将其提供给下一节要用到的 API。

这是通过命令行 shell 中的一个命令自动完成的:

```
python3 -m estimator train --conf config.json
```

*注意*:为了不使这篇太长的帖子变得更长，这里我不考虑 Keras 中最伟大的特性之一:回调。可以设置一个度量并在训练期间监控它，以便当你的模型在多个时期内没有改善时自动停止拟合，或者自动保存最佳度量结果，不管它已经到达哪个时期，等等。一个很好的参考来源是 Tensorflow 的官方文档[。](https://www.tensorflow.org/guide/keras/custom_callback)

# 2.部署

既然您的模型已经被训练并保存在本地的某个地方，我们就能够开始部署的迷人篇章了。

通过这个黑暗和邪恶的词，我们简单地意味着使我们的模型可用和可达(从用户或其他应用程序)以便提供预测。

方案很简单:我们想把我们的模型放在某个地方，让它接收*请求*并给出预测。在我们的示例中，我们希望向模型中输入房屋(更恰当地说是描述房屋的特征向量),并获得房屋的*响应*价格预测。

因为我们不想成为唯一的用户或我们的模型，所以我们必须选择一个可以从我们的本地系统外部公开访问的协议(或者至少对于某个用户列表)。

现代计算机科学中的一个主要观点是<https://www.sumologic.com/glossary/container/>**。*这里，我们指的是将一个应用与它所需的所有相关配置文件、库和依赖项捆绑在一起，以便在不同的计算环境中以高效、无错误的方式运行。沿着这条路走下去，我们将复杂的应用程序拆分成包，然后在 other 中将容器组合在一起，这样就不用担心它们将在哪个系统上执行了。*

*了解容器的一个很好的来源是下面的 nice medium 帖子。*

*<https://medium.com/cycleplatform/a-brief-introduction-to-containers-d34e64e61bc1>  ![](img/33524c046291c73dcb4719f02f5ce8cb.png)

作者画的一幅更糟糕的图像。我们根据下面的步骤添加了数字。

本着这种精神，并遵循上图中的方案，我们必须:

1.  创建一个 API 来服务我们的模型。我们将通过使用 FastAPI 来实现这一点。
2.  封装我们的 API。我们将由码头工人来做这件事。
3.  在某些服务上推送 docker 图像。我们将使用 AWS ECR。
4.  在某个服务上部署包含应用程序的 docker 容器。我们将使用一个虚拟机服务:AWS EC2。

请注意，这些步骤与要部署的任何其他软件应用程序或多或少是相同的。

> 数据科学家是开发人员，必须能够遵循典型的软件生命周期。

事实上，人们也可以遵循这些完全相同的步骤，并部署“hello world”API(*即*待办事项列表)。这实际上是一个非常好的练习，可以用来练习这样的程序。要开发待办事项 API，可以参考下面的文章。

<https://surikavii.medium.com/making-a-todo-app-from-a-beginners-perspective-part-1-intro-to-fastapi-5006abbcb7a2>  

不要害怕，这些部分很简单。这里没有需要考虑的分析，没有试错法。我们在这里要定义的对象主要是由一代又一代的编程人员复制粘贴下来的代码。

## API 创建

我们将使用 FastAPI，这是一种可能的 web 框架，您可以使用它将 Python 代码转换为通过 HTTP 协议通信的应用程序。

FastAPI 的优点是速度快(几乎像编译语言一样)、容易学习、使用广泛(大量在线支持)，并且它为你的应用程序构建了方便的在线文档。

所以让我们来构建我们的应用程序。
我们需要创建以下目录结构:

```
├── __init__.py├── ai|  ├── __init__.py|  ├── regressor.py|  └── services.py├── main.py├── model|  └── model.h5└── schemas|  ├── __init__.py└──└── schemas.py
```

这并不是绝对必要的，你可以把所有东西放在同一个文件中(愿上帝原谅你)，或者你可以使用框架来为你构建 Python 项目，*例如* [千篇一律的](https://github.com/cookiecutter/cookiecutter)。然而，我强烈建议遵循固定的模式来适应这种结构。

我们将分别研究每个文件。

让我们从 main 开始，这是我们将要启动来执行我们的应用程序的文件。

**main.py**

你可以看到，代码是不言自明的，即使不知道该功能的确切实现，你也可以说这个应用程序在高层次上做了什么。它接收数据(类型为`InputData`)并返回包含输入数据和预测的字典(实际上是类型为`ResponseDataAPI`，参见函数装饰器)。

在 FastAPI 中，decorators 用于定义 HTTP 方法，在我们的应用程序中，我们只需要一个 get(检查应用程序的状态)和一个 post(发送数据和获得预测)。

我们可以通过终端命令在本地运行应用程序

```
uvicorn main:app --reload
```

打开浏览器，进入本地地址[http://127 . 0 . 0 . 1:8000/docs](http://127.0.0.1:8000/docs)我们会看到自动交互 API 文档(由 [Swagger UI](https://github.com/swagger-api/swagger-ui) 提供)。您可以在[http://127 . 0 . 0 . 1:8000/redoc](http://127.0.0.1:8000/redoc)访问替代文档页面。

![](img/08980666c060de9026be6c91bb06417c.png)

这是你应该看到的。图片由作者提供。

有大量的功能需要探索(一个很好的参考总是写得很好的 [FastAPI 文档](https://fastapi.tiangolo.com/)，这里我只想提到两个主要方面:
*1 .*API 自动附带一个 web 界面。这允许你展示你的模型的工作原理，甚至向一些非技术人员展示。这是免费的。
*2。*您有一个 URL，可以用 json 字符串进行查询，并以相同的格式进行响应。这在生产中非常有用，*即*你可以在网络服务器上托管这个应用程序，并开始与其他应用程序集成，这是生产程序的关键。

这个第二方面可以由终端明确询问

```
curl -X 'POST' \
  '[http://127.0.0.1:8000/](http://127.0.0.1:8000/docs)v1/services/predict' \
  -H 'accept: application/json' \
  -H 'Content-Type: application/json' \
  -d '[
  {
    "CRIM": "float", 
    "ZN": "float", 
    "INDUS": "float", 
    "CHAS": "int", 
    "NOX": "float", 
    "RM": "float", 
    "AGE": "float", 
    "DIS": "float", 
    "RAD": "float", 
    "TAX": "float",
    "PTRATIO": "float", 
    "B": "float", 
    "LSTAT": "string"
  }
]'
```

注意，我们必须在 json 中提供例子，收集我们训练模型的所有特性。我们可能已经更改了名称，但是由于缺乏想象力，我们让它们相对于初始数据集保持不变。

因此，例如，为了得到一个预测，我们可以给出终端命令

```
curl -X 'POST' \
  '[http://127.0.0.1:8000/](http://127.0.0.1:8000/docs)v1/services/predict' \
  -H 'accept: application/json' \
  -H 'Content-Type: application/json' \
  -d '[
  {
    "CRIM": 0.09178, 
    "ZN": 0.0, 
    "INDUS": 4.05, 
    "CHAS": 0, 
    "NOX": 0.510, 
    "RM": 6.416, 
    "AGE": 84.1, 
    "DIS": 2.6463, 
    "RAD": 5.0, 
    "TAX": 296.0,
    "PTRATIO": 16.6, 
    "B": 395.50, 
    "LSTAT": 9.04
  }
]'
```

API 响应将打印在屏幕上

```
{
 "inputData": 
  [{
    "CRIM": 0.09178, 
    "ZN": 0.0, 
    "INDUS": 4.05, 
    "CHAS": 0, 
    "NOX": 0.510, 
    "RM": 6.416, 
    "AGE": 84.1, 
    "DIS": 2.6463, 
    "RAD": 5.0, 
    "TAX": 296.0,
    "PTRATIO": 16.6, 
    "B": 395.50, 
    "LSTAT": 9.04
  }],
 "predictions": [{"Prediction price 0": 26.900973}]
}
```

**ai/regressor.py**

这是包含我们模型的类定义的文件。我们定义了`PriceEstimator`类并给出了最少的方法。通常，你不希望有人可以从 API 训练你的模型，因此这个对象没有`train`方法。有`predict`一千，因为你想暴露你的预测模型。

您可以看到我们在数据管道中使用的功能。任何程序员的动力都应该是懒惰。
在这种情况下，一种更简洁的方法是从我们之前定义的模块中简单地导入相同的函数，尽管通常情况下，您希望将代码的训练部分和预测部分很好地分开。

**ai/services.py**

该文件包含从预先训练的保存文件中返回预测和模型载荷的代码。注意我们是如何从环境中读取`MODEL_FOLDER`变量的。这意味着(要在本地运行应用程序)我们必须将模型路径添加到环境中。如何做到这一点取决于操作系统，快速的谷歌搜索会给你答案。在 linux/mac 上，这可以通过以下方式在终端中完成

```
export MODEL_FOLDER='path/to/your/model'
```

**schemas/schemas.py**

这个文件定义了应用程序输入和输出的自定义类型(在 Python 中，任何对象都是一种类型)。

众所周知，Python 是一种动态类型的编程语言，这意味着变量的类型不是声明的，而是通过它们的值来推断的，并且还可以在程序执行期间改变。
这种行为真的很方便，因为你不需要定义变量类型，也不需要注意不要给不同类型的值分配相同的变量名，等等。然而，这也是 Python 与其他静态类型语言(如 C/C++、Go 等)相比如此缓慢的主要原因之一。
关于详细的讨论，人们可以阅读[这篇极其有趣的参考文献](http://gallium.inria.fr/~remy/mpri/cours1.pdf)或[相关的维基百科页面](https://en.wikipedia.org/wiki/Type_system)。

我们已经指出，我们想要使用 FastAPI 这样的 web 框架的原因是，它几乎与编译语言一样快，这是通过允许稍微修改类型声明的行为来实现的。
因此，我们将定义和设置输入和输出数据的固定类型。

这是由`[typing](https://docs.python.org/3/library/typing.html)` [库](https://docs.python.org/3/library/typing.html)完成的，并不是真正的类型声明，更多的是一种暗示。通过这种方式，我们允许 Python 解释器为“声明的”类型分配适量的内存。

我们定义了一个可以在 FastAPI 的交互式文档页面上显示的示例。

请注意我们是如何定义 main.py 函数中声明的类型的。

## 对接 FastAPI

我们现在准备好对接我们的应用程序。
我们已经讨论了为什么容器在现代软件行业中至关重要。在这里，我只想综合 docker 解决的一个问题。

> 看我的模特有多酷。我会把脚本发给你，这样你就可以在你的机器上执行了。
> *B* :收到，但是导入 xxx 库时出错，我通过堆栈溢出解决了，现在我的数字和你的不一样。
> *A* :过来看看我的屏幕，这里果然好用。

如果 A 给 B 发送了一个 docker 图像，这种情况就不会发生。

让我们看看如何对接我们的 API。要对应用程序进行 dockerizeize 化，首先，您需要一个 docker 文件

Dockerfiles 描述了如何为容器组装私有文件系统，还可以包含一些元数据，描述如何基于映像运行容器。(回想一下图像和容器的区别，[此处](https://phoenixnap.com/kb/docker-image-vs-container))。

我们还将使用一个名为`requirements.txt`的文件，它是我们在项目中使用的所有库及其版本的列表。我们希望我们的应用程序在任何机器上运行都没有兼容性问题。

如前所述，接下来的部分可以非常简单地呈现出来，在这里，我密切关注下面的精彩帖子，只是根据当前的情况调整命令。

<https://medium.com/@meetakoti.kirankumar/deploying-fastapi-web-application-in-aws-a1995675087d>  

*i)创建一个 docker 文件*

这里我们让我们的应用程序运行在地址[*http://0 . 0 . 0 . 0:5000*](http://0.0.0.0:5000)。
注意我们是如何将模型位置定义为环境变量的:这是因为我们的应用程序从环境中读取这样的值。

*ii)构建 Docker 映像:*

这是通过行命令完成的

```
docker build -t price_estimator_api:latest .
```

*iii)运行 docker 镜像*

现在简单的部分:让我们运行 docker。

```
docker run -p 5000:5000 price_estimator_api:latest
```

在 [*页面打开浏览器即可验证 app 运行 http://localhost:5000/docs*](http://localhost:5000/docs)*。*

想了解更多关于 Docker 的知识，可以看看官方教程。

<https://docs.docker.com/get-started/overview/>  

## 推进亚马逊弹性容器注册

现在，您已经验证了您的应用程序容器正在本地运行，我们准备将它移动到某个可访问的“位置”。这可以是任何具有公共(或至少非本地)访问权限的服务器。这里我们想使用 AWS EC2 服务。

我们不想考虑机器配置、可伸缩性等问题。因此，我们将把 docker 图像放在 docker 注册中心，即 Amazon 弹性容器注册中心(ECR)上。这是一个完全管理的 [Docker](https://aws.amazon.com/docker/) 容器注册中心，使开发人员可以轻松地存储、管理和部署 Docker 容器映像。

首先，为了将 docker 映像推送到 ECS，您需要安装和配置 AWS CLI。AWS 网站上有相关说明。

  

配置 AWS CLI 的一种方法是从 AWS 控制台中的**身份和访问管理(IAM)获取访问密钥 ID 和秘密访问密钥。**

您只需创建新的访问键并存储它们以备将来之用。

![](img/4a86434da88b709c3a7457e367dac356.png)

在终端中，您可以配置访问权限。

```
aws configureAWS Access Key ID [None]: ***AKIAIOSFODNN7EXAMPLE***
AWS Secret Access Key [None]:***wJalrXUtnFEMI/K7MDENG/bPxRfiCYEXAMPLEKEY***
Default region name [None]: ***eu-west-3***
Default output format [None]:***json***
```

您现在可以使用以下命令访问 ECR，提供**地区**和**账户 id** 的详细信息。

```
aws ecr get-login-password  --region **region** | docker login --username AWS --password-stdin **aws_account_id.**dkr.ecr.**region**.amazonaws.com
```

如果一切顺利，您应该会在控制台上看到**“登录成功”**。

现在，您可以在 AWS 注册表上推送您的映像。
首先，创建一个资源库。

```
aws ecr create-repository --repository-name price_estimator_api
```

标记您的图像，以推动注册表。

```
docker tag price_api:latest **aws_account_id**.dkr.ecr.**region**.amazonaws.com/price_estimator_api
```

最后，运行命令将映像推送到 ECR 上。

```
docker push **aws_account_id**.dkr.ecr.**region**.amazonaws.com/price_estimator_api
```

## 在 EC2 上部署

在将 docker 映像推送到 AWS ECR 之后，我们现在准备创建 Amazon EC2 实例，它可以为您的 web 应用程序提供服务。AWS 在免费层中提供了许多实例，在这种情况下我选择了一个 Linux 实例(您也可以使用其他实例，但一定要记住相应地更改配置)。

*i)去亚马逊控制台；选择 Amazon Linux EC2 实例。*

*ii)选择通用 t2.micro 实例类型，例如。*

![](img/773367c37563ea1afefcb975db95cf57.png)

*iii)通过使用自定义 TCP 作为类型和端口作为 5000(我们公开了 API 的该端口)添加规则来更改安全设置，并单击 review 并启动实例。这些配置是我们的 web 应用程序运行所必需的。*

![](img/f82b67ab471fb57ca69c4a8decf7c84f.png)

*iv)从下拉菜单*创建新的密钥对*中选择，创建一个新的密钥对，为密钥对提供一个名称，然后下载。*

由私钥和公钥组成的密钥对是一组安全凭据，用于在连接到实例时证明您的身份。

您应该能够看到实例正在运行。

您下载了密钥对，所以需要一个`.pem`密钥。假设我们给出了名称`priceestimator.pem`，我们可以使用这样一个文件通过安全的 ssh 连接登录到 EC2。

```
ssh -i priceestimator.pem ec2-user@**ec2-18-221-11-226.us-east-2.compute.amazonaws.com**
```

登录后，我们在 EC2 上安装 docker，配置 AWS access 以授予机器访问 docker 映像的权限，并将 docker 容器拉至机器。

AWS 配置如上开始，

```
aws configureAWS Access Key ID [None]: ***AKIAIOSFODNN7EXAMPLE***
AWS Secret Access Key [None]:***wJalrXUtnFEMI/K7MDENG/bPxRfiCYEXAMPLEKEY***
Default region name [None]: ***eu-west-3***
Default output format [None]:***json***
```

然后我们运行以下命令来添加 ec2 用户，以便在 Linux 机器上执行 docker 命令。

```
sudo groupadd dockersudo gpasswd -a ${USER} dockersudo service docker restart
```

退出实例，并再次通过 ssh 进入 EC2 实例。

```
ssh -i priceestimator.pem ec2-user@**ec2-18-221-11-226.eu-west-3.compute.amazonaws.com**
```

登录 Amazon ECR 注册表:

```
aws ecr get-login --region **region** --no-include-email
```

输出

```
docker login -u AWS -p <password> -e none https://<aws_account_id>.dkr.ecr.<region>.amazonaws.com
```

复制上面的输出，并在命令行中运行它。成功登录 AWS ECR 后，您可以在控制台中看到“登录成功”。

从 EC2 上的 AWS ECR 中提取 docker 图像。

```
docker pull **aws_account_id**.dkr.ecr.**region**.amazonaws.com/price_estimator_api:latest
```

在 Linux 机器上运行 docker 容器

```
docker run -p 5000:5000 **aws_account_id**.dkr.ecr.**region**.amazonaws.com/price_estimator_api
```

最后，我们可以从实例详细信息页面获取 IPv4 公共 IP，并在浏览器中启动它时添加端口 5000。这将显示在 EC2 上运行的 API。
现在你可以用一个更容易理解的 URL 来代替这样的 IP 地址。

尽情享受吧！

# 4.维护

最后是甜点:维护部署的应用程序至关重要。数据分布的变化可能导致模型没有预测能力。

现在你的模型工作正常，你真幸运。你很高兴，并准备为另一位客户和另一位模特重复这个故事。

然而，房地产代理机构开始在一个非常不同的位置出售一些房子。他们使用你的模型，他们发现你的估计完全错误！他们对你的公司很生气，你的老板开始数落你糟糕的工作。

你会怎么处理这件事？

幸运的是，通过像你所做的那样行动，你可以很容易地部署一个新的模型，通过对新数据进行训练，使你的模型在新数据分布上更加准确。让我们看看怎么做。

我们之前所做的不会丢失。我们可以加载当前模型，并将其用作新列车的起点。我们利用以前学习的权重，并简单地根据新数据训练我们的模型。如果有必要(出于某种原因，你想完全改变你的模型)，你也可以开发一个新的模型。如果你编码正确，在整个管道中不会有任何变化。

**加载当前模型** **作为`config.json`中的预训练**。现在您可以重新启动训练脚本并获得新训练的模型。更改保存模型名称以保持版本控制。

**保存新模型**和以前一样，我们将模型保存在 API 将要读取的文件夹中。我们可以在命名约定中添加日期时间参考，以便跟踪版本。

**修改 Dockerfile** 因为我们对应用进行了版本化，所以我们修改了`Dockerfile`以获得要部署的正确模型版本。

最后，**推送 Docker 镜像**，使其如上运行。

*注意:*通常建议将部署部分写成管道，在每次推送某个 git 存储库时自动启动。所有 git 控制系统都提供了安全注册一些存储库环境变量的可能性。人们可以在那里设置一些秘密的 AWS 凭证，并自动完成这一部分。更多信息，请看下面的教程，例如。

  

# 结论

首先，如果你读到这里，让我祝贺你！这个帖子变得比我预期的长了一点。

总之，我试图通过一个具体的例子，根据我的经验总结典型的数据科学项目是如何发展的。我想警告你:这不是完整的故事。我错过和忽略了许多要点，许多警告，许多不同的请求，等等。

例如，另一种可能性可能是根本不用容器和服务，而是用其他语言转换我们的模型，使之易于在线运行。
例如，对于一个 web 应用程序，可以用 JavaScript 转换模型(例如利用在浏览器上运行模型的 TensorFlowJS 或 TorchJS ),然后在某个服务器上建立一个网站。这是一个具体案例的例子。的确，这可以通过网站访问，但如果你需要一个应用程序来访问它呢？还是其他一些非人类的服务？

Docker 是一种更通用和广泛使用的方法，这就是为什么我选择在这里介绍它。
因此，在不完整的情况下，这篇文章旨在以图解的方式说明机器学习项目的生命周期。我希望这能对初级数据科学家有用，他们经常在这个广阔的世界中感到迷失，也希望更多的专家人物能给我一些提示，告诉我我肯定忘记了的要点。

我希望你喜欢读这篇文章！

> 重要的是:我发表这篇文章也是为了得到建议，讨论和了解我的编码中的弱点。请指出任何错误/冗余代码！😫

🤯 ***你见过多次点击“拍手”按钮会发生什么吗？******