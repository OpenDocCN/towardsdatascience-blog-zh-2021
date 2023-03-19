# Python 熊猫阅读 CSV

> 原文：<https://towardsdatascience.com/python-pandas-reading-a-csv-7865a11939fd?source=collection_archive---------19----------------------->

## 学习如何阅读 CSV 文件并创建熊猫数据帧

![](img/663d5ba395f7cbdf8f395bb8e90097ae.png)

照片由[米卡·鲍梅斯特](https://unsplash.com/@mbaumi?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

# 介绍

作为数据分析师或数据科学家，您将经常需要组合和分析来自各种数据源的数据。我经常被要求分析的一种数据类型是 CSV 文件。CSV 文件在企业界很受欢迎，因为它们可以处理强大的计算，易于使用，并且通常是企业系统的输出类型。今天我们将演示如何使用 [Python](https://www.python.org/) 和 [Pandas](https://pandas.pydata.org/) 打开并读取本地机器上的 CSV 文件。

# 入门指南

你可以通过 pip 从 [PyPI](https://pypi.org/project/pandas/) 安装 Panda。如果这是你第一次安装 Python 包，请参考 [Pandas 系列&数据帧讲解](/pandas-series-dataframe-explained-a178f9748d46)或 [Python Pandas 迭代数据帧](/python-pandas-iterating-a-dataframe-eb7ce7db62f8)。这两篇文章都将为您提供安装说明和今天文章的背景知识。

# 句法

在学习熊猫时，对我来说最具挑战性的部分是关于熊猫功能的大量教程，比如`.read_csv()`。然而，教程往往忽略了处理真实世界数据时所需的复杂性。一开始，我经常发现自己不得不在 StackOverflow 上发布问题，以学习如何应用特定的参数。下面我们包括了所有的参数，以及概念上更复杂的例子。

上面的 python 片段显示了 Pandas read csv 函数的语法。

上面的语法乍一看似乎很复杂；然而，我们将只设置少数几个参数，因为大多数参数都被赋予了默认值。然而，这些参数预示着熊猫强大而灵活的天性。

# 因素

*   `filepath_or_buffer`:您可以传入一个字符串或路径对象，该对象引用您想要读取的 CSV 文件。该参数还接受指向远程服务器上某个位置的 URL。

上面的 Python 片段展示了如何通过向 filepath_or_buffer 参数提供文件路径来读取 CSV。

*   `sep`&`delimiter`:`delimiter`参数是`sep`的别名。你可以使用`sep`来告诉熊猫使用什么作为分隔符，默认情况下这是`,`。但是，您可以为制表符分隔的数据传入 regex，如`\t`。
*   `header`:该参数允许您传递一个整数，该整数捕获 CSV 头名称所在的行。默认情况下，`header`被设置为`infer`，这意味着 Pandas 将从第 0 行获取标题。如果您打算重命名默认标题，则将`header`设置为`0`。
*   `name`:这里您有机会覆盖默认的列标题。为此，首先设置`header=0`，然后传入一个数组，该数组包含您想要使用的新列名。

上面的 Python 片段重命名了原始 CSV 的头。我们在上面的例子中使用的 CSV 的副本可以在[这里](https://gist.github.com/deanjamesss/662a90fde120188ae570cfba83e6a1ef.js)找到。

*   `index_col`:对于不熟悉 DataFrame 对象的人来说，DataFrame 的每一行都有一个标签，称为索引。如果 CSV 文件包含表示索引的列，则可以传递列名或整数。或者，您可以通过`False`告诉 Pandas 不要使用您的文件中的索引。如果`False`通过，Pandas 将使用一系列递增的整数创建一个索引。
*   `usecols`:您可以使用该参数返回文件中所有列的子集。默认情况下，`usecols`被设置为`None`，这将导致 Pandas 返回 DataFrame 中的所有列。当您只对处理某些列感兴趣时，这很方便。

在上面的 Python 片段中，我们告诉 Pandas 我们只希望读取第 1 和第 2 列。您可以使用[这个 CSV](https://gist.github.com/deanjamesss/ff26390b8dc8083abfa6750a8302f524.js) 来测试上面的代码片段。

*   `squeeze`:当处理一个单列 CSV 文件时，您可以将该参数设置为`True`，这将告诉 Pandas 返回一个系列，而不是一个数据帧。如果对熊猫系列不熟悉，可以参考[熊猫系列& DataFrame 讲解](/pandas-series-dataframe-explained-a178f9748d46)进行概述。
*   `prefix`:如果你还没有指定要使用的列标签前缀，你可以在这里设置。当没有指定列时，默认行为是使用整数序列来标记它们。使用该参数，您可以将列`0`、`1`和`2`设置为`column_0`、`column_1`和`column_2`。

在上面的 Python 片段中，我们试图读取一个没有头文件的 CSV 文件。默认情况下，熊猫会添加数字列标签。在上面，我们已经用“column_”作为列的前缀。我们在这个例子中使用的 CSV 可以在这里找到[。](https://gist.github.com/deanjamesss/1b6399a2dc6489f4f1e2218e24cd8dff.js)

*   `mangle_dupe_cols`:如果您正在阅读的 CSV 文件包含同名的列，Pandas 将为每个重复的列添加一个整数后缀。将来`mangle_dupe_cols`将接受`False`，这将导致重复的列相互覆盖。
*   `dtype`:您可以使用这个参数来传递一个字典，这个字典将列名作为键，数据类型作为它们的值。当您有一个以零填充的整数开头的 CSV 时，我发现这很方便。为每一列设置正确的数据类型也将提高操作数据帧时的整体效率。

上面我们已经确保了 employee_no 列将被转换为一个字符串。这还演示了保留前导零的能力，因为您会注意到数据集中的 job_no 没有被转换，因此丢失了前导零。数据集可以在[这里](https://gist.github.com/deanjamesss/a42b4a8e930ae14d536b50155b324311.js)找到。

*   `engine`:目前熊猫接受`c`或`python`作为解析引擎。
*   `converters`:这遵循与`dtype`相似的逻辑，但是，除了传递数据类型，您还可以传递在读取时操作特定列中的值的函数。

上面的 Python 代码片段将应用 double_number()函数，我们将它定义为第 1 列的转换器。我们的示例 CSV 文件可以在[这里](https://gist.github.com/deanjamesss/adab4a5a30d624f05cdbe919c7ddace2.js)找到。

*   `true_values` & `false_values`:这个参数挺俏皮的。例如，在您的 CSV 中，您有一个包含`yes`和`no`的列，您可以将这些值映射到`True`和`False`。这样做将允许您在将文件读入 Pandas 时清除一些数据。

上面的 Python 片段演示了如何定义 True & False 值。这里我们将 yes & maybe 设为 True，no 设为 False。此处可找到一个 CSV 示例[。](https://gist.github.com/deanjamesss/efbc4bd1e3532f0a49cdf010ea4347e4.js)

*   `skipinitialspace`:您可以将此参数设置为`True`，告诉熊猫分隔符后可能有前导空格的行。然后，Pandas 将删除分隔符之后和非分隔符之前的任何前导空格。
*   `skiprows`:在处理系统生成的 CSV 文件时，有时文件的开头会包含参数行。通常我们不想处理这些行，而是跳过它们。您可以将`skiprows`设置为一个整数，表示在开始读取之前要跳过的行数。或者，您可以提供一个 callable，当函数计算结果为`True`时，它将导致 Pandas 跳过一行。
*   `skipfooter`:类似于`skiprows`这个参数允许你告诉熊猫在文件的末尾要跳过多少行。同样，如果报告参数在 CSV 文件的末尾，这也很方便。
*   `nrows`:您可以使用它来设置从 CSV 文件中收集的行数的限制。我发现在探索阶段，当试图对数据有所感觉时，这很方便。这意味着您可以测试您的逻辑，而不必将大文件加载到内存中。
*   `na_values`:默认情况下，Pandas 有大量的值被映射到`NaN`(不是一个数字)。如果您有需要清理和映射的特定于应用程序的值，可以将它们传递给此参数。使用这个参数意味着您可以捕获所有的值，这些值都可以映射到一个默认的预处理。
*   `keep_default_na`:该参数可以设置为`True`或`False`。如果`False`和 CSV 包含默认的`NaN`值，那么 Pandas 将保留原来的`NaN`值。如果`True` Pandas 将解析`NaN`值并在数据帧中用`NaN`屏蔽。
*   `na_filter`:当您希望 Pandas 解释您的数据中缺失的值时，您可以将此设置为`True`。提示一下，当读取您知道没有任何丢失值的大文件时，将此参数设置为`False`。
*   `verbose`:默认设置为`False`。将`verbose`设置为`True`将向控制台输出额外的数据，如`NaN`值的数量或特定过程花费的时间。
*   有时，我们收到的数据可能包含空行。通过将`skip_blank_lines`设置为`True`，Pandas 将跳过这些行，而不是将它们计为`NaN`值。
*   `parse_dates`:使用这个参数告诉 Pandas 你希望 CSV 文件中的日期如何被解释。你可以通过`True`，这会让熊猫把索引解析成日期。或者，您可以传递 Pandas 将用来创建日期的列名或列列表。

上面的 Python 代码片段将第 0 列转换成索引，并将其解析为日期。用于本例的 CSV 文件可以在这里找到[。运行上述脚本时，请注意针对日期时间格式对列 0 所做的更改。](https://gist.github.com/deanjamesss/ad38f306a4005dc7ed104696e6e303f6.js)

*   `infer_datetime_format`:您可以将此参数设置为`True`，它将告诉熊猫推断日期时间格式。当与`parse_dates`结合时，这样做将导致更大的处理速度。
*   `keep_date_col`:如果您已经为`parse_dates`设置了一个值，您可以使用该参数来保留创建数据的列。默认行为是将这些列放到适当的位置。如果您不希望这种情况发生，请将`keep_date_col`设置为`True`。

上面的 Python 脚本将通过尝试解析列 0、1 和 2 来创建一个日期。此外，keep_date_col 已被设置为 True，这将导致保留列 0、1 和 2。我们的示例 CSV 可以在[这里](https://gist.github.com/deanjamesss/df9916c1cd681a1b03d55e61cf220935.js)找到。

*   `date_parser`:如果您已经知道 CSV 中的日期格式，您可以传递一个函数给`date_parser`来有效地格式化日期时间，而不是推断格式。
*   `dayfirst`:如果你的日期时间格式是`DD/MM`，则通过`True`。
*   `cache_dates`:默认设置为`True`。Pandas 将创建一组独特的日期-时间字符串转换，以加快重复字符串的转换。
*   `iterator`:将该参数设置为`True`将允许您调用 Pandas 函数`.get_chunk()`，该函数将返回要处理的记录数。
*   `chunksize`:这将允许您设置数据帧内块的大小。这样做很方便，因为您可以循环数据帧的一部分，而不是将整个数据帧延迟加载到内存中。
*   `compression`:如果您正在读取的数据在磁盘上被压缩，那么您可以设置动态解压缩的压缩类型。
*   `thousands`:这是千位单位的分隔符。在 CSV 文件中，你有时可以看到用`1_000_000`表示的一百万，因为`,`被用作分隔符。将千设置为`_`将导致`1_000_000`反映为`1000000`。
*   `decimal`:如果偏离`.`，您可以在 CSV 文件中提供代表小数的字符。
*   `lineterminator`:如果你已经将`engine`设置为`c`，你可以用这个参数告诉熊猫你希望这些行以什么字符结束。
*   `quotechar`:这是在整个 CSV 文件中使用的字符，表示引用元素的开始和结束。
*   `quoting`:在这里你可以设置你想要应用到你的元素的报价级别。默认情况下，这是 0，将报价设置为最小；您也可以将其设置为 1-全部引用、2-引用非数字或 3-不引用。
*   `doublequote`:当两个引号字符出现在一个引号元素中时，您可以使用这个参数告诉 Pandas 该做什么。当`True`通过时，双引号字符将变成单引号字符。
*   `escapechar`:长度为一的字符串，熊猫将使用它来转义其他字符。
*   `comment`:您可以使用该参数来表示您不想处理该行的剩余部分。例如，如果`comment`设置为`#`并且`#`出现在当前行中，熊猫到达`#`后将移动到下一行。
*   `encoding`:如果您正在使用非英语的数据，请将此值设置为特定的字符编码，以便可以正确读取数据。
*   `dialect`:CSV 方言是一组告诉 CSV 解析器如何读取 CSV 文件的参数。常见的方言有`excel`、`excel-tab`和`unix`另外，你可以自己创作，传给熊猫。
*   `error_bad_lines`:如果 Pandas 遇到一个具有两个以上属性的行，通常会引发一个异常，Python 会暂停执行。如果将`False`传递给`error_bad_lines`，那么任何通常会引发此类异常的行将从数据帧中删除。
*   `warn_bad_lines`:如果您已经将`error_bad_lines`设置为`False`，那么您可以将`warn_bad_lines`设置为`True`，这将输出每一行会引发异常的代码。
*   `delim_whitespace`:这个参数与`delimiter`相似，但是它只针对空白。如果你想用空格作为分隔符，那么你可以将`delimiter`设置为`\s+`，或者将`delim_whitespace`设置为`True`。
*   `low_memory`:默认情况下，Pandas 将此设置为`True`，这将导致分块处理，然而，存在不匹配类型推理的风险。通过确保设置了`dtype`参数，可以避免可能的类型不匹配。
*   `memory_map`:如果你已经传递了一个文件给`filepath_or_buffer` Pandas 在内存中映射文件对象，以提高处理较大文件的效率。
*   `float_precision`:您可以在这里为浮动元素的`c`引擎设置合适的转换器。
*   `storage_options`:从远程位置读取 CSV 文件时，可以使用该参数传递特定选项。

# 接下来去哪里

既然您已经了解了如何使用 Pandas `.read_csv()`，我们的建议是通过 [Pandas 系列& DataFrame 解释](/pandas-series-dataframe-explained-a178f9748d46)来学习更多关于 Pandas 数据结构的知识，或者在 [Python Pandas 迭代 DataFrame](/python-pandas-iterating-a-dataframe-eb7ce7db62f8) 中学习如何导航数据帧。如果你已经掌握了这些概念，你的下一步应该是阅读[旋转熊猫数据框架](/pivoting-a-pandas-dataframe-c8ddfae35d2)或[如何组合 Python、熊猫& XlsxWriter](/how-to-combine-python-pandas-xlsxwriter-8edd25678a6f) 。

# 摘要

作为一名数据分析师，学习如何使用 Pandas `.read_csv()`是一项至关重要的技能，可以将各种数据源结合起来。正如你在上面看到的,`.read_csv()`是一个非常强大和灵活的工具，你可以适应各种现实世界的情况来开始你的数据收集和分析。

感谢您花时间阅读我们的故事，我们希望您发现它很有价值。