# 使用 Python 可以完成的 7 种常见文件系统操作

> 原文：<https://towardsdatascience.com/7-common-file-system-operations-you-can-do-with-python-e4670c0d92f2?source=collection_archive---------9----------------------->

## 使用 OS 和 Pathlib 模块来自动化 Python 的任务。

![](img/3e32ddeae5dc13315fd43a875530462a.png)

作者图片(Canva 上制作)

在不安装任何第三方库的情况下，您可以在 Python 中做的最酷的事情之一是执行文件系统操作，例如创建文件夹、重命名文件和处理目录。尽管这些任务可以很容易地手动完成，但是您可以使用 Python 代码来自动化它们，以节省一些时间。

在本文中，我们将看到在 Python 中使用 os 和 Pathlib 模块可以完成的 7 种文件系统操作。每个操作包括实际的例子，所以你可以理解这两个模块之间的区别。

# 1.获取当前工作目录

在 Python 脚本中处理路径时，了解当前工作目录是最基本的。有两种路径—相对路径和绝对路径。绝对路径是指文件系统中相对于根目录(/)的相同位置，而相对路径是指文件系统中相对于当前工作目录的特定位置。

相对路径为你的脚本提供了灵活性，这就是为什么它有时比绝对路径更受欢迎。我们可以使用当前的工作目录来创建一个相对路径。

对于这个操作以及本文中列出的所有文件系统操作，我们必须导入操作系统和路径。

```
import osIn [1]: os.getcwd()
Out [1]: /Users/frank/PycharmProjects/DataScience
```

从上面的代码可以看出，我得到了包含我的数据科学项目的所有脚本的目录。这是我在 Pycharm 的工作目录。我们可以用 Pathlib 库获得相同的目录

```
from pathlib import Path

In [1]: Path.cwd()
Out [1]: /Users/frank/PycharmProjects/DataScience
```

如果我们打印`type(os.getcwd())`或`type(Path.cwd()`，我们可以看到 os 和 Pathlib 模块之间的主要区别。第一个只返回一个`string`，而第二个返回一个`PosixPath`对象，可以帮助我们做额外的操作。

# 2.列出目录内容

除了显示当前的工作目录，我们还可以列出目录中的所有文件。

```
In [1]: os.listdir()
Out [1]: ['script1.py', 'script2.py', 'script3.py']
```

就我而言，我的“数据科学”文件夹中只有 3 个 Python 脚本。我们可以用`Path().iterdir()`得到同样的结果，但是我们会得到一个`generator object`。为了从这个生成器中获得一个列表目录内容，我们使用了如下面的代码所示的`list()`。

```
In [1]: list(Path().iterdir())
Out [1]: [PosixPath('script1.py'), PosixPath('script2.py'), PosixPath('script3.py')]
```

我们甚至可以看到特定文件夹中的内容。例如，我的“数据科学”文件夹中有一个名为“数据集”的文件夹我们可以用下面的代码列出“Dataset”文件夹中的目录内容。

```
# os
os.listdir('Dataset')# pathlib
list(Path('Dataset').iterdir())
```

# 3.连接路径

在 Python 中连接字符串就像在两个单词之间使用“+”号一样简单；然而，当涉及到连接路径时，事情变得有点棘手。这是因为 Mac OS 在目录名中使用正斜杠“/”，而 Windows 使用反斜杠“\”，所以如果您希望 Python 代码能够跨平台运行，您需要一种不同的方法。幸运的是，os 和 Pathlib 模块可以解决这个问题。

```
# os
os.path.join(os.getcwd(), 'Dataset')# pathlib
from pathlib import Path, PurePath
PurePath.joinpath(Path.cwd(), 'Dataset')
```

如果我们打印上面写的代码，我们将得到一个包含工作目录和“数据集”的路径路径将看起来像这样`/Users/frank/PycharmProjects/DataScience/Dataset`。OS 库创建一个字符串，而 Pathlib 创建一个 PosixPath 类。但是，我们还没有创建“数据集”文件夹。要创建目录，请检查下一个文件系统操作。

# 4.创建目录

虽然您可以在几秒钟内创建一个目录，但当涉及到任务自动化时，编写一些 Python 代码将是最佳解决方案。我们可以用这两个库来执行这个任务，只是略有不同。让我们创建一个“数据集”文件夹作为示例。

```
# os
os.mkdir('Dataset')# pathlib
Path('Dataset').mkdir()
```

如果“数据集”文件夹不存在，将创建该文件夹。然而，如果我们试图创建一个已经存在的目录，Python 会抛出一个错误。也就是说，如果我已经有了一个名为 Dataset 的文件夹，脚本将会中断。幸运的是，我们可以用 Pathlib 轻松控制这种行为。

```
Path('Dataset').mkdir(exist_ok=True)
```

通过添加`exist_ok`参数，我们可以忽略错误，以防目录已经存在。

# 5.重命名文件名

与创建目录类似，重命名文件名是一项我们可以用 Python 脚本自动完成的任务。使用操作系统库，我们可以通过引入字符串名称来重命名文件。例如，让我们将“数据集”文件夹重命名为“数据”

```
os.rename('Dataset', 'Data')
```

我们也可以用 Pathlib 来完成这个任务。在这种情况下，目标名称可以是字符串或另一个 path 对象。让我们将文件夹重命名为“数据集”

```
current_path = Path('Data')
target_path = Path('Dataset')
Path.rename(current_path, target_path)
```

您甚至可以使用 for 循环重命名具有特定扩展名的多个文件。比如，我要在每个前面加上 2021 年。csv 文件在我的工作目录。

```
for i, file in os.listdir():
    if file.endswith('.csv'):
        os.rename(file, f'2021_{file}')
```

如果您想更进一步，您可以使用[时间模块](https://docs.python.org/3/library/time.html)将脚本运行的日期添加到文件名等等！

# 6.检查现有文件

您可以使用 OS 和 Pathlib 模块检查文件/目录是否存在。让我们看看文件夹数据集是否存在于我的工作目录中。

```
# os
os.path.exists('Dataset')# pathlib
check_path = Path('Dataset')
check_path.exists()
```

如果打印上面的代码，Python 将在文件夹存在时返回一个`True`值，或者在文件夹不存在时返回`False`值。因为我有一个名为“数据集”的文件夹，所以我得到了`True`。

# 7.[计]元数据

元数据是描述其他数据的数据。例如，创建日期、修改日期和文件大小。让我们获取我的工作目录中的一个`test.py`脚本的绝对路径。

```
# os
os.path.abspath('test.py')# pathlib
script = Path('test.py')
script.resolve()
```

打印上面的代码后得到了绝对路径`/Users/frank/PycharmProjects/DataScience/test.py`。

使用 Pathlib，我们可以很容易地获得文件的词干、后缀、文件大小和出生时间。

```
In [1]: print(script.stem)
In [2]: print(script.suffix)
In [3]: print(script.stat().st_size)
In [4]: print(script.stat().st_birthtime)Out [1]: test
Out [2]: .py
Out [3]: 1060
Out [4]: 1614190828.6
```

如你所见，出生时间是时间戳格式的。如果您想获得一个 datetime 对象，运行下面的代码。

```
from datetime import datetime

timestamp = 1614190828.6
dt_object = datetime.fromtimestamp(timestamp)

print("dt_object =", dt_object)
```

*就是这样！现在你知道了使用 Python 可以做的 7 个文件系统操作，你可以在* [*os*](https://docs.python.org/3/library/os.html) *和*[*pathlib*](https://docs.python.org/3/library/pathlib.html)*文档中查看更多操作。*

[**与 3k 以上的人一起加入我的电子邮件列表，获取我在所有教程中使用的 Python for Data Science 备忘单(免费 PDF)**](https://frankandrade.ck.page/bd063ff2d3)