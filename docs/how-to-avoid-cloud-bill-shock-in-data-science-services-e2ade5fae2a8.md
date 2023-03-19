# 如何避免数据科学服务中的云账单冲击

> 原文：<https://towardsdatascience.com/how-to-avoid-cloud-bill-shock-in-data-science-services-e2ade5fae2a8?source=collection_archive---------28----------------------->

## [行业笔记](https://pedram-ataee.medium.com/list/notes-from-industry-265207a5d024)

## 使用 ngrok 在不到 30 秒的时间内构建互联网服务器的方法

![](img/13490553e37ad017f6c82a0b57cda6a9.png)

由[雅各布·本特辛格](https://unsplash.com/@jacobbentzinger?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

故事开始于大约一年前，当时我构建了一个名为 [Owl](https://rapidapi.com/pedram.ataee/api/word-similarity/) 的单词相似度服务。我还创建了一系列 REST API 供社区使用这项服务。API 可在[这里](https://rapidapi.com/pedram.ataee/api/word-similarity/)获得。这是我的一个爱好项目。就我想让它对数据科学社区可用而言，我不想为在云基础架构上托管它支付太多费用。在本文中，我想与您分享如何在您的本地计算机上托管数据科学服务，并在不到 30 秒的时间内将其公开到互联网。我没有夸大其词。如果你想了解更多关于 OWL API 的知识，你可以阅读下面的文章。

[](/how-to-compute-word-similarity-a-comparative-analysis-e9d9d3cb3080) [## 如何计算词语相似度——比较分析

### Owl API 是一个强大的单词相似性服务，使用高级文本聚类技术和各种 word2vec 模型

towardsdatascience.com](/how-to-compute-word-similarity-a-comparative-analysis-e9d9d3cb3080) 

## —远离大牌。

首先，我在谷歌云平台上部署了这项服务。我在 GCP 的经历比 AWS 更好。对不起 AWS 爱好者，这是我的亲身经历。与 AWS 相比，GCP 的用户界面很友好，他们的文档也足够好。然而，GCP 不是为主持爱好项目而设计的。每月的账单来了，我发现在社区项目中使用 GCP 是没有意义的。

> 当涉及到一个爱好或者一个社区项目的时候，远离名人。他们每月的账单对你不利！

## —在您能够轻松处理成本之前，不要安定下来！

我决定把我的托管服务从 GCP 转到 OVH，一家提供各种网络服务的法国云计算公司。我使用 OVH 成功地将云成本削减了一半。然而，仍然有一些隐性成本让我感到惊讶，每月账单对我来说仍然没有意义。我爱上了我的单词相似度服务，我想继续努力让它变得越来越好。所以，我想让它在网上保持活力。问题是我怎样才能减少每月的账单？

> 如果你想让一个数据科学服务在互联网上长期存在，不要在你不能轻松管理的每月账单上结算！

## —将您的计算机视为一个机会！

我决定把我的电脑变成互联网服务器。然而，我不想花太多时间来配置东西。我做了一点研究，发现了一个名为“ [ngrok](https://ngrok.com/) ”的库。在他们的网站上，他们声称“ *Ngrok 通过安全隧道将防火墙后的本地服务器暴露给公共互联网。*“起初，我无法猜测我能快速轻松地做到这一点。但是，我决定试一试。

当您将本地计算机转变为互联网服务器时，您将无法获得与云基础架构相同的服务级别。您的本地计算机不时会重新启动，您的服务可能会在短时间内无法访问。然而，你无法相信使用这项技术可以节省多少钱。我最终将每月的账单从 100 多美元减少到了 10 美元！是不是很酷？

> 你无法相信使用 ngrok 可以节省多少钱。我把每月的托管费用减少了十分之一！

## —我如何使用 ngork 构建互联网服务器？

我用了不到 30 秒的时间和 3 个步骤就把我的电脑变成了互联网服务器。我没有夸大其词！让我们假设您有一个服务在`localhost`上启动并运行。您可以通过运行 docker 并在`port 80`上公开它来确保 web 服务在`localhost`可用。这部分我不想描述了。**现在的问题是如何通过 3 步把** `**localhost**` **暴露在公众互联网上。**这 3 个步骤简单如下:

1.  下载`ngrok.zip`和`unzip /path/to/ngrok.zip`。
2.  运行`./ngrok authtoken YOUR_OWN_TOKEN`
3.  运行`./ngrok http 80`

当你在他们的网站上注册时，他们会给你一个下载软件包的链接。注册他们的服务时，他们还会为您提供`YOUR_OWN_TOKEN`。运行`./ngrok http 80`之后，ngrok 服务在`ngrok.io`域上为您的项目创建一个专用的 URL，看起来像下面的链接。

```
[http://YOUR_DEDICATED_SUBDOMAIN.ngrok.io/](http://d9500fdc2530.ngrok.io/)
```

你的服务现在在互联网上对每个人都是可利用的。**是不是很神奇？**

## —最后的话

正如我所说的，这种服务不如专用服务器可靠，并且与特定的云解决方案相比不可扩展。然而，让一个数据科学项目长期存活下来是如此的方便和实惠。你知道机器学习引擎有很高的计算负担。将计算和托管的成本加在一起，你可以想象使用 ngrok 技术将你的电脑作为互联网服务器是多么经济！

```
**“Ngrok is an amazing technology!”**
```

# 感谢阅读！

如果你喜欢这个帖子，想支持我…

*   *跟我上* [*中*](https://medium.com/@pedram-ataee) *！*
*   *在* [*亚马逊*](https://www.amazon.com/Pedram-Ataee/e/B08D6J3WNW) *上查看我的书！*
*   *成为* [*中的一员*](https://pedram-ataee.medium.com/membership) *！*
*   *连接上*[*Linkedin*](https://www.linkedin.com/in/pedrama/)*！*
*   *关注我* [*推特*](https://twitter.com/pedram_ataee) *！*

[](https://pedram-ataee.medium.com/membership) [## 通过我的推荐链接加入 Medium—Pedram Ataee 博士

### 作为一个媒体会员，你的会员费的一部分会给你阅读的作家，你可以完全接触到每一个故事…

pedram-ataee.medium.com](https://pedram-ataee.medium.com/membership)