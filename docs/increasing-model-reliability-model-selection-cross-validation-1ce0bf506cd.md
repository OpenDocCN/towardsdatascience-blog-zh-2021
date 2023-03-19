# 增加模型可靠性:模型选择——交叉验证——

> 原文：<https://towardsdatascience.com/increasing-model-reliability-model-selection-cross-validation-1ce0bf506cd?source=collection_archive---------18----------------------->

## 在一个视图中使用 python 实现来增加结果可靠性的模型选择/类型

至关重要的是，机器学习中准备的模型为外部数据集提供可靠的结果，即泛化。在数据集的一部分被保留作为测试并且模型被训练之后，从测试数据获得的准确度在测试数据中可能是高的，而对于外部数据是非常低的。例如，如果在带有 x，y，z 标签的数据集中只选择带有 x 标签的数据作为随机选择的测试数据集，这实际上只给出了 x 标签的准确性，而不是模型，但这可能不会被开发人员注意到。这个模型不是一般化的，肯定是一个不理想的情况。本文包含不同的配置，从中可以选择训练数据和测试数据，以增加模型结果的可靠性。这些方法对于模型正确响应开放世界的项目是必不可少的。

```
**Table of Contents** 
**1.** Train Test Split
**2.** Cross Validation
**2.1.** KFold Cross Validation
**2.2.** Stratified KFold Cross Validation
**2.3.** LeaveOneOut Cross Validation
**2.4.** Repeated KFold Cross Validation
**2.5.** ShuffleSplit Cross Validation
**2.6.** Group KFold Cross Validation
```

![](img/2d28851226473d81c5e8cb617d343222.png)

图片由[暹罗谭](https://unsplash.com/@siamialtrice_?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

# 1.列车测试分离

在机器学习应用中，用数据集训练模型。通过这种训练，模型学习特征和输出(目标)之间的关系。然后，使用相同格式的另一个数据集评估模型的准确性。训练测试分离以用户选择的速率将数据分离成训练数据和测试数据。

```
IN[1]
from sklearn.datasets import load_iris
from sklearn.model_selection import train_test_split
iris = load_iris()
x_train,x_test,y_train,y_test = train_test_split(iris.data, iris.target, test_size=0.2, random_state=2021)
print("shape of x_train",x_train.shape)
print("shape of x_test",x_test.shape)
**OUT[1]
shape of x_train (120, 4)
shape of x_test (30, 4)**
```

如 OUT[1]所示，数据集分为 20%的测试数据和 80%的训练数据。

# 2.交叉验证

虽然用 *train_test_split* 分离模型看起来很有用，但是从测试数据集得到的准确率可能并不能反映真实情况。例如，当我们用 *train_test_split* 随机分离包含 A、B、C 标签的数据集时，可以将包含 A、B 标签的数据分离为 train，将所有 C 标签分离为 test。在这种情况下，训练数据和测试数据之间存在差异，这对于模型的成功会产生误导。因此，当将数据集分成训练数据和测试数据时，使用不同的方法。现在让我们检查基于统计数据的交叉验证的类型，并使用 scikit learn 库轻松实现。

## 2.1.交叉验证

数据集被分成用户选择的数(k)。该模型被分割为多个部分，每个部分被称为折叠，并且在每个分割中不同的折叠被用作测试数据集。例如，如果一个包含 150 个数据的数据集被设置为 k=3，那么模型会给出 3 个精度值，而不同的 1/3 部分(0–50、50–100、100–150)会被用来测试每个精度。剩余的 2/3 部分用于每次迭代中的训练。

```
IN[2]
heart_dataset=pd.read_csv('heart.csv')
heart_data   =heart_dataset.drop('output',axis=1)
heart_target =heart_dataset['output']
rb=RobustScaler()
heart_robust=rb.fit_transform(heart_data)IN[3]
kf = KFold(n_splits=5)
i=1
for train_data,test_data in kf.split(X=heart_data, y=heart_target):
    print('iteration',i)
    print(train_data[:10],"length:", len(train_data))
    print(test_data[:10],"length:", len(test_data))
    print("**********************************")
    i +=1
**OUT[3]
iteration 1
[61 62 63 64 65 66 67 68 69 70] train_length: 242
[0 1 2 3 4 5 6 7 8 9] test_length: 61
**********************************
iteration 2
[0 1 2 3 4 5 6 7 8 9] train_length: 242
[61 62 63 64 65 66 67 68 69 70] test_length: 61
**********************************
iteration 3
[0 1 2 3 4 5 6 7 8 9] train_length: 242
[122 123 124 125 126 127 128 129 130 131] test_length: 61
**********************************
iteration 4
[0 1 2 3 4 5 6 7 8 9] train_length: 243
[183 184 185 186 187 188 189 190 191 192] test_length: 60
**********************************
iteration 5
[0 1 2 3 4 5 6 7 8 9] train_length: 243
[243 244 245 246 247 248 249 250 251 252] test_length: 60
************************************
```

心脏病数据集由 14 列和 303 行组成。所有特征值都是数字，因此应用了*鲁棒定标器*。数据集的输出由 0 和 1 组成。OUT[3]显示了每个分割中测试和训练数据的前 10 个数据。可以看出，来自第一次分割的测试数据从数据集的第一个数据开始；第二次分割的测试数据从 61 开始。数据；第三次分割的测试数据从 122 开始。数据；来自数据的测试数据第四次分割从数据 183.data 开始，最后一次分割从数据 243 开始。数据

```
IN[4]
scores=cross_val_score(LogisticRegression(),heart_robust,heart_target,cv=5)
print(scores)
print("mean accuracy:",scores.mean())
**OUT[4]
[0.83606557 0.86885246 0.83606557 0.86666667 0.76666667]
mean accuracy: 0.8348633879781422**
```

303 个数字数据的心脏病数据集已经用 k=5 的逻辑回归分裂了 5 次。每个分割的逻辑回归准确度分别为[0.83606557 0.86885246 0.83606557 0.86666670.7666667]。

**KFold 交叉验证与 Shuffle**

在 k 倍交叉验证中，数据集被按顺序分成 k 个值。当 KFold 选项中的*洗牌*和 *random_state* 值被设置时，数据被随机选择:

```
IN[5]
kfs = KFold(n_splits=5, shuffle=True, random_state=2021)
scores_shuffle=cross_val_score(LogisticRegression(),heart_robust,heart_target,cv=kfs)
print(scores_shuffle)
print("mean accuracy:",scores_shuffle.mean())
**OUT[5]
[0.83606557 0.78688525 0.78688525 0.85       0.83333333]
mean accuracy: 0.8186338797814209**
```

## 2.2.分层交叉验证

数据集被分成用户选择的数字(k)部分。与 KFold 不同的是，每个目标也是由 k 进行拆分和合并的，比如我们考虑 iris 数据集(前 50 个数据 iris setosa50-100 朵杂色鸢尾，100-150 朵海滨鸢尾)并通过选择 k 值 5:

```
IN[6]
iris_dataset=pd.read_csv('iris.csv')
iris_data   =iris_dataset.drop('Species',axis=1)
iris_data   =iris_data.drop(['Id'],axis=1)
iris_target =iris_dataset['Species']IN[7]
skf = StratifiedKFold(n_splits=5)
i=1
for train_data,test_data in skf.split(X=iris_data, y=iris_target):
    print('iteration',i)
    print(test_data,"length", len(test_data))
    print("**********************************")
    i +=1
**OUT[7]
iteration 1
[  0   1   2   3   4   5   6   7   8   9  50  51  52  53  54  55  56  57 58  59 100 101 102 103 104 105 106 107 108 109] length 30
**********************************
iteration 2
[ 10  11  12  13  14  15  16  17  18  19  60  61  62  63  64  65  66  67 68  69 110 111 112 113 114 115 116 117 118 119] length 30
**********************************
iteration 3
[ 20  21  22  23  24  25  26  27  28  29  70  71  72  73  74  75  76  77 78  79 120 121 122 123 124 125 126 127 128 129] length 30
**********************************
iteration 4
[ 30  31  32  33  34  35  36  37  38  39  80  81  82  83  84  85  86  87 88  89 130 131 132 133 134 135 136 137 138 139] length 30
**********************************
iteration 5
[ 40  41  42  43  44  45  46  47  48  49  90  91  92  93  94  95  96  97 98  99 140 141 142 143 144 145 146 147 148 149] length 30
************************************
```

当分析测试数据集时，每个目标的前 10 个数据(0–10；50–60;100–110 ),第二次迭代中每个目标的第二个 1/5 切片(20–30；60- 70;110–120)等等。每次迭代中的剩余数据用于训练，并且线性回归用于每次迭代中的训练数据，自然地，获得了 5 个不同的精度。

```
IN[8]
lr=LogisticRegression()
le=LabelEncoder()
iris_labels=le.fit_transform(iris_target)
rb=RobustScaler()
iris_robust=rb.fit_transform(iris_data)
iris_robust=pd.DataFrame(iris_robust)IN[9]
scores_skf = []
i = 1
for train_set, test_set in skf.split(X=iris_robust, y=iris_labels):
    lr.fit(iris_robust.loc[train_set], iris_labels[train_set])
    sco = lr.score(iris_robust.loc[test_set], iris_labels[test_set])
    scores_skf.append(sco)
    i += 1
print(scores_skf)
print("mean accuracy:",sum(scores_skf) / len(scores_skf))
**OUT[9]**
**[0.9, 0.9666666666666667, 0.9333333333333333, 0.9333333333333333, 0.9666666666666667]
mean accuracy: 0.9400000000000001**
```

由于 *"'numpy.ndarray '对象没有属性' loc'"* ， *iris_robust* 的 numpy 数组被转换为 pandas DataFrame。

分层 KFold 交叉验证可以轻松实现，如 Scikit learn 中的 *cross_val_score* 所示。

```
IN[10]
score_skf=cross_val_score(lr,iris_robust,iris_labels,cv=skf)
print(score_skf)
print("mean accuracy:",score_skf.mean())
**OUT[10]
[0.9        0.96666667 0.93333333 0.93333333 0.96666667]
mean accuracy: 0.9400000000000001**
```

两个结果是一样的。

> 由于 iris 数据集中的目标值相等(50–50–50 ),因此每个目标值的位置相等。但是，如果数据集中的标注比例不同，则每个折叠都将以此比例包含数据。例如，如果 label-x 有 100 个数据，label-y 有 900 个数据，则每个文件夹将包含 90% label-y 数据和 10% label-x 数据。
> 
> 查看 scikit learn 的[网站](https://scikit-learn.org/stable/modules/generated/sklearn.model_selection.StratifiedKFold.html)中的 split 方法，看到 y 值应该是(n_samples，)，所以设计为用标签编码器运行，而不是 OneHotEncoder。

## 2.3.LeaveOneOut 交叉验证

每个数据被认为是一个折叠，所以 k 的值等于数据的数量。每个数据被逐一分离，并用剩余的数据训练模型。用训练好的模型测试分离的数据。如果我们考虑虹膜数据集:

```
IN[11]
loo = cross_val_score(estimator=LogisticRegression(), X=iris_robust, y=iris_labels,
                               scoring='accuracy', cv=LeaveOneOut())print(loo,"len of loo=",len(loo))
print("mean accuracy:",loo.mean())
**OUT[11]
[1\. 1\. 1\. 1\. 1\. 1\. 1\. 1\. 1\. 1\. 1\. 1\. 1\. 1\. 1\. 1\. 1\. 1\. 1\. 1\. 1\. 1\. 1\. 1\. 1\. 1\. 1\. 1\. 1\. 1\. 1\. 1\. 1\. 1\. 1\. 1\. 1\. 1\. 1\. 1\. 1\. 0\. 1\. 1\. 1\. 1\. 1\. 1\. 1\. 1\. 1\. 1\. 1\. 1\. 1\. 1\. 1\. 1\. 1\. 1\. 1\. 1\. 1\. 1\. 1\. 1\. 1\. 1\. 1\. 1\. 0\. 1\. 1\. 1\. 1\. 1\. 1\. 0\. 1\. 1\. 1\. 1\. 1\. 0\. 1\. 1\. 1\. 1\. 1\. 1\. 1\. 1\. 1\. 1\. 1\. 1\. 1\. 1\. 1\. 1\. 1\. 1\. 1\. 1\. 1\. 1\. 0\. 1\. 1\. 1\. 1\. 1\. 1\. 1\. 1\. 1\. 1\. 1\. 1\. 0\. 1\. 1\. 1\. 1\. 1\. 1\. 1\. 1\. 1\. 1\. 1\. 1\. 1\. 0\. 0\. 1\. 1\. 1\. 1\. 1\. 1\. 1\. 1\. 1\. 1\. 1\. 1\. 1\. 1\. 1.] len of loo= 150
mean accuracy: 0.9466666666666667**
```

可以看出，由 150 个数据集组成的虹膜数据集用 *LeaveOneOut* 建模，并应用逻辑回归。每个数据都是分开的，并用剩余的数据训练模型。总的来说，该模型已经训练了 150 次，并且做出了 150 个预测(针对每个数据)。全部取平均值。

![](img/d61efa4caa7c829bf504301ecd94717b.png)

照片由[布莱恩·苏曼](https://unsplash.com/@briansuman?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

## 2.4.重复 KFold 交叉验证

根据用户选择的值，重复 k 倍交叉验证的次数。对于虹膜数据集:

```
IN[12]
rkf = cross_val_score(estimator=LogisticRegression(), X=iris_robust, y=iris_labels, scoring='accuracy', cv=RepeatedKFold(n_splits=5, n_repeats=5))
print("accuracy:", rkf)
print("mean accuracy",rkf.mean())
**OUT[12]
accuracy: 
[0.96666667 0.9       0.93333333 0.93333333 0.93333333 0.9
 0.93333333 1\.         0.96666667 0.96666667 0.86666667 0.96666667
 0.96666667 0.96666667 1\.         0.96666667 0.9        1.
 0.93333333 0.93333333 0.86666667 0.96666667 1\.         0.9
 0.96666667]
mean accuracy 0.9453333333333331**
```

数据集被分成 5 部分，算法被拟合 5 次。结果，获得了 25 个精度值。

对于分层折叠，也可以这样做:

```
IN[13]
rskf = cross_val_score(estimator=LogisticRegression(), X=iris_robust, y=iris_labels, scoring='accuracy', cv=RepeatedStratifiedKFold(n_splits=5, n_repeats=5))
print("accuracy", rskf)
print("mean accuracy",rskf.mean())
**OUT[13]
accuracy
[0.96666667 0.9        0.96666667 0.96666667 0.96666667 0.9
 0.96666667 0.96666667 0.9        0.96666667 0.96666667 0.96666667
 0.9        0.96666667 0.96666667 0.9        0.93333333 1.
 0.96666667 0.96666667 0.96666667 0.93333333 1\.         0.9
 0.93333333]
mean accuracy 0.9493333333333333**
```

## 2.5.洗牌分割交叉验证

使用 *n_splits* 设置数据集的迭代次数，并以用户指定的比率为每次分割随机选择训练和测试数据:

```
IN[14]
shuffle_split = ShuffleSplit(test_size=.4, train_size=.5, n_splits=10)
scores_ss = cross_val_score(LogisticRegression(), iris_robust, iris_labels, cv=shuffle_split)
print("Accuracy",scores_ss)
print("mean accuracy:",scores_ss.mean())
**OUT[14]
Accuracy
[0.9        0.93333333 0.88333333 0.9        0.95       0.95
 0.93333333 0.91666667 0.95       0.95      ]
mean accuracy: 0.9266666666666665**
```

对于分层洗牌拆分，可以进行相同的过程:

```
IN[15]
shuffle_sfs=StratifiedShuffleSplit(test_size=.4, train_size=.5, n_splits=10)
scores_sfs = cross_val_score(LogisticRegression(), iris_robust, iris_labels, cv=shuffle_sfs)
print("Accuracy",scores_sfs)
print("mean accuracy:",scores_sfs.mean())
**OUT[15]
Accuracy
[0.88333333 0.93333333 0.93333333 0.93333333 0.91666667 0.96666667
 0.96666667 0.96666667 0.88333333 0.9       ]
mean accuracy: 0.9283333333333333**
```

## 2.6.集团交叉验证

当从同一个对象接收到多个数据时使用。例如，在医学数据中，为了模型的泛化，最好在训练数据集中具有来自同一患者的多个图像。为了实现这一点，可以使用 *GroupKFold* ，它接受一个组数组作为参数，我们可以用它来指示图像中的人是谁。这里的组数组指定了在创建训练集和测试集时不应拆分的数据组，并且不应与类标签混淆。使用 GroupKFold，该组或者在训练集上，或者在测试集上。

## 回到指南点击[此处](https://ibrahimkovan.medium.com/machine-learning-guideline-959da5c6f73d)。

<https://ibrahimkovan.medium.com/machine-learning-guideline-959da5c6f73d> 