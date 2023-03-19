# 融合效率 Net & YoloV5 —高级对象检测 2 阶段管道教程

> 原文：<https://towardsdatascience.com/fusing-efficientnet-yolov5-advanced-object-detection-2-stage-pipeline-tutorial-da3a77b118d1?source=collection_archive---------14----------------------->

## 通过将 YoloV5 与 EfficientNet 集成，将对象检测性能提高约 20%

![](img/1cfeeb40b3dd8f3e662d094e141544e9.png)

[格蕾塔·法内迪](https://unsplash.com/@gretafarnedi?utm_source=medium&utm_medium=referral)在[号航天飞机](https://unsplash.com?utm_source=medium&utm_medium=referral)上的照片

在本文中，我将解释一个我称之为“2 类过滤器”的概念。这是一种用于对象检测和分类模型的集成技术，在过去几周我一直在进行的一场 Kaggle 竞赛中大量使用。这项技术已经被几乎每个参加比赛的人所使用，它似乎能提高 5-25%左右的成绩，这是相当有用的。

## 物体检测 YoloV5

我们首先在数据集上训练一个 YoloV5 模型，同时使用加权盒融合(WBF)进行后处理/预处理，如果您想了解更多信息，我建议查看以下两篇文章:

[](/advanced-yolov5-tutorial-enhancing-yolov5-with-weighted-boxes-fusion-3bead5b71688) [## 高级 YoloV5 教程-使用加权盒融合增强 YoloV5

### 关于使用 YoloV5 和与 WBF 一起提高性能的深入教程

towardsdatascience.com](/advanced-yolov5-tutorial-enhancing-yolov5-with-weighted-boxes-fusion-3bead5b71688) [](/wbf-optimizing-object-detection-fusing-filtering-predicted-boxes-7dc5c02ca6d3) [## WBF:优化对象检测—融合和过滤预测框

### 加权盒融合已经成为优化目标检测模型的新 SOTA 方法

towardsdatascience.com](/wbf-optimizing-object-detection-fusing-filtering-predicted-boxes-7dc5c02ca6d3) 

我不想再详述和 WBF 一起训练约洛娃的细节。但是，实际上您需要做的就是使用 WBF 消除重复的方框，然后对数据进行预处理，在上面运行 YoloV5。YoloV5 需要一个特定的层次结构，以便数据集能够开始培训和评估。

## 分类-效率网

接下来要做的是在数据集上训练一个分类网络。但是，有趣的是，尽管在 14 个不同的类别(13 种不同类型的疾病和 1 种无疾病类别)上训练了对象检测模型，但是我们将仅在 2 个类别(疾病和无疾病)上训练分类网络。您可以将其视为简化我们的数据科学问题的建模技巧，因为对一个网络分类 2 类比分类 14 类要容易得多，当我们融合这 2 个网络时，我们并不真正需要每种疾病的细节，我们只需要这 2 类中的一种。当然，对于您的问题来说，这可能有点不同，所以您可能需要尝试不同的设置，但是希望您可以从本文中获得一个或两个想法。

目前最先进的分类网络之一是高效网。对于这个数据集，我们将使用经过 Keras (TensorFlow)培训的 B6 效率网，以及以下增强功能:

```
(
    rescale=1.0 / 255,
    rotation_range=40,
    width_shift_range=0.2,
    height_shift_range=0.2,
    shear_range=0.2,
    zoom_range=0.2,
    horizontal_flip=True,
    fill_mode="nearest",
)
```

如果你想看完整个教程，我建议你看看这个:

[](/an-in-depth-efficientnet-tutorial-using-tensorflow-how-to-use-efficientnet-on-a-custom-dataset-1cab0997f65c) [## 使用 TensorFlow 的深入 EfficientNet 教程-如何在自定义数据集上使用 EfficientNet。

### 使用 Tensorflow 在具有挑战性的 Kaggle 数据集上训练效率网

towardsdatascience.com](/an-in-depth-efficientnet-tutorial-using-tensorflow-how-to-use-efficientnet-on-a-custom-dataset-1cab0997f65c) 

## 组装

这就是 2 类滤波器提高性能的地方，也是本文真正要讨论的内容。我不想谈论太多关于培训 YoloV5 和 EfficientNet 的内容，因为有很多关于它们的资源。

我想强调的主要思想是，虽然 Yolo 的分类预测非常好，但如果你可以将它们与另一个更强大的网络的分类混合，你可以获得相当不错的性能提升。让我们看看这是如何实现的。

这里使用的想法是设置一个高阈值和一个低阈值。然后我们会检查每个分类预测。如果概率小于低阈值，我们将任何预测设置为“无疾病”。回想一下，我们最初的问题是对 14 种疾病中的一种或“无疾病”进行分类。这个低阈值可以是 0 到 1 之间的任何值，但是很可能是 0 到 0.1 之间的某个值。此外，如果分类预测在低阈值和高阈值之间，我们**添加**一个“无疾病”预测，该预测具有 **EfficientNet 的**置信度(不是 Yolo 的),因为在这种情况下有更高的几率患任何疾病。最后，如果分类预测高于高阈值，我们什么也不做，因为这意味着网络高度可信。

这可以通过以下方式实现:

```
low_thr  = 0.08
high_thr = 0.95def filter_2cls(row, low_thr=low_thr, high_thr=high_thr):
    prob = row['target']
    if prob<low_thr:
        *## Less chance of having any disease*
        row['PredictionString'] = '14 1 0 0 1 1'
    elif low_thr<=prob<high_thr:
        *## More chance of having any disease*
        row['PredictionString']+=f' 14 **{**prob**}** 0 0 1 1'
    elif high_thr<=prob:
        *## Good chance of having any disease so believe in object detection model*
        row['PredictionString'] = row['PredictionString']
    else:
        raise **ValueError**('Prediction must be from [0-1]')
    return row
```

来源:[卡格尔](https://www.kaggle.com/awsaf49/vinbigdata-2-class-filter)

**最终想法**

我在比赛期间在各种不同的场景和模型上试验了这种 2 级过滤器，它似乎总是能够将性能提高多达 25%，这令人惊讶。我认为如果你想把它应用到你的自定义场景中，你需要考虑在什么情况下分类网络预测可以帮助你的对象检测模型。这并不完全是交换预测信心，而是以一种聪明的方式“融合”它们。