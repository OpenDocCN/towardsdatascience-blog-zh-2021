# 如何设计熊猫图和图表的样式

> 原文：<https://towardsdatascience.com/styling-pandas-plots-and-charts-9b4721e9e597?source=collection_archive---------18----------------------->

## 用现成的样式表定制你的熊猫图——或者设计你自己的

![](img/ef5a1b67ac420ed6afd1881bd32279d2.png)

*mathplotlib* 样式—作者图片

网站、出版物和新闻来源都有自己的风格。看看英国广播公司(BBC)或《纽约时报》(NewYork Times)发布的金融数据。或者内特·西尔弗的 538 网站上的投票数据。它们每个都有清晰一致的外观。

我们将看看如何用我们的熊猫图表和情节实现类似的东西。首先，通过使用我们可用的内置样式，然后看看如何创建我们自己的定制。

默认样式呈现如下图所示的折线图。这是一个干净的图像，但如果你喜欢不同的东西，还有几个内置的风格。

![](img/e7206999fc91f25e3a715eb7cf41da5d.png)

默认样式—按作者分类的图像

要获得 Mathplotlib 中所有可用样式的列表，请使用以下命令。

```
plt.style.available
```

您将得到如下所示的列表(注意，不同版本的 mathplotlib 可能有不同的样式，这些样式包含在我的 matplotlib 3.3.4 系统中)

```
['Solarize_Light2',
 '_classic_test_patch',
 'bmh',
 'classic',
 'dark_background',
 'fast',
 'fivethirtyeight',
 'ggplot',
 'grayscale',
 'seaborn',
 'seaborn-bright',
 'seaborn-colorblind',
 'seaborn-dark',
 'seaborn-dark-palette',
 'seaborn-darkgrid',
 'seaborn-deep',
 'seaborn-muted',
 'seaborn-notebook',
 'seaborn-paper',
 'seaborn-pastel',
 'seaborn-poster',
 'seaborn-talk',
 'seaborn-ticks',
 'seaborn-white',
 'seaborn-whitegrid',
 'tableau-colorblind10']
```

在开始绘图之前，您可以通过执行这样的代码来更改特定的样式。

```
matplotlib.style.use('ggplot')
```

这将选择 *ggplot* 样式，它会给你一个类似下图的图形。

![](img/7b60fa767945b32a75afd43d7d70cf2a.png)

一个 ggplot 样式的图表—作者图片

已经为绘图包 Seaborn 创建了许多样式，但是您可以将它们用于任何基于 mathplotlib 的绘图库。

其他样式是对其他绘图系统或网站的模拟。 *ggplot* 样式基于 R 语言中常用的 *ggplot2* 库。

在本文的最后，我附上了一个图片列表，展示了上面列表中的所有风格。

这里有一些代码，您可以尝试不同的风格。

```
import pandas as pd
import matplotlib
import matplotlib.pyplot as plt
import numpy as np# Change 'default' to the style that you want to try out
matplotlib.style.use('default')df = pd.DataFrame(np.random.rand(100, 2), columns=['x', 'y'])
df.plot.scatter(x='x', y='y')
```

该代码只生成两列随机数据，并绘制一个散点图。下面是我把*默认*改成 *dark_background 得到的结果。*

![](img/0485e9fabd97e527a1cdcaf789ffc512.png)

深色 _ 背景样式—按作者分类的图像

这是 *Solarize_Light2:*

![](img/6c0c9e0f196ac1c53a255e3625cf369e.png)

Solarize_Light2 样式-作者提供的图像

这就是我们如何改变到一个内置的风格，其中一个可能适合你的目的。或者你想更冒险一点。

## 更改单个属性

Matplotlib 图形的属性存储在名为 *rcParams* 的字典中。您可以通过在该字典中设置值来更改单个属性。因此，如果您想将字体大小从默认值 10 更改为更小的值，您可以这样做:

```
matplotlib.rcParams['font.size'] = 8
```

您所做的任何后续绘图的字体大小现在将为 8。

只需打印出来，就可以很容易地找到所有的 rcParams。

```
print(matplotlib.rcParams)
```

它们数量惊人，其中一些的目的并不明显，但这里有几个目的相当明确:

```
 'font.family': ['sans-serif'],
          'font.size': 10.0,
          'font.style': 'normal',
          'font.weight': 'normal',
          'lines.color': 'C0',
          'lines.linestyle': '-',
          'lines.linewidth': 1.5,
          'scatter.marker': 'o',
          'text.color': 'black',
```

下面是一个简短的程序，说明如何使用这些参数。我们将绘制一个散点图，如上图所示，但改变了一些参数。

```
matplotlib.rcParams['scatter.marker'] = '*'
matplotlib.rcParams['figure.facecolor'] = 'yellow'
matplotlib.rcParams['axes.facecolor'] = 'lightblue'
matplotlib.rcParams['font.family'] = 'serif'
matplotlib.rcParams['font.size'] = 8
matplotlib.rcParams['font.weight'] = 'bold'
matplotlib.rcParams['text.color'] = 'red'df.plot.scatter(x='x', y='y', title='Scatter graph')
plt.show()
```

这是结果:

![](img/b2cb684ba1cdb466a3ddf72baa2104e2.png)

一种颇为可疑的定制风格——作者图片

您可以在生成的图表中看到每个参数的变化—这些参数的含义相当清楚。(顺便说一下，我并不是建议你采用这种特殊的配色方案！)

## 创建自己的样式表

你不必在你写的每个程序中都指定你想要的参数。您可以创建自己的样式表，并使用它来代替内置的样式表。

您所要做的就是创建一个文本文件，其中的 rcParams 按照您想要的方式进行设置，然后以类似于内置样式表的方式将它用作样式表，例如:

```
matplotlib.style.use('path-to-my-style-sheet')
```

当然，您不必指定所有的 rcParams，只需指定您想要更改的那些即可。

因此，如您所见，Matplotlib rcParams 允许您为熊猫图创建自己独特的、可重用的样式。

当然，这里的例子非常简单，但是您可以通过查看它们的源代码来探索如何在内置样式中使用这些参数。

## 样式库

你可以在 Matplotlib Github 库的 [Stylelib 文件夹](https://github.com/matplotlib/matplotlib/tree/master/lib/matplotlib/mpl-data/stylelib)中找到所有不同风格的代码。试着看看经典风格——这演示了设置几乎所有可能的参数，但你也应该看看其他一些。

最后，下面是与每个内置主题相关的图像。

一如既往，感谢阅读，我希望你发现这是有用的。如果你想知道我什么时候发表其他文章，请考虑订阅我的免费、不定期的简讯[这里](https://technofile.substack.com)。

## 内置主题

下面的三幅图像是在 *matplotlib* 版本 3.3.4 中使用内置主题渲染的相同图形。

![](img/219a8e973d8a83d08bea880172c74b1a.png)![](img/26a0db1c9148533332c7402c5b95059d.png)![](img/8cbb6390b8fd499496e5086ede3f0be3.png)![](img/06123799b8f374970b7dbd2258b009c4.png)![](img/a836362652e4c5f94836e1a929c6f154.png)![](img/5572acc9dd3046b0e7c0d1782be69449.png)![](img/ba6f31d2e674c0726298619089aa33eb.png)![](img/6ab50fa576e0226aa8fadfb5f54dfbaf.png)![](img/fb84eb596c3c13718e16c8c8a8477871.png)![](img/bf950ea05a80b7d6271213e41d3f8738.png)![](img/c86a401324245935cf3ccfb4b7c82a0a.png)![](img/00e81f0938685f19ce5c483ca8343f33.png)![](img/93708f48ba33dafadcbf5d76475d342d.png)![](img/1992cb23dbc62dc65dae3ae22cb22f36.png)![](img/7eb4cb8dc99c1bb5838e0d42e58f0fe6.png)![](img/cdb12eeb709dad0262b28553933a358a.png)![](img/afecfbe7829ce7ae2a5dc5e055719db5.png)![](img/ef9883cfaf497a619e21fd8f66b61029.png)![](img/b8ff585cb95e03adcecd2fff7ff4f8bf.png)![](img/0fb63b5884fa00e512deb7b5784ef478.png)![](img/97abe2f0146fe8436d6758849b962deb.png)![](img/5e11da96881b774d17dbc3890aab285f.png)![](img/64994e86513096c554ba7ac578209440.png)![](img/c3084284bd4271f66cd1692d8895179c.png)![](img/ae462f3121f6299059806789df4060f8.png)![](img/5af910da80c0ca0013518ce2a31471ec.png)![](img/bc8690c365711741feb289e6c1cb837b.png)

内置样式的图像—按作者分类的图像