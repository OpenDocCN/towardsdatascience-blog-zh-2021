# 预测短生命周期产品的需求

> 原文：<https://towardsdatascience.com/forecasting-demand-for-short-life-cycle-products-8e26f563412e?source=collection_archive---------24----------------------->

## 你应该如何预测保质期短的产品？让我们回顾一下最佳实践。

最近在 LinkedIn 上收到以下问题。

> “我们每年在短期内(比如 20-40 天)销售一次某些产品。第二年，同样的升级产品或全新的产品将被出售。你对那些短生命周期的产品有什么建议吗？”

预测生命周期短的产品非常困难:你不能依赖任何历史需求。那么，我们该如何面对这个挑战呢？

这里有一些想法。

# 🛍️ **如果产品正在替换旧版本**

如果你每年发布一个新的产品版本，确保你的(产品生命周期)主数据是正确的。您将需要映射一个产品版本到另一个产品版本之间的链接(映射链接，如“2020 年的 A”->“2021 年的 B”)。然后，您将使用*清理后的*历史需求来创建预测基线(使用您常用的预测模型)。

如果您冒着新旧版本之间互相蚕食的风险，那么在做预测时要跟踪任何剩余的旧产品。我喜欢同时预测新旧版本的需求。然后由计划工具/软件/团队根据剩余库存(或通过应用分割因子)将预测分配给旧/新版本。

# 👜**如果产品是全新的**

一些新产品没有任何历史前辈，但仍然与其他当前产品相似。然后你可以依靠**历史类比**来预测新产品的需求。也就是说，根据观察到的其他类似项目的需求模式来预测新产品的需求。例如，要预测一种新的早餐谷物的上市，你可以基于一种相似类型的谷物的历史销售模式。

如果这个历史类比不起作用，你可以尝试其他技巧:

*   判断性预测(注意认知偏差！) [1]
*   在更高的层次结构层预测需求:您可以在较高的汇总层(如每个类别/系列/品牌)预测需求，然后将其传回 SKU。例如，您可以分析年度趋势来预测新产品的需求。
*   为了进一步改善你的预测，不要犹豫使用协作预测(与你的客户、供应商、一线员工讨论)。

# 📦**关注库存优化**

你应该考虑库存优化，而不是关注点预测。记住，预测只是达到目的的一种手段:它只帮助你购买/生产正确数量的商品。简而言之，重要的是在货架上获得正确的产品数量。没有得到最佳点估计预测。

你也可以和生产方合作，减少最小批量，提高灵活性等。这将有助于你轻松应对不断变化的需求。

## 📰报童模型

您可以使用报童模型来优化购买/生产的商品数量。为此，您需要:

*   概率预测。
*   对错过一笔销售的成本的估计(称为**超额成本， *Co*** )。这应该是盈利能力加上一些商誉损失。
*   赛季末剩下一个人的成本(称为**未成年成本， *Cu*** )。该成本应包括生产/采购成本减去折扣价。

产品的最佳服务水平应该是 Cu / (Cu + Co)。[2]

# 结论

与供应链一样，没有一种方法可以解决所有行业和上市模式的所有问题。历史上行之有效的方法在未来可能行不通(尤其是在出现 COVID 这样的中断时)。你需要了解历史上成功和失败的关键因素，以评估未来赛季的正确策略。记住，预测只是达到目的的一种手段:做出正确的供应链决策。了解你的极限，并据此制定计划。

# 来源

[1]范德普特，尼古拉(2021)。*供应链预测数据科学第二版*。柏林的胡家。

[2]尼古拉斯·范德普特(2020)。*库存优化:模型和模拟*。柏林的胡家。

## 👉[我们在 LinkedIn 上连线吧！](https://www.linkedin.com/in/vandeputnicolas/)

# 感谢

这篇文章是我在 LinkedIn [的一篇帖子](https://www.linkedin.com/posts/vandeputnicolas_demandplanning-forecasting-supplychainmanagement-activity-6736677455974690817-7ADk)的摘要。如果你对这样的辩论感兴趣，那就[连线吧！](https://www.linkedin.com/in/vandeputnicolas/)感谢以下人士在原讨论中的真知灼见: [Tatiana Usuga](https://www.linkedin.com/in/tatiana-usuga/) 、[Bruno vini cius gon alves](https://www.linkedin.com/in/brunovgoncalves/)、 [Chris Mousley](https://www.linkedin.com/in/cmousley/) 、 [Zachary MacLean](https://www.linkedin.com/in/zackmaclean/) 和 [Levent Ozsahin](https://www.linkedin.com/in/levent-ozsahin-96389510/) 。

# 关于作者

<https://www.linkedin.com/in/vandeputnicolas/>  

icolas Vandeput 是一名供应链数据科学家，擅长需求预测和库存优化。他在 2016 年创立了自己的咨询公司 [SupChains](http://www.supchains.com/) ，并在 2018 年共同创立了 [SKU 科学](https://bit.ly/3ozydFN)——一个快速、简单、实惠的需求预测平台。尼古拉斯对教育充满热情，他既是一个狂热的学习者，也喜欢在大学教学:自 2014 年以来，他一直在比利时布鲁塞尔为硕士学生教授预测和库存优化。自 2020 年以来，他还在法国巴黎的 CentraleSupelec 教授这两个科目。他于 2018 年出版了 [*供应链预测的数据科学*](https://www.amazon.com/Data-Science-Supply-Chain-Forecasting/dp/3110671107)(2021 年第 2 版)和 2020 年出版了 [*库存优化:模型与模拟*](https://www.amazon.com/Inventory-Optimization-Simulations-Nicolas-Vandeput/dp/3110673916) 。

![](img/c41de08f0cf4d975881aee57c407363c.png)