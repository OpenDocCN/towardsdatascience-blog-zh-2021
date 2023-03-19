# 一个绝对的快乐:简化

> 原文：<https://towardsdatascience.com/an-app-solute-delight-streamlit-f9f1e7d55a85?source=collection_archive---------41----------------------->

## 探索帮助构建数据应用程序的超酷工具

![](img/d1212aa1365670b6be079484b920e970.png)

艾萨克·史密斯在 [Unsplash](https://unsplash.com/s/photos/data?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上拍摄的照片

一个用户友好的工具，通过在几分钟内将数据脚本转换为可共享的应用程序，帮助轻松部署任何机器学习模型和任何 Python 项目？是的，这是真的。就在这里！

# Streamlit 入门视频

*开始使用 Streamlit |构建您的第一个数据/ML 应用程序，作者 Anuj Syal*

# 解码流水线

Streamlit 由 Adrien Treuille、Thiago Teixeira 和 Amanda Kelly 创建，是一个开源的 Python 库，使您能够毫不费力地为机器学习和数据科学构建漂亮的定制 web 应用程序，而不用担心前端，而且是免费的。该工具是在考虑数据科学家和 ML 工程师的情况下精心开发的，允许他们创建一个交互式环境，从而更容易地共享他们的模型并将其展示给他们的同事和客户。这些值可以根据提供的输入进行更改，所有更改和结果都以交互方式实时发生，这都归功于它的热重装功能。Streamlit 主要使用 Markdown 和 Python，但也支持 HTML 和 CSS。Streamlit 免费且易于安装，以网络应用程序的形式将您的想法在互联网上变为现实。

# 是什么让 Streamlit 脱颖而出？

该工具背后的主要思想是让数据应用程序的创建像编写 Python 脚本一样简单。这正是创造者们努力实现的目标。

1.  它是交互式的，尤其是对你的客户来说，非常美观
2.  如果你知道如何编写 Python 脚本，你就可以创建很酷的应用程序了
3.  从您的 GitHub 库进行即时部署

# 如何简化？

人们只需要输入:

```
pip install streamlit
```

在您的终端/命令提示符下，它已经可以使用了(前提是您首先安装了 pip)。成功安装 Streamlit 后，运行下面给出的 python 代码，如果没有出现错误，则自动表示 Streamlit 安装成功。打开命令提示符或 Anaconda shell 并键入:

```
streamlit run filename.py
```

> ***注意:*** *为了避免与其他应用程序的任何版本兼容性问题，请在单独/独立的虚拟环境中安装 Streamlit。*

![](img/09629e82b21be0fc8526e2b6e4618770.png)

[*Max Duzij*](https://unsplash.com/@max_duz?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)T5[*Unsplash*](https://unsplash.com/?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

虽然称这个工具相当简单和方便是一回事，但对它进行测试又是另一回事。让我们开发一个简单的 web 应用程序来读取和可视化熊猫数据框。

# 测试 Streamlit 的时间到了:我的演示 Web 应用

## 步骤 1:导入 Python 库添加应用程序的标题+主体

```
import streamlit as st
import pandas as pd
import numpy as np
st.title(‘App by Anuj’)
st.write('This app walkthroughs on creating your first data app with streamlit. It demonstrates how easy it is now for data scientists to develop and deploy quick prototypes.')
```

## 步骤 2:将数据读入数据帧

```
df = pd.read_csv('game_of_thrones_battles.csv')
```

## 第三步:加入互动元素

```
#Use sidebar
option = st.sidebar.selectbox(
    'Which number do you like best?',
     df['first column'])

'You selected:', option
```

## 第四步:创建你的第一张图表

```
chart_data = pd.DataFrame(
     np.random.randn(20, 3),
     columns=['a', 'b', 'c'])

st.line_chart(chart_data)
```

## 步骤 5:创建一个带有地块的地图

```
map_data = pd.DataFrame(
    np.random.randn(1000, 2) / [50, 50] + [37.76, -122.4],
    columns=['lat', 'lon'])

st.map(map_data)
```

## 步骤 6:添加进度条

```
# Add a placeholder
latest_iteration = st.empty()
bar = st.progress(0)

for i in range(100):
  # Update the progress bar with each iteration.
  latest_iteration.text(f'Iteration {i+1}')
  bar.progress(i + 1)
  time.sleep(0.1)
```

## 步骤 7:保存并运行

现在我们已经完成了一个简单的脚本，是时候保存它了。我将把它保存为从终端运行的 myapp_ [streamlit.py](http://streamlit.py) (确保安装了 pip 需求)

`streamlit run myapp_streamlit.py`

(命令 streamlit run 后跟您的 Python 脚本名称)

现在你知道了！您的第一个演示 web 应用程序。随意探索其他命令，添加边栏、进度条、列和其他迭代，特别是当你有太多东西要在 UI 上显示，但想要一个更整洁的外观时。

Streamlit 的缓存功能是其主要亮点之一。缓存的能力使它相对更快，尤其是当你做一些小的调整，然后试图重新运行。

> *注意:我建议我的数据科学家同事将代码和应用程序的浏览器窗口并排打开，以便高效、快速地获得结果，因为可以实时看到变化。*

# 关注 streamlit 的竞争对手

虽然 Streamlit 完全在 Python 中工作，但 R 和 Plotly Dash 是另一个支持 Python 和 Julia 的流行选择。特别是对于 R 用户来说，R shiny 是快速开发 web 应用的一个很好的选择。

# 我的想法:Streamlit 真的亮了吗？

Streamlit 是数据科学家的福音。关于它没有两种方法。它不仅可以帮助他们构建 ML web 应用程序，还可以方便地向利益相关者、客户和同事共享和展示他们的模型，特别是如果他们是非技术人员和/或不熟悉程序或脚本的话。今天，它已经成为 ML 工程师事实上的选择，使他们能够快速构建和共享概念证明，同时给他们一个快速调整的选项。Streamlit 美观且用户友好，熟悉 Python 脚本的任何人都可以轻松学会。Streamlit 具有丰富的特性，它使用户能够轻松地部署模型，这是值得称赞的。很简单。而且是杰出的！

有兴趣看代码的话，看看这个 github rep [链接](https://github.com/syalanuj/youtube/tree/main/Getting%20started%20with%20Streamlit)

# 在 Linkedin 和 Twitter 上关注我

如果你对类似的内容感兴趣，点击 Medium 上的关注按钮，或者在 [Twitter](https://twitter.com/anuj_syal) 和 [Linkedin](https://www.linkedin.com/in/anuj-syal-727736101/) 上关注我

*原载于*[*https://anujsyal.com*](https://anujsyal.com/an-app-solute-delight-streamlit)*。*