# 如何测试你的 Spark Scala 代码

> 原文：<https://towardsdatascience.com/how-to-test-your-spark-scala-code-268e5de471fd?source=collection_archive---------8----------------------->

## 让我们使用 Mockito 和 scalatest 为 Spark Scala 数据帧转换编写一些测试

![](img/fbdd79501ce32ecdc0af0f84aac682ab.png)

克里斯托夫·高尔在 [Unsplash](https://unsplash.com/s/photos/code?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上拍摄的照片

Spark 转换的单元测试可能很棘手，可能你甚至不能编写 try 单元测试([我喜欢 stackoverflow](https://stackoverflow.com/a/43769845/5444759) 的这个回答)。然而，您需要以某种方式测试您的转换。我写了这段代码来分享我最近在这个问题上的经验以及我是如何解决这个问题的。

# 问题陈述:

假设您的数据管道中有一个如下结构的对象:

它读取数据->转换数据->加载到某个存储。简单的 ETL，很容易。现在让我们使用 Mockito 和 scalatest 为该对象编写一个测试。

# 写作测试

对于测试，我将使用 [mockito-scala](https://github.com/mockito/mockito-scala) 和 [scalatest](https://www.scalatest.org/) 库来使用它，您需要将它添加到您的 build.sbt:

```
libraryDependencies ++= Seq("org.scalatest" %% "scalatest" % "3.2.2" % Test,"org.mockito" %% "mockito-scala" % "1.16.23" % Test)
```

现在实际的测试类:

万岁，我们刚刚为我们的火花代码做了一个测试🎉🎉 🎉

## 让我们重复一下我们在这里所做的事情:

1.  我们用 mockito 模拟 IO 方法，所以我们没有加载实际的文件，而是不调用*loaddatafrombaute*方法，而是替换 DataFrame，它通常会返回我们为测试准备的测试数据
2.  类似的事情发生在 *writeResultsToBucket* 方法上，它从未被调用过
3.  我们用 ArgumentCaptor 捕获 spark 转换的结果。
4.  我们使用简单的断言比较了转换的实际结果和预期结果

# 您可能会考虑的一些其他事项:

**测试专用火花会议:**

你可以从 scalatest*(org . scalatest . BeforeAndAfter)*中使用 before and after，请看这里的例子:[https://www.scalatest.org/getting_started_with_fun_suite](https://www.scalatest.org/getting_started_with_fun_suite)

或者只为测试创建一个专用的 spark 会话。

**如何比较数据帧:**

比较数据帧的更好方法可能是借助 spark-fast-tests 库:[https://github.com/MrPowers/spark-fast-tests/](https://github.com/MrPowers/spark-fast-tests/)

特别是如果出于某种原因你需要比较大的数据帧，那么这个库将会非常方便

**测试数据:**

上面描述的方法依赖于您需要为您的测试准备的数据样本，因为您可能不希望查询与您的生产代码稍后要做的一样多的数据。所以你可以做以下列表中的事情:

*   如果您的样本非常小，比如只有 1-2 列，请在存储库中保存一个小的数据样本
*   作为测试的一部分，随时生成数据，基本上将测试数据硬编码在 scala 代码中
*   将样本数据保存在某个远程存储桶中，并在测试期间加载它
*   最后，您可以从数据库中查询示例数据

每种选择都有其利弊，我个人更喜欢将测试数据以某种人类可读的格式存储在存储库中，比如 JSON。但是这完全取决于你项目的具体情况。

**感谢阅读！让我知道你的想法，以及你如何为 spark 编写测试？欢迎随时在** [**LinkedIn**](https://www.linkedin.com/in/aosipenko/) **，**[**GitHub**](https://github.com/subpath)**或我的网站**[**piece-data.com**](https://piece-data.com/)

# 参考资料:

1.  莫奇托-斯卡拉:[https://github.com/mockito/mockito-scala](https://github.com/mockito/mockito-scala)
2.  https://www.scalatest.org/
3.  火花快速测试:[https://github.com/MrPowers/spark-fast-tests/](https://github.com/MrPowers/spark-fast-tests/)