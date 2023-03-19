# 清晰存储:存储面部图像保留政策的伦理

> 原文：<https://towardsdatascience.com/clear-storage-the-ethics-of-deletion-policies-for-stored-facial-images-a1748b0fe950?source=collection_archive---------18----------------------->

## 2021 年春天，华特·迪士尼世界[测试了面部识别软件](https://www.travelandleisure.com/travel-news/disney-world-facial-recognition-entry),旨在将客人的面部与他们的公园预订联系起来，引发了一场关于数据所有权的隐私辩论

![](img/fc69e3d6fb536737648508b69e8eb4ac.png)

由[马库斯·斯皮斯克](https://unsplash.com/@markusspiske?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)在 [Unsplash](https://unsplash.com/s/photos/data?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上拍摄的照片

虽然面部识别的话题令人不安，但听到它在一个与儿童般天真无邪相关的地方应用，对那些担心迪士尼公司侵犯他们隐私的人来说，尤其不和谐。应该指出的是，在迪士尼的案例中，对面部扫描仪感到不舒服的客人可以选择侵入性较小的门票扫描，并且没有关于该技术以非自愿方式实施的报告。

# 选择加入？

然而，并非所有公司都允许客户选择不收集面部数据。7 月，The Verge 报道称 [Lowe's、Macy's 和 Ace Hardware 目前都在使用面部识别算法，而麦当劳、沃尔格林甚至 7–11 都在考虑未来使用面部识别。虽然这听起来很可怕，但这种做法并不违法，因为面部识别技术在美国和世界大部分地区都不受监管。虽然面部识别的立法被忽视了(目前)，但有一个关于长期数据保留的更大的辩论正在激烈进行。当一起讨论时，很明显，动态或静态面部图像的长期存储对任何规模的组织都提出了道德和基础设施的困境。](https://www.theverge.com/2021/7/14/22576236/retail-stores-facial-recognition-civil-rights-organizations-ban)

> 数据常被比作石油；然而，与化石燃料不同，数据是一种可再生资源。它不需要被无限期地存储和访问才是无价的。

# 欧盟保留:被遗忘的权利

对于总部位于美国的公司来说，制定数据保护和保留政策目前是可选的，但加利福尼亚州除外，在加利福尼亚州，企业受加利福尼亚州消费者隐私法(CCPA)的约束，应用户的要求披露数据。对于在欧盟运营或影响欧盟用户的公司，数据透明是强制性的。《通用数据保护条例》( GDPR)没有规定公司被允许存储客户数据的时间期限。然而，它确实包括了具体的[数据保留指南](https://ico.org.uk/for-organisations/guide-to-data-protection/guide-to-the-general-data-protection-regulation-gdpr/principles/storage-limitation/)，并且是目前为止关于这个主题最全面的资源。几个亮点:

*   公司不得将个人数据保留超过他们需要的时间
*   公司必须能够证明他们为什么保存数据
*   公司需要建立保留期的政策
*   公司必须对他们如何使用数据进行定期审查
*   只有在符合公共利益的情况下，公司才能长期保留数据

最后，GDPR 向欧盟公民保证，他们对自己的数据拥有最终所有权，包括[被遗忘的权利](https://gdpr.eu/right-to-be-forgotten/):

> 如果您不再需要数据，个人有权擦除数据——一般数据保护法规(GDPR)

![](img/c3dfac5782d25e231e1737505749d6ac.png)

斯蒂芬·菲利普斯-Hostreviews.co.uk 在 [Unsplash](https://unsplash.com/s/photos/data?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上拍摄的照片

# 图像和隐私无法扩展

然而，值得注意的是，这些定律通常应用于结构化的、基于文本的数据，而不是非结构化的图像数据。就基础设施而言，图像文件通常比文本文件大几十倍或几百倍，使用谷歌或亚马逊等公司提供的基于云的数据库来存储图像文件，公司每月将花费更多的成本，特别是因为谷歌按千兆字节收取活动存储费用。最重要的是，虽然存储方法可以用于较小的数据库，但扩展数百万图像文件的存储可能是一个基础架构挑战。

基础设施和成本可以预测和调整；决定存储一些可以收集的最私人的数据的道德性，一个人的脸，在服务器上多年，涉及数据相关领域内外的那些人。十年前，医学成像领域的研究人员在云计算出现之前就面临这些问题。甚至在谷歌云产品等基于云的服务器广泛使用之前，这些人[就认识到了在将图像数据批量或流式传输到异地服务器](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC3056978/)之前存储和加密图像数据的挑战。

![](img/128e96f78f7f6e13137e8c7076b6afa2.png)

由[托拜厄斯·图利乌斯](https://unsplash.com/@tobiastu?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)在 [Unsplash](https://unsplash.com/s/photos/privacy?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 拍摄的照片

建立一套清晰的道德准则可以帮助人工智能开发者和商业领袖确定如何以平衡他们收集数据的权利和消费者隐私权的方式构思、测试和部署面部识别。非营利组织隐私论坛的未来建议，利用面部识别的组织必须关注隐私原则，如同意、数据安全和透明度。然而，对于这些数据的长期存储，却没有明确的指导方针。因此，出现了许多伦理难题，特别是在从弱势群体中收集面部数据方面。

例如，这里有一个假设:公司应该有权收集、利用和无限期存储从未成年人那里收集的面部图像数据吗？

> 在一个父母对在社交媒体上发布他们年幼孩子的照片持谨慎态度的时代，公司可以捕捉并永久存储他们的面部图像这一事实是一种令人担忧的边缘反乌托邦思想。

# 被遗忘的辩论

有时候，似乎唯一与数据科学相关的伦理争论是负责任的人工智能设计和使用。当谈到面部识别时，对模型偏差和数据集缺乏多样性的担忧是可以理解的，也是[认可的。坦率地说，这些都是相关的、有趣的讨论。诚然，数据保留并不是一个令人兴奋的话题。然而，随着越来越多的网站和应用程序兜售选择退出功能，技术公司和数据科学家将不得不应对的一个问题是:我们是否在道德上采购、处理和保护我们的数据？](https://sitn.hms.harvard.edu/flash/2020/racial-discrimination-in-face-recognition-technology/)

![](img/f414e202a9cf674af1fc0fedd8ebd7f9.png)

照片由[伯纳德·赫曼特](https://unsplash.com/@bernardhermant?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)在 [Unsplash](https://unsplash.com/s/photos/privacy?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上拍摄

慢慢地，我们开始看到公司，尤其是 FAANG 公司，解决这个问题。最近，谷歌更新了其 BigQuery 数据库产品[以包括数据到期参数，以阻止用户保留数据超过合理的时间范围](https://cloud.google.com/bigquery/docs/best-practices-storage)。谷歌本身正在带头努力改革数据保留政策。[2020 年，谷歌宣布将在 18 个月后开始自动删除位置历史、网页历史和语音记录](https://www.wired.com/story/google-auto-delete-data/)。谷歌在其新的数据保留政策中包括非结构化个人身份信息(PII)数据，如录音，这一事实应该为组织如何存储敏感的多媒体数据提供一些先例。

对于在数据行业工作的任何人，包括我自己，虽然我们没有任何希波克拉底誓言，但有一个隐含的责任是从道德上访问、操作和存储数据。限制存储能力或监管行业的想法不一定是可怕的想法。相反，对 PII 数据收集的监管，可以让企业建立、甚至在某些情况下重新赢得用户的信任，这些用户习惯于交出自己的信息，希望有人在后台考虑他们的最佳利益。