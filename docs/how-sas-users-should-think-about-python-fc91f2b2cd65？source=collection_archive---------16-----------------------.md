# SAS 用户应该如何看待 Python

> 原文：<https://towardsdatascience.com/how-sas-users-should-think-about-python-fc91f2b2cd65?source=collection_archive---------16----------------------->

## 让你的思想正确，让你的代码正确

有相当多的资源可以帮助 SAS 用户开始编写一些 Python 代码。但是对于我们这些把 SAS 作为第一语言的人来说，切换到 Python 不仅仅是翻译代码行。这是世界观的改变。以下是如何看待世界的一个 Pythonista。采用这种观点不仅有助于你复制你的 SAS 工作。这将有助于拓展你的视野，让你了解编程语言能为你做些什么。

![](img/13123b74c51138b9bebb7e30a8533b88.png)

尼克·里克特在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上的照片

首先，这不是一篇 SAS vs Python 的文章。我非常尊重这两种生态系统。它们可以单独使用，也可以无限组合在一起使用。我想我们都是成年人了，您已经做了尽职调查，并提出希望为您的业务添加一些 Python 功能。这篇文章会给你一个心理工具包，帮助你从这个决定中获得最大收益。如果你和我一样，你出发的时候至少会有这一套问题。让我们浏览一遍，让您了解 Python 世界。

## 为什么每次打喷嚏都要装新库？

![](img/77bc07a51cee104a176aab021ed6b961.png)

由[劳伦·麦科纳奇](https://unsplash.com/@coldwisper?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 拍摄的照片

Python 的强大功能..最大的痛苦是它的图书馆系统。除非你想从头开始复制经典的计算机科学算法，比如冒泡排序，否则你可能需要为你写的每个程序导入至少两个库。这曾经是非常痛苦的。有些库安装起来会有问题，并且经常会破坏一些相关的库！对你来说幸运的是，Python 中的 conda 和 pip 社区现在已经使这变得非常干净了。除了 TensorFlow 的 GPU 安装，这里的事情会进行得相当顺利。

仍然存在发生灾难性错误的小风险。为了管理这一点，创建一个虚拟环境来隔离您的项目。第一天不用担心。但是当你开始认真对待 Python 时，总是为一个项目使用一个新的 *venv* ,这样你就有了一套不会与其他项目冲突的新的库。您可以将这个库列表保存到一个需求文件中，然后在一行代码中构建一个新的 *venv* 。【https://docs.python.org/3/library/venv.html 

为什么他们不把 pandas 和 numpy 这样的流行库添加到核心 python 中，然后就完事了呢？嗯，这种分散式开发意味着你很早就能获得最先进的强大功能，比如 Python 中的强化学习。尽管您可能仍然希望为某些生产需求购买商业软件，但您至少可以用 Python 来组装 NLP、ML、graph、深度学习、模拟或计算机视觉项目的所有启动工具，每种工具都有两行代码:一个用于 pip install 的 shell 命令；和一个导入语句。在许多情况下，可用的功能会将您一直带到企业部署。

图书馆系统是非凡的，它将为你在数据分析和数据科学方面的创新提供动力，不需要任何成本，只需要你的汗水。

## Python 中数据科学最好的入门库和资源有哪些？

我不会在这里赘述一般情况。有很多关于使用 Python 进行数据科学研究的资源。它已经成为该领域事实上的语言。但是这里有一些注意事项，特别是对你的 SAS 用户。

![](img/0a625c4a3b4bd882effb74b16f0a9934.png)

照片由[杰伊·温宁顿](https://unsplash.com/@jaywennington?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

熊猫数据框架是数据集的等价物。你可以操纵它们，例如旋转，衍生新的领域，总结，连接和计算到你的心的内容都在熊猫。Proc merge 在 df.merge 中有对应的功能，排序和重命名都是非常标准的。我发现的唯一问题是，熊猫虽然很强大，但相当啰嗦。重命名一个列是一个通过花括号和方括号的旅程！继续阅读，了解在 Python 中哪里也可以切换到普通的旧 SQL 进行标准查询和转换。

如果你是一个 SAS IML 用户，并且对矩阵做了大量的工作，请直接进入 Numpy。您将在这里获得所有的线性代数功能，并拥有强大的省时功能，如广播。如果你想和很酷的 Python 数据科学家一起玩，就把你的矩阵叫做“张量”吧！

对于机器学习来说，Python 中没有相当于 SAS Enterprise Miner 的开源。这实际上是关于 scikit-learn、TensorFlow、PyTorch 或 Keras 中的编码。好的一面是这些都是很棒的图书馆。如果你确实想走 GUI 的道路，你可以在 AutoML、RapidMiner、DataRobot 或更多上面花点钱。或者继续使用 Enterprise Miner 作为您的 GUI ML。

## 数据保存在哪里？

![](img/3638475d8b25769a5ded4d706977e68d.png)

在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上由[Duan veverkolog](https://unsplash.com/@veverkolog?utm_source=medium&utm_medium=referral)拍摄的照片

*(好吧，我在想哪种动物如此执着……乌龟。不知道我能让动物主题持续多久，但是让我们看看！)*

在 sas 中，我们传统上处理位于文件系统中的. sas7bdat 文件——每个表(或者使用 SAS 术语的数据集)一个文件。如果您通过 ODBC 或本地连接连接到 Teradata 或 SQL Server 之类的数据库，您也可以访问这些关系数据库模式。如果您是 SAS BI 用户，您还可以访问 OLAP 数据存储。

Python 呢？嗯，Python 是一种语言，而 SAS 是一个垂直集成的系统，所以最终 Python 并不关心那么多。它将支持任何东西。但实际上，数据分析师和数据科学家通常将数据存储为文件，如。磁盘上的 csv，至少在 R&D 模式下是这样。csv 的好处在于它相当通用。它不保存数据类型或其他元数据。您可以根据需要在导入时指定字段的数据类型。因此，您可以毫无问题地在软件之间传递 csv。另一面是..您必须在导入时定义您的数据类型。这可能是一大堆管道代码。祝您在寻找格式错误时好运，尤其是日期。(日期在任何语言中都是一场噩梦！)

因此，如果数据类型对您的程序真的很重要，您可以考虑 parquet，这是一个二进制文件，它很容易导入和导出，但它为您提供了标准的数据类型。

现在，如果您已经创建了一个非常酷的 Python 对象，比如带有多索引或多维数组甚至 ML 模型的 pandas dataframe，会怎么样呢？那么在这里，joblib 图书馆来拯救。你可以简单地用一行程序把你的对象保存到磁盘，然后在你喜欢的地方重新导入它。

如果你真的想连接到一个数据库，有很好的支持。例如，SQLAlchemy 允许您用几行代码和一个库导入连接到大多数数据库格式。一旦连接上，您就可以像 pandas 表一样处理数据，将结果保存回数据库，或者在代码中直接针对数据库本身编写 SQL。

## 如何在内存中保存数据？

![](img/f3020f435f62c1b90d955e7935f05b41.png)

Geran de Klerk 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

Python 支持你可能会想到的所有基本数据结构——变量、列表和集合。可能最方便的额外工具是字典，一个简单的键-值对映射，经常会派上用场。我不确定我是否写过不使用字典的 Python 程序。由于您的值可以采用任何数据类型，一个很好的用例是拥有一个名为 pandas 数据帧的字典。这允许您遍历这组表，将数据转换一次自动应用到整个集合。在 SAS 中，你可以通过一个宏循环来完成。相信我，Python 的方式要简单得多，也更易读。

在做数据科学时，我们通常将更复杂的数据保存在更高层次的数据结构中，如 pandas dataframes 和 numpy 数组。

一旦您的需求变得更加定制化，就像在任何复杂的程序中一样，您就可以自由地创建自己的类来存放数据。Python 有一个名为 dataclass 的特殊类，允许您简洁地创建这些。然后，具有可组合性和继承性的 OO 的力量允许您创建一个复杂的数据模型，可以在内存中轻松地对其进行编程。

如果您经常处理数据库，您可能希望使用对象关系模型工具包(如 SQLAlchemy)来创建内存中的关系模式。这将为用查询、连接和更新遍历数据模型提供许多方便的功能。

回到主题，Python 在一个程序中组合如此多不同数据结构的能力也可能成为令人头疼的问题。与动态类型相结合，它会产生各种副作用和代码模糊性。好的模块化或面向对象设计，注意命名约定和类型提示可以解决大部分问题。

## 我能找到相当的企业指南吗？

![](img/6b5487fdb517adcf3e0570b61ad47f67.png)

照片由[埃利奥特·范·布根豪特](https://unsplash.com/@eli29?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 拍摄

我可以在这里拐弯抹角，想出一些选择，但是……答案只是‘不’。你不会找到的。开源世界中更少的指南。这是更勇敢的数据科学家的地方！

开源世界是非常以程序员为中心的。你不会找到一个免费的基于 Python 的工具，允许你操作和连接数据，运行模型，获取统计数据，将模块构建成流等等。一般来说，你可以在一个强大的用户界面中做任何你想做的事情。与 Enterprise Miner 一样，这是 SAS 真正的亮点。如果不想编码，就坚持使用 EGuide。或者查看一些商业替代品，如用于数据工作的 SnowFlake 和上面列出的用于建模的 ML GUI 提供者。

## 我需要开始写面向对象的代码吗？

不，你不知道。但是你应该掌握干净的模块化代码。虽然 SAS 正在向函数方向发展，但大多数代码库都是围绕着 datastep 编写的，其中宏用于可重用的部分。Python 以函数、模块和包的形式组织，更易于维护和阅读。如果你学会了这些基础知识，你将会开始编写更简洁、更易维护的代码。

对于某些用例，面向对象确实胜过模块化。当你的程序增长超过一定的复杂度时，你就开始陷入混乱，函数调用的函数很难跟踪。其中一个迹象是，当你需要通过 10 个函数传递一堆数据时，才使用它。

从面向对象开始。无论您的模块化方法看起来有什么缺陷，都可以考虑为一组数据创建一个类，该类需要一组公共函数。看一看基本的 UML 和 OO 模式，你将很快开始构建值得软件工程师使用的程序。

## 为什么创建可视化需要这么多代码？

![](img/66ce9107a2741c31fa120cac9facd379.png)

在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上由 [Carlos Muza](https://unsplash.com/@kmuza?utm_source=medium&utm_medium=referral) 拍摄的照片

一旦您开始使用 matplotlib 和 seaborn，您将会看到 Python 在数据分析方面的真正威力。但是随着强大的力量而来的是巨大的复杂性。每次创建哪怕是最简单的图表，我还是得去谷歌一下。

如果你是一个图形迷，就一头扎进去吧。这里对你来说太多了。Seaborn 补充 matplotlib。散景、dash、streamlit 等将带您体验互动。如果你像我一样是个图形白痴，那就坚持用熊猫吧。他们已经将所有的基础知识嫁接到了这个库中，所以大胆猜测一下，在 dataframe 中添加类似. plot.bar()的东西可能会给你一些合理的东西。

对于 dashboarding，可以考虑像 Superset 这样的开源工具集，以节省许多代码行，并获得一些漂亮的交互式功能。如果你想对这个过程有更多的控制，Bokeh 和 Streamlit 这两个工具可以让你创建交互式的 web 应用程序，而不用太深入 web 应用程序设计。但是最终，就像 EGuide 和 EMiner 一样，开源世界并没有像商业机构那样做这种用户友好的软件。通过坚持使用 SAS BI 或检查 Tableau/PowerBI/Qlik(至少对于企业级仪表板),您可能会省去很多麻烦。

## 我如何分享我的工作？

![](img/d64e5c7bd9d8f25187d5d6143874d746.png)

在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上由[米米·蒂安](https://unsplash.com/@mimithian?utm_source=medium&utm_medium=referral)拍摄的照片

SAS 和其他垂直集成的商业数据系统的一个很大的优势是，它们支持围绕工件共享的客户机-服务器类型的安排。Python 中有几个选项。首先，对于开发者之间的协作，托管的 Jupyter 服务，如 https://tljh.jupyter.org/en/latest/[的最小的 Jupyter hub](https://tljh.jupyter.org/en/latest/)是共享代码和数据的好方法。

为了与业务用户共享结果，上面提到的仪表板解决方案是受欢迎的选择。但是对于一个更具凝聚力的企业数据分析系统，您将希望回到商业世界，使用 SAS 或替代产品(如雪花)来支持常见的数据消费和生产工作流。

## 我需要担心 Git 和版本控制吗？

![](img/b7cf4de82ca371968a4127f69a514a72.png)

由 [Roman Synkevych](https://unsplash.com/@synkevych?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

你不需要担心它，但你应该使用它。并不是说 SAS 用户从来不用 Git。但是他们不愿意。我们在版本控制和没有版本控制的发布上陷入了可怕的混乱。Git 正在彻底改变软件构建的方式，使得程序维护和更新更加干净和没有错误。它还为 CICD(持续集成持续部署)铺平了道路，允许大型团队可靠而快速地推送代码。

Git 的问题在于它并不容易。像 Python 生态系统中的其他东西一样，它是为最大功能而设计的。你可以用它做任何事情，但有时似乎你不能用它做任何事情。作为一个新手，你将不可避免地陷入合并、存储和重置的可怕困境。Windows 用户可以选择一个漂亮的图形用户界面来简化事情。

但是命令行值得学习。把你的痛苦看作是一次性的。一旦你接受了它，你就离不开它了。不再有周一早上的会议问‘我的代码去哪了？“我可以看到科林清理数据的新程序，但我们似乎没有生成任何……数据！”

## 应该用什么 IDE 写代码？

![](img/0a85fa1a23acf03adbb78b127515ea7e.png)

阿姆扎·安德烈在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

如果你是从 SAS 世界出来的，我认为你更可能是一名数据科学家，而不是一名经过培训的计算机科学家。CS 的小伙子和姑娘们总是回避这个问题，并声称使用 Vi 或 vim 来编写代码，就像我们的祖先那样。我们这些重视理智的人将使用现代编辑器来帮助捕捉错误，提供自动完成功能，并访问许多其他便利功能，如数据库查询、降价呈现等等。

现在所有酷小孩都花大量时间在 Jupyter 笔记本上写 Python。当你在一个“单元”中运行一个代码片段时，这是一个非常方便的方法来编写带有快速反馈的代码片段序列。安装相当容易，并且您可以在浏览器中工作。

然而，一旦一个程序达到了几百行代码，你认为你可能需要重用它(提示:你会重用它！)，我发现更传统的 ide 是必不可少的。

使用 VSCode、PyCharm、Spyder 和其他工具，您可以更容易地创建包含包和模块的应用程序结构，并了解程序实际上是如何调用的，而不仅仅是按照编写的顺序。关于这一点有无止境的争论，但是请将 Jupyter 和成熟的 ide 视为互补，并根据您的需求进行选择。

我是一个 VSCode 用户，对它说不出足够多的好话。Python 调试器让我摆脱了很多麻烦，pytest、flask 和其他集成是创建我的生产应用程序的关键。我听说皮查姆一样好，如果不是更好的话。

## 我能找到类似 Proc SQL 的东西吗？

![](img/f1dd99d7719aa5846bb6dd464110b54a.png)

照片由 [Sunder Muthukumaran](https://unsplash.com/@sunder_2k25?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 拍摄

是的，你可以！它叫做 pandasql，工作起来就像做梦一样。您可以将 sql 写成一个字符串，并针对任何 Pandas 数据帧或数据帧集运行它。我已经使用它毫无问题地构建了完整的 ETL 管道。对于使用规范化的数据，它提供了一个更干净、更易读的代码库，您会喜欢回头再看。

但是对于涉及复杂的可重用函数、多维索引或时间序列的重型 ETL，Python 和 Pandas 会给你更大的马力。(流行的数据库系统通常求助于围绕 SQL 的过程化语言，比如 PSQL 或 PL/SQL。)在企业级，您可以开始使用一些很棒的框架，比如 Prefect for orchestration 和 Great Expectations for schema validation。

## 你现在是皮托尼斯塔了

![](img/617bf4d2ca1ab2c2d80deb5b03f5ccea.png)

Patrick Beznoska 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上的照片

嗯，也许先写点 Python 代码吧！但是现在您已经将您的 SAS 概念和需求翻译成了 Pythonese，我希望您能够看到您已经采用的这种奇怪的新语言的一些可能性。享受旅程！