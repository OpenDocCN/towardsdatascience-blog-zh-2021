# 流处理变得简单

> 原文：<https://towardsdatascience.com/stream-processing-made-easy-5f4892736623?source=collection_archive---------59----------------------->

## 卡夫卡+react vex = Maki Nage

![](img/2e8a41f2f4b6c6bafcb1fa0209b9aadf.png)

[迈克·刘易斯 HeadSmart 媒体](https://unsplash.com/@mikeanywhere?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)在 [Unsplash](https://unsplash.com/s/photos/waterfall-bridge?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上拍摄的照片

越来越多的数据科学用例是实时完成的:警报、缺陷检测、预测、自动恢复就是一些例子。然而，实施和部署它们可能非常具有挑战性。 [Maki Nage](https://www.makinage.org) 是一个 Python 框架，旨在简化实时处理应用的开发和部署。本文介绍了如何使用它，说明应该简化流处理！

# 解决推理差距

在我们接触一些例子之前，让我简单解释一下为什么我们创建 Maki Nage。在我的数据实验室，我们经常致力于实时流媒体服务。在实践中，我们构建的算法和机器学习模型是由卡夫卡流提供的。然而，我们用于学习和训练的数据是从数据湖中检索的。这些是批量数据，而推理是以流模式运行的。因此，我们为数据准备编写的代码不能用于部署:我们通常使用 Pandas 和 Spark 来处理批量数据，没有办法在部署中重用它。所以最后另一个团队必须重新实现特性工程部分，通常是用 Java 和 Kafka 流。这种情况在数据科学中相当普遍。流媒体通常会加剧这种情况。

最终，这种双重实现会降低部署速度，并增加潜在的错误，因为数据在训练和推理之间的转换方式不同。

Maki Nage 旨在缩小这一推理差距。它通过以下方式实现:

*   公开一个简单且富于表现力的 Python API。
*   以流模式处理批处理数据。
*   准备部署为 Kafka 服务，无需额外的基础设施。

已经有一些工具可以进行流处理。但是他们中的大多数目标更多的是开发者而不是数据科学家:Kafka Streams、Apache Flink 和 RobinHood Faust 就是这样的框架。Spark 结构化流似乎是以专用集群为代价的例外。

Maki Nage 允许运营团队部署由数据科学家编写的代码。尽管如此，它支持整个 Python 数据科学栈。

现在让我们开始吧！

# 第一步

在本文中，我们将使用 Maki Nage 的数据科学库: [RxSci](https://github.com/maki-nage/rxsci/) 。使用 pip 以通常的方式进行安装:

```
python3 -m pip install rxsci
```

在接下来的示例中，我们将使用整数列表作为数据源:

```
import rx
import rxsci as rs

source = rx.from_([1, 2, 3, 4, 5, 6, 7, 8])
```

*from_* 操作符从 Python Iterable 创建事件流。在 RxSci 术语中，一个流被称为一个*可观测的*，一个事件被称为一个*项*。这些名字来自 [ReactiveX](http://reactivex.io/documentation/observable.html) ，Maki Nage 的一个构建模块。

这里，源可观察对象由一个小列表组成，但它也可能是一个不适合内存的大数据集。流式处理的一个优点是可以处理比可用系统内存大得多的数据集。

现在让我们处理这些数据。

# 无状态转换

第一类转换是无状态操作。在这些操作符中，输出仅依赖于输入项，而不依赖于过去接收到的其他项。你可能已经知道其中一些了。 *map* 操作符是许多转换的构建块。它对每个源项应用一个函数，并发出另一个应用了该函数的项。下面是它的使用方法:

```
source.pipe(
    rs.ops.map(lambda i: i*2)
).subscribe(on_next=print)
```

结果是:

```
2 4 6 8 10 12 14 16
```

第一行中的*管道*操作符允许连续链接几个操作。在这个例子中，我们只使用了*映射*操作符来加倍每个输入项。最后的 subscribe 函数是执行计算的触发器。RxSci API 是*惰性的*，这意味着您首先定义一个计算图，然后运行它。

另一个广泛使用的操作符是*过滤器*。*过滤器*操作员根据条件丢弃项目:

```
source.pipe(
    rs.ops.filter(lambda i: i % 2 == 0)
).subscribe(on_next=print)
```

结果是:

```
2 4 6 8
```

# 有状态转换

第二类转换是有状态转换。这些运算符的输出取决于输入项和某些累积状态。有状态操作符的一个例子是 *sum* :计算总和依赖于当前项目的值和所有先前收到的项目的值。总和是这样计算的:

```
source.pipe(
    rs.state.with_memory_store(rx.pipe(
        rs.math.sum(reduce=True)
    )),
).subscribe(on_next=print)
```

结果是:

```
36
```

使用有状态转换时有一个额外的步骤:它们需要一个状态存储来存储它们的累积状态。顾名思义， *with_memory_store* 操作符创建一个内存存储。还要注意*和*操作符的*减少*参数。默认行为是发出每个输入项的当前总和。Reduce 意味着我们希望只有当源可观察性完成时才发出一个值。显然，这只对非无限的可观察对象有意义，比如批处理或窗口。

# 作文

当将所有这些基本块组合在一起时，RxSci 开始变得很棒。与大多数数据科学库不同，聚合并不是世界末日:在聚合之后可以使用任何可用的操作符。

考虑下面的例子。它首先将项目分组为奇数/偶数。然后，对于每个组，应用 2 个项目的窗口。最后，在这些窗口中的每一个上，计算总和。这是用 RxSci 用非常有表现力的方式写出来的:

```
source.pipe(
    rs.state.with_memory_store(rx.pipe(
        rs.ops.group_by(lambda i: i%2, pipeline=rx.pipe(
            rs.data.roll(window=2, stride=2, pipeline=rx.pipe(
                rs.math.sum(reduce=True),
            )),
        )),
    )),
).subscribe(on_next=print)
```

结果是:

```
4 6 12 14
```

# 现实的例子

我们将以一个现实的例子来结束本文:实现基于天气指标预测房屋耗电量的特性工程部分。

如果您想了解更多细节，可在[制造文档](https://www.makinage.org/doc/makinage-book/latest/handson_home.html)中找到完整示例。

我们使用的数据集是一个 CSV 文件。我们将使用它的下列栏目:大气压力，风速，温度。我们想要的功能是:

*   温度。
*   大气压和风速之间的比率。
*   滑动窗口内温度的标准偏差。

这些都是简单的功能。它们应该在几行代码中实现！

首先是存储要素和读取数据集的一些准备工作:

```
Features = namedtuple('Features', [
    'label', 'pspeed_ratio', 'temperature', 'temperature_stddev'
])
epsilon = 1e-5
source = csv.load_from_file(dataset_path, parser)
```

然后，处理分三步完成:

```
features = source.pipe(
    # create partial feature item
    rs.ops.map(lambda i: Features(
        label=i.house_overall,
        pspeed_ratio=i.pressure / (i.wind_speed + epsilon),
        temperature=i.temperature,
        temperature_stddev=0.0,
    )),
    # compute rolling stddev
    rs.state.with_memory_store(rx.pipe(
        rs.data.roll(
            window=60*6, stride=60,
            pipeline=rs.ops.tee_map(
                rx.pipe(rs.ops.last()),
                rx.pipe(
                    rs.ops.map(lambda i: i.temperature),
                    rs.math.stddev(reduce=True),
                ),
            )
        ),
    )),
    # update feature with stddev
    rs.ops.map(lambda i: i[0]._replace(temperature_stddev=i[1])),
)
```

第一步将 CSV 数据集的输入行映射到一个*特征*对象。除了温度的标准偏差之外的所有字段都在该运算符之后准备就绪。

然后创建一个步长为 1 小时的 6 小时滚动窗口。对于每个窗口，计算温度的标准偏差。然后转发每个窗口的最后一项。

最后一步是更新 Features 对象。第二步中使用的 *tee_map* 操作符对每个输入项进行了多次计算。它输出一个元组，每个计算分支一个值。这里我们有两个，所以元组中有两个条目:第一个是窗口的最后一个特征对象，第二个是温度的标准偏差。

# 更进一步

希望您对 Maki Nage APIs 的外观以及如何用它们处理时间序列有一个很好的了解。从一开始就记住，目标之一是使用相同的代码进行训练和推理。最后一个例子是机器学习模型的特征工程阶段。该代码可以用于在生产中处理来自 Kafka 流的数据。

Maki Nage 包含将 RxSci 代码部署为服务的工具。它还有一个模型服务工具来部署与 MlFlow 打包在一起的模型。

请阅读[文档](https://www.makinage.org/doc/makinage-book/latest/index.html)了解更多详情。

在撰写本文时，Maki Nage 仍处于 alpha 阶段，但我们预计不会有重大的 API 变化。我们仍在致力于该框架的许多方面，您可以参与进来:请随意尝试，给出一些反馈，以及[贡献](https://github.com/maki-nage/)！

*原载于 2021 年 1 月 11 日*[*【https://blog.oakbits.com】*](https://blog.oakbits.com/stream-processing-made-easy.html)*。*