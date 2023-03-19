# 数据工程师不应该写气流图——第 1 部分

> 原文：<https://towardsdatascience.com/data-engineers-shouldnt-write-airflow-dags-b885d57737ce?source=collection_archive---------4----------------------->

## 意见

## 是时候寻找一种更好的方法来使用 Apache 气流了

![](img/a2db4fe0456cd75a85a23d6197e7ed86.png)

里卡多·戈麦斯·安吉尔在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上的照片

我认为数据工程师不应该写气流图。相反，**数据工程师应该构建自动化那些 Dag 创建方式的框架**。

没有什么比一遍又一遍地做同样的事情更无聊的了。这正是你写 DAG 时要做的。你一遍又一遍地做着同样的事情。过了一段时间，工作变得重复，你的技能开始得不到充分利用。

Stitch Fix 的数据平台副总裁杰夫·马格努松几年前写了一篇[文章](https://multithreaded.stitchfix.com/blog/2016/03/16/engineers-shouldnt-write-etl/)，他在文章中指出 ***工程师不应该编写 ETL*** *。*他的观点是**数据工程师的工作不是编写 ETL，而是概括和抽象这些 ETL 的制作方式**。

我相信这正是我们在气流方面做错的地方。我们只是在编写 Dag，而不是构建概括和抽象 Dag 创建方式的框架。当你做了一段时间后，它会变得重复并且不好玩。

此外，重复代码不是编写可维护代码的聪明方法。[干](https://en.wikipedia.org/wiki/Don%27t_repeat_yourself) ( *不要重复自己*)原则存在是有原因的，*也就是*，写可维护的代码。

这篇文章旨在与你分享我写了两年气流 Dag 后学到的一些经验。我希望这能对社区有所帮助，并从中受益。我相信是时候找到一种更好的方法来使用 Apache Airflow 并改进我们进行数据工程的方式了。

> **TL；构建框架，不要写 Dag。你可以通过使用动态 Dag、OOP 和软件设计模式来做到这一点。**

# 数据工程师的职责

数据工程师的主要职责不是写 ETL。同样，对于 Apache Airflow，数据工程师的职责不是编写 Dag。为什么？很简单。

> “工程师擅长抽象、概括，并在需要的地方找到有效的解决方案。”杰夫·马格努松

我相信数据工程可以被看作是软件工程的一个专门化。软件工程的主要目的是编写**可靠的、可伸缩的、可维护的代码**。我认为这也是我们作为数据工程师应该努力的事情。

实现这一点的方法之一是对事物进行概括和抽象。那是我们的工作。通过概括和抽象来构建事物，这样我们就可以授权数据分析师和数据科学家来完成他们的工作。

> “那么，在这个新的、水平的世界里，工程师的角色是什么？总而言之，工程师必须部署平台、服务、抽象和框架，让数据科学家能够自主地构思、开发和部署他们的想法(比如用于构建、调度和执行 ETL 的工具、框架或服务)。”杰夫·马格努松

# 数据工程师不应该写 Dag

你需要学习的第一件事是**,确定什么时候写 DAG，什么时候不写。**

我不希望你去工作，开始疯狂地概括和抽象事物。过度工程化和过度一般化也是一个[问题](https://medium.com/@rdsubhas/10-modern-software-engineering-mistakes-bc67fbef4fc8)。你应该努力保持平衡。

这里有我写博客时学到的 4 课。我希望这给你带来一些关于如何停止写 Dag 并开始构建更多框架的指导！

## 1.DAG 写入有时会重复

我写的第一个 DAG 是一个 MySQL 数据库的 ELT 过程。任务很简单。考虑到一些限制条件，我必须提取数据。把它载入 S3 自动气象站。最后，使用基于 SQL 的转换来转换数据。就这样。所以，我只是用几个 [PythonOperators](https://airflow.apache.org/docs/apache-airflow/stable/howto/operator/python.html) 写了一个 DAG 文件。

集成之后，我被要求再集成两个数据库。一个是另一个 MySQL 数据库，另一个是 MongoDB 数据库。

很简单。像第一个任务一样的另一个任务。因此，**我从第一个 DAG 文件中复制了一些代码，并将这些代码复制到几个 DAG 文件中**。我对另外几个积分做了同样的事情。没什么大不了的。只是多了 5 到 6 个数据源。

几个月后，我们决定改变使用气流操作器的方式。我们从 PythonOperators 迁移到了 KubernetesPodOperators。你猜怎么着？我发现**我在所有 DAG 文件中都有几乎相同的代码。因此，我不得不在所有 DAG 文件中进行相同的更改**。

这时我意识到我做错了。所以，我们开始使用[动态 DAGs](https://www.astronomer.io/guides/dynamically-generating-dags) 。然后，我们使用面向对象编程(OOP)改进了这种方法。特别是，我们构建了一个定制的 DAG 工厂。工厂允许我们从一段独特的代码中创建多个 Dag。这让事情变得简单多了！

这是我在**学到的第一课**。**你** **可以使用动态 Dag 和面向对象的方法来构建你的 Dag。但是，要小心，动态 Dag 在大量使用时会产生问题。点击阅读更多[。](https://www.astronomer.io/guides/dynamically-generating-dags)**

查看这些文章了解更多信息。

<https://www.astronomer.io/guides/dynamically-generating-dags>  </how-to-build-a-dag-factory-on-airflow-9a19ab84084c>  

## 2.构建允许人们与您的 Dag 进行交互的界面

在我们开始动态生成 DAGs 之后，感觉棒极了。编写用于集成新数据源的新 DAG 可能需要 5 分钟。**除非你知道把代码放在哪里。**

这是我的第二堂课。你不是给自己造东西，而是给别人用。

我会让数据分析师写他们自己的 Dag，但是他们不能，因为太难了。那时，我们萌生了编写一个框架来加载基于 YAML 文件的数据源的想法。没什么特别的，但是很实用。

这里的底线是构建允许人们与你的 Dag 交互的界面。在我们的例子中是一个简单的 YAML 文件。但是，天空是极限。如果您需要的话，您甚至可以构建一个 web UI 来实现这一点。

</generalizing-data-load-processes-with-airflow-a4931788a61f>  

## 3.气流操作员工厂是必要的

我喜欢从 Airflow 1.10 迁移到 2.0。但是，你知道吗？我讨厌在使用 KubernetesPodOperator 的地方修改所有的代码行。

在气流 2.0 中，KubernetesPodOperator 发生了很大的变化。以下是影响我们的一些事情:

*   **端口已从列表[Port]迁移到列表[V1ContainerPort]**
*   **env_vars 已从字典迁移到列表【v1 env var】**
*   **资源已经从 Dict 迁移到 V1ResourceRequirements**
*   **资源已经从 Dict 迁移到 V1ResourceRequirements**
*   …点击此处查看变更的完整列表

那是我的**第三课**。你不仅需要一个 DAG 工厂，还需要一个 Operator 工厂。

迁移之后，我们构建了自己的操作符工厂。如果 KubernetesPodOperator 再次发生变化，我们现在已经做好了准备。下一次改变将在一个文件中引入**，而不是在 10 个不同的文件中。**

## 4.动态 Dag 和 OOP 是不够的

现在你知道我们喜欢动态 DAGs 和 OOP，让我告诉你另一件事。一切都很好，直到我们开始发展和采用新的用例。我们意识到**动态 Dag 和 OOP 不足以解决我们的问题**。

我们意识到**我们构建了紧密耦合的模块。我们做得太差了，以至于我们的 DAG 工厂只有在包含两种特定类型的任务时才会生成 DAG。这两种类型是提取和转换。我们有一个用例，我们想要生成一个只包含提取任务的 **DAG。**我们不能这么做，因为我们的 DAG 工厂不允许。我们期望**包括提取和转换任务。****

这是我的第四课。你必须小心你如何概括和抽象事物。你不能只是在生活中到处扔随机的概括和抽象。

**我们解决代码紧密性的方法是使用** [**软件设计模式**](https://en.wikipedia.org/wiki/Software_design_pattern) **。**DAG 工厂和 Operator 工厂遵循[工厂方法模式](https://en.wikipedia.org/wiki/Factory_method_pattern)。这是最广为人知的设计模式之一，但是你应该知道还有更多的！我们还发现遵循[策略模式](https://refactoring.guru/design-patterns/strategy)和[模板模式](https://refactoring.guru/design-patterns/template-method)非常有用。

# 结论

我知道我在这篇文章中所做的一些陈述可能会很强烈并引起争议。请让我知道你对它的想法！

小心我在文章中建议的原则。不要把事情过度概括，过度工程化！

> *“复制远比错误的抽象便宜”桑迪·梅斯*

感谢阅读！

最后，感谢[胡安·费利佩·戈麦斯](https://medium.jfgomez.me/)让这一切成为可能。

> **更新:**本文的第二部分现已推出

</data-engineers-shouldnt-write-airflow-dags-part-2-8dee642493fb>  

*如果你想随时更新我的作品，* ***请加入我的*** [***简讯***](https://metadatacommunity.substack.com/) ***！*** *偶尔，我会和我的读者分享一些东西。如果你加入我会很感激的:)*