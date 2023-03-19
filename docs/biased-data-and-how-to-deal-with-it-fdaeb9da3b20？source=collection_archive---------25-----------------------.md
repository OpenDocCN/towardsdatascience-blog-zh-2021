# 有偏数据及其处理方法

> 原文：<https://towardsdatascience.com/biased-data-and-how-to-deal-with-it-fdaeb9da3b20?source=collection_archive---------25----------------------->

![](img/9d3ff6a451e224178a6cadc9b40a67a5.png)

由 [Edge2Edge 媒体](https://unsplash.com/@edge2edgemedia?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 拍摄的照片

## 欠采样的故事

我的一个学生刚刚完成了一个涉及分类的数据科学荣誉项目——或者至少他认为他完成了。他收到了波士顿地区大约 2000 名学生和大约 700 名企业家发布的几千条推文。他必须训练一个 ML 模型(RandomForestClassifier)来预测一条推文是由学生还是由企业家发布的。听起来很容易。他随机选择了 70%的推文用于训练，其余的用于测试。这个模型看起来很有效，准确率达到了 83%——对于一个课程项目来说，这已经很不错了。

不幸的是，该模型非常擅长识别学生推文(95%的准确率)，但识别企业家推文(55%的准确率)却相当糟糕。为什么？因为训练数据集有大约 1400 名学生，只有大约 500 名企业家。换句话说，它偏向于学生，并学会了如何更好地识别他们的推文。

修复训练数据集的一种方法是为每个结果提供相等或大致相等数量的样本。如果我们在培训数据中只有大约 500 名企业家，那么我们也应该只包括大约 500 名学生，除非我们的目标是扭曲培训过程。Python 中最流行的数据分析包 Scikit-Learn 没有无偏倚的训练/测试分裂功能。但是我们可以自己造一个。

假设 X 是一个 Pandas 数据框架，它包含我们整个数据集的特征(预测)和结果。特征在列 x1 中..xN 和结果在 y 列。首先计算支持每个结果的样本数:

```
from numpy.random import binomial
np.random.seed(0)
# Generate synthetic dataset with three outcomessamples = 100
X = pd.DataFrame({"x1": binomial(1, 0.3, samples),
                  "x2": binomial(1, 0.6, samples),
                   "y": binomial(2, 0.5, samples)})grouper = X.groupby("y")
sizes = grouper.size()
#0 26 <- the smallest
#1 46
#2 28
```

假设我们仍然希望从最小的*组样本中选择 70%用于训练，并从较大的组中选择匹配数量的样本。我们可以计算来自较大组的样本的百分比，以匹配最小组的 70%。这些百分比是选择样本的概率:*

```
fracs = (sizes.min() * 0.7) / sizes
#0 0.700000 <- the smallest
#1 0.395652
#2 0.650000
```

现在我们知道了概率，我们可以使用二项式分布(`np.random.binomial`)及其布尔补码来分别随机选择训练和测试样本。当我们在相同结果的组中循环时，我们将在两个最初为空的数据帧中累积样本:

```
test  = pd.DataFrame()
train = pd.DataFrame()for grp, data in grouper: # reuse the grouper!
    mask = binomial(1, fracs.loc[grp], data.shape[0]).astype(bool)
    train = train.append(data[mask])
    test  =  test.append(data[~mask])
```

现在，对于每个结果，训练数据集中的行数大致相同。训练数据集是平衡的，不偏向任何特定的结果:

```
train.groupby("y").size()
#y
#0    21
#1    20
#2    23
```

在该数据集上训练的模型将会看到属于这三个类的大约相等数量的样本，并且不会有偏差。

我们是否也应该均衡不平衡的测试数据集？很可能不是:您只将它用于测试，它的偏差不应该是一个问题。