# 带寄存器的 Python 多态性| Python 模式

> 原文：<https://towardsdatascience.com/python-polymorphism-with-class-discovery-28908ac6456f?source=collection_archive---------4----------------------->

## 学习一种模式来隔离包，同时扩展 Python 代码的功能。

![](img/b27a81cf9f28d862709af9b3e59b67d0.png)

照片由 [N.](https://unsplash.com/@ellladee?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

动态多态是面向对象编程最强大的特性，它允许我们针对那些实际行为只在运行时定义的抽象进行编程。根据 Robert C. Martin 的说法，这也是真正[定义 OOP](https://blog.cleancoder.com/uncle-bob/2014/11/24/FPvsOO.html) 的唯一特性。

多态性由两部分组成:具有类型相关实现的方法和类型不可知的方法调用。在 python 代码中:

```
class TypeA:
  def speak(self):
    print("Hello, this is an object of type TypeA")class TypeB:
  def speak(self):
    print("Greetings from type TypeB")def agnostic_speak(speaker):
  speaker.speak()agnostic_speak(TypeA())
agnostic_speak(TypeB())>> Hello, this is an object of type TypeA
   Greetings from type TypeB
```

这个简单的例子展示了多态方法 speak()和调用它的通用函数，而不知道调用该方法的对象的实际类型。

对于这样一个简单的例子，一个基于参数类型的条件语句(if/else)可以完成这个任务，但是它也有缺点。首先，条件代码使得函数更难阅读。在这个版本中，我们立即看到它做了什么，它让一个对象*说话*，而对于条件代码，我们需要首先理解不同的分支做什么。此外，如果将来我们想要添加更多的说话方式，我们将需要返回到这个函数并修改它以添加新的分支。反过来，新的分支将使代码越来越不可读。

假设我们有一个文本分类应用程序，它在输入中接收一行文本，并输出一个代表其主题的标签。像任何好的机器学习项目一样，我们在不同的模型上执行许多迭代，我们希望我们的代码使用它们。当然，我们不希望一个大的 switch 语句(if-elif-elif-…-else)带有运行任何单一模型的逻辑！

出于示例的原因，所有模型共享相同的输入/输出词汇表，即相同的映射 word->input index 和 output index->label。唯一的区别在于底层模型所执行的操作。

在我们代码的第一个版本中，我们只有两个模型架构:一个前馈网络和一个 LSTM 网络，它们提供多态的 *forward()* 方法来封装每个模型的逻辑:

```
# main.py
from models import FfModel, LstmModeldef main(args):
  model_type = args.model_type
  model_path = args.model_path
  if model_type == "ff":
    model = FfModel(model_path)
  elif model_type == "lstm":
    model = LstmModel(model_path)
  else:
    raise NotImplementedError("Unrecognizer type %s" % model_type) outputs = [] with open(args.input_text, 'r') as fid:
    for line in fid:
      word_ids = convert_to_id(tokenize(line)) 
      output = model.forward(word_ids)
      outputs.append(convert_to_labels(output)) show_results(outputs)
```

该代码包含许多未定义的函数，以便让我们专注于本文的相关部分。在这个片段中，我们可以看到模型类型是由用户提供的参数。它用于选择哪种模型类型，由一个类表示，应该用来加载和使用我们保存的模型。注意，这两个类都被导入到主文件中。在代码片段的后半部分，我们有对 *forward* 的多态调用，它根据深度学习模型的类型正确地执行深度学习模型操作。最后，我们有在文本和不依赖于模型的神经网络格式之间转换输入和输出的函数。

这段代码片段成功地提供了多态行为，但它显然打破了软件设计的[坚实](https://en.wikipedia.org/wiki/SOLID)原则之一的[开/闭原则](https://en.wikipedia.org/wiki/Open%E2%80%93closed_principle)。这个原则规定我们的程序应该*对扩展开放，但对修改*关闭。

在实践中，这意味着每当我们想在程序中添加一个新的模型类型时，我们必须为该类型创建一个新的类，然后将其导入 main.py，最后在 if/else 语句中为其添加一个新的分支。我们的程序对扩展开放，但也对修改开放。如果我们想将允许的类型添加到我们的软件帮助中，这样用户就可以很容易地发现它们。每当我们添加一个新的类型时，我们也应该把它的名字添加到允许的类型列表中，否则会给用户带来很大的困扰。

改进上述代码的第一种方法是用一个函数替换条件分支，该函数将模型类型及其路径作为输入，并返回构建的对象(类似于[工厂方法](https://en.wikipedia.org/wiki/Factory_method_pattern))。上面的代码将变成:

```
# main.py
from models import model_factorydef main(args):
  model_type = args.model_type
  model_path = args.model_path model = model_factory(model_type, model_path)

  outputs = [] with open(args.input_text, 'r') as fid:
    for line in fid:
      word_ids = convert_to_id(tokenize(line)) 
      output = model.forward(word_ids)
      outputs.append(convert_to_labels(output)) show_results(outputs)
```

现在模型选择被移动到 __init__。模型包的 py 文件，在这里我们还用对字典的调用替换了条件代码:

```
# models.__init__.py
__all__ = ["factory_method"]from .ff_model import FfModel
from .lstm_model import LstmModel__MODEL_DICT__ = {
  "ff": FfModel,
  "lstm": LstmModel
}def factory_method(model_type, model_path):
  return __MODEL_DICT__[model_type](model_path)
```

这里我们只导出 factory_method，它从字典中选择正确的类型名(在 python 中是用于创建该类型对象的工厂),构建一个对象并将其返回给调用者。字典是将条件代码简化为线性流的简单有效的方法。

有了这段代码，我们的主文件就可以忽略现有的模型类型，我们可以添加或删除类，而根本不用修改主文件。然而，我们只是将开放修改代码从 *main.py* 移到了这个新的 __init.py 文件中。我们通过将主文件与可能的模型修改隔离开来，实现了改进，但是 __init__ 中的代码。出于同样的原因，py 仍然可以修改。

我们能做得比这更好吗？当我们添加一个新的模型类型时，我们可以用这样的方式编写代码吗？我们只需添加新的类和一个名称来标识它，这样就足以将它添加到 __MODEL_DICT__ 字典中了。

事实证明，这不仅在 python 中是可能的，而且只需要一堆代码就可以实现。该机制由两部分组成:

1.  **类发现:**一种无需为类编写显式导入语句即可导入类的算法。
2.  **注册:**被发现的类自动调用的函数，被添加到字典中，如 __MODEL_DICT__

让我们轮流看他们。

# 自动发现

我们如何导入类而不为它们或者甚至包含它们的文件(模块)写一个显式的导入呢？或者更根本地说，在给定这些约束的情况下，我们如何让 Python 解释器在运行时知道它们？

这个解决方案是通过 [importlib](https://docs.python.org/3/library/importlib.html) 包中的 [import_module](https://docs.python.org/3/library/importlib.html#importlib.import_module) 函数实现的。来自 importlib 官方文档:

> […]实现`[import](https://docs.python.org/3/reference/simple_stmts.html#import)`的组件在这个包中公开，使得用户更容易创建他们自己的定制对象(一般称为[导入器](https://docs.python.org/3/glossary.html#term-importer))来参与导入过程。

正是我们需要的！让我们看看可以添加到 __init__ 中的代码。py 使它成为我们模型类的自定义导入器:

```
# Iterate all files in the same directory
for file in os.listdir(os.path.dirname(__file__)):                                 
  # Exclude __init__.py and other non-python files
  if file.endswith('.py') and not file.startswith('_'):
    # Remove the .py extension  
    module_name = file[:-len('.py')]     
    # Assume src to be the name of the source root directory                              
    importlib.import_module('models.' + module_name)
```

通过这四行代码，我们的软件自动导入模型包中的所有模块。当一个模块被导入时，它的代码被执行，所以我们的下一步是实现一个机制，让我们的类把它们自己添加到我们的 __MODEL_DICT__。

# 子类注册

第一种方法修改了初始化子类的 Python 机制。幸运的是，Python 公开了在第一次执行类定义时调用的方法 __init_sub_class__。每个类都自动调用父类的 __init_sub_class__ 并将其类型作为参数。因此，我们可以在父类中集中注册新类的逻辑:

```
# models.model.py__MODEL_DICT__ = dict()class Model:
  def __init_sub_class__(cls, **kwargs):
    assert "name" in kwargs
    super().__init_sub_class__()
    if kwargs["name"] in __MODEL_DICT__:
      raise ValueError("Name %s already registered!" % name)
    __MODEL_DICT__[kwargs["name"]] = cls
```

__init_sub_class__ 接受一个名为 cls 的参数作为输入，这是类类型参数的 Python 约定，以及存储在 kwargs 附加库中的任意数量的键值对。我们首先检查 *"name"* 是否作为一个键存在于附加参数中，因为如果我们不知道用哪个名字注册一个类，我们就不能在字典中注册它。然后，如果以前没有注册过具有该名称的类，只需将它添加到字典中，否则程序会退出并显示一个错误，通知用户存在重复。

现在，我们只需要每个子类将其名称作为附加参数传递给 __init_sub_class__。这不是通过直接调用函数来完成的，而是在定义父类时完成的:

```
# models.ff_model.py
from .model import Modelclass FfModel(Model, name="ff"):
  def __init__(self, *args, **kwargs):
    # Construct object def forward(self, x):
    # Operations for computing output probabilities of x
```

类实现不知道注册过程，它只通过父类名旁边的赋值 name="ff "出现。该赋值以及您可能愿意添加的任何其他赋值将构成模型 __init_sub_class__ 的**kwargs。

同样，对于 LstmModel:

```
# models.lstm_model.py
from .model import Modelclass LstmModel(Model, name="lstm"):
  def __init__(self, *args, **kwargs):
    # Construct objectdef forward(self, x):
    # Operations for computing output probabilities of x
```

在 __init__ 时。py 我们需要执行发现代码并公开一个工厂方法来访问字典

```
# __init__.py
__all__ == ["factory_method"] import os
import importlibfrom models.model import Model, __MODEL_DICT__ def factory_method(name, path):
    return __MODEL_DICT__[name](path)

for file in os.listdir(os.path.dirname(__file__)):
    if file.endswith('.py') and not file.startswith('_'):
        module_name = file[:file.find('.py')]
        module = importlib.import_module('models.' + module_name)
```

# 寄存器功能

第二种方法是定义一个包装我们的类的“注册”函数。register 函数的内部工作有点复杂，所以请原谅我。当包含由我们的注册函数包装的类的模块第一次被执行时，它用它的参数运行包装函数。包装函数定义了一个内部函数，该函数将一个类**类型**作为输入，并对其进行处理。此外，内部函数可以访问包装函数中的变量，因为它是一个[闭包](https://betterprogramming.pub/5-essential-aspects-of-python-closures-494a04e7b65e)。register 函数简单地返回它的内部函数，这个内部函数又以包装的类类型作为它的输入被立即执行。内部函数是实际注册类的函数，然后返回将由 python 解释器添加到当前名称空间的类类型。

这个过程在 __init__ 之间再次拆分。py 文件和包含实际类的模块文件。

```
# models.__init__.py
__all__ = ["factory_method"]

import os
import importlib

from .model import Model

def factory_method(name):
    return __MODEL_DICT__[name]

__MODEL_DICT__ = dict()

def register_function(name):

    def register_function_fn(cls):
        if name in __MODEL_DICT__:
            raise ValueError("Name %s already registered!" % name)
        if not issubclass(cls, Model):
            raise ValueError("Class %s is not a subclass of %s" % (cls, Model))
        __MODEL_DICT__[name] = cls
        return cls

    return register_function_fn

for file in os.listdir(os.path.dirname(__file__)):
    if file.endswith('.py') and not file.startswith('_'):
        module_name = file[:file.find('.py')]
        module = importlib.import_module('models.' + module_name)
```

同样， *factory_method* 是我们的 main 调用的函数，它基本上是我们字典的包装器。请注意，在我们的内部 register_function_fn 中，我们可以执行我们通常可以在任何函数中执行的任何操作，因此我们额外检查了给定的类是 Model 的子类。这是保持基于层次结构的多态性所必需的，因为我们不能保证 register 类确实是一个模型。

另一种可能的检查可以基于[鸭类型](https://en.wikipedia.org/wiki/Duck_typing)，并且将检查类中是否存在*转发*方法，而不是检查其类型。python 的动态性也允许我们不执行任何检查，但是当我们使用 buggy 模型时，这种勇敢会导致令人讨厌的错误出现。相比之下，在我们的注册机制中插入检查将在每次运行程序时检查所有模型的一致性。在静态类型语言中，这种检查是由类型检查器执行的，而 Python 为我们提供了更多的动态性，但也为我们清理自己的混乱提供了更多的责任。

现在，我们只需要使用 register_function 作为我们的类的[装饰器](https://gist.github.com/Zearin/2f40b7b9cfc51132851a)，以使它的功能如上所述:

```
# models.ff_model.py
from models.model import Model @register_function("ff")
class FfModel(Model):
  def __init__(self, *args, **kwargs):
    # Construct object def forward(self, x):
    # Operations for computing output probabilities of x
```

通过使用 register_function 作为装饰器，注册机制在类的外部，因此它可以完成它的工作，然后模型类不需要任何关于注册的代码。

该示例的工作方式如下:

*   *register_function* 以*“ff”*作为唯一参数被调用
*   它返回 *register_function_fn* 封闭绑定 *name="ff"*
*   *用 *cls=FfModel* 调用寄存器 _ 函数 _fn*
*   *register_function_fn* 在我们的 *__MODEL_DICT__* 中注册键值对*“ff”= FfModel*，并将 ff MODEL 返回给解释器
*   现在可以通过函数 *factory_method()* 在应用程序中使用 FfModel，并且不需要显式导出。

第一次很难掌握像这样的注册功能的工作原理，主要是因为所有的操作都是在启动阶段执行的。然而，如果你能遵循上面的解释，你将最终掌握一个强大的工具，它的应用远远超出了这个例子。

与前面的解决方案相比，register 函数的难度为我们带来了更多的灵活性，因为它没有将我们限制在类的层次结构中，也可以用于函数而不是类。此外，从[单一责任原则](https://en.wikipedia.org/wiki/Single-responsibility_principle) (SRP)的角度来看，这段代码更好，因为注册子类的责任被交给了一个只负责注册子类的函数，而不是父模型类。

# 结论

类发现是一个强大的工具，它通过 Python 项目的两个组件之间的抽象接口提供多态行为。

最终的设计在不同的包之间有更少的显式依赖，而在同一个包内有更多的依赖。然后，它强制不同包及其内部内聚性的解耦。

此外，我们还看到了这种模式是如何产生更加尊重可靠原则的代码的。

当一个包主要由相同层次结构中的类组成时，我推荐使用它，这样就可以用不同的实现达到相同的目的。只有在定义代码的地方才需要修改代码，这让我们在开发的时候心情愉快。

# 承认

我从脸书的 FAIRseq 开源项目中学会了如何使用注册函数。你可以在这里找到他们当前的实现或者从我的[老叉](https://github.com/mattiadg/FBK-Fairseq-ST/blob/f30e2b57659f1efb0ab95a35e44025d75a70af05/fairseq/models/__init__.py#L36-L62)找到一个更简单的版本。

__init_sub_class__ 方法的一个很好的解释可以在[堆栈溢出](https://stackoverflow.com/a/50099920)中找到。