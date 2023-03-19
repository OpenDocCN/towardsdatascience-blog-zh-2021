# 用于排列测试的 BigQuery 存储过程

> 原文：<https://towardsdatascience.com/bigquery-stored-procedure-for-permutation-test-35597d6379e4?source=collection_archive---------21----------------------->

## 在 Google BigQuery SQL 中运行置换测试的简单方法

存储的过程允许我们执行打包成一个“函数”的多个**Google biqquery SQL**操作。了解**如何使用存储过程快速有效地对任何数据集应用置换测试**。

> 只需要密码？从这里开始别忘了启动它！⭐️

![](img/c29c1a618da8e924ac3d51a221d5f03a.png)

向您的 BigQuery SQL pantry 添加另一罐技巧|照片由 [Luisa Brimble](https://unsplash.com/@luisabrimble?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 在 [Unsplash](https://unsplash.com/s/photos/pantry?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上发布

我还有几个 BigQuery 教程:

</advanced-random-sampling-in-bigquery-sql-7d4483b580bb>  </loops-in-bigquery-db137e128d2d>  </load-files-faster-into-bigquery-94355c4c086a>  

> 要获得所有媒体文章的完整访问权限，包括我的文章，请考虑在此订阅。

# 什么是存储过程？

![](img/032514fe6799bf37418572028161647d.png)

不，存储过程不存储在软盘上|照片由 [Fredy Jacob](https://unsplash.com/@thefredyjacob?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 在 [Unsplash](https://unsplash.com/s/photos/memory?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上拍摄

在 BigQuery 中，可以编写自定义，**用户自定义函数**(UDF)。这些函数非常棒，因为您可以减少代码中的重复，使代码对其他人更具可读性。然而，你也可以在 BQ 中编写**脚本。这包括循环、if 语句、变量声明等。不允许在函数中使用它，因为函数是作用于表元素的，而不是作用于脚本中的变量。**

这就是存储过程的用武之地。把它们看作是函数的等价物，但不是脚本的等价物。有了它们，您可以重复所有复杂的 SQL 逻辑，即使它们包括变量和 for 循环的赋值。

> 这篇来自 Google Cloud 的[文章介绍了存储过程的概念，如果你需要复习 BigQuery 脚本，](https://cloud.google.com/blog/products/data-analytics/command-and-control-now-easier-in-bigquery-with-scripting-and-stored-procedures)[这里有关于如何编写循环和条件的官方文档](https://cloud.google.com/bigquery/docs/reference/standard-sql/scripting)。

谈到存储过程，有几个问题。最大的问题是它们可以接受一个或多个输入，但是输出必须是声明的变量。例如，要运行文档中的示例，我们必须首先声明一个适当的变量来存储结果:

这个例子摘自[官方公告](https://cloud.google.com/blog/products/data-analytics/command-and-control-now-easier-in-bigquery-with-scripting-and-stored-procedures)。

> 如果你不知道上面的程序在做什么，也不用担心。我们稍后会在这里做一个例子。

一旦您用`CREATE PROCEDURE`命令创建了您的过程，它将像 BigQuery 中的任何其他表一样显示，并且您将能够用通常的`dataset.procedure()`符号调用它。

让我们看看这一切是如何工作的。

# 带有存储过程的斐波那契数

![](img/2c417e54b39c42313d3e7df4b8c4957f.png)

斐波那契无处不在|照片由[Yair MeJA](https://unsplash.com/@20ymn17?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)在 [Unsplash](https://unsplash.com/s/photos/sunflower?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上拍摄

我总是发现，如果例子中的逻辑很容易理解，那么例子是最好的。为此，我将创建一个存储过程，当调用时，**计算第 n 个斐波纳契数**。

作为复习，斐波纳契数列是一个自然数序列，其中第 n 个元素由`F(n)`表示，并由`F(n) = F(n-1) + F(n-2)`和 `F(0) = 0`以及`F(1) = 1`定义。所以你得到:0，1，1，2，3，5，8，13…

为了做到这一点，我们将首先使用脚本来完成，然后将其包装在一个过程中。我们需要做的是:

1.  **初始化我们的变量**——我们需要记录我们已经计算了多少个数字，以及何时完成计算
2.  做一个 **for 循环**，其中每一步我们**更新我们当前跟踪的 2 个数字**
3.  **选择我们的答案**并打印到控制台

我在上面的脚本中添加了一堆注释，所以要确保你明白是怎么回事。

> 我总是建议先写脚本，然后把它改成存储过程。您会发现这样调试容易多了。

这些关键字执行以下操作:

*   `DECLARE`创建一个具有特定类型的**新变量，并将其设置为默认值**
*   `SET` **将已经声明的变量**更新为新值
*   `WHILE <cond> DO`和`END WHILE` **缠绕我们的 while 循环**。
*   `SELECT`是你的**普通 SQL 语句**打印到控制台的答案

如果我们运行上面的代码，我们会将`13`打印到控制台，这是第 7 个斐波那契数列的正确答案。通过更改代码的第一行并将`i`设置为其他值来尝试其他数字。

现在剩下要做的就是将上述内容包装在一个过程中，并将其存储在一个数据集中。我们是这样做的:

变化不大，但有几件重要的事情需要注意:

*   我们**通过`CREATE PROCEDURE`声明我们的过程**,后面是数据集和我们想要存储它的过程名。*确保您已经创建了数据集* `*ds*` *！*
*   我们必须**声明我们的过程输入**—`i INT64`在过程的顶部。
*   我们对它的输出做同样的事情:`OUT answer INT64`。
*   输入和输出不再需要在过程内部声明，所以我们`SET`将答案设为 0，而不是使用默认值。
*   我们**将整个脚本**包装在`BEGIN` `END`语句中。

如果您运行上面的代码，您应该会得到类似于:*该语句创建了一个名为 your-project-name.ds.fibonacci 的新过程。*

为了使用这个过程，我们需要声明一个整数变量和`CALL`过程来填充它。

如果你都做对了，你应该得到:55 (8+13 =21，13+21 = 34，21+34=55)。🎉

仅此而已。您现在知道了如何使用 BigQuery 脚本编写存储过程。给自己一个读到这里的掌声吧！

我想，即使是岩石也赞同 BigQuery 过程… | [来源](https://media.giphy.com/media/ZU9QbQtuI4Xcc/giphy.gif)

# 什么是排列测试？

![](img/b943cae17bf6d5fb10e9be4945b4c9d8.png)

由[卡洛斯·鲁伊斯·华曼](https://unsplash.com/@carlos_ruizh?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)在 [Unsplash](https://unsplash.com/s/photos/alpaca?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上拍摄的照片

我听到你在说什么了。"**这些都很酷，但为什么有用呢？**”。好吧，如果我告诉你，你可以用 SQL 在一些巨大的数据集上运行一些超级的统计测试，而且很便宜，会怎么样呢？这正是我们在这里要做的😉！

我知道我不会很好地解释什么是排列测试，所以看看 Jared Wilber 的这个很棒的网站[,在那里你会读到一个羊驼洗发水的实验。](https://www.jwilber.me/permutationtest/)

简而言之，这就是排列测试:

*   无效假设是两组的统计数据相同。
*   你用统计学来衡量差异(在我们的例子中是指)。
*   然后，假设零假设是真的，你在 2 组之间排列观察。这里的逻辑是，如果零假设确实是真的，那么你如何给你的实验对象贴上标签并不重要。
*   然后你数一数有多少次你观察到排列的统计差异比正确标记的设置更极端。
*   这一比率将给出您的 p 值，即我们由于随机运气观察到这样或更极端值的概率。
*   如果 p 值很低(例如< 5%) then you reject the null hypothesis.

But as I said, [Jared 的网站](https://www.jwilber.me/permutationtest/)做得更好，所以请去看看。

# 使用 BigQuery SQL 进行排列测试

![](img/69c1743f527c7a37b57bd019ff0fc5e6.png)

使用 BigQuery 的强大功能，我们可以快速有效地调整数据集|图片来自[Amol ty agi](https://unsplash.com/@amoltyagi2?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)on[Unsplash](https://unsplash.com/s/photos/shuffle?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

我再次建议，在用程序把事情复杂化之前，先弄清楚你的脚本的**逻辑。在我们做任何事情之前，我们需要设定一些要求。我们需要一个至少有两列的表:一列是我们要取平均值的值，另一列是我们的组标签的 0 和 1。我们还假设我们的样本大小相等。这不是问题，因为我们将使用 BigQuery 中 iris 数据集的子集，其中 Virginica 和 Versicolor 物种都有 50 个观察值。**

我们准备好实施我们的计划了。这是我们要做的:

1.  **将我们的表格列**重命名为 measurement and treatment(0 和 1 ),以使脚本的其余部分**更加一致**。
2.  添加另一个`row_id`列**以确保我们能够以一种简单的方式对**进行采样。我在这里解释了我们如何以及为什么要这样做[。](/advanced-random-sampling-in-bigquery-sql-7d4483b580bb)
3.  **用整数列表进行交叉连接**——每个置换样本一个。
4.  记录原始表格的**观察差异**。
5.  记录我们排列的**平均差异**。
6.  **计算我们得到的差高于或等于我们原始均值差的次数的比率**。

下面是 SQL 的样子:

以上只是普通的 SQL，我一直建议你以这种方式开始。当我们想把它变成一个程序时，事情就变得复杂了…

我们希望我们的过程接受下面的**输入**:

*   我们要对其应用测试的表的名称。
*   包含测量值的列的名称。
*   包含我们的治疗标志的列的名称。
*   我们想要进行的排列的数量。

据我所知，我们不能只是将表名粘贴到脚本中，所以我们将在 BigQuery 中使用可爱且非常强大的`EXECUTE IMMEDIATE`命令来完成这项工作。这个家伙基本上运行一个字符串作为 SQL 语句，**允许我们动态地编写 SQL 字符串，然后执行它**。相当整洁！

是的，立即执行就是那种强大的… | [来源](https://media.giphy.com/media/MCZ39lz83o5lC/giphy.gif)

由于我们的查询很长，我决定使用`CONCAT`函数将多行字符串组合成一行。这对于调试来说非常烦人，所以我建议您**运行您的函数的第一遍，将合成的 SQL 复制到一个新窗口中，并使用 BigQuery UI 在那里调试您的查询**——相信我，您会看到许多遗漏的逗号和空格弄乱了您的查询😉，我当然做了。

第 15、16 和 19 行是奇迹发生的地方。

这比我们之前的查询更难理解，但基本上是一样的。我只是在列名和表名中使用了字符串串联`||`到**粘贴**以及我们在前 2 个表中要求的排列数。其余的不变——因此有了标准化的列名😃。

运行以上程序来存储您的过程！然后我们准备准备我们的桌子。我们将使用经典的免费 Iris 数据集，它可以作为 BigQuery 表在`bigquery-public-data:ml_datasets.iris`下获得。我们将扔掉`setosa`品种，因为我们只需要 2 个，我们将询问海滨锦鸡儿花是否有更大的萼片宽度。如果你需要复习虹膜数据集，我[推荐这款 Kaggle 笔记本](https://www.kaggle.com/benhamner/python-data-visualizations)。

我特意将所有的专栏都留在那里，以便您也可以对其他专栏尝试这个过程。

我们有桌子和程序。是时候创建一个浮点变量并调用我们的排列测试了:

运行上面的程序，我得到的 p 值是 0.0008499，非常小。因此，我们得出结论，弗吉尼亚种的萼片宽度明显大于云芝种。

BigQuery 的美妙之处在于，我们**为查询的字节**付费，而不是为 CPU 时间付费。因此，我们可以将排列增加到 100 万，但仍然只为 1.56KBs 的查询付费。

🙏请**在一些较大的数据集上尝试上述方法**并评论一下它完成查询的速度，我很想知道你的结果。

# 结论

![](img/42897208739a7720db7618b4e9f58066.png)

照片由 [Massimo Sartirana](https://unsplash.com/@sarti46?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 在 [Unsplash](https://unsplash.com/s/photos/finished?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上拍摄

希望您会发现以上内容对您的数据科学工作流程有用。现在，您不必将大型 BigQuery 数据集导出到磁盘，租用半 TB 的内存机器并在其上运行 Python 来做一些统计测试，**您可以在 BigQuery 中便宜而有效地完成所有这些工作**。

如果没有别的，请记住以下几点:

*   一个**存储过程是一个可重用的 BigQuery 脚本**(带有循环和变量)，它接受输入并产生一个输出变量。
*   程序以`CREATE PROCEDURE dataset.procedure_name(inputs..., output) BEGIN`开始，以`END`结束。
*   不要`DECLARE`输入或输出变量。使用`SET`改变变量值。
*   `EXECUTE IMMEDIATE`可能是最强大的脚本关键字，允许您将任何字符串作为 SQL 语句执行。
*   用`CONCAT`和`||`将表名和变量名粘贴到 SQL 字符串中。
*   总是首先在过程之外调试 SQL 脚本！

*感谢一路读到最后。我写了一些关于 BigQuery、Julia 和 Python 的文章，所以如果这些教程对你有帮助的话，请随时关注我。*

想要更多 BigQuery 采样教程吗？[看看这个](/advanced-random-sampling-in-bigquery-sql-7d4483b580bb)。