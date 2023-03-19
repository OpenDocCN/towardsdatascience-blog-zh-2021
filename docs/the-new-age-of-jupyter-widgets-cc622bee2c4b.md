# Jupyter 小工具的新时代

> 原文：<https://towardsdatascience.com/the-new-age-of-jupyter-widgets-cc622bee2c4b?source=collection_archive---------11----------------------->

## 如果您可以使用 NPM 的好东西用 Python 来构建 Jupyter 小部件会怎么样？

![](img/2eebd5b0d23593c988c4fc08cde7b9ce.png)

米利安·耶西耶在 [Unsplash](https://unsplash.com/s/photos/data-science?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上拍摄的照片

笔记本一直是软件思想增量开发的工具。数据科学家使用 Jupyter 来记录他们的工作，探索和实验新的算法，快速勾画新的方法，并立即观察结果。

这种互动性是 Jupyter 如此吸引人的原因。为了更进一步，数据科学家使用 Jupyter 小工具来可视化他们的结果或创建迷你网络应用程序，以方便浏览内容或鼓励用户互动。

然而， [IPyWidgets](https://github.com/jupyter-widgets/ipywidgets) 并不总是容易共事。它们不遵循前端开发人员开创的声明式设计原则，并且生成的组件不能像在浏览器环境中那样进行传输。此外，开发人员创建这些库主要是为了满足数据科学家的可视化需求。因此，它们缺少像 React 和 Vue 这样的流行前端框架带来的特性。

是时候迈出下一步了；这个故事介绍了 IDOM:一套用于定义和控制交互式网页或创建可视化 Jupyter 组件的库。我们将讨论后者。

> [Learning Rate](https://www.dimpo.me/newsletter?utm_source=medium&utm_medium=article&utm_campaign=jupyter-idom) 是为那些对 AI 和 MLOps 的世界感到好奇的人准备的时事通讯。你会在每周五收到我关于最新人工智能新闻和文章的更新和想法。订阅[这里](https://www.dimpo.me/newsletter?utm_source=medium&utm_medium=article&utm_campaign=jupyter-idom)！

# IDOM:用 Python 反应设计模式

那么，让我们从 IDOM 开始。对于那些熟悉 React 的人来说，你会发现 IDOM 做事的方式有很多相似之处。

我们将在 Jupyter 中创建一个简单的 TODO 应用程序。是的，我不知道这对数据科学家有什么帮助，但我想做的是展示 IDOM 的能力。如果你发现任何数据科学用例，请在评论中留下！

首先是代码。然后，我们将一次浏览一行，了解它是如何工作的。

`idom.component`装饰器创建一个[组件](https://idom-docs.herokuapp.com/docs/core-concepts.html#stateful-components)构造器。该组件使用其下方的函数(如`todo()`)呈现在屏幕上。然后，为了显示它，我们需要调用这个函数并在最后创建这个组件的一个实例。

现在，让我们来看看这个函数是做什么的。首先，`[use_state()](https://idom-docs.herokuapp.com/docs/package-api.html#idom.core.hooks.use_state)`功能是一个[挂钩](https://idom-docs.herokuapp.com/docs/life-cycle-hooks.html#life-cycle-hooks)。调用这个方法返回两件事:一个当前状态值和一个我们可以用来更新这个状态的函数。在我们的例子中，当前状态只是一个空列表。

然后，我们可以在一个`add_new_task()`方法中使用 update 函数来做我们想做的任何事情。该函数获取一个事件，并检查该事件是否是由敲击键盘的`Enter`键产生的。如果是这样，它将检索事件的值，并将其附加到任务列表中。

为了存储用户创建的任务，我们将它们的名称附加到一个单独的`tasks` Python 列表中，旁边是一个简单的删除按钮。当按下 delete 按钮时，调用`remove_task()`函数，该函数像`add_new_task()`函数一样更新状态。但是，它不是向当前状态添加新项目，而是删除选定的项目。

最后，我们创建一个 input 元素来创建 TODO 任务，并创建一个 HTML table 元素来保存它们。在最后一步，我们使用一个`div` HTML 标签来呈现它们。

# 越来越好了

到目前为止，一切顺利。然而，IDOM 给我们的力量不仅限于显示 HTML 元素。IDOM 的真正力量来自它无缝安装和使用任何 React 生态系统组件的能力。

在这个例子中，让我们使用`[victory](https://www.npmjs.com/package/victory)`，这是一组用于模块化图表和数据可视化的 React 组件。要安装 victory，我们可以使用 IDOM 命令行界面:

```
!idom install victory
```

然后，让我们在代码中使用它:

恭喜你。您刚刚在 Jupyter 笔记本中创建了一个带有`victory`的饼图！当然，导入和使用现有的 JavaScript 模块也很简单。参见[文档](https://idom-docs.herokuapp.com/docs/javascript-components.html#import-javascript-bundles)。

# 结论

数据科学家使用 Jupyter 小工具来可视化他们的结果或创建迷你网络应用程序，以方便浏览内容或鼓励用户互动。

然而， [IPyWidgets](https://github.com/jupyter-widgets/ipywidgets) 并不总是容易共事。它们也有一些缺点:它们不遵循声明式设计原则，并且产生的组件不能像在浏览器环境中那样被转移。

是时候迈出下一步了；这个故事研究了 IDOM:一组用于定义和控制交互式网页或创建可视 Jupyter 组件的库。

IDOM API 在[文档](https://idom-docs.herokuapp.com/docs/index.html)中有更详尽的描述。此外，在安装前，您可以在[活页夹](https://mybinder.org/v2/gh/idom-team/idom-jupyter/main?filepath=notebooks%2Fintroduction.ipynb)上玩`idom-jupyter`。

# 关于作者

我叫 [Dimitris Poulopoulos](https://www.dimpo.me/?utm_source=medium&utm_medium=article&utm_campaign=jupyter-idom) ，我是为 [Arrikto](https://www.arrikto.com/) 工作的机器学习工程师。我曾为欧洲委员会、欧盟统计局、国际货币基金组织、欧洲央行、经合组织和宜家等主要客户设计和实施过人工智能和软件解决方案。

如果你有兴趣阅读更多关于机器学习、深度学习、数据科学和数据操作的帖子，请关注我的 [Medium](https://towardsdatascience.com/medium.com/@dpoulopoulos/follow) 、 [LinkedIn](https://www.linkedin.com/in/dpoulopoulos/) 或 Twitter 上的 [@james2pl](https://twitter.com/james2pl) 。此外，请访问我网站上的[资源](https://www.dimpo.me/resources/?utm_source=medium&utm_medium=article&utm_campaign=jupyter-idom)页面，那里有很多好书和顶级课程，开始构建您自己的数据科学课程吧！