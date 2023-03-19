# 房间里的大象:如何编写 PySpark 单元测试

> 原文：<https://towardsdatascience.com/the-elephant-in-the-room-how-to-write-pyspark-unit-tests-a5073acabc34?source=collection_archive---------3----------------------->

## 在 Azure DevOps 中自动化 Spark 单元测试的分步教程

![](img/1d8b2341f1e5714b6d2e6cbd1b99c5a4.png)

照片由[纸张纹理](https://unsplash.com/@inthemakingstudio?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)在[上不飞溅](https://unsplash.com/s/photos/sticky-notes?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

在铺天盖地的信息空间中，存在着选择的悖论。也很难想象实现一个小目标需要付出多大的努力。我们往往会忽略这种努力的好处。

> 该是我们解决这个问题的时候了。

为您的数据管道编写有效且成熟的测试用例通常被认为是一个雄心勃勃的想法或项目本身。这通常是真的，测试数据管道和测试传统软件带来了不同的挑战。尽管前面的任务相当艰巨，但你必须从某个地方开始。让我们从挑选一块馅饼开始。单元测试 Spark ETL！

**在本教程中，我们将做以下工作。**

1.  使用 PySpark 构建一个简单的 ETL。
2.  **使用 python 模块 *unittest 实现单元测试。***
3.  **在 Azure DevOps 中使用 CI/CD 管道运行自动化测试。**

这绝不是编写高级复杂的测试用例来端到端测试数据管道的教程。它只是试图展示您可以相对容易地开始自动化测试，重点是只测试您的转换的功能，而不需要在测试环境中启动集群。

> 对于本教程，假设您对 Python、PySpark、Azure Devops 库、yaml 管道有基本的了解。

## 0.设置

> 如果你想跳到大团圆结局，想要 TL；DR 版本你可以克隆下面的 [git 仓库](https://github.com/xavier211192/Xavier-Az-Learn-PySpark-UnitTests.git)，里面有你需要的一切。

假设我们将在本地 IDE 上开发，然后在 Azure DevOps 管道中的微软托管代理上执行测试，那么准备我们的代码库将是一件好事，因为它将在自动化管道上执行。

克隆或创建您想要在 Azure DevOps 上使用的存储库。按照如下类似的结构准备文件夹和文件。

![](img/ca16a4cf098f478cb8369dae9ffd1f8a.png)

图 1: PySpark 单元测试存储库结构(图片由作者提供)

由于我们对测试 Spark 代码感兴趣，我们需要安装`pyspark` python 包，它与启动和关闭本地 Spark 实例所需的 Spark JARs 捆绑在一起。让我们将这个包作为需求添加到我们的`test-requirements.txt`文件中。如果您的 ETL 依赖于其他 python 包，这里是列出它们的地方。

![](img/0d06a265cda0f659980484a5c0f6f17d.png)

图 2: Requirements.txt 文件(图片由作者提供)

## 1.在 PySpark 中构建一个简单的 ETL 函数。

为了编写测试用例，我们首先需要需要测试的功能。在这个例子中，我们将编写一个执行简单转换的函数。

基本上，ETL 作业必须完成以下工作:

*   **从数据源提取**数据。
*   应用**转换**。
*   **加载**转换后的数据到目的地。

为了保持简单，我们将只关注测试 ETL 过程的转换阶段。

让我们在 *etl.py* 文件中创建上面的片段。

> 数据工程师的面包和黄油！

下图更好地解释了我们希望在此函数中应用的转换。我们想把我们的数据从图 3 转换成图 4。

![](img/43677e35c24b5277ae58178fef4a21fe.png)

图 3:输入数据框(作者图片)

![](img/600e4f037bc0800748af2251873c8e34.png)

图 4:转换后的数据框(图片由作者提供)

> 一张图胜过千言万语。对不对？

## 2.实现单元测试

为了编写 ETL 逻辑的单元测试，我们将使用 python 单元测试框架，这是一个名为 **unittest 的内置 python 模块。**

让我们从在 *test_etl.py* 文件中导入创建测试用例所需的模块开始。

前两条语句导入了 **unittest** 模块和需要测试的实际转换函数。构建 Spark 操作需要其他导入语句。

在 **unittest** 框架中，通过编写你自己的扩展`unittest.TestCase`类的测试用例类来创建一个测试用例。您在这个类中定义的方法引用了您想要运行的单个测试。这些方法的名称必须以单词`test`开头。这个命名约定有助于测试运行人员识别您的测试。让我们从创建测试用例类开始。

与其他测试框架一样， **unittest** 允许你定义设置和拆卸指令，这些指令是简单的，分别在测试之前和之后执行。这些指令可以分为两个层次，或者使用`setUp()`方法，或者使用`setUpClass()`方法。不同之处在于，setUp 方法将针对每个单独的测试执行，而 setUpClass 方法将针对整个测试用例只执行一次。

在我们的例子中，我们需要在运行测试之前建立一个本地 spark 会话。我们使用`SparkSession.builder`方法来做这件事。我们将它作为一个类方法，因为我们希望只初始化一次 *SparkSession* ，而不是为每个测试方法初始化，这样可以节省我们一些时间。我们还定义了一个 tearDown 类方法来在所有测试结束时停止 *SparkSession* 。

既然设置已经处理好了，我们可以专注于本质细节，编写实际的测试方法本身。unittest 的想法是通过模拟输入和预期输出并断言结果的正确性来测试你的代码的功能。

我们将执行以下操作:

1.  准备一个模拟源数据的输入数据框。
2.  准备一个预期的数据帧，这是我们期望的输出。
3.  将我们的变换应用于输入数据框。
4.  断言到预期数据帧的转换的输出。

上面概述的步骤可以通过许多不同的方式实现，您可以从存储在存储库中的文件中读取输入和预期数据，或者通过代码生成这些数据框。为了使本教程简单且可重复，我们将通过代码生成数据框。

在代码片段的步骤 4 中，我们执行两种类型的断言，首先比较两个数据帧的模式，以检查转换后的数据是否具有我们期望的模式，我们还比较两个数据帧中的数据，以检查值是否是我们期望的。这些是我们可以在数据帧上做的最常见的断言类型，但是当然，一旦你完成了这项工作，你就可以编写更复杂的验证。

此时，我们的测试用例已经准备好了。如果您想在本地运行它们，您可以通过终端中的命令`python -m unittest -v` 来运行它们。

## 3.使用 Azure DevOps 管道自动化测试

我们的测试用例现在已经准备好集成到我们的 CI/CD 管道中。对于本教程，我们将使用 Azure DevOps 管道来实现它，但同样可以在其他工具上实现，如 bitbucket-pipelines 或 Github actions。

让我们将下面这段代码添加到我们存储库中的 *azure-pipeline.yml* 文件中。

`vmImage: ubuntu-latest` 指示我们的管道使用 azure 托管的 ubuntu 机器作为执行环境。

`sudo apt-get install default-jdk -y` 将最新的 java jdk 安装到 ubuntu 机器上。为了在本地实例上运行 Pyspark，我们需要安装 java。

命令`pip install -r $(System.DefaultWorkingDirectory)/src/tests/test-requirements.txt` pip 安装需求文件中列出的所有包，在这种情况下，它将安装`pyspark` python 包。

最后，命令`cd src && python -m unittest -v` 基本上筛选`src`文件夹，并执行任何扩展了`unittest.TestCase`类的文件。如果您创建了多个包含测试用例的文件，它们也将被执行。

提交并推动您的更改。一旦一切都设置好了，创建一个 Azure DevOps 管道，并将其挂接到存储库中的 *azure-pipeline.yaml* 文件。

运行管道，当它执行时，您可以在执行日志中验证结果。

![](img/0cec648b258bbc0023fcd217a1c37b23.png)

图 Azure DevOps 管道中通过的单元测试示例(图片由作者提供)

为了检查测试是否在预期失败时失败，您还可以调整预期的数据帧。例如，如果您期望一个列的类型是 Integer，但是您应用的转换返回一个 Long 类型，那么您期望您的测试用例能够捕捉到它。在这种情况下，您应该看到一个失败的管道，日志指向断言错误。

![](img/900fcffb6101670fd4c9812234d0b214.png)

图 Azure DevOps 管道中失败的单元测试示例(图片由作者提供)

## 结论

那都是乡亲们！以这种方式自动化单元测试是非常容易的，一旦你开始工作，前途无量。有时当我们被选择宠坏了，最好试一试，让你内心的现实主义者接管。这里是我的 git repo 的完整源代码的链接: [PySpark 单元测试 Repo](https://github.com/xavier211192/Xavier-Az-Learn-PySpark-UnitTests.git)

期待听到您对构建数据管道自动化测试的想法和经验。建议是最受欢迎的！

## 参考

1.  科里·斯查费的 Python 单元测试教程。
2.  [Eriks Dombrovskis 测试 PySpark 数据转换](/testing-pyspark-dataframe-transformations-d3d16c798a84)
3.  [Python 单元测试模块文档](https://docs.python.org/3/library/unittest.html)