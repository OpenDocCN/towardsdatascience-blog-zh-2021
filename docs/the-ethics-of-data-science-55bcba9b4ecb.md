# 数据科学的伦理

> 原文：<https://towardsdatascience.com/the-ethics-of-data-science-55bcba9b4ecb?source=collection_archive---------16----------------------->

## 大数据:合理使用还是潜在侵犯隐私？

![](img/6db2553e1f76998da74326b5f45c63ef.png)

照片由[弗兰克](https://unsplash.com/@franckinjapan?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

当想到数据科学时，您首先会想到的机制之一是大数据。但是，当你更多地考虑数据时，你可能会考虑存储的是什么类型的数据:全名、电子邮件地址、电话号码，甚至是某人的家庭地址。根据数据的具体程度，你甚至可以知道性别、年龄、职业等等。起初，我忽略了这种数据收集，以改善预测分析，甚至有针对性的营销。

我本周对数据科学的趋势搜索提出了数据收集和使用中涉及的道德问题。正如我说过的，有些人认为它被用来提高机器学习做出的智能猜测。然而，其他人认为存储的数据类型可能会侵犯隐私。所以，这是我想从两个角度看整个讨论的地方。首先，我想听听可能存在的潜在担忧。其次，我希望看到数据科学家试图解决这些问题的要点。

因此，在今天的文章中，我们将首先讨论使用或收集数据时的一些隐私问题，然后我们将讨论当您作为数据科学家在您的旅程中收集数据时保护他人隐私的几种方法。

# **用户同意**

首要的隐私问题之一是用户是否同意获取数据。回想一下，第一部分将从用户的角度出发。例如，当你在一个网站上时，你的点击或购买可以被跟踪。或者你正在使用一个流行的应用程序，它利用你的浏览器历史来为你选择应用程序。尤其是在谈论敏感数据时，可能具体到信用卡号，也可能笼统到名字，您会想知道正在跟踪什么。

此外，您甚至可能想知道数据的用途。归根结底，这是你的数据和隐私，所以你会想知道什么信息被拿走了，为什么。

但是一些网站使得这些信息很难找到。如果网站提示您接受或拒绝 cookies，您通常需要单击另一个链接来查看正在跟踪的内容。它可能会让您选择决定哪些 cookies 可以跟踪，但浏览列表可能会花费更多的时间。

# **用户同意:解决方案**

从数据科学家的角度来看，您已经知道您将跟踪什么信息以及获取数据背后的目的。但是你也需要获得用户的同意。对于一个网站，甚至一个应用程序，这可能很容易做到。“条款和条件”页面列出了你在网站/应用上执行的规则，但通过提供“接受 cookies”，用户可以选择是否允许你跟踪数据。有了这些 cookies 或者你的“服务条款”，你应该清楚地说明你到底追踪的是什么或者是什么意图。

# **用户同意:困境**

你正在使用的应用程序或网站写了冗长的“服务条款”，详细说明他们跟踪的所有内容和你同意的内容，并详细说明在你接受这些内容之前 cookies 中跟踪了哪些信息。但是，作为用户，你也懒得看吗？尤其是当内容冗长时，你是否至少浏览了一下他们可能会跟踪的信息，或者你是否节省了自己的时间并接受了？不通读的不止你一个。许多用户没有时间或耐心阅读关于每一项的冗长、详细的报告。

这种安排的困境是，你可以添加所有的信息供用户阅读，但他们可能只是点击“接受 cookies”或“接受服务条款”，而从来没有真正阅读条款。那么，你的用户同意了吗？是的，也许是这样。但是，作为用户，这不是知情同意。

# **对被盗数据的隐私担忧**

当您跟踪大量数据时，必须有一些地方来存储这些数据。通常，这是一个大型数据库。但是如果有人黑了那个数据库会怎么样呢？总会有隐私问题，即使有最好的安全团队，您仍可能有违规行为。

每隔一段时间，一家著名的航空公司会警告其用户，他们的数据库被黑客攻击，信用卡信息可能被盗。作为用户，你不得不担心你的信息被卖给错误的人，或者错误的人找到访问你的数据的方法。

# **避免数据失窃**

作为一名数据科学家或任何从事安全甚至开发工作的人，您的目标应该是尽可能保证数据的安全。但是，事情有时确实会发生，所以尽快恢复并在需要的地方进行损害控制是你的工作。

虽然安全是解决办法，但它并不总是最简单的解决方案。但是，如果你打算获取用户数据，你必须确保像保护自己的数据一样保护它。

# **公平和算法的伦理**

当一台机器在做决策时，你必须给它一些可供学习的数据。然而，在样本集里，不管有多大，你可能会发现自己偏爱某些组。例如，您的测试集可能包含不符合总体的人口统计数据。如果你检查一个街区，你可能会，或者你的机器学习模型可能会对人口做出不真实的假设。这可能会产生有偏见的算法。

我读过的一篇文章中有一个有偏算法的例子。在 Kylie Ying 的《数据科学伦理——什么可能出错以及如何避免出错》中，Staples 试图通过提供更便宜的价格和更好的交易来击败竞争对手。然而，根据他们的算法，这些更好的价格是在更富裕的社区中发现的，这意味着更富裕的社区将比一般的贫困社区获得更低的价格。

但是公平不仅仅是关于算法或者人口。公平也可以指数据的使用方式。道德包括如何使用或出售数据。例如，获取某人的敏感数据并将其出售给不可信方是不道德的。然而，向以销售为目标的公司出售通用的基于兴趣的数据不一定是不道德的。

根据数据集的不同，算法也可能遵循不同的假设。例如，在同一篇文章中，Kylie Ying 谈到她有一个关于算法的有争议作用的视频，该算法的任务是确定量刑和假释。在算法中，有一个反对黑人被告和它是如何有利于白人被告。这显示了算法中的种族差异。

# **让数据收集世界变得更美好**

正如你所看到的，数据科学，即机器学习，更具体地说是数据收集，有一些灰色区域是不道德的。然而，有更多合乎道德的方法来处理它。第一是在收集数据时保持透明。这样，您的用户不仅知道您在收集什么数据，还知道您打算如何使用这些数据。接下来，保护隐私。尽可能保护好数据。就好像这是你的数据而不是用户的一样。在这两者中，你将需要小心地收集。考虑你需要什么数据，至于不要拿太多。考虑数据的上下文，因为它经常改变含义。最后，当然，融入包容性。这样，您的算法将找到尽可能小的偏差，因此您的数据也可以反映总体，而不仅仅是样本。

# **结论**

今天，我们探讨了数据科学中的伦理世界。我们了解了一些不同的问题，包括透明度(知情同意)、安全性、差异(无论是种族还是性别)，以及确保您的算法不包含偏见。在研究算法的同时，我们还研究了两个例子，它们是如何基于您的数据集而变得有偏差的。

虽然数据科学，特别是数据收集和机器学习，本质上并非不道德，但在深入研究之前，仍然有几个实践是你应该知道的。我当然觉得我学到了更多关于数据科学的道德规范，以及为什么会有更好的可见性。希望你也学到了一些东西，也希望你觉得这篇文章读起来很有趣。下次见，干杯！

***用我的*** [***每周简讯***](https://crafty-leader-2062.ck.page/8f8bcfb181) ***免费阅读我的所有文章，谢谢！***

***想阅读介质上的所有文章？成为中等*** [***成员***](https://miketechgame.medium.com/membership) ***今天！***

查看我最近的一些文章

[](/all-about-deepfakes-e481a55cf7e5) [## 关于 Deepfakes 的一切

### 什么是 deepfake，它在当今世界有多“真实”？

towardsdatascience.com](/all-about-deepfakes-e481a55cf7e5) [](https://medium.com/codex/quitting-kubernetes-kubeadm-and-switching-to-microk8s-b487e2abd482) [## 退出 Kubernetes Kubeadm，转投 MicroK8s

### 为不易出错的群集让路…

medium.com](https://medium.com/codex/quitting-kubernetes-kubeadm-and-switching-to-microk8s-b487e2abd482) [](https://miketechgame.medium.com/leaving-your-job-for-selfish-reasons-af1060ea51bf) [## 因为“自私”的原因离职

### 有什么“不好的”理由让你为了新的工作而辞职吗？

miketechgame.medium.com](https://miketechgame.medium.com/leaving-your-job-for-selfish-reasons-af1060ea51bf) [](https://python.plainenglish.io/video-editing-with-python-73d419ba93ae) [## 如何用 Python 编辑视频

### 通过使用 MoviePy Python 模块加速回放来创建延时的指南。

python .平原英语. io](https://python.plainenglish.io/video-editing-with-python-73d419ba93ae) [](https://medium.com/codex/kubernetes-vs-docker-swarm-d209e7cd58d7) [## Kubernetes vs Docker Swarm

### 评估容器编排平台…

medium.com](https://medium.com/codex/kubernetes-vs-docker-swarm-d209e7cd58d7) 

参考资料:

[](https://datascienceethics.com/data-science-ethics-in-practice/#:~:text=Data%20Science%20Ethics%20in%20Practice%201%20Protect%20Privacy.,7%20Consider%20Context.%20...%208%20Encode%20Equity.%20) [## 实践中的数据科学伦理-

### 数据科学道德最佳实践的目的是成为一个框架，我们可以在其中运行，而不是一个…

datascienceethics.com](https://datascienceethics.com/data-science-ethics-in-practice/#:~:text=Data%20Science%20Ethics%20in%20Practice%201%20Protect%20Privacy.,7%20Consider%20Context.%20...%208%20Encode%20Equity.%20) [](https://www.freecodecamp.org/news/the-ethics-of-data-science/) [## 数据科学伦理——什么可能出错以及如何避免出错

### 数据科学模型就在你身边。它们可能会影响你的入学，不管你是被录用(还是被解雇)…

www.freecodecamp.org](https://www.freecodecamp.org/news/the-ethics-of-data-science/) [](https://datascience.stanford.edu/research/research-areas/ethics-and-data-science) [## 伦理和数据科学

### 新技术经常引发新的道德问题。例如，核武器的出现带来了巨大的压力…

datascience.stanford.edu](https://datascience.stanford.edu/research/research-areas/ethics-and-data-science) [](https://www.karmelsoft.com/ethical-challenges-of-big-data/) [## 大数据的伦理挑战

### 随着数据技术的进步，大数据的伦理挑战增加了，并提出了社会从未有过的问题…

www.karmelsoft.com](https://www.karmelsoft.com/ethical-challenges-of-big-data/)