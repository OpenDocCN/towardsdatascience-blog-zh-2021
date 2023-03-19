# 平衡您的数据…

> 原文：<https://towardsdatascience.com/balancing-your-data-bd97123a7aaf?source=collection_archive---------46----------------------->

![](img/285736fa94cae6471a13f015dbcd3c1e.png)

罗伯特·阿纳奇在 [Unsplash](https://unsplash.com/s/photos/stone-balance?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上的照片

*‘如果你被问题的大小压垮了，把它分成小块……’*

机器学习中的类不平衡是一个重要的问题，也是处理数据集时最受关注的问题之一。具有类不平衡的数据集往往更具欺骗性，如果处理不当，其结果可能会导致错误的决策。

最近，我碰巧在一个具有显著的类不平衡的数据集中工作。由于我不是数据生成技术的狂热爱好者，并且由于不平衡严重，dint 希望使用分层 K-fold 之类的技术，所以我使用了一种比传统技术好不了多少的技术来帮助模型学习数据。也许我使用这个技术有点晚了，但我想我会在这里分享它，以便它对某人来说是新的和有用的。

```
import pandas as pdtrain=pd.read_csv('/Desktop/Files/train_data.csv')
print(train['Top-up Month'].value_counts())No Top-up Service    106677
 > 48 Months           8366
36-48 Months           3656
24-30 Months           3492
30-36 Months           3062
18-24 Months           2368
12-18 Months           1034
Name: Top-up Month, dtype: int64
```

以上是来自训练数据的因变量的值计数。从上面可以清楚地看出，在“无充值服务”类别和其他类别之间存在严重的类别不平衡。

## 方法…

因此，我在这里采用的方法是将多数类数据分成大小为“n”的小折叠。每个数据块(文件夹)将具有与多数类相关“k”个数据点。所有其他次要类别的数据点被组合在一起(“m”)

```
def chunks(df,folds):

    df_no_topup=df.loc[df['Top-up Month']==0]
    df_topup=df.loc[df['Top-up Month']==1]

    recs_no_topup=int(df.loc[df['Top-up Month']==0].shape[0]/folds)

    start_no_topup=0
    stop_no_topup=recs_no_topup
    list_df=[]

    for fold in range(0,folds):
        fold_n=df_no_topup.iloc[start_no_topup:stop_no_topup,:]
        start_no_topup=stop_no_topup
        stop_no_topup=start_no_topup+recs_no_topup
        df=pd.concat([fold_n,df_topup],axis=0)
        list_df.append(df)
    return list_df
```

上面的代码片段将主类数据分成多个文件夹，每个文件夹包含相同数量的数据点。然后，多数类数据的第一个 fold(在上面的代码中称为 fold_n)与次要类(df_topup)的所有数据连接在一起。这发生在一个循环中，并且该循环将继续，直到等于折叠“n”的大小。可以看出，每次，多数类数据的新块都与次要类的相同(完整)数据连接，从而允许模型在训练阶段训练相等比例的数据。

```
Major class initially - 106677
Fold size(n) - 5
Major class data(k) per fold=106677/5 - 21335
Minor class(all minor classes combined) - 21978
Total data per fold(major+minor) - 43313
```

下面的代码片段可以验证这一点。

```
list_data=chunks(df_train_main,5)
list_data_shape=[df.shape for df in list_data]
print(list_data_shape)[(43313, 6), (43313, 6), (43313, 6), (43313, 6), (43313, 6)]
```

现在可以看到，一个具有严重类别不平衡问题的大数据集已被分成 5 个类别不平衡程度较轻的小数据集(因为一些次要类别与主要类别相比仍然不平衡)

然而，以下是我从上述方法中认识到的两个问题:

*   添加到每个文件夹的多数类数据的分布。由于我们只是将整个主要类别数据划分为多个折叠(n ),每个折叠大小相等(k ),因此任何折叠中主要类别数据的分布可能不是训练数据中主要类别总体数据的样本表示。
*   在需要解决的所有问题中，类别(12-18 个月)和主要类别(“无充值服务”)之间仍然存在不平衡。但是在二进制类的情况下，这种方法效果更好。

然后将这些折叠的数据传递给一个模型，以使用更少的不平衡数据来训练该模型。这是一种通用方法，除了结构化数据之外，它甚至可以应用于图像等非结构化数据。我个人在一次 Kaggle 比赛中使用了同样的方法进行图像分类，并获得了更高的准确率。

*尝试一下，并告诉我们您对该方法的宝贵想法……*