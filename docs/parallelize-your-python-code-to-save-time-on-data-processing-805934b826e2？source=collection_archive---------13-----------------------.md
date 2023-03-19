# 并行化您的 python 代码以节省数据处理时间

> 原文：<https://towardsdatascience.com/parallelize-your-python-code-to-save-time-on-data-processing-805934b826e2?source=collection_archive---------13----------------------->

## 为处理数据时的长时间等待而烦恼？这个博客是给你的！

![](img/93e3879685ccf9f473bc6b29938d4e26.png)

埃里克·韦伯在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上的照片

您是否曾经遇到过在处理数据时必须等待很长时间的情况？老实说，我经常遇到这种事。因此，为了减少一点痛苦，我确保使用所有人类可用的计算资源来最小化这种等待。

我们都试图通过使用时间复杂度最小的算法来优化代码的运行时。但是让我告诉你我所说的“所有计算资源”是什么意思通过这篇博客，我们将确保通过并行化我们的代码来使用我们机器中所有可用的处理器。

由[吉菲](https://giphy.com/)

## 平行化申请

你会问，并行化有什么用？假设您有多个独立的任务，这些任务由一个“ *for 循环*执行在这里，并行化是超级英雄，它使每个独立的任务能够由不同的处理器处理，从而减少总时间。

例如，假设我在一个文件夹中保存了 1000 张图像，对于每张图像，我必须执行以下操作。

*   将图像转换为灰度
*   将灰度图像调整到给定的大小
*   将修改后的图像保存在文件夹中

现在，对每个图像执行此过程是相互独立的，即处理一个图像不会影响文件夹中的任何其他图像。因此多重处理可以帮助我们减少总时间。我们的总时间将减少一个因子，该因子等于我们并行使用的处理器的数量。这是使用并行化节省时间的众多例子之一。

回顾一下并行化的优势:

*   减少总时间
*   提高效率
*   最大限度减少可用资源的浪费
*   在等待代码运行时，在 YouTube 上观看更少的视频😛

## 并行化的类型

并行化可以通过两种方式实现:

*   多线程—使用进程/工作线程的多个线程。
*   多重处理——使用多个处理器(我们在上面的例子中讨论过的那个)。

多线程对于 I/O(输入/输出)绑定的应用程序非常有用。比如我要下载上传多个文件的时候，多线程会更有帮助。

多重处理对于 CPU 受限的应用程序很有用。您可以将上面的例子视为多处理的用例之一。

## 履行

例子已经够多了；让我们更深入地理解并行化是如何实现的。

让我们首先举一个简单的例子来说明多重处理是如何实现的。为了进行并行化，我们必须使用一个*多处理*库。在下面的例子中，我们将并行化计算平方的代码。

```
**from** **multiprocessing** **import** Pool**import os
def** f(x):
    **return** x*xworkers = os.cpu_count()**if** __name__ == '__main__':
    **with** Pool(workers) **as** p:
        print(p.map(f, [1, 2, 3]))
output: [1, 4, 9]
```

在上面的例子中，我们使用了“os.cpu_count()”来计算机器中可用的处理器数量。因此，如果处理器的数量为三个或更多，那么将并行计算所有三个数字的平方。如果处理器的数量是 2，那么将首先计算两个数字的平方，然后计算剩余数字的平方。让我们来看看我前面提到的图像处理示例的实现。

```
from multiprocessing import Pool
import cv2
import ospath = "./images"
save_path = "./images_save"
os.makedirs(save_path, exist_ok=True)def image_process(image_path):
    img = cv2.imread(os.path.join(path, image_path))
    gray = cv2.cvtColor(img, cv2.COLOR_BGR2GRAY)
    resized = cv2.resize(gray, (224, 224), interpolation = cv2.INTER_AREA)
    cv2.imwrite(os.path.join(save_path, image_path), resized)
    returndef main():
    list_image = os.listdir(path)
    workers = os.cpu_count()
    # number of processors used will be equal to workers
    with Pool(workers) as p:
        p.map(image_process, list_image)if __name__ == '__main__':
    main()
```

为了让您了解效率的提高，我在 4 核 CPU 上运行了上面的图像处理示例。使用并行化，完成任务需要 13 秒，而串行(没有并行化)需要 49 秒。想象一下，当您处理数百万张图像时，时间会减少多少。

现在，让我们看一个多线程的例子。对于 I/O 任务，如查询数据库或加载网页，CPU 只是在查询后等待答案。因此，多处理，即使用多个处理器，将会浪费资源，因为这些查询将会锁定所有的处理器。然而，多线程可以帮助我们减少为多个查询获取答案的时间，而不会浪费计算资源。为了实现多线程，我们将使用 *concurrent.futures* 库。

```
**import** **concurrent.futures**
**import** **urllib.request**

URLS = ['http://www.foxnews.com/',
        'http://www.cnn.com/',
        'http://europe.wsj.com/',
        'http://www.bbc.co.uk/',
        'http://some-made-up-domain.com/']

*# Retrieve a single page and report the URL and contents*
**def** load_url(url, timeout):
    **with** urllib.request.urlopen(url, timeout=timeout) **as** conn:
        **return** conn.read()

*# Use a with statement to ensure threads are cleaned up promptly*
**with** concurrent.futures.ThreadPoolExecutor(4) **as** executor:
    future_to_url = {executor.submit(load_url, url, 60): url **for** url **in** URLS}
    **for** future **in** concurrent.futures.as_completed(future_to_url):
        url = future_to_url[future]
        **try**:
            data = future.result()
        **except** Exception **as** exc:
            print('*%r* generated an exception: *%s*' % (url, exc))
        **else**:
            print('*%r* page is *%d* bytes' % (url, len(data)))
```

我希望上面的例子能帮助你更好地理解多重处理和多线程。

# 结论

因此，我们了解到并行化可以通过两种方式实现。多线程对于 I/O 绑定的进程很有用，多处理对于 CPU 绑定的进程很有用。我希望这篇文章能让您对并行化有一个很好的了解。如果你觉得有帮助，请在这个博客上发表评论，让我们知道。

关注我们的[媒体](https://medium.com/@AnveeNaik)，阅读更多此类内容。

*成为* [*介质会员*](https://medium.com/@AnveeNaik/membership) *解锁并阅读介质上的许多其他故事。*