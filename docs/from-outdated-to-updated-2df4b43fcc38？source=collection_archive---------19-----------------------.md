# 从过时到更新

> 原文：<https://towardsdatascience.com/from-outdated-to-updated-2df4b43fcc38?source=collection_archive---------19----------------------->

## 如何使用 OCR、Python 和 SQLite 创建可搜索的 pdf 文档

![](img/c562ae655bf4b530e9bfb8c962a5bb47.png)

作者图片，基于来自 [Pixabay](https://pixabay.com/?utm_source=link-attribution&utm_medium=referral&utm_campaign=image&utm_content=1285165) 的 Pexels 图片(卡带)和来自 [Pixabay](https://pixabay.com/?utm_source=link-attribution&utm_medium=referral&utm_campaign=image&utm_content=616012) 的 [Firmbee](https://pixabay.com/users/firmbee-663163/?utm_source=link-attribution&utm_medium=referral&utm_campaign=image&utm_content=616012) 图片(智能手机)

您是否发现自己希望能够处理 pdf 文件中的文本数据，然后使这些信息可以跨文件搜索，而不仅仅是在文档中搜索？常见问题吧？对个人来说可能不太常见，但一个常见的例子可能是创建一个可搜索的报告存档，这些报告已被保存为 pdf 文档，以便您可以更容易地找到符合特定兴趣的报告。或者，您可能希望让遗留系统中的电子邮件可以被搜索到。

在这篇文章中，我们将探索一种计算机视觉的应用，从 pdf 中提取文本，然后将文本结果输入到一个可搜索的数据库中，以便最终用户应用程序可以快速访问这些信息。像谷歌一样思考，但对于在线不可用的文档。这样做的好处是，您可以非常快速地搜索大量文档(例如，对住院应用程序用户有好处)，并支持对文本进行更复杂的搜索功能(例如，对挑剔的应用程序用户有好处)。

为了解决这个问题，我们必须采取一些步骤，所以请耐心听我解释每一部分。我试图将这些部分分解成独立的部分，解释如何解决这个更大的问题。我们将使用 Python 和 ImageMagick 预处理 pdf 或图像以进行文本提取，使用 Tesseract 执行从图像中提取文本的计算机视觉部分，使用 sqlite 作为我们的数据库解决方案来创建提取文本的可搜索存储库。

![](img/50d2fff59fc347d69a8a6bdceb16f7bc.png)

图片来自 [Pixabay](https://pixabay.com/?utm_source=link-attribution&utm_medium=referral&utm_campaign=image&utm_content=4417511)

# **在我们开始之前，先给我们的环境命名:**

Windows 10 操作系统

宇宙魔方 4.0

ImageMagick 6.9.10-Q8

Sqlite

Python 3.6 库

立方体 0.2.5

PIL

魔杖 0.4.4

sqlite3 2 . 6 . 0(Python 3 自带。x 作为标准库)

# 设置

Tesseract 和 ImageMagick 都是独立的软件工具，需要安装在您的环境中，以便启用 pdf-to-image 处理、预 OCR 处理和 OCR(文本提取)。有许多优秀的教程展示了如何做到这一点，我在下面列出了其中的一些:

要在 Windows 10 上安装 Tesseract OCR，请观看此视频。

这里的重点是 Windows 环境而不是 Linux，但是我们下面生成的大部分代码都是一样的。像 Tesseract 和 ImageMagick 这样的开源工具往往更容易加载到 Linux 环境中，但是因为我不得不同时使用这两种工具，所以我想在 Windows 环境中执行这个操作，以证明这是可能的。关于在 Windows 环境中安装的一些注意事项:

# **ImageMagick**

您将需要使用 ImageMagick 6.9.10-Q8，而不是其最新版本。这是因为就我们将用来利用这款软件的 Python 函数而言，第 6 版在本文发布时是最稳定的。你可以在这里找到合适的 dll 文件。

在那里你可以找到 32 位和 64 位的可执行文件。因为我在 64 位 Windows 10 机器上运行，所以我下载了以下文件:

ImageMagick-6.9.10–11-Q8-x64-dll.exe

下载完成后，您可以双击并按照安装说明进行操作。

ImageMagick 安装到您的计算机上后，您将需要添加一个名为 MAGICK_HOME 的新环境变量，该变量的位置路径指向您的实例在您的计算机上的安装位置。例如，我的安装在 C 驱动器的程序文件中，所以我的路径变量如下所示:

c:\ Program Files \ ImageMagick-6 . 9 . 10-Q8

![](img/c7798e654d9586c7261c4eebe3721f59.png)

图片由作者提供，基于来自 [Pixabay](https://pixabay.com/?utm_source=link-attribution&utm_medium=referral&utm_campaign=image&utm_content=4932432) 的 [Gerd Altmann](https://pixabay.com/users/geralt-9301/?utm_source=link-attribution&utm_medium=referral&utm_campaign=image&utm_content=4932432) 的图片(左)和来自 [Pixabay](https://pixabay.com/?utm_source=link-attribution&utm_medium=referral&utm_campaign=image&utm_content=158648) 的 [OpenClipart-Vectors](https://pixabay.com/users/openclipart-vectors-30363/?utm_source=link-attribution&utm_medium=referral&utm_campaign=image&utm_content=158648) 的图片(右)

# **宇宙魔方**

要获得适用于您的特定 Windows 环境的安装程序，请访问[此处](https://github.com/UB-Mannheim/tesseract/wiki)并下载适当的可执行文件

一旦下载并安装到您的机器上，您将需要将 Tesseract 添加到环境变量中的路径变量中。当您访问您的环境变量时，打开您的 PATH 变量并将 Tesseract 的位置添加到列表中。我的道路看起来像这样:

c:\ Program Files(x86)\ tessera CT-OCR

# **Sqlite**

Sqlite 是一个非常轻量级的数据库软件，具有文本索引功能。因为它是世界上最常用的数据库软件，而且它附带了 Python，所以我们将在这里使用它。[这里的](http://www.sqlitetutorial.net/download-install-sqlite/)是一个精彩的教程，展示了如何下载 Windows 的 sqlite 工具

现在我们已经安装了所有的非 python 软件，我们准备开始了！

![](img/37bf6a9a226639e7f35b59471b9e64c2.png)

图片由[Williams 创作](https://pixabay.com/users/williamscreativity-17210051/?utm_source=link-attribution&utm_medium=referral&utm_campaign=image&utm_content=5502836)来自 [Pixabay](https://pixabay.com/?utm_source=link-attribution&utm_medium=referral&utm_campaign=image&utm_content=5502836)

# **开始编码吧！**

第一步是确保您已经安装了所有必要的 Python 库并将其放入内存。如果你正在使用像 IPython 这样的交互式 python 控制台，你可以像这样在 pip 之前使用“shebang ”!：

```
!pip install pytesseract==0.2.5
!pip install pillow==4.2.1
!pip install wand==0.4.4
```

# **第一步:将 PDF 转换为图像**

安装后，我们将开始从 pdf 到图像的转换，因为宇宙魔方不能消费 pdf 的。为了快速创建一堆 pdf 文件，我为 Chrome 使用了一个名为“ [Save-emails-to-pdf](https://chrome.google.com/webstore/detail/save-emails-to-pdf/dngbhajancmfmdnmhhdknhooljkddgnk?hl=en) ”的扩展。它速度很快，只需勾选复选框中的电子邮件并点击下载按钮，就可以将 Gmail 中的大量电子邮件保存为 pdf 文件:

![](img/8e492add764583dc195003bd4a0cc1fb.png)

作者图片

我们将使用这些 pdf 文件转换成图像，然后执行 OCR。如果您在想“嘿，为什么不直接使用 Python 中的 pdf 库来提取文本呢”，您将是正确的，因为像这样创建 pdf 文件确实可以直接从 pdf 代码中提取文本。问题是许多 pdf 文件没有嵌入文本，而是表示文本的图像。在这些情况下，提取文本的唯一方法是从图像文件中执行 OCR。但是我跑题了，继续…

```
from wand.image import Image
import wand.image
import wand.api
from os import listdirpath = 'C:/betacosine/OCR/'
pdf_list = [x for x in listdir(path) if x.endswith('.pdf')]
```

对于这个例子，我将示例 pdf 电子邮件放在上面代码中描述的路径中。然后，我们使用 listdir 方法获取指定目录中所有 pdf 文件的列表。

```
import ctypes
MagickEvaluateImage = wand.api.library.MagickEvaluateImage
MagickEvaluateImage.argtypes = [ctypes.c_void_p, ctypes.c_int, ctypes.c_double]def evaluate(self, operation, argument):
      MagickEvaluateImage(
            self.wand, 
            wand.image.EVALUATE_OPS.index(operation),
            self.quantum_range * float(argument))
```

在我们将 pdf 文件处理成图像之前，我们需要设置我们的 ImageMagick 方法和函数，这些方法和函数将用于将 pdf 文件转换成用于 OCR 的图像。我发现，当我们将 pdf 转换为灰度并在通过 OCR 引擎之前使用某种阈值对图像进行预处理时，Tesseract 在数字文本提取方面表现最佳。

阈值处理是一种简单有效的将图像中的前景从背景中分离出来的方法。为了使用 ImageMagick 完成这项工作，我们需要指定一些额外的信息，因此我们开发了一个函数来为我们执行阈值处理。

还有一个叫 OpenCV 的软件工具，它有一个更容易使用的 Python 接口，用于阈值处理和其他图像预处理，但为了让事情变得简单一点，我只关注 ImageMagick。参见[这篇](https://www.pyimagesearch.com/2018/09/17/opencv-ocr-and-text-recognition-with-tesseract/)关于使用 OpenCV 的 Tesseract 的精彩教程。

所以，上面的代码并不是最漂亮的，我当然明白了，尽管我试图通过把它放在 GitHub Gist 中使它更易读；)这是我作为数据科学家而不是程序员展示我的卡片的地方，但是上面的代码确实工作并且是有效的。因为它很复杂，所以让我向您介绍一下这里发生的事情。在第一行中，我们创建了一个名为 text_list 的开放列表容器，这是我们放置 OCR 文本结果的地方。在“for 循环”的开始，我们从遍历目录中的每个 pdf 文件开始。因为大多数 pdf 文件都是多页的，我们希望对每一页进行 OCR，所以我们需要在每一页上迭代我们的任务。因此，我们使用第一个“with”语句来获取每个 pdf 文件的总页数。

第二个“for 循环”遍历每个页码，并在每页上执行第二个“with”语句中包含的函数。在第二个“with”语句中，我们首先将图像转换为灰度，然后执行阈值处理。在以“img_buffer”开始的下一行中，我们使用 ImageMagick 中的“make_blob”方法时得到的二进制文件创建了一个 Numpy 数组。然后我们将它转换成一个 bytes 对象，这样我们就可以使用 PIL 库打开它。所有这些都是为了让我们不需要花费宝贵的计算资源将图像写入光盘，然后将它读回内存以执行 OCR。这样我们就可以直接将对象传递给 Tesseract 进行 OCR。

最后，我们将得到的文本追加到 text_list2 中，然后再追加到 text_list 中。我们剩下的是一个列表列表:

![](img/05d4a4624b4b0f42418318495c589c06.png)

作者图片

你会注意到我在这里只处理 6 封邮件。

```
flat_list = ['\n'.join(map(str, x)) for x in text_list]
```

在上面的下一行代码中，我们通过将多个页面合并到一个列表而不是一个子列表中，将结果列表简化为一个列表。

此时，您可以添加一些文本处理步骤，进一步提高已提取文本的可读性和准确性。

# **第二步:创建一个可搜索的数据库**

现在我们已经有了文本数据，我们希望将它输入到一个数据库中，该数据库对我们希望能够搜索的文本字段进行索引。

```
import sqlite3sqlite_db = 'email_db.db'zip_list = list(zip(pdf_list,flat_list))conn = sqlite3.connect(sqlite_db)c = conn.cursor()
```

在接下来的两行代码中，我们将导入 sqlite3 库，并为我们的数据库提供一个名称(email_db，original，我知道)。然后，我们将每个电子邮件的 pdf 文件名列表加入到文本结果列表中，这样可以快速插入数据库。

在最后两行代码中，我们创建了一个到数据库的连接。如果数据库不存在，这将创建它。然后，我们激活光标，以便能够与数据库进行交互。

```
c.execute('CREATE VIRTUAL TABLE email_table USING fts4(email_title TEXT, email_text TEXT)')c.executemany('INSERT INTO email_table(email_title,email_text) VALUES(?,?)', zip_list)conn.commit()
```

接下来，我们通过使用 FTS4 扩展创建一个表来使用 Sqlite 的文本索引功能。在 Sqlite 中，FTS 3–6 提供了文本索引功能，显著减少了从查询中获取结果所需的时间，并增加了额外的文本搜索功能。点击了解更多[。](https://www.sqlite.org/fts3.html)

最后，我们将 zip_list 中的数据插入到新表中，并提交它。

搞定了。现在，您已经创建了一个可搜索的文本数据数据库，即使是最复杂的文本搜索，它也能在数毫秒内响应数百万行。我已经对超过 1200 万行数据这样做了，并且在 0.1 到 0.2 秒内得到搜索结果。

此外，您现在可以利用 SQLite 的 FTS 扩展中提供的更复杂的文本查询功能。例如，您现在可以搜索一定数量的其他单词并返回结果。您甚至可以返回包含额外字符的结果，这些字符会突出显示您的搜索词在文本中出现的位置。我在下面包含了一些示例代码。

```
import pandas as pddf = pd.read_sql('''select * from email_table where email_text MATCH 'trump NEAR/5 tweet*';''',conn)conn.close()
```

在上面的代码中，我使用 pandas 拉出一个结果数据框，在其中我搜索任何说 trump 和 tweet 的行，它们之间的距离不超过 5 个单词。很酷吧！？！

一如既往，我们期待着任何评论，想法，或反馈，这可能已经启发了你。

比如参与学习更多关于数据科学的知识？[加入我](https://www.facebook.com/groups/thinkdatascience)。