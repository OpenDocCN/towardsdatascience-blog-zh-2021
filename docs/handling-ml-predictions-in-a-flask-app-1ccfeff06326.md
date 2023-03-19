# 在烧瓶应用程序中处理 ML 预测

> 原文：<https://towardsdatascience.com/handling-ml-predictions-in-a-flask-app-1ccfeff06326?source=collection_archive---------37----------------------->

## 不要让长时间运行的代码拖慢你的 Flask 应用程序

![](img/285997d2ce2fc1551a64deb8a9347475.png)

[HalGatewood.com](https://unsplash.com/@halacious?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍照

我为 Metis 数据科学训练营做的一个项目涉及创建一个 Flask 应用程序的 MVP，向用户显示电影推荐(想想网飞主屏幕)。

我的建议包括模型预测与 SQL 查询的结合——所有这些都是在请求进来时、响应发出之前完成的。演示日到来时，加载网站主页大约需要 30 秒。

我的 Flask 应用程序的简化版代码

公平地说，这是我在*数据科学*训练营的一个很短的期限内创建的一个 MVP 不是网络开发训练营。尽管如此，30 秒的等待时间并不太好。

毕业后，一旦我有了更多的时间，我就重新审视我的项目，看看我还能改进什么。

这里有两个我可以探索的选项，它们可以大大加快我的页面加载时间，而不必改变我的预测算法。

# 1)加载页面，然后进行预测

不要在返回主页之前进行预测(就像上面的代码一样)，而是将预测代码与 Flask 应用程序中的页面响应代码分开。返回没有预测的页面。然后，加载页面后，使用 JavaScript 调用 API。下面是更新后的 Flask 应用程序代码的样子:

更新 Flask 应用程序，其中 ML 预测被移动到单独的路线

下面是 JavaScript 代码的样子:

用于查询 Flask API 的 JavaScript 代码片段

这是对初始代码的一个小改动，但对用户来说却有很大的不同。该页面最初可以加载占位符图像或加载栏，以便用户在等待加载预测时仍然可以与您的站点进行交互。

# 2)把工作交给芹菜

通过在 Flask 响应函数中运行 ML 预测或复杂的 SQL 查询等缓慢的过程，您会使 Flask 服务器陷入困境。这可能不是你关心的问题，取决于你期望得到多少流量。也许这只是一个概念验证，或者一次只有少数人会使用你的服务。在这种情况下，只要使用我们的 API 方法就可以了。否则，您可能需要考虑一个可以水平扩展的解决方案。

进入[Celery](https://docs.celeryproject.org/en/stable)——一个用于创建“分布式任务队列”的 python 库。使用 Celery，您可以创建一个工人池来处理收到的请求，这就像在 python 函数中添加一个装饰器一样简单。

对于我们新的 Celery 工作流，我们将把 API 路径分成两部分:一部分用于安排预测，另一部分用于获取预测结果。

让我们来看看更新后的 Flask 片段:

这是新的芹菜片段:

以及更新后的 JavaScript:

现在我们开始加载页面，然后 JavaScript 将安排模型预测，并继续检查结果，直到它们准备好。

诚然，这增加了我们解决方案的复杂性(您需要添加一个像 Redis 这样的代理，并启动一个单独的芹菜工人进程)，但它允许我们通过向我们的池中添加所需数量的芹菜工人来横向扩展我们的应用程序。

要查看完整的示例，请查看 GitHub repo:

<https://github.com/a-poor/flask-celery-ml>  

感谢阅读！我很想听听你的反馈。让我知道你使用什么技术来添加 ML 到 Flask 应用程序中。请在下面留下你的评论，或者在 Twitter 或 LinkedIn 上联系我。