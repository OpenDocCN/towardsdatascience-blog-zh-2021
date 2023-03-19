# 掌握 R 编程应该学习的 5 R 对象

> 原文：<https://towardsdatascience.com/5-r-objects-you-should-learn-to-master-r-programming-685341ce6661?source=collection_archive---------26----------------------->

## 这些对象是每个 R 程序员工具箱中不可或缺的一部分——在本文中，我们将浏览它们并了解它们的属性。

![](img/ed9a3f1b21559b489f21eaf6b1c49625.png)

照片由[@ lmfeliciano](https://unsplash.com/@lmfeliciano)@ Unsplash.com 拍摄

T2 是一种令人敬畏的语言。作为[最受欢迎的编程语言之一](https://www.techrepublic.com/article/r-programming-language-continues-to-grow-in-popularity/)，这种语言是初学者进入数据科学世界的良好开端。它的可访问性和简单的设置使它成为想开始学习如何编码的人的最佳语言之一，特别是在数据分析和科学领域。

开始时，大多数人会立即进入学习数据框架，因为这是整个数据管道中常用的对象。但是，事实是 R 包含的远不止这些。它包含其他对象，这些对象有自己的一套特性，这些特性对于让您的 R 脚本大放异彩非常重要。学习这些对象和它们的特性应该会给你极好的工具，使你的 R 脚本灵活而高效。

在这个过程中，您将学习如何调试那些奇怪的 R 错误，这些错误可能在您操作返回这些对象之一的某些 R 基函数时发生。

让我们认识他们！

# r 向量

如果你以前已经和 R 一起工作过，你可能已经预料到了，对吗？:-)

最简单的 R 对象是向量。这种一维单一类型的对象是编程语言中许多其他对象的基础。但是不要被它的简单性所迷惑，这是一个超级强大的对象，能够在您的脚本上执行许多任务。从向数据框提供数据到帮助索引，这个对象应该很容易学习和应用。

创建向量有多种方法，但最著名的是使用命令 *c()* 或命令*:，*，例如:

```
# Vector with elements 1,2,3,4
c(1, 2, 3, 4)# Vector with 10 elements, from 1 to 10
1:10
```

它们有几个区别于其他 R 对象的属性:

*   **它们只支持 1 种类型的元素。**如果你的向量中有一个字符，如“A ”,所有其他元素将被转换为字符类型。
*   **它们是一维的，**例如，这意味着它们不能将数据表示为表格。

在哪里可以学到更多关于向量的知识？使用以下资源:

*   [教程点 R 矢量页面](https://www.tutorialspoint.com/r/r_vectors.htm)
*   [R 为绝对初学者矢量段](https://www.udemy.com/course/r-for-absolute-beginners/?referralCode=F839A741D06F0200F312)

# 数组

向量最大的缺点之一是它是一维的。当然，当你想做一些涉及多个维度的数学计算时，这是一个很大的痛苦——这在数学/统计或机器学习中很常见。幸运的是，我们有阵列！

数组是能够扩展到多维的单一类型对象。好消息是数组可以扩展到理论上无限多的维度，所以你不再局限于一个维度。

此外，当您使用数组时，您会开始理解如何在 R 中操作多维度并尝试多个索引，这对于在数据分析和争论中占据主导地位是极其重要的。您处理超过 2 维的数据的可能性很小(如果您是某种类型的机器学习工程师和/或有任何必须处理张量的用例，您可能只会处理这种类型的数据)，但是，学习如何处理非 2D 的其他对象肯定不会有什么坏处。阵列的特征:

*   **它们是多维的；**
*   **他们当时只能处理一种类型的元素；**

你可以用 R 中的 *array()* 函数创建一个数组:

```
# Creating an array with 10 elements, with 2 rows, 5 columns and 2 different tables (3 Dimensions)array(
 1:10,
 dim = c(2,5,2)
)
```

在哪里可以学到更多关于 R 数组的知识？查看这些资源:

*   [教程点 R 数组页面](https://www.tutorialspoint.com/r/r_arrays.htm)
*   [DataCamp R 阵列教程](https://www.datacamp.com/community/tutorials/arrays-in-r)
*   [R 为绝对初学者数组段](https://www.udemy.com/course/r-for-absolute-beginners/?referralCode=F839A741D06F0200F312)

# 矩阵

矩阵是数组的特例，只有二维和自己的构造函数。好的一面是，在你学习了 R 中的数组之后，你就可以操作矩阵了！

矩阵与数据框非常相似，因为它们有行和列，唯一的缺点是它们只能处理单一类型的数据(就像数组和向量一样)。它们的特征类似于数组(关于数据类型)和数据帧(关于维数):

*   **它们只有两个维度；**
*   **他们当时只能处理一种类型的元素；**

您可以使用数组来访问方法，但是矩阵有一个更漂亮的构造函数:

```
# Creating a matrix with two rows and 5 columnsmatrix(
 data = 1:10,
 nrow = 2,
 ncol = 5
)
```

了解矩阵的一些资源:

*   [教程点 R 矩阵页面](https://www.tutorialspoint.com/r/r_matrices.htm)
*   [数据手册 R 矩阵页面](https://www.datamentor.io/r-programming/matrix/)
*   [R 为绝对初学者矩阵章节](https://www.udemy.com/course/r-for-absolute-beginners/?referralCode=F839A741D06F0200F312)

# 列表

在前面的对象中，您可能已经注意到了一些东西—它们都不能处理多种类型的数据(例如，混合字符和数值)。

如果 R 只启用单一类型元素，那么在我们通常有多种类型的数据科学中执行一些常见操作将会非常麻烦——想想您过去分析过的大多数数据帧，因为它们通常混合了多种数据类型。

幸运的是，我们有两个主要的 R 对象，非常适合处理多类型元素。第一个是列表——它们是一个非常灵活的对象，使我们不仅可以存储多种类型，还可以在其中存储其他 R 对象。

例如，在一个列表中，我们可以存储一个字符串，一个数字和一个数组！这非常有趣，因为现在我们有了一个对象，它是终极的灵活性工具，使我们能够在其中存储多维对象。

要创建一个列表，可以使用 *list()* 命令:

```
# Creating an example list with a character, a number and an array
example_list <- list(
 my_char = ‘a’,
 my_number = 1,
 my_array = array(1:4, dim=c(2,2))
)
```

了解列表的一些资源:

*   [教程点 R 列表页面](https://www.tutorialspoint.com/r/r_lists.htm)
*   [R 为绝对初学者列表部分](https://www.udemy.com/course/r-for-absolute-beginners/?referralCode=F839A741D06F0200F312)

# 数据帧

现在，最著名的 R 对象之一，数据框！

数据框是数据分析的圣杯，在大量的数据科学项目中都有使用。在 R 中，它们非常灵活(最酷的是，它们实际上是一个列表！)并显示类似于其他二维对象的数据。

使用数据框，您可以像处理 SQL 中的数据表或 Excel 文件中的简单表一样处理数据。它们面向行和列，也可以有索引名。

大多数 R 教程都包含数据框示例。在编程语言中进行数据分析时，它们是最灵活、最方便的对象。哦，如果你学会了它们，当你开始用 Python 使用 [Pandas](https://pandas.pydata.org/) 库——最著名的用另一种语言操作数据帧的库之一——编码时，你会有一个良好的开端！

您可以使用 *data.frame()* 命令创建一个:

```
# Creating a data frame with two columns and two rowsexample_df <- data.frame(
 name = c(‘John’,’Mary’),
 age = c(19,20)
)
```

您可以在以下资源中了解有关数据框的详细信息:

*   [R 数据帧 DataCamp 文档](https://www.rdocumentation.org/packages/base/versions/3.6.2/topics/data.frame)
*   [教程点数据框页面](https://www.tutorialspoint.com/r/r_data_frames.htm)
*   [R 为绝对初学者数据帧段](https://www.udemy.com/course/r-for-absolute-beginners/?referralCode=F839A741D06F0200F312)

就是这样！当你想掌握 r 时，这 5 个对象是非常重要的。在了解这些之后，你应该能够理解更高级的数据结构，甚至是那些在外部库中创建的，比如[tible](https://tibble.tidyverse.org/)。

你认为还有另一个对象应该包括在这个列表中吗？写在下面的评论里吧！

***我在***[***Udemy***](https://www.udemy.com/course/r-for-absolute-beginners/?referralCode=F839A741D06F0200F312)***上开设了一门关于从零开始学习 R 的课程——这门课程是为初学者设计的，包含 100 多个练习，我希望你能在身边！***