# Apache Spark 中的本机会话化

> 原文：<https://towardsdatascience.com/native-sessionization-in-apache-spark-624883a8a9e2?source=collection_archive---------10----------------------->

在 Spark 3.2.0 的下一个版本中，在许多新特性中，有一个将使我们在使用 Spark 结构化流解决*会话化*问题时变得更加容易。

在进入细节之前，让我们澄清一些流概念，如*窗口。*

![](img/13bbd51df147e995d9f256a9c17138fa.png)

照片由[卢卡·布拉沃](https://unsplash.com/@lucabravo?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)在 [Unsplash](https://unsplash.com/s/photos/windows?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 拍摄

当你向窗外看时，你只能看到陆地的一部分，而不能看到全部。这里是一样的🤓

在流式传输中，由于数据流是无限的，所以我们不能尝试使用批处理方法对数据进行分组，这也没有意义。

> 那么，你是如何分组的呢？

窗口、窗口和窗口。

Windows 将数据流分成有限大小的“块”，我们可以在这些块上进行计算。如前所述，没有他们，这是不可能的。

最常见的窗口类型是*翻转窗口、滑动窗口、*和 ***会话窗口。***

你可以想象，在这篇文章中，我们将讨论后者。

> **会话窗口**

会话窗口捕获数据中的一段活动时间，该时间因**非活动间隙**而终止。与*翻滚*和*滑动*窗口相反，它们没有重叠，也没有固定的开始和结束时间。

非活动间隙将用于关闭当前会话，并且以下事件将被分配给新会话。

作为会话的一个例子，考虑在您最喜欢的音乐提供商那里听音乐。

一旦您播放了第一首歌曲，将为您的用户创建一个会话窗口，这个会话将在一段时间(比如 10 分钟)不活动后结束。

这意味着，如果你只听一首歌，窗口将在 10 分钟后关闭，但如果你继续听更多的歌曲，窗口不会关闭，直到达到不活动的时期。

当研究用户在特定时间段内参与某项活动时，它们在数据分析中特别有用。

> 如何在 Spark< 3.2.0

This would be too long to cover 😩, and there are great posts about it, as a hint search for *flatMapGroupsWithState 中创建会话窗口？*

> 如何在 Spark ≥3.2.0 中创建会话窗口

这正是你在这里的原因，今天是你的幸运日，因为它们比以前简单多了，简单多了🎉。

你需要定义的只是你的时间列(必须是 *TimestampType)* 和不活动的间隙。

```
def session_window(timeColumn: Column, gapDuration: String)
```

让我们举个简单的例子。

> 输入数据:

```
{"time":"2021-10-03 19:39:34", "user_id":"a"} - first event
{"time":"2021-10-03 19:39:41", "user_id":"a"} - gap < 10 seconds
{"time":"2021-10-03 19:39:42", "user_id":"a"} - gap < 10 seconds
{"time":"2021-10-03 19:39:49", "user_id":"a"} - gap < 10 seconds
{"time":"2021-10-03 19:40:03", "user_id":"a"} - **gap > 10 seconds**
```

> 让我们使用会话窗口对这些数据进行分组:

```
val sessionDF = df
    .groupBy(*session_window*('time, "10 seconds"), 'user_id)
    .count
```

简单对吗？😭

> 输出:

```
+------------------------------------------+-------+-----+
|session_window                            |user_id|count|
+------------------------------------------+-------+-----+
|{2021-10-03 19:39:34, 2021-10-03 19:39:59}|a      |4    |
+------------------------------------------+-------+-----+
```

如您所见，我们有 4 个事件落在第一个会话窗口中，因为非活动期始终小于 10 秒。第五个事件将出现在该用户的下一个会话窗口中，因为活动间隔大于 10 秒。

## 结论

Spark 发展很快，以前不可能的事情现在可能会发生，跟上时代总是好的，不仅是 Spark，而是每项技术。