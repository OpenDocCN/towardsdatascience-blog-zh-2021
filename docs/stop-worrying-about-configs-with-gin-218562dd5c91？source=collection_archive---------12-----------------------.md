# 不要担心杜松子酒的配置

> 原文：<https://towardsdatascience.com/stop-worrying-about-configs-with-gin-218562dd5c91?source=collection_archive---------12----------------------->

## `gin-config`能帮助消除数据科学项目中令人头痛的配置文件吗？

![](img/8ca16f18ddf5ce6dd5827440ccc90d3a.png)

图片由 [Ernest_Roy](https://pixabay.com/users/ernest_roy-1284978/) 在 pixabay 上提供

我们都去过那里，你在建模，一个接一个的超参数开始增加。起初，你忽略了这个问题，并告诉自己以后会回来解决它，但不可避免地不会。有许多解决项目配置问题的坏方法，它们包括但绝不限于:

配置项目的真正愚蠢的方法:

1.  在你的脚本顶部创建一个变量列表，你可以修改它，直到你找到合适的为止
2.  也许你只是为你动态改变的函数和类定义了默认值

配置项目的不那么愚蠢的方法:

1.  使用一个 [TOML](https://github.com/toml-lang/toml) 或 [YAML](https://yaml.org/) 文件，并随身携带一个配置变量字典
2.  使用配置脚本来设置由您的包读取的环境变量

最后两个选项并不是不好的选项，但是它们带来了额外的开销，即必须携带一个参数字典，您可以将它作为 T1 传递给函数或类。如果我们能把这些都抽象出来，让 Python 来帮我们做所有的重活，会怎么样？这就是杜松子酒的用武之地。Gin 是 Google 开发的一个轻量级 Python 库，专门尝试解决这个问题。在这篇文章中，我们将看一下如何使用这个库的基础知识，然后试着找出你是否真的会用到它。

## 如何使用杜松子酒

考虑以下函数:

```
def make_drink(drink_name:str, ingredients:list, ice:bool=False):
    print(f"New drink called: {drink_name}")
    print(f"Drink contains: " + ", ".join(ingredients))
    if ice is True:
        print("With ice")
    else:
        print("Without ice")
```

要使这个 Gin 可配置，我们需要做的就是给函数`[@gin](http://twitter.com/gin).configurable`添加一个简单的装饰器，它告诉库将这个函数索引为一个可以在`.gin`文件中配置的函数——如果你不知道什么是装饰器，我在下面链接了一篇解释它们如何工作的文章。

[](/level-up-your-code-with-python-decorators-c1966d78607) [## 用 Python decorators 提升你的代码

### 日志记录、类型检查、异常处理等等！

towardsdatascience.com](/level-up-your-code-with-python-decorators-c1966d78607) 

因此，让我们开始编写 gin 文件:

```
make_drink.drink_name = "G&T"
make_drink.ingredients = ["Gin", "Tonic", "Lemon"]
make_drink.ice = False
```

语法非常简单，第一部分是函数或类名，后面是一个点，然后是参数名。这种语法支持所有主要 Python 变量类型的赋值，正如你所料，这里我们有一个字符串、一个列表和一个布尔值，但同样也可以是一个字典、一个整数或一个元组。

所以现在我们的功能变成了:

```
import gin
gin.parse_config_file("path/to/gin_file.gin")[@gin](http://twitter.com/gin).configurable
def make_drink(drink_name:str, ingredients:list, ice:bool=False):
    print(f"New drink called: {drink_name}")
    print(f"Drink contains: " + ", ".join(ingredients))
    if ice is True:
        print("With ice")
    else:
        print("Without ice")
```

我们可以这样称呼它:

```
>>> make_drink()
New drink called: G&T
Drink contains:Gin, Tonic, Lemon
Without ice
```

如果你和我一样，这应该会让你感到烦恼。我们没有为任何参数定义任何默认值，所以对于任何阅读代码的人来说，不清楚这些值来自哪里！

Gin 的开发者看到了这一点，并提供了一个`gin.REQUIRED`值，可以用在参数的位置，以明确该值应该来自哪里。如果您未能在 Gin 文件中提供该值，这带来了自定义错误的额外好处。

```
>>> make_drink(gin.REQUIRED, gin.REQUIRED, ice=gin.REQUIRED)
New drink called: G&T
Drink contains:Gin, Tonic, Lemon
Without ice
```

现在让我们构建我们的代码并创建一个`Drink`类…

```
[@gin](http://twitter.com/gin).configurable
class Drink:
    def __init__(self, name, ingredients, ice=True):
        self.name = name
        self.ingredients = ingredients
        self.ice = ice[@gin](http://twitter.com/gin).configurable
def make_drink(drink: Drink):
    print(f"New drink called: {drink.name}")
    print(f"Drink contains: " + ", ".join(drink.ingredients))
    if drink.ice is True:
        print("With ice")
    else:
        print("Without ice")
    print("\n")
```

类和函数都是可配置的。Gin 很酷，所以我们可以对它进行配置，让`make_drink`接受`Drink`的一个实例，该实例本身接受参数值:

```
make_drink.drink = [@Drink](http://twitter.com/Drink)()
Drink.name = "Gin Martini"
Drink.ingredients = ["Gin", "Vermouth"]
Drink.ice = False
```

并称之为…

```
>>> make_drink(gin.REQUIRED)
New drink called: Gin Martini
Drink contains: Gin, Vermouth
Without ice
```

但是你知道什么比一杯酒更好…两杯酒…让我们考虑下面的例子…

```
[@gin](http://twitter.com/gin).configurable
def make_two_drinks(drink1: Drink, drink2:Drink):
    print("Making drink 1")
    make_drink(drink1)

    print("Making drink 2")
    make_drink(drink2)
```

两者都需要一个`Drink`的实例，你可以试着写类似这样的东西。

```
make_two_drinks.drink1 = [@Drink](http://twitter.com/Drink)()
Drink.name = "Gin Martini"
Drink.ingredients = ["Gin", "Vermouth"]
Drink.ice = Falsemake_two_drinks.drink2 = [@Drink](http://twitter.com/Drink)()
Drink.name = "Gin Fizz"
Drink.ingredients = ["Gin", "Sparkling Water"]
Drink.ice = True
```

但是这不会有预期的效果。

```
>>> make_two_drinks(gin.REQUIRED, gin.REQUIRED)
Making drink 1
New drink called: Gin Fizz
Drink contains: Gin, Sparkling Water
With ice

Making drink 2
New drink called: Gin Fizz
Drink contains: Gin, Sparkling Water
With ice
```

我们只要两杯杜松子汽酒！幸运的是，Gin 有一个作用域的概念，允许我们为两个输入定义不同的饮料。

```
make_two_drinks.drink1 = [@drink1/Drink](http://twitter.com/drink1/Drink)()
drink1/Drink.name = "Gin Martini"
drink1/Drink.ingredients = ["Gin", "Vermouth"]
drink1/Drink.ice = Falsemake_two_drinks.drink2 = [@drink2/Drink](http://twitter.com/drink2/Drink)()
drink2/Drink.name = "Gin Fizz"
drink2/Drink.ingredients = ["Gin", "Sparkling Water"]
drink2/Drink.ice = True
```

这正是我们想要的。

```
>>> make_two_drinks(gin.REQUIRED, gin.REQUIRED)
Making drink 1
New drink called: Gin Martini
Drink contains: Gin, Vermouth
Without ice

Making drink 2
New drink called: Gin Fizz
Drink contains: Gin, Sparkling Water
With ice
```

如果一个函数调用另一个函数，并且两者都在 Gin 中进行了配置，那么树中最上面的将优先。

```
make_two_drinks.drink1 = [@drink1/Drink](http://twitter.com/drink1/Drink)()
drink1/Drink.name = "Gin Martini"
drink1/Drink.ingredients = ["Gin", "Vermouth"]
drink1/Drink.ice = Falsemake_two_drinks.drink2 = [@drink2/Drink](http://twitter.com/drink2/Drink)()
drink2/Drink.name = "Gin Fizz"
drink2/Drink.ingredients = ["Gin", "Sparkling Water"]
drink2/Drink.ice = Truemake_drink.drink = [@Drink](http://twitter.com/Drink)()
Drink.name = "French 75"
Drink.ingredients = ["Gin", "Champagne"]
Drink.ice = False
```

会给我们…

```
>>> make_two_drinks(gin.REQUIRED, gin.REQUIRED)
Making drink 1
New drink called: Gin Martini
Drink contains: Gin, Vermouth
Without ice

Making drink 2
New drink called: Gin Fizz
Drink contains: Gin, Sparkling Water
With ice
```

正如我们所期望的…

```
>>> make_drink(gin.REQUIRED)
New drink called: French 75
Drink contains: Gin, Champagne
Without ice
```

正如我们所料。

这里我已经向您介绍了基本功能，Gin 包含了大量的其他特性，比如宏和以编程方式设置和检索配置参数的能力。

## 真实世界的例子

这是基础知识，接下来我们将看看这如何应用于现实世界的例子。我们将构建一个快速模型和一个训练循环，并展示如何使用 Gin 来配置它。

```
import torch.nn as nn
import torch.nn.functional as F[@gin](http://twitter.com/gin).configurable
class DNN(torch.nn.Module):
    def __init__(self, input_size, output_size, hidden_size):
        super(DNN, self).__init__()

        self.fc1 = nn.Linear(input_size, hidden_size)
        self.fc2 = nn.Linear(hidden_size, hidden_size)
        self.fc3 = nn.Linear(hidden_size, output_size) def forward(self, x):
        x = F.relu(self.fc1(x))
        x = F.relu(self.fc2(x))
        x = self.fc3(x)
        return x[@gin](http://twitter.com/gin).configurable
def train(model: torch.nn.Module, 
          optimizer: torch.optim.Optimizer, 
          loss_func: torch.nn.modules.loss,
          epochs: int,
          train_dataloader: torch.utils.data.DataLoader,
         ):
    opt = optimizer(model.parameters()) for epoch in range(epochs): for i, (inputs, labels) in enumerate(train_dataloader):
            opt.zero_grad() outputs = model(inputs)
            loss = loss_func(outputs, labels)
            loss.backward()
            opt.step() print('Finished Training')
```

我们可以像这样建立一个 Gin 文件…

```
train.epochs = 10
train.model = [@DNN](http://twitter.com/DNN)()
DNN.input_size = 10
DNN.output_size = 10
DNN.hidden_size = 128
```

这很好，但是优化器和损失函数呢？我们也能配置它们吗？Gin 允许你注册外部函数，这样我们就可以！

```
gin.external_configurable(torch.optim.Adam)
gin.external_configurable(torch.nn.CrossEntropyLoss)
```

Gin 内置了很多 TensorFlow 和 PyTorch 函数，所以你不需要为每件事都这样做，你需要做的只是导入`gin.tensorflow`和/或`gin.torch`。我们的新 Gin 文件看起来像这样…

```
train.epochs = 10
train.model = [@DNN](http://twitter.com/DNN)()
DNN.input_size = 10
DNN.output_size = 10
DNN.hidden_size = 128train.optimizer = [@Adam](http://twitter.com/Adam)
Adam.lr = 0.001
train.loss_func = [@CrossEntropyLoss](http://twitter.com/CrossEntropyLoss)()
```

请注意`Adam`和`CrossEntropyLoss()`之间的细微差别，Adam 需要用模型参数实例化，这发生在`fit`函数中，而`CrossEntropyLoss()`作为实例化的类传递。

## 这真的有什么好处吗？

简单的回答是肯定的。我认为它节省了很多配置项目的任务带来的麻烦。我认为配置带来的困难，总的来说，是知道你可能会做过头。在配置文件中隐藏得越多，代码的可读性就越差。Gin 开发者也意识到了这一点，并表示我们应该“负责任地使用 Gin”。另一件值得补充的事情是，学习 us Gin 和学习最佳实践会有一点开销，这样您的代码仍然是 Pythonic 化的和可读的。这对于学习使用 TOML 或 YAML 来说就不那么真实了。使用这些有自己的学习曲线，但我只是觉得有更少的可能出错。

我将补充说明这个观点:为了写这篇文章，我花了几个小时研究这个库的一些核心功能。在现实中，我可能应该在愤怒地将它用于现实世界的项目后形成一种正确的观点。但是，嘿，你不应该相信我的意见，你自己试试，让我知道你是怎么想的。

## 相关信息

1.  [GitHub 回购](https://github.com/google/gin-config)
2.  [杜松子酒漫游](https://github.com/google/gin-config/blob/master/docs/walkthrough.md)