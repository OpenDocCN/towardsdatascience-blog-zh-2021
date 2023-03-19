# 用 Python 探索 Google Analytics 实时数据

> 原文：<https://towardsdatascience.com/exploring-google-analytics-realtime-data-with-python-8625849c7d7a?source=collection_archive---------13----------------------->

## 使用 REST API 和 Python 充分利用所有 Google Analytics 特性和数据

![](img/a90b35a03b42aedabb8244806c66b076.png)

马库斯·温克勒在 [Unsplash](https://unsplash.com/?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上的照片

谷歌分析可以提供很多关于流量和用户访问你的网站的信息。在 web 控制台中，这些数据中有很多都是以很好的格式提供的，但是如果您想要构建自己的图表和可视化，进一步处理数据或者只是以编程方式处理数据，该怎么办呢？这就是 Google Analytics API 可以帮助您的地方，在本文中，我们将了解如何使用它来查询和处理 Python 的实时分析数据。

# 探索 API

在开始使用某些特定的 Google API 之前，先试用其中一些可能是个好主意。使用谷歌的 API explorer，你可以找到哪个 API 对你最有用，它也可以帮助你确定在谷歌云控制台中启用哪个 API。

我们将从[实时报告 API](https://developers.google.com/analytics/devguides/reporting/realtime/v3) 开始，因为我们对实时分析数据感兴趣，其 API explorer 在[此处](https://developers.google.com/analytics/devguides/reporting/realtime/v3/reference/data/realtime/get?apix=true)可用。要找到其他有趣的 API，请查看[报告登录页面](https://developers.google.com/analytics/devguides/reporting)，从这里您可以导航到其他 API 及其浏览器。

为了让这个特定的 API 工作，我们至少需要提供两个值— `ids`和`metrics`。第一个是所谓的*表 ID* ，它是您的分析配置文件的 ID。要找到它，请转到您的[分析仪表板](https://analytics.google.com/analytics/web/)，点击左下角的*管理*，然后选择*视图设置*，您将在*视图 ID* 字段中找到 ID。对于这个 API，您需要提供格式为`ga:<TABLE_ID>`的 ID。

您需要的另一个值是一个度量。您可以在这里从指标栏[中选择一个。对于实时 API，您可能需要`rt:activeUsers`或`rt:pageviews`。](https://developers.google.com/analytics/devguides/reporting/realtime/dimsmets)

设置好这些值后，我们可以单击 execute 并浏览数据。如果数据看起来不错，并且您确定这是您需要的 API，那么是时候启用它并为它设置项目了…

# 安装

为了能够访问 API，我们需要首先在 Google Cloud 中创建一个项目。为此，请转到[云资源管理器](https://console.cloud.google.com/cloud-resource-manager)并点击*创建项目*。或者，你也可以通过命令行界面，用`gcloud projects create $PROJECT_ID`来完成。几秒钟后，你会在列表中看到新项目。

接下来，我们需要为这个项目启用 API。你可以在 [API 库](https://console.cloud.google.com/apis/library)中找到所有可用的 API。我们感兴趣的是*谷歌分析报告 API*——可以在[这里](https://console.cloud.google.com/apis/library/analyticsreporting.googleapis.com)找到。

API 现在可以使用了，但是我们需要凭证来访问它。根据应用程序的类型，有几种不同类型的凭据。其中大多数都适合需要用户同意的应用程序，比如客户端或 Android/iOS 应用程序。用于我们的用例(查询数据和本地处理)的是使用*服务帐户*。

要创建服务帐户，请进入[凭证页面](https://console.cloud.google.com/apis/credentials)，点击*创建凭证*并选择*服务帐户*。给它起个名字，记下服务帐户 ID(第二个字段)，我们马上就要用到它。点击*创建并继续*(无需给予服务账户访问或权限)。

接下来，在[服务帐户页面](https://console.cloud.google.com/iam-admin/serviceaccounts)选择您新创建的服务帐户，并转到*密钥*选项卡。点击*添加密钥*和*创建新密钥*。选择 JSON 格式下载。请确保将其安全存储，因为它可用于在 Google Cloud 帐户中访问您的项目。

完成后，我们现在有了启用了 API 项目和具有以编程方式访问它的凭证的服务帐户。然而，该服务帐户无法访问您的 Google Analytics 视图，因此无法查询您的数据。要解决这个问题，您需要在 Google Analytics 中添加前面提到的服务帐户 ID ( `XXXX@some-project-name.iam.gserviceaccount.com`)作为用户，并具有*读取&分析*访问权限-添加用户的指南可在[此处](https://support.google.com/analytics/answer/1009702)找到。

最后，我们需要安装 Python 客户端库来使用 API。我们需要两个，一个用于认证，一个用于实际的 Google APIs:

# 基本查询

说完这些，让我们编写第一个查询:

我们首先使用我们的服务帐户的 JSON 凭证(之前下载的)对 API 进行身份验证，并将凭证的范围仅限于只读分析 API。之后，我们构建一个用于查询 API 的服务——`build`函数获取 API 的名称、版本和之前创建的凭证对象。如果您想访问不同的 API，请参见此[列表](https://googleapis.github.io/google-api-python-client/docs/dyn/)以了解可用的名称和版本。

最后，我们可以查询 API——我们设置了`ids`、`metrics`和可选的`dimensions`,就像我们之前在 API explorer 中做的那样。你可能想知道我在哪里找到了`service` object ( `.data().realtime().get(...)`)的方法——它们都记录在[这里](https://googleapis.github.io/google-api-python-client/docs/dyn/analytics_v3.data.html)。

当我们运行上面的代码时，`print(...)`将向我们展示如下内容(为了可读性而进行了调整):

这是可行的，但是考虑到结果是 dictionary，您可能希望访问结果的单个字段:

前面的例子展示了 API 的`realtime()`方法的用法，但是我们还可以利用另外两个方法。首先是`ga()`:

该方法从 Google Analytics 返回历史(非实时)数据，并且有更多参数可用于指定时间范围、采样级别、分段等。这个 API 还有额外的必填字段— `start_date`和`end_date`。

您可能还注意到这个方法的度量和维度有点不同——这是因为每个 API 都有自己的度量和维度集。这些总是以 API 的名称为前缀—在本例中是`ga:`，而不是前面的`rt:`。

第三种可用的方法`.mcf()`是针对*多通道漏斗数据*，这超出了本文的范围。如果这听起来对你有用，看看[文档](https://support.google.com/analytics/topic/1191164)。

谈到基本查询，最后要提到的是分页。如果构建返回大量数据的查询，可能会耗尽查询限制和配额，或者在一次处理所有数据时出现问题。为了避免这种情况，您可以使用分页:

在上面的代码片段中，我们添加了`start_index='1'`和`max_results='2'`来强制分页。这使得`previousLink`和`nextLink`被填充，分别用于请求上一页和下一页。然而，这对于使用`realtime()`方法的实时分析并不适用，因为它缺少所需的参数。

# 度量和维度

API 本身非常简单。可定制性很强的部分是`metrics`和`dimensions`等参数。所以，让我们更好地看看所有的参数及其可能的值，看看我们如何才能充分利用这个 API。

从指标开始—有 3 个最重要的值可供选择— `rt:activeUsers`、`rt:pageviews`和`rt:screenViews`:

*   `rt:activeUsers`为您提供当前浏览您网站的用户数量及其属性
*   `rt:pageviews`告诉您用户正在查看哪些页面
*   `rt:screenViews` -与页面浏览量相同，但仅与应用程序相关，如 Android 或 iOS

对于每个指标，可以使用一组维度来分解数据。有太多的方法可以在这里列出，所以让我们看看一些指标和维度的组合，你可以插入到上面的例子中，以获得一些关于你的网站访问者的有趣信息:

*   `metrics='rt:activeUsers', dimensions='rt:userType'` -根据当前活跃用户是新用户还是老用户来区分他们。
*   `metrics='rt:pageviews', dimensions='rt:pagePath'` -按路径细分的当前页面浏览量。
*   `metrics='rt:pageviews', dimensions='rt:medium,rt:trafficType'` -按媒介(如电子邮件)和流量类型(如有机)分类的页面浏览量。
*   `metrics='rt:pageviews', dimensions='rt:browser,rt:operatingSystem'` -按浏览器和操作系统细分的页面访问量。
*   `metrics='rt:pageviews', dimensions='rt:country,rt:city'` -按国家和城市细分的页面浏览量。

如您所见，有很多数据可以查询，由于数量庞大，可能有必要对其进行过滤。要过滤结果，可以使用`filters`参数。语法非常灵活，支持算术和逻辑运算符以及正则表达式查询。让我们看一些例子:

*   `rt:medium==ORGANIC` -仅显示有机搜索的页面访问量
*   `rt:pageviews>2` -仅显示页面浏览量超过 2 次的结果
*   `rt:country=~United.*,ga:country==Canada` -仅显示来自以“United”(英国、美国)或加拿大开头的国家的访问(`,`充当`OR`操作符，`AND`使用`;`)。

有关过滤器的完整文件，请参见本页。

最后，为了使结果更具可读性或更容易处理，您还可以使用`sort`参数对它们进行排序。对于升序排序，您可以使用`sort=rt:pagePath`，对于降序排序，您可以在前面加上`-`，例如`sort=-rt:pageTitle`。

# 超越实时 API

如果你找不到一些数据，或者你错过了实时分析 API 中的一些功能，那么你可以尝试探索其他谷歌分析 API。其中之一可能是[报告 API v4](https://developers.google.com/analytics/devguides/reporting/core/v4) ，它比旧的 API 有一些改进。

然而，它在构建查询时也有一些不同的方法，所以让我们看一个例子来帮助您开始:

如您所见，这个 API 没有提供大量您可以填充的参数，相反，它只有一个`body`参数，该参数将我们之前看到的所有值放入请求体中。

如果你想更深入地了解它，那么你应该查看文档中的[示例](https://developers.google.com/analytics/devguides/reporting/core/v4/samples)，它们给出了它的特性的完整概述。

# 结束语

尽管本文只展示了分析 API 的用法，但它应该能让你大致了解如何将所有 Google APIs 用于 Python，因为客户端库中的所有 API 都使用相同的通用设计。此外，前面展示的身份验证可以应用于任何 API，您需要更改的只是范围。

虽然本文使用了`google-api-python-client`库，但谷歌也在[https://github.com/googleapis/google-cloud-python](https://github.com/googleapis/google-cloud-python)为个人服务和 API 提供了轻量级库。在撰写本文时，用于分析的特定库[仍处于测试阶段，缺乏文档，但当它正式发布(或更稳定)时，您可能应该考虑探索它。](https://github.com/googleapis/python-analytics-data)

*本文最初发布于*[*martinheinz . dev*](https://martinheinz.dev/blog/62?utm_source=medium&utm_medium=referral&utm_campaign=blog_post_62)

</secure-password-handling-in-python-6b9f5747eca5>  </all-the-ways-to-compress-and-archive-files-in-python-e8076ccedb4b>  </the-unknown-features-of-pythons-operator-module-1ad9075d9536> 