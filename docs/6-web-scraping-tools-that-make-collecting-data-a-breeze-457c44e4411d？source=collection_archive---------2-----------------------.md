# 6 个网页抓取工具，让收集数据变得轻而易举

> 原文：<https://towardsdatascience.com/6-web-scraping-tools-that-make-collecting-data-a-breeze-457c44e4411d?source=collection_archive---------2----------------------->

## 任何数据科学项目的第一步都是数据收集。

![](img/2172f238c24c28224e2601cdee1c3b06.png)

马库斯·斯皮斯克在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上的照片

没有数据就没有完整的数据科学项目；我甚至可以主张，没有数据就不能说“数据科学”。通常，在大多数数据科学项目中，您需要分析和用来建立机器学习模型的数据存储在某个数据库中。某个地方有时是网络。

您可能会从关于特定产品的特定网页或社交媒体收集数据，以发现模式或进行情感分析。无论您为什么收集数据或打算如何使用数据，从网络上收集数据(网络搜集)是一项非常乏味的任务，但您需要为您的项目完成它的目标。

网络抓取是你作为数据科学家需要掌握的重要技能之一；你需要知道如何寻找、收集和清理你的数据，这样你的结果才是准确和有意义的。

[](/choose-the-best-python-web-scraping-library-for-your-application-91a68bc81c4f) [## 为您的应用选择最佳的 Python Web 抓取库

### 前 5 个库的概述以及何时使用它们。

towardsdatascience.com](/choose-the-best-python-web-scraping-library-for-your-application-91a68bc81c4f) 

网络抓取一直是一个灰色的法律领域，所以在我们深入研究可以帮助您的数据提取任务的工具之前，让我们确保您的活动完全合法。2020 年，美国法院将公开数据的网络抓取完全合法化。也就是说，如果任何人都可以在网上找到这些数据(比如维基文章)，那么刮掉这些数据就是合法的。

但是，当您这样做时，请确保:

1.  你不会以侵犯版权的方式重复使用或重新发布数据。
2.  你尊重你试图抓取的网站的服务条款。
3.  你有一个合理的爬行速度。
4.  你不要试图抓取网站的隐私部分。

只要你不违反这些条款，你的网络抓取活动应该是合法的。

如果您正在使用 Python 构建您的数据科学项目，那么您可能会使用 BeatifulSoup 和 requests 来收集您的数据并使用 Pandas 来分析它。本文将向您介绍 6 种不包含 BeatifulSoup 的 web 抓取工具，您可以免费使用它们来收集您下一个项目所需的数据。

# №1: [普通爬行](https://commoncrawl.org/)

Common Crawl 的创建者开发了这个工具，因为他们相信每个人都应该有机会探索和分析他们周围的世界，并揭示其模式。他们向任何好奇的人免费提供只有大公司和研究机构才能获得的高质量数据，以支持他们的开源信念。

这意味着，如果你是一名大学生，一名在数据科学中导航的人，一名寻找下一个感兴趣的主题的研究人员，或者只是一个喜欢揭示模式和发现趋势的好奇的人，你可以使用这个工具，而不用担心费用或任何其他财务问题。

公共爬行提供原始网页数据和文本提取的开放数据集。它还为教育工作者教授数据分析提供了非基于代码的使用案例和资源支持。

# №2: [猥琐](http://crawly.diffbot.com/)

Crawly 是另一个惊人的选择，特别是如果您只需要从网站提取基本数据，或者如果您希望提取 CSV 格式的数据，以便您可以在不编写任何代码的情况下分析它。

你所需要做的就是输入一个 URL，你的电子邮件地址来发送提取的数据，以及你想要的数据格式(选择 CSV 或 JSON)，瞧，抓取的数据就在你的收件箱里供你使用。您可以使用 JSON 格式，然后使用 Pandas 和 Matplotlib 在 Python 中分析数据，或者使用任何其他编程语言分析数据。

虽然如果你不是程序员，或者你刚刚开始接触数据科学和 web scarping，Crawly 是完美的，但它有其局限性。它只能提取有限的 HTML 标签集，包括:`Title`、`Author`、`Image URL`和`Publisher`。

[](/data-science-lingo-101-10-terms-you-need-to-know-as-a-data-scientist-981aa17d5cdf) [## 数据科学行话 101:作为数据科学家你需要知道的 10 个术语

### 理解基础数据科学术语的指南

towardsdatascience.com](/data-science-lingo-101-10-terms-you-need-to-know-as-a-data-scientist-981aa17d5cdf) 

# №3: [内容抓取器](https://contentgrabber.com/Manual/understanding_the_concept.htm)

内容抓取器是我最喜欢的网络抓取工具之一。原因是，它非常灵活；如果你只想删除一个网页，不想指定任何其他参数，你可以使用他们简单的图形用户界面。然而，如果你想完全控制提取参数，内容抓取器给你这样做的选项。

内容抓取的优势之一是你可以安排它从网上自动抓取信息。众所周知，大多数网页会定期更新，因此定期提取内容非常有益。

它还为提取的数据提供了多种格式，从 CSV、JSON 到 SQL Server 或 MySQL。

# №4: [Webhose.io](https://webhose.io/)

Webhose.io 是一个 web scraper，允许您从任何在线资源中提取企业级的实时数据。Webhose.io 收集的数据是结构化的，clean 包含情感和实体识别，并有 XML、RSS、JSON 等不同格式。

Webhose.io 为任何公共网站提供全面的数据覆盖。此外，它提供了许多过滤器来细化您提取的数据，以便您可以在更少的清理任务之前直接进入分析阶段。

Webhose.io 免费版每月提供 1000 个 HTTP 请求。付费计划提供更多通话，对提取数据的权力，以及更多好处，如图像分析和地理定位以及长达 10 年的存档历史数据。

[](/5-chrome-extension-to-ease-up-your-life-as-a-data-scientist-c5c483605d0d) [## 5 Chrome 扩展，让您作为数据科学家的生活更加轻松

### 这些扩展对于流畅的工作流是必须的

towardsdatascience.com](/5-chrome-extension-to-ease-up-your-life-as-a-data-scientist-c5c483605d0d) 

# №5: [ParseHub](https://www.parsehub.com/)

ParseHub 是一个强大的网络抓取工具，任何人都可以免费使用。只需点击一下按钮，它就能提供可靠、准确的数据提取。您还可以安排抓取时间，以保持数据最新。

ParseHub 的优势之一是它可以轻松地删除更复杂的网页。你甚至可以指示它搜索表格、菜单、登录网站，甚至点击图像或地图来进一步收集数据。

还可以为 ParseHub 提供各种链接和一些关键字，它可以在几秒钟内提取相关信息。最后，您可以使用 REST API 下载提取的数据，以 JSON 或 CSV 格式进行分析。您还可以将收集的数据导出为 Google Sheet 或 Tableau。

# №6: [报废蜜蜂](https://bit.ly/2P8gRAA)

我们列表中的最后一个抓取工具是 Scrapingbee。Scrapingbee 为 web 抓取提供了一个 API，它甚至可以处理最复杂的 Javascript 页面，并将它们转换成原始的 HTML 供您使用。此外，它有一个使用谷歌搜索的网络抓取专用 API。

报废蜜蜂有三种用途:

1.  一般的 Web 抓取，例如，提取股票价格或客户评论。
2.  搜索引擎结果页面通常用于 SEO 或关键字监控。

3.成长黑客，包括提取联系信息或社交媒体信息。

Scrapingbee 提供了一个免费计划，包括 1000 个信用点和无限使用的付费计划。

# 最后的想法

在数据科学项目工作流程中，为项目收集数据可能是最无趣也是最乏味的一步。这项任务可能相当耗时，如果你在公司工作，甚至是自由职业者，你知道时间就是金钱，这总是意味着如果有更有效的方式来做某事，你最好利用它。

好消息是网页抓取不一定是乏味的；您不需要执行它，甚至不需要花费太多时间来手动执行它。使用正确的工具可以帮助您节省大量时间、金钱和精力。此外，这些工具对于分析师或没有足够编码背景的人来说是有益的。

[](/6-data-science-certificates-to-level-up-your-career-275daed7e5df) [## 6 个数据科学证书提升您的职业生涯

### 充实你的投资组合，离你梦想的工作更近一步

towardsdatascience.com](/6-data-science-certificates-to-level-up-your-career-275daed7e5df) 

当你想要选择一个工具来抓取网页时，你需要考虑一些因素，如 API 集成和大规模抓取的可扩展性。本文向您介绍了一些可用于不同数据收集机制的工具；给他们一个机会，选择一个让你的下一个数据收集任务变得轻而易举的方法。