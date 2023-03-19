# Python 干净代码:让 Python 函数更具可读性的 6 个最佳实践

> 原文：<https://towardsdatascience.com/python-clean-code-6-best-practices-to-make-your-python-functions-more-readable-7ea4c6171d60?source=collection_archive---------2----------------------->

## 停止编写需要三分钟以上才能理解的 Python 函数

![](img/df8bee93f05daefd61f9fe9380b3db35.png)

图为[创意交流](https://unsplash.com/@thecreative_exchange?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 动机

你有没有看过自己一个月前写的函数，发现 3 分钟很难理解？如果是这样的话，是时候重构你的代码了。如果你花了超过 3 分钟来理解你的代码，想象一下你的队友需要多长时间来理解你的代码。

如果你希望你的代码是可重用的，你希望它是可读的。编写干净的代码对于与不同角色的其他团队成员合作的数据科学家来说尤其重要。

您希望您的 Python 函数:

*   变小
*   做一件事
*   包含具有相同抽象级别的代码
*   少于 4 个参数
*   没有重复
*   使用描述性名称

这些实践将使您的函数更具可读性，更容易发现错误。

受到 Robert C. Martin 的《干净的代码:敏捷软件工艺手册》一书的启发，我决定写一篇关于如何用 Python 为数据科学家编写干净代码的文章。

在本文中，我将向您展示如何利用上面提到的 6 种实践来编写更好的 Python 函数。

# 开始

我们先来看看下面的函数`load_data`。

函数`load_data`试图从 Google Drive 下载数据并提取数据。即使这个函数有很多评论，3 分钟也很难理解这个函数是做什么的。这是因为:

*   这个函数太长了
*   该函数试图做多件事
*   函数中的代码处于多个抽象层次。
*   该函数有 3 个以上的参数
*   有多处重复
*   函数名不是描述性的

我们将通过使用上面提到的 6 个实践来重构这段代码

# 小的

函数应该很小，因为更容易知道函数做什么。多小才算小？一个函数中的代码应该很少超过 20 行。可以小到如下图。函数的缩进级别不应大于一或二。

# 做一个任务

一个函数应该只完成一个任务，而不是多个任务。函数`load_data`尝试执行多项任务，例如下载数据、解压缩数据、获取包含训练和测试数据的文件名，以及从每个文件中提取文本。

因此，它应该被分成如下多个功能

而且每个功能应该只做一件事:

函数`download_zip_data_from_google_drive`只从 Google Drive 下载一个 zip 文件，其他什么都不做。

# 一个抽象层次

函数`extract_texts_from_multiple_files`中的代码与函数处于不同的抽象层次。

> 抽象层次是观察或编程一个系统的复杂程度。级别越高，细节越少。级别越低，越详细。— PCMag

这就是为什么:

*   功能`extract_texts_from_multiple_files`是一个高层次的抽象。
*   代码`list_of_text_in_one_file =[r.text for r in ET.parse(join(path_to_file, file_name)).getroot()[0]]`处于一个较低的抽象层次。

为了使函数中的代码处于相同的抽象级别，我们可以将低级代码放入另一个函数中。

现在，代码`extract_texts_from_each_file(path_to_file, file)`处于一个高抽象层次，与函数`extract_texts_from_multiple_files`处于同一抽象层次。

# 复制

下面的代码有重复。用于获取训练数据的代码部分与用于获取测试数据的代码部分非常相似。

我们应该避免重复，因为:

*   这是多余的
*   如果我们对一段代码做了更改，我们需要记住对另一段代码做同样的更改。如果我们忘记这样做，我们会在代码中引入错误。

我们可以通过将重复的代码放入函数中来消除重复。

由于从训练文件中提取文本的代码和从测试文件中提取文本的代码是相似的，我们将重复的代码放入函数`extract_tests_from_multiple_files`中。这个函数可以从训练或测试文件中提取文本。

# 描述性名称

> 一个长的描述性的名字比一个短的神秘的名字更好。一个长的描述性名称比一个长的描述性注释更好。—罗伯特·马丁的《干净的代码》

用户可以通过查看其名称来了解函数`extract_texts_from_multiple_files`的作用。

不要害怕写长名字。**写长名字比写模糊名字好。**如果你试图通过编写类似`get_texts`的代码来缩短你的代码，其他人不看源代码就很难理解这个函数到底做了什么。

如果函数的描述性名称太长，如`download_file_from_ Google_drive_and_extract_text_from_that_file`。这是一个很好的迹象，表明你的函数正在做多件事情，你应该把它分成更小的函数。

# 少于 4 个参数

一个函数的参数不应超过 3 个，因为这表明该函数正在执行多项任务。测试一个超过 3 种不同变量组合的函数也很困难。

例如，函数`load_data`有 4 个参数:`url`、`output_path`、`path_train`和`path_test` 。所以我们可能会猜测它试图同时做多件事:

*   使用`url`下载数据
*   在`output_path`保存
*   提取`output_path`中的训练和测试文件，并保存到`path_train`、`path_test`

如果一个函数有 3 个以上的参数，考虑把它变成一个类。

例如，我们可以将`load_data`分成 3 个不同的函数:

由于函数`download_zip_data_from_google_drive`、`unzip_data`和`get_train_test_docs`都试图实现一个目标:获取数据，我们可以将它们放入一个名为`DataGetter`的类中。

*旁注:在上面的代码中，我使用* `*staticmethod*` *作为一些方法的装饰器，因为这些方法不使用任何类属性或类方法。更多关于这些方法的* [*在这里*](https://realpython.com/instance-class-and-static-methods-demystified/) *。*

正如我们所看到的，上面的函数都没有超过 3 个参数！尽管使用类的代码比使用函数的代码更长，但是可读性更好！我们也确切地知道每段代码做什么。

# 我怎么写这样一个函数？

开始写代码的时候不要试图做到完美。从写下符合你想法的复杂代码开始。然后随着代码的增长，问问你自己你的函数是否违反了上面提到的任何实践。如果是，重构它。[测试一下](/pytest-for-data-scientists-2990319e55e6)。然后继续下一个功能。

# 结论

恭喜你！您刚刚学习了 6 种编写可读和可测试函数的最佳实践。由于每个函数执行一项任务，这将使您更容易测试您的函数，并确保它们在发生更改时通过单元测试。

如果你能让你的队友毫不费力地理解你的代码，他们会很乐意在其他任务中重用你的代码。

这篇文章的源代码可以在[这里](https://github.com/khuyentran1401/Data-science/tree/master/python/good_functions)找到。

我喜欢写一些基本的数据科学概念，并尝试不同的算法和数据科学工具。你可以通过 [LinkedIn](https://www.linkedin.com/in/khuyen-tran-1ab926151/) 和 [Twitter](https://twitter.com/KhuyenTran16) 与我联系。

如果你想查看我写的所有文章的代码，请点击这里。在 Medium 上关注我，了解我的最新数据科学文章，例如:

</pytest-for-data-scientists-2990319e55e6>  </stop-using-print-to-debug-in-python-use-icecream-instead-79e17b963fcc>  </3-python-tricks-to-read-create-and-run-multiple-files-automatically-5221ebaad2ba>  </how-to-get-a-notification-when-your-training-is-complete-with-python-2d39679d5f0f>  

# 参考

马丁，R. C. (2009 年)。干净的代码:敏捷软件技术手册。上马鞍河:普伦蒂斯霍尔。