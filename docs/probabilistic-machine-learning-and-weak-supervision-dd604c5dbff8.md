# 概率机器学习和弱监督

> 原文：<https://towardsdatascience.com/probabilistic-machine-learning-and-weak-supervision-dd604c5dbff8?source=collection_archive---------15----------------------->

## [思想和理论](https://towardsdatascience.com/tagged/thoughts-and-theory)

## 用 scikit-learn 将领域专业知识和概率标记注入机器学习:手动标记的替代方法。

我们最近写了一篇文章[手工标注被认为是有害的](https://www.oreilly.com/radar/arguments-against-hand-labeling/)关于主题和领域专家如何更有效地与机器合作来标注数据。举个例子，

*监管薄弱的领域越来越多，中小企业指定启发法，然后系统使用这些启发法对未标记的数据进行推断，系统计算一些潜在的标签，然后中小企业评估这些标签，以确定可能需要添加或调整更多启发法的地方。例如，当基于医疗记录建立手术是否必要的模型时，SME 可以提供以下启发:如果记录包含术语“麻醉”(或与其类似的正则表达式)，则手术可能发生。*

![](img/9f1e3e3faba0b7090271db0d37d4b96e.png)

图片由作者准备，基于[这张图片](https://pixabay.com/photos/pug-dog-blanket-dog-face-801826/)

在这篇技术文章中，我们展示了关于人类如何与机器合作来标记训练数据和建立机器学习模型的原理证明。

我们在概率标签和预测的上下文中这样做，也就是说，我们的模型输出特定行是否具有给定标签的概率，而不是标签本身。正如我们在[手动标记被认为有害](https://www.oreilly.com/radar/arguments-against-hand-labeling/)中明确指出的，建立基础事实很少是微不足道的，即使它是可能的，概率标记是分类标记的概括，它允许我们对产生的不确定性进行编码。

我们介绍了三个关键数字，数据科学家或主题专家(SME)可以利用这些数字来衡量概率模型的性能，并使用这些数字编码的性能来迭代模型:

*   基本速率分布，
*   概率混淆矩阵，以及
*   标签预测分布。

我们展示了这样的工作流程如何让人类和机器做他们最擅长的事情。简而言之，这些数字是中小企业的指南:

*   **基本比率分布**编码了我们在等级平衡方面的不确定性，并且将是一个有用的工具，因为我们希望我们的预测标签尊重数据中的基本比率；
*   当我们有分类标签时，我们使用混淆矩阵来衡量模型性能；现在我们有了概率标签，我们引入一个广义的**概率混淆矩阵**来衡量模型性能；
*   **标签预测分布**图向我们展示了概率预测的整体分布；正如我们将看到的，这种分布尊重我们对基本比率的了解是很关键的(例如，如果我们在上面的“手术”的基本比率是 25%，在标签分布中，我们预计大约 25%的数据集有很高的手术机会，大约 75%的数据集有很低的手术机会。)

让我们现在开始工作吧！我们将经历的步骤是

*   手动标记少量数据并建立基本速率；
*   建立一些领域信息启发/提示；
*   通过查看概率混淆矩阵和标签分布图来衡量我们的模型性能；
*   建立更多的提示，以提高我们的标签质量。

你可以在这个 [Github 资源库](https://github.com/Watchfulio/Probabilistic-Machine-Learning)中找到所有相关的代码。

所以让我们开始贴标签吧！

# 手动标签和基本费率

工作流程的第一步是手工标注一小部分数据样本。我们将在这里处理的数据是来自 Kaggle ( [CC0 许可证](https://creativecommons.org/publicdomain/zero/1.0/))的[医疗转录数据集](https://www.kaggle.com/tboyle10/medicaltranscriptions)，任务是预测任何给定的转录是否涉及手术，如“医疗专业”列中给出的。出于教学目的，我们将使用本专栏来手工标记几行，但这通常是 SME 通过转录来标记行。让我们来看看我们的数据:

```
# import packages and data
import numpy as np
import pandas as pd
import matplotlib.pyplot as plt
import seaborn as sns
import warnings; warnings.simplefilter(‘ignore’)
sns.set()
df = pd.read_csv(‘data/mtsamples.csv’)
df.head()
```

![](img/cbd6a035db93dbb1c9c8903bb3e4e04f.png)

这张图片和后面所有的图片都是作者笔记本中代码的输出。

‍

检查完我们的数据后，现在让我们使用“medical_specialty”列手工标记一些行:

```
# hand label some rows
N = 250
df = df.sample(frac=1, random_state=42).reset_index()
df_labeled = df.iloc[:N]
df_unlabeled = df.iloc[N:]
df_labeled[‘label’] = (df_labeled[‘medical_specialty’].str.contains(‘Surgery’) == 1).astype(int)
df_unlabeled[‘label’] = None
df_labeled.head()
```

![](img/fc985f8dcc86364ceab8192e2b27b339.png)

‍

这些手工标记的行为有两个目的:

*   教我们关于职业平衡和基本比率的知识
*   创建验证集

在构建我们的模型时，关键是要确保模型至少近似地考虑了职业平衡，就像这些手牌的基本比率中所编码的那样，所以现在让我们来计算基本比率。

```
base_rate = sum(df_labeled[‘label’])/len(df_labeled)
f”Base rate = {base_rate}”
```

输出:

```
‘Base rate = 0.24'
```

# 基本利率分布

现在是时候介绍第一个关键数字了:基本利率分布。假设我们有一个实际的基本利率，我们这里的“分配”是什么意思？一种思考方式是，我们已经从数据样本中计算出了基本利率。这意味着我们不知道确切的基本利率，我们围绕它的不确定性可以用分布来描述。一种表述这种不确定性的技术方法是使用贝叶斯技术，本质上，我们关于基础率的知识是由后验分布编码的。你不需要知道太多关于贝叶斯方法的要点，但如果你想知道更多，你可以查看一些介绍材料[这里](https://github.com/ericmjl/bayesian-stats-modelling-tutorial)。在笔记本中，我们已经编写了一个绘制**基本速率分布**的函数，然后我们为上面手工标注的数据绘制分布(老鹰会注意到我们已经缩放了概率分布，因此它在 y=1 处有一个峰值；我们这样做是出于教学目的，所有相对概率保持不变)。

![](img/22606a72bcaa3b532f347c45e278968e.png)

首先请注意，分布的峰值位于我们计算的基本速率，这意味着这是最有可能的基本速率。然而，分布的差异也捕捉到了我们对基本利率的不确定性。

当我们迭代我们的模型时，必须注意这个数字，因为任何模型都需要预测接近基本速率分布峰值的基本速率。

*   随着你产生越来越多的数据，你的后验概率变得越来越窄，也就是说，你对自己的估计越来越确定。
*   相对于 p=0 或 p=1，当 p=0.5 时，你需要更多的数据来确定你的估计。

下面，我们绘制了 p=0.5 和增加 N (N=5，20，50，100)的基本速率分布。在[笔记本](https://github.com/Watchfulio/Probabilistic-Machine-Learning/blob/master/Machine-teaching-with%20Naive-bayes.ipynb)中，你可以用一个 widget 构建一个互动的人物！

![](img/90c6e199a8bbb5a27ab9f5f1fa87effe.png)

# 带有域提示的机器标记

手动标记了数据的子集并计算了基本速率后，是时候让机器使用一些领域专业知识为我们做一些概率标记了。

例如，一名医生可能知道，如果转录包括术语“麻醉”，那么很可能发生了手术。这种类型的知识，一旦被编码用于计算，就被称为提示器。

我们可以使用这些信息以多种方式构建模型，包括构建一个生成模型，我们很快就会这样做。为简单起见，作为第一个近似值，我们将通过以下方式更新概率标签

*   如果转录包括术语“麻醉”,则从基本速率增加 P(手术)
*   如果没有，就什么也不做(我们假设没有这个术语就没有信号)。

有许多方法来增加 P(手术)，为了简单起见，我们取当前 P(手术)和权重 W 的平均值(权重通常由 SME 指定，并编码他们对提示与阳性结果相关的置信度)。

```
# Combine labeled and unlabeled to retrieve our entire dataset
df1 = pd.concat([df_labeled, df_unlabeled])
# Check out how many rows contain the term of interest
df1[‘transcription’].str.contains(‘ANESTHESIA’).sum()# Create column to encode hinter result
df1[‘h1’] = df1[‘transcription’].str.contains(‘ANESTHESIA’)
## Hinter will alter P(S): 1st approx. if row is +ve wrt hinter, take average; if row is -ve, do nothing
## OR: if row is +ve, take average of P(S) and weight; if row is -ve
##
## Update P(S) as follows
## If h1 is false, do nothing
## If h1 is true, take average of P(S) and weight (95), unless labeled
W = 0.95
L = []
for index, row in df1.iterrows():
 if df1.iloc[index][‘h1’]:
 P1 = (base_rate + W)/2
 L.append(P1)
 else:
 P1 = base_rate
 L.append(P1)
df1[‘P1’] = L
# Check out what our probabilistic labels look like
df1.P1.value_counts()
```

输出:

```
0.240 3647
0.595 1352
Name: P1, dtype: int64
```

现在我们已经使用 hinter 更新了我们的模型，让我们深入了解我们的模型是如何执行的。需要检查的两件最重要的事情是

1.  我们的概率预测如何与我们的手形标签相匹配
2.  我们的标签分布如何与我们所知道的基本比率相匹配。

对于前一个问题，进入概率混淆矩阵。

# 概率混淆矩阵

在经典的混淆矩阵中，一个轴是你的手牌，另一个轴是模型预测。

在概率混淆矩阵中，你的 y 轴是你的手形标签，x 轴是模型预测。但在这种情况下，模型预测是一种概率，而不是经典混淆矩阵中的“是”或“否”。

```
plt.subplot(1, 2, 1)
df1[df1.label == 1].P1.hist();
plt.xlabel(“Probabilistic label”)
plt.ylabel(“Count”)
plt.title(“Hand Labeled ‘Surgery’”)
plt.subplot(1, 2, 2)
df1[df1.label == 0].P1.hist();
plt.xlabel(“Probabilistic label”);
plt.title(“Hand Labeled ‘Not Surgery’”);
```

‍

![](img/50fd01c9299e8b17c0a155dc212ea340.png)

我们在这里看到

*   大多数标有“外科手术”的数据的 P(S)约为 0.60(尽管相差不大)，其余的约为 0.24；
*   所有手写标记为“非手术”的行的 P 值约为 0.24，其余的约为 0.60。

这是一个好的开始，因为 P(S)对于标有“非手术”的是偏左的，对于标有“手术”的是偏右的。然而，对于那些标有手术的，我们希望它更接近 P(S) = 1，所以还有工作要做。

# 标签分布图

下一个关键图是跨数据集的标签预测分布图:我们希望了解我们的标签预测是如何分布的，以及这是否与我们已知的基本速率相匹配。例如，在我们的案例中，我们知道我们的基本利率可能在 25%左右。因此，我们期望在标签分布中看到的是约 25%的数据集有接近 100%的手术机会，约 75%的数据集有较低的手术机会。

```
df1.P1.plot.kde();
```

![](img/d3ad230803cc84b97af1551a8ab0c6ea.png)

我们在大约 25%和大约 60%处看到峰值，这意味着我们的模型还没有很强的标签意识，所以我们想添加更多的提示。

本质上，我们真的希望看到我们的概率预测更接近 0 和 1，在一定程度上尊重基本利率。

# 建立更多的提示

为了构建更好的标签，我们可以通过添加更多的提示来注入更多的领域专业知识。下面我们通过增加两个积极的提示(与“外科手术”相关的)来做到这一点，这以类似于上面的提示的方式改变了我们的预测概率。代码见笔记本。这是概率混淆矩阵和标签分布图:

![](img/09a604869fba98dced04b3bf6ef0fe87.png)![](img/b9f6903217e6bc55a35d6352c2c731fe.png)

随着我们增加提示者的数量，发生了两件事:

1.  在我们的概率混淆矩阵中，我们看到那些标有“手术”的手的直方图向右移动，这很好！我们还看到标记为“手术”的手的直方图稍微向右移动，这是我们不希望的。请注意，这是因为我们只引入了肯定的提示，所以接下来我们可能要引入否定的提示，或者使用更复杂的方法从提示转移到概率标签。
2.  我们的标签分布图现在在 P(S) = 0.5 以上具有更大的密度(右侧更大的密度)，这也是所希望的。回想一下，我们期望在标签分布中看到大约 25%的数据集有接近 100%的手术机会，大约 75%的数据集有较低的手术机会。

# 提示者的生成模型

这样的暗示，虽然像玩具例子一样有指导意义，但也只能是表演性的。现在让我们使用一个更大的提示集来看看我们是否能建立更好的训练数据。

我们还将使用一种更复杂的方法来从提示转移到概率标签:我们将使用朴素贝叶斯模型，这是一种生成模型，而不是对权重和先前的概率预测进行平均。生成模型是对特征 X 和目标 Y 的*联合概率* P(X，Y)进行建模的模型，与根据特征对目标的条件概率 P(Y|X)进行建模的判别模型相反。与随机森林之类的判别模型相比，使用生成模型的优势在于，它允许我们对数据、目标变量和暗示者之间的复杂关系进行建模:它允许我们回答诸如“哪个暗示者比其他的更嘈杂？”之类的问题以及“在哪些情况下它们会很吵？”关于生成模型的更多信息，谷歌有一个很好的介绍[在这里](https://developers.google.com/machine-learning/gan/generative)。

为了做到这一点，我们为任何给定的行创建了编码提示是否存在的数组。首先，让我们创建正面和负面暗示的列表:

```
# List of positive hinters
pos_hinters = [‘anesthesia’, ‘subcuticular’, ‘sponge’, ‘retracted’, ‘monocryl’, ‘epinephrine’, ‘suite’, ‘secured’, ‘nylon’, ‘blunt dissection’, ‘irrigation’, ‘cautery’, ‘extubated’,‘awakened’, ‘lithotomy’, ‘incised’, ‘silk’, ‘xylocaine’, ‘layers’, ‘grasped’, ‘gauge’, ‘fluoroscopy’, ‘suctioned’, ‘betadine’, ‘infiltrated’, ‘balloon’, ‘clamped’]# List of negative hinters
neg_hinters = [‘reflexes’, ‘pupils’, ‘married’, ‘cyanosis’, ‘clubbing’, ‘normocephalic’, ‘diarrhea’, ‘chills’, ‘subjective’]
```

对于每个提示，我们现在在数据帧中创建一列，编码该术语是否在该行的转录中:

```
for hinter in pos_hinters:
 df1[hinter] = df1[‘transcription’].str.contains(hinter, na=0).astype(int)
 # print(df1[hinter].sum())
for hinter in neg_hinters:
 df1[hinter] = -df1[‘transcription’].str.contains(hinter, na=0).astype(int)
 # print(df1[hinter].sum())
```

我们现在将**标记的数据**转换成 NumPy 数组，并将其分成训练集和验证集，以便在前者上训练我们的概率预测，并在后者上测试它们(注意，我们目前只使用正提示)。

```
# extract labeled data
df_lab = df1[~df1[‘label’].isnull()]
#df_lab.info()
# convert to numpy arrays
X = df_lab[pos_hinters].to_numpy()
y = df_lab[‘label’].to_numpy().astype(int)
## split into training and validation sets
from sklearn.model_selection import train_test_split
X_train, X_test, y_train, y_test = train_test_split(
 X, y, test_size=0.33, random_state=42)
```

我们现在对训练数据训练一个[伯努利(或二进制)朴素贝叶斯算法](https://scikit-learn.org/stable/modules/generated/sklearn.naive_bayes.BernoulliNB.html)):

```
# Time to Naive Bayes!
from sklearn.naive_bayes import BernoulliNB
clf = BernoulliNB(class_prior=[base_rate, 1-base_rate])
clf.fit(X_train, y_train);
```

有了这个训练好的模型，现在让我们对我们的验证(或测试)集进行概率预测，并可视化我们的概率混淆矩阵，以将我们的预测与我们的手动标签进行比较:

```
probs_test = clf.predict_proba(X_test)
df_val = pd.DataFrame({‘label’: y_test, ‘pred’: probs_test[:,1]})
plt.subplot(1, 2, 1)
df_val[df_val.label == 1].pred.hist();
plt.xlabel(“Probabilistic label”)
plt.ylabel(“Count”)
plt.title(“Hand Labeled ‘Surgery’”)
plt.subplot(1, 2, 2)
df_val[df_val.label == 0].pred.hist();
plt.xlabel(“Probabilistic label”)
plt.title(“Hand Labeled ‘Not Surgery’”);
```

![](img/b4449a708859048c7a42a4984c51f5a2.png)

这太酷了！使用更多的提示和朴素贝叶斯模型，我们看到我们已经成功地增加了真阳性*和真阴性*的数量。这在上面的图中是可见的，因为手部标签“手术”的直方图更偏向右侧，而“非手术”的直方图偏向左侧。

现在，让我们绘制整个标签分布图(从技术上来说，我们需要在 x=0 和 x=1 处截断这个 KDE，但是出于教学目的，我们很好，因为这不会改变太多):

```
probs_all = clf.predict_proba(df1[pos_hinters].to_numpy())
df1[‘pred’] = probs_all[:,1]
df1.pred.plot.kde();
```

![](img/20aab43cc06ce03a99373af6886ea879.png)

如果你问“什么时候停止贴标签？”，你问了一个关键且基本上很难的问题。让我们把它分解成两个 questions:‍

*   你什么时候停止手工标签？
*   你什么时候停止整个过程？也就是说，你什么时候停止创建提示？

回答第一个问题，最起码，当你的基本利率被校准时，你就停止手工标注(一种思考方式是当你的基本利率分布停止跳跃时)。像许多 ML 一样，这在许多方面更像是一门艺术而不是科学！另一种思考方式是绘制一条基本利率相对于标记数据大小的学习曲线，一旦达到稳定状态就停止。考虑何时完成手动标签的另一个重要因素是，当你觉得你已经达到了统计意义上的基线，这样你就可以确定你的程序标签有多好。

现在你什么时候停止添加提示？这类似于问“你什么时候有足够的信心开始根据这些数据训练一个模型？”答案因科学家而异。你可以通过滚动和目测来定性，但大多数科学家更喜欢定量的方法。最简单的方法是计算你的概率标记数据与手工标记数据的准确度、精确度、召回率和 F1 分数，这样做的最低提升方法是使用概率标记的阈值:例如，标签为< 10% would be 0, > 90% 1，以及弃权之间的任何值。需要说明的是，这样的问题仍然是《观察》杂志研究的活跃领域…请关注这个空间！

雨果·鲍恩·安德森和沙扬·莫汉蒂

*非常感谢* [*马*](https://ericmjl.github.io/) *对本帖工作草案的反馈。*

‍

*最初发布于*[*https://www . watchly . io*](https://watchful.io/resources/probabilistic-machine-learning-and-weak-supervision)*。*