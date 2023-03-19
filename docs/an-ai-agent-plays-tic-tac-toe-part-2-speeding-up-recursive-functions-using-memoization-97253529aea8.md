# 人工智能代理玩井字游戏(第 2 部分):使用记忆化加速递归函数

> 原文：<https://towardsdatascience.com/an-ai-agent-plays-tic-tac-toe-part-2-speeding-up-recursive-functions-using-memoization-97253529aea8?source=collection_archive---------19----------------------->

## *我们提高了蛮力树搜索的速度，使其在强化学习中更实用*

*本文是让计算机使用强化学习玩井字游戏系列的一部分。你可以在这里找到*<https://towardsdatascience.com/tagged/rl-series-paul>**的所有文章。我们的目标是提供一个完整的实现，您可以真正从中挑选并学习强化学习。按顺序阅读文章可能是最好的。文章包括所有代码* [*都可以在 Github*](https://github.com/PaulHiemstra/memoise_paper/blob/master/memoise_paper.ipynb) *上找到。**

*在本系列的第 1 部分中，我们实现了一个树搜索极大极小算法，作为我们的强化学习(RL)代理的对手。结论是，虽然它有效，但算法太慢，不能用于训练我们的 RL 代理。第 2 部分的目标是显著加快极大极小算法的速度。*

*![](img/8aa4c39508e2136b010184a4b412495a.png)*

*一个可能的解决策略是最小化树的大小。例如，井字游戏是全面对称的，所以我们可以彻底消除大约一半的树。然而，我选择让算法和树保持原样，并且更专注于用一种叫做[记忆化](https://youtu.be/P8Xa2BitN3I)的高级编程技术来解决这个问题。*

*一般的想法是，当一个函数被调用时，函数的结果被存储在一个字典中，其中的键等于函数调用参数。下一次使用这些参数调用函数时，字典中的结果将被简单地返回。在我们的例子中，这将减少从递归搜索树到在字典中查找值的最佳移动。*

*让我们首先将树加载回内存，并加载我们的 minimax 代码。请注意，github 存储库包含一个生成该树的 Python 脚本。*

*现在我们可以请求最小化玩家的下一步行动，假设最大化玩家已经开始了第`a`步:*

```
*2.9204039573669434*
```

*在我的机器上大约需要 3 秒钟。*

*在网上我找到了[下面的记忆实现](https://www.python-course.eu/python3_memoization.php)。它创建了一个记忆化类，我们可以用它来[修饰](https://www.datacamp.com/community/tutorials/decorators-python)我们的递归极大极小树搜索。这很好地将记忆功能与被调用来完成工作的实际函数分离开来。注意，我从字典中排除了第一个参数，以防止键变得太大，从而减慢记忆过程。*

*这种技术的工作方式类似于我在本文[中使用的函数操作风格](/advanced-functional-programming-for-data-science-building-code-architectures-with-function-dd989cc3b0da)，将核心函数与辅助函数分离开来。在这里，函数操作符是装饰类的一个很好的替代品。*

*有了记忆，我们可以测试它是否真的表现得更好。在下面的代码中，我们调用该函数两次，并计算有内存的版本要快多少:*

```
*49962.9296875*
```

*很好，这产生了大约 50k 倍的速度提升。最后一步是通过 minimax 函数强制所有可能的棋盘状态，用所有函数调用填满记忆缓冲区:*

*在我的机器上大约需要 30 秒。*

*由于预先计算了所有的树搜索，`determine_move`现在足够快，可以快速运行所需数量的井字游戏。在下一部分，我们将实现称为 Q-learning 的 RL 算法。*

# *我是谁？*

*我叫 Paul Hiemstra，是荷兰的一名教师和数据科学家。我是科学家和软件工程师的混合体，对与数据科学相关的一切都有广泛的兴趣。你可以在 medium 上关注我，或者在 LinkedIn 上关注我。*

*如果你喜欢这篇文章，你可能也会喜欢我的其他一些文章:*

*   *[没有像应用数据科学这样的数据科学](/there-is-no-data-science-like-applied-data-science-99b6c5308b5a)*
*   *[牛郎星图解构:可视化气象数据的关联结构](/altair-plot-deconstruction-visualizing-the-correlation-structure-of-weather-data-38fb5668c5b1)*
*   *[面向数据科学的高级函数式编程:使用函数运算符构建代码架构](/advanced-functional-programming-for-data-science-building-code-architectures-with-function-dd989cc3b0da)*