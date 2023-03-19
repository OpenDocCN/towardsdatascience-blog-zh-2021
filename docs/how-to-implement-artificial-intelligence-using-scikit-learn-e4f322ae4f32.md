# 如何使用 scikit-learn 实现人工智能

> 原文：<https://towardsdatascience.com/how-to-implement-artificial-intelligence-using-scikit-learn-e4f322ae4f32?source=collection_archive---------24----------------------->

## [入门](https://towardsdatascience.com/tagged/getting-started)

## 这篇介绍 power Python 工具的文章将让你很快应用人工智能

![](img/ea89632e6257babdbd550cd9ffbb7888.png)

图片来源:[阿迈德·加德](https://pixabay.com/users/ahmedgad-9403351/?utm_source=link-attribution&utm_medium=referral&utm_campaign=image&utm_content=3501528)在[皮克斯拜](https://pixabay.com/?utm_source=link-attribution&utm_medium=referral&utm_campaign=image&utm_content=3501528)

数据科学家将人工智能(AI)用于一系列强大的用途。它现在运行控制系统，减少建筑能耗，它为购买衣服或观看节目提供建议，它有助于改善农业实践和我们可以种植的食物量，有一天它甚至可能为我们驾驶汽车。知道如何使用这些工具将使你有能力解决下一代社会的技术挑战。

幸运的是，对于已经熟悉 Python 和数据分析的人来说，开始使用人工智能并不那么具有挑战性。您可以利用强大的 scikit-learn 包来为您完成大部分艰苦的工作。

## 什么是 scikit-learn？

scikit-learn 是一个 python 包，旨在促进机器学习和人工智能算法的使用。它包括用于分类、回归和聚类的算法，包括流行的随机森林和梯度推进算法。这个软件包被设计成可以很容易地与常见的科学软件包 numpy 和 scipy 接口。尽管不是专门为熊猫设计的，但它也能很好地与熊猫互动。

scikit-learn 还包括一些有用的工具，有助于使用机器学习算法。开发准确预测系统行为的机器学习管道需要[将数据分成训练和测试集](/how-to-choose-between-multiple-models-a0c274b4228a)，以及对算法进行评分以确定它们的运行情况，并[确保模型既不过度拟合也不欠拟合](/a-primer-on-model-fitting-e09e757fe6be)。scikit-learn 界面包括执行所有这些任务的工具。

## scikit-learn 算法是如何工作的？

开发和测试 scikit-learn 算法可以分三个一般步骤进行。它们是:

1.  使用描述需要模型预测的现象的现有数据集来训练模型。
2.  在另一个现有数据集上测试该模型，以确保其性能良好。
3.  使用模型来预测项目所需的现象。

scikit-learn 应用程序编程接口(API)提供了通过单个函数调用来执行这些步骤的命令。所有的 scikit-learn 算法在这个过程中使用相同的函数调用，所以如果你学会了一个，你就学会了所有的。

训练 scikit-learn 算法的函数调用是*。fit()* 。来训练你称为。函数，并将训练数据集的两个组件传递给它。这两个组件是 x 数据集，提供描述数据集的*特征*的数据，以及 y 数据，提供描述系统的*目标*的数据(*特征*和*目标*是机器学习术语，本质上表示 x 和 y 数据)。然后，该算法创建由所选算法和模型参数确定的数学模型，该模型尽可能匹配所提供的训练数据。然后，它将参数存储在模型中，允许您根据项目需要调用模型的 fit 版本。

测试模型拟合度的函数是*。score()* 。要使用该函数，您需要再次调用该函数，并传递表示特征的 x 数据集和表示目标的相应 y 数据集。与定型模型时相比，使用不同的数据集(称为测试数据集)非常重要。当对训练数据评分时，模型很可能得分很高，因为它在数学上被迫与该数据集匹配。真正的测试是模型在不同数据集上的表现，这是测试数据集的目的。调用*时。score()* 函数 scikit-learn 将返回 [r 值](https://en.wikipedia.org/wiki/Coefficient_of_determination#:~:text=R2%20is%20a%20statistic,predictions%20perfectly%20fit%20the%20data.)，说明模型使用提供的 x 数据集预测提供的 y 数据集的效果如何。

您可以使用 scikit-learns *预测给定输入的系统输出。预测()*函数。重要的是，您只能在拟合模型后执行此操作。拟合是指如何调整模型以匹配数据集，因此如果不首先进行拟合，模型将不会提供有价值的预测。一旦模型合适，您就可以将一个 x 数据集传递给*。predict()* 函数，它将返回模型预测的 y 数据集。通过这种方式，你可以预测一个系统未来的行为。

这三个函数构成了 scikit-learn API 的核心，对您将人工智能应用于技术问题大有帮助。

## 如何创建培训和测试数据集？

创建单独的训练和测试数据集是训练人工智能模型的关键组成部分。如果不这样做，我们就无法创建一个与我们试图预测的系统相匹配的模型，也无法验证其预测的准确性。幸运的是，scikit-learn 再次提供了一个有用的工具来促进这一过程。那个工具就是[*train _ test _ split()*](https://scikit-learn.org/stable/modules/generated/sklearn.model_selection.train_test_split.html)函数。

*train_test_split()* 做的和听起来一样。它将提供的数据集分成训练数据集和测试数据集。您可以使用它来创建所需的数据集，以确保您的模型正确预测您正在研究的系统。您向 *train_test_split()* 提供一个数据集，它提供您需要的训练和测试数据集。然后，它返回拆分为定型数据集和测试数据集的数据集，您可以使用这些数据集来开发您的模型。

使用 *train_test_split()* 的时候有几个需要注意的地方。首先， *train_test_split()* 本质上是随机的。这意味着，如果使用相同的输入数据多次运行，train_test_split()将不会返回相同的训练和测试数据集。如果您想要测试模型准确性的可变性，这可能是好的，但是如果您想要在模型上重复使用相同的数据集，这也可能是不好的。为了确保每次都能得到相同的结果，您可以使用 *random_state* 参数。随机状态设置将强制它在每次运行时使用相同的随机化种子，并提供相同的数据集分割。当使用 random_state 时，习惯上将其设置为 42，这可能是对《银河系漫游指南》的幽默回应，而不是出于任何技术原因。

## 这两者是如何结合在一起的？

总之，这些工具创建了一个简化的界面来创建和使用 scikit-learn 工具。让我们以 scikit-learn 的 [*线性回归*](https://scikit-learn.org/stable/modules/generated/sklearn.linear_model.LinearRegression.html) 模型为例来讨论一下。

要实现这一过程，我们必须首先导入所需的工具。它们包括 scikit-learn 模型、 *train_test_split()* 函数和用于数据分析过程的 pandas。这些函数按如下方式导入:

```
from scikit-learn.linear_model import LinearRegression
from scikit-learn.model_selection import train_test_split
import pandas as pd
```

然后我们可以读入一个数据集，这样它就可以用于训练和测试模型。我创建了一个[真实数据集](https://peter-grant.my-online.store/HPWH_Performance_Map_Tutorial_Data_Set/p6635995_20036443.aspx)，展示热泵热水器(HPWHs)的[性能作为其运行条件的函数](/tutorial-automatically-creating-a-performance-map-of-a-heat-pump-water-heater-7035c7f208b0)，专门帮助人们学习数据科学和工程技能。假设您已经下载了数据集，并将其保存在与脚本相同的文件夹中，您可以使用下面的代码行打开它。如果没有，您可以根据需要调整这些步骤，在您喜欢的任何数据集上进行练习。

```
data = pd.read_csv('COP_HPWH_f_Tamb&Tavg.csv', index_col = 0)
```

下一步是将数据集分成 X 和 y 数据。为此，我们创建新的数据框，指定代表要素和目标的数据集列。在 HPWHs 的情况下，特征是水箱温度和环境温度，而目标是电力消耗。数据集包含八列，显示储水箱中八个不同深度的水温，每一列名为“Tx (deg F)”，其中 x 是代表测量位置的数字。它还包含一个列，显示热水器周围空间的测量环境温度，名为“T_Amb(华氏度)”。最后，数据集包含一个存储电力消耗数据的列，称为“P_Elec (W)”。在这种情况下，过滤我们的数据集也很重要，这样我们只在耗电时使用数据。如果我们跳过这一步，我们将把非线性引入线性模型，这将使模型失败。

我们可以使用以下代码完成所有这些步骤:

```
# Filter the data to only include points where power > 0
data = data[data['P_Elec (W)'] > 0]# Identify X columns and create the X data set
X_columns = ['T_Amb (deg F)']
for i in range(1, 9):
    X_columns.append('T{} (deg F)'.format(i))

X = data[X_columns]# Create the y data set
y = data['P_Elec (W)']
```

现在我们有了 X 和 y 数据集，我们可以将这些 X 和 y 数据集分成训练和测试数据集。这可以通过调用 scikit-learn 的*函数 train_test_split()* 来完成，如下所示。

```
X_train, X_test, y_train, y_test = train_test_split(X, y, random_state = 42)
```

既然我们已经准备好了训练和测试数据集，我们可以创建并拟合数据集的*线性回归*模型。为此，我们首先创建模型的一个实例，然后调用*。fit()* 功能如下。

```
model = LinearRegression()
model = model.fit(X_train, y_train)
```

注意，这个实现使用了线性回归模型的默认参数。这可能会也可能不会产生与数据的良好拟合，我们可能需要更改参数来获得良好的拟合。我将在以后的文章中提供实现这一点的高级方法，但是现在使用默认参数就足以学习这些概念了。

下一步是在测试数据集上对模型进行评分，以确保它很好地符合数据集。你可以通过调用*来实现。score()* 并通过测试数据。

```
score = model.score(X_test, y_test)
```

如果模型在测试数据集上得分很高，那么你就有机会拥有一个训练有素的、适合数据集的模型。如果没有，那么您需要考虑收集更多的数据，调整模型的参数，或者使用完全不同的模型。

如果模型运行良好，那么您就可以声明模型可以使用，并开始预测系统的行为。因为我们现在没有额外的数据集可以预测，所以我们可以简单地预测测试数据集的输出。为此，您调用了*。预测()*功能如下。

```
predict = model.predict(X_test)
```

当暴露于 X_test 定义的输入时，预测变量现在将保存系统的预测输出。然后，您可以使用这些输出直接与 y test 中的值进行比较，使您能够更仔细地研究模型拟合和预测准确性。

## 这款车型表现如何？

由于我们计算了模型的分数，并将其保存到变量分数中，因此我们可以很快看到模型对 HPWH 耗电量的预测有多好。在这种情况下，模型的得分为 0.58。

r 是从 0 到 1 的度量。零表示该模型根本不能解释观察到的系统行为。一个表明模型完美地解释了系统的行为。0.58 的 r 值表明该模型解释了一半以上的观察到的行为，这不是很好。

我上面提到的三个潜在改进是:

*   收集更多的数据，
*   调整模型的参数，
*   使用完全不同的模型。

我们当然可以收集更多的数据或调整*线性回归*模型的参数，但这里的核心问题可能是热泵功耗和水温之间的关系是非线性的。线性模型很难预测非线性的东西！

我们可以使用为非线性系统设计的模型来尝试同样的方法，看看我们是否能得到更好的结果。一个可能的模型是*随机森林回归器*。我们可以通过在脚本末尾添加以下代码来尝试。

```
from sklearn.ensemble import RandomForestRegressormodel = RandomForestRegressor()
model.fit(X_train, y_train)
score = model.score(X_test, y_test)
```

这种方法得到了非常高的分数 0.9999，这在另一方面是可疑的。这个模型很有可能对数据集过度拟合，实际上在未来不会产生现实的预测。不幸的是，给定可用的数据集，我们无法真正确定这一点。如果您要使用这个模型来开始预测系统，您将需要监控它，以查看它在更多数据可用时的表现，并继续训练它。根据测量数据绘制预测图还可以洞察模型的行为。

对于这个特定数据集的例子，我会说我会信任这个模型。这是因为该数据集不包含实际测量数据，而是我通过实施回归方程创建的示例数据集，以显示 HPWH 在这些条件下的表现。这意味着 *RandomForestRegressor* 可能与数据匹配得很好，因为它确定了我用来创建数据集的方程。

有了这些，你就可以开始使用 scikit-learn 来实现机器学习和人工智能了！如果您记得所有 scikit-learn 算法都使用 *fit()* 、 *score()* 和 *predict()* 函数，并且您可以使用 *train_test_split()* 创建您的数据集，那么您就踏上了学习不同算法和预测实际系统行为的道路。