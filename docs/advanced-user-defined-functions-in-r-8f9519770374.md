# R 中的高级用户定义函数

> 原文：<https://towardsdatascience.com/advanced-user-defined-functions-in-r-8f9519770374?source=collection_archive---------41----------------------->

您是否遇到过这样的情况:您希望有一个内置函数来实现某个结果，而不是为它编写多行代码？你想知道如何优化你的 R 代码吗？您知道使用 UDF 可以轻松编写 R 代码吗？UDF 是基本的构建块，您可以在编写代码时即插即用。请继续阅读，了解更多…

![](img/b92171d6526ad527340b72814e34f720.png)

瑞安·昆塔尔在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

UDF(用户定义函数)使你的代码结构化，有助于容易地识别错误，并使将来的修改更加容易。一旦你习惯了编写和定义 UDF，就没有回头路了！你会沉迷于它们对你有多有用。

如果你是一个初学者，并且想知道如何创建用户自定义函数的基础知识，请阅读我下面提到的文章:

</your-first-user-defined-function-in-r-1eedc634ead4>  

现在我们都准备好了，让我们从一些高级 UDF 的例子开始。

# 在 UDF 中使用多个“if 条件”

UDF 非常灵活，可以通过在 UDF 中设置多个“if 条件”和内置函数来定义。请在下面的链接中找到一个从摄氏温度到华氏温度的转换函数，反之亦然。我使用了简单的“如果条件”和内置函数来创建这个 UDF。

<https://github.com/shruthigeetha/R_UDFunctions/blob/main/TemperatureConversion.R>  

现在，这种 UDF 可以修改，以包括其他温度单位和更多的故障安全方法。请随意投稿。

# 在 UDF 中运行“for loops”

可以在 UDF 中使用 For 循环，以确保对数据子集而不是整个数据执行类似的操作。

以下示例中的结果可以通过多种方式获得，如使用简单的聚合函数或应用函数。通过这个例子，我想提出一种非传统的方法，使用 UDF 从每日数据中获取月度信息。该 UDF 可用于任何类似的数据集，以获得每月的汇总信息。

<https://github.com/shruthigeetha/R_UDFunctions/blob/main/UDF_WithForLoops.R>  

# UDF 内的 UDF

事情变得有趣多了！

在 UDF 里用过 UDF 吗？现在你们都是专家了，我们可以有嵌套的 UDF 来帮助简化你的代码。这是向编写易于解释和解码的紧凑代码迈出的一大步！

为此，我将使用之前的温度转换和月度数据提取示例，向您展示 UDF 内 UDF 的示例。

<https://github.com/shruthigeetha/R_UDFunctions/blob/main/UDFs_within_UDFs.R>  

在上面的例子中，我获得了以摄氏温度而不是最初的华氏温度来表示的每月信息。

# 下一步是什么？

现在，您可以在日常的 R 代码中尝试并开始使用 UDF 了。从上面的例子中，我们可以清楚地看到创建可重用和多用途的 UDF 是多么灵活和容易。

如果你能想到 UDF 可以应用的其他场景，或者如果你有任何可以利用 UDF 的主题，请随时发表评论或打开一个讨论论坛。