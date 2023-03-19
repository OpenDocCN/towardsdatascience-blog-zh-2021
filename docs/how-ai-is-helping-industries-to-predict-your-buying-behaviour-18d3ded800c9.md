# 人工智能如何帮助行业预测你的购买行为

> 原文：<https://towardsdatascience.com/how-ai-is-helping-industries-to-predict-your-buying-behaviour-18d3ded800c9?source=collection_archive---------44----------------------->

## 你的社交媒体数据就是他们的石油

![](img/5004c5e0bb2345e57c3def09ec7a5e75.png)

图片由 [**安德里亚·皮亚卡迪奥**](https://www.pexels.com/@olly?utm_content=attributionCopyText&utm_medium=referral&utm_source=pexels) 来自 [**像素**](https://www.pexels.com/photo/woman-holding-card-while-operating-silver-laptop-919436/?utm_content=attributionCopyText&utm_medium=referral&utm_source=pexels)

你有没有想过为什么像谷歌、脸书这样的大公司完全免费提供服务？所有这些公司都有如此庞大的用户群，即使他们收取最低的服务价格，他们每天也能创造数十亿美元的收入。

那么他们如何产生收入，他们用什么来获得这些钱呢？简单的答案是**你。**

谷歌和脸书等科技巨头将他们的用户作为数据点，收集他们在使用网站时分享的个人数据。你一定听说过*脸书因向剑桥分析公司这样的私人组织出售数百万用户的数据而上了新闻。*

在这篇文章中，我们将看到科技公司如何使用你在社交媒体上获得的数据来训练他们复杂的 ML 算法，以预测你的购买行为。

# **预测的整体模型是什么？**

谷歌、脸书、YouTube 等服务主要通过向观众展示广告来创收。为了提高广告的转换率，他们必须预测你的购买模式，只向你展示你感兴趣的产品的广告。

所以*公司使用最大似然算法、神经网络等。根据你在网站上分享的数据预测你的行为。*

例如，如果有人在谷歌或脸书上搜索智能手机，他们会得到更多与智能手机相关的广告。你也可以用你自己的系统尝试这个实验。

# **对预测的详细分析**

现在，让我们深入了解整个过程中发生的更具技术性的事情:

## **第一步:数据收集和处理**

首先，数据是从社交媒体网站的搜索引擎中收集的。一部分数据用于训练神经网络，其余数据用于测试神经网络。

下一步是**特征提取**。在此过程中，根据数据的属性(例如用户的地理位置、年龄组、搜索的产品类型、搜索次数等)，数据和相应的用户被分组并且*被标记为不同的类别。现在处理过的数据被用来训练神经网络。*

## **第二步:训练最大似然算法**

下一步是使用有组织的数据序列 ML 算法。主要有两种类型的算法可用于此:

*   **长短期记忆(LSTM):**

这是一种递归神经网络，其中上一步的*输出可用作当前步*的输入。LSTM 的主要优势在于它可以长时间保留数据，并可以从少量数据中获取大量信息。

*   **强化学习:**

使用这种算法，计算机可以*使用实时反馈*给出最优化的预测。例如，如果系统向观众显示更合适的广告，则系统对于下一次推荐的准确度增加。

## **步骤 3:使用预测显示广告**

现在，该算法根据系统使用的用户数据生成一些新信息，以显示用户想要查看的产品。反馈*机制*在这种情况下也起作用，其中**点击率**(广告点击次数与广告显示次数的比率)用于确定系统的效率并微调算法。

# 如果我不在社交媒体上怎么办？

来自佛蒙特大学的研究人员已经表明，算法已经变得如此复杂，一个人的行为甚至可以从这个人的朋友圈中预测出来，即使他不在社交媒体上。

在这种情况下，预测的含义是利用该人的朋友的近似地理区域和购买行为来实现的。所以逃网的机会只有一点点！

# **结论**

随着**行为经济学的发展，**数据安全问题一直是当局关注的问题。一些政府也像公司一样，积极利用用户数据。

那么，我们应该关心我们的数据是如何被使用的吗？

答案是**是的**。虽然你不能完全逃离网络，但你绝对可以监控这些公司如何使用你的数据。感谢政府的严格指导方针，你可以相信这些公司只会将你的数据用于开发目的。

# 参考

*   [新销售。简化。:必备手册](https://www.amazon.com/New-Sales-Simplified-Prospecting-Development/dp/0814431771)
*   [如何为销售成功定义销售流程](https://www.nasp.com/blog/how-to-define-a-sales-process-for-sales-success/)
*   [https://www . uvm . edu/news/story/study-Facebook-and-Twitter-your-privacy-risk-even-if-you-not-have-account](https://www.uvm.edu/news/story/study-facebook-and-twitter-your-privacy-risk-even-if-you-dont-have-account)

以下是我的一些最佳选择:

[https://towards data science . com/7-amazing-python-one-liners-you-must-know-413 AE 021470 f](/7-amazing-python-one-liners-you-must-know-413ae021470f)

[https://better programming . pub/10-python-tricks-that-wow-you-de 450921d 96 a](https://betterprogramming.pub/10-python-tricks-that-will-wow-you-de450921d96a)

[https://towards data science . com/5-data-science-projects-the-you-can-complete-over-the-weekend-34445 b 14707d](/5-data-science-projects-that-you-can-complete-over-the-weekend-34445b14707d)

觉得这个故事有趣？如果你想问我私人问题，请在 Linkedin 上联系我。如果你想直接通过邮件获得更多关于数据科学和技术的令人兴奋的文章，那么这里有我的免费简讯: [Pranjal 的简讯](https://mailchi.mp/4d33914bb328/pranjals-newsletter)。