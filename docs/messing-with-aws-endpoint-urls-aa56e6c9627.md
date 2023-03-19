# 扰乱 AWS 端点 URL

> 原文：<https://towardsdatascience.com/messing-with-aws-endpoint-urls-aa56e6c9627?source=collection_archive---------40----------------------->

## AWS CLI 不知道的不会伤害它。

![](img/8e7a717db971a02a58a3d6f34f1457d7.png)

照片由[this engineering RAEng](https://unsplash.com/@thisisengineering?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)在 [Unsplash](https://unsplash.com/s/photos/technology?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

如果您键入`aws s3 ls s3://my-bucket`来列出一个 S3 桶的内容，那么您会期望连接到真正的桶并列出它的内容，这是完全有道理的。

但是没有硬性规定说你*必须*连接到真正的桶。事实上，有一个简单的参数[可以传递给上面的 CLI 命令，从而轻松地连接到您选择的任何 URL。](https://docs.aws.amazon.com/cli/latest/reference/)

请考虑以下情况:

```
$ aws s3 ls s3://my-bucket --endpoint-url http://localhost:5000
```

注意包含了指向端口 5000 上的本地主机的参数`--endpoint-url`？我们不会在某个地方的 AWS 服务器上列出`my-bucket`的内容，相反，我们会检查我们自己的机器，寻找能够响应`ls`命令的类似桶的资源。

这就引出了一个问题:为什么 AWS 要像这样公开一个参数，为什么我们要使用它？

事实证明，两者都有很好的理由。在本文中，我们将讨论两种情况，在这两种情况下，搞乱`endpoint_url`是有用的。

# 用例 1:本地测试

任何在本地测试过他们代码的人都知道当调用外部依赖时情况会变得多么令人担忧。例如，我们同意单元测试最好是*而不是* *实际上*将测试记录插入到我们的数据库中。

为了解决这个问题，我们有一些策略。我们可以[嘲笑](https://en.wikipedia.org/wiki/Mock_object)。我们可以做 T21 的猴子补丁。或者我们会不愉快地在代码中添加测试专用的逻辑。

通过运行 AWS 服务的[本地实例](https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/DynamoDBLocal.DownloadingAndRunning.html)，然后将代码中的`endpoint_url`参数设置为 localhost 和正确的端口，我们可以轻松有效地模拟服务。

让我们来看看实际情况。

## 使用 moto 的示例

作为一个完整的例子，我会提到优秀的 moto docs，这是一个专门用来模仿 AWS 服务的包。

Moto 有一个独立的服务器模式，这使得它的工作原理特别清晰。pip 安装包后，启动本地 moto 服务器:

```
$ moto_server s3
 * Running on http://127.0.0.1:5000/
```

然后在一段代码中，我们可以使用 endpoint_url 指向本地的 moto S3 实例:

```
mock_s3 = boto3.resource(
    service_name='s3',
    region_name='us-east-1',
    **endpoint_url='http://localhost:5000'**,
)
```

现在我们可以在`mock_s3`对象上做任何我们想做的操作，而不用担心改变实际存储桶的内容。

# 用例 2:无缝集成

鉴于“云的兴起”，云服务的 API 变得无处不在也就不足为奇了。不管是好是坏，AWS S3 API 现在是“对象存储领域事实上的标准”[ [1](https://min.io/)

当多种技术采用相同的标准时，集成每一种成对组合的痛苦就消失了。

[lakeFS](https://lakefs.io/) 、 [MinIO](https://min.io/) 和 [Ceph](https://ceph.io/ceph-storage/object-storage/) 是说 S3 语的技术的例子。或者更准确地说，它们与 S3 API 的一个有意义的子集保持兼容。

因此，任何希望连接到 S3 的工具也可以通过端点 URL 与这些工具无缝集成！

## 以 Spark 和 lakeFS 为特色的示例

Spark 是经常与 S3 互动的技术的一个例子。最常见的是将数据读入数据帧，进行一些转换，然后将其写回 S3。

lakeFS 旨在增强对象存储上的数据湖的功能，使分支、合并和恢复等操作成为可能。

如果您想同时利用 Spark 和 lakeFS 的优势，并且无法设置自定义端点 URL，那么就必须在两者之间开发一个定制的集成。

幸运的是，通过可配置的端点，集成变成了一个[单行程序](https://docs.lakefs.io/using/spark.html)，将 Spark 的 S3 端点指向一个 lakeFS 安装:

```
spark**.**sparkContext**.**hadoopConfiguration**.**set**(**"fs.s3a.endpoint"**,** "https://s3.lakefs.example.com"**)**
```

现在从 Spark 访问 lakeFS 中的数据与从 Spark 访问 S3 数据完全一样！

# 最后的想法

虽然只是对这个主题的介绍，但希望您对什么是端点 URL 以及何时改变它们的默认值有更好的理解。

*注:本文首次发表于 2011 年 4 月 20 日 lakeFS 博客* *上。*