# 机器学习和国际象棋

> 原文：<https://towardsdatascience.com/machine-learning-how-i-coded-my-own-python-chess-engine-c4c81bcb2a9a?source=collection_archive---------6----------------------->

## 我如何编写自己的 python 象棋引擎

![](img/4691ab8cbdd375906bb1c3e9b5a2e5cd.png)

由[杰斯温·托马斯](https://unsplash.com/@jeswinthomas?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

**源代码:** [Github](https://github.com/iAmEthanMai/chess-games-dataset) | [对战 A.I](https://colab.research.google.com/github/iAmEthanMai/chess-engine-model/blob/main/python_chess_engine.ipynb#scrollTo=dsacAJ-5gkN0)

**快速介绍:**

2020 年末，网飞发布了《女王的策略》,这是一部发生在 20 世纪 50 年代的电视节目，我们跟随一个年轻女性成长为最好的棋手的旅程。

然而，与主角贝丝·哈蒙不同，象棋并不是我最擅长的。所以我做了退而求其次的事情，构建了自己的 python 象棋引擎来弥补我糟糕的象棋技巧。

**第一集:**策略

当涉及到构建任何棋盘游戏引擎时，有多种策略:

*   强化学习
*   极大极小
*   基于人类知识训练的 ML 模型
*   …

我选择了第二种和第三种策略的混合:作为一名数据科学家，我喜欢处理大型数据集和机器学习模型。此外，尽管有点慢，极大极小算法是一个相当可靠的算法，并在捕捉敌人的作品做得很好。强化学习简直太复杂了。

我的策略很简单:

*   首先，使用机器学习模型，引擎将驳回给定棋盘的 50%的可能走法。它通过找到一步棋是“好棋”的概率来做到这一点。所有的移动都按照这个概率排序，只剩下第 50 个百分位数。
*   其次，引擎将对剩余的移动执行最小最大算法。根据棋盘的复杂程度，搜索树会达到一定的深度。

**第二集:**数据

为了做出准确的预测，我们需要大量的数据。对于这个项目，我将使用我在 [Kaggle](https://www.kaggle.com/liury123/chess-game-from-12-top-players) 上找到的数据集。这个数据集是国际象棋大师以`.pgn`格式(可移植游戏符号)进行的数千场游戏的集合。

下一步是从所有的游戏中提取所有的招式，并标记为好或坏。

> 在我看来,‘好棋’是赢家在游戏过程中的某个时刻走的一步棋。“坏棋”是赢家选择不走的合法棋。

[这个](https://github.com/iAmEthanMai/chess-games-dataset/blob/main/Script/process_data.py) python 脚本完成了工作，给我留下了一个全新的`.csv`数据集。

你可以在 Kaggle 和 Github 上找到我的数据集:

<https://www.kaggle.com/ethanmai/chess-moves>  <https://github.com/iAmEthanMai/chess-games-dataset>  

然后，我们需要将数据帧导入我们的 Jupyter 笔记本。我们可以简单地在第一个代码单元中运行以下命令:

```
In[1]: import pandas as pd
       df = pd.read_csv('data.csv')
       print(df.shape)Out:   (1600000,193)
```

数据框架的列如下:

*   前 64 列代表当前棋盘的 64 个方块。
*   从 65 到 128 的列代表移动的开始位置:如果一个棋子从那个方块移动，则为 1.0，否则为 0.0。
*   从 130 到 192 的列代表移动的结束位置:如果一个棋子移动到那个方块，则为 1.0，否则为 0.0。
*   最后一列是标签:如果这是一步好棋，则为真，否则为假。

```
In[2]: df.head()
```

df.head()

**第三集:**模特

在这个项目中，我将使用 tensorflow 线性分类器。这是一个非常常见的模型，因为它被很好地[记录下来](https://www.tensorflow.org/api_docs/python/tf/estimator/LinearClassifier)并且非常有性能。

```
In[3]: linear_est = tf.estimator.LinearClassifier(feature_columns)
```

**第四集:**训练

要训练估计器，我们可以简单地运行以下命令:

```
In[4]: linear_est.train(input_function)
```

关于这个项目的机器学习部分的更多细节，请确保查看我的 [Colab 笔记本](https://colab.research.google.com/github/iAmEthanMai/chess-engine-model/blob/main/train_chess_engine.ipynb)，在那里你可以找到所有的源代码。

**第五集:** Minimax

极大极小算法将探索合法移动的递归树直到某一深度，并使用评估函数评估叶子。

然后，我们可以根据回合将最大或最小的子节点值返回给父节点。这允许我们最小化或最大化树的每一层的结果值。点击了解更多关于 Minimax [的信息。](https://www.freecodecamp.org/news/simple-chess-ai-step-by-step-1d55a9266977/)

评估功能寻找两个标准:

*   一块的类型。例如，骑士比卒更有价值。
*   棋子的位置。例如，一个骑士在棋盘中间很强，而国王很脆弱。

评价函数算法

**第六集:**游戏处理

为了获得合法的移动和显示棋盘，我使用了 python-chess 库。您可以通过运行以下命令用 pip 安装它。

```
!pip3 install python-chess
```

这个库让事情变得非常方便，因为它消除了硬编码国际象棋规则和微妙之处的麻烦。

**第七集:**播放

剩下唯一的事情就是玩游戏。如果你想试一试，运行所有的代码单元，你应该可以开始了…祝你好运！

<https://colab.research.google.com/github/iAmEthanMai/chess-engine-model/blob/main/python_chess_engine.ipynb#scrollTo=dsacAJ-5gkN0> [## 和我的人工智能比赛

python-chess-engine.ipynb](https://colab.research.google.com/github/iAmEthanMai/chess-engine-model/blob/main/python_chess_engine.ipynb#scrollTo=dsacAJ-5gkN0) 

**结论:**

谢谢你留下来，我希望你喜欢这个项目。

欢迎在 [Github](https://github.com/iAmEthanMai) 、 [Kaggle](https://www.kaggle.com/ethanmai) 和 [Linkedin](http://www.linkedin.com/in/ethan-mai-0430a4198) 上与我联系。