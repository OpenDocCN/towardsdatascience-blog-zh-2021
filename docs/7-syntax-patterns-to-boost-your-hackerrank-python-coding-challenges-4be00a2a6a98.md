# 要提升的 7 个语法模式🚀你的黑客等级🐍Python 编码挑战

> 原文：<https://towardsdatascience.com/7-syntax-patterns-to-boost-your-hackerrank-python-coding-challenges-4be00a2a6a98?source=collection_archive---------20----------------------->

## 这也帮助我在 3 周内清除了所有的 Hackerrank 挑战

![](img/9f0904b56153d482d4dcaaca09212e27.png)

照片由 [Fotis Fotopoulos](https://unsplash.com/@ffstop?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

# 动机😎

D 迎接编码挑战可能是快速学习编程语言实用方面的最好方法。与做实际项目相比，它更侧重于利用特定的语言概念和语法来提高你解决问题的技能。这是**深度**(项目)对**广度**(编码挑战)。

当你也阅读其他人的解决方案和评论时，你会从中受益更多。你会发现不同的思维方式，不同的语言技巧被用来解决同一个问题，并通过多次练习快速掌握它们。我认为每个想要使用特定编程语言作为主要开发工具的人都应该至少清除一次 Hackerrank 上的所有编码挑战。

我以前很难读懂别人用 Python 写的代码，但在我用 Python 清除了 HackerRank 上的挑战后，我就没有这个问题了。

在这篇文章中，我将记录我一路走来所学到的东西，希望它能让你的旅程更加顺利。我们开始吧！

# 获得正确的问题输入

信不信由你，像这样一件简单的事情会大大提高你的生活质量。所有 [HackerRank](https://medium.com/u/d3ac51b2731c?source=post_page-----4be00a2a6a98--------------------------------) 挑战都使用`stdin`作为输入，并期望你输出正确答案给`stdout`。当您到达 Hackerrank Python 之旅的终点时，以下片段会感觉相对琐碎。但是当你刚刚起步的时候，抓住这些会让最初的几步变得容易得多。另外，`**map**` 功能和列表理解也是很好的练习:
1。将一行`**n**`数字放入一个列表: `list(map(int, input().strip().split()))`
2。`n, m = input(), input()`得到两个整数:
3。将一行字符串放入列表:`list(input().strip().split())`
4 .获取一个将`**n**` 行作为 int: `a = np.array(input().strip().split(), int) for _ in range(n) ]`的 NumPy 数组

# (嵌套)列表理解

![](img/2775108ea09907ba2eb3872cb4cbf1ae.png)

里卡多·戈麦斯·安吉尔在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

嗯，列表理解并不是什么新鲜事。我们都喜欢它的简洁和表现力。你会在挑战中大量使用**。列表理解更具挑战性的部分是钉钉'**嵌套'**部分，尤其是再加上`**if**` 语句。最简单的方法是像写`**for-loop**`一样写，然后转化为列表理解。考虑以下三层深度嵌套 for 循环的条件:**

**![](img/c0fba43191abb076614d72b7aac403fa.png)**

**作者使用 [manim](https://docs.manim.community/en/stable/index.html) 制作的动画**

**如何把它变成一个列表理解的一句话？这比你想象的要容易。只需将每一行一个接一个地向上拖动成一行，然后将最里面的块`**lst.append([i,j,k])**`放在最左边的位置(该行的开始)，就像这样:
`[**[i,j,k]** for i in range(x) for j in range(y) for k in range(z) if i > j and j > k]`
正如 Python PEP 202 所述:**

> **建议允许使用 for 和 if 子句有条件地构造列表文字。**它们会以同样的方式嵌套循环和 if 语句。****

**如果你读到这里，那么恭喜你！您已经掌握了嵌套列表理解的艺术！你也可以在这里找到一篇好文章[。](https://spapas.github.io/2016/04/27/python-nested-list-comprehensions/)**

# **使用'`getattr'` '来访问类函数**

**![](img/ad65b2c3748095caa5b3dc102807fcb7.png)**

**加里·巴特菲尔德在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片**

**ackerrank Python 挑战赛有时是为了教你如何使用某些数据结构，比如集合和集合。这种类型的问题通常会有数据结构的多个函数的输入，并让你一个一个地尝试。对于这类问题，使用`**if/elif/else**`会非常麻烦。在这里，我建议一种更好的方法，使用`**getattr**`来访问特定于数据结构的函数。这样，你就不需要写多行`**if/elif/else**`，只需要一个`**getattr**`单行程序。我们来看一个[的例子](https://www.hackerrank.com/challenges/py-set-mutations/) :
这个问题需要你得到**集合**命令`**update**`、`**intersection_update**`、`**difference_update**`等。，然后使用它来操作所提供的集合，以获得所需的结果。你当然可以用`**if/elif**`做到这一点，但是请看下面的片段:**

```
# A is the set provided
(_, A) = (int(input()),set(map(int, input().split()))) # B is the number of commands to test
B = int(input())

for _ in range(B):
  # get the command and parameter  
  (command, newSet) = (input().split()[0], set(map(int, input().split()))) # ‘getattr’ one-liner, yay 🎆  
  getattr(A, command)(newSet) 
 print (sum(A))
```

**它的优点是从输入中读取的命令可以直接用于访问 set 命令，因此是一行程序。这个技巧可以用在多种挑战上，节省你相当多的时间，同时让你对函数式编程有一点了解。你也可以像这样使用`**eval**`:`eval(“A.”+op+”(B)”)`，但据说这不太稳定，通常推荐使用`**getattr**` 而不是`**eval**`。**

# **使用 all()和 any()进行布尔列表聚合**

**T he `**and()**`和`**any()**`内置在 Python 中并不新鲜，但是意识到使用它是一个需要养成的好习惯。它允许你更抽象地思考，让你的代码更简洁。作为良好的布尔迭代器聚合器，它们使您能够在一行代码中检查一长串的`**and**` 或`**or**` 条件。此外，如果你碰巧也从事数据科学/机器学习，让自己适应聚合从长远来看将有利于你的旅程。例如，在机器学习中，你会进行大量的数组计算。你可以用类似于`for-loop`的东西来做这件事，但是把数据聚集成一个矩阵，尽可能地集中处理它们，将会大大减少你的计算时间。你越早进入这种思维方式，计算(和时间)就越多⏲还有钱💸连同它)你将能够保存。**

# **排序()和已排序()的“关键字”。**

**![](img/dcf5cdd91f435d6550849ef82f2df570.png)**

**Silas k hler 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片**

**U 唱`**sort()**`和`**sorted()**`不是很有挑战性。然而，没有太多人意识到它的全部潜力。这🔑**键**是用`**key**` 论证。(双关意！).当使用`**sort()**`或`**sorted()**`时，您可以添加一个参数`**key**` ，该参数期望一个函数被传递给它，然后该函数将被用于为排序生成值。这使得排序更加灵活和强大。你可以使用它对你的物品做各种事情。几个想法:
1。`key = len`；使用字符串、列表等的长度。，来做排序。
2。`key = str.lower`；将字符串全部转换成小写，然后排序。这使得排序不区分大小写。
3。`key = lambda x: x[::-1]`；恢复字符串的顺序，然后排序。
4。`key = lambda x: int(x.split()[K])`；挑选出 iterable 的`**k**`列，因为要排序的值可能会在类似表格的数据处理中使用。
5。`key = lambda x: (item1, item2, item3, …, item_n)`；这里有`**item1**`、`**item2**`等。都是 x 的变体，所以可以做嵌套排序。如你所见，这里的潜力是无限的。在这里使用`**key**` 论点的唯一限制是我们自己的创造力。如果想了解更多，可以查看这篇[文章](https://realpython.com/python-sort/)。**

# ***arg 和**kwargs 无处不在！**

**我们通常在函数参数中使用`***args**`和`****kwargs**`。从 Python 3.5 版本开始，使用 [PEP 448](https://www.python.org/dev/peps/pep-0448/) 作为解包操作符，它变得更加强大。它可以将任何迭代器解包到它的所有项中。单星号操作符`*****`可以用在 Python 提供的任何 iterable 上，而双星号操作符`******`只能用在字典上。
在 Hackerrank challenges 中，它通常与`**print**` 函数一起使用来解包要打印的 iterable。例如，如果您有一个列表:**

**`lst = [‘May’, ‘the’, ‘force’, ‘be’, ‘with’, ‘you.’]` 
把这个打印成句子的一种方法是:`print(‘ ‘.join(lst))`，或者你可以只做`**print(*lst)**`。
另一个好玩的用法是解包一个字符串，比如说`str = “Stay a while and listen.”`，可以做`**list(str)**`，也可以做`***str**`得到`[‘S’, ‘t’, ‘a’, ‘y’, ‘ ‘, ‘a’, ‘w’, ‘h’, ‘i’, ‘l’, ‘e’, ‘ ‘, ‘a’, ’n’, ‘d’, ‘ ‘, ‘l’, ‘i’, ‘s’, ‘t’, ‘e’, ’n’, ‘.’]`。
它还有很多其他的用途，意识到这一点会让你更快地通过挑战。**

# **正则表达式—前视/后视断言**

**![](img/12eb83de08f7a03a00f105451baa2db5.png)**

**照片由[安德鲁·尼尔](https://unsplash.com/@andrewtneel?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄**

**在 Hackerrank 中有很多挑战。三分之二的 Python“硬”挑战是正则表达式挑战。Hackerrank 中复杂正则表达式问题的关键？我提供两种配料:**

## **用非捕获组匹配处理非重叠匹配**

**有时，挑战要求您匹配重叠的模式，比如说，如果要求您在字符串“3333”中找到所有的“33”。你应该找到其中的 3 个，但如果你只用`r'33'`，你只会找到两个，因为前两个‘33’会被‘消耗/捕获’。这就是非捕捉组可以大放异彩的地方。使用`(?:<regex>)`，您可以轻松地捕获一个模式组，而无需“消耗”它，从而允许捕获所有不重叠的匹配。对于我们的“3333”案例，我们可以使用`r’(?:33)’`来查找所有三个匹配。另外两个有用的是`Lookahead`和`Lookbehind`断言。用法挺像的。你可以点击查看文章[了解更多详情。](https://realpython.com/regex-python/#lookahead-and-lookbehind-assertions)**

## **使用\1 查找重复的匹配组**

**这是一个简单的方法，但在 Python 挑战中被多次使用。考虑下面的例子:
`m = re.search(r’(\d)\1{3}’, ‘4444–5555’)`
这将匹配‘4444’和‘5555’。`r’(\w+),\1,\1'`将匹配“走，走，走”或“不，不，不”。**

**Regex 本身可能非常复杂，我发现将它们分成几个步骤并逐个解决它们很有帮助。**

# **结论**

**以上片段绝非包罗万象。然而，我发现它们有助于加快速度。此外，其中一些是需要养成的好习惯，是需要建立的良好心态，从长远来看，这将有利于您的编码之旅。祝你好运，编码快乐！**

**我希望你觉得这篇文章读起来很有趣，并从中学习到一些东西。如果你想更多地了解我对数据科学的思考、实践和写作，可以考虑报名成为 Medium 会员。每月 5 美元，你可以无限制地阅读媒体上的故事。如果你注册使用我的链接，我会赚一小笔佣金。**

**<https://lymenlee.medium.com/membership> **