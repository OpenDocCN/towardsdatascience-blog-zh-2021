# 随机森林和独热编码简介

> 原文：<https://towardsdatascience.com/random-forests-and-one-hot-encoding-introduction-solving-kaggles-titanic-machine-learning-dataset-1607793f6988?source=collection_archive---------10----------------------->

## 解决 Kaggle 的巨大机器学习数据集

![](img/41b76e34e040b64dca853e6d41434b7a.png)

图片由 S.Herman 和 F. Richter 来自 Pixabay

Kaggle 的巨型机器学习数据集——机器学习领域的经典开源介绍。虽然这可能是一个初学者项目，但仍然有一个排行榜(当你继续编写代码时，看到自己的排名上升是很吸引人的)。

首先，您需要直接从 Kaggle 下载“train.csv”和“test.csv”文件。这个可以在这里下载:[https://www.kaggle.com/c/titanic/data](https://www.kaggle.com/c/titanic/data)。请确保文件已放在指定的位置，以便以后提取。

在这场比赛中，一个大型火车数据集描述了那些因泰坦尼克号致命事故而缩短生命的人的各种特征，以及他们如何与生存相关。这包括分类数据，如性别、出发地点和各自的姓名。然而，也有数字数据，如年龄和票价。我们必须使用一次性编码算法来使整个程序和预测更适合分类数据。

我将在项目中使用的软件将是谷歌 Colab。然而，在这种情况下，这并不重要。我们不需要 GPU。使用没有 GPU 的 Jupyter 笔记本电脑也能正常工作。与计算机科学和机器学习中的许多其他项目类似，这个项目中要做的第一件事是对我们需要拉动的导入进行编程。

```
import pandas as pd from sklearn.model_selection import train_test_split from sklearn.ensemble import RandomForestClassifier 
```

Pandas 将用于解析该任务的文件。第二个导入“train_test_split”将用于分割和分离培训和测试文件。我们需要的最后一个导入是 RandomForestClassifier，这是使用随机森林所需的大型总体导入。

正如我所说，我正在使用谷歌 Colab。因此，我需要将程序“安装”到 Google Colab 上来访问 Google Drive。我将用下面的代码做到这一点。

```
from google.colab import drive drive.mount('/content/gdrive')
```

现在，我们必须接受。csv 文件适当。这可以通过下面的代码来完成。

```
train_df = pd.read_csv('/content/gdrive/My Drive/train.csv') test_df = pd.read_csv('/content/gdrive/My Drive/test.csv') 
```

接下来，我将删除各种分类信息。这包括票、船舱和名字。这些名字已被删除，因为它们通常与存活与否无关。然而，如果这是一个广泛深入的分析，一些名字可能会更“重要”例如，基于姓氏或前缀。然而，在很大程度上，这将是一项徒劳无功的工作。此外，机票和客舱信息缺乏竞争力，因此最好忽略这些完整的数据集。

这可以通过下面的代码来完成。

```
train_df = train_df.drop(['Name'],  axis=1) test_df = test_df.drop(['Name'], axis=1) train_df = train_df.drop(['Cabin'],  axis=1) test_df = test_df.drop(['Cabin'], axis=1)train_df = train_df.drop(['Ticket'],  axis=1) test_df = test_df.drop(['Ticket'], axis=1)
```

要查看数据现在的样子，打印出列车数据集合可能是有益的。为此，请键入以下内容。

```
train_df
```

对于测试数据也可以这样做。

```
test_df
```

接下来，我们应该考虑数据集中的“已上船”类别。与机票和客舱数据集类似，缺少值。然而，并没有太多明显的差距。最常见的装载值是“S”，因此对于少数缺少的值，这些值将用“S”填充。这并不精确或完美，但每个值都必须填充。

```
common_value = 'S' data = [train_df, test_df] for dataset in data: dataset['Embarked'] = dataset['Embarked'].fillna(common_value) 
```

现在，我们必须使用一次性编码来组织分类数据。这将应用于剩余的两个分类数据集，即“sex”和“embark”这将应用于“训练”和“测试”数据集。

```
def clean_sex(train_df): try: return train_df[0] except TypeError: return "None"train_df["Sex"] = train_df.Sex.apply(clean_sex) categorical_variables = ['Sex', 'Embarked'] for variable in categorical_variables: train_df[variable].fillna("Missing", inplace=True)   discarded = pd.get_dummies(train_df[variable],prefix = variable)           train_df= pd.concat([train_df, discarded], axis = 1) train_df.drop([variable], axis = 1, inplace = True) 
```

以上应用于“列车”数据。现在，我们必须对“测试”数据集做同样的事情。

```
def clean_sex(test_df):try: return test_df[0] except TypeError: return "None"test_df["Sex"] = test_df.Sex.apply(clean_sex) categorical_variables = ['Sex', 'Embarked']for variable in categorical_variables: test_df[variable].fillna("Missing", inplace=True) discarded = pd.get_dummies(test_df[variable], prefix = variable) test_df= pd.concat([test_df, discarded], axis = 1) test_df.drop([variable], axis = 1, inplace = True) 
```

组织分类数据后，我们现在应该考虑数字数据。对于“年龄”和“费用”数据，存在未被考虑的缺失值。我们需要找到实现数据值的方法，而不是完全忽略这些数据集。为此，我们将取“年龄”和“费用”的平均值，并将其用于我们没有的值。我们必须对“训练”和“测试”数据都这样做。

```
train_df["Age"].fillna (train_df.Age.mean(), inplace = True)test_df["Age"].fillna (test_df.Age.mean(), inplace = True) train_df["Fare"].fillna (train_df.Fare.mean(), inplace = True)test_df["Fare"].fillna (test_df.Fare.mean(), inplace = True)
```

我们现在应该再次查看我们的数据，以再次检查所有适当的申请是否已经完成。

这将打印“列车”操纵的数据。

```
train_df
```

这将打印“测试”处理过的数据。

```
test_df
```

现在，因为年龄通常表示为整数，所以最好将其转换为基于整数的描述。我们同样需要对“训练”和“测试”数据集都这样做。

```
train_df = train_df.round({'Age':0})test_df = test_df.round({'Age':0})
```

此时，我们已经在“体面的水平”上操纵了我们的数据。虽然不完全精确，但我们可以开始使用随机森林算法来制定预测。

因此，对于我们的局部估计，我们将删除名为“X_train”的新变量的“幸存”值。然后，对于“Y_train”，我们将只包括“幸存”然后，我们直接让 X_test 等于“test_df”。最后，打印出形状。

```
X_train = train_df.drop("Survived", axis=1) Y_train = train_df["Survived"] X_test  = test_df X_train.shape, Y_train.shape, X_test.shape
```

为了仔细检查，打印出“x_train”可能是有用的同样，这可以简单地用下面的命令来完成。

```
X_train
```

接下来，我们应该初始化我们的随机森林。我们还应该使用各种参数来改进我们的算法。

```
random_forest = RandomForestClassifier(criterion='gini', n_estimators=700,    min_samples_split=10,     min_samples_leaf=1,     max_features='auto',     oob_score=True,     random_state=1, n_jobs=-1) random_forest.fit(X_train, Y_train)Y_pred = random_forest.predict(X_test) acc_random_forest = round(random_forest.score(X_train, Y_train) * 100, 2) acc_random_forest 
```

“n_estimators”值表示随机森林中使用的估计值的数量(森林中的树木数量)。在这种情况下，700 是一个运行良好的值。“Min_samples_split”是分离和拆分内部节点所需的样本数(最小值)。“Min_samples_leaf”表示成为叶节点所需的最小样本数。“Max_features”用于描述分割节点时考虑的随机特征子集的大小。设置为“自动”时，没有特征子集。“oob_score”设置为 true 时，将尝试使用“Out of Bag”分数来验证随机森林模型。当“n_jobs”设置为-1 时，将使用所有处理器来运行。

上述代码的最后一段符合模型，并生成一个与 X_test 数据一起工作的预测(y_pred)。随机森林的准确性将被打印出来。本地测试该数据产生大约 90%的准确度(91.81%)。然而，这只是基于从训练数据集中分离生存值的局部预测。为了得到准确的估计，我们需要直接提交给 Kaggle。为此，编写以下代码以获取“submission.csv”文件。

```
submission = pd.DataFrame({ "PassengerId": test_df["PassengerId"], "Survived": Y_pred})submission.to_csv('submission.csv', index=False)
```

**结论:**

Kaggle 产生的精度值为“0.79186”或“79.186%。”当我使用这个预测提交时，我在 19649 人中排名 1573，大约是前 8%。本地预测和 Kaggle 预测之间的区别在于，“训练”数据被修改为“测试混合”数据集，而 Kaggle 使用实际的“测试数据”。然而，在这两种情况下，都有一定程度的随机化。

总的来说，使用随机森林似乎产生了不错的结果。在很大程度上，这些预测是准确的。然而，为了简单起见，数据被丢弃了，这无疑损害了这一预测的整体实力。此外，虽然随机森林适用于低级别估计，但使用其他算法会产生更可靠、更准确的结果，并且通常与更长的训练时间相关。