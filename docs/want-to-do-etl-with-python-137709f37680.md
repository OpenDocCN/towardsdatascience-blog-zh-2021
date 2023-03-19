# 想用 Python 做 ETL？

> 原文：<https://towardsdatascience.com/want-to-do-etl-with-python-137709f37680?source=collection_archive---------5----------------------->

## 找到最适合您的用例的 Python ETL 工具

![](img/a2787995117b560633aab183c547b885.png)

在 [Unsplash](https://unsplash.com/?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上拍摄的 [ThisisEngineering RAEng](https://unsplash.com/@thisisengineering?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

> 提取、转换和加载

现代组织依赖于使用一流的工具和技术收集的巨大数据池来提取数据驱动的见解，这有助于做出更明智的决策。由于现在的行业标准技术进步带来的改进，组织现在可以更容易地访问这些数据池。

但是在这些公司实际使用这些数据之前，它需要通过一个叫做 **ETL 的过程，ETL 是提取、转换和加载的缩写。**

ETL 不仅负责向这些组织提供数据，还负责确保数据具有正确的结构，能够被他们的业务应用程序高效地使用。今天的企业在选择正确的 ETL 工具时有很多选择，比如用 Python、Java、Ruby、GO 等构建的工具，但是对于这篇文章，我们将更多地关注基于 Python 的 ETL 工具。

# 什么是 ETL？

作为数据仓库的核心组件，ETL 管道是称为提取、转换和加载的三个相关步骤的组合。组织使用 ETL 过程来统一从多个来源收集的数据，以便为他们的企业应用程序(如商业智能工具)构建数据仓库、数据中心或数据湖。

您可以将整个 ETL 过程视为一个集成过程，它帮助企业建立数据管道并开始将数据接收到最终系统中。下面是对 ETL 的简要说明。

**●提取:**包括从 CSV、XML 和 JSON 等多种格式中选择正确的数据源，提取数据，以及测量其准确性。

**● Transformation:** 当数据在临时或暂存区中等待最后一步时，所有的转换功能(包括数据清理)都应用于该数据。

**●加载:**涉及将转换后的数据实际加载到数据存储或数据仓库中。

# 2021 年的 Python ETL 工具

Python 凭借其简单性和高效性风靡全球。它现在正被用于开发一系列领域的大量应用程序。更有趣的是，热情的 Python 开发者社区正在积极地生产新的库和工具，使 Python 成为最激动人心和最通用的编程语言之一。

因为它现在已经成为数据分析和数据科学项目的首选编程语言，Python 构建的 ETL 工具现在非常流行。**为什么？**

这是因为他们利用 Python 的优势提供了一个 ETL 工具，不仅能满足您最简单的需求，还能满足您最复杂的需求。

以下是目前在 ETL 行业引起轰动的 10 大 Python ETL 工具。

## 1. **Petl**

是 Python ETL 的缩写， [petl](https://petl.readthedocs.io/en/stable/index.html?utm_source=xp&utm_medium=blog&utm_campaign=content) 是一个完全用 Python 构建的工具，设计得非常简单。它提供了 ETL 工具的所有标准特性，比如从数据库、文件和其他来源读取和写入数据，以及大量的数据转换功能。

petl 也足够强大，可以从多个数据源提取数据，并支持大量文件格式，如 CSV、XML、JSON、XLS、HTML 等等。

它还提供了一组方便的实用函数，可以让您可视化表、查找数据结构、计算行数、值的出现次数等等。作为一个快速简单的 etl 工具，petl 非常适合创建小型 ETL 管道。

虽然 petl 是一个一体化的 etl 工具，但是有些功能只能通过安装第三方包来实现。

在这里了解更多[。](https://petl.readthedocs.io/en/stable/)

## 2.**熊猫**

Pandas 已经成为一个非常受欢迎的用于数据分析和操作的 Python 库，这使得它成为数据科学社区中最受欢迎的库。这是一个非常容易使用和直观的工具，充满了方便的功能。为了在内存中保存数据，pandas 将 R 编程语言中的高效 dataframe 对象引入 Python。

对于您的 ETL 需求，它支持几种常用的数据文件格式，如 JSON、XML、HTML、MS Excel、HDF5、SQL 和许多其他文件格式。

Pandas 提供了标准 ETL 工具所能提供的一切，使其成为快速提取、清理、转换和向终端系统写入数据的完美工具。Pandas 还可以很好地使用其他工具，如可视化工具等，以使事情变得更容易。

在使用熊猫时，你应该记住的一件事是，它将所有东西都放入内存中，如果内存不足，可能会出现问题。

在这里了解更多[。](https://pandas.pydata.org/)

## 3.**路易吉**

Spotify 的 Luigi 是一个**工作流管理系统**，可以用来创建和管理大量的批处理作业管道。Luigi 允许用户将数以千计的任务连锁化和自动化，同时通过网络仪表板方便地提供所有管道的实时更新。它提供了大量的模板，让您可以快速创建数百个任务。

Luigi 还提供了一个依赖图，为您提供了管道中各种任务及其依赖关系的可视化表示。

Luigi 最大的优点之一是它确保所有文件系统操作都是原子的。由于 Luigi 的良好记录，它提供了列表中其他 ETL 工具中最好和最强大的 ETL 管道创建功能。这就是像 Spotify、Foursquare、Stripe、Buffer 等企业继续依赖 Luigi 的原因。

在这里了解更多[。](https://github.com/spotify/luigi)

## 4.**阿帕奇气流**

Apache Airflow 是一个非常易用的**工作流管理系统**，它允许用户无缝地创建、调度和监控所有工作流管道，甚至是 ETL 管道。它最初是由 Airbnb 开发的，但后来被添加到 Apache 软件基金会的剧目中。

为了让您了解作业管道的最新进展，Airflow 附带了一个名为 Airflow WebUI 的直观用户界面。

气流可以在不同级别的工作负载之间平稳地上下调节。这里要注意的主要事情是，Airflow 本身并不做 ETL，相反，它让您能够在一个地方监督管道处理。

您可以使用自己选择的其他库和操作器来扩展气流。Airflow 还可以与微软、谷歌和亚马逊等一系列云服务提供商很好地集成，并提供简单的即插即用运营商。

在这里了解更多[。](https://airflow.apache.org/)

</5-essential-tips-when-using-apache-airflow-to-build-an-etl-pipeline-for-a-database-hosted-on-3d8fd0430acc>  

## 5.美味的汤

Beautiful Soup 是目前最受欢迎的基于 Python 的 web 抓取工具之一，当谈到 ETL 时，Beautiful Soup 可以帮助你从任何你想要的网站上提取数据。

如果您不知道 web scraper 是什么，它是一个位于 HTML 或 XML 解析器之上的小程序，在我们的例子中，它提供 Pythonic 式的习惯用法，用于从解析树中找到正确的数据。Beautiful Soup 不仅允许您抓取数据并按原样存储，还可以用来为您的数据提供一个定义好的结构。

使用它，您可以导航、搜索和修改文档的解析树，并提取您需要的任何内容。默认情况下，它还可以处理所有的文档编码和解码，这样就少了一件需要担心的事情。Beautiful Soup 完全由 Python 构建，甚至可以与各种产品无缝集成。

在这里了解更多[。](https://www.crummy.com/software/BeautifulSoup/)

## 6.**倭**

Bonobo 是一个**完全包含的轻量级 ETL 框架**，它可以完成从提取数据、转换数据到加载到终端系统的所有工作。

为了创建 ETL 管道，Bonobo 使用了图表，这使得构建和可视化所涉及节点的一切变得更加容易。

Bonobo 还支持管道图中元素的并行处理，并确保转换过程中的完全原子性。如果你想从 Bonobo 中获得更多，你可以通过它的扩展来实现。

它的一些流行的扩展是 SQLAlchemy、Selenium、Jupyter、Django 和 Docker。真正让 Bonobo 轻量级和快速的是它针对小规模数据的能力，这使它成为简单用例的一个好选择。

如果你知道如何使用 Python，你会毫无疑问地找到 Bonobo。

在这里了解更多[。](https://www.bonobo-project.org/)

## 7. **Odo**

Odo 是 [**Blaze 生态系统**](http://blaze.pydata.org/) 的五个库之一，旨在帮助用户存储、描述、查询和处理手头的数据。

这里列出 Odo 是因为它擅长将数据从一个容器迁移到另一个容器。作为一个轻量级的数据迁移工具，Odo 可以在小型的内存容器和大型的核外容器上创造奇迹。

Odo 使用小型数据转换函数网络将数据从一种格式转换为另一种格式。支持的数据格式列表包括内存中的结构，如 NumPy 的 N 维数组、Pandas 的 DataFrame 对象、列表，以及传统的数据源，如 JSON、CSV、SQL、AWS 等等。

由于所支持的数据库具有极快的原生 CSV 加载能力，Odo 声称它可以击败任何其他纯粹基于 Python 的加载大型数据集的方法。

在这里了解更多。

## 8. **Pygrametl**

**网站/GitHub 回购:**[https://chrthomsen.github.io/pygrametl/](https://chrthomsen.github.io/pygrametl/)

这个开源框架非常类似于 Bonobo，并允许 ETL 管道的顺利开发。当实际使用 pygrametl 工具时，您必须对 Python 有所了解，因为该工具要求开发人员在其中编码整个 etl 管道，而不是使用图形界面。

该工具提供了常用操作的抽象，例如与来自多个来源的数据交互、提供并行数据处理能力、维护缓慢变化的维度、创建雪花模式等等。

使用这种方法的一个主要好处是，它允许 pygrametl 与其他 Python 代码无缝集成。这在简化这种 ETL 过程的开发中起着关键作用，甚至在需要时便于创建更复杂的操作或管道。

在这里了解更多[。](https://chrthomsen.github.io/pygrametl/)

## 9.**马拉**

如果您不喜欢自己编写所有代码，并且认为 Apache Airflow 对您的需求来说太复杂，那么您可能会发现 Mara 是您 ETL 需求的最佳选择。您可以将 Mara 视为编写纯基于 Python 的脚本和 Apache Airflow 之间的轻量级中间地带。**为什么？**

这是因为 Mara 基于一套预定义的原则，可以帮助您创建 ETL 管道。这些假设解释如下:

**●** 使用 Python 代码创建数据集成管道。

**●** PostgreSQL 将作为数据处理引擎。

**●** 一个 web UI 将用于检查、运行和调试 ETL 管道。

**●** 节点依赖上游节点完成，无数据依赖或数据流。

**●** 命令行工具将被用作与数据和数据库交互的主要来源。

**●** 将使用基于 Python 多处理能力的单机流水线执行。

**●** 成本较高的节点先运行。

这些假设负责降低管道的复杂性，但由于一些技术问题，Mara 仅在 Linux 和 Docker 上可用。Mara 还提供了一系列工具和实用程序，用于在其 GitHub repo 上创建数据集成管道。

在这里了解更多。

## 10.**气泡**

Bubbles 是另一个基于 Python 的框架，可以用来做 ETL。但是 Bubbles 不仅仅是一个 ETL 框架，它还有更多。Bubbles 为用户提供了一组工具，可以对数据进行多种操作，比如监控、审计、清理和集成。

大多数 ETL 工具使用脚本或图形来描述它们的 ETL 管道，而不是气泡。在其核心，Bubbles 使用元数据来描述其管道，使管道设计者的任务变得容易得多。

使用 Bubbles 的最好理由之一是它是[技术](https://blog.digitalogy.co/top-technology-trends-that-will-rule-in-2021/)不可知的，这意味着您不必担心如何使用数据存储，您可以简单地专注于以您选择的格式获取数据。由于其技术无关性，Bubbles 提供了一个抽象的 ETL 框架，可以在各种系统上快速使用，以执行所有必要的 ETL 操作。

在这里了解更多[。](http://bubbles.databrewery.org/)

# 结束…

如今，现代企业更加依赖数据来做出明智的决策，ETL 工具在这一过程中发挥着至关重要的作用。它们不仅能帮你节省时间，而且非常经济实惠。鉴于 ETL 工具日益增长的重要性，它已经成为当今企业的必需品。

现在市场上有很多 Python ETL 工具，它们是用一系列编程语言构建的，可以满足您所有的 ETL 需求。请记住，并不是所有的 ETL 工具都是一样的，虽然其中一些工具可能提供了丰富的特性集，但其中一些工具可能非常简单。

# **认识你的作者**

克莱尔 D 。是 **Digitalogy** 的内容制作师和战略家，他可以将你的内容想法转化为清晰、引人注目、简洁的文字，与读者建立强有力的联系。

在 [**上跟我连线**](https://medium.com/@harish_6956)**[**Linkedin**](https://www.linkedin.com/in/claire-d-costa-a0379419b/)**&**[**Twitter**](https://twitter.com/ClaireDCosta2)**。****