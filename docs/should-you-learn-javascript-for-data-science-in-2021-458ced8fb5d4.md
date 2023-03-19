# 2021 年数据科学该不该学 JavaScript？

> 原文：<https://towardsdatascience.com/should-you-learn-javascript-for-data-science-in-2021-458ced8fb5d4?source=collection_archive---------15----------------------->

## Javascript 是不是会永远改变 DS/ML 的语言？

![](img/fb558bf7daaaeefbf629d00fee2cbd84.png)

(src =[https://pixabay.com/images/id-4523100/](https://pixabay.com/images/id-4523100/)

# 介绍

早在 2018 年，Tensorflow 就在其支持的语言列表中引入了 JavaScript 接口。我认为这对于许多数据科学社区来说是一个相当大的惊喜，但是这个决定肯定是有意义的。JavaScript 是一种非常流行的编程语言，使用它进行开发的人肯定希望利用 Tensorflow 库所提供的优势。当然，机器学习根本不是 JavaScript 的应用，所以有一个问题是为什么会出现这种实现。

JavaScript 在机器学习生态系统中的位置是什么？在接下来的几年里，我们有可能发现自己在 Python 上编写 JavaScript 吗？这些只是我想在这篇文章中触及并试图展开对话的一些问题。在数据科学中，了解该领域的最新技术总是很重要的，即使看起来不像，JavaScript 也不例外。所以我想讨论使用 JavaScript 进行机器学习的一些缺点，并讨论它对整个数据科学的潜在影响。

# JavaScript 的潜力

JavaScript 在 web 开发方面的能力是众所周知的，但是这些能力如何应用到数据科学中呢？首先，我们应该考虑到 JavaScript 不是一种统计编程语言。这种语言很大程度上是为 web 开发而创建的，而这正是这种语言所擅长的。这意味着从典型统计学家的角度来看，JavaScript 可能有点难以理解。然而，Python 过去也是这种情况。Python 本身从未打算在机器学习领域引领世界，但由于它总体上是一门伟大的语言，所以占据了这个位置。JavaScript 有可能占据这个位置吗？

JavaScript 当然有潜力，但在我们认为它是机器学习领域的亚军之前，有一些关键的事情需要考虑。第一件事是 JavaScript 有一个很好的网络开发生态系统。虽然 Python 有很多处理数据、线性代数和机器学习的优秀包，但 JavaScript 却不是这样。在这方面，JavaScript 需要很长时间才能赶上。

另一个要考虑的是 JavaScript 很慢。像 Python 一样，JavaScript 是一种解释型编程语言，但也有一些缺点。然而，当谈到可以使用 C 编写大部分后端代码的语言时，我们发现它们的速度往往不如在其他语言中那样重要。记住，虽然从 Python 到 JavaScript 可能会降低速度，但在 Tensorflow 的例子中，最终可能会运行完全相同的代码。

# 缩小差距

Tensorflow.js 真正酷的地方在于，它显示了某种弥合我们和 web 开发团队之间差距的主动性。大多数利用机器学习的全栈应用程序通常向外部 API 发出请求，该 API 从用其他语言(如 Python)编写的模型中返回预测。这意味着 web 开发团队通常依赖端点来查询，以便为他们的全栈应用程序获取输入数据。

如果能看到一个内部利用 Tensorflow 的全栈 web 应用程序，那就太棒了。当然，即使在全栈的世界中，前端和后端之间通常也是分离的，但是如果能看到这一切都在 JavaScript 中完成，那就太酷了。我认为这肯定是一个很酷的概念，将所有东西整合到一种编程语言中。这当然有利于数据的传输，因为在某些情况下，用 JSON 数据正确地传递数据类型是非常困难的。当我们考虑 JavaScript 的类型和其他语言的类型时，这一点更是如此。

# 科学程序员和 JavaScript

JavaScript 是否会在机器学习领域扮演重要角色的问题是采用。科学程序员对 JavaScript 感兴趣吗？有几个原因让我不太喜欢实际使用 JavaScript，尽管我很喜欢这种语言本身和它所取得的成就。当然，这些都是主观的，根据程序员的不同，这些特性中的一些可能是使用这种语言的好处，而不是坏处。

我对 JavaScript 最大的问题是它的类型。JavaScript 具有非常弱的类型，这是由设计隐含的。我认为，在某些方面，这可以使语言变得非常容易使用，但也使类型有时可以做编译器想做的任何事情。我并不认为这是科学计算的最佳选择，尽管我可以肯定地看到在某些情况下这个特性肯定是有用的。然而，我的主观偏好是显式类型——我更喜欢告诉我的编译器做什么。此外，如果您想了解更多关于显式和隐式类型的知识，我不久前写了一篇文章，您可以在这里阅读，这篇文章更详细地介绍了这些概念:

[](/all-about-typing-explicit-vs-implicit-and-static-vs-dynamic-980da4387c2f) [## 关于类型:显式与隐式，静态与动态

### 快速浏览类型以及它们如何与语言交互。

towardsdatascience.com](/all-about-typing-explicit-vs-implicit-and-static-vs-dynamic-980da4387c2f) 

我认为 JavaScipt 有一些非常酷的处理数据的方法。我当然指的是 JavaScript 的数据数组，它可以被视为包含命名数据的对象，就像字典一样。我认为这些应用于科学计算会很棒。在很多方面，它们就像是数据类的基础实现版本，更接近语言本身。我最近也写了一篇关于 Python 的数据类的文章，我认为值得一读，你可以在这里查看:

[](/pythons-data-classes-are-underrated-cc6047671a30) [## Python 的数据类被低估了

towardsdatascience.com](/pythons-data-classes-are-underrated-cc6047671a30) 

# 结论

总的来说，我认为 Tensorflow 的 JavaScript 实现是 Tensorflow 家族中一个受欢迎的新成员。事实上，我非常期待 JavaScript 在机器学习领域的发展。我认为数据科学家了解 JavaScript 是相当普遍的，这种语言本身很像 Python。也就是说，我很好奇有多少数据科学家会将这种语言用于数据科学。当然也有一些缺点。最主要的担忧当然是缺乏生态系统工具来处理数据，以及语言的速度。我认为这两个问题都有解决方案，但可能需要几年时间才能有结果。

现在让我们来回答这个问题。数据科学该不该学 JavaScript？如果你碰巧知道 Python，我会说试试吧。一般来说，这两种语言非常相似，学会一种语言会使另一种语言更容易掌握。也就是说，如果你想做的只是数据科学，当然有一些理由先学习 Python。Python 速度更快，有更多的行业标准工具，这些工具历史悠久，使用良好，记录完善，可以让你找到工作，更重要的是，可以让你用半信半疑的方法实践事情。如果你熟悉 Python，并考虑将 JavaScript 作为你机器学习的下一门语言，我也建议你先尝试一些其他语言。

当然，还有许多其他优秀编程语言的例子可以用于统计分析和机器学习。一个这样的例子是 R，我认为它在分析甚至可视化方面有优势。此外，shiny 只是一个很棒的包，可以构建一些非常酷的仪表板，RStudio 只是一个很好的工作环境。另一种你可以考虑的语言是 Julia。虽然 Julia 仍然是我在本文中尚未提到的最小的语言，但它在过去几年中发展迅速。当然，我仍然会首先推荐 Python，因为它是最用户友好的，通过像 StackOverflow 这样的网站提供最多的在线支持。请查看这个比较不同语言提问的可视化图片:

[](https://insights.stackoverflow.com/survey/2020#most-popular-technologies) [## 堆栈溢出开发者调查 2020

### 每个月，大约有 5000 万人访问 Stack Overflow 来学习、分享和建立他们的职业生涯。行业估计…

insights.stackoverflow.com](https://insights.stackoverflow.com/survey/2020#most-popular-technologies) 

我认为任何可用的工具都值得学习，如果你有时间的话。当然，在数据科学的世界里，可能有更重要的东西需要学习，因为每时每刻都有太多的东西需要学习。但我认为 JavaScript 是一种可以应用于如此多不同应用程序的技能，它肯定会派上用场。如果你是那种最终与 web 开发人员一起工作的数据科学家，当他们问问题时，知道他们在谈论什么也是很方便的，所以出于许多不同的原因，它肯定是一种很有价值的语言。感谢您的阅读，我希望这篇文章提供了一些重要的信息和一些关于 JavaScript 的好建议。我真的对 JavaScript 在机器学习领域的未来感到兴奋，因为我认为它有潜力，而且肯定会是一种很酷的语言！