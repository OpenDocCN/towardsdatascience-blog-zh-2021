# 如何使用 Tesseract OCR 引擎和 Python 从图像中提取文本

> 原文：<https://towardsdatascience.com/how-to-extract-text-from-images-using-tesseract-ocr-engine-and-python-22934125fdd5?source=collection_archive---------4----------------------->

## 从 100 多种语言中选择

![](img/a1c1c80dca7fcaa86b2547cf7c5577ee.png)

照片由[上的](https://unsplash.com/s/photos/laptop?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)[面](https://unsplash.com/@surface?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)拍下

W 帽子是[宇宙魔方](https://opensource.google/projects/tesseract)？这是一个开源的 OCR(光学字符识别)引擎，可以识别超过 [100](https://github.com/tesseract-ocr/tessdoc/blob/master/Data-Files-in-different-versions.md) 种支持 Unicode 的语言。此外，它可以被训练识别其他语言。OCR 引擎可以通过数字化文档而不是手动键入文档内容来节省时间。在这篇文章中，您将学习如何使用 Tesseract OCR 引擎和 Python 从图像中提取文本。

# 设置

对于设置，我将使用 [Tesseract OCR 引擎](https://github.com/tesseract-ocr/tesseract)、 [Python](https://www.python.org/downloads/) 和 macOS。首先，我们需要安装宇宙魔方 OCR 引擎。在终端中键入以下命令。

```
brew install tesseract
```

您可以使用以下命令来查看您的计算机上运行的是哪个版本的 Tesseract。

```
tesseract --version
```

使用以下命令列出 Tesseract OCR 引擎的可用语言。

```
tesseract --list-langs
```

在这种情况下，我有三种语言。

```
eng   #English
osd   #Special data file for orientation and script detection
snum  #Serial number identification
```

如果你想下载额外的语言，你可以从[这里](https://github.com/tesseract-ocr/tessdata_best)得到。我将[下载](https://github.com/tesseract-ocr/tessdata_best/raw/master/ben.traineddata)孟加拉语的列车数据。一旦下载到您的机器上，您需要将该文件移动到以下文件夹中。

```
/usr/local/Cellar/tesseract/4.1.1/share/tessdata/
```

现在从[这里](https://www.python.org/downloads/)安装 Python，如果你还没有的话。之后，我们将安装 [Python-tesseract](https://pypi.org/project/pytesseract/) ，这是 Tesseract OCR 引擎的包装器。还有，我们需要安装一个 Python 映像库[枕头](https://pypi.org/project/Pillow/)。在终端中键入以下命令。

```
pip install pytesseract
pip install Pillow
```

出于演示的目的，我创建了一个 GitHub 库，你可以从[这里](https://github.com/lifeparticle/tesseract-python)获得。在存储库中，我包含了两张英语和孟加拉语的测试图片。此外，我有一个名为`main.py`的 Python 脚本。让我们来分解一下`main.py`文件中的个人贫困人口。

`main.py`

这里我创建了一个方法`process_image`，它将图像名称和语言代码作为参数。在方法内部，我使用了一个 pytesseract 方法`image_to_string`，它以字符串形式从 Tesseract OCR 返回未修改的输出。此外，我还添加了两个助手方法。`print_data`方法打印字符串输出，`output_file`方法将字符串输出写入文本文件。下面，我得到了字符串输出。

**test_eng.png** 文件的输出。

```
_ The'quick brown fox' .
-jumps over the lazy:
dog.
```

**test_ben.png** 文件的输出。

```
পথের দেবতা প্রসন্ন হাসিয়া বলেন-মূর্খ বালক, পথ তো
আমার শেষ হয়নি তোমাদের গ্রামে, বাশের বনে, ঠ্যাঙাড়ে
'বীরু রায়ের বটতলায় কি ধলচিতের খেয়াঘাটের সীমানায়.
তোমাদের সোনাডাঙা মাঠ ছাড়িয়ে ইচ্ছামতী পার হয়ে
পদ্মফুলে ভরা মধূখালি বিলের পাশ কাটিয়া বেত্রবতীর
খেয়ায় পাড়ি দিয়ে, পথ আমার চলে গেল সামনে, সামনে,
'শুধুই সামনে...দেশ ছেড়ে দেশান্তরের দিকে, সূর্যোদয় ছেড়ে
সূর্যাস্তের দিকে, জানার গন্ডী এড়িয়ে অপরিচয়ের উদ্দেশে.
```

# 包裹

文档数字化使机器可读。例如，我们可以转换重要的扫描文档来执行搜索、翻译和文字处理。另外，我们可以用它来自动接收和识别车牌。OCR 的潜力是无穷的。

查看我关于 Python 的其他帖子。

</how-to-deploy-a-python-serverless-function-to-vercel-f43c8ca393a0>  </how-to-connect-to-a-postgresql-database-with-a-python-serverless-function-f5f3b244475> 