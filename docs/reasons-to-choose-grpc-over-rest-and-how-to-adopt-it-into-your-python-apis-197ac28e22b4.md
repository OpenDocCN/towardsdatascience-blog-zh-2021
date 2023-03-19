# 选择 gRPC 而不是 REST 的原因，以及如何在 Python APIs 中采用它

> 原文：<https://towardsdatascience.com/reasons-to-choose-grpc-over-rest-and-how-to-adopt-it-into-your-python-apis-197ac28e22b4?source=collection_archive---------6----------------------->

## [*小窍门*](https://towardsdatascience.com/tagged/tips-and-tricks)

## gRPC 什么时候会成为 REST 的替代消息传递解决方案？

![](img/7ccae4a2f9949987ca4b20d47e7752ce.png)

照片由 [Mathyas Kurmann](https://unsplash.com/@mathyaskurmann?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 在 [Unsplash](https://unsplash.com/s/photos/mail?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上拍摄

很长一段时间以来，我们一直在休息(表象状态转移)。REST APIs 舒适、温暖、友好，并且非常灵活。如果允许我在这里有点唐突，感谢上帝我不用再用肥皂了！

那么，我们究竟为什么要再次改变一切呢？为什么我们一直在发明新的闪亮的东西来代替工作得如此好的工具？这个 gRPC 现在是什么东西？

嗯，简单的回答是，“没有放之四海而皆准的尺寸。”如果你要尽可能可靠、快速地传递一封信，你会怎么做？我脑海中闪现的第一个想法是我会自己做这件事。跳上我的车，开到目的地，用我自己的手指按门铃。

但也许我没有时间自己做。因此，我可能会呼叫优步并使用相应的服务。现在就等；这太贵了。还有，如果目的地在世界的另一边呢？特快专递服务是最终要走的路。同一问题的不同解决方案。

在任何情况下，当设计微服务之间的通信 API 时，了解使用一种形式的消息传递技术优于另一种形式的许多优点和缺点是至关重要的。在这个故事中，我们看到了何时选择 gRPC 而不是 REST，并在 Python 中实现了 gRPC 服务。

> [学习率](https://www.dimpo.me/newsletter?utm_source=medium&utm_medium=article&utm_campaign=gRPC)是为那些对 AI 和 MLOps 的世界感到好奇的人准备的时事通讯。你会在每周五收到我关于最新人工智能新闻和文章的更新和想法。在这里订阅！

# 什么是休息？

正如我们已经看到的，REST 代表“代表性状态转移”它提供了一套关于如何创建 web APIs 的指南。但也仅此而已，一套*准则*。它不试图强制执行任何东西。

回到我们的邮件递送类比，有一些写信的指南。你可以从日期、信息和收件人的地址开始，然后是称呼。最后，你可以在信的末尾签上自己的名字。

然而，有没有人或事来执行那些规则呢？邮递员会拒绝投递不符合这些准则的信件吗？不要！如果你愿意，你甚至可以在里面画一幅画。毕竟他们说一张图胜过千言万语。

那么 REST 的属性到底是什么呢？

1.  客户机-服务器:客户机发出请求，并从服务器接收响应。客户端不需要知道服务器是如何实现的。它也不需要知道服务器是否发送回缓存的响应，或者它的请求在得到响应之前是否经过了一百层。
2.  无状态:每个请求都是独立的，并且包含了处理它所需的所有信息。服务器不跟踪任何上下文。
3.  统一接口:API 接口应该是一致的，记录消息格式、端点和消息头。

用 Python 设计 REST API 非常简单，但是超出了本文的范围。如果你想看看它是如何做到的，请参考 Python 的 Flask [文档](https://flask.palletsprojects.com/en/2.0.x/quickstart/#a-minimal-application)。

# gRPC 呢？

gRPC 是 Google 开发的一种消息传递技术。但 gRPC 中的“g”并不代表“Google”；实际上是随机的。比如 gRPC `1.12`中的“g”**代表“光荣”。**

**gRPC 与编程语言无关，这似乎是微服务通信领域的新趋势。与 REST 相比，gRPC 以较低的灵活性为代价提供了更高的性能。**

**所以，gRPC 没有提供一套关于如何创建 web APIs 的指南；它执行规则。这一次，如果你在信的开头没有恰当的称呼，邮差会非常生气的！**

**这是它相对于 REST 的主要优势:在大多数情况下，gRPC 更快、更健壮，因为它定义了每个请求和响应应该遵守的一组特定的规则。**

# **gRPC 和 Python**

**现在让我们用 Python 构建一个 gRPC 服务器。我们应该采取的第一步是将我们的消息结构定义为一个`protobuf`文件。在这个例子中，我们假设我们是一家书店，我们想获得目录中的书籍并添加新书。**

**这个文件(即`book.proto`)定义了一个新的消息`BookMessage`，该消息**必须**包含 ISBN 代码、标题、作者、描述、价格和条件，并且按照这个顺序。**

**然后，它定义了一个新的服务(即`BookService`)，可以创建一本新书或者获取所有现有的书。**

**接下来，用 Python 实现 gRPC 涉及两个库:**

*   **`grpcio`运行客户端和服务器端代码**
*   **`grpcio-tools`生成定义代码**

**您可以使用以下命令安装这两个程序:**

```
pip install grpcio grpcio-tools
```

**安装完这些库之后，使用`grpcio-tools`生成构建 gRPC 服务器和客户机所需的代码。运行以下命令:**

```
python -m grpc_tools.protoc -I./ *--python_out=./ --grpc_python_out=./ book.proto*
```

**该命令应该生成两个文件:**

*   **`book_pb2.py`**
*   **`book_pb2_grpc.py`**

**直接编辑那些文件并不是一个好主意。如果您需要更改什么，请更改`.proto`文件并重新运行该命令来重新生成两个 Python 文件。**

**我们现在可以使用这些文件来创建 gRPC 服务:**

**最后，让我们创建一个客户端来测试我们的服务:**

**客户端创建一个新的`BookMessage`对象并提交给服务器。由此，我们假设服务器将这本新书存储在商店的目录中。**

# **结论**

**gRPC 似乎是镇上新的、酷的孩子，但它不是来取代 REST 的。一般来说，REST 有更广泛的应用。另一方面，gRPC 更加结构化，开箱即用通常速度更快。**

**假设你需要速度或者结构，用 gRPC。因此，gRPC 的一个很好的用例是微服务的内部通信。另一方面，最好向外界提供一个 REST 端点，因为每个人都在使用 REST。**

# **关于作者**

**我叫 [Dimitris Poulopoulos](https://www.dimpo.me/?utm_source=medium&utm_medium=article&utm_campaign=gRPC) ，是一名为 [Arrikto](https://www.arrikto.com/) 工作的机器学习工程师。我曾为欧洲委员会、欧盟统计局、国际货币基金组织、欧洲央行、经合组织和宜家等主要客户设计和实施过人工智能和软件解决方案。**

**如果你有兴趣阅读更多关于机器学习、深度学习、数据科学和数据操作的帖子，请关注我的 [Medium](https://towardsdatascience.com/medium.com/@dpoulopoulos/follow) 、 [LinkedIn](https://www.linkedin.com/in/dpoulopoulos/) 或 Twitter 上的 [@james2pl](https://twitter.com/james2pl) 。**

**所表达的观点仅代表我个人，并不代表我的雇主的观点或意见。**