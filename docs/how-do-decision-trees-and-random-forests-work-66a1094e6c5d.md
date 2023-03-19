# 决策树和随机森林是如何工作的？1a

> 原文：<https://towardsdatascience.com/how-do-decision-trees-and-random-forests-work-66a1094e6c5d?source=collection_archive---------29----------------------->

## 决策树

![](img/b1fa0f4ebf516e5b7c7abd59326f7622.png)

在 [Unsplash](https://unsplash.com/s/photos/tree?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上[veeterzy](https://unsplash.com/@veeterzy?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)([Vanja ter zic](https://medium.com/u/569b8382d655?source=post_page-----66a1094e6c5d--------------------------------))拍摄的照片

决策树和随机森林是预测建模中两种常用的算法。在这篇文章中，我将讨论决策树背后的过程。我计划在第二部分继续讨论随机森林，然后比较两者。

首先:决策树。决策树是根据出来的图形的形状来命名的。下图显示了一个决策树，用于确定哪些因素影响了泰坦尼克号灾难中的幸存者。

![](img/223358eb3b5cd279a9a54c9b57a3ee11.png)

作者吉尔金——自己的作品，CC BY-SA 4.0，[https://commons.wikimedia.org/w/index.php?curid=90405437](https://commons.wikimedia.org/w/index.php?curid=90405437)

在我们继续之前，我应该介绍一些术语。树中的每个分支点称为一个节点，代表一个包含起始数据集的部分或全部记录的数据集。按照“树”的主题，顶部(或起始)节点也称为根节点。这包含数据集的所有记录(行、个人，无论您想如何称呼它们)(或者至少是您想要包含的所有记录)。树从这个根开始生长，给我们更多的节点，直到它生成终端节点(即，那些没有被分割的节点)。这些终端节点被称为叶子。上面的树有四片叶子。每个都标有该节点的最终预测。

有两种类型的决策树:分类和回归。分类树预测因变量的类别——是/否、苹果/橘子、死亡/幸存等。回归树预测数值变量的值，类似于线性回归。回归树需要注意的一点是，它们不能像线性回归那样在训练数据集范围之外进行外推。然而，与线性回归不同，回归树可以直接使用分类输入变量。

虽然 Titanic 决策树显示了二叉分裂(每个非叶节点产生两个子节点)，但这不是一般的要求。根据决策树的不同，节点可能有三个甚至更多的子节点。在本文的其余部分，我将集中讨论分类决策树，但是回归树和分类树的基本思想是一样的。

最后，我将提到这个讨论假设在 r 中使用 rpart()函数。我听说 Python 不能直接处理类别变量，但是我对 Python 不太熟悉，尤其是对于数据分析。我相信基本理论是一样的，只是实现方式不同。

决策树是以迭代的方式创建的。首先，扫描变量以确定哪一个给出了最好的分割(稍后将详细介绍)，然后基于该确定将数据集分割成更小的子集。然后再次分析每个子集，创建新的子集，直到算法决定停止。这个决定部分由您为算法设置的参数控制。

这种划分是基于对所讨论的数据集(或子集)的预测有多好。上面的巨大决策树最初基于性别创建了两个子集，这两个子集具有更好的预测价值。如果我们看看泰坦尼克号的例子，icyousee.org 报告说总存活率是 32%。看上面的决策树，我们看到 73%的雌性存活了下来。总结整个树，我们可以生成以下规则:

*   如果乘客是女性，她很可能幸存(73%的几率)
*   如果乘客是男性，那么存活率取决于年龄和机上兄弟姐妹的数量。
*   没有兄弟姐妹的年轻男孩很有可能存活下来(89%的几率)
*   大多数雄性都不走运

从现在开始，我将使用我创建的人工数据集，记录 10 名成年男性和 10 名成年女性的身高和体重。(身高是根据 CDC 研究的实际平均值和百分位数随机生成的——见下文。基于身高值加上身高和体重标准偏差生成体重。见最后的 R 代码。)

![](img/2ff784a6bc580feda20bc18d0caacf9d.png)

作者创建的表格和数据

让我们看看我们是否可以使用这个数据集来预测一个个体是男性还是女性。如果我们随机选择一个人，我们有 50%的机会得到一个男性或女性。有了一个像样的预测模型，我们应该可以做得更好。男性往往更高，更重，所以也许我们可以使用其中一个或两个变量。

决策树使用一种叫做熵的东西来告诉我们我们的预测有多确定。在物理术语中，熵指的是系统中无序的数量。这里也是一样。如果熵为零，就没有无序。只有当我们完全确定从数据集中挑选某人时，我们知道会得到什么，这种情况才会发生。不要期望这在现实生活中发生——永远不要。然而，我们确实希望熵尽可能的低。存在两个类的数据集的熵计算如下:

![](img/2c0e23c8ea6b80a6c34357f0465cea81.png)

作者创建的所有方程式图像

其中:

*   S =数据集的熵
*   p₁ =个人属于 1 类的概率
*   p₂ =个人属于类别 2 的概率

对于原始数据集，S = 1，这是该方程可实现的最大无序度。你自己试试吧。确保您使用的是以 2 为基数的对数。使用基数 2 并不重要，但这是获得最大值 1 的唯一方法。(注意:如果特定数据集只包含一个类，则不存在第二个术语。如果一个因变量有两个以上的可能值，就会增加额外的项。)

在决策树中拆分数据集时，总熵是用子集熵的加权平均值计算的，如下所示:

![](img/1a8e3afb8dc1235c9bda8464c3512ce1.png)

其中:

*   Sₜ =分裂后的总熵
*   Sₓ =子集 x 的熵(即，S₁ =子集 1 的熵)
*   fₓ =进入子集 x 的个体比例

任何点的总熵都是从所有当前叶节点计算的(即使它们以后可能会分裂)。例如，在早期的泰坦尼克树中，总熵在第一次(性别)分裂后有两个( *f S* )项，在第二次(年龄)分裂后有三个项，在第三次(兄弟姐妹)分裂后有四个项。

决策树算法将查看不同变量的不同值，以根据原始值的熵减少来确定哪个给出最佳分割。

让我们用身高/体重数据集来分析一下。先以体重为分界点。女性的平均体重是 177 磅，而男性的平均体重是 201 磅。让我们在两者的中点分开(189 磅)。这将给我们两个子集:

*   子集 1 (< 189 磅，预测女性)有 6 名女性和 3 名男性(总共 9 名)
*   子集 2 (≥ 189 磅，预测男性)有 4 名女性和 7 名男性(共 11 名)

根据这些信息，我们可以计算两个子集的熵:

![](img/06169678d2e587984a1988894c768ef2.png)

和

![](img/c7e4b7bfc453df59b11876959adb24b8.png)

亲爱的读者，我将把证实第二熵的任务留给你。为了计算分裂的总熵，我们使用等式 2。

![](img/01ce880ed3753e5e4bdeba1f03aaaeec.png)

所以通过分解 189 磅的重量，我们得到了熵的轻微减少。决策树算法会检查许多值，看它们是否会给出更好的结果。结果是，如果我们把分裂值降到 186 磅，总熵会降到 0.88。

当然，我们也可以尝试基于身高的拆分。让我们从均值的中点开始。这将给我们一个 66.2 英寸的分割值。通过这种分割，我们得到以下两个子集:

*   子集 1 (< 66.2 英寸，预测为雌性)有 9 只雌性和 2 只雄性(总共 11 只)
*   子集 2 (≥ 66.2 英寸，预测男性)有 1 名女性和 8 名男性(共 9 名)

有了这些信息，我们可以计算这种分裂的熵:

![](img/f9c455c0e499841c470562f04b1a1f9f.png)

这比使用权重进行分割要好得多。但是如果我们将分裂点提高到 68.2 英寸，我们可以做得更好，总熵为 0.51。现在我们的决策树有了第一次分裂。接下来，该算法将检查这些子集中的每一个，以查看它们是否可以再次被分割，这次是基于权重。这样做可能会让我们得到更好的熵，但我们分裂得越多，我们就越有可能过度拟合数据。请记住，我在这里只采样了 20 个人，其中一些看起来像异常值(例如，最高的女性体重最轻)。对于包含许多变量的大型数据集，您可能会得到一个真正混乱的决策树。有几个参数可以用来限制这一点，但这超出了我想在这里讨论的范围。

在以前的帖子中，我谈到了使用 rattle 来帮助学习 r。事实证明，Rattle 包对决策树有很好的绘图功能。这是我通过 Rattle 的决策树算法运行数据集时得到的图像，使用标准参数(注意，它只在一次分割后就停止了):

![](img/a3aaa2a82c028b95fbb0801f561d35ab.png)

作者图片

这个情节有很多特点，我现在就来介绍一下。

*   颜色对应预测的性别(绿色=女性，蓝色=男性)。
*   节点 1(也称为“根节点”)具有 50/50 的性别比例，并且占观察值的 100%。它的绿色意味着如果它必须从这组动物中预测性别，它会选择雌性。
*   在这个节点下面，我们看到第一次拆分。如果高度< 68 inches, then we predict female. Otherwise, we predict male.
*   Node 2 accounts for 65% of the dataset and has a 77/23 split of females to males. Its green color means that everybody in this subset is predicted to be female
*   Node 3 accounts for 35% of the dataset and is 100% male, which is why it’s blue.

The biggest benefit of decision trees is the ease of understanding. They’re easy to read and it’s easy to see how the model made its prediction.

In part 2, we will investigate random forests, and see how they compare to decision trees.

## Further Reading

[](https://www.cdc.gov/nchs/fastats/body-measurements.htm) [## FastStats

### Men: Height in inches: 69.0 Weight in pounds: 199.8 Waist circumference in inches: 40.5 Height in inches: 63.5 Weight…

www.cdc.gov](https://www.cdc.gov/nchs/fastats/body-measurements.htm) [](http://www.icyousee.org/titanic.html) [## Titanic: Demographics of the Passengers

### Demographics of the TITANIC Passengers: Deaths, Survivals, Nationality, and Lifeboat Occupancy I hesitated before…

www.icyousee.org](http://www.icyousee.org/titanic.html) [](https://www.amazon.com/gp/product/1441998896/ref=as_li_tl?ie=UTF8&camp=1789&creative=9325&creativeASIN=1441998896&linkCode=as2&tag=medium0074-20&linkId=42b704775edff7eb2f7eef0d5185e66f) [## Data Mining with Rattle and R: The Art of Excavating Data for Knowledge Discovery (Use R!)

### Amazon.com: Data Mining with Rattle and R: The Art of Excavating Data for Knowledge Discovery (Use R!) (8582569999992)…

www.amazon.com](https://www.amazon.com/gp/product/1441998896/ref=as_li_tl?ie=UTF8&camp=1789&creative=9325&creativeASIN=1441998896&linkCode=as2&tag=medium0074-20&linkId=42b704775edff7eb2f7eef0d5185e66f) 

(Note that the Amazon link above is an affiliate link.)

[](/use-rattle-to-help-you-learn-r-d495c0cc517f) [## Use Rattle to Help You Learn R

### A beginner’s guide

towardsdatascience.com](/use-rattle-to-help-you-learn-r-d495c0cc517f) 

## R Code

**创建数据集**

```
# Data from [https://www.cdc.gov/nchs/data/series/sr_03/sr03-046-508.pdf](https://www.cdc.gov/nchs/data/series/sr_03/sr03-046-508.pdf)
# means are directly retrieved from report
# SDs are estimated from 15th and 85th percentiles
library(dplyr)
set.seed(1)# Weight of Females over 20 - Table 4 - excludes pregnant females
FWnum <- 5386 # number of females in sample
FWmean <- 170.8 # mean weight of females, in pounds
#15% = 126.9
#85% = 216.4
#diff / 2 = 44.75
FWSD <- 44 # estimated std dev, in pounds# Weight of Males over 20 - Table 6
MWnum <- 5085 # number of males in sample
MWmean <- 199.8 # mean weight of males, in pounds
#15% = 154.2
#85% = 243.8
#diff / 2 = 44.8
MWSD <- 44 # estimated std dev, in pounds# Height of Females over 20 - Table 10
FHnum <- 5510 # number of females in sample
FHmean <- 63.5 # mean height of females over 20, in inches
#15% = 60.6
#85% = 66.3
#diff / 2 = 2.85
FHSD <- 2.8 # estimated std dev, in pounds# Height of Males over 20 - Table 12
MHnum <- 5092 # number of females in sample
MHmean <- 69.0 # mean height of females over 20, in inches
#15% = 66.0
#85% = 72.0
#diff / 2 = 3.0
MHSD <- 3 # estimated std dev, in pounds# create 10 normally distributed female heights
FemaleHeight <- round(rnorm(10, mean = FHmean, sd = FHSD), 1)# Calculate weight based on comparison of height to mean height
FemWCorrel <- FemaleHeight/FHmean * FWmean 
# throw in some random deviation based on weight SD
FemWAdj <- rnorm(10, sd = FWSD/2)
FemaleWeight <- round(FemWCorrel + FemWAdj, 0)
F <- data.frame(Height = FemaleHeight, 
                Weight = FemaleWeight, 
                Gender = "F")# create 10 normally distributed male heights
MaleHeight <- round(rnorm(10, mean = MHmean, sd = MHSD), 1)
# Calculate weight based on comparison of height to mean height
MaleWCorrel <- MaleHeight/MHmean * MWmean 
# throw in some random deviation based on weight SD
MaleWAdj <- rnorm(10, sd = MWSD/2)
MaleWeight <- round((MaleWCorrel + MaleWAdj), 0)
M <- data.frame(Height = MaleHeight, 
                Weight = MaleWeight, 
                Gender = "M")df <- rbind(F, M)
```