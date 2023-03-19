# 使用 Python 解析 QIF 文件以检索金融数据

> 原文：<https://towardsdatascience.com/parsing-qif-files-to-retrieve-financial-data-with-python-f599cc0d8c03?source=collection_archive---------24----------------------->

## Quiffen 包的基本概述以及它如此有用的原因

![](img/8c6be1f0f1eb1615c7f34d2f398e53d8.png)

照片由[通讯社跟随](https://unsplash.com/@olloweb?utm_source=medium&utm_medium=referral)于 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 拍摄

如果你曾经试图完成一个围绕个人财务的数据科学项目，你可能熟悉银行导出的交易数据 CSV 文件的乐趣。

简而言之，它们非常糟糕。大多数银行会允许你将银行对账单导出为 CSV 格式，但很多银行不会告诉你这些 CSV 文件的结构有多糟糕。不同的银行对金额、参考等使用不同的标题名称，对每个银行进行硬编码几乎是不可能的。

这就是 QIF 档案的用处。QIF(Quicken Interchange Format)是由 Intuit 开发的一种文件格式，最初用于 QuickBooks 产品。然而，这种格式现在已经很老了(甚至比 OFX 还老)。

尽管如此，大多数银行和其他金融机构(股票经纪人等。)将在导出数据时提供 QIF 作为选项。尽管年代久远，QIF 文件格式仍然有用，因为:

*   它是标准化的—不再需要为不同的银行硬编码不同的 CSV 模式
*   它被广泛使用
*   它是用纯文本编写的，所以很容易解析

Quiffen 是一个 Python 包，用于解析 QIF 文件，并以更有用的格式导出数据，如字典或熊猫数据帧。Quiffen 可以通过运行以下命令从命令行安装:

```
pip install quiffen
```

从那里，您就可以开始分析您的数据了！

## 解析 QIF 文件

以下是如何解析 QIF 文件并将其中的事务导出到数据帧的代码示例:

您可以选择不同的数据来导出，例如类别(由树表示)或存储在`Qif`对象中的帐户。您还可以将数据从导出到带有标题的 CSV 文件中，每次都与`to_csv()`方法保持一致。

## 创建您自己的 QIF 文件

Quiffen 还支持 QIF 结构的创建和导出！你可以创建自己的`Qif`对象，添加账户、交易、类别、类等等。下面显示了一个例子，但是完整的 API 参考可以在 Quiffen 文档中找到:

  

您可以在 GitHub 上查看 Quiffen 包的完整源代码。任何反馈或(建设性的！)批评不胜感激！

<https://github.com/isaacharrisholt/quiffen>  

我强烈建议用你自己的交易数据来试试 Quiffen，如果你也想看看这个让我的金融屁股进入状态的程序，请也这样做！

此外，如果您有任何问题，请随时在 [**GitHub**](https://github.com/isaacharrisholt/) 上发送给我，或在 [**LinkedIn**](https://www.linkedin.com/in/isaac-harris-holt/) 上与我联系！我喜欢写我的项目，所以你可能也想在 [**Medium**](https://isaacharrisholt.medium.com/) 上关注我。

</beating-monzo-plus-with-python-and-pandas-83cb066c1b95>  

***这一条就这么多了！我希望你喜欢它，任何反馈都非常感谢！***