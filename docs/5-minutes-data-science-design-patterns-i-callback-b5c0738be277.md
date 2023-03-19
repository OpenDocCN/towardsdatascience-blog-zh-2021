# 5 分钟数据科学设计模式 I:回访

> 原文：<https://towardsdatascience.com/5-minutes-data-science-design-patterns-i-callback-b5c0738be277?source=collection_archive---------23----------------------->

## 数据科学设计模式的迷你集合——从回调开始

![](img/50e02b08e19968b9b17805cddece93ec.png)

[Unsplash](https://unsplash.com/photos/71CjSSB83Wo)[Pavan Trikutam](https://medium.com/u/1867b379dd15?source=post_page-----b5c0738be277--------------------------------)

*注意:这些系列是为数据科学家编写的软件设计快速入门，比设计模式圣经更轻量级的东西—* [*干净代码*](https://www.amazon.com/Clean-Code-Handbook-Software-Craftsmanship/dp/0132350882) *我希望在我刚开始学习时就存在。设计模式指的是一些常见问题的可重用解决方案，有些恰好对数据科学有用。很有可能其他人已经解决了你的问题。如果使用得当，它有助于降低代码的复杂性。*

由于代码块比较多，这里的[是语法高亮的版本。](https://noklam.github.io/blog/python/kedro/2021/07/10/3minutes-data-science-design-pattern-callback.html)

# 那么，回调到底是什么？

`Callback` function，或 call after，简单来说就是一个函数会在另一个函数之后被调用。它是作为参数传递给另一个函数的一段可执行代码(函数)。[【1】](https://stackoverflow.com/questions/824234/what-is-a-callback-function)

```
**def** **foo**(x, callback**=None**):
    print('foo!')
    **if** callback:
        callback(x)
    **return** **None**>>> foo('123')
foo!>>> foo('123', print)
foo!
123
```

这里我将函数`print`作为回调函数传递，因此字符串`123`在`foo!`之后被打印。

# 为什么我需要使用回调？

回调在高级深度学习库中非常常见，很可能你会在训练循环中找到它们。

*   fastai — fastai 为 PyTorch 提供高级 API
*   Keras—tensor flow 的高级 API
*   [点燃](https://github.com/pytorch/ignite) —使用事件&处理程序，这在他们看来提供了更多的灵活性

```
import numpy **as** np

*# A boring training Loop*
**def** **train**(x):
    n_epochs **=** 3
    n_batches **=** 2
    loss **=** 20

    **for** epoch **in** range(n_epochs):
        **for** batch **in** range(n_batches):
            loss **=** loss **-** 1 *# Pretend we are training the model*
    **return** loss>>> x **=** np**.**ones(10)
>>> train(x)
14
```

因此，假设您现在想要打印一个时期结束时的损失。您可以只添加一行代码。

# 简单的方法

```
**def** **train_with_print**(x):
    n_epochs **=** 3
    n_batches **=** 2
    loss **=** 20 **for** epoch **in** range(n_epochs):
        **for** batch **in** range(n_batches):
            loss **=** loss **-** 1 *# Pretend we are training the model*
        print(f'End of Epoch. Epoch: {epoch}, Loss: {loss}')
    **return** losstrain_with_print(x)End of Epoch. Epoch: 0, Loss: 18
End of Epoch. Epoch: 1, Loss: 16
End of Epoch. Epoch: 2, Loss: 14
```

# 回调方法

或者您调用来添加一个 **PrintCallback** ，它做同样的事情，但是用了更多的代码。

```
**class** **Callback**:
    **def** **on_epoch_start**(self, x):
        **pass** **def** **on_epoch_end**(self, x):
        **pass** **def** **on_batch_start**(self, x):
        **pass** **def** **on_batch_end**(self, x):
        **pass** **class** **PrintCallback**(Callback):
    **def** **on_epoch_end**(self, x):
        print(f'End of Epoch. Epoch: {epoch}, Loss: {x}')**def** **train_with_callback**(x, callback**=None**):
    n_epochs **=** 3
    n_batches **=** 2
    loss **=** 20 **for** epoch **in** range(n_epochs): callback**.**on_epoch_start(loss) **for** batch **in** range(n_batches):
            callback**.**on_batch_start(loss)
            loss **=** loss **-** 1  *# Pretend we are training the model*
            callback**.**on_batch_end(loss) callback**.**on_epoch_end(loss)
    **return** loss>>> train_with_callback(x, callback**=**PrintCallback())End of Epoch. Epoch: 2, Loss: 18
End of Epoch. Epoch: 2, Loss: 16
End of Epoch. Epoch: 2, Loss: 14
```

通常，一个回调定义了几个特定的事件`on_xxx_xxx`，表示函数将根据相应的条件执行。所以所有回调都将继承基类`Callback`，并覆盖期望的函数，这里我们只实现了`on_epoch_end`方法，因为我们只想在最后显示损失。

为做一件简单的事情而写这么多代码似乎有些笨拙，但是有很好的理由。考虑现在你需要添加更多的功能，你会怎么做？

*   模型检查点
*   提前停止
*   学习率计划程序

你可以只在循环中添加代码，但是它将开始成长为一个真正的大函数。测试这个功能是不可能的，因为它同时做 10 件事。此外，额外的代码甚至可能与训练逻辑无关，它们只是用来保存模型或绘制图表。所以，最好把逻辑分开，一个功能按照[单一责任原则](https://en.wikipedia.org/wiki/SOLID)应该只做 1 件事。这有助于你降低复杂性，因为你不需要担心你会不小心打碎 10 件东西，一次只考虑一件事情会容易得多。

当使用回调模式时，我可以多实现几个类，而训练循环几乎没有被触及。我不得不改变训练函数一点，因为它应该接受 1 个以上的回调。

包装回调列表的**回调**类

```
**class** **Callbacks**:
    """
    It is the container for callback
    """

    **def** __init__(self, callbacks):
        self**.**callbacks **=** callbacks

    **def** **on_epoch_start**(self, x):
        **for** callback **in** self**.**callbacks:
            callback**.**on_epoch_start(x)

    **def** **on_epoch_end**(self, x):
        **for** callback **in** self**.**callbacks:
            callback**.**on_epoch_end(x)

    **def** **on_batch_start**(self, x):
        **for** callback **in** self**.**callbacks:
            callback**.**on_batch_start(x)

    **def** **on_batch_end**(self, x):
        **for** callback **in** self**.**callbacks:
            callback**.**on_batch_end(x)
```

附加回调的伪实现

```
 **class** **PrintCallback**(Callback):
    **def** **on_epoch_end**(self, x):
        print(f'[{type(self)**.**__name__}]: End of Epoch. Epoch: {epoch}, Loss: {x}')

**class** **ModelCheckPoint**(Callback):
    **def** **on_epoch_end**(self, x):
        print(f'[{type(self)**.**__name__}]: Save Model')

**class** **EarlyStoppingCallback**(Callback):
    **def** **on_epoch_end**(self, x):
        **if** loss **<** 3:
            print(f'[{type(self)**.**__name__}]: Early Stopped')

**class** **LearningRateScheduler**(Callback):
    **def** **on_batch_end**(self, x):
        print(f'    [{type(self)**.**__name__}]: Reduce learning rate')

**def** **train_with_callbacks**(x, callbacks**=None**):
    n_epochs **=** 3
    n_batches **=** 6
    loss **=** 20

    **for** epoch **in** range(n_epochs):

        callbacks**.**on_epoch_start(loss)                             *# on_epoch_start*
        **for** batch **in** range(n_batches):
            callbacks**.**on_batch_start(loss)                         *# on_batch_start*
            loss **=** loss **-** 1  *# Pretend we are training the model*
            callbacks**.**on_batch_end(loss)                           *# on_batch_end*
        callbacks**.**on_epoch_end(loss)                               *# on_epoch_end*
    **return** loss
```

这是结果。

```
>>> callbacks **=** Callbacks([PrintCallback(), ModelCheckPoint(),
                      EarlyStoppingCallback(), LearningRateScheduler()])
>>> train_with_callbacks(x, callbacks**=**callbacks)[LearningRateScheduler]: Reduce learning rate
    [LearningRateScheduler]: Reduce learning rate
    [LearningRateScheduler]: Reduce learning rate
    [LearningRateScheduler]: Reduce learning rate
    [LearningRateScheduler]: Reduce learning rate
    [LearningRateScheduler]: Reduce learning rate
[PrintCallback]: End of Epoch. Epoch: 2, Loss: 14
[ModelCheckPoint]: Save Model
    [LearningRateScheduler]: Reduce learning rate
    [LearningRateScheduler]: Reduce learning rate
    [LearningRateScheduler]: Reduce learning rate
    [LearningRateScheduler]: Reduce learning rate
    [LearningRateScheduler]: Reduce learning rate
    [LearningRateScheduler]: Reduce learning rate
[PrintCallback]: End of Epoch. Epoch: 2, Loss: 8
[ModelCheckPoint]: Save Model
    [LearningRateScheduler]: Reduce learning rate
    [LearningRateScheduler]: Reduce learning rate
    [LearningRateScheduler]: Reduce learning rate
    [LearningRateScheduler]: Reduce learning rate
    [LearningRateScheduler]: Reduce learning rate
    [LearningRateScheduler]: Reduce learning rate
[PrintCallback]: End of Epoch. Epoch: 2, Loss: 2
[ModelCheckPoint]: Save Model
```

希望它能让你相信回调使代码更干净，更容易维护。如果你只是使用简单的`if-else`语句，你可能会得到一大块`if-else`从句。

# 参考

1.  [https://stack overflow . com/questions/824234/what-a-callback-function](https://stackoverflow.com/questions/824234/what-is-a-callback-function)