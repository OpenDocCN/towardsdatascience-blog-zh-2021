# 如何用 Python 解决优化问题

> 原文：<https://towardsdatascience.com/how-to-solve-optimization-problems-with-python-9088bf8d48e5?source=collection_archive---------4----------------------->

## 如何使用纸浆库用几行代码解决线性规划问题

![](img/e3186555231360a6e9c9c6a14c3be949.png)

查尔斯·德鲁维奥在 [Unsplash](https://unsplash.com/) 上拍摄的照片

线性规划(或线性优化)是在有约束的数学问题中求解最佳结果的过程。 [PuLP](https://pypi.org/project/PuLP/) 是一个强大的库，它只需要几行代码就可以帮助 Python 用户解决这类问题。

我发现 PuLP 是解决这类线性优化问题的最简单的库。目标函数和约束都可以添加到一个有趣的分层方法中，每个方法只需一行代码。这意味着我们可以花更少的时间编码，花更多的时间解决问题。文档也易于阅读，包括五个易于理解的[案例研究](https://coin-or.github.io/pulp/CaseStudies/index.html)。

在本文中，我们将使用来自 [Fanduel](https://www.fanduel.com/) 的 daily fantasy sports (DFS)数据来演示如何解决具有多个约束的最大化问题。目的是这些步骤可以推广到你想解决的其他问题。

我们将使用 DFS 数据，因为它允许我们从理解现实世界的问题到根据目标函数和约束条件定义问题，再到最终用 Python 编写解决方案的整个过程。所有这些步骤都是任何线性规划问题的重要组成部分。DFS 是一个足够简单的上下文来理解这些步骤，同时又足够复杂来讨论它们。

如果您想跟进，请按照以下步骤免费获取数据:

1.  在 [Fanduel](https://www.fanduel.com/) 创建一个免费的幻想账户
2.  前往大厅内的 NBA 选项卡
3.  单击下面的任何比赛，然后单击“输入新阵容”按钮
4.  最后，点击页面顶部的“下载球员名单”来获取 csv 格式的数据

![](img/aa87ed5c75349980d79511640334e3fd.png)

照片由 [JC Gallidon](https://unsplash.com/@jcgellidon) 在 [Unsplash](https://unsplash.com/photos/XmYSlYrupL8) 上拍摄

# NBA 中 DFS 的背景

在我们进入这篇文章之前，我们将快速看一下 Fanduel 为 NBA 组织比赛的方式。

这一背景将构成我们如何为我们试图解决的问题设置约束的基础。在坐下来实际编写代码之前，理解线性编程中的问题总是必要的。当我们坐下来写代码时，这有助于我们形成约束和目标函数。

目标是建立一个 9 人阵容，尽可能多的得分。玩家通过在当天的比赛中做成功的事情来获得分数，如得分或抢到一个篮板，而消极的行为如把球翻过来会被扣分。每个篮球运动员那天都有一份假想的薪水(来自 Fandeul ),你有 60，000 美元分配给这些运动员。你必须选择 2 名控球后卫，2 名得分后卫，2 名小前锋，2 名大前锋和 1 名中锋。

![](img/ada95f1bad702801c90c2ecb52ad180a.png)

照片由[rupixen.com](https://unsplash.com/@rupixen)在 [Unsplash](https://unsplash.com/photos/Q59HmzK38eQ) 上拍摄

# 我们想要解决的问题

既然我们对我们试图解决的问题有了很好的理解，让我们用我们的目标函数正式定义它:

*   最大化我们 9 名玩家的投射点数。

我们想在问题中加入的约束:

1.  一个玩家最多只能买 1 次。
2.  拥有 2 个控卫，2 个得分后卫，2 个小前锋，2 个大前锋，1 个中锋。
3.  花费不超过 6 万美元。

# 用纸浆来解决这个问题

我们现在可以开始实际编写代码来解决这个问题。由于这是一篇关于优化的文章(而不是一篇关于预测结果的文章)，我们将使用每个玩家的平均分数作为他们今天的预测分数。如果我们正在为 Fanduel 构建一个真正的优化器，我们会希望改进我们的估计，以包括其他变量，如比赛和每个球员的预计上场时间。

## 初始设置

首先，让我们在命令行中使用 pip 来安装该软件包:

```
pip install pulp
```

并在我们的 Jupyter 笔记本或 IDE 中导入必要的包:

```
from pulp import *
import pandas as pd
```

然后，我们将使用`pd.read_csv()`读取数据，给我们一个熊猫数据帧，包括`Nickname`(Fanduel 上玩家的名字)`FPPG`(该玩家每场比赛的平均得分)、`Salary`和`Position`变量，我们将称之为`data`。

## 设置数据结构

我们现在需要使用字典定义变量，因为这些是`PuLP`使用的数据结构:

```
# Get a list of players
players = list(data['Nickname'])# Initialize Dictionaries for Salaries and Positions
salaries = dict(zip(players, data['Salary']))
positions = dict(zip(players, data['Position']))# Dictionary for Projected Score for each player
project_points = dict(zip(players, data['FPPG']))# Set Players to Take either 1 or 0 values (owned or not)
player_vars = LpVariable.dicts("Player", players, lowBound=0, upBound=1, cat='Integer')
```

除了最后一行，所有行都建立了字典，将存储在`Nickname`中的玩家名字指向我们感兴趣的其他变量。

最后一行使用了`LpVariables`，它定义了与第二个参数(在本例中是`players`)数值相关的变量。其他参数定义了`player_vars`可以采用的值。参数`cat`可以设置为`'Integer'`或`'Continuous'`。我们只剩下一个将玩家名字指向整数的字典(我们将使用整数分别用 1 或 0 来表示我们是否拥有该玩家)。

## 定义问题

接下来，我们需要使用`LpProblem()`设置我们的问题:

```
total_score = LpProblem("Fantasy_Points_Problem", LpMaximize)
```

第一个参数是问题的名称，第二个参数是名为`sense`的参数，可以设置为`LpMinimize`或`LpMaximize`。我们使用`LpMaximize`，因为我们试图最大化我们的投影点。

在我们定义了问题之后，我们使用`lpsum()`添加我们的目标函数:

```
total_score += lpSum([project_points[i] * player_vars[i] for i in player_vars])
```

我们的薪资限制:

```
total_score += lpSum([salaries[i] * player_vars[i] for i in player_vars]) <= 60000
```

以及我们的位置限制:

```
# Get indices of players for each position
pg = [p for p in positions.keys() if positions[p] == 'PG']
sg = [p for p in positions.keys() if positions[p] == 'SG']
sf = [p for p in positions.keys() if positions[p] == 'SF']
pf = [p for p in positions.keys() if positions[p] == 'PF']
c = [p for p in positions.keys() if positions[p] == 'C']
# Set Constraints
total_score += lpSum([player_vars[i] for i in pg]) == 2
total_score += lpSum([player_vars[i] for i in sg]) == 2
total_score += lpSum([player_vars[i] for i in sf]) == 2
total_score += lpSum([player_vars[i] for i in pf]) == 2
total_score += lpSum([player_vars[i] for i in c]) == 1
```

## 解决问题

一旦我们定义了问题，我们可以用一行代码解决问题！

```
total_score.solve()
```

一旦我们这样做了，我们的优化变量通过调用`total_score.variables()`被存储在一个列表中，我们每个玩家的值被存储在变量`varValue`中，我们的值的名字被存储在每个变量的变量`name`中。因此，我们可以通过查找具有非零值的球员来打印我们的阵容，如下所示:

```
for v in total_score.variables():
    if v.varValue > 0:
        print(v.name)
```

![](img/f660c8ceca5e1add23701c403641fb77.png)

[Wadi Lissa](https://unsplash.com/@w_lissa071) 在 [Unsplash](https://unsplash.com/photos/6coEA1R1Ecs) 上拍摄的照片

# 结论

我们现在能够用 Python 中的 PuLP 解决复杂的线性编程问题了！一旦我们理解了我们试图解决的问题，我们就可以使用这个库用几行代码来解决它。

线性优化是许多领域的重要组成部分，如运营、物流、资本配置等。学习这种技能的最好方法是自己解决问题。您可以使用我们在上面走过的相同步骤:

1.  理解问题
2.  根据目标函数和约束条件来定义问题
3.  用纸浆解决问题

我鼓励你把这些步骤应用到你感兴趣的问题上，我很高兴在下面的评论中听到你在做什么项目！

感谢您花时间阅读本文，并祝您在下一个线性编程问题上好运。