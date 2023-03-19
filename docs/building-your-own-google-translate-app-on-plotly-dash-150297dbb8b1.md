# 用 Plotly Dash 构建自己的谷歌翻译应用程序

> 原文：<https://towardsdatascience.com/building-your-own-google-translate-app-on-plotly-dash-150297dbb8b1?source=collection_archive---------24----------------------->

## 语言不分国界！

![](img/b6f04f9fcbedc38ad4f66b92ddd88cdf.png)

[Artem Beliaikin](https://unsplash.com/photos/v6kii3H5CcU) 在 [Unsplash](https://unsplash.com) 上拍照

***动机***

我喜欢探索！我一直对世界各地的不同语言很感兴趣。我来自一个使用多种语言的国家，我可以证明，掌握多种语言肯定会打开你的世界，因为你可以很容易地与他人联系。虽然谷歌翻译确实让我们的生活变得更加轻松，语言不再是障碍，但我认为如果我能在一次搜索中获得所有语言中一个单词的**翻译，而**则是惊人的。不仅如此，作为一个视觉学习者，**将这些翻译的单词映射到地图上各自的国家**有助于让我的学习更容易，幸运的是，Plotly Dash 使这成为可能！

***先睹为快***

这是该应用程序的最终产品，当你在一个国家徘徊时，翻译的文本就会出现。很想创造一个吗？让我们开始吧！

# 图书馆

以下库将用于构建此应用程序。

1.  **googletrans** :实现 Google Translate API 的 python 库
2.  一个交互式的开源绘图/可视化库
3.  **dash** :用于构建 web 分析应用程序的高效 Python 框架
4.  **pycountry** :提供语言&国家的 ISO 数据库
5.  熊猫:提供快速、灵活和富于表现力的数据结构的包

```
pip install googletrans==3.1.0a0
pip install dash
pip install pycountry
pip install pandas
```

我已经安装了 alpha 版本`googletrans==3.1.0a0`,因为由于谷歌翻译 API 的变化，其他版本中出现了一些问题。

***数据集的预览***

数据集可以在我的 [GitHub 库](https://github.com/Vanessawhj/translation-dashapp/blob/main/updated_population.csv)中找到。

# Googletrans

谷歌翻译 API 支持多种语言。要列出它们，只需运行下面的代码。

```
import googletrans
from pprint import pprintpprint(googletrans.LANGUAGES)
```

现在，让我们看看翻译器是如何工作的。首先需要从`googletrans`模块导入 Translator 并创建一个`Translator`类的对象，然后将文本作为参数传递给`Translator`类对象的`translate()`方法。默认情况下，`translate()`方法返回传递给它的文本的英语翻译，因此您需要使用`dest`属性指定目标语言。这里我们用`‘fr'`初始化了`dest`，这意味着`translate()`方法会将文本从英语转换成法语。

```
result = translator.translate("hello world", dest="fr").text
```

由于我们处理的是 pandas DataFrame，而`dest`因国家而异，所以我使用了`.apply()`方法来传递函数，并将其应用于 Panda 系列`ISO_lang`的每个值。

```
df["transl"] = df["ISO_lang"].apply(lambda x: translator.translate(text, src=lang, dest = x).text)
```

现在我们有了一个包含翻译文本的新列`transl`。

# Plotly Express

为了可视化我们翻译的文本，我将使用 Plotly Express，这是一个新的高级 Python 可视化库，也是 Plotly.py 的包装器，允许使用复杂图表的简单语法。Plotly 是最好的可视化库之一，因为它允许交互式 T21 可视化。但最重要的是，它可以**部署在 Dash 应用**上，这才是本文的主旨！

在我们的可视化中使用的图是一个`choropleth`地图，它允许我们通过根据值着色的区域&呈现数据。数据帧中的每一行被表示为 choropleth 的一个区域。

`color`参数本质上是我们想要用于颜色编码的数据帧的列。在这里，我使用了`population`列，这样我们的 choropleth 地图也允许我们按照国家来可视化世界人口。

# 破折号

现在进入激动人心的部分，让我们把所有东西放在一起，构建我们的 dash 应用程序！Dash 是 Plotly 的开源框架，用于构建分析性 web 应用程序和以 Plotly 图表为特色的仪表盘。这是一个 web 应用框架，**围绕 HTML、CSS 和 JavaScript** 提供抽象，因为它构建在 Flask、React.js 和 Plotly.js 之上。

让我们首先导入 dash 库。

Dash 应用由两部分组成:布局**和交互**两部分。****

***一、布局***

布局使用两个库，分别是`dash_html_components`和`dash_core_components`。`dash_html_components`库**为所有的 HTML 标签**提供了类，关键字参数描述了 HTML 属性，如`style`、`className`和`id`。它允许我们设计应用程序的外观。简而言之，dash 允许我们使用 Python 结构和 dash 组件库来构建布局，而不是编写 HTML，dash 组件库使用 Div 类**转换 Python 类**来创建 HTML**Div。另一方面，`dash_core_components`库生成更高级别的组件，如控件和图形。`dcc.Graph`类允许我们在布局中创建一个图表。在幕后，Dash 使用 Plotly.js 创建交互式图形，其中的`dcc.Graph`组件需要一个包含绘图数据和布局的图形对象或 Python 字典。同时，`dcc.Input`接受用户输入(要翻译的文本)并将其作为参数传递给 Dash 回调中的`update_output`函数。**

***二世。回调***

回调函数本质上是 Python 函数，每当输入组件的属性发生变化时，dash 就会自动调用这些函数**，因此**允许 Dash 应用程序中的交互**。为了做到这一点，我们需要从`dash.dependencies`导入`Input, Output & State`。**

每一个`state` & `input`都需要表示为回调函数内部的参数。`input`和`state`的区别在于，每当输入的`component_property`发生变化时，自动触发回调。然而，当状态的`component_property`改变时，它不会被触发。从长远来看，每当用户在文本框中输入时，图表不会更新。当用户点击“翻译”按钮时，只有这时图形才会更新。

```
[dash.dependencies.Input(component_id='submit_button', component_property='n_clicks')],     [dash.dependencies.State(component_id='input_state', component_property='value')]
```

让我们了解一下**依赖关系**是如何工作的。回调函数中的`component_id=’submit_button’`基本上与之前在`app.layout`中初始化的`html.Button(id=’submit_button’, n_clicks=0, children=’Submit’)`参数进行交互。这意味着当我们的应用程序界面中的提交按钮被点击时，它触发回调函数执行`update_output`函数，在这里它作为`num_clicks`参数被传递。回调函数将数据返回给`output`的`component_property='figure'`，这是 choropleth 映射。

参数的**顺序对于应用程序的运行至关重要**，即**输出→输入→状态**，我们的`update_output`函数中的参数对应于回调函数中的`input`和`state`参数，顺序相同。例如，第一参数`num_clicks`与输入`component_property='n_clicks'`相关联，第二参数`value`与状态`component_property='value'`相关联。

在某些情况下，当您不想更新回调输出时，您可以像我一样在回调函数中引发一个`PreventUpdate`异常。

最后，插入我们之前的代码片段，在`update_output`函数中创建 choropleth 映射。为了查看我们的可视化，我们需要**运行我们的 web 服务器**，就像在 Flask 中使用`app.run_server(debug=False)`一样。如果你在 Jupyter 笔记本上运行，记得**禁用调试模式**和。我们的 dash 应用程序现在可以运行了！

# 结论

恭喜你！在本文中，你已经学习了**破折号布局和回调**，这两者对于构建你的第一个 web 应用程序都是必不可少的。Dash 创建交互式 dashboard 的能力超乎想象，所以去创建一些有趣的东西并与世界分享吧！

我应该注意到，应用程序加载时间与数据集中的国家数量相对应，这是因为`googletrans`在生成输出之前必须逐行翻译。

完整的代码可以在我的 [GitHub 库](https://github.com/Vanessawhj/translation-dashapp):)中找到

**资源:**

1.  [Plotly Express — Choropleth 文档](https://plotly.github.io/plotly.py-docs/generated/plotly.express.choropleth.html)

2.[仪表板组件库](https://dash.plotly.com/plugins)

3. [Googletrans 文档](https://pypi.org/project/googletrans/)

我总是乐于接受反馈和建设性的批评。此外，请随时与我分享你的工作，我也期待着向你学习！你可以在 Linkedin 上找到我。:)