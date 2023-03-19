# 这两个工具将提升您的 Python 脚本。

> 原文：<https://towardsdatascience.com/two-tools-that-will-boost-your-python-scripts-82adb3d8949e?source=collection_archive---------29----------------------->

## if __name__ == 'main '和 argparse (+ bonus)

你将在这里读到的这两个工具将会成倍地提高你的 python 脚本的可用性。

![](img/45abd2dc9e4cb5c8595bc50884bfb849.png)

由[马克塔·马塞洛娃](https://unsplash.com/@ketdee?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

如果你读了一堆代码，或者你正在开发一个大项目，你需要写不止一个脚本，你需要知道 if __name__== 'main '是什么。一种只编写一次函数并在所有脚本中使用它的方法。

如果你想做一个程序，那么任何人都可以执行 argparse 库，它会帮你高效地完成。向代码中引入帮助和不同参数的方法。

**if __name__ == 'main':**

__name__ 变量是怎么回事？当 python 解释器读取源文件时，它首先定义几个特殊变量。在这种情况下，__name__ 就是其中之一。

主要的呢？你会知道你的代码可能有不同的部分。你定义的函数是其中之一，另一个是主要部分。通常，main 是您使用函数的地方，以使您的代码完成设计的目的。正如你所知道的，它没有义务使用函数，但是如果你需要多次做一件具体的事情，最好把它封装在一个函数中，并有一个不言自明的名字。这是一个很好的方法来组织你的代码结构，所有的功能都在主要部分之上。现在你会明白为什么了。对于一些陌生人来说，这将使你的工作更容易，可读性更强。这总是一件好事。谁知道在编写代码后的几个月内，这个陌生人会不会是你。

好了，理解了 __name__ 变量和' main '变量之后，让我们看看当我们把它们放在一起时会发生什么。最好的方法是通过一个例子。

想象一下，你有一个想要做任何事情的脚本。您正在构建另一个脚本，其中需要旧脚本中创建的一些函数。你能做什么？你可以利用古老的复制粘贴习惯。但是这会在你的代码中增加更多的行，而且不清楚新的脚本会做什么。太多事情了。如果能够像我们通常使用的库(numpy、pandas、seaborn 等)一样导入旧脚本中创建的函数，那将非常有用。).这是你的解决方案:

```
if __name__ == 'main':
```

您将这一行代码放在旧脚本的主要部分之上，所有后续代码都适当缩进，就像普通 if 一样。就是这样。您已经准备好了导入函数的代码。这丝毫不会改变脚本之前的行为。事实上，如果您正常执行它，它会正常工作。

但是现在，如果您决定使用您已经创建的函数，您只需导入旧的脚本，因为它是一个库。假设旧脚本的名称是 old_script.py，您只需放入新脚本:

```
from old_script import this_function, that_function
```

在新脚本中有您的旧函数(并且只有这些函数)。

**argparse**

![](img/5bb3b193938948c7daeb4b574e2931c3.png)

由[克里斯托弗·高尔](https://unsplash.com/@cgower?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

通常，你的代码会被其他人执行，他们不会打开你的代码。所以你需要给他们一些信息，或者如果你的脚本需要一些具体的输入，你必须为用户找到一个合适的方式。例如，如果你在脚本中进行声音分析，你需要输入一个声音。

传统的方法是输入脚本，搜索处理过的输入的加载位置，并针对您想要分析的声音对其进行修改。这，有时候并不是一件容易的事情。尤其是当脚本很长或者读者在理解代码的作用时。因此，当我们希望我们的程序被大量的人使用，或者可能被非技术人员使用时，argparse 库将会给我们很大的帮助。它会给他们一个代码描述，它会告诉用户为了让你的代码运行，他必须输入什么。

Argparse 是一个 Python 库，它将您的脚本与您的终端进行通信。它允许编码者在代码中放置某种标志(及其相应的解释),以便从终端执行它们。

让我们来看看它是如何工作的。

```
import argparseparser = argparse.ArgumentParser(description='''The purpose of this script is to with two given numbers save the result of its multiplication''', formatter_class=argparse.RawTextHelpFormatter)
```

首先，我们有我们的脚本描述。正如我们在上面看到的，这个脚本(为了简单起见)只是一个乘数，你需要在这里引入两个术语。formatter_class 属性仅适用于将它打印到终端的方式。我们马上就能看到。

```
parser.add_argument('-ft', '--first_term', default=1, type=int, help='''First term to multiply''')
```

这个增加的论点更有趣。我们看到两个可能的标志('-'和'-')，这与 Linux 终端的工作方式完全相同。一个破折号是两个破折号的缩写形式。我们还有将要介绍的数据类型。一个默认值，以防用户没有引入。和一个说明该参数作用的帮助属性。

```
parser.add_argument('-st', '--second_term', default=1, type=int, help='''Second term to multiply''')
```

第二个论点并不神秘。它的工作原理和第一个完全一样。

```
parser.add_argument('-od', '--output_directory', default='/home/toni_domenech/Python/Datasets/multiplication/', type=str, help='Output folder for the .txt')
```

这个包含了我们想要保存乘法结果的地方。请注意，该类型是一个字符串，因为我们在这里给出了一个路径。在这种情况下，默认输出是我的电脑的目录。所以如果试图运行我的代码的人，在这里没有引入任何东西，代码就不能正常运行。

最后，让我们看看如何在我们的代码中使用这些参数。

```
args = parser.parse_args()args.first_term * args.second_term
```

这里我们有使用它们的方法。我们将使用国旗的名字。仅此而已。

执行这段代码的方式与您之前编写的任何其他 python 脚本是一样的。

```
python script.py -h
```

它将为我们提供脚本的描述和所有要用它的描述填充的标志。

现在让我们给它们一个值。

```
python script.py -ft=5 -st=7 -od='/my_computer/desktop/'
```

就是这样！

如果你已经读完了，这里有一份礼物:

</how-to-write-code-like-a-senior-developer-9ee34555858f>  

在这个故事中，你会发现作为一个高级开发人员使用一个超级简单的包来编写 Python 的秘密工具。

如果你有任何疑问尽管问，我很乐意回答。如果你喜欢，你可以鼓掌！它将帮助其他人找到这个故事！

Thx！