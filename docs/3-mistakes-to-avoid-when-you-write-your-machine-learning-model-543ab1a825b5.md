# 编写机器学习模型时要避免的 3 个错误

> 原文：<https://towardsdatascience.com/3-mistakes-to-avoid-when-you-write-your-machine-learning-model-543ab1a825b5?source=collection_archive---------36----------------------->

## 机器学习

## 关于如何优化机器学习模型的开发过程以避免部署阶段出现意外的一些提示。

![](img/79a6dde51b31e90d6571f93570b1f594.png)

由[尼克·奥瓦尔](https://unsplash.com/@astro_nic25?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 拍摄的照片

最终我可以松一口气了:**我的机器学习模型在训练和测试集上都工作得非常好**。用于衡量我的模型性能的所有指标都达到了非常高的性能。我终于可以说我的工作差不多完成了:只需要部署就可以了。

**相反，正是在部署阶段，所有的问题都出现了**:根据新数据，模型似乎给出了不好的结果。最重要的是，它似乎有代码实现的问题。

在本文中，我描述了在开发机器学习模型时必须避免的三个常见错误，以防止部署阶段出现意外。

# 1 忘记保存缩放器

在机器学习模型开发的第一阶段，我们着手清理数据，并将其规范化和标准化。

在预处理阶段，可能出现的错误之一是执行以下类型的操作:

```
df['normalized_column'] = df['column']/df['column'].max()
```

在前面的例子中，所有东西似乎都可以在原始数据集上工作。但是，当你去部署时，列规范化的问题就出现了。**您应该使用哪个值作为比例因子？**

这个问题的一个可能的解决方案是将比例因子存储在某个地方:

```
file = open("column_scale_factor.txt", "w")
scale_factor = df['column'].max()
file.write(scale_factor)
file.close()
```

前面的解决方案非常简单。或者，可以使用更强大的缩放器，如 Scikit-learn 预处理包提供的缩放器:

```
form [sklearn.preprocessing](https://scikit-learn.org/stable/modules/classes.html#module-sklearn.preprocessing) import MaxAbsScalerscaler = **MaxAbsScaler**()
feature = np.array(df[column]).reshape(-1,1)
scaler.fit(feature)
```

**安装好定标器后，不要忘记保存它！您将在部署阶段使用它！**

我们可以通过利用`joblib` Python 库来保存缩放器:

```
import joblibjoblib.dump(scaler, 'scaler_' + column + '.pkl')
```

听起来不错的 Gif 由 Giphy 上的 Leroy Patterson 制作

# 2 无限制地增加您的笔记本数量

在寻找表示数据的最佳模型时，我们可能会在获得最佳模型之前测试不同的算法和不同的场景。

在这种情况下，*一个可能的错误是在我们测试新算法*时创建一个新笔记本。**随着时间的推移，您的文件系统中可能会出现塞满文件的文件夹。**因此，存在无法准确跟踪模型开发过程中采取的步骤的风险。

**那么如何解决问题呢？**

仅仅对代码进行评论是不够的，我们必须将**对话名称**输入到各种笔记本中。一个可能的解决方案是在笔记本的标题前加上一个**渐进数字**，该数字精确地指示在哪个点执行给定的步骤。

这里有一个可能的例子:

```
01_01_preprocessing_standard_scaler.ipynb
01_02_preprocessing_max_abs_scaler.ipynb02_01_knn.ipynb
02_02_decisiontree.ipynb
..
```

在前面的示例中，我们使用了以下命名格式:

```
stepnumber_alternative_name.ipynb
```

因此，第一个数字表示管道中的步骤号，而第二个数字(下划线后)表示可能的替代方案。

当替代方案被接受时，我们可以将文件重命名为:

```
02_02_decisiontree_accepted.ipynb
```

这样，我们在寻找测试中实际使用的模型时就变得容易了。

![](img/a809671005a018ba63121bffbfc49e5b.png)

乔纳森·沃尔夫在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

# 3 使用不同的库版本

我们可能遇到的最后一个错误是**在训练/测试和部署阶段**使用不同版本的各种被利用的库。

使用不同版本的风险是会有**意想不到的行为，这可能会导致错误的预测**。

这个问题的一个可能的解决方案是**创建一个虚拟环境**并安装所有必要的库，同时指定要使用的版本，然后在培训/测试阶段和部署阶段使用这个虚拟环境。

在我的[上一篇名为*的文章*](/have-you-ever-thought-about-using-python-virtualenv-fc419d8b0785)中，你有没有想过使用 Python virtualenv？我描述了如何建立一个虚拟环境。

![](img/06b2fa1e27520d33d42c5b1508db1587.png)

照片由[安东](https://unsplash.com/@uniqueton?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

# 摘要

在这篇短文中，我描述了在机器学习项目的建模阶段要避免的三个错误:

*   忘记保存缩放器
*   没有任何标准地增加您的笔记本
*   在培训/测试和部署阶段使用不同的库版本

如果你避免了这三个错误，你的工作肯定会进展得更快

如果你已经走了这么远来阅读，对我来说今天已经很多了。谢谢！你可以在[这篇文章](https://alod83.medium.com/which-topics-would-you-like-to-read-c68314dc6813)中读到更多关于我的信息。

# 你愿意支持我的研究吗？

你可以每月订阅几美元，解锁无限的文章——[点击这里](https://alod83.medium.com/membership)。

# 相关文章

<https://pub.towardsai.net/3-different-approaches-for-train-test-splitting-of-a-pandas-dataframe-d5e544a5316>  </have-you-ever-thought-about-using-python-virtualenv-fc419d8b0785>  </three-tricks-on-python-functions-that-you-should-know-e6badb26aac2> 