# 将您的实验路由到文件

> 原文：<https://towardsdatascience.com/route-your-experiments-to-files-11a65be823d2?source=collection_archive---------39----------------------->

## 提高深度学习体验的小技巧

![](img/3e672faa1f3b470836c1fda9303c0b21.png)

汤姆·赫尔曼斯在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

运行深度学习实验可能是一项令人生畏的任务。您安排了一批实验通宵运行，结果前几天发现几乎所有的实验都失败了——并且任何堆栈跟踪都被其他测试遗忘了很久。多好的开始一天的方式:x .如果你和我一样，看到这种事情发生的次数比你愿意承认的多，这里有一个纯 Python 的方法来防止丢失控制台消息:

**自动控制台到文件记录器**

将任何控制台输出镜像到备份日志文件的小 Python 代码片段。

总的来说，在调用`mirror_console_to_file(file)`之后，任何发送到`stdout` 和`stderr` 流的消息都将被拦截，路由到一个文件，然后像往常一样打印出来。这模仿了 Linux 中使用纯 Python 的‘tee’命令。这具有跨所有平台工作的优势，并且它给了您对过程的编程控制。

在这个实现中，`stdout` 和`stderr` 都指向同一个文件，并且有一个带有系统时间戳的开始和结束消息，这对于检查测试完成/失败花费了多长时间很有用。更进一步，可以通过手动检查消息中的`\n`字符来添加每条消息的时间戳。

需要注意的是，使用`\r`字符的消息不会像往常一样被处理。相反，日志将按顺序包含每条消息，而不是每条消息覆盖前一条消息。这让我很恼火，所以我在开发和测试时分别打开和关闭路由代码。

如果你是那种把一堆旧测试结果放在身边的人，我强烈推荐你下载 [GitPython](https://gitpython.readthedocs.io/en/stable/intro.html) 。使用它，您可以获取当前的提交摘要/散列，并在日志的开头打印出来。这将极大地帮助您解析旧的日志，因为它将准确地告诉您在项目时间表的哪个点进行了这个测试。下面是一个片段:

```
import git
head = git.Repo(search_parent_directories=True).head.object
summary, sha = head.summary, head.hexsha
```

最后一点，您可能想知道:为什么手动将标准输出发送到一个文件，而不是使用日志库？有几个理由支持手动方法:

*   大多数人的代码都是基于基本的 print，所以将其转换成 logger 实现可能会花费太多的时间和精力。
*   实际上，(非库)深度学习代码并不长，也不复杂，无法保证日志记录级别或分布式日志记录等功能。
*   有用的调试信息通常来自库警告，而不是您自己的消息。

有关训练深度学习模型的更多提示，请考虑阅读:

[](/taking-keras-and-tensorflow-to-the-next-level-c73466e829d3) [## 让 Keras 和 TensorFlow 更上一层楼

### 充分利用 Keras 和 TensorFlow 的 11 个技巧和诀窍

towardsdatascience.com](/taking-keras-and-tensorflow-to-the-next-level-c73466e829d3) [](/memory-efficient-data-science-types-53423d48ba1d) [## 高效内存数据科学:类型

### 通过在正确的时间使用正确的浮点和整数，节省高达 90%的内存。

towardsdatascience.com](/memory-efficient-data-science-types-53423d48ba1d) 

如果您对本文有任何问题，请随时发表评论或与我联系。如果你刚接触媒体，我强烈推荐[订阅](https://ygorserpa.medium.com/membership)。对于数据和 IT 专业人士来说，中型文章是 [StackOverflow](https://stackoverflow.com/) 的完美搭档，对于新手来说更是如此。注册时请考虑使用[我的会员链接。](https://ygorserpa.medium.com/membership)

感谢阅读:)