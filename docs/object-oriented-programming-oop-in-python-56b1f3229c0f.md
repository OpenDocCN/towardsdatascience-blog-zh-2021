# Python 中的面向对象编程(OOP)

> 原文：<https://towardsdatascience.com/object-oriented-programming-oop-in-python-56b1f3229c0f?source=collection_archive---------15----------------------->

## 编程；编排

## 揭开类、对象、继承等等的神秘面纱

![](img/d86cd1b62cbf3fe00659ca231ccef962.png)

来源:[Undraw.co](http://Undraw.co)

# OOP 是什么？

面向对象编程是一种通过将相关的属性和行为分组到单个对象中来组织程序的方法。OOP 的基本构建模块是**对象**和**类**。

一个**类**是一个创建对象的代码模板，我们可以把它想象成一个蓝图。它描述了某种类型的每个对象可能具有的状态和行为。例如，如果我们说“每个雇员都有名字和薪水，并且可以加薪”，那么我们就定义了一个类！

**对象**是存储关于某个实体的状态和行为的信息的数据结构，并且是一个类的实例。例如，一个代表雇员的对象可以有一些相关的属性，比如薪水和职位，以及加薪等行为。

> 对象=状态+行为

关于对象状态的信息包含在**属性**中，行为信息包含在**方法**中。此外，对象属性或状态由变量表示，如数字、字符串或元组。而对象方法或行为由函数来表示。

OOP 的显著特征是状态和行为捆绑在一起。这意味着，例如，我们不是将雇员数据与雇员行为分开考虑，而是将它们视为一个代表雇员的单元。这被称为**封装**，是面向对象编程的核心原则之一。

# 定义类别

如前所述，类是创建对象的蓝图。现在，让我们把我们的第一个蓝图变成现实。

```
class Employee:
   pass
```

`Employee`类现在没有太多的功能。我们将从添加所有`Employee`对象应该具有的一些属性开始。为了简单起见，我们只添加姓名和薪水属性。

## 将属性分配给类

所有`Employee`对象必须具有的属性在一个名为`.__init__()`的方法中定义，或者在**构造器方法**中定义。每次创建一个新的`Employee`对象时，构造函数方法都会被自动调用。

让我们用一个创建`.name`和`.salary`属性的`.__init__()`方法来更新`Employee`类:

```
class Employee:
    def __init__(self, name, salary=0):
        self.name = name
        self.salary = salary
```

注意`self`在任何方法定义中都被用作第一个参数，包括我们的构造函数方法。另外，`self.name = name`创建了一个名为`name`的属性，并给它分配了参数`name`的值。对于 salary 属性也是如此，只是我们将默认的 salary 设置为 0。

## 实例属性与类属性

在`.__init__()`中创建的属性被称为**实例属性**。实例属性的值特定于类的特定实例。所有的`Employee`对象都有一个名字和一份薪水，但是`name`和`salary`属性的值会根据`Employee`实例的不同而不同。

另一方面，**类属性**是对所有类实例具有相同值的属性。您可以通过在`.__init__()`之外给变量名赋值来定义一个类属性，如下所示:

```
class Employee:
   #Class attribute
   organization = "xxx" def __init__(self, name, salary=0):
        self.name = name
        self.salary = salary
```

总而言之，类属性用于定义每个类实例应该具有相同值的特征，而实例属性用于定义不同实例的不同属性。

# 将方法分配给类

## **实例方法**

这些函数是在类内部定义的，只能从该类的实例中调用。就像构造函数方法一样，实例方法的第一个参数总是`self`。让我们演示如何通过构建前面的`Employee`类示例来编写实例方法。

```
class Employee:
   organization = "xxx" def __init__(self, name, salary=0):
        self.name = name
        self.salary = salary #Instance method
    def give_raise(self, amount):
        self.salary += amount
        return f"{self.name} has been given a {amount} raise"
```

如您所见，实例方法类似于常规函数，不同之处在于将`self`作为第一个参数。

## 类方法

类方法是绑定到类的方法，而不是类的对象。它们可以访问类的状态，因为它接受一个类参数，而不是典型的`self`，它指向类而不是对象实例。

要定义一个类方法，首先要有一个 class method 装饰器，然后是一个方法定义。唯一的区别是，现在第一个参数不是`self`，而是`cls`，引用类，就像 self 参数引用特定的实例一样。然后你把它写成任何其他函数，记住你不能在那个方法中引用任何实例属性。

因为类方法只能访问这个`cls`参数，所以它不能修改对象实例状态。这需要访问`self`。但是，类方法仍然可以修改应用于该类所有实例的类状态。

```
class MyClass: # instance method 
   def method(self):
        return 'instance method called', self

    @classmethod
    def classmethod(cls):
        return 'class method called', cls
```

class 方法的思想与 instance 方法非常相似，唯一的区别是，我们现在不是将实例作为第一个参数传递，而是将类本身作为第一个参数传递。因为我们只向方法传递一个类，所以不涉及实例。这意味着我们根本不需要实例，我们调用类方法就像调用静态函数一样:

```
MyClass.classmethod()
```

然而，如果我们想调用实例方法，我们必须先实例化一个对象，然后调用函数，如下所示:

```
object = MyClass()object.method()
```

如果您不确定实例化一个对象意味着什么，我们将在接下来讨论这个问题。

# 实例化一个对象

从一个类创建一个新对象叫做**实例化**一个对象。我们可以用如下属性实例化新的`Employee`对象:

```
e1 = Employee("yyy", 5000)
e2 = Employee("zzz", 8000)
```

您可以使用点符号访问属性，如下所示:

```
# access first employee's name attribute
e1.name# access second employee's salary attribute
e2.salary
```

# 类继承

类继承是一个类继承另一个类的属性和方法的机制。新形成的类称为**子类**，子类派生的类称为**父类**。子类拥有所有的父数据。

父类的属性和方法可以被子类覆盖或扩展。换句话说，子类继承其父类的所有属性和方法，但是它们也可以定义自己的属性和方法。

声明一个继承自父类的子类非常简单。

```
class Manager(Employee):
   pass
```

现在，即使我们没有定义构造函数，我们也可以创建一个`Manager`对象。

```
m1 = Manager("aaa", 13000)
```

## 通过继承定制功能

假设我们想给子类添加额外的属性。通过专门为我们的子类定制构造函数并调用父类的构造函数，我们可以很容易地做到这一点。

```
class Manager(Employee): def __init__(self, name, salary=0, department):
        Employee.__init__(self, name, salary=0)
        self.department = department
```

# 最佳实践

在使用类和对象时，有一些准则需要记住。

*   要命名你的类，使用 camel case，这意味着如果你的类名包含几个单词，它们应该没有分隔符，每个单词应该以大写字母开头。
*   对于方法和属性，情况正好相反——单词应该用下划线隔开，并以小写字母开头。
*   “自我”这个名字是约定俗成的。你实际上可以为一个方法的第一个变量使用任何名字，不管怎样，它总是被当作对象引用。尽管如此，最好还是坚持使用`self`。
*   不要忘记为你的类编写 docstrings，这样你的代码对潜在的合作者和未来的你来说更容易理解。

# 结论

总之，我们讨论了什么是 OOP、类和对象。我们还讨论了实例方法和类方法之间的区别，对于实例属性和类属性也是如此。我们还简要介绍了什么是类继承，以及使用类时的一些最佳实践。

就这样，我们找到了我们的向导。我希望你觉得这篇文章很有见地！如果你有，那么你可能会发现这些也很有趣。一如既往，我很乐意听到您的任何意见或问题。快乐学习！

[](/working-with-datetime-in-python-e032b8d2f512) [## 在 Python 中使用日期时间

### 立刻成为日期和时间的主人

towardsdatascience.com](/working-with-datetime-in-python-e032b8d2f512) [](/a-machine-learning-approach-to-credit-risk-assessment-ba8eda1cd11f) [## 信用风险评估的机器学习方法

### 预测贷款违约及其概率

towardsdatascience.com](/a-machine-learning-approach-to-credit-risk-assessment-ba8eda1cd11f)