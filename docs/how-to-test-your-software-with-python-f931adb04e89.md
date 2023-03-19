# 如何用 Python 测试你的软件

> 原文：<https://towardsdatascience.com/how-to-test-your-software-with-python-f931adb04e89?source=collection_archive---------31----------------------->

## Python 中测试模块的概述

![](img/b0878e3240ceec6e1670dea92d46b8e4.png)

(src =[https://pixabay.com/images/id-2521144/](https://pixabay.com/images/id-2521144/)

# 介绍

不管你写的是什么样的软件，在开发过程中总会遇到一些问题和错误。不管你的软件写得有多好，或者你碰巧是个多么优秀的程序员，这都很重要。幸运的是，有一些方法可以让我们通过测试来发现软件中的错误和问题。

测试是解决软件缺陷和问题的行业标准技术。测试不仅对于修复错误很重要，对于监控软件中隐含的变化也很重要。有时，依赖的方法或类型可能会被意外地完全改变，测试可以让你在最终将你的软件推向精通之前意识到这一点。今天，我想回顾一下 Python 编程语言的测试基础，这样你就可以放心地编写代码了！另外，对于那些也喜欢用 Julia 编程或者只是好奇的人，我也有另一篇用 Julia 做同样事情的文章！：

[](/how-to-test-your-software-with-julia-4050379a9f3) [## 如何用 Julia 测试你的软件

towardsdatascience.com](/how-to-test-your-software-with-julia-4050379a9f3) 

# 单元测试与集成测试

在开始实际测试我们的 Python 服务器之前，让我们快速回顾一下 Python 中常见的两种测试技术之间的区别。单元测试用于测试软件内部的小组件。另一方面，集成测试用于测试相互依赖的组件。

为了阐明这个观点，让我们考虑一个类比。我们有一栋房子，灯泡坏了。我们可以通过扳动电灯开关来测试灯是否熄灭。这将被认为是一个单元测试。然而，如果我们要测试这盏灯所依赖的电气系统，它将被视为一个集成测试。集成测试同时测试多个组件。

# 测试

在 Python 中，相当流行的方法是断言测试。断言测试使用条件来提供我们遇到问题时的抛出。我们可以使用许多方法来断言，考虑下面的例子:

```
def test_sum():
    assert sum([1, 2, 3]) == 6, "Should be 6"

if __name__ == "__main__":
    test_sum()
    print("Everything passed")
```

这将抛出一个错误。对于后续测试，当在执行过程中抛出错误时，后续测试将不会运行。记住，我们可以通过使用 Python 中的 unittest 模块来解决这样的问题。这通常是通过构建单元测试的测试子类来完成的。测试用例类。我们可以这样写这个类:

```
**import** **unittest**

**class** **TestStringMethods**(unittest.TestCase):
```

现在我们将我们的每个测试方法添加到这个类中:

```
**def** test_upper(self):
        self.assertEqual('foo'.upper(), 'FOO')

    **def** test_isupper(self):
        self.assertTrue('FOO'.isupper())
        self.assertFalse('Foo'.isupper())

    **def** test_split(self):
        s = 'hello world'
        self.assertEqual(s.split(), ['hello', 'world'])
        *# check that s.split fails when the separator is not a string*
        **with** self.assertRaises(TypeError):
            s.split(2)
```

最后，我们需要调用 unittest.main()。这个函数在我们的模块内部运行 TestCase 类型的所有子类，并调用它们的所有方法。这将执行测试，并给我们一些可爱的输出，显示我们的测试是如何执行的，同时也运行所有的测试，不管失败与否。

# 结论

测试是软件工程过程中的一个重要步骤。尽管有时这看起来是额外的工作，但它对于确保您的软件按照预期的方式工作并在未来工作是至关重要的。它还可以用来监控我们软件的隐含变化。我还想说，我真的很喜欢 Python 的测试实现。我认为 unittesting 模块在测试集上加入面向对象的元素是很酷的。这是一个很酷的方法，尽管我仍然会说我更喜欢 Julia 的测试，原因纯粹是主观的——这是一个很好的实现！感谢您的阅读，祝您编码愉快！