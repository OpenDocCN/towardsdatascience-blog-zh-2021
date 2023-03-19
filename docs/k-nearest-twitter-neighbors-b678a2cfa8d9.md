# 使用 K 近邻预测推文的作者

> 原文：<https://towardsdatascience.com/k-nearest-twitter-neighbors-b678a2cfa8d9?source=collection_archive---------29----------------------->

## (相对)直观算法的介绍

嗨！我是劳拉，我在学习机器学习。我也是加州州立大学东湾分校的数学讲师，很幸运能够作为数据科学研究员与我的导师 [Prateek Jain](https://www.linkedin.com/in/prateekj/) 在[sharpes minds](https://www.sharpestminds.com/)共事。这个项目被选为我从头开始练习编写机器学习算法的一种方式(不允许 scikit-learn！)并因此深入学习和理解 k-最近邻算法，或 kNN。

如果您还不熟悉 kNN，那么这是一个很好的 ML 算法，因为它相对直观。邮政编码通常是个人的有用代表，因为住在同一街区的人通常有相似的经济背景和教育程度，因此也可能有共同的价值观和政治观点(尽管不是保证！).因此，如果你想预测某项特定的立法是否会在某个地区通过，你可以对该地区的一些选民进行民意调查，并假设这些选民的大多数邻居对你的法案的看法与你调查的大多数人相似。换句话说，如果你被问及该地区的某个人可能会如何投票，你肯定不知道他们会如何投票，但你会看看他们的受访邻居说他们会如何投票，并假设你的人会投票给投票中最常见的答案。

![](img/3410b9ff07c1b9fec0beaef31bfac703.png)

[粘土堤](https://unsplash.com/@claybanks?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)在 [Unsplash](https://unsplash.com/s/photos/political-map?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上拍摄的照片

kNN 的工作方式非常相似。给定一组已标记的数据和一个未标记的数据点`x`，我们将查看`x's`邻居中的某个数量`k`，并将这些`k`邻居中最常见的标签分配给`x`。让我们来看一下:在下图中，我们有一个未标记的点和它的邻居的 15 个邻居。在这 15 个邻居中，9 个有“A”标签，6 个有“B”标签。

![](img/72a634b9d93502d5547374b3ebcadac1.png)

由我插画！

由于更多的未标记点的邻居有“A”标签而不是“B”标签，我们应该用“A”来标记我们的点。那就是 kNN！

但是，请注意 kNN 不是什么:我们没有给我们的点加上它的最近邻居的标签！我们的点最接近 6 个“B”点，但我们没有称它为“B”。这种区别在我写代码的时候让我犯了一个错误，也许是因为算法名称中的“最近”这个词？总之，我一直在编写一个函数，它查看一个数据点的邻居，并选择距离该数据点 k 以内的邻居，这实际上是一个完全不同的算法:半径邻居分类器。我应该再做一次，将结果与 kNN 进行比较！或许在另一篇博客中。

关于 kNN 不是什么还需要知道的是，对于存在于高维空间的问题，kNN 不是一个好的算法选择(剧透:这将成为这篇文章的笑点！).由于 kNN 假设彼此“接近”的数据点很可能是相似的，并且由于在高维空间中数据可以分散得非常远，所以“接近”的概念变得毫无意义(关于这一点的更长解释，请查看这个[视频](https://youtu.be/DyxQUHz4jWg)！).想想澳大利亚和新西兰的岛屿:它们是彼此最近的邻居，但它们仍然相距数千公里，因此它们有非常不同的气候、地形和生态系统。

![](img/c1e590dd6d9d0380dbba0441afb7e4a6.png)![](img/bc61c0682f12c820f68067e9eb94be17.png)

左图:澳大利亚沙漠的图片，由 [Greg Spearritt](https://unsplash.com/@mcoot20?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 在 [Unsplash](https://unsplash.com/s/photos/australian-desert?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上拍摄。右图:Unsplash[上的新西兰森林](https://unsplash.com/s/photos/new-zealand-forest?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)[托比亚斯·图利乌斯](https://unsplash.com/@tobiastu?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)的照片。

好了，现在我们知道了 kNN 是什么，不是什么，让我们来看看我是如何编码的。请注意，这显然是一个非常初学者的练习，它不会像更有经验的从业者那样优雅或巧妙。我的指示是从头开始编写所有代码，所以我没有使用 Pandas 来读取我的数据，也没有查看其他人是如何做到的。我不建议你使用我的代码，我也不需要评论或电子邮件来说明你不喜欢它，因为我的任务是学习这个算法，并花更多的时间在我的 [IDE](https://www.jetbrains.com/pycharm/) 而不是 Jupyter 笔记本上，我成功实现了这些目标。:)

我的导师建议我可以在这个项目中使用 Twitter 数据，我需要能够下载单个作者的大量推文。理想情况下，这位作者会有非常独特的写作风格，使算法更容易识别出一条未标记的推文是否属于该作者。这是 2020 年的夏天，美国的总统选举正在紧锣密鼓地进行，所以我的导师认为用唐纳德·特朗普作为测试案例会很有趣(很多推文？没错。与众不同的写作风格？没错)。注意:这个项目绝对不是对前一个人的认可，实际上我几乎是从另一个作者的新推文开始的，因为我不想给他更多的空间，但我最终决定反对范围蔓延，只是完成这个项目。

我发现了一个[网站，其中 TFG 的所有推文](https://dataverse.harvard.edu/dataset.xhtml?persistentId=doi:10.7910/DVN/KJEBIL)都聚集到一个 txt 文件中，还有一个 [Kaggle 数据集](https://www.kaggle.com/rtatman/the-umass-global-english-on-twitter-dataset)，其中有超过 10，000 条不同人的推文，放在一个 tsv 文件中，作为算法的对比。然后，我需要编写一个函数，将这些文件中的数据读入数组，这样我就可以使用它们了。

但是我的数据不是以 csv 文件的形式出现的；它在一个 txt 文件和一个 tsv 文件中！所以我需要另一个函数来将这些其他文件类型转换成 csv。我决定通过包含几种不同文件类型的情况来使该函数更加强大:

现在，我有一组来自 TFG 的推文和一组其他人的推文，但是如果我照原样比较它们，我会浪费很多时间来比较无处不在的词，如“the”或“and”，这些词被称为“停用词”，因为它们对分析没有意义。所以我找到了一个停用词列表，并编写了一个函数将它们从数组中删除，同时也删除了数字和标点符号，因为它们对分析也没有帮助。我还将所有字符转换为小写。最后，我需要制作一个“[单词包](https://en.wikipedia.org/wiki/Bag-of-words_model)”或语料库，以包含所有 tweets 中使用的所有单词的列表，在清理文本的同时这样做是有意义的，所以我只保留每个“清理”单词的副本。

接下来，我需要对我的数据进行矢量化，以便以后能够使用 NumPy 函数。为此，我初始化了一个 NumPy 数组，其行数与我的 tweets 数相同，每行的长度等于语料库中单词的数量+ 2。我添加了两个额外的列，这样最后一列将跟踪 twee 是否是 TFG 写的(1 代表 TFG，0 代表其他人)，倒数第二个字符将存储 tweet 的索引，以便我可以在解释时引用它。对于每条推文，我的函数将查看语料库中的每个单词，如果该单词出现在推文中，则在引用该单词的列中输入“1”，如果该单词没有出现在该推文中，则输入“0”。这意味着这些是非常稀疏的数组(大部分是零)——哦，预示着！

然后我需要随机化 NumPy 数组中向量的顺序，这样我就可以进行训练和测试。

我对数据进行了拆分，使 80%进入训练集，20%进入测试集。

快到了。现在，我需要一种方法来比较两条推文彼此之间的相似程度，我将通过获取它们向量之间差异的绝对值来完成(绝对值是因为 1–0 与 0–1 在这个目的上是相同的，因为两者都意味着一个单词出现在一条推文中，而不是另一条推文中)。单词的每一个差异都会使两条推文之间的“距离”增加 1。

现在该是找到最近邻居的时候了！我需要编写一个函数，它将接收一个 tweet 向量和数字`k`，并比较所有其他 tweet，以找到与有问题的 tweet 最相似的`k`列表并返回它。

最后，我需要一个函数来确定前一个函数返回的列表中的`k`tweet 的最常见标签，这是我添加到每条 tweet 末尾的 1 或 0，表示作者将派上用场。

太好了，我们做到了！然而，事实证明，我的程序把我扔出去的每条推文都归类为 TFG 写的，即使我很确定它不是。

Womp womp 长号 Gif(通过[https://media.giphy.com/media/D0feQxfAek8k8/giphy.gif](https://media.giphy.com/media/D0feQxfAek8k8/giphy.gif))

我意识到推文向量的稀疏性意味着——有点像澳大利亚和新西兰——两条推文是彼此最近的邻居并不意味着推文实际上彼此非常相似。但是没关系！这个项目的目的是让我从头开始编写 kNN，并学习在没有熊猫的情况下处理数据。(我还学会了如何[处理](https://www.geeksforgeeks.org/understanding-python-pickling-example/)我的数据，如何以及为什么在我的函数中使用[类型转换](https://www.geeksforgeeks.org/understanding-python-pickling-example/)，以及如何使用 [Sphinx](https://www.sphinx-doc.org/en/master/) 为我的项目创建[文档——尽管它还不能正常工作。还在学！).所以这个项目达到了它的目的，即使准确率最终非常糟糕。](http://lauralangdon.io/knn/)

我希望你喜欢我学习旅程中的这一站！你可以在这里查看我以前的帖子。

在 [LinkedIn](https://linkedin.com/in/laura-langdon/) 上和我联系，或者在 [Twitter](https://twitter.com/laura_e_langdon) 上和我打招呼！你也可以在 [lauralangdon.io](https://lauralangdon.iol) 找到我。