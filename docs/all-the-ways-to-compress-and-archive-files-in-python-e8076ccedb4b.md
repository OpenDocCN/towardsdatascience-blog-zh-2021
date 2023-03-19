# Python 中压缩和归档文件的所有方法

> 原文：<https://towardsdatascience.com/all-the-ways-to-compress-and-archive-files-in-python-e8076ccedb4b?source=collection_archive---------2----------------------->

## 用 Python 压缩、解压缩和管理你可能需要的所有格式的档案和文件

![](img/c598a233f99d748bc7882acccc1b4109.png)

托马斯·索贝克在 [Unsplash](https://unsplash.com/?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上拍摄的照片

Python 标准库为你能想到的几乎任何任务提供了很好的模块和工具，处理压缩文件的模块也不例外。无论是像`tar`和`zip`这样的基础，还是像`gzip`和`bz2`这样的特定工具或格式，甚至是像`lzma`这样更奇特的格式，Python 都有。有了所有这些选择，决定什么可能是手头任务的正确工具可能就不那么明显了。因此，为了帮助您浏览所有可用的选项，我们将在本文中探索所有这些模块，并了解如何借助 Python 的标准库来压缩、解压缩、验证、测试和保护各种格式的档案。

# 所有的格式

如上所述，Python 拥有(几乎)所有可以想象的工具/格式的库。所以，让我们先来看看它们，看看为什么你会想使用它们:

*   `zlib`是一个库和 [Python 模块](https://docs.python.org/3/library/zlib.html#module-zlib)，它提供了使用 [Deflate 压缩和解压缩](https://en.wikipedia.org/wiki/Deflate)格式的代码，这种格式被`zip`、`gzip`和其他许多人使用。因此，通过使用这个 Python 模块，您实际上是在使用`gzip`兼容的压缩算法，而没有方便的包装器。关于这个库的更多信息可以在维基百科上找到。
*   `bz2`是为`bzip2`压缩提供支持的[模块](https://docs.python.org/3/library/bz2.html#module-bz2)。该算法通常比 deflate 方法更有效，但可能会更慢。它也只对单个文件起作用，因此不能创建档案。
*   `lzma`既是算法的名字，也是 [Python 模块](https://docs.python.org/3/library/lzma.html#module-lzma)的名字。它可以产生比一些旧方法更高的压缩比，并且是`xz`实用程序(更具体地说是 LZMA2)背后的算法。
*   `gzip`是我们大多数人都熟悉的一种实用工具。这也是一个 [Python 模块](https://docs.python.org/3/library/gzip.html#module-gzip)的名字。这个模块使用已经提到的`zlib`压缩算法，并作为一个类似于`gzip`和`gunzip`实用程序的接口。
*   `shutils`是一个[模块](https://docs.python.org/3/library/shutil.html#archiving-operations)，我们通常不会将其与压缩和解压缩联系起来，但它提供了处理档案的实用方法，可以方便地生成`tar`、`gztar`、`zip`、`bztar`或`xztar`档案。
*   `zipfile`——顾名思义——允许我们用 Python 处理`zip`文档。这个[模块](https://docs.python.org/3/library/zipfile.html#module-zipfile)提供了创建、读取、写入或附加到 ZIP 文件的所有预期方法，以及更容易操作这些文件的类和对象。
*   `tarfile` -和上面的`zipfile`一样，你大概可以猜到这个[模块](https://docs.python.org/3/library/tarfile.html#module-tarfile)是用来处理`tar`档案的。可以读写`gzip`、`bz2`、`lzma`文件或档案。它还支持我们从`tar`实用程序中了解到的其他功能，这些功能的列表可以在上面的链接文档页面的顶部找到。

# 压缩和解压缩

我们有很多图书馆可供选择。其中一些更基本，一些有很多额外的特性，但它们的共同点是(显然)都包括压缩功能。因此，让我们来看看如何对它们中的每一个执行这些基本操作:

第一个上来，`zlib`。这是一个相当低级的库，因此可能不会经常使用，所以让我们只看一下整个文件的基本压缩/解压缩:

在上面的代码中，我们使用由`head -c 1MB </dev/zero > data`生成的输入文件，它给了我们 1MB 的零。我们打开并读取这个文件到内存中，然后使用`compress`函数创建压缩数据。然后，这些数据被写入输出文件。为了证明我们能够恢复数据，我们再次打开压缩文件并对其使用`decompress`函数。从打印语句中，我们可以看到压缩和解压缩数据的大小是匹配的。

下一个你可以使用的格式和库是`bz2`。它的使用方式与上面的`zlib`非常相似:

不出所料，这些模块的接口几乎是相同的，所以为了显示一些不同，在上面的例子中，我们简化了压缩步骤，减少到几乎只有一行，并使用`os.stat`来检查文件的大小。

这些低级模块中的最后一个是`lzma`，为了避免重复显示相同的代码，我们这次做一个增量压缩:

我们首先创建一个输入文件，该文件由从`/usr/share/dict/words`中提供的字典中提取的一串单词组成。这是为了让我们实际上可以确认解压缩的数据是相同的原始数据。

然后我们像前面的例子一样打开输入和输出文件。然而这一次，我们以 1024 位的块迭代随机数据，并使用`LZMACompressor.compress`压缩它们。这些块然后被写入输出文件。在整个文件被读取和压缩后，我们需要调用`flush`来完成压缩过程，并从压缩器中清除任何剩余的数据。

为了确认这是可行的，我们以通常的方式打开并解压缩文件，首先打印文件中的几个单词。

继续学习更高级别的模块——现在让我们使用`gzip`来完成相同的任务:

在这个例子中，我们结合了`gzip`和`shutils`。看起来我们像之前用`zlib`或`bz2`做了同样的批量压缩，但是由于`shutil.copyfileobj`我们得到了分块的增量压缩，而不必像用`lzma`那样循环数据。

`gzip`模块的一个优点是它还提供了命令行接口，我不是在说 Linux `gzip`和`gunzip`而是在说 Python 集成:

# 拿把大锤子来

如果你更喜欢使用`zip`或`tar`，或者你需要其中一个提供的格式的文档，那么这一节将告诉你如何使用它们。除了基本的压缩/解压缩操作，这两个模块还包括一些其他的实用方法，如测试校验和，使用密码或在档案中列出文件。因此，让我们深入了解一下所有这些活动。

这是一段相当长的代码，但是涵盖了`zipfile`模块的所有重要特性。在这个代码片段中，我们首先使用`ZipFile`上下文管理器在*“write”*(`w`)模式下创建 ZIP 存档，然后将文件添加到这个存档中。您会注意到，我们实际上并不需要打开正在添加的文件——我们需要做的只是调用`write`传递文件名。添加完所有文件后，我们还使用`setpassword`方法设置了存档密码。

接下来，为了证明它有效，我们打开归档文件。在读取任何文件之前，我们检查 CRC 和文件头，之后我们检索存档中所有文件的信息。在这个例子中，我们只打印了`ZipInfo`对象的列表，但是您也可以检查它的属性来获得 CRC、大小、压缩类型等。

检查完所有文件后，我们打开并阅读其中一个。我们看到它有预期的内容，所以我们可以继续将它提取到 path 指定的文件中(`/tmp/`)。

除了创建阅读档案/文件，ZIP 还允许我们将文件添加到现有的档案中。为此，我们只需将访问模式更改为*“追加”* ( `"a"`):

和`gzip`模块一样，Python 的`zipfile`和`tarfile`也提供了 CLI。要执行基本的存档和提取，请使用以下命令:

最后但同样重要的是，`tarfile`模块。该模块类似于`zipfile`，但也实现了一些额外的功能:

我们从归档的基本创建开始，但是这里我们使用访问模式`"w:gz"`，它指定我们想要使用 GZ 压缩。之后，我们将所有文件添加到存档中。有了`tarfile`模块，我们还可以传入符号链接或整个目录，它们将被递归添加。

接下来，为了确认所有的文件都确实存在，我们使用了`getmembers`方法。为了深入了解单个文件，我们可以使用`gettarinfo`，它提供了所有的 Linux 文件属性。

`tarfile`提供了一个我们在其他模块中没有见过的很酷的特性，那就是当文件被添加到存档时，可以修改文件的属性。在上面的代码片段中，我们通过提供修改`TarInfo.mode`的`filter`参数来更改文件的权限。该值必须以八进制数的形式提供，这里`0o100600`将权限设置为`0600`或`-rw-------.`。

为了在做了这一更改后获得文件的完整概览，我们可以运行`list`方法，它给出了类似于`ls -l`的输出。

最后要做的是打开并解压`tar`文件。为此，我们使用`"r:gz"`模式打开它，使用文件名检索 info 对象(`member`)，检查它是否真的是一个文件，并将其提取到所需位置:

# 结论

正如你所看到的，Python 的模块提供了很多选项，有低级的也有高级的，有特定的也有通用的模块，有简单的也有更复杂的接口。您选择什么取决于您的用例及需求，但一般来说，我会建议使用通用模块，如`zipfile`或`tarfile`，只有在必要时才求助于像`lzma`这样的模块。

我试图涵盖这些模块的所有常见用例，以便为您提供完整的概述，但显然还有更多函数、对象、属性等。因此，请务必查看第一部分中链接的文档，找到一些其他有用的信息。

*本文最初发布于*[*martinheinz . dev*](https://martinheinz.dev/blog/57?utm_source=medium&utm_medium=referral&utm_campaign=blog_post_57)

</the-unknown-features-of-pythons-operator-module-1ad9075d9536>  </functools-the-power-of-higher-order-functions-in-python-8e6e61c6e4e4>  </the-correct-way-to-overload-functions-in-python-b11b50ca7336> [## Python 中重载函数的正确方法

towardsdatascience.com](/the-correct-way-to-overload-functions-in-python-b11b50ca7336)