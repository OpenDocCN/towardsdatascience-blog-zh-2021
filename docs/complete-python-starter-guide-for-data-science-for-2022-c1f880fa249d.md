# 2022 年数据科学完整 Python 入门指南

> 原文：<https://towardsdatascience.com/complete-python-starter-guide-for-data-science-for-2022-c1f880fa249d?source=collection_archive---------11----------------------->

## 涵盖所有 Python 的基础知识和基本概念，通过代码示例帮助您了解数据科学

![](img/c82903dfcfb01187fd1be81532efac51.png)

米利安·耶西耶在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

Python 是现代最重要的编程语言之一。尽管这种语言是在近 30 年前开发的，但它仍在不断发展，仍然具有巨大的价值和更多的功能，特别是在数据科学和人工智能方面。

Python 3.10 的当前版本是从以前的 Python 2 时代发展而来的，这种编程语言及其社区的增长正处于历史最高水平。

由于这些技术的持续发展和进步，数据科学和人工智能获得了巨大的普及，人们非常好奇这些巨大的主题会飞多远，特别是使用 Python 作为它们的主要开发语言。

我们将经历一个新的时代，因为我们有更多的爱好者，他们不断地吞没这些现代概念，并为这些领域的进步做出巨大贡献。随着下一年的迅速临近，我们中的许多人都有了新的目标，去学习新的有趣的话题并取得更大的进步。

在本文中，我们的主要焦点是建立对数据科学有用的所有基本概念的基本理解，并初步了解我们如何利用 Python 在人工智能、机器学习和数据科学领域变得更加精通。

我们将关注开发人员在从事数据科学项目时最应该关注的特定主题，以获得最佳结果。如果您正在寻找关于即兴 Python 编程的更高级的作品，请查看下面的文章，以了解 Python 的一些最佳实践。

</how-to-write-code-effectively-in-python-105dc5f2d293>  

# 迭代语句:

在简要了解了使用 Python 进行面向对象编程的意义之后，让我们来探讨一下 Python 中迭代语句的概念。像 Java 和 C++这样的大多数编程语言通常使用相当多的迭代语句，例如 for 循环、while 循环、do-while 语句、switch case 和其他类似的迭代。

在 Python 中，我们大多只有效地利用*进行*循环或 *While* 循环。大多数计算都是用这两条迭代语句来执行的。在 Python 编程的帮助下，只要满足某个条件(即 True)，就可以运行这些迭代循环。因此，执行特定的代码块变得很容易，直到持续满足所需的目的。

无论是数据科学还是简单的 Python 编程，迭代语句都被认为是必须的。几乎每一个单独的项目都利用这些重复的循环来执行特定的任务。我以前文章中的大多数项目也使用了这些语句。下面的一个最好的例子来自我以前的一个博客，是关于创建一个语言亵渎测试器的。查看下面的代码，并访问文章后面的[获取更多信息。](https://medium.com/me/stats/post/d6502f9c224b)

```
sentence = "You are not only stupid , but also an idiot ."def censor(sentence = ""):
    new_sentence = ""

    for word in sentence.split():
        if word in Banned_List:
            new_sentence += '* '
        else:
            new_sentence += word + ' '

    return new_sentence
```

# 哎呀:

Python 是一种面向对象的编程语言，它是 Python 最本质的方面之一。然而，由于 Python 的其他惊人特性，这个特性有时会被忽略。因此，这个主题应该是我们开始使用 Python 进行数据科学研究的主要焦点。当处理 Python 的许多方面时，有时可能会忘记面向对象编程的重要性。

每一个用于机器学习、数据科学或任何内置于 Python 的深度学习框架的库都将主要由两个基本的主要组件组成，即对象和类。现实世界中的实体，如类、封装、多态和继承，在 Python 中也实现得相当好。因此，我们的目标是非常详细地理解所有的概念，我们将在下一篇文章中深入探讨这些概念。

下面是一些快速入门的代码。查看下面的文章中的[以获得关于这个代码块的更多信息。](/best-library-to-simplify-math-for-machine-learning-ed64cbe536ac)

```
class Derivative_Calculator:
    def power_rule(*args):
        deriv = sympy.diff(*args)
        return deriv
    def sum_rule(*args):
        derive = sympy.diff(*args)
        return derivdifferentiatie = Derivative_Calculator
differentiatie.power_rule(Derivative)
```

# 列表:

列表是可变的有序元素序列。可变意味着列表可以被修改或改变。列表用方括号“[ ]”括起来。列表是一种有序的数据结构，列表中的每个元素都被分配了一个特定的索引号，通过该索引号可以对其进行访问。列表中的每个项目或元素由逗号(，)分隔。

```
lst = ['one', 'two', 'three', 'four']
lst.append('five')
lst
```

## 输出:

```
['one', 'two', 'three', 'four', 'five']
```

append 函数是编程和数据科学领域中使用的最重要的命令之一。我们还可以在列表上执行和操作其他一些功能。要了解更多关于其他可用选项的信息，我强烈推荐通过下面提供的链接查看使用 Python 编程掌握列表的详细版本。

</mastering-python-lists-for-programming-5423b011d937>  

# 词典:

字典允许用户相应地访问键和值。假设你必须存储一个人的一些数据，那么你会考虑使用字典，比如存储一个联系人的名字和他们的号码。字典还可以存储与特定。学校中学生的特定姓名可以存储许多科目的分数。字典是 Python 中的数据结构，被定义为无序的数据集合。下面是一些样例代码和输出，用于开始使用字典。

```
# Return a list of tuples of the dictionary items in the (key, value) form
my_dict = {1: 'A', 2: 'B', 3: 'C'}
print(my_dict.items())# Return a new view of the dictionary keys
my_dict = {1: 'A', 2: 'B', 3: 'C'}
print(my_dict.keys())# Return a new view of the dictionary values
my_dict = {1: 'A', 2: 'B', 3: 'C'}
print(my_dict.values())
```

## 输出:

```
dict_items([(1, 'A'), (2, 'B'), (3, 'C')])
dict_keys([1, 2, 3])
dict_values(['A', 'B', 'C'])
```

上面的起始代码应该允许用户对如何使用字典值和关键元素的一些基本概念有一个简单的理解。如果你期待一个关于字典和集合的扩展指南，我推荐你看看下面的文章，以获得更多关于这些主题的知识。

</mastering-dictionaries-and-sets-in-python-6e30b0e2011f>  

# 功能:

函数允许用户在**def**function name():命令下快速操作代码块内的可重复任务。这一概念在编程中非常有用，尤其是在数据科学中，您需要对大量数据重复特定的操作。利用函数来实现这一目标将减少开发人员需要执行的大量计算。

Python 还允许它的用户直接访问它的一些匿名(或高级)函数选项，这将有助于更快更高效地开发您的项目。我已经在另一篇文章中非常详细地介绍了以下主题，如果您有兴趣进一步探讨这个主题，我建议您查看一下。下面提供了相同内容的链接。

</understanding-advanced-functions-in-python-with-codes-and-examples-2e68bbb04094>  

# 探索用于数据科学的 Python 库:

Python 最好的特性是这种编程语言有大量可用的库。对于您想要执行的几乎每一种类型的任务或想要进行的任何类型的项目，Python 提供了一个库，它将极大地简化或减少工作。

在 Python 提供的一些最好的数据科学库的帮助下，您可以完成您想要完成的任何类型的任务。让我们探索一些数据科学初学者必须知道的库。

## 1.熊猫:

对于使用数据科学，主要要求之一是分析数据。Python 为其用户提供的最好的库之一是 Pandas 库，通过它，您可以访问互联网上以结构化格式提供的大多数内容。它为开发人员提供了访问各种格式的大量文件的选项，如文本、HTML、CSV、XML、latex 等等。下面是一个可以用来访问 CSV 格式类型数据的示例。

```
data = pd.read_csv("fer2013.csv")
data.head()
```

![](img/05aaaf1c593614d8b514e32fd4b9a8ff.png)

作者图片

为了更多地了解 Pandas 并征服这个库背后的分析工具，我建议查看我以前的一篇文章，该文章介绍了每个数据科学家的武器库中必须包含的 14 个最重要的 Pandas 操作。以下是相同的以下链接。

</14-pandas-operations-that-every-data-scientist-must-know-cc326dc4e6ee>  

## 2.Matplotlib:

![](img/bc7c19acb91718d3ae3e4521f717f3e5.png)

作者图片

一旦您完成了数据分析，下一个重要的步骤就是相应地将它们可视化。对于数据的可视化，matplotlib 和 seaborn 是 Python 中可用的最佳选项之一。你可以用这个奇妙的库和简单的代码来可视化几乎任何基本的实体。它支持像 NumPy 这样的数字扩展，您可以将这些数字扩展组合在一起以可视化大多数数据元素。

上面的图像展示了一个在 matplotlib 库的帮助下构建的条形图。我们可以使用 matplotlib 执行更多的可视化、图表和其他统计可视化。要了解有关数据科学项目的不同类型的可视化的更多信息，请查看下面提供的链接。

</8-best-visualizations-to-consider-for-your-data-science-projects-b9ace21564a>  

## 3.NumPy:

简言之，数值 Python 或 NumPy 是 Python 中计算数学问题的最佳选择之一。您可以利用 numpy 数组的概念来简化数据科学领域中涉及的复杂数学。它有助于您处理大型多维数组和矩阵，以及高效构建您的数据科学项目。

如果没有 numpy 的适当效用，解决大多数复杂的数学问题和机器学习项目几乎是不可能的。因此，非常详细地理解这个概念是至关重要的。建议读者阅读下面的文章，了解每个数据科学家都必须了解的十五个 numpy 功能。

</15-numpy-functionalities-that-every-data-scientist-must-know-f6d69072df68>  

## 4.sci kit-学习:

Scikit-learn 是最好的库之一，使用它可以实现所有基本的机器学习算法，如分类、回归、聚类、预处理(如下面的代码所示)、模型选择、维数减少等等。library toolkit 利用简单但高效的工具来分析和计算数据。它不仅像前面提到的其他三个模块一样安装简单，而且它是建立在 matplotlib、numpy 和 scipy 等关键包之上的。对于初学者来说，这个开源工具是更有效地实现机器学习项目的必学工具。

```
from sklearn.model_selection import train_test_splitX_train, X_test, y_train, y_test = train_test_split(questions, response, test_size=0.20)
```

## 5.NLTK:

自然语言工具包是处理人类语言数据的最佳库之一。一开始，大多数机器学习和数据科学项目将处理大量的自然语言处理任务。对于大多数与自然语言处理相关的问题，清理数据是数据准备阶段所需的最基本的步骤之一。因此，如果您是该领域的初学者，学习和掌握这个库是非常重要的。

```
import nltksentence = "Hello! Good morning."
tokens = nltk.word_tokenize(sentence)
```

如果你正在研究图像处理方面的东西，那么计算机视觉库 Open-CV 是非常值得推荐的。从下面的链接查看以下库的完整指南。

</opencv-complete-beginners-guide-to-master-the-basics-of-computer-vision-with-code-4a1cd0c687f9>  

# 结论:

![](img/840fee93cc0e4fd2808e39c5fa621a36.png)

[自由股票](https://unsplash.com/@freestocks?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上的照片

*“代码就像幽默。当你不得不解释它时，它是糟糕的。”—* **科里屋**

Python 是一种革命性的编程语言，由于其简单、易学、多功能和许多其他令人难以置信的特性，几十年来它一直保持着相关性。随着过去几年人工智能和数据科学的出现，Python 为自己创造了巨大的声誉，成为这些领域的主导语言之一，也是每个人最终都必须掌握的东西。

在本文中，我们介绍了 Python 入门的大部分基本概念，以便更加精通数据科学。我们集中讨论了 Python 中的大多数基本主题，这些主题在数据科学的大多数领域都有巨大的用途，并且将有助于大多数项目的成功完成。如果您能够掌握本文中提到的所有方法，您将能够轻松完成大多数基础数据科学项目。

如果你想在我的文章发表后第一时间得到通知，请点击下面的[链接](https://bharath-k1297.medium.com/membership)订阅邮件推荐。如果你希望支持其他作者和我，请订阅下面的链接。

<https://bharath-k1297.medium.com/membership>  

如果你对这篇文章中提到的各点有任何疑问，请在下面的评论中告诉我。我会尽快给你回复。

看看我的一些与本文主题相关的文章，你可能也会喜欢阅读！

</generating-qr-codes-with-python-in-less-than-10-lines-f6e398df6c8b>  </5-best-python-projects-with-codes-that-you-can-complete-within-an-hour-fb112e15ef44>  </17-must-know-code-blocks-for-every-data-scientist-c39a607a844d>  

谢谢你们坚持到最后。我希望你们都喜欢这篇文章。祝大家有美好的一天！