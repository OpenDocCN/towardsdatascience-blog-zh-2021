# Python 解析库:反转 F 字符串的简单方法

> 原文：<https://towardsdatascience.com/python-parse-library-simple-way-for-reversing-f-strings-72ad4d59e7c4?source=collection_archive---------10----------------------->

## 我们有时需要反过来。

![](img/8c50136a9bd15f71718a4d37bef78e33.png)

马修·斯特恩在 [Unsplash](https://unsplash.com/s/photos/reverse?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上的照片

字符串插值是使用占位符修改字符串的过程。结果字符串包括占位符的值。在 Python 中，format 方法和 f 字符串是两种常用的字符串插值方法。

在跳到[解析](https://pypi.org/project/parse/)库之前，做几个例子来演示什么是字符串插值是有帮助的。

```
folder = "notebook"
subfolder = "parse"file_path = f"documents/{folder}/{subfolder}"print(file_path)
documents/notebook/parse
```

占位符用花括号表示，它们的值包含在输出中。

这是另一个例子。

```
name = "John"
age = "23"print(f"{name} is {age} years old.")
John is 23 years old.
```

我们现在更熟悉 f 弦和一般意义上的弦插值。解析库所做的是字符串插值过程的逆过程。

可以使用解析库提取字符串中的值。我们将做几个例子来解释清楚这个过程。

解析库可以很容易地与 pip 一起安装。如果你用的是 jupyter 笔记本，就加“！”在匹普之前。

```
!pip install parse
```

## 从语法上分析

在第一个示例中，我们使用预定义的文件夹和子文件夹名称创建了一个文件路径。让我们使用解析库从路径中获取文件夹名。我们首先需要从解析库中导入*解析*函数。

```
from parse import parsefile_path = "documents/notebook/pandas"parse("documents/{folder}/{subfolder}", file_path)
<Result () {'folder': 'notebook', 'subfolder': 'pandas'}>
```

我们传递文件路径以及表示占位符的字符串。parse 函数返回一个结果对象，但是我们可以通过添加命名方法使它看起来更漂亮。

```
parse("documents/{folder}/{subfolder}", file_path).named
{'folder': 'notebook', 'subfolder': 'pandas'}
```

假设我们有一个格式相同的路径列表，我们需要提取文件夹和子文件夹的名称。这个任务可以通过解析器功能和列表理解的完美结合来轻松完成。

```
file_paths = [
    "documents/notebook/pandas",
    "documents/notebook/parse",
    "documents/notes/python"
][parse("documents/{folder}/{subfolder}", path).named for path in file_paths][{'folder': 'notebook', 'subfolder': 'pandas'},
 {'folder': 'notebook', 'subfolder': 'parse'},
 {'folder': 'notes', 'subfolder': 'python'}]
```

## 搜索

解析库还提供了一些其他函数，在特殊情况下会很方便。例如， *search* 函数在字符串中查找某种格式。因此，我们不必提供整个字符串的确切格式。

```
from parse import searchtxt = "Name: Jane, Department: Engineering, Age: 22"search("Name: {Name},", txt).named
{'Name': 'Jane'}
```

当我们不确定确切的字符串时，也可以使用搜索功能。因此，它提供了额外的灵活性。

考虑以下字符串:

```
txt = "The department of civil engineering has 23 employees"
txt2 = "The civil engineering department has 23 employees"
```

我们需要找到文本中雇员的数量。我们可以用同样的子模式找到它。

```
search("has {number_of_employees} employees", txt).named
{'number_of_employees': '23'}search("has {number_of_employees} employees", txt2).named
{'number_of_employees': '23'}
```

## 芬达尔

解析库中另一个有用的函数是 *findall* 函数。如果有多个我们感兴趣的相同模式的部分，我们可以使用 findall 而不是 parse。

以下示例显示了 findall 函数的一种可能用法。

```
from parse import findallpaths = "documents/notebook/pandas/ documents/notebook/parse/ documents/notes/python/"findall("documents/{folder}/{subfolder}/", paths)
<parse.ResultIterator at 0x2056556ab80>
```

我们有一个很长的字符串，其中包含多个格式相同的文件路径。findall 函数，顾名思义，查找给定路径结构的所有文件夹和子文件夹名称。

默认情况下，它返回一个迭代器，但我们可以很容易地将其转换成一个列表。

```
list(findall("documents/{folder}/{subfolder}/", paths))[<Result () {'folder': 'notebook', 'subfolder': 'pandas'}>,
 <Result () {'folder': 'notebook', 'subfolder': 'parse'}>,
 <Result () {'folder': 'notes', 'subfolder': 'python'}>]
```

该列表包含 3 个结果对象。我们可以将命名方法分别应用于这些对象。

```
a = list(findall("documents/{folder}/{subfolder}/", paths))a[0].named
{'folder': 'notebook', 'subfolder': 'pandas'}
```

## 结论

Parse 是一个非常实用的函数库。正如我们在示例中看到的，它提供了在字符串中查找模式和值的简单方法。某种意义上是字符串插值的逆向运算。

还有其他方法来执行相同的任务。例如，本文中的例子也可以用 regex 来完成。但是，对于某些操作来说，正则表达式可能太复杂了。我觉得解析库提供了更简单的解决方案。

如果你想了解更多关于解析库的知识，请随意访问官方文档。

感谢您的阅读。如果您有任何反馈，请告诉我。