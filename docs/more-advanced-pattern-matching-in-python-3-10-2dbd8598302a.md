# Python 3.10 中更多的模式匹配

> 原文：<https://towardsdatascience.com/more-advanced-pattern-matching-in-python-3-10-2dbd8598302a?source=collection_archive---------19----------------------->

## 比‘开关’厉害多了。我在哈利·波特和一只死鹦鹉的帮助下解释

![](img/0dc2998a80209d3429bd088c4e8a0f26.png)

[Ashkan Forouzani](https://unsplash.com/@ashkfor121?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上的照片

Python 终于有了一个 *switch* 语句。万岁！

但它不是你在 C 或 Java 中会发现的常见或普通的 switch 语句——当然不是，这是 Python。Python 3.10 实现了结构化模式匹配，可以像一个 *switch* 语句一样简单，但也可以更复杂。

我在这里介绍了结构模式匹配的基础知识[——本文深入探讨了这个主题，着眼于捕获匹配的模式(获取可能不止一个值的匹配值)并向模式添加条件。](/pattern-matching-in-python-3-10-6124ff2079f0)

但是，首先，让我们复习一下基础知识。结构匹配模式是用关键字`match`实现的。这包含一个块，该块包含每个匹配的一列`case`语句。您可以在下面的代码中看到语法——它非常简洁明了。

下面的代码运行一个循环，给变量`quote`赋值 1，2，3，4 和 5。我们定义了四种情况，第一种匹配数字 1 或 4(`|` 字符是 or 操作符)；第二个和第三个分别匹配 2 和 3，最后一个是匹配任何内容的通配符。大小写是按顺序处理的，所以只有前面的任何一个不匹配时，通配符才匹配。此外，与 C 不同，这里没有“中断”语句——在每种情况下的代码块被执行后，控制被转移到`match`的末尾。

```
for quote in [1,2,3,4,5]:
    match quote:
        case 1|4:
            print("He's gone to meet his maker.")
        case 2:
            print("He should be pushing up the daisies.")
        case 3:
            print("He's shuffled off his mortal coil.")
        case _:
            print("This is a dead parrot!")
```

这是运行代码的结果:

```
He's gone to meet his maker.
He should be pushing up the daisies.
He's shuffled off his mortal coil.
He's gone to meet his maker.
This is a dead parrot!
```

## 捕获匹配的模式

有时，当我们匹配 OR 模式时，我们希望能够使用匹配的值。

让我们想象一下，某个偶然熟悉 Python 的书呆子，决定表示一个类似于下面这样的 Monty Python 脚本:

```
script = [["The Dead Parrot Sketch"],
          ["Cast","Customer","Shop Owner"],
          ["Customer","He's gone to meet his maker."],
          ["Owner","Nah, he's resting."],
          ["Customer","He should be pushing up the daisies."],
          ["Owner", "He's pining for the fjords"],  
          ["Customer", "He's shuffled off his mortal coil."],
          ["Customer", "This is a dead parrot!"]]
```

这是一个列表列表。第一个包含一个字符串—标题。第二个是 cast，它是字符串“Cast ”,后面跟有许多其他字符串，代表*剧中人*。其余的是两个字符串的列表，演职人员和他或她应该说的台词。

这里没有数据设计奖。

我们如何解读这个结构？标题很容易匹配，因为它是唯一的单个字符串。通过指定一个以字符串“cast”开头，后跟许多其他元素的列表来匹配转换(`*actors`就是这样做的)。脚本的行由最后一个案例匹配:它匹配两个元素，第一个是“Customer”或“Owner ”,第二个是`line`,它将捕获脚本的行。问题是我们想在下面的代码中使用“客户”或“所有者”的值，所以我们使用关键字`as`将它绑定到变量`person`。

```
for line in script:
    match line:
        case [title]:
            print(title)
        case ["Cast",*actors]:
            print("Cast:")
            for a in actors: print(a)
            print("---")
        case [("Customer"|"Owner") as person, line] :
            print(f"{person}: {line}")
```

运行代码，我们得到这个:

```
The Dead Parrot Sketch
Cast:
Customer
Shop Owner
---
Customer: He's gone to meet his maker.
Owner: Nah, he's resting.
Customer: He should be pushing up the daisies.
Owner: He's pining for the fjords
Customer: He's shuffled off his mortal coil.
Customer: This is a dead parrot!
```

## 条件格

在这种情况下，我们希望在满足某些条件的情况下创建匹配。看看下面的代码:我们有一个哈利波特角色列表和一个银河系漫游指南中的角色列表。

我们要求输入一个名字，然后尝试匹配这个名字。

```
harrypotter = ["Harry","Hermione","Ron"]hhgtg = ["Arthur","Ford","Zaphod","Trillian"]name = input("Enter a name: ")match name:
    case n if n in hhgtg:
        print(f"{n} is a character in Hitchhiker's guide to the Galaxy.")
    case n if n in harrypotter:
        print(f"{n} is a character in the Harry Potter stories.")
    case _:
        print(f"{n} is an unknown character.")
```

使用这种结构:

```
case <match> if <match> in <list> 
```

我们只匹配一个大小写，如果它满足我们想要匹配的字符串在列表中的条件。我认为代码是不言自明的，所以这是程序运行几次的结果。

```
Enter a name: Harry
Harry is a character in the Harry Potter stories.Enter a name: Zaphod
Zaphod is a character in Hitchhiker's guide to the Galaxy.Enter a name: Elizabeth Bennet
Elizabeth Bennet is an unknown character.
```

最后一种情况当然是通配符，它将匹配以前没有处理过的任何内容。

要了解更多，请访问 Python.org 网站并阅读 [PEP 636](https://www.python.org/dev/peps/pep-0636/) ，这是一篇关于结构模式匹配的教程。

要了解更多关于 Python 3.10 中的新特性，你可以看看其他媒体文章，比如 James Briggs 的 Python 3.10 中的新特性。

正如我在我的[上一篇文章](/pattern-matching-in-python-3-10-6124ff2079f0)中提到的，你还不应该认真使用 Python 3.10，因为它仍然是一个测试版，但是如果你想试用它，可以从 Python 网站上下载。