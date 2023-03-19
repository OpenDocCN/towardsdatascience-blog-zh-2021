# 回调、布局和引导:如何在 Plotly Dash 中创建仪表板

> 原文：<https://towardsdatascience.com/callbacks-layouts-bootstrap-how-to-create-dashboards-in-plotly-dash-1d233ff63e30?source=collection_archive---------2----------------------->

![](img/35063c0cf8d9e8368997e58800ded8c5.png)

由[卢克·切瑟](https://unsplash.com/@lukechesser?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

数据科学家和分析师最重要的角色之一是以可理解的方式向利益相关者呈现数据和见解。创建仪表板是显示您在数据探索和机器学习建模中发现的东西的一种很好的方式。Plotly 提供了一个健壮的开源版本的库，用于构建仪表盘 Dash。此外，还有多个额外的开源库，使得在 Dash 中构建仪表板变得容易。本教程概述了使用 Dash 实现您的第一个多页面仪表板应用程序的一种方法。

# Dash 是什么？

Dash 是一个框架，允许您创建高度可定制和交互式的数据可视化应用程序，这些应用程序仅使用 Python 在您的 web 浏览器中呈现。Dash 在 R 和 Julia 中也可用，但本教程将重点介绍 Python。Dash 中的仪表板构建在 Plotly、Flask 和 React 之上，能够部署到互联网上，并在移动设备上跨平台显示。本质上，Dash 能够创建 web 应用程序，而不直接使用 HTML 或 CSS。这里我们将重点介绍免费的开源版本，但是 Plotly 为 Dash 提供了一个企业版，用于管理企业仪表盘。

## Dash 的关键要素

在我们深入细节之前，让我们回顾一下我们将在构建第一个多页 dash 应用程序时使用的一些关键想法:

*   **布局&组件:** 布局描述了您的仪表板的外观。这包括样式选择，如 CSS 脚本生成的字体和颜色，以及哪些组件将填充应用程序。`dash_core_components`和`dash_html_components`库提供了 dash 应用程序的各种常用组件。HTML 组件是 HTML 标记的 python 版本，可以在 dash 应用程序中使用，允许您创建 div、显示文本、构建段落/标题等。你也可以像在 HTML 中一样给它们分配一个样式、一个类或者一个 id。核心组件是可以放在仪表板上的各种有用的元素，就像下拉菜单、图表、滑块、按钮等等。你可以在 [Dash 的文档](https://dash.plotly.com/)中找到许多额外的 Dash 组件库。
*   **回调:** 回调是 python 装饰器，控制你的 dash 应用的交互性。装饰器是一个包装另一个函数的函数，在它们装饰的函数之前和/或之后执行代码。在 Dash 中，回调装饰器使用该属性来查找用户对您的 dashboard 应用程序组件所做的更改，并执行您定义的任何相关更改。例如，您可以设置一个回调，从仪表板上的滑块组件获取信息，然后根据滑块的输入对图形进行更改。
*   **Bootstrap:** 如果你已经熟悉 CSS，你可以通过编写自己的 CSS 脚本来设计你的 Dash 应用程序。但是，如果你不熟悉 CSS 或者不想花时间自己写，Bootstrap 是个不错的选择。Bootstrap 是一个流行的 CSS 框架，用于创建交互式的移动应用。faculty.ai 的`dash_bootstrap_components`库提供了 CSS 样式以及附加组件，如应用导航工具、可折叠内容、对话框等等。Bootstrap 使您能够创建包含组件的列和行，从而简化了内容的组织。[这里有一个关于它如何操作的概要](https://betterprogramming.pub/a-guide-to-the-bootstrap-grid-system-18d2f8a1c7c3)，查看[文档](https://dash-bootstrap-components.opensource.faculty.ai/docs/)了解如何将其翻译成 python。

# 开始使用您的仪表板

首先，您需要安装 Dash 和 Dash 引导组件(核心和 HTML 组件应该预装在 Dash 中)

```
$ pip install dash==1.19.0
$ pip install dash-bootstrap-components
```

安装 dash 后，您需要创建一个目录来保存构建应用程序的所有 python 脚本。注意，有多种方法来构造这个目录和其中的 python 文件，下面只是构造它的一种方法。我喜欢用这种方式组织它，因为我发现它有助于我保持代码的条理性。在这个结构中，您将有四个**文件:**

## PyFile #1:应用程序

`app.py`:这个脚本简短明了。它会创建您的 dash 应用程序，并将其连接到服务器。如果您选择使用 Bootstrap CSS 样式(或另一个预制的 CSS 样式表)，您也可以在这里这样做。

## PyFile #2:索引

这将是你进入仪表板的主要入口。这意味着当你想运行 dashboard 应用程序时，你可以在命令行中输入`python index.py`。在这里，您还将定义在应用程序的哪些页面上显示哪些布局。`app.layout`将是你的总体结构。在这个例子中，我有一个 location 组件，它将结构与 URL 联系起来，实例化一个水平导航栏，并将页面内容放在一个 HTML div 中。这种结构将在应用程序的所有页面上显示相同。下面的回调是一个特殊的回调，当你点击导航栏中的链接到一个新页面时，它将页面布局发送到结构中的`page-content` HTML div。确保从`layouts.py`中导入您想要显示的布局、组件和样式。

## PyFile #3:布局

`layouts.py`:这是你设计仪表盘应用的地方。你需要将每个布局、组件或样式设置为一个变量或存储在一个函数中，这样你就可以在`index.py`和这个布局脚本的其他地方访问它。请记住，`layouts.py`只是定义每个页面的布局和各种附加组件，如导航栏。**我们将在后面讨论如何在** `**callbacks.py**` **中实现交互性。**

在这个例子中，我已经导入了经典的 iris 数据集来生成两个 Plotly Express 图形进行显示。我还创建了一个导航栏、两种样式和两种布局。每个布局由一个 1 行 2 列的引导网格组成。在第一个布局中，左边的列有两个选项卡，将显示不同的图形。右边的列显示与用户在图上的点击相关联的文本。对于第二个布局，它展示了两个非常有利于交互性的 dash 核心组件:下拉菜单和单选按钮。除了显示选择的值之外，它们现在不做太多事情，但这是您可以发挥创造力并弄清楚如何使用选择的值与其他组件和图形进行交互的地方。

`layouts.py`很可能是你最长的 py 剧本。

## PyFile #4:回调

您想要创建的任何互动都应该放在这里。您会注意到上面的`layouts.py`脚本中的许多元素都传递了一个`id=`参数。这些 id 是`callbacks.py`中的回调如何识别哪些组件监听用户输入，以及在哪里显示回调的输出。回调的结构如下:

```
@app.callback(
    Output("component-id-to-output-to", "children"),
    Input("component-id-to-listen-to", "valuesIn")
)
def callback_name(valuesIn):
    #code for callback to execute
    return output_to_children
```

每个回调都监听一个组件并从中获取值。这是`Input()`的作品。然后使用这些值执行回调函数。一旦函数返回一个值或对象，它就被发送到`Output()`。`"children"`是每次回调的通用输出。子元素是组件的子元素。我们实际上已经用了很多孩子。在上面的脚本中，每次我们将组件嵌套在另一个组件中时，我们都添加了一个子组件。所以本质上，当`children`在回调的输出中时，它告诉 Dash 将返回的内容放在 id 与输出中列出的 id 相匹配的组件中。例如，`callbacks.py`中的第一个回调从 id 为`tabs`的组件获取值`active_tabs`，并将回调的输出返回给 id 为`tab-content`的组件。返回的内容现在是`tab-content`组件的子组件。

我在下面的脚本中设置了四个回调:

1.  监听当前点击了哪个选项卡，并返回相关的图表
2.  监听图表上的点击并返回与该点击相关联的文本
3.  监听下拉菜单并以字符串形式返回选定的值
4.  监听单选按钮并以字符串形式返回选定的值

大多数回调并不有趣，但是在开始构建更复杂的回调之前，理解简单的回调是有帮助的。

一旦你设置好了所有的四个文件，编辑它们到你的数据，来自 Plotly 的可视化，等等。此外，尝试改变结构/布局，并创建自己的回调。学习 Dash 的最好方法是尝试不同的布局和风格。当你想看看你的应用程序头输入`python index.py`到你的命令行。它应该是这样的:

```
$  python index.pyDash is running on http://127.0.01:5000/* Serving Flask app "app" (lazy loading)* Environment: productionWARNING: This is a development server. Do not use it in a production deployment.Use a production WSGI server instead.* Debug mode: on
```

在任何网络浏览器中输入运行 Dash 的 URL，看看你创建了什么！如果调试模式是打开的，你应该能够更新你的脚本，你的应用程序将改变生活。您可以通过设置`debug=False`来关闭`index.py`中的调试模式。

Dash 是一个庞大、灵活、强大的库，需要学习的东西太多了，一开始可能会有点不知所措。但是这是一个很好的工具，一旦你投入进去，它就值得了。

在这里找到这个项目的 [GitHub 库](https://github.com/mitkrieg/dash_demo)。