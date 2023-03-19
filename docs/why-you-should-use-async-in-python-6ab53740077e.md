# 为什么应该在 Python 中使用异步？

> 原文：<https://towardsdatascience.com/why-you-should-use-async-in-python-6ab53740077e?source=collection_archive---------5----------------------->

## 并提高您的 I/O 代码性能。

![](img/814ade8b3b1f5ae3fea606af79bc0674.png)

由 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上的 [Clément Hélardot](https://unsplash.com/@clemhlrdt?utm_source=medium&utm_medium=referral) 拍摄

从 Python 3.5 开始，可以在你的脚本中使用异步。这种发展允许使用新的关键字`async`和`await`以及新的模块 asyncio。

Async 和 Await 最初是在 C#中引入的，目的是以类似于编写阻塞代码的方式构造非阻塞代码。这些关键字随后被导出到几种编程语言中，因为它方便了异步代码的管理。

事实上，还有其他方式来编写异步代码，如果你知道 Javascript/Nodejs，那么你可能已经使用过回调了:

```
fs = require('fs');
fs.writeFile('hello.txt', 'call back exemple in nodejs', (err) => {
  if (err) return console.log(err);
  console.log('file writen');
});
```

一旦事件发生，在第二个位置传递的回调函数将被调用

此外，还有承诺，允许更好地管理具有表达性词语的异步调用:

```
import axios from 'axios'let data;
axios.get("https://jsonplaceholder.typicode.com/posts")
  .then(resp => {
                data = resp.data *//do data processing as data is actually available* })
  .catch(err => { console.log(err) })console.log(data) *// undefined even if the instruction is after the API call*
```

这些技术的一个问题是，我们脱离了影响可读性的代码水平阅读流程。无论如何，让我们回到它如何影响你作为一个 Python 用户。

## Python 中标准 IO 代码的问题。

IO 代码是调用外部服务的指令。例如，这可以是 HTTP 请求或对数据库的调用。标准 python 代码的问题在于，如果你的代码只调用这些服务，那么在继续下一条指令之前，必须等待这些服务的响应。

```
import requestsdata = (requests
       .get("https://jsonplaceholder.typicode.com/posts")
       .json()
) *# blocking code block, python waiting doing nothing*for d in data: # executed once the API has return
   print(d)
```

在上面的例子中，这不是很烦人，因为只有一个对外部服务的调用，但是让我们看看下面的代码。

```
def fetch_comments():
  data = []
  for i in range(1, 500):
    url = f"https://jsonplaceholder.typicode.com/comments/{i}"
    resp = requests.get(url).json()
    data.append(resp)fetch_comments()
```

这段代码花了 **126.95 秒在我的机器上完成**。因此，我们可以看到它不是最佳的。我们需要引入并行性，即同时执行几个请求。

## 传统方式:线程

在 Python 中出现 asyncio 之前，要处理这类问题，你必须使用线程。线程是进程的一个组成部分，可以被并发管理。然而，Python 有一个全局解释器锁，它将线程的使用限制在不使用解释代码的指令上。这不是这里的情况，因为我们使用外部服务。事不宜迟，下面是调用 API 的代码的线程版本。

API 调用的线程版本来源:作者

可以看出，线程版本的代码比同步版本需要更多的工作。事实上，我们必须管理一个队列，允许线程选择不同的 URL 进行处理。

另一方面，在我的机器上，执行这段包含 5 个线程的代码只花了 **7.05 秒，与同步版本相比减少了 94%** 。但是如果我告诉你我们可以做得更好。

## 现代方式:异步代码

让我们看看如何用异步代码进行 API 调用。

API 调用的异步版本来源:作者

这里也一样，要获得与同步版本相同的结果需要付出更多的努力，而且至少要付出与线程版本一样多的努力，那么这样做值得吗？在我的机器上，这段代码花了 **0.4880 秒**完成，你告诉我。

那么，为什么与线程相比，异步版本的性能如此之好呢？因为生成和管理线程不是免费的，而异步版本不需要管理这些机制。

## 结论

本文旨在向您展示异步代码如何提高某些任务的性能。网上有很多教程可以学习如何在 python 中使用这种新范式。

尽管 Python 支持异步已经有几年了，但你必须记住，并不是所有的旧库都支持异步，因为这意味着必须重写代码。例如，您一定已经注意到，我不能在异步示例中使用请求，而是使用 aiohttp，它是等效的异步版本

如果您确信并希望切换到异步模式， [aio-libs](https://github.com/aio-libs) repository 提供了一套基于 asyncio 的库，他们可能有一个符合您的需求。