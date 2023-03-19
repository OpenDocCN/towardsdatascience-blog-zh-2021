# 超越初级阶段的 Python

> 原文：<https://towardsdatascience.com/python-beyond-beginner-stage-good-practices-and-tools-75ddd55b445d?source=collection_archive---------16----------------------->

## 帮助您更上一层楼的工具和最佳实践

![](img/15b9c113fdbe1625201939fb441342a8.png)

图片来源:[undraw.co](http://undraw.co)

所以你已经开始用 Python 了。你对语法、控制流和操作符了如指掌。您可能已经熟悉了一些特殊的 Python 特性，比如 list 和 dict comprehension、lambda 函数、inline if、for..以此类推。你可能也用过类和生成器。

我想分享一些特性、工具、库和良好的实践，你可以将它们包含在你的开发流程中，以提高效率，使你的代码更加健壮。

# 排除故障

学习 python 调试器真的有回报。

只需在您希望程序暂停的地方插入`import pdb; pdb.set_trace()`，然后您可以键入`!my_var`来查看变量并执行任何语句，如`!my_function(my_var)`您可以通过键入`s`来单步执行代码(步骤)。

使用`ipdb`作为基于 ipython 的`pdb`的替代，以获得更友好的用户体验和多行代码支持。

*如果您希望代码在发生错误(异常)*时暂停，而不插入 set_trace 断点，将`python my_program.py`更改为`python -m pdb -c continue my_program.py`，您将在第一个未捕获的异常处得到 pdb 提示。如果代码中没有出现异常，您可以使用`u`跳出调用堆栈。

**如果你使用的是 jupyter** **notebook** 的话，你很幸运继承了 ipython 的特性。如果您的单元格引发了异常，创建一个新的单元格并键入`%debug`来附加调试器。谷歌工程-colab 和 vs 代码 jupyter 笔记本扩展太。

**如果你正在使用 vs 代码**，你可以使用 python 插件的调试功能，这是上帝派来调试多处理和多线程代码的——允许你单独检查每个进程，并在代码运行时给你创建断点*的自由。*

**如果你正在使用 python 的命令行控制台**，我建议你使用带有`%debug`神奇功能支持的`ipython`来替代`python`。

# 测试

每个人通常都测试自己的工作。他们至少运行一次他们的程序，看看它是否给出了期望的输出。

有了经验，你可能会意识到，用不同的输入场景或环境条件来检查你的程序，对于避免以后错误破坏你的软件是很重要的。

您可以手动完成，但是当您的代码变得更加复杂，或者您的经理想要测试用例通过的证明时，您会想要考虑自动化和记录测试流程。

对于初学者来说，您可以在您的`my_code.py`旁边创建一个名为`test_my_code.py`的文件，并用`assert`语句
创建一个函数`test_my_func()`来检查各种输入情况下的输出。

您可以调用`test_my_code.py`的`__main__`块中的函数并通过命令行运行它，或者更好地使用`pytest`及其丰富的功能框架来运行`pytest test_my_code.py`，它调用带有`test`前缀的文件中的所有函数。我也在我的主要项目中使用`unittest`，它是一个 python 内置库。

一旦您的代码开始与其他昂贵而复杂的组件进行交互，您将需要单独测试您的代码组件(如函数和类)。为此，您需要暂时删除其他组件，并用固定输出替换它们。这叫猴子打补丁。

在`pytest`中，您可以使用 [monkeypatch](https://docs.pytest.org/en/stable/monkeypatch.html) ，在`unittest`中有`[mock](https://docs.python.org/3/library/unittest.mock.html)`模块。

这些测试框架的其他用例是:

*   检查所有的测试用例，即使有一个失败。计算错误的总数。
*   使用`==`比较两个字典、嵌套列表、floats(考虑精度)以及其他一些不能简单比较的对象类型。
*   检查传递给模拟对象的参数和它被调用的次数以及每次调用的参数。
*   断言应该在代码块中引发异常。
*   并行运行测试

# 开发—使用打字

键入是指定您希望变量包含的数据的“类型”。虽然它不会影响 Python 的执行，但它有助于代码开发。举个例子:

```
def get_marks(name: str) -> float:
    return random.rand() * 100
```

它说 get_marks 接受一个字符串(在变量`name`中)并返回一个浮点数。怎么又有用了？

**原因 1** 假设我对`random`库不熟悉，想用函数检查艾伯特的分数是否等于玛丽的分数。我可能会假设返回类型是一个整数，并使用`==`操作符，由于涉及到精度问题，将其用于 float 是不安全的。

理由二有助于提高可读性。代码审查者会感谢你的。使用你图书馆的人会喜欢你的。

**原因 3** 型跳棋。你可以使用像`mypy`这样的库，它使用类型检查来确定某些操作是否与对象的类型不兼容。比如 strings 有`.tolower()`方法但是 int 没有。在大型代码库中，运行`mypy`比用所有测试用例运行程序来找出这样一个 bug 要容易得多。

**原因 4** 运行时数据验证器。

示例 1:您正在通过 web 服务一个应用程序，并希望自动验证“post”数据的类型。对`fastapi`库使用类型提示。它返回适当的状态代码，生成模式和 swagger 文档。

示例 2:一个类中的变量只能接受某些数据类型，而你不想将其他数据类型传递给`__init__`？使用`pydantic`来验证在其 dataclass 中传递的每个值。

# 架构—使用数据类

> Python 5 将只有这些[数据类]，没有'普通'类。奥利弗·拉塞尔

尽管 Oliver 是以开玩笑的方式说的，但是数据类的用处不能被夸大。除了在`__init__`中初始化变量，你可以做以下事情:

```
from dataclasses import dataclass
@dataclass
class Food:
    name: str
    carbs_percent: int
    fiber_percent: int

    @property
    def net_carbs(self):
        return self.carbs_percent + self.fiber_percentFood("Apple", 40, fiber_percent=5).net_carbs 
# returns 45
```

有几个实现数据类的框架。除了内置的`dataclass`库之外，还有`attrs`和`pydantic`，它们都有各自的缺点和优点。我推荐具有数据验证特性的`pydantic`。

您可以使用`dataclasses.as_dict`函数轻松地将数据类转换为字典，这对于序列化来说很方便，并且可以通过使用字典解包`**my_dict`或外部工具(如支持数据类内部数据类的`dacite.from_dict`)将其转换回来。三个库中的每一个对于`as_dict`和`from_dict`具有相同的功能。

使用 dataclasses 的一个副作用是，它迫使您键入变量，这在 Python 中是一个非常好的编码实践。

要指定变量的默认值，在默认值不可变的情况下直接指定，或使用`dataclasses.field`类似如下:

```
from dataclasses import field
@dataclass
class Food:
    name: str = ''
    subitems: list = field(default_factory=list)
```

如果您直接分配`subitems: list = []`，那么您对一个实例如`Food().subitems.append(3)`所做的任何更改都将改变所有`Food`实例的值。这就是为什么这是不允许的，而且`dataclasses`会在定义中提出一个例外。**然而，如果赋值不是默认类型(list、dict 等),它不会检测赋值是否可变。)**所以需要小心。例如，不要做以下事情

```
# USE DEFAULT FACTORY INSTEAD OF THE FOLLOWING
@dataclass
class Animal:
    name: str = ''
@dataclass
class Store:
    animal: Animal = Animal("dog")store1 = Store()
store1.animal.name = "cat"store2 = Store()
print(store2.animal.name) # prints "cat"
```

改为做`animal: Animal = field(default_factory=lambda: Animal("dog"))`

# 架构—使用命名元组

如果你想在 python 中返回多个变量，你通常要做如下的事情

```
def get_time_and_place() -> Tuple[datetime, str]:
    return datetime.now(), 'Combodia'
```

请注意，当您返回多个变量时，返回类型是一个元组。元组可以像下面这样解包

`mytime, myplace = get_time_and_place()`

当返回值的数量变得很多时，跟踪`myplace`是在元组的第 5 个索引还是第 6 个索引变得更加困难。您必须仔细阅读函数的 docstring，以确保您以正确的顺序解包返回值。键入函数返回值变得繁琐，比如`-> Tuple[int, str, str, List[int]]`。

这些问题可以通过使用`typing.NamedTuple`或`collections.namedtuple`来解决，我推荐使用打字。因其用户友好的特性而得名。

```
class TimeAndPlace(typing.NamedTuple):
    time: datetime
    place: str = 'Madagascar'def get_time_and_place() -> TimeAndPlace:
    output = TimeAndPlace(datetime.now())
    return output
```

现在，您获得了一个以名称作为属性的元组，其索引和解包特性保持不变。

```
mytime, myplace = TimeAndPlace(dattetime.now())
TimeAndPlace(dattetime.now())[1] # returns Madagascar
```

您可以在这里使用 dataclasses，但是会丢失元组的属性。您不能索引或解包。你也失去了不变性，所以不能作为字典中的一个键。

# 架构—使用枚举

```
from enum import Enumclass Label(Enum):
    dog='dog'
    cat='cat'
```

Enum 是一种数据结构，当某个特定变量有不同的值时，可以使用它。这解决了一个常见的问题，即使用字符串存储变量的类，然后当值为`cat`并且语句评估为 False 时，错误地使用了`label == 'cats'`。

如果您写`label == Label.cats`，Enum 通过引发 AttributeError 来防止这种情况发生。它还能让 pylint 和 mypy 这样的 linters 检测到错误。

如果你使用 IDE，比如 jupyter notebook 或者 VS-code with Pylance，你会得到自动完成功能，防止你一开始就写`Label.cats`。

如果您从一个文本文件中获得标签字符串输入，那么您可以按照以下方式使用 Enum 进行数据验证

```
label_text: str = open("my_label.txt").read()
label = Label(label_text)
```

如果 my_label.txt 是`cats`而不是`cat`，上面的代码会引发 ValueError，否则会将字符串转换为适当的属性。

`Label.cat.value`返回可用于序列化和保存的“cat”字符串。

如果在其中一个值中使用 json.dumps 和 Enum，将会引发异常。在[这个堆栈中有解决方法。在我看来，最好的方法是使用 IntEnum，它也是 python `int`的子类，这样你就可以直接使用`0 == Label.cat`，而不是先将你的变量显式转换成 Enum。缺点是你失去了序列化的可读性。](https://stackoverflow.com/questions/24481852/serialising-an-enum-member-to-json)

# 架构—使用路径库

内置的`pathlib`实现了操作和验证文件路径的面向对象的方法。考虑以下情况:

**遍历目录并读取文件**

使用操作系统

```
import os
for maybe_file in os.listdir(my_dir):
    if not os.path.isfile(maybe_file): continue
    with open(os.path.join(my_dir, file)) as f:
        print(f.read())
```

使用 pathlib

```
from pathlib import Path
for file in Path(my_dir).iterdir():
     if not file.is_file(): continue
     with file.open() as f:
         print(f.read())
```

这是一种更干净的方法。关键是通过传递一个类似于代表路径的对象`./`的字符串来实例化的`Path`对象。对象有`is_file()`、`exists()`、`name`、`glob('*.jpeg')`等方法和属性

最引人注目的是使用`/`操作符构建路径的方式。考虑:

```
>>> os.path.join(new_dir, os.path.basename(file))
/home/ubuntu/project/x.jpeg
```

相对

```
>>> new_dir / file.name
Path(/home/ubuntu/project/x.jpeg)
```

# 林挺

linter 可以帮助你在不运行程序的情况下发现许多类型的错误。有时有一些类型的错误你的测试用例会错过，但是 linter 可能会捕捉到。如果你使用任何像 VS 代码一样的 IDE，你可以在每次保存代码时找到错误。

例如，您可以知道是否拼错了变量或函数名，是否使用了不存在的类属性，是否传递的参数比函数预期的多，或者是否多次传递参数。如果你有类型注释，linters 将捕捉所有类型的不兼容性。

如果代码中有类似于捕获一般异常或未使用变量的味道，它也会发出警告。它还建议你做重构和其他在 PEP 和其他地方推荐的最佳实践。

我推荐同时使用`pylint`和`mypy`，这可以通过在 VS 代码设置(JSON)中设置以下标志在 VS 代码中实现

```
"python.linting.mypyEnabled": true,
"python.linting.pylintEnabled": true,
```

在自动化构建管道中运行测试之前，您可以使用 pylint 来检查失败。您可以检查该命令返回的退出代码，以查看错误级别(致命/错误/警告…)[https://pypi.org/project/pylint-exit/](https://pypi.org/project/pylint-exit/)

# 可读性—样式和格式

1.  好的库对每个函数、类、类方法和模块都有文档字符串。我推荐 google 风格的 docstrings[https://sphinxcontrib-Napoleon . readthe docs . io/en/latest/example _ Google . html](https://sphinxcontrib-napoleon.readthedocs.io/en/latest/example_google.html)
2.  代码布局基于 https://pep8.org/`PEP8`T21
3.  使用手动格式化和自动格式化的组合，如`yapf`或`autopep8`(还有其他的你可以试试)。

# 结论

*   使用`pdb`、vs code python extension、`ipython` magic commands 这样的工具，会减少调试的痛苦。
*   在你的代码设计中包括:类型提示、`dataclasses`、`enum`、`pathlib`和`namedtuples`。这些将使您的代码更具可伸缩性、可读性、易用性和可扩展性。
*   像`pytest`和`unittest`这样的测试框架将帮助你以编程方式编写测试。
*   像`mypy`、`pylint`、`pylance` (vs 代码)这样的 Linter 提高了代码质量。
*   遵循代码风格指南 PEP8 和`docstring`指南，以提高可读性和文档。