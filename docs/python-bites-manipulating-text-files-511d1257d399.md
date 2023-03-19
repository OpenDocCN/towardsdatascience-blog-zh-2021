# Python Bites:操纵文本文件

> 原文：<https://towardsdatascience.com/python-bites-manipulating-text-files-511d1257d399?source=collection_archive---------9----------------------->

## 读取、写入和追加

![](img/d0a37c1ae6d1b155117e7a36bd2de4e4.png)

照片由[帕特里克·福尔](https://unsplash.com/@patrickian4?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)在 [Unsplash](https://unsplash.com/s/photos/food-bite?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 拍摄

文本文件通常用于存储数据。因此，它们在数据科学系统中是必不可少的。Python 提供了多种功能和方法来处理文本文件。

创建文本文件有许多选项。因为这里的重点是操作文本文件，而不是创建文本文件，所以我们不会涵盖所有的内容。让我们用一种你可以在 jupyter 笔记本上使用的方法。

```
%%writefile myfile.txt
Python is awesome
This is the second line
This is the last line
```

我们现在在当前工作目录中有一个名为“myfile.txt”的文本文件。为了阅读文本文件的内容，我们首先需要打开它。

```
f = open("myfile.txt")
```

我们现在可以读取“myfile.txt”的内容了。

```
f.read()
'Python is awesome\nThis is the second line\nThis is the last line'
```

read 函数，顾名思义，读取文件的内容。您可能已经注意到，新行用“\n”表示。

使用 readlines 函数可以逐行读取文件内容。它返回一个列表，其中每一行都包含一个项目。

```
f.readlines()
[]
```

我们的 readlines 函数返回一个空列表，这有点问题。原因是一旦你读取一个文件，光标会一直移动到文件的末尾。下一次尝试读取同一文件时，光标找不到任何要读取的内容，因为它已经位于文件的末尾。

解决方法是将光标设置回初始位置。然后，我们将能够再次读取该文件。

```
f.seek(0)
0f.readlines()
['Python is awesome\n', 'This is the second line\n', 'This is the last line']
```

一旦内容被读取，我们应该关闭文件。否则，我们可能会遇到意想不到的结果。

```
f.close()
```

如果您试图在文件关闭后读取它，Python 会抛出一个值错误，指示对关闭的文件进行了 I/O 操作。

我们也可以写入一个现有的文件。模式参数用于选择操作类型。默认值为“r ”,代表读取。因此，在读取文件时，我们不需要指定模式参数的值。

如果要覆盖内容，模式设置为“w”。也可以通过 append 选项(' a ')向现有文件添加新内容。

我们还将查看演示如何覆盖文件或添加新内容的示例。在进入这一部分之前，我想介绍一种打开文件的新方法。

每次阅读文件时都要关闭文件，这听起来可能是一个乏味的操作。幸运的是，Python 提供了一种更实用的读写文件的方式。

```
with open("myfile.txt", mode='r') as f:
    content = f.read()print(content)
Python is awesome 
This is the second line 
This is the last line
```

我们不再需要显式调用文件的 close 方法。让我们给我们的文件添加一个新文件。我们应该选择追加选项。

```
with open("myfile.txt", mode='a') as f:
    f.write("\nTHIS IS THE NEW LINE")
```

这是新的内容。

```
with open("myfile.txt", mode='r') as f:
    updated_content = f.read()print(updated_content)
Python is awesome 
This is the second line 
This is the last line 
THIS IS THE NEW LINE
```

写模式覆盖文件。因此，它删除文件中已经存在的内容，并写入新内容。

```
with open("myfile.txt", mode='w') as f:
    f.write("THIS WILL OVERWRİTE THE EXISTING LINES")
```

让我们读新内容。

```
with open("myfile.txt", mode='r') as f:
    updated_content = f.read()print(updated_content)
THIS WILL OVERWRİTE THE EXISTING LINES
```

mode 参数也接受允许多个操作的参数。例如，“r+”打开一个文件进行读写。

```
with open("myfile.txt", mode='r+') as f:
    content = f.read()
    f.write("\nHere is a new line")
    f.seek(0)
    updated_content = f.read()
```

我们打开文件并阅读其内容。然后我们写新的一行，读取更新的内容。

```
print(content)
THIS WILL OVERWRİTE THE EXISTING LINESprint(updated_content)
THIS WILL OVERWRİTE THE EXISTING LINES 
Here is a new line
```

## 结论

我们做了几个简单的例子来展示 Python 如何处理文本文件的读写。当然，还有更多。这篇文章可以被认为是一个介绍，它将使你熟悉操作。

全面了解基础知识总是很重要的。通过积累基础知识，你将获得高级技能。

感谢您的阅读。如果您有任何反馈，请告诉我。