# 厌倦了关于代码格式的无意义讨论？有一个简单的解决方案

> 原文：<https://towardsdatascience.com/tired-of-pointless-discussions-on-code-formatting-a-simple-solution-exists-af11ea442bdc?source=collection_archive---------25----------------------->

## 我喜欢我的咖啡，就像我喜欢我的 Python 代码格式化程序——黑色一样。

![](img/4d5a7d8e20d54bc3d7f08ed835c862f1.png)

马特·霍夫曼在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上的照片

# 您的旅程概述

1.  [设置舞台](#44b2)
2.  你信任的朋友 PEP8 呢？
3.  [真正重要的是什么](#8bdd)
4.  [黑色——不妥协的代码格式化程序](#7466)
5.  [看看使用黑色有多简单！](#10f7)
6.  [黑色是所有代码质量问题的解决方案吗？](#ecbe)
7.  [包装](#ad98)

# 搭建舞台

我们都经历过。当你的大学提出代码格式化的时候。小心！你将要进入一个耗费时间的讨论。

事实证明，许多 Python 开发人员对代码格式有强烈的看法。我也是。为了解决代码格式的问题，我想给你以下三个问题的答案:

*   **pep 8 不是规定了 Python 中的代码格式吗？**
*   **代码格式化最重要的是什么？**
*   如何在项目中快速实现代码格式化？

以上问题将带我们来到[Black——不妥协的代码格式化程序](https://black.readthedocs.io/en/stable/index.html)。我将向你展示如何在项目中使用黑色可以节省大量的时间。因此，你可以把时间花在实际编写令人敬畏的 Python 代码上😀

如果你不相信我的话，那么 [SQLAcademy](https://www.sqlalchemy.org/) 的作者对黑色有如下看法:

> “在我的整个编程生涯中，我想不出有哪一个工具的引入给了我更大的生产力提升。我现在可以用大约 1%的击键次数进行重构，而这在以前我们无法让代码自行格式化时是不可能的。”— **迈克·拜尔**

# 你信任的朋友 PEP8 呢？

每个 Python 开发者都应该或多或少地熟记[pep 8](https://www.python.org/dev/peps/pep-0008/)**——它是 Python 的官方风格指南。**

例如，PEP8 喜欢在变量赋值中等号`=`的两边都有空格。所以不要写以下任何内容:

```
# Ahh! My eyes!
my_variable=5
my_variable =5
my_variable= 5
```

相反，你应该写:

```
my_variable = 5
```

简单吧？PEP8 对对称情有独钟。

然而，PEP8 中的指导方针不足以解决所有的决策。作为一个例子，考虑口袋妖怪的列表:

```
pokemon = ["Bulbasaur", "Charmander", "Squirtle", "Pikachu"]
```

上面我写单子`pokemon`的方式是 PEP8-approved。然而，以下两种方式也是如此:

```
# Another acceptible way
pokemon = [
    "Bulbasaur", "Charmander", 
    "Squirtle", "Pikachu"
]# Yet another one
pokemon = [
    "Bulbasaur", 
    "Charmander", 
    "Squirtle", 
    "Pikachu"
]
```

小心！主观性进入了聊天。

事实是，PEP8 允许一些灵活性。正是这种灵活性引发了如此多耗费时间的讨论。

想象一下，向其他开发人员展示上面的代码并询问:

> 上面的`pokemon`列表中哪一个是最好的？哪一个最 Pythonic？

下图代表了你接下来 30 分钟的生活😅

![](img/6edcf84d3d0a468ba75c84192a3dbd06.png)

约翰·施诺布里奇在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上的照片

# 真正重要的是

> 归根结底，格式化代码最重要的是**一致性**。*让代码看起来在整个代码库中都是一样的，会让代码更容易阅读。*

前面的`pokemon`列表的精确格式选择**并不那么重要**。我提出的所有三个 PEP8 批准的选项都非常可读。重要的是项目**中的每个人都坚持相同的格式！**

与其在这个选择上引发一场办公室大战，为什么不让第三方来决定呢？这就是黑色进入画面的地方。

# 黑色——不妥协的代码格式化程序

![](img/a55b7e7c1eaadd497bf33332652e3e64.png)

黑色的官方标志——不妥协的代码格式器

> *Black 是不折不扣的 Python 代码格式化程序。通过使用它，您同意放弃对手写格式的细节的控制。你将节省时间和精力用于更重要的事情。”—黑色的文档*

*[黑色](https://github.com/psf/black)是一个 Python 模块，眨眼之间就能格式化你的代码。不再有无意义的格式化讨论。黑色让你专注于重要的事情。达斯丁·菲利普斯说得好:*

> *"布莱克固执己见，所以你不必如此。"—达斯丁·菲利普斯*

*如果你想在承诺之前测试一下黑色，那么你可以在[黑色游乐场](https://black.vercel.app/)玩一玩。你也可以查看[黑色代码风格](https://black.readthedocs.io/en/stable/the_black_code_style/current_style.html)来查看黑色代码将在你的代码中强制执行的代码风格。*

*要开始使用 Black，需要先下载。注意，Black 的默认版本需要 Python 3.6.2 或更高版本。在终端中运行以下命令来安装 Black:*

```
*pip install black*
```

*就是这样！您现在已经安装了 Black👊*

> *有趣的事实:黑人的座右铭是“任何你喜欢的颜色”这是对亨利·福特名言的发挥:*
> 
> *任何顾客都可以把车漆成他想要的任何颜色，只要是黑色的。”—亨利·福特*
> 
> *Black 的座右铭暗示了这样一个事实，即 Black 在其代码格式中不具有兼容性。这也解释了(至少对我来说)为什么 Black 的 logo 看起来像车标。*

# *看看使用黑色有多简单！*

*![](img/a0ee9c4f5d83d0b9758742b91b71c492.png)*

*由 [Kasya Shahovskaya](https://unsplash.com/@kasya?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片*

*你希望我开始漫谈你需要设置的各种配置吗？再想想！黑色附带电池😃*

## *简单的例子*

*以下代码片段是错误代码格式的一个示例:*

*呸！您应该让 Black 来完成这项工作，而不是手动修改格式。如果您在文件`bad_code_formatting.py`所在的文件夹中，那么您可以运行命令:*

```
*python -m black bad_code_formatting.py*
```

*通过这样做，布莱克应该给你传达这样的信息:*

```
***All done! ✨ 🍰 ✨
1 file left unchanged.***
```

*可爱。现在`bad_code_formatting.py`中的代码如下所示:*

*文件名`bad_code_formatting.py`不再合适:代码已经被自动转换成 PEP8 格式。PEP8 是灵活的，Black 选择了如何格式化代码。你不需要做任何决定！*

## *修改线长度*

***默认行长**为 88 个黑色字符。如果你有一个雇主为你写的行数付钱，那么你总是可以运行这个命令:*

```
*python -m black --line-length=70 bad_code_formatting.py*
```

*文件`bad_code_formatting.py`现在看起来如下:*

*除此之外，没有什么理由去改变 Black 给出的默认行长度。*

## *在文件夹上使用黑色*

*假设你在一个名为`multiple_python_files`的文件夹中有很多 Python 文件。您不需要对每个文件单独运行 Black。相反，您可以使用以下命令对整个文件夹运行 Black:*

```
*python -m black multiple_python_files*
```

*Black 现在已经格式化了文件夹中的所有 Python 文件😍*

# *黑色是所有代码质量问题的解决方案吗？*

*我希望…但是不行。*

*认为 Black 将修复所有代码质量问题的想法是错误的。例如，黑色不会给你的变量**起描述性的名字**。这也不会让**写得很差的文档变得更好。布莱克不会将**类型的提示**引入你的代码库。也不会实施有效的设计原则。***

*正确格式化糟糕的代码就像给汽车残骸喷漆一样！*

> *当考虑代码质量时，黑色应该只是你工具箱中的一个工具。*

# *包扎*

*在这篇博文中，我宣传 Black 是一种方便的代码格式化程序。黑色的伟大之处在于它不妥协的本性。不幸的是，没有更多的话要说了:黑色既简单又有用💜*

*另一种提高代码质量的方法是编写**类型提示**。类型提示是一个伟大的 Python 特性，许多项目都可以从中受益。如果你是输入提示的新手，那么我已经写了一篇关于这个主题的博文:*

*</modernize-your-sinful-python-code-with-beautiful-type-hints-4e72e98f6bf1>  

如果你对数据科学、编程或任何介于两者之间的东西感兴趣，那么请随意在 LinkedIn 上加我，并向✋问好*