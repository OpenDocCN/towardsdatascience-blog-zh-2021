# 为什么坚实的原则不是软件设计的坚实的解决方案

> 原文：<https://towardsdatascience.com/why-solid-principles-are-not-a-solid-solution-for-software-design-fa71b6017ed6?source=collection_archive---------5----------------------->

## 软件世界

## 应用坚实的原则是一个目标，而不是命运

![](img/0281d01b058373ddc25adf8f3aad211a.png)

由[迈克尔·泽兹奇](https://unsplash.com/@lazycreekimages?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

obert J. Martin 在 2000 年介绍了坚实的原则，当时面向对象编程是程序员的艺术。每个人都想设计一些持久的东西，可以尽可能地重复使用，在未来只需要最小的改变。固体是它的完美名字。

事实上，当我们能够区分什么将会保留，什么将会改变时，面向对象编程工作得最好。坚实的原则有助于倡导这一点。

我个人喜欢坚实的原则背后的想法，并从中学到了很多。

## 然而…

有一个主要的挑战，所有的软件都意味着改变。要预测变化真的很难，如果不是不可能的话。因此，对我们来说，很难明确界定什么会保持不变，什么会改变。

为了说明这个挑战如何影响坚实的原则，让我们来看看每一个原则。

# 单一责任原则

> "一个[类](https://en.wikipedia.org/wiki/Class_(computer_programming))的改变不应该有一个以上的原因."换句话说，每个类应该只有一个责任。

让我们假设，我们的程序需要一个计算函数。这个班的唯一职责就是计算。

```
class Calculate {
   fun add(a, b) = a + b
   fun sub(a, b) = a - b
   fun mul(a, b) = a * b
   fun div(a, b) = a / b
}
```

对某些人来说，这是完美的，因为它只有一个责任，即计算。

但有人可能会反驳说:“嘿！它做 4 件事！加减乘除！”

谁是对的？我会说视情况而定。

如果你的程序只使用`Calculate`来执行计算，那么一切都好。进一步抽象化将会过度工程化。

但是谁知道呢，将来可能有人只想做加法运算而不需要通过`Calculate`类。那么上面需要改！

单一责任的定义取决于程序的上下文，并且会随着时间的推移而改变。我们现在可以实现这个原则，但不能永远实现。

# 开闭原理

> "软件实体…应该对扩展开放，但对修改关闭."

这个原则是一个理想主义的原则。它假设一旦一个类被编码，如果编码正确，就不需要再修改了。

让我们看看下面的代码

```
interface Operation {
   fun compute(v1: Int, v2: Int): Int
}class Add:Operation {
   override fun compute(v1: Int, v2: Int) = v1 + v2
}class Sub:Operation {
   override fun compute(v1: Int, v2: Int) = v1 - v2
}class Calculator {
   fun calculate(op: Operation, v1: Int, v2: Int): Int {
      return op.compute(v1, v2)
   } 
}
```

上面有一个接受操作对象进行计算的`Calculator`类。我们可以轻松地用 Mul 和 Div 操作进行扩展，而无需修改`Calculator`类。

```
class Mul:Operation {
   override fun compute(v1: Int, v2: Int) = v1 * v2
}class Div:Operation {
   override fun compute(v1: Int, v2: Int) = v1 / v2
}
```

很好，我们实现了开闭原则！

但是有一天，一个新的需求进来了，说他们需要一个叫逆的新操作。它只需要一个操作符，比如 X，然后返回 1/X 的结果。

到底谁能想到这个会进来？我们已经将操作界面的计算功能固定为 2 个参数。现在我们需要一个只有 1 个参数的新操作。

现在怎样才能避免修改计算器类？如果我们事先知道这一点，我们可能就不会这样编写计算器类和操作接口了。

变化永远不可能完全计划好。如果只能完全计划好，我们可能就不再需要软件了:)

# 利斯科夫替代原理

> "使用指向基类的指针或引用的函数必须能够在不知道的情况下使用派生类的对象."

当我们小的时候，我们学习动物的基本属性。他们是可移动的。

```
interface Animal {
   fun move()
}class Mammal: Animal {
   override move() = "walk"
}class Bird: Animal {
   override move() = "fly"
}class Fish: Animal {
   override move() = "swim"
}fun howItMove(animal: Animal) {
   animal.move()
}
```

这符合利斯科夫替代原理。

但是我们知道上面所说的并不正确。一些哺乳动物游泳，一些飞行，一些鸟类行走。所以我们改成

```
class WalkingAnimal: Animal {
   override move() = "walk"
}class FlyingAnimal: Animal {
   override move() = "fly"
}class SwimmingAnimal: Animal {
   override move() = "swim"
}
```

酷，一切还好，因为我们下面的功能还可以用动物。

```
fun howItMove(animal: Animal) {
   animal.move()
}
```

今天，我发现了一些事情。[有些动物根本不动](http://www.madsci.org/posts/archives/2003-03/1047050472.Gb.r.html)。它们被称为无柄的。也许我们应该换成

```
interface Animal interface MovingAnimal: Animal {
   move()
}class Sessile: Animal {}
```

现在，这将打破下面的代码。

```
fun howItMove(animal: Animal) {
   animal.move()
}
```

我们没有办法保证永远不会改变`howItMove`的功能。基于我们在那个时间点所了解的情况，我们可以做到这一点。但是当我们意识到新的需求时，我们需要改变。

即使在现实世界中，也已经有如此多的例外。软件世界不是真实的世界。一切皆有可能。

# 界面分离原理

> “许多特定于客户端的接口比一个通用接口要好。”

再来看看动物界。我们有一个动物界面如下。

```
interface Animal {
   fun move()
   fun eat()
   fun grow()
   fun reproduction()
}
```

但是，正如我们上面意识到的，有一些动物是不动的，这种动物叫做无柄动物。所以我们应该把`move`功能分离出来作为另一个接口

```
interface Animal {
   fun eat()
   fun grow()
   fun reproduction()
}interface MovingObject {
   fun move()
}class Sessile : Animal {
   //...
}class NonSessile : Animal, MovingObject {
   //...
}
```

然后我们也想有植物。也许我们应该把`grow`和`reproduction`分开

```
interface LivingObject {
   fun grow()
   fun reproduction()
}interface Plant: LivingObject {
   fun makeFood()
}interface Animal: LivingObject {
   fun eat()
}interface MovingObject {
   fun move()
}class Sessile : Animal {
   //...
}class NonSessile : Animal, MovingObject {
   //...
}
```

我们很高兴，因为我们尽可能多地分离出特定于客户端的接口。这看起来是一个理想的解决方案。

然而，有一天，有人哭了，“歧视！有些动物不育，并不意味着它们不再是有生命的物体！”。

看来我们现在必须将`reproduction`从`LivingObject`接口中分离出来。

如果我们这样做，我们实际上每个接口都有一个功能！它非常灵活，但如果我们不需要如此精细的分离，它可能会过于灵活。

我们应该把我们的接口分离得多好，取决于我们程序的上下文。不幸的是，我们节目的背景会不时改变。因此，我们也应该继续重构或分离我们的接口，以确保它仍然有意义。

# 依赖性倒置原则

> "依靠抽象，而不是具体."

这个原则是我喜欢的，因为它是相对普遍正确的。它应用了[独立依赖解决方案概念](/the-root-of-all-software-design-challenge-independent-or-dependent-31252051bf0e)的思想，其中软件实体虽然依赖于一个类，但仍然独立于它。

如果我们要严格地实践这一点，任何东西都不应该直接依赖于一个类。

让我们看看下面的例子。它确实应用了依赖性倒置原则。

```
interface Operation {
   fun compute(v1: Int, v2: Int): Int
   fun name(): String
}class Add:Operation {
   override fun compute(v1: Int, v2: Int) = v1 + v2
   override fun name() = "Add"
}class Sub:Operation {
   override fun compute(v1: Int, v2: Int) = v1 - v2
   override fun name() = "Subtract"
}class Calculator {
   fun calculate(op: Operation, v1: Int, v2: Int): Int {
      println("Running ${op.name()}")
      return op.compute(v1, v2)
   } 
}
```

`Calculator`不依赖于`Add`或`Sub`。但这使得`Add`和`Sub`反而依赖于`Operation`。看起来不错。

然而，如果有人从 Android 开发组使用它，它有一个问题。`println`在安卓系统中不工作。我们将需要`Lod.d`来代替。

要解决这个问题，我们还应该让`Calculator`不直接依赖于`println`。相反，我们应该注入一个打印接口

```
interface Printer {
   fun print(msg: String)
}class AndroidPrinter: Printer {
   override fun print(msg: String) = Log.d("TAG", msg)
}class NormalPrinter: Printer {
   override fun print(msg: String) = println(msg)
}class Calculator(**val printer: Printer**) {
   fun calculate(op: Operation, v1: Int, v2: Int): Int {
      **printer.print("Running ${op.name()}")**
      return op.compute(v1, v2)
   } 
}
```

这解决了问题，同时满足依赖性反转原则。

但是如果 Android 永远不需要使用这个`Calculator`，而我们提前创建了这样一个界面，我们可能已经违反了 [YAGNI](https://martinfowler.com/bliki/Yagni.html) 。

# TL；DR；

让我重申，对我来说，坚实的原则是软件设计解决方案的好原则，值得我们去追求。当我们在那个时间点很好地知道什么是固定的，什么可能会改变时，这尤其有用。

然而，变化的发生超出了我们的预料。当这种情况发生时，新的需求将使我们最初的设计不再坚持坚实的原则。

这是正常的。会发生的。我们只需要应用新的需求，并在时间点上再次改变它，重新巩固它。

软件的本质是软的，把它做实才是硬的。对于软件来说，应用坚实的原则是一个目标，而不是命运。