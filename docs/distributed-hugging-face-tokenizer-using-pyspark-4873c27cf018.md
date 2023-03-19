# 使用 PySpark 的分布式拥抱人脸标记器

> 原文：<https://towardsdatascience.com/distributed-hugging-face-tokenizer-using-pyspark-4873c27cf018?source=collection_archive---------32----------------------->

![](img/e38d85b7a24870418b121aafc35119b4.png)

埃里克·普劳泽特在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

## 如何广播 tokenizer 并将其与 UDF 一起使用

在这篇短文中，我将展示如何使用[拥抱脸](https://huggingface.co/) [记号赋予器](https://github.com/huggingface/tokenizers)。在详细介绍如何使用 PySpark 在 DataFrame 上应用 tokenizer 之前，让我们先看一些简单的 tokenizer 代码。

上面的代码只是加载了一个预先训练好的记号赋予器 [roberta-base](https://huggingface.co/transformers/model_doc/roberta.html) 。它打印出一本有两个关键字的字典。在本文中，我们将只关注*input _ id*，它们基本上是与单词“hello”相对应的标记。

在数据帧上使用记号赋予器的第一步是将其转换为 UDF。在下面的代码中，我们创建了一个方法 tokenize，它接受一个字符序列(字符串),我们在输入字符串上使用了前面启动的 tokenizer。我们只输出键 *input_ids 的值。*由于值是整数列表，我们将模式定义为 ArrayType(IntegerType())。

现在让我们在一个测试数据框架上使用这个 UDF。首先，我们创建一个包含两行的 DataFrame，字段为“value”。然后我们使用 *tokenize_udf* 来标记每一行中的“value”字段。

# 广播标记器

在上面的实现中，标记器不是显式传播的，而是隐式传播的。我们可以广播标记化器，并使用广播变量进行标记化。

在上面的代码中，我们广播了 tokenizer，并在 *bc_tokenize* 方法中使用了广播的变量 *bc_tokenizer* ，该变量又在 *bc_tokenize_udf* 中被转换以用于 DataFrame。

在这篇短文中，我解释了如何在 DataFrame 中以分布式方式使用标记化器。虽然由于 Rust 实现，标记化器通常很快，但在开发人员使用 Spark 处理数据的管道中，这比在机器上收集所有数据、处理(标记化)并重新转换为数据帧更方便。尤其是流式实现从基于 UDF 的记号赋予器中获益更多。