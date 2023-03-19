# Python 中静态方法和类方法有什么区别？

> 原文：<https://towardsdatascience.com/whats-the-difference-between-static-and-class-methods-in-python-1ef581de4351?source=collection_archive---------13----------------------->

## 关于 classmethod 和 staticmethod，您只需要知道

![](img/46edc37871326f9e6b36d6b44cd6bfe0.png)

照片由[克里斯·利维拉尼](https://unsplash.com/@chrisliverani)在[unsplash.com](https://unsplash.com/photos/iRxZSVvombU)拍摄

除了实例方法——这是面向对象编程环境中最常见的类成员 Python 中的类也可以有静态和类方法。该语言带有两个装饰器，即`@staticmethod`和`@classmethod`，它们允许我们在类中定义这样的成员。理解这些概念是很重要的，因为它们将帮助你编写更清晰、结构更合理的面向对象的 Python，并最终使维护变得更容易。

在本文中，我们将探讨这两者的作用，如何创建它们，以及在哪些情况下应该使用其中一个。

花点时间浏览一下下面的代码，因为它将被用作示例 Python 类，我们将使用它来探索一些概念。

```
class Employee: NO_OF_EMPLOYEES = 0

    def __init__(self, first_name, last_name, salary):
        self.first_name = first_name
        self.last_name = last_name
        self.salary = salary
        self.increment_employees() def give_raise(self, amount):
        self.salary += amount @classmethod
    def employee_from_full_name(cls, full_name, salary):
        split_name = full_name.split(' ')
        first_name = split_name[0]
        last_name = split_name[1]
        return cls(first_name, last_name, salary) @classmethod
    def increment_employees(cls):
        cls.NO_OF_EMPLOYEES += 1 @staticmethod
    def get_employee_legal_obligations_txt():
        legal_obligations = """
        1\. An employee must complete 8 hours per working day
        2\. ...
        """
        return legal_obligations
```

## 类方法(@classmethod)

类方法接受类本身作为一个*隐式参数*和——可选地——定义中指定的任何其他参数。理解一个类方法**不能访问对象实例(像实例方法一样)是很重要的。因此，类方法不能用于改变实例化对象的状态，而是能够改变该类的所有实例共享的类状态。**

当我们需要访问类本身时，类方法通常是有用的——例如，当我们想要创建一个工厂方法时，这是一个创建类实例的方法。换句话说，classmethods 可以作为可选的构造函数。

在我们的示例代码中，`Employee`的一个实例可以通过提供三个参数来构造；`first_name`、`last_name`和`salary`。

```
employee_1 = Employee('Andrew', 'Brown', 85000)
print(employee_1.first_name)
print(employee_1.salary)'Andrew'
85000
```

现在让我们假设一个雇员的名字可能出现在一个字段中，其中名和姓用空格隔开。在这种情况下，我们可以使用名为`employee_from_full_name`的类方法，它总共接受三个参数。第一个是类本身，这是一个隐式参数，意味着在调用方法时不会提供它——Python 会自动为我们做这件事。

```
employee_2 = Employee.employee_from_full_name('John Black', 95000)
print(employee_2.first_name)
print(employee_2.salary)'John'
95000
```

注意，也可以从对象实例中调用`employee_from_full_name`,尽管在这种情况下没有太大意义:

```
employee_1 = Employee('Andrew', 'Brown', 85000)
employee_2 = employee_1.employee_from_full_name('John Black', 95000)
```

我们可能想要创建类方法的另一个原因是当我们需要改变类的状态时。在我们的例子中，类变量`NO_OF_EMPLOYEES`跟踪当前为公司工作的雇员人数。每次创建一个新的`Employee`实例时都会调用这个方法，并相应地更新计数:

```
employee_1 = Employee('Andrew', 'Brown', 85000)
print(f'Number of employees: {Employee.NO_OF_EMPLOYEES}')employee_2 = Employee.employee_from_full_name('John Black', 95000)
print(f'Number of employees: {Employee.NO_OF_EMPLOYEES}')Number of employees: 1
Number of employees: 2
```

## 静态方法(@staticmethod)

另一方面，在静态方法中，实例(即`self`)和类本身(即`cls`)都不会作为隐式参数传递。这意味着这些方法不能访问类本身或它的实例。

现在有人可能会说，静态方法在类的上下文中没有用，因为它们也可以放在助手模块中，而不是作为类的成员添加。在面向对象编程中，将类组织成逻辑块是很重要的，因此，当我们需要在类**下添加方法时，静态方法非常有用，因为它在逻辑上属于类**。

在我们的例子中，名为`get_employee_legal_obligations_txt`的静态方法简单地返回一个字符串，该字符串包含公司每个员工的法律义务。这个函数既不与类本身交互，也不与任何实例交互。它可以被放入一个不同的助手模块，但是，它只与这个类相关，因此我们必须把它放在`Employee`类下。

静态方法可以直接从类中访问

```
print(Employee.get_employee_legal_obligations_txt()) 1\. An employee must complete 8 hours per working day
    2\. ...
```

或者来自类的实例:

```
employee_1 = Employee('Andrew', 'Brown', 85000)
print(employee_1.get_employee_legal_obligations_txt()) 1\. An employee must complete 8 hours per working day
    2\. ...
```

## 结论

在本文中，我们介绍了 Python 中静态方法和类方法的工作原理，以及应该优先使用这两种方法的主要原因。

可以使用`@classmethod`装饰器创建类方法，它们用于访问类本身，但同时，它们不能访问单个实例。当我们需要创建可选的构造函数时，它们是非常有用的，这是一个创建同一个类的实例的类方法(可能接受稍微不同的参数)。

另一方面，静态方法可以通过`@staticmethod`装饰器来构造，但是它不同于实例或类方法，它没有任何隐式参数(既不是`self`也不是`cls`)，因此它们不能访问类或它的实例。当我们需要将一个成员放入一个类中时，它们通常是有用的，因为它在逻辑上属于这个类。

这两种类型的方法都可以帮助您编写易于阅读和维护的代码。