# 深度学习。人工智能和 ML 教程代码质量问题

> 原文：<https://towardsdatascience.com/deeplearning-ai-and-the-ml-tutorial-code-quality-problem-4e3a37109383?source=collection_archive---------13----------------------->

## 干净的代码很重要！甚至(特别是)在机器学习研究领域

![](img/7fad2ff9927d4b86d0bbd1ae7274510d.png)

由 [Charles Deluvio](https://unsplash.com/@charlesdeluvio?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

2014 年，吴恩达的 Coursera 课程让我第一次接触到了机器学习。这些课程提供了一个可访问的，全面的 ML 方法回顾；他们让我着迷了！

最近重温了一些课程内容，报名参加了[深度学习。AI TensorFlow 开发者专精](https://www.coursera.org/professional-certificates/tensorflow-in-practice)作为进修。和以前一样，我对解释的清晰性感到惊讶。讲师建立重要的直觉，并慢慢分解复杂的技术内容。

然而，在整个课程中，我注意到一个我在机器学习教程源代码中看到的问题:糟糕的代码质量。下面，我列举了一些风格上的例子。我还指出了一些来自[谷歌风格指南](https://google.github.io/styleguide/pyguide.html)的风格惯例，这将提高可读性！

## 不一致的报价使用

样式指南备选方案:“与文件中字符串引用字符的选择保持一致。选择`'`或`"`并坚持下去。可以在字符串中使用另一个引号字符，以避免需要在字符串中对引号字符进行反斜线转义。

## **不一致的空格**

样式指南备选方案:“用一个空格将二进制运算符括起来，用于赋值(`=`)、比较(`==, <, >, !=, <>, <=, >=, in, not in, is, is not`)和布尔(`and, or, not`)。对于在算术运算符(`+`、`-`、`*`、`/`、`//`、`%`、`**`、`@`)周围插入空格，请使用您更好的判断……不要使用空格来垂直对齐连续行上的标记，因为这会成为维护负担(适用于`:`、`#`、`=`等)。)"

## 不一致的文档样式

风格指南替代方案:“注释的最后一个地方是代码中棘手的部分。如果你要在下一次[代码审查](http://en.wikipedia.org/wiki/Code_review)中解释它，你应该现在就注释它。复杂的操作在操作开始前会得到几行注释。非显而易见的注释位于行尾……为了提高可读性，这些注释应该以注释字符`#`开始，距离代码至少 2 个空格，然后在注释文本本身之前至少一个空格。

## 不一致的大小写

风格指南备选方案:“虽然它们在技术上是变量，但模块级常量是允许的，也是鼓励的。比如:`_MAX_HOLY_HANDGRENADE_COUNT = 3`。常量必须全部用带下划线的大写字母命名。见下面的[命名](https://google.github.io/styleguide/pyguide.html#s3.16-naming)。因为这些是参数，不是常量，我们应该避免使用大写形式。

## 不一致的进口水平

风格指南替代:“使用`import x`导入包和模块；使用`from x import y`，其中`x`是包前缀，`y`是不带前缀的模块名；如果要导入两个名为`y`的模块，或者如果`y`是一个不方便的长名称，则使用`from x import y as z`；仅当`z`是标准缩写时，才使用`import y as z`(例如`np`代表`numpy`)。”

显然，这些格式问题都不会从根本上影响课程。这些讲座仍然提供了宝贵的资源。不过，至少不一致的风格会分散人们对材料的注意力。最坏的情况是，它在教程环境中的展示鼓励了糟糕的软件实践，这扩大了基于笔记本的实验和操作源代码之间的鸿沟。

教育代码旨在用作参考，自然会邀请一定程度的 [cargo cult 编程](https://en.wikipedia.org/wiki/Cargo_cult_programming)。通常情况下，学习者的 Python 接触很少。这种设置提供了展示最佳实践的理想机会。

有了 nbQA 这样的工具进行林挺、格式化和静态类型检查，开发整洁的笔记本变得前所未有的简单！

<https://github.com/nbQA-dev/nbQA>  

感谢阅读！如果你同意，请在评论中告诉我！