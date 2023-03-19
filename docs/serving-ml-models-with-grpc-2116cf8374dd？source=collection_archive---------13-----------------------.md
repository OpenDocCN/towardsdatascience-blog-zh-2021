# 使用 gRPC 服务 ML 模型

> 原文：<https://towardsdatascience.com/serving-ml-models-with-grpc-2116cf8374dd?source=collection_archive---------13----------------------->

## 跳过休息，给 gRPC 一个尝试

![](img/033afd65673cf3a0957d88605b5839cc.png)

照片由[米卡·鲍梅斯特](https://unsplash.com/@mbaumi?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

大多数希望将新训练的 ML 模型投入生产的人转向 REST APIs。这就是我认为您应该考虑使用 gRPC 的原因。

# 等等！休息怎么了！？

没什么！REST APIs 的主要好处是它们的普遍性。每一种主要的编程语言都有一种制作 HTTP 客户端和服务器的方法。并且有几个现有的框架可以用 REST APIs 包装 ML 模型(例如 BentoML、TF Serving 等)。但是，如果您的用例不适合这些工具中的任何一个(即使它适合)，您可能会发现自己想要编写一些更加定制的东西。同样，使 REST APIs 变得通用的东西也会使它们难以使用。

# gRPC 是什么？

正如它的[网站所说的](https://grpc.io/)，gRPC 是“一个高性能、开源的通用 RPC 框架”，最初由 Google 开发。gRPC 核心的三个主要元素是:代码生成、HTTP/2 和协议缓冲区。

[协议缓冲区](https://developers.google.com/protocol-buffers)是一种二进制结构化数据格式，由 Google 设计，体积小，速度快。gRPC 服务及其请求/响应消息格式都在`.protobuf`文件中定义。

gRPC 客户机和服务器代码是从`.protobuf`定义文件中以您喜欢的语言生成的。然后，您填充业务逻辑来实现 API。

[基于 HTTP/2](https://developers.google.com/web/fundamentals/performance/http2) 的传输为 gRPC 提供了[几个关键优势](https://grpc.io/blog/grpc-load-balancing/#why-grpc):

*   二元协议
*   双向流
*   标题压缩
*   在同一连接上多路复用多个请求

好吧，但这对我意味着什么？

# 为什么使用 gRPC？

我们从几个角度来比较 gRPC 和 REST。

## 类型安全和文件

因为 gRPC APIs 是通过 protobufs 定义的，所以它们本身是文档化的和类型安全的。相反，REST APIs 没有这样的保证，你需要像 OpenAPI 这样的额外工具来定义和记录你的服务，还需要一个库来验证客户端请求。

## 速度、二进制数据和流

gRPC 充分利用 HTTP/2 和协议缓冲区，使您的 API 尽可能快。与 REST 的纯文本、JSON 编码的消息相比，gRPC 消息由高效打包的二进制数据组成。

[一个经常被引用的测试](https://medium.com/@EmperorRXF/evaluating-performance-of-rest-vs-grpc-1b8bdf0b22da#:~:text=gRPC%20is%20roughly%207%20times,of%20HTTP%2F2%20by%20gRPC.)显示 gRPC 大约比 REST 快 7-10 倍。虽然对于较小的请求，差异可能不太明显，但 ML 模型的输入通常很大(例如，大型数据表、要处理的图像，甚至是视频)，其中压缩和二进制格式表现突出。

事实上，由于协议缓冲区允许二进制数据，请求可以是用 [Apache Arrow](https://arrow.apache.org/) 或 [Apache Parquet](https://parquet.apache.org/) 编码的大型数据表。此外，由于 HTTP/2 的功能，大的二进制消息可以分解成块并进行流式传输。

# 缺点和替代方案

gRPC 当然不是完美的。例如，以下是您可能会遇到的一些问题:

*   初期发育较慢
*   不常用
*   消息不是人类可读的，这使得调试更加困难
*   需要生成客户端库

其他方法可能更适合您的工作流程。 [BentoML](https://www.bentoml.ai/) 、 [TF 发球](https://www.tensorflow.org/tfx/guide/serving)和 [Ray 发球](https://docs.ray.io/en/latest/serve/index.html)都是为 ML 模特发球的绝佳选择。或者，如果你正在寻找一些更加可定制的东西， [FastAPI](https://fastapi.tiangolo.com/) 和 [Flask](https://flask.palletsprojects.com/en/2.0.x/) 是两个很好的选择，可能是更好的搭配。

另外，对于部分方法，您也可以考虑将您的消息格式从 JSON 交换到 [BSON](https://bsonspec.org/) 或 [MessagePack](https://msgpack.org/index.html) 。

# 结论/TL；速度三角形定位法(dead reckoning)

gRPC APIs 快速且易于使用。它们本质上是类型安全的，它们允许双向流消息(例如，将大文件分成块)，并且它们使用快速有效的消息格式(协议缓冲区)。

下次您需要通过 API 提供 ML 模型时，请考虑使用 gRPC！

# 进一步阅读

*   [gRPC](https://grpc.io/) ( [gRPC Python 库](https://grpc.io/docs/languages/python/quickstart/))
*   [协议缓冲区](https://developers.google.com/protocol-buffers)
*   [HTTP/2](https://developers.google.com/web/fundamentals/performance/http2)

# 脚注

[1]我知道“RESTful”这个术语可能有点太宽泛了——它适用于任何基于 HTTP 的 API。在本文中，我使用 REST API 的通俗定义。

[2]这有点过于简化了——gRPC 是高度可定制的。例如，可以用 JSON 代替 protobufs，用 HTTP/1 代替 HTTP/2。但是…你应该吗？

感谢阅读！我很想听听你对这篇文章的看法。你用 gRPC 服务 ML 模型吗？还是为了别的？或者你更喜欢休息？请在评论中告诉我，或者随时联系 [Twitter](https://twitter.com/austin_poor) 或 [LinkedIn](https://www.linkedin.com/in/austinpoor/) ！