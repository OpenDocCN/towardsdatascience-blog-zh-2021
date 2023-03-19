# 使用 Python 和机器学习构建图像分类器，从相册中过滤掉未使用的图像

> 原文：<https://towardsdatascience.com/building-an-image-classifier-to-filter-out-unused-images-from-your-photo-album-with-python-and-6bc574ae57de?source=collection_archive---------22----------------------->

![](img/9baf24771bf13ea7656c729c2943b061.png)

照片由 [Soragrit Wongsa](https://unsplash.com/@invictar1997?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

## 使用 Keras 将我的相册图像分类为“保留”或“不保留”

浏览文件夹中的一堆图片，试图找出哪些应该保留，可能会很麻烦。为了解决这个问题，我决定通过构建一个图像分类器来自动化这个过程，该分类器根据我是否想要保留照片来对照片进行分类。

***在本文中，我们将使用 Keras 实现一个简单的二进制分类器，将照片分类为“保留”或“不保留”，以自动过滤掉您的个人相册。***

如果你愿意，你可以在这里查看我的 Youtube 视频:

# 台阶

做到这一点的步骤将是:

*   **设置和安装**
*   **创建数据集**
*   **创建训练/测试文件夹**
*   **训练模型**
*   **运行 app**

如果你想跳过这篇文章直接看代码，可以在这里找到[。现在，让我们来看一下这些步骤。](https://github.com/EnkrateiaLucca/photo_album_filter_ml#readme)

## **设置和安装**

首先，我们将使用 conda 创建一个环境，并安装必要的包(`keras`、`matplotlib`和`streamlit`):

```
conda create -n photo_album_sorter_mlpip install -r requirements.txt
```

## **创建数据集**

我们将采取的第一步是按照创建时间对图像进行分类，并将它们移动到一个名为`files_with_dates,`的文件夹中，以保持图像相对有序:

在这个脚本中，我们简单地遍历用户提供的文件夹，将图像移动到`files_with_dates`文件夹，同时在文件名中添加创建时间。

要运行此命令，请在终端中键入:

```
python sort_creation_time.py — path ./path/to/your/images/folder
```

现在，我们可以为数据集创建文件夹，我们将标记图像来训练分类器。

这里，我们只是为数据集创建数据文件夹，并使用 matplotlib 建立一个简单的循环，将图像分类为“保留”或“不保留”。要运行此程序，请执行以下操作:

```
python create_dataset.py
```

现在，我们将文件拆分到 train 和 test 文件夹中。

在这里，我们只是将图像分割为 80%用于训练，20%用于测试。要运行 do:

```
python create_train_test.py
```

## **训练模型**

我们将训练两种类型的分类器，从一个经典 CNN 的简单 Keras 实现开始，它是从 Fran ois Chollet 的博客文章[中借来的。](https://blog.keras.io/building-powerful-image-classification-models-using-very-little-data.html)

要运行:

```
python train.py — epochs 10 — batch_size 16
```

注意你的图片的大小，在这个实现中，我们根据博客文章中关于猫和狗的例子将所有图片的大小调整为 150x150。

尽管这个模型的性能不是很好(在我的示例数据集上大约是 58%)，我还是把它作为问题的基线。

我们还使用取自 [Keras 文档](https://keras.io/api/applications/)的模板代码来训练预训练的 inception v3 模型。

在这里，我们只是按照一个基本的设置来微调一个 inception v3 模型，并添加一个小的 CLI 工具来使它易于运行:

```
python train_pretrained.py — epochs 10 — batch_size 16
```

这个模型产生了一些非常令人印象深刻的结果，没有过度拟合，在测试集上达到了 93%的性能！

这是一个好消息，但由于每个人都将使用自己的数据集，因此应该会有不同的表现，这取决于一系列因素，如图像的性质或您想从相册中排除的照片类型。

## **运行 app**

现在，在训练之后，为了查看分类器的运行情况，我们将编写一个简单的 streamlit 应用程序来包装我们的模型:

要运行，请键入:

```
streamlit run app.py
```

您可以使用 streamlit 的文件上传程序加载图像，分类将写在图像下方。

# 关于使用 ML 过滤个人照片的最终想法

鉴于机器学习模型的插值性质，我们总是冒着模型将图像错误分类的风险，这可能最终导致我们丢失我们关心的重要照片，所以我的想法是将这作为一种清除杂乱图像的第一种方法，但在完全删除图像之前，总是先进行粗略的查看。

[本文的源代码](https://github.com/EnkrateiaLucca/photo_album_filter_ml)

如果你喜欢这篇文章，[加入媒体](https://lucas-soares.medium.com/membership)，[关注](https://lucas-soares.medium.com/)，[订阅我的简讯](https://lucas-soares.medium.com/subscribe)。还有，订阅我的 [youtube 频道](https://www.youtube.com/channel/UCu8WF59Scx9f3H1N_FgZUwQ)在 [Tiktok](https://www.tiktok.com/@enkrateialucca?lang=en) 、[推特](https://twitter.com/LucasEnkrateia)、 [LinkedIn](https://www.linkedin.com/in/lucas-soares-969044167/) 、 [Instagram](https://www.instagram.com/theaugmentedself/) 上和我联系！谢谢，下次再见！:)

如果你对机器学习设备感兴趣，这里有一个来自 MSI 的 3070 GPU 的附属链接:

[](https://www.amazon.com/gp/product/B097MYTZMW/ref=as_li_tl?ie=UTF8&camp=1789&creative=9325&creativeASIN=B097MYTZMW&linkCode=as2&tag=lucassoare079-20&linkId=1fd3f206c6077e5372d08c4518764124) [## 微星游戏 GeForce RTX 3070 LHR 8GB GDRR6 256 位 HDMI/DP Nvlink Torx 风扇 4 RGB 安培架构…

### 微星标志性游戏系列的最新版本再次带来了高性能、低噪音效率和美学…

www.amazon.com](https://www.amazon.com/gp/product/B097MYTZMW/ref=as_li_tl?ie=UTF8&camp=1789&creative=9325&creativeASIN=B097MYTZMW&linkCode=as2&tag=lucassoare079-20&linkId=1fd3f206c6077e5372d08c4518764124) 

> 这是一个附属链接，如果你购买的产品，我得到一小笔佣金，干杯！:)

# 参考

*   [https://blog . keras . io/building-powerful-image-class ification-models-using-very-little-data . html](https://blog.keras.io/building-powerful-image-classification-models-using-very-little-data.html)
*   [https://keras.io/api/applications/](https://keras.io/api/applications/)