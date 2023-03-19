# Python 中两个必须知道的 OOP 概念

> 原文：<https://towardsdatascience.com/2-must-know-oop-concepts-in-python-48d643a7385?source=collection_archive---------0----------------------->

## 遗传和多态性

![](img/eee862480d235f2c011a5e433f151045.png)

在 [Unsplash](https://unsplash.com/s/photos/two?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上由[zdenk macha ek](https://unsplash.com/@zmachacek?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)拍摄的照片

面向对象编程(OOP)范式是围绕拥有属于特定类型的对象的思想而构建的。在某种意义上，类型是向我们解释对象的东西。

Python 中的一切都是对象，每个对象都有一个类型。这些类型是使用[类](/a-comprehensive-guide-for-classes-in-python-e6bb72a25a5e)声明和定义的。因此，类可以被认为是 OOP 的核心。

为了用 Python 开发健壮的、设计良好的软件产品，全面理解 OOP 是必不可少的。在本文中，我们将详细阐述 OOP 的两个关键概念，即继承和多态。

继承和多态都是设计健壮、灵活和易于维护的软件的关键因素。这些概念最好通过例子来解释。让我们从创建一个简单的类开始。

```
class Employee(): def __init__(self, emp_id, salary):
      self.emp_id = emp_id
      self.salary = salary def give_raise(self):
      self.salary = self.salary * 1.05
```

我们创建了一个名为 Employee 的类。它有两个数据属性，即雇员 id (emp_id)和薪金。我们还定义了一个名为 give_raise 的方法。它将雇员的工资提高了 5%。

我们可以创建 Employee 类的一个实例(即一个 Employee 类型的对象),并如下应用 give_raise 方法:

```
emp1 = Employee(1001, 56000)print(emp1.salary)
56000emp1.give_raise()print(emp1.salary)
58800.0
```

OOP 允许我们基于另一个类创建一个类。例如，我们可以基于 Employee 类创建 Manager 类。

```
class Manager(Employee):
   pass
```

在这个场景中，Manager 是 Employee 类的一个子类。子类从父类复制属性(数据和程序属性)。这个概念叫做**继承。**

需要注意的是，继承并不意味着复制一个类。我们可以部分继承父类(或基类)。Python 还允许添加新属性以及修改现有属性。因此，继承带来了很大的灵活性。

我们现在可以创建一个经理对象，就像创建一个雇员对象一样。

```
mgr1 = Manager(101, 75000)
print(mgr1.salary)
75000
```

除了从父类继承的属性之外，子类还可以有新的属性。此外，我们可以选择修改或覆盖继承的属性。

让我们更新 give_raise 方法，以便它为经理应用 10%的加薪。

```
class Manager(Employee): def give_raise(self):
      self.salary = self.salary * 1.10mgr1 = Manager(101, 75000)
print(mgr1.salary)
75000mgr1.give_raise()
print(mgr1.salary)
82500
```

我们将创建 Employee 类的另一个子类。Director 类继承了 Employee 类的属性，并用 20%的增量修改了 give_raise 方法。

```
class Director(Employee): def give_raise(self):
     self.salary = self.salary * 1.20
```

我们现在有三个不同类，它们都有一个 give_raise 方法。虽然方法的名称是相同的，但是对于不同类型的对象，它的行为是不同的。这是一个**多态性**的例子。

多态允许对不同的底层操作使用相同的接口。关于我们的 manager 和 director 对象的例子，我们可以像使用 employee 类的实例一样使用它们。

让我们看看多态性的作用。我们将定义一个将加薪应用于雇员列表的函数。

```
def bulk_raise(list_of_emps):
   for emp in list_of_emps:
      emp.give_raise()
```

bulk_raise 函数获取雇员列表，并将 give_raise 函数应用于列表中的每个对象。下一步是创建不同类型的员工列表。

```
emp1 = Employee(101, 45000)
emp2 = Manager(103, 60000)
emp3 = Director(105, 70000)list_of_emps = [emp1, emp2, emp3]
```

我们的列表包含一个雇员、一个经理和一个董事对象。我们现在可以调用 bulk_raise 函数。

```
bulk_raise(list_of_emps)print(emp1.salary)
47250.0print(emp2.salary)
66000.0print(emp3.salary)
84000.0
```

尽管列表中的每个对象都有不同的类型，但我们不必在函数中显式地声明它。Python 知道应用哪个 give_raise 方法。

正如我们在例子中看到的，多态性是通过继承实现的。子类(或子类)利用父类的方法来建立多态性。

## 结论

继承和多态都是面向对象编程的基本概念。这些概念帮助我们创建可扩展且易于维护的代码。

继承是消除不必要的重复代码的好方法。子类可以部分或全部继承父类。Python 在继承方面非常灵活。我们可以添加新的属性和方法以及修改现有的属性和方法。

多态性也有助于 Python 的灵活性。具有特定类型的对象可以像属于不同类型一样使用。我们已经看到了一个使用 give_raise 方法的例子。

感谢您的阅读。如果您有任何反馈，请告诉我。