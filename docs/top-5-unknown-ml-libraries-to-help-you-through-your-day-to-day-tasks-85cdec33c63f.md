# 帮助你完成日常任务的 5 大未知 ML 库

> 原文：<https://towardsdatascience.com/top-5-unknown-ml-libraries-to-help-you-through-your-day-to-day-tasks-85cdec33c63f?source=collection_archive---------49----------------------->

## 用这 5 个必备的库提高你的编码速度

![](img/5ed37fa4110fb7d1dc60cf1bf373ec6c.png)

[https://unsplash.com/photos/Nj8pk8c8uI4](https://unsplash.com/photos/Nj8pk8c8uI4)

我一直注意到，任何领域的顶级专家都能够比普通人更快地创造出令人惊叹的东西，这不是因为他们更聪明，而是因为他们能够更快地重复自己的想法。快速迭代的一个基本要素是拥有一些代码片段和库，它们有助于更容易地构建复杂的模型。在这篇文章中，我想分享我的必备机器学习库，你可能还没有听说过。

## [PyTorch 预测](https://pytorch-forecasting.readthedocs.io/)

今天我名单上的第一位客人是一个令人敬畏的 PyTorch 预测 Python 库。在这个工具的帮助下，我能够在创建时间序列预测模型时测试更多的方法。该库包含多个模型，如 NBeats 和 TemporalFusionTransformer，正如作者坚持认为的那样，它们优于亚马逊 DeepAR 算法。要使用这些模型进行训练和预测，您必须首先准备数据集，然后才能使用新模型进入拟合/预测阶段。

```
**import** pytorch_lightning **as** pl
**from** pytorch_lightning.callbacks **import** EarlyStopping**,** LearningRateMonitor

**from** pytorch_forecasting **import** TimeSeriesDataSet**,** TemporalFusionTransformer

*# load data*
data **=** **...**

*# define dataset*
max_encode_length **=** **36**
max_prediction_length **=** **6**
training_cutoff **=** "YYYY-MM-DD"  *# day for cutoff*

training **=** TimeSeriesDataSet**(**
    data**[lambda** x**:** x**.**date **<** training_cutoff**],**
    time_idx**=** **...,**
    target**=** **...,**
    *# weight="weight",*
    group_ids**=[** **...** **],**
    max_encode_length**=**max_encode_length**,**
    max_prediction_length**=**max_prediction_length**,**
    static_categoricals**=[** **...** **],**
    static_reals**=[** **...** **],**
    time_varying_known_categoricals**=[** **...** **],**
    time_varying_known_reals**=[** **...** **],**
    time_varying_unknown_categoricals**=[** **...** **],**
    time_varying_unknown_reals**=[** **...** **],**
**)**

*# create validation and training dataset*
validation **=** TimeSeriesDataSet**.**from_dataset**(**training**,** data**,** min_prediction_idx**=**training**.**index**.**time**.**max**()** **+** **1,** stop_randomization**=True)**
batch_size **=** **128**
train_dataloader **=** training**.**to_dataloader**(**train**=True,** batch_size**=**batch_size**,** num_workers**=2)**
val_dataloader **=** validation**.**to_dataloader**(**train**=False,** batch_size**=**batch_size**,** num_workers**=2)**

*# define trainer with early stopping*
early_stop_callback **=** EarlyStopping**(**monitor**=**"val_loss"**,** min_delta**=1e-4,** patience**=1,** verbose**=False,** mode**=**"min"**)**
lr_logger **=** LearningRateMonitor**()**
trainer **=** pl**.**Trainer**(**
    max_epochs**=100,**
    gpus**=0,**
    gradient_clip_val**=0.1,**
    limit_train_batches**=30,**
    callbacks**=[**lr_logger**,** early_stop_callback**],**
**)**

*# create the model*
tft **=** TemporalFusionTransformer**.**from_dataset**(**
    training**,**
    learning_rate**=0.03,**
    hidden_size**=32,**
    attention_head_size**=1,**
    dropout**=0.1,**
    hidden_continuous_size**=16,**
    output_size**=7,**
    loss**=**QuantileLoss**(),**
    log_interval**=2,**
    reduce_on_plateau_patience**=4**
**)**
print**(**f"Number of parameters in network: {tft**.**size**()/1e3**:.1f}k"**)**

*# find optimal learning rate (set limit_train_batches to 1.0 and log_interval = -1)*
res **=** trainer**.**tuner**.**lr_find**(**
    tft**,** train_dataloader**=**train_dataloader**,** val_dataloaders**=**val_dataloader**,** early_stop_threshold**=1000.0,** max_lr**=0.3,**
**)**

print**(**f"suggested learning rate: {res**.**suggestion**()**}"**)**
fig **=** res**.**plot**(**show**=True,** suggest**=True)**
fig**.**show**()**

*# fit the model*
trainer**.**fit**(**
    tft**,** train_dataloader**=**train_dataloader**,** val_dataloaders**=**val_dataloader**,**
**)**
```

此外，您可以在 PyTorch 预测的基础上轻松构建更复杂的东西。Pytorch 一如既往地出色，并且易于扩展。

## [TabNet](https://github.com/dreamquark-ai/tabnet)

继论文之后 [TabNet:专注的可解释表格学习](https://arxiv.org/pdf/1908.07442.pdf) TabNet 模型已经成为许多表格数据任务的超级明星。例如，在最近的 Kaggle challenge [行动机制(MoA)预测](https://www.kaggle.com/c/lish-moa)中，TabNet 胜过了树相关模型。TabNet 肯定不能更好地处理任何类型的表格数据任务，但我肯定会尝试一下，尤其是因为它有一个非常非常简单的 API。

```
from pytorch_tabnet.tab_model import TabNetClassifier, TabNetRegressor

clf = TabNetClassifier()  #TabNetRegressor()
clf.fit(
  X_train, Y_train,
  eval_set=[(X_valid, y_valid)]
)
preds = clf.predict(X_test)
```

如您所见，这与 PyTorch 默认 API 调用几乎没有任何区别。

## [Python 的媒体管道](https://google.github.io/mediapipe)

谷歌的 Mediapipe 库可以大大节省你的时间，当你创建计算机视觉模型，如人脸检测，姿势估计，头发分割，等等！Mediapipe API 真的很简单。下面是通过 Python 脚本在桌面上运行姿态估计模型的完整代码。

```
import cv2
import mediapipe as mp
mp_drawing = mp.solutions.drawing_utils
mp_pose = mp.solutions.pose

# For webcam input:
pose = mp_pose.Pose(
    min_detection_confidence=0.5, min_tracking_confidence=0.5)
cap = cv2.VideoCapture(0)
while cap.isOpened():
  success, image = cap.read()
  if not success:
    print("Ignoring empty camera frame.")
    continue

  image = cv2.cvtColor(cv2.flip(image, 1), cv2.COLOR_BGR2RGB)
  image.flags.writeable = False
  results = pose.process(image)
  image.flags.writeable = True
  image = cv2.cvtColor(image, cv2.COLOR_RGB2BGR)
  mp_drawing.draw_landmarks(
      image, results.pose_landmarks, mp_pose.POSE_CONNECTIONS)
  cv2.imshow('MediaPipe Pose', image)
  if cv2.waitKey(5) & 0xFF == 27:
    break
pose.close()
cap.release()
```

您不仅可以非常快速地创建和部署这些模型，而且它们比它们的类似物工作得更好。例如，iPhone 上的 MediaPipe hand landmark 模型比 iPhone 上可用但默认的相同模型运行得快得多。如果你想在 Python 上尝试这些模型，目前只有手界标、姿势界标、面部网格和整体模型可用。但是正如开发者所说，他们很快就会添加更多的 Python 模型。

## [急流](https://rapids.ai/)

你有没有花几个小时训练一个 KNN 模特，然后失去耐心，停止训练？嗯，我做到了...好的一面是，你不会在一个令人敬畏的急流图书馆中再次体验到这一点。它允许在 GPU 或 GPU 集群上运行许多标准的 ML 算法。不仅如此，它还允许在 GPU 上运行熊猫数据帧的计算，这有时会很方便。模型拟合的 API 也非常简单，只需几行代码，就大功告成了！

```
 model = NearestNeighbors(n_neighbors=KNN)
model.fit(X_train)
distances, indices = model.kneighbors(X_test)
```

你可以在这里看到完整的 KNN 试衣教程[。](https://www.kaggle.com/cdeotte/rapids-knn-30-seconds-0-938)

## [达斯克](https://dask.org/)

我今天名单上的最后一个但绝对不是最不重要的是达斯克。我过去常常纠结于 PySpark API，并没有真正设法完全学会它。Dask 有类似的功能，但它更像默认的 Numpy/Pandas API，事实上，它甚至有类似名称的方法，我觉得这非常有用。

```
df[['x', 'y']].rolling(window='24h').mean().head()
```

你能猜到是 Dask 执行了这个操作吗？你看，和熊猫一样！

## 最后的话

感谢您抽出时间阅读本材料！我希望它能拓宽你的视野，甚至对你未来的项目有所帮助。我正计划写更多像那样有帮助的帖子，如果你喜欢，你可以关注我的个人资料，不要错过它们。

如果你对我是谁和我的项目感兴趣，你可以访问我的个人网站了解更多:[artkulakov.com](https://artkulakov.com/)