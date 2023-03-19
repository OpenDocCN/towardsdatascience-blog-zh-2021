# 3 种方法可以极大地提高你自己的编程技能

> 原文：<https://towardsdatascience.com/3-ways-to-drastically-improve-your-programming-skills-on-your-own-9359ce670099?source=collection_archive---------1----------------------->

## 成为更好的程序员

![](img/0f8cd62503867171706f3df8d4e0e15f.png)

[帕卡塔·高](https://unsplash.com/@pakata?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上的照片

我一直认为自己是一个体面的程序员，但是，我从来没有任何东西来衡量它。嗯，直到我看到一个真正优秀的程序员是什么样子——现在我痴迷于发展我的技能。

我不解释发生了什么，我给你看。任务是重构代码，分析来自 [UCI 机器学习库](https://archive.ics.uci.edu/ml/datasets/wine+quality)的葡萄酒质量数据集。

**我建议所有读者在继续阅读本文其余部分之前，尝试一下我创建的** [**谷歌实验室**](https://colab.research.google.com/drive/1tE1TkBi8pmNRN0nTplH2i7OtHEtJKDDi?usp=sharing) **中的挑战。**

> **免责声明**:此任务取自 Udacity 上的机器学习工程师 Nanodegree。

在我揭示启发我的无知的解决方案之前，让我向你展示课程开发者要求我们改进的解决方案…

```
**# Course providers solution**
labels = list(df.columns)
labels[0] = labels[0].replace(' ', '_')
labels[1] = labels[1].replace(' ', '_')
labels[2] = labels[2].replace(' ', '_')
labels[3] = labels[3].replace(' ', '_')
labels[5] = labels[5].replace(' ', '_')
labels[6] = labels[6].replace(' ', '_')
df.columns = labelsdf.head()
```

这似乎是一个非常简单的任务。以下是我的解决方案…

```
**# My improvement of the solution**
def remove_spaces(df):
    cols = list(df.columns)
    for i in range(len(cols)):
        cols[i] = cols[i].replace(" ", "_")
    df.columns = cols
    return dfdf = remove_spaces(df)
df.head()
```

有人可能会说我的解决方案是对他们解决方案的改进，因为它消除了代码的重复性——因此遵守了软件工程中的“不要重复自己(DRY)”的最佳实践。但是，这不是最佳解决方案，还可以改进。

以下是课程提供商如何优化他们的解决方案…

```
df.columns = [label.replace(' ', '_') for label in df.columns]
df.head()
```

**简单大方。**

他们的解决方案最让我恼火的是我理解。我很清楚其中的逻辑。我可以像他们一样写出理解清单。但是，我想不出解决办法。我从未想过要做一个列表理解，现在想起来很疯狂，因为我用了一个`for`循环。

这个失误激活了我最近的任务。我没有一天不在努力提高我的编程技能。

下面，我列出了我每天用来提高编程技能的 3 种方法。

## #1 阅读代码

不言而喻……如果你想成为一个更好的作家，你必须成为一个更好的读者——这意味着读更多的书，以及更广泛的书。

同样，如果你想成为一名更好的程序员，这实际上是一种不同形式的写作，你应该寻求阅读更多的代码，尤其是来自非常优秀的程序员的代码。

一些有很好代码的 Github 库是:

*   [Scikit-Learn](https://github.com/scikit-learn/scikit-learn)
*   [由](https://github.com/jjrunner/stackoverflow) [JJruner](https://github.com/jjrunner) 从 Stackoverflow 得到的发现
*   [自举](https://github.com/twbs)

它不会停留在阅读代码上。

有很多书可以帮助你成为一名更好的程序员——一本让你入门的流行书籍是戴维·托马斯·安德鲁·亨特的《实用程序员》。

> **注意**:点击上面的图书链接，你将通过我的会员链接被导向亚马逊。我还集成了地理链接，所以如果你不在英国，你会被自动引导到你当地的亚马逊商店。

## #2 留出重构时间

老实说，最初，我采取了“如果有效，那就是好的”的心态。重构代码总是被推迟。事后看来，这其实挺傻的。我绝不会在不反复阅读一两次的情况下发表一篇文章，以确保我传达了我想要传达的信息。

当然，代码重构有不同的目的。重构代码的目的是提高代码的效率和可维护性，或者两者兼而有之。

要成为一个更好的程序员，你必须留出时间来重构。为了提高你的重构技能，你必须学习重构——这会给你一个寻找什么的想法。最后，确保你投入大量时间重构代码。您可以重新访问过去的项目或其他人的项目，并修改他们的代码，使其更有效、更易维护，或者两者兼而有之。

## #3 边做边练

如果你想成为一名更好的作家，你必须多写。如果你想成为一个更好的厨师，你必须多做饭。如果你想成为一个更好的程序员，你必须写更多的程序。

你可以偷到一个小窍门来写更多的程序，那就是从写很多小程序开始。这将允许你增加每天编写的代码量，这将允许你创建更多的程序。

然而，大量的小程序并不能涵盖优秀程序员所需的编程技能。在某些时候，从编写大量小程序过渡到编写更大的程序是很重要的，因为这将揭示一系列新的挑战，迫使你成为一名更好的程序员。

## 最终想法

虽然这些方法在您独自工作以提高编程技能时非常有用，但在现实世界中，您很可能会与其他人合作。从我的小经验来看，真正的成长来自于你走出孤立，开始与他人一起工作，尤其是那些比你聪明得多的人，因为你可以采用他们的方法成为更好的程序员。

如果你认为我遗漏了一些想法，请留下评论，这样我们可以继续协调发展。

感谢阅读！

如果你喜欢这篇文章，请通过订阅我的**免费** [每周简讯](https://mailchi.mp/ef1f7700a873/sign-up)与我联系。不要错过我写的关于人工智能、数据科学和自由职业的帖子。

## 相关文章

[](/data-scientist-should-know-software-engineering-best-practices-f964ec44cada) [## 数据科学家应该知道软件工程的最佳实践

### 成为不可或缺的数据科学家

towardsdatascience.com](/data-scientist-should-know-software-engineering-best-practices-f964ec44cada) [](/developing-proficiency-with-new-programming-languages-and-frameworks-1bfe622be28b) [## 熟练掌握新的编程语言和框架

### 如何更快的发展流利度

towardsdatascience.com](/developing-proficiency-with-new-programming-languages-and-frameworks-1bfe622be28b) [](/thriving-as-a-remote-data-scientist-98f9aa79f5ab) [## 作为远程数据科学家蒸蒸日上

### 我远程工作的经历

towardsdatascience.com](/thriving-as-a-remote-data-scientist-98f9aa79f5ab)