# 无代码、低代码和亲代码

> 原文：<https://towardsdatascience.com/no-code-low-code-and-pro-code-a71b25935d2b?source=collection_archive---------27----------------------->

![](img/61a3a15b93fd1ee6b953f34c3b602b87.png)

Djehouty， [CC BY-SA 4.0](https://creativecommons.org/licenses/by-sa/4.0) ，via [Wikimedia Commons](https://commons.wikimedia.org/wiki/File:%C3%84gyptisches_Museum_Kairo_2016-03-29_Tutanchamun_Grabschatz_09.jpg)

## *当我们使用这些时髦术语时，我们的意思是什么？它们为什么重要？这些不就是已经存在的事物的新名称吗？*

# 让我们从为什么这些术语很重要开始

我想你会同意:

*   开发人员应该能够用最少的干净、可维护的代码构建新颖、有用、可伸缩、安全的软件。
*   最小化代码的复杂性和范围能够更快地部署更易于维护和改进的软件应用程序。
*   最小化代码的复杂性和范围使得更大、更多样化、更领域化的开发人员池能够设计、实现、维护和改进软件。
*   所有这些导致了更多的软件，更有用的软件， [*更快地吞噬世界*](https://a16z.com/2011/08/20/why-software-is-eating-the-world/) 。

![](img/4e3268ab98f0b0f0ae0220deeac2089b.png)

最初的食世界者，约尔蒙冈德[，维基共享](https://commons.wikimedia.org/wiki/File:Jormungandr.jpg)

# 我们如何召唤这条吞噬世界的软件大蛇？

通过最小化和优化代码的使用！

*   **用专门的语言，库**[**SDK**](https://en.wikipedia.org/wiki/Software_development_kit)**。**例如，在 [R 语言](https://en.wikipedia.org/wiki/R_(programming_language))中，开发人员很少需要编写代码来实现统计计算和算法——有无数的统计库可以完成这项工作。类似地， [Python](https://en.wikipedia.org/wiki/Python_(programming_language)) 拥有 [SciPy](https://en.wikipedia.org/wiki/SciPy) 和 [NumPy](https://en.wikipedia.org/wiki/NumPy) 库来完成大部分繁重的工作。*编码级别:* ***高码*** *，只是少了它的*
*   **用软件开发框架。**例如， [Angular framework](https://en.wikipedia.org/wiki/Angular_(web_framework)) 自动组织和编排 web 软件的许多方面，因此开发人员可以专注于特定于其应用的 HTML、CSS 和 JavaScript/TypeScript 代码。 [Node.js](https://en.wikipedia.org/wiki/Node.js) 是服务器端有用的软件开发框架的另一个例子。*编码级别:* ***高码*** *，只是少了一点*
*   **带有可定制的软件组件。**例子很多，包括 [Apache Spark](https://en.wikipedia.org/wiki/Apache_Spark) 、 [Docker](https://en.wikipedia.org/wiki/Docker_(software)) 、[关系数据库](https://en.wikipedia.org/wiki/Relational_database)、[云服务](https://aws.amazon.com/route53/)。与编程语言相比，像这样的软件组件在范围上受到了极大的限制——但这是故意的——限制范围使开发人员能够轻松配置这些强大的软件组件，并通过清晰、最少的编码将它们集成到他们的技术堆栈中。*编码级别:* ***低码*** *(配置、模板)* ***亲码*** *(插件、脚本)*
*   **用经过训练的人工智能软件组件。**这是将经过训练的人工智能模型作为模块化预测器/生成器插入大型软件应用程序的地方。例如，定向广告系统包括一个人工智能组件，它获取用户的数据，并通过一个训练有素的算法来运行它，以选择最佳的广告显示给用户。值得注意的是，人工智能软件组件需要的编码比人们(即高管)预期的多得多。*编码级别:* ***高码*** *(数据准备、设计、优化)* ***无码*** *(训练、测试、运行时)*
*   **采用可定制的全栈软件。**例如， [WordPress](https://wordpress.com/) 让开发者可以用最少的设置和编码来创建和部署网站。类似地， [R/Shiny](https://shiny.rstudio.com/) 使开发人员能够创建和部署用于数据分析的 web 应用程序，而无需编写任何 web 接口代码。*编码级别:* ***低编码*** *(配置、模板)* ***高编码*** *(插件、脚本)*
*   **带有可定制的最终用户软件。**在这一点上，我们完全摆脱了编码领域，允许用户通过人机界面配置软件的专门行为——例如，通过点击、滑动和拖动、搜索查询、数据输入表单和语音命令。有数百万个这样的例子，其中最流行的可能是 [Excel](https://en.wikipedia.org/wiki/Microsoft_Excel) 、 [Gmail](https://mail.google.com/) 、[脸书](https://www.facebook.com/)、 [Alexa](https://en.wikipedia.org/wiki/Amazon_Alexa) (我不确定为什么我需要那里的链接——我猜是为了那些刚刚从昏迷中醒来的读者)。
    - *编码级别:* ***无编码*** *(结构化表单，非结构化查询)*
*   **用 AI 内置的终端用户软件？**哈哈。嗯。先给我 AI-build 单元测试，*s ' il vous plat .编码级别:*[***vaporware***](https://en.wikipedia.org/wiki/Vaporware)

# 最近才听说无码，低码，亲码，不都是新的吗？

不，不是真的。
但如今有许多更有用的 SDK、框架和公司——*咳*、 [Tag.bio](https://tag.bio) 、*咳*——它们最大限度地减少了编码，以促进有用的软件开发和行为。这也是目前风险投资领域的一种投资趋势。

**无编码**表示配置软件时没有编码。有些人可能会争辩说，无代码应该被限制在用户明确打算通过他们的无代码行为来制作/发布新颖的软件应用程序的情况下，但这只是一个观点和 [UX](https://en.wikipedia.org/wiki/User_experience) 的问题。当用户执行感兴趣的任务时，比如在 Twitter 上发帖，最好不要编写代码。为其他人发布一个新的软件应用程序的意图——例如[一个可以在特定的 url 上看到的互动网页](https://twitter.com/aparnapkin/status/726205822238339072)——是一个次要目标。

*例如，Tag.bio 允许医生和生物学家通过迭代* ***无代码*** *参数配置(点击)对数据进行* [*高级分析*](https://tag.bio/project/iterative-analysis-allows-faster-discoveries/) *。*

低代码意味着编写高级配置和模板(静态代码),为软件组件或系统定制广泛的行为。JSON 和 YAML 似乎是目前最流行的语法选项。低代码并不总是简单的——配置和模板通常为每个软件组件定制属性、操作和模式[，开发人员必须学习这些属性、操作和模式](https://docs.docker.com/engine/reference/builder/)。配置/模板系统变得越复杂，它就越成为自己的高级代码编程语言。

*例如，Tag.bio 使数据工程师能够通过***JSON 模板，快速设计、构建、部署领域驱动的* [*去中心化的数据产品*](https://tag.bio/solutions/data-mesh/) *。**

***Pro-code** 是指用 Python 之类的常规编程语言，或者 [SQL](https://en.wikipedia.org/wiki/SQL) 之类的专用编程语言编写代码，并将这些代码作为可插拔模块插入到更大的软件系统中。Pro-code 提供了优于 low-code 的优势，让开发者在插件/脚本的狭窄范围内完全控制整个过程。*

**例如，Tag.bio 使数据科学家能够通过****pro-code****[*R/Python 插件*](https://tag.bio/solutions/data-science-delivery/) *快速构建和定制数据分析应用，供研究人员使用。***

# **感谢阅读！**

**有什么想法吗？欢迎评论。**