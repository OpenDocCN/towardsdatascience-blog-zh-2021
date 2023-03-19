# 每个程序员都应该知道的 17 个终端命令

> 原文：<https://towardsdatascience.com/17-terminal-commands-every-programmer-should-know-4fc4f4a5e20e?source=collection_archive---------2----------------------->

## 改善您的日常工作流程

![](img/276e0b42d9dbe924f04ab441b9b0657a.png)

[Goran Ivos](https://unsplash.com/@goran_ivos?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

大多数计算机用户通过图形用户界面进行交互，因为这是操纵机器的一种简单方式。尽管 GUI 应用程序在广大用户中仍然很受欢迎，但命令行界面通过运行文本命令给用户提供了对计算机的更多控制。这是一种与电脑互动的强大方式。例如，您可以运行文本命令来创建 100 个文件夹，但是通过 GUI 应用程序来实现相同的结果将花费更多的时间和精力。

```
mkdir test
cd test
for i in {1..100}
do
   mkdir "$i"
done
```

为了能够运行这些命令，我们需要使用终端应用程序。这个终端为你提供了外壳。Shell 是一个我们可以运行 shell 命令和 shell 脚本的环境。它允许我们控制命令行工具。macOS X 自带 bash shell(又是 Bourne shell)，但是你可以把你的 Shell 改成 csh (C shell)、zsh (Z shell)、ksh (Korn Shell)、tcsh (TENEX C Shell)等等。现在让我们来看看一些基本的 shell 命令，看看发生了什么。列出 macOS X 中所有可用的 shells。

```
cat /etc/shells
```

查看您运行的是哪个 shell。

```
echo $0
```

改为 bash shell。

```
chsh -s /bin/bash
```

要改成 zsh shell。

```
chsh -s /bin/zsh
```

根据您使用的 shell，您将获得不同的特性。我选择的贝壳是 zsh。从这篇文章中，您将学习一些基本的 shell 命令，通过节省时间和精力来改进日常工作流程。

# 1.显示当前工作目录

`pwd`命令代表**打印工作目录**。它将打印当前工作目录的完整路径。

```
pwd
# /Users/universe/Desktop
```

如果一个单词以`#` (hash)开头，shell 会忽略该行上的任何剩余字符。

```
# /Users/universe/Desktop
```

# 2.mkdir

`mkdir`命令代表**制作目录**。这里我们在我的桌面上创建一个名为`neptune`的目录。

```
mkdir neptune
```

我们可以使用`-p`选项创建多个目录。

```
mkdir -p space/neptune
```

上面的命令将创建一个名为`space`的文件夹和一个嵌套文件夹或子文件夹`neptune`。如果我们想在`neptune`下创建第三个文件夹`naiad`，我们可以使用下面的命令。我们不必为此命令提供`-p`选项，因为路径`space/neptune`已经存在。

```
mkdir space/neptune/naiad
```

# 3.激光唱片

`cd`命令代表**改变目录**。让我们使用这个命令来访问我们之前创建的`space`目录。

```
cd space
cd neptune
cd naiad
```

或者你可以使用下面的命令来访问`naiad`目录。

```
cd space/neptune/naiad
```

两个命令将产生相同的结果。现在我们应该在`naiad`目录中。

![](img/dd9e95d92af174b75828eedd9f1f2cd0.png)

单周期和双周期

`..`(两个句点)表示或指向包含当前目录的目录，也称为父目录。这里`naiad`的父目录是`neptune`。让我们使用下面的命令在目录层次结构中向上移动一级。

```
pwd
# /Users/universe/Desktop/space/neptune/naiad
cd ..
pwd
# /Users/universe/Desktop/space/neptune
```

此外，我们可以移动到多个层次。在这种情况下，让我们向上移动 2 级。

```
cd ../..
pwd
# /Users/universe/Desktop
```

现在，我们回到了`Desktop`目录。

`.`(单句点)代表或指向当前工作目录。当我们在运行命令时不想写完整的路径时，这很有帮助。例如，如果我们的命令行工具在当前目录中，我们可以使用下面的命令。

```
./space-robot
```

`~` (tilda)或`$HOME`代表或指向主目录。对于 macOS X，用户的主目录在`/Users`目录下。

```
cd ~
pwd
# /Users/universe
cd $HOME
pwd
# /Users/universe
```

通过使用这种简写方式，您不必键入主目录的完整路径。

此外，我们可以看到一个`/`(正斜杠)用于分隔路径名中的目录。通过使用下面的命令，我们可以移动到根目录。

```
cd /
```

# 4.触控

使用`touch`命令，我们可以创建一个文件。

```
cd ~/Desktop/neptune 
touch todo.txt
```

# 5.限位开关（Limit Switch）

`ls`命令代表**列表**。我们可以用它来列出指定目录的所有内容；如果没有指定路径，它将列出当前目录中的所有内容。

```
cd ~/Desktop/neptune
mkdir todo
ls
# todo     todo.txt
```

使用-a 标志列出隐藏的文件和目录。

```
ls -a
```

使用-l 标志列出详细信息。

```
ls -l
```

或者一起用。

```
ls -al
```

# 6.清楚的

我们已经写了一段时间的命令，所以让我们清除终端屏幕。

```
clear
```

# 7.平均变化

`mv`命令代表**移动**。我们可以使用这个命令将文件和目录从一个地方移动到另一个地方。此外，我们可以用它来重命名文件和目录。以下命令将把 **todo.txt** 文件从其当前目录移动到其子目录`todo`。

```
cd ~/Desktop/neptune
mv todo.txt todo/todo.txt
```

现在让我们重命名该文件。这里我们将 **todo.txt** 文件重命名为 **my-todo.txt** 。

```
cd todo
mv todo.txt my-todo.txt
```

# 8.丙酸纤维素

`cp`命令代表**复制**。现在，让我们将我们的 **my-todo.txt** 文件复制到父目录。

```
cd ~/Desktop/neptune
cp todo/my-todo.txt my-todo-bu.txt
```

我们可以使用`-r`标志来复制一个目录。下面的命令会将`todo`文件夹中的所有内容复制到一个名为`bu`的文件夹中。

```
cp -r todo bu
```

# 9.rm 和 rmdir

`rm`指令代表**移除，**和`rmdir`指令代表`Remove Directory`。`rmdir`命令只能删除空目录。现在让我们删除一个文件和目录。

```
cd ~/Desktop/neptune
rm my-todo-bu.txt
mkdir empty
rmdir empty
```

我们可以使用`-r`标志删除一个非空文件夹。

```
rm -r bu
rm -r todo
```

# 10\. > < * ? [] ; $ \

`>`可用于重定向`stdout`(标准输出)。这里，我们将一个任务添加到我们的 **todo.txt** 文件中。`echo`命令将其参数写入标准输出。该`>`命令将覆盖已经存在的 **todo.txt** 。

```
cd ~/Desktop/neptune
echo 1\. Make space robot > todo_today.txt
```

如果我们想要追加数据，我们需要使用`>>`命令。

```
echo 2\. Make space robot v2 >> todo_today.txt
```

`<`可用于重定向`stdin`(标准输入)。这里，我们将 **todo.txt** 文件的内容重定向到`wc`命令，以打印行数、字数和字符数。

```
wc < todo_today.txt
#        2       9      43
```

`*`(星号)可以用作通配符来匹配零个或多个字符。

```
ls *.txt
# todo_today.txt
```

`?`(问号)可以用来匹配单个字符。

```
ls ????_?????.txt
# todo_today.txt
```

`[]`(方括号)可以用来匹配其中任意数量的字符。

```
ls t[o]do_*.???
# todo_today.txt
```

`;`(分号)可用于在一行上写多个命令；我们所要做的就是用分号分隔每个命令。

```
cd ~/Desktop/neptune ; wc < todo.txt
#        2       9      43
```

`$`(美元)可以用来创造和储存价值。

```
echo $HOME
# /Users/universe
MyValue=99
echo $MyValue
# 99
```

`\`(反斜杠)可以用来转义一个特殊字符。

```
echo \$MyValue
# $MyValue
echo \$$MyValue
# $99
```

# 11.猫

`cat`命令代表**连接**。它将一个或多个文件的内容打印到 stdout。打印单个文件的内容。

```
cd ~/Desktop/neptune
cat todo.txt
# 1\. Make space robot
# 2\. Make space robot v2
```

打印多个文件的内容。

```
cd ~/Desktop/neptune
echo FILE 1 > file_1.txt
echo FILE 2 > file_2.txt
echo FILE 3 > file_3.txt
cat file_1.txt file_2.txt file_3.txt
# FILE 1
# FILE 2
# FILE 3
```

# 12.可做文件内的字符串查找

`grep`命令代表**全局正则表达式打印**。它在用给定搜索模式指定的文件中搜索文本。默认情况下，grep 区分大小写。对于案例激励搜索，我们可以使用`-i`标志。

```
cd ~/Desktop/neptune
grep "FILE" file_1.txt
# FILE 1
grep -i "file" file_1.txt
FILE 1
```

`-r`标志用于递归搜索目录。在这里，它将搜索当前目录及其子目录中的所有内容。

```
grep -r "FILE" .
# ./file_1.txt:FILE 1
# ./file_2.txt:FILE 2
# ./file_3.txt:FILE 3
```

还有其他有用的标志，比如`-n`标志将打印带有行号的匹配行，`-c`标志将打印匹配行的计数。

# 13\. |

`|`命令或管道命令用于添加两个或更多命令，其中前一个命令的输出用作下一个命令的输入。

```
command_1 | command_2 | .................. | command_N
```

在这里，我们通过组合`cat`和`sort`命令，对 **todo.txt** 文件进行逆序排序。

```
cat todo.txt | sort -r
# 2\. Make space robot v2
# 1\. Make space robot
```

# 14.头和尾

命令`head`和`tail`可用于将文件内容打印到标准输出。标志`-n`用于定义我们想要输出多少行。现在，让我们打印文件的前 1 行。

```
cd ~/Desktop/neptune
head -n 1 todo.txt
# 1\. Make space robot
```

打印文件的最后一行。

```
tail -n 1 todo.txt
# 2\. Make space robot v2
```

处理大文件时，这些命令会很有用。

# 15.发现

`find`命令用于搜索文件。

```
find dir -name search_pattern
```

让我们找到当前目录中的所有文本文件。

```
cd ~/Desktop/neptune
find . -name "*.txt"
# ./file_1.txt
# ./todo.txt
# ./file_2.txt
# ./file_3.txt
```

# 16.打开

`open`命令可用于使用 Finder 应用程序打开文件和目录。

```
cd ~/Desktop/neptune
open .
open todo.txt
```

# 17.男人

最后，我们有`man`命令，代表**手动**。它打印命令的用户手册。

```
man cd
```

使用上下箭头键浏览文档。使用`f`键前进一页，使用`b`键后退一页——按`q`退出。

恭喜你！现在你知道了所有这些很酷的 shell 命令，它们将帮助你节省时间和精力。你可以在我的 [GitHub](https://github.com/lifeparticle/Terminal-Bot/tree/master/commands) 上找到我讨论过的所有 shell 命令。编码快乐！

# 相关帖子

[](/the-ultimate-markdown-cheat-sheet-3d3976b31a0) [## 终极降价备忘单

### 写一篇精彩的自述

towardsdatascience.com](/the-ultimate-markdown-cheat-sheet-3d3976b31a0)