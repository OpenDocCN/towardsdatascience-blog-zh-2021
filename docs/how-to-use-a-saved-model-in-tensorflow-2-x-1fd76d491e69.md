# 如何在 Tensorflow 2.x 中使用已保存的模型

> 原文：<https://towardsdatascience.com/how-to-use-a-saved-model-in-tensorflow-2-x-1fd76d491e69?source=collection_archive---------16----------------------->

## 关于保存和重用 Tensorflow 训练模型的教程

![](img/4ba3cf92a1e9b5c0ed658931839f6b5b.png)

作者使用 wordart.com 生成的图像

在我之前的[文章](https://medium.com/analytics-vidhya/tensorflow-2-model-validation-regularization-and-callbacks-49c5ace1e8b)中，我写了关于使用 TensorFlow 2.x 的模型验证、正则化和回调。在机器学习管道中，创建一个经过训练的模型是不够的。一旦我们完成了训练、验证和测试，并保留了一部分数据，我们将如何处理训练好的模型呢？在实践中，我们希望导入这样一个经过训练的模型，以便它可以在一些实际应用中有用。例如，假设我在相机图像上训练了一个模型来识别行人。最终，我想使用训练好的模型，通过安装在自动驾驶汽车上的摄像头，对检测行人进行实时预测。此外，训练模型还需要将模型保存为检查点，特别是当您在非常大的数据集上训练模型或者训练时间大约为几个小时时。如果您的训练由于某些原因而中断，比如您的编程逻辑中的缺陷、您的笔记本电脑的电池没电、存在 I/O 错误等等，模型保存也是有用的。

<https://medium.com/analytics-vidhya/tensorflow-2-model-validation-regularization-and-callbacks-49c5ace1e8b>  

> 最终，我想使用训练好的模型，通过安装在自动驾驶汽车上的摄像头，对检测行人进行实时预测。

# 保存和加载模型

在保存模型时，我们可以做几件事情。我们希望在每次迭代(历元)中保存模型权重和训练参数，每隔一段时间保存一次，还是在训练完成后保存？我们可以使用内置回调，就像我们在我之前的[文章](https://medium.com/analytics-vidhya/tensorflow-2-model-validation-regularization-and-callbacks-49c5ace1e8b)中看到的那样，在训练过程中自动保存模型权重。或者，一旦训练完成，我们也可以保存模型权重和其他必要的信息。

保存的模型有两种主要格式:一种是原生 TensorFlow 格式，另一种是 HDF5 格式，因为我们通过 Keras API 使用 TensorFlow。

**在训练过程中保存模型的例子:**

```
from tensorflow.keras.models import Sequential
from tensorflow.keras.layers import Dense
from tensorflow.keras.losses import BinaryCrossentropy
from tensorflow.keras.callbacks import ModelCheckpointmodel = Sequential( [
Dense(128, activation='sigmoid', input_shape = (10, )),
Dense(1)])model.compile(optimizer = 'sgd', loss = BinaryCrossentropy(from_logits  = True))checkpoint = ModelCheckpoint('saved_modelname', save_weights_only=True)model.fit(X_train, y_train, epochs = 10, callbacks = [checkpoint])
```

你可以看到，我使用`ModelCheckpoint`类创建了一个对象`checkpoint`，它接受一个参数，该参数将被用作保存模型的文件名。由于使用了`save_weights_only=True`，所以只会节省权重，不会节省网络架构。最后，我们将`callback = [checkpoint]`传递给 fit 函数。

如果我们提供`‘saved_modelname.h5`而不是`‘saved_modelname’`，那么模型将以 HDF5 格式保存。

为了加载先前保存的权重，我们调用`load_weights`函数。

```
model = Sequential( [
Dense(128, activation='sigmoid', input_shape = (10, )),
Dense(1)])
model.load_weights('saved_modelname')
```

## 手动保存模型权重，无需回调

我们也可以通过在培训结束时调用`save_weights`来手动保存模型

```
model = Sequential( [
Dense(128, activation='sigmoid', input_shape = (10, )),
Dense(1)])model.compile(optimizer = 'sgd', loss = BinaryCrossentropy(from_logits  = True))model.fit(X_train, y_train, epochs = 10)
**model.save_weights("saved_modelname")**
```

您还可以分析保存模型的目录:

```
total 184K
-rw-r--r-- 1 ivory ivory   61 Jan 12 01:08 saved_modelname
-rw-r--r-- 1 ivory ivory 174K Jan 12 01:08 saved_modelname.data-00000-of-00001
-rw-r--r-- 1 ivory ivory 2.0K Jan 12 01:08 saved_modelname.index
```

在这里，您可以看到保存的实际模型是`saved_modelname.data-00000-of-00001`，文件的其余部分包含元数据。

# 保存整个模型

到目前为止，我们看到我们只保存了模型权重。然而，保存整个模型是非常容易的。在实例化`ModelCheckpoint` 类时只需传递`save_weights_only=False`。

```
checkpoint_dir = 'saved_model'
checkpoint = ModelCheckpoint(filepath=checkpoint_dir, 
                            frequency = "epoch",
                            save_weights_only = False,
                             verbose= True)model.fit(X_train, y_train, callbacks=[checkpoint])
```

在这种情况下，将创建一个包含以下内容的新目录:

```
total 128
drwxr-xr-x 2 ivory ivory   4096 Jan 12 01:14 assets
-rw-r--r-- 1 ivory ivory 122124 Jan 12 01:14 saved_model.pb
drwxr-xr-x 2 ivory ivory   4096 Jan 12 01:14 variables
```

在这种情况下，主模型保存在文件`saved_model.pb`中，其他文件是元数据。

最后，我们可以如下使用保存的模型:

```
from tensorflow.keras.models import load_model
model = load_model(checkpoint_dir)
```

如果我们想在训练程序完成后保存模型，我们可以如下调用`save`函数:

```
model.save("mysavedmodel")
```

如果使用`model.save(“mysavedmodel.h5”)`，那么模型将被保存为一个单独的文件`mysavedmodel.h5`。

保存的模型可用于使用全新的数据集进行预测。

```
model.predict(X_test)
```

我的 GitHub repo 在[https://GitHub . com/rahulbhadani/medium . com/blob/master/01 _ 12 _ 2021/Saving _ Model _ tf2 . ipynb](https://github.com/rahulbhadani/medium.com/blob/master/01_12_2021/Saving_Model_TF2.ipynb)中给出了一个更具描述性的例子。

## **确认**

这篇文章的动机是作者从 TensorFlow2 Coursera 课程[https://www . Coursera . org/learn/getting-started-with-tensor-flow 2/。读者可能会发现这个例子与 Coursera 课程中的例子有相似之处。教师允许使用 Coursera 课程中的代码示例(以其原始形式或经过一些修改)。](https://www.coursera.org/learn/getting-started-with-tensor-flow2/.)