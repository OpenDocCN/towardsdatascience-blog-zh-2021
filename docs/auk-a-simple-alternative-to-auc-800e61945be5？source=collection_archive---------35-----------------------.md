# AUK——AUC 的简单替代品

> 原文：<https://towardsdatascience.com/auk-a-simple-alternative-to-auc-800e61945be5?source=collection_archive---------35----------------------->

## *对于不平衡数据，比 AUC 更好的性能指标*

![](img/2e3d7103822aa61e281371567a18fe2c.png)

由 [Javier Quesada](https://unsplash.com/@quesada179?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

**二进制分类和评价指标**

分类问题在数据科学中普遍存在。通过分类，模型被训练来标注来自固定标签集的输入数据。当这个固定集合的长度为二时，那么这个问题就叫做*二元分类*。

通常，经过训练的二元分类模型为每个输入返回一个实数值 *r* 。如果实数值高于设定的阈值 *t* ，则为输入分配一个正标签，否则，将为输入分配一个负标签。

为二元分类问题计算的评估指标通常基于[混淆矩阵](/understanding-confusion-matrix-a9ad42dcfd62)。

为了评估分类模型的性能或对不同的模型进行排序，可以选择选取一个阈值 *t* 并基于它计算*精度、召回率、F(1)分数*和*精度*。此外，可以通过关注所有阈值(测试集中唯一的 *r* 分数的数量)来评估模型的性能，而不是选择一个阈值。基于后一种方法的一个常用图表是 [ROC 曲线](/understanding-auc-roc-curve-68b2303cc9c5)，它描绘了真实正比率(TP / TP + FN)与真实负比率(TN / TN + FP)的关系。然后，该曲线下的面积(AUC，0 到 1 之间的值)用于评估模型的质量，并将其与其他模型进行比较。

**联合自卫军的缺点**

AUC 是用于对模型性能进行排名的最常用的标量之一。然而，AUC 的缺点鲜为人知；Hand (2009)已经表明 AUC 对不同的分类器使用不同的分类成本分布(在这种情况下；对于不同的阈值 *t* )，并且它不考虑数据中的类偏度。然而，误分类损失应该取决于属于每个类别的对象的相对比例；AUC 不考虑这些前科。这相当于说，使用一个分类器，错误分类类别 1 是错误分类类别 0 的*倍。但是，使用另一个分类器，误分类类 1 是 *P* 倍严重，其中*P*≦*P*。这是无意义的，因为单个点的不同种类的错误分类的相对严重性是问题的属性，而不是碰巧被选择的分类器。*

**海雀**

为了克服这些缺点，Kaymak、Ben-David 和 Potharst (2012)提出了一个相关但不同的指标；Kappa 曲线下的面积(AUK)，这是基于被称为 [Cohen 的 Kappa](/cohens-kappa-9786ceceab58) 的公认指标。它测量绘制 Kappa 对假阳性率的图形下的面积。就像 AUC 可以被视为整体性能的指标一样，海雀也可以。然而，Kappa 由于偶然性而导致正确的分类。因此，它固有地解释了类偏度。

在本文中，作者展示了海雀的一些特征和优点:

*   Kappa 是真阳性率和假阳性率之差的非线性变换。
*   凸 Kappa 曲线具有唯一的最大值，可用于选择最佳模型。

此外，如果数据集是平衡的:

*   科恩的 Kappa 提供了与 ROC 曲线完全相同的信息。
*   AUK = AUC — 0.5 (AUK 和 AUC 仅相差一个常数)。
*   AUK = 0.5 基尼(当数据集中没有偏斜时，AUK 等于基尼系数的一半)
*   当 ROC 曲线的梯度等于 1 时，Kappa 值最大。因此，通过 Kappa 找到最优模型没有任何附加价值。

**结论**

也就是说，如果数据集是平衡的。然而，AUC 和 AUK 可能对不平衡数据集有不同的模型排名(请阅读论文中的示例)，这在投入生产时会产生巨大的影响。由于 AUK 解释了类偏度，而 AUC 没有，AUK 似乎是更好的选择，应该成为任何数据科学家工具箱的一部分。

**代码**

假设您有:

*   *概率:*你的分类模型的输出；一个长度为 k 的实数列表。
*   *标签:*分类模型的实际标签；一个由 0 和 1 组成的 *k* 长度列表。

然后，可以调用下面的类来计算 AUK 和/或得到 Kappa 曲线。

```
class AUK:
    def __init__(self, probabilities, labels, integral='trapezoid'):
        self.probabilities = probabilities
        self.labels = labels
        self.integral = integral
        if integral not in ['trapezoid','max','min']:
            raise ValueError('"'+str(integral)+'"'+ ' is not a valid integral value. Choose between "trapezoid", "min" or "max"')
        self.probabilities_set = sorted(list(set(probabilities)))

    #make predictions based on the threshold value and self.probabilities
    def _make_predictions(self, threshold):
        predictions = []
        for prob in self.probabilities:
            if prob >= threshold:
                predictions.append(1)
            else: 
                predictions.append(0)
        return predictions

    #make list with kappa scores for each threshold
    def kappa_curve(self):
        kappa_list = []

        for thres in self.probabilities_set:
            preds = self._make_predictions(thres)
            tp, tn, fp, fn = self.confusion_matrix(preds)
            k = self.calculate_kappa(tp, tn, fp, fn)
            kappa_list.append(k)
        return self._add_zero_to_curve(kappa_list)

    #make list with fpr scores for each threshold
    def fpr_curve(self):
        fpr_list = []

        for thres in self.probabilities_set:
            preds = self._make_predictions(thres)
            tp, tn, fp, fn = self.confusion_matrix(preds)
            fpr = self.calculate_fpr(fp, tn)
            fpr_list.append(fpr)
        return self._add_zero_to_curve(fpr_list)

    #calculate confusion matrix
    def confusion_matrix(self, predictions):
        tp = 0
        tn = 0
        fp = 0
        fn = 0
        for i, pred in enumerate(predictions):
            if pred == self.labels[i]:
                if pred == 1:
                    tp += 1
                else: 
                    tn += 1
            elif pred == 1:
                fp += 1
            else: fn += 1
            tot = tp + tn + fp + fn
        return tp/tot, tn/tot, fp/tot, fn/tot

    #Calculate AUK
    def calculate_auk(self):        
        auk=0
        fpr_list = self.fpr_curve()

        for i, prob in enumerate(self.probabilities_set[:-1]):
            x_dist = abs(fpr_list[i+1] - fpr_list[i])

            preds = self._make_predictions(prob) 
            tp, tn, fp, fn = self.confusion_matrix(preds)
            kapp1 = self.calculate_kappa(tp, tn, fp, fn)

            preds = self._make_predictions(self.probabilities_set[i+1]) 
            tp, tn, fp, fn = self.confusion_matrix(preds)
            kapp2 = self.calculate_kappa(tp, tn, fp, fn)

            y_dist = abs(kapp2-kapp1)
            bottom = min(kapp1, kapp2)*x_dist
            auk += bottom
            if self.integral == 'trapezoid':
                top = (y_dist * x_dist)/2
                auk += top
            elif self.integral == 'max':
                top = (y_dist * x_dist)
                auk += top
            else:
                continue
        return auk

    #Calculate the false-positive rate
    def calculate_fpr(self, fp, tn):
        return fp/(fp+tn)

    #Calculate kappa score
    def calculate_kappa(self, tp, tn, fp, fn):
        acc = tp + tn
        p = tp + fn
        p_hat = tp + fp
        n = fp + tn
        n_hat = fn + tn
        p_c = p * p_hat + n * n_hat
        return (acc - p_c) / (1 - p_c)

    #Add zero to appropriate position in list
    def _add_zero_to_curve(self, curve):
        min_index = curve.index(min(curve)) 
        if min_index> 0:
            curve.append(0)
        else: curve.insert(0,0)
        return curve #Add zero to appropriate position in list
    def _add_zero_to_curve(self, curve):
        min_index = curve.index(min(curve)) 
        if min_index> 0:
            curve.append(0)
        else: curve.insert(0,0)
        return curve
```

要计算 AUK，请使用以下步骤:

```
auk_class = AUK(probabilities, labels)auk_score = auk_class.calculate_auk()kappa_curve = auk_class.kappa_curve()
```

最后，我强烈建议只在计算积分时使用*梯形*，因为这也是 [sklearn 计算 AUC](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.auc.html) 积分的方式。

# 参考

Hand，D. J. (2009)。测量分类器性能:ROC 曲线下面积的一致替代方法。*机器学习*， *77* (1)，103–123。

凯马克，u .，本大卫，a .，，波塔斯特，R. (2012)。海雀:AUC 的简单替代。*人工智能的工程应用*， *25* (5)，1082–1089。