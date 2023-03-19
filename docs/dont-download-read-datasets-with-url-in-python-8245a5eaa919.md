# 不要下载！使用 Python 中的 URL 读取数据集

> 原文：<https://towardsdatascience.com/dont-download-read-datasets-with-url-in-python-8245a5eaa919?source=collection_archive---------9----------------------->

## 无需以任何格式在本地下载即可阅读 UCI 数据集的教程

![](img/8bb094108b95c085f6c985fd45d223cd.png)

旧方法不会打开新的门！[Unspalsh 拍摄的照片](https://unsplash.com/photos/caND1D-Kh9Y)

对于数据科学专业的学生来说，经常会用到公共数据集。 [UCI](https://archive.ics.uci.edu/ml/index.php) 是最常见的机器学习知识库，包含成千上万的数据。但是，对于一个赋值，如果您在本地下载数据集，其他人不能在其他机器上运行您的代码，除非下载并替换本地路径。此外，为了在 google collaboratory 等在线平台上训练模型，您需要将数据集保存在 google drive 上，安装 google drive 并使用它。如果没有获得谷歌硬盘的许可，这种功能在其他 collab 笔记本上也无法运行。另一方面，如果从 URL 读取数据集，就不会有这些问题。

在本教程中，我将解释如何读取五种不同类型的数据文件格式，它们是:

1.  `.data`文件格式
2.  `.csv`文件格式
3.  `.arff`文件格式
4.  `.zip`文件格式
5.  `.rar`文件格式

# **如何从网址**读取 `**data**` **或** csv 文件**格式**

感谢`pandas`，前两种情况最容易。要读取数据集，您只需要输入数据集 URL。例如:

**例 1:** [输血数据集](https://archive.ics.uci.edu/ml/datasets/Blood+Transfusion+Service+Center)`.data`格式。

读取输血数据集

**例二:** [宫颈癌数据集](https://archive.ics.uci.edu/ml/datasets/Cervical+cancer+%28Risk+Factors%29)带`.csv`格式。在本例中，我们已经知道数据集有问号形式的缺失值。因此，为了正确读取数据集，我们包含了`na_values=['?']`。

读取宫颈癌数据集

对于其他情况，第一步是使用`urllib.request.urlopen()`打开 URL，然后从请求的 URL 读取数据文件。然而，其余的取决于数据文件格式。

# 如何从 URL 读取 arff 文件格式

要读取作为数据帧的`.arff`文件格式，打开 URL 后，读取 URL 并使用`scipy.io.arff.loadarff()`加载`.arff`文件。在大多数情况下，我们也需要解码它。然后我们可以通过`pd.DataFrame()`将其转换为熊猫数据帧。

**例 3:** [糖尿病视网膜病变数据集](https://archive.ics.uci.edu/ml/datasets/Diabetic+Retinopathy+Debrecen+Data+Set)`.arff`格式。我从链接中获得了列名，以分配给数据帧。

一个常见的问题是，在某些情况下，整数列被读取为`object`数据类型，因此我们用`b'1'`代替了`1`。为了解决这个问题，我们检查了所有的列，如果数据类型是`object`，我们再次解码它们(第 18 行)。

读取糖尿病数据集

# 如何从 URL 读取`zip`文件格式

如果数据文件格式是`.zip`，它可能包含几个文件，如元数据或数据文件。要读取这样的数据文件，我们需要先解压，剩下的就看数据文件格式在 zip 文件中压缩成什么样了。

**例 4:** [肥胖水平数据集](https://archive.ics.uci.edu/ml/datasets/Estimation+of+obesity+levels+based+on+eating+habits+and+physical+condition+)`.zip`格式，其中包含一个`.csv`文件。

读取肥胖水平数据集

# 如何从 URL 读取`rar`文件格式

我相信这是最具挑战性的文件格式。有几个 Python 库可以提取和读取 rar 文件，比如`rarfile`、`patool`、`pyunpack`等。我选择`rarfile`是因为它使用起来更简单。

**例 5:** [慢性肾脏疾病数据集](https://archive.ics.uci.edu/ml/datasets/chronic_kidney_disease)`.rar`格式，其中包含多个`.arff`文件。

阅读慢性肾脏疾病数据集

# 摘要

好消息是，你可以使用包含函数的 [Python 库](https://github.com/maryami66/uci_dataset)轻松读取 UCI 数据集。到目前为止，它包含了 [36 个数据集](https://github.com/maryami66/uci_dataset/tree/main/lists)，它期待你的贡献来添加更多的数据集。😊

对于上面的例子，加载数据集最简单的方法是安装`uci_dataset`

`pip install uci_dataset`

之后，您可以通过从那里加载函数来读取上述所有数据集。

**示例 6:** 从`[uci_dataset](https://github.com/maryami66/uci_dataset)` Python 库中读取数据集。

从 uci_dataset 库中读取 UCI 数据集

[](https://sciencenotes.medium.com/membership) [## 加入媒体阅读伟大的教程和故事！

### 我写机器学习、深度学习和数据科学教程。升级阅读更多。

sciencenotes.medium.com](https://sciencenotes.medium.com/membership)