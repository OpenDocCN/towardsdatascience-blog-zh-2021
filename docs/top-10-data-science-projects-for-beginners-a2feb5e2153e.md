# 面向初学者的十大数据科学项目

> 原文：<https://towardsdatascience.com/top-10-data-science-projects-for-beginners-a2feb5e2153e?source=collection_archive---------1----------------------->

## 加强你的技能，建立一个突出的投资组合

![](img/fe36510a1dc4699cbcd7d72c97f7f312.png)

Jo Szczepanska 在 [Unsplash](https://unsplash.com/s/photos/project?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上拍摄的照片

作为一名有抱负的数据科学家，你一定听过“*做数据科学项目*”这个建议超过一千次。

数据科学项目不仅是一次很好的学习经历，还能帮助你从一群希望进入该领域的数据科学爱好者中脱颖而出。

## 然而，并不是所有的数据科学项目都有助于你的简历脱颖而出。事实上，在你的投资组合中列出错误的项目弊大于利。

在这篇文章中，我将带你浏览简历上的那些**必备**项目。

我还将为您提供**样本数据集**来试验每个项目，以及相关的**教程，帮助您完成项目**。

# 技能 1:数据收集

![](img/f18e907b4a0d2abc1995bbc736cd5870.png)

詹姆斯·哈里森在 [Unsplash](https://unsplash.com/s/photos/code?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上拍摄的照片

数据收集和预处理是数据科学家最重要的技能之一。

在我的数据科学工作中，我的大部分工作涉及到 Python 中的数据收集和清理。了解业务需求后，我们需要访问互联网上的相关数据。

这可以通过使用 API 或 web 抓取器来完成。一旦完成，数据需要被清理并存储到数据帧中，其格式可以作为输入输入到机器学习模型中。

这是数据科学家工作中最耗时的方面。

我建议通过完成以下项目来展示你在数据收集和预处理方面的技能:

## 网络搜集——食品评论网站

教程:[使用 BeautifulSoup 的 Zomato Web Scraping](https://datascienceplus.com/zomato-web-scraping-with-beautifulsoup-in-python/)

语言:Python

从食品配送网站上搜集评论是一个有趣且实用的项目，可以写进你的简历。

只需构建一个 web scraper，从该网站的所有网页中收集所有评论信息，并将其存储在一个数据框中。

如果你想让这个项目更进一步，你可以使用收集的数据建立一个情感分析模型，并对这些评论中的哪些是积极的，哪些是消极的进行分类。

下一次你想找点吃的，挑一家评论总体情绪最好的餐馆。

## 网络搜集—在线课程网站

教程:[用 Python 在 8 分钟内搭建一个 Web 刮刀](/scrape-websites-using-python-in-5-minutes-931cd9f44443)

语言:Python

想找到 2021 年最好的在线课程吗？很难在数百门数据科学课程中找到一门价格合理但评价很高的课程。

您可以通过抓取在线课程网站并将所有结果存储到数据框中来实现这一点。

更进一步，你还可以围绕价格和评级等变量创建可视化，以找到一个既负担得起又质量好的球场。

你也可以创建一个情感分析模型，得出每个在线课程的总体情感。然后你可以选择总体情绪最高的课程。

## 奖金

创建一些项目，使用 API 或其他外部工具收集数据。这些技能通常会在你开始工作时派上用场。

大多数依赖第三方数据的公司通常会购买 API access，您需要借助这些外部工具来收集数据。

您可以做一个示例项目:使用 Twitter API 收集与特定 hashtag 相关的数据，并将数据存储在数据框中。

# 技能 2:探索性数据分析

![](img/400bf6882d5a6146059be8375a05d185.png)

由[卢克·切瑟](https://unsplash.com/@lukechesser?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)在 [Unsplash](https://unsplash.com/s/photos/data?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上拍摄的照片

收集和存储数据后，您需要对数据框中的所有变量进行分析。

你需要观察每个变量是如何分布的，并了解它们之间的关系。你还必须能够在可用数据的帮助下回答问题。

作为一名数据科学家，这是你经常要做的工作，甚至可能比预测建模还要多。

以下是一些 EDA 项目的想法:

## 识别心脏病的危险因素

数据集:[弗雷明汉心脏研究](https://biolincc.nhlbi.nih.gov/studies/framcohort/)

教程:[弗雷明汉心脏研究:决策树](https://medium.com/swlh/the-framingham-heart-study-decision-trees-83a7fb62718e)

语言:Python 或 R

该数据集包括用于预测患者心脏病发作的预测因子，如胆固醇、年龄、糖尿病和家族史。

您可以使用 Python 或 R 来分析该数据集中存在的关系，并得出以下问题的答案:

*   糖尿病患者在年轻时更容易患心脏病吗？
*   有没有某个人口统计群体比其他人患心脏病的风险更高？
*   经常锻炼会降低患心脏病的风险吗？
*   吸烟者比不吸烟者更容易患心脏病吗？

能够在可用数据的帮助下回答这些问题是数据科学家必须具备的一项重要技能。

这个项目不仅有助于加强你作为分析师的技能，还将展示你从大型数据集获得洞察力的能力。

## 世界幸福报告

数据集:[世界幸福报告](https://www.kaggle.com/unsdsn/world-happiness)

教程:[世界幸福报告 EDA](https://www.kaggle.com/rushilpatel2000/world-happiness-report-eda)

语言:Python

《世界幸福报告》追踪了衡量全球幸福的六个因素——预期寿命、经济状况、社会支持、廉洁、自由和慷慨。

对该数据集执行分析时，您可以回答以下问题:

*   世界上哪个国家最幸福？
*   一个国家幸福的最重要因素是什么？
*   总体幸福感是增加了还是减少了？

再说一遍，这个项目将有助于提高你作为分析师的技能。我在大多数成功的数据分析师身上看到的一个特质是好奇心。

数据科学家和分析师总是在寻找促成因素。

他们总是在寻找变量之间的关系，并不断提出问题。

如果你是一名有抱负的数据科学家，做这样的项目将有助于你培养分析思维。

# 技能 3:数据可视化

![](img/ab57e1afde9a261b36e0dc45a5304d93.png)

由[卢卡斯·布拉塞克](https://unsplash.com/@goumbik?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)在 [Unsplash](https://unsplash.com/s/photos/data?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上拍摄

当你开始作为一名数据科学家工作时，你的客户和利益相关者通常是非技术人员。

您将需要分解您的见解，并向非技术观众展示您的发现。

最好的方法是以可视化的形式。

展示一个交互式仪表板将有助于你更好地传达你的见解，因为图表乍一看很容易理解。

正因如此，许多公司将数据可视化列为数据科学相关职位的必备**技能。**

以下是一些你可以在作品集上展示的项目，以展示你的数据可视化技能:

## 构建新冠肺炎仪表板

数据集:[约翰·霍普金斯大学的新冠肺炎数据仓库](https://github.com/CSSEGISandData/COVID-19)

教程:[用 Python 和 Tableau 搭建新冠肺炎仪表盘](/building-covid-19-dashboard-with-python-and-tableau-296b0016920f)

语言:Python

您首先需要使用 Python 预处理上面的数据集。然后，您可以使用 Tableau 创建一个交互式新冠肺炎仪表板。

Tableau 是最受欢迎的数据可视化工具之一，是大多数入门级数据科学职位的先决条件。

使用 Tableau 构建仪表板并在您的投资组合中展示它将有助于您脱颖而出，因为它展示了您使用该工具的熟练程度。

## 构建 IMD b-电影数据集仪表板

数据集: [IMDb 顶级电影](https://www.imdb.com/chart/top)

教程:[用 Tableau 探索 IMDb 250 强](https://www.edupristine.com/blog/exploring-internet-movie-database-in-tableau)

您可以使用 IMDb 数据集进行实验，并使用 Tableau 创建交互式电影仪表盘。

正如我上面提到的，展示你制作的 Tableau 仪表盘可以帮助你的投资组合脱颖而出。

Tableau 的另一个优点是，你可以将你的可视化上传到 Tableau Public，并与任何想使用你的仪表板的人分享链接。

这意味着潜在雇主可以与你的仪表板互动，从而激发兴趣。一旦他们对你的项目感兴趣，并能真正地摆弄最终产品，你就离得到这份工作更近了一步。

如果你想入门 Tableau，可以在这里访问我的教程[。](https://medium.datadriveninvestor.com/tableau-tutorial-for-beginners-43483adf719)

# 技能 4:机器学习

![](img/f38edb1eeefde644cf4faab960b558ca.png)

凯文·Ku 在 [Unsplash](https://unsplash.com/s/photos/machine-learning?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上拍摄的照片

最后，你需要展示展示你在机器学习方面的熟练程度的项目。

我建议两者都做——有监督的和无监督的机器学习项目。

## 食品评论的情感分析

数据集:[亚马逊美食评论数据集](https://www.kaggle.com/snap/amazon-fine-food-reviews?select=Reviews.csv)

教程:[Python 情感分析初学者指南](/a-beginners-guide-to-sentiment-analysis-in-python-95e354ea84f6)

语言:Python

情感分析是机器学习的一个非常重要的方面。企业经常用它来衡量顾客对其产品的总体反应。

客户通常会在社交媒体和客户反馈论坛上谈论产品。可以收集和分析这些数据，以了解不同的人如何对不同的营销策略做出反应。

基于所进行的情感分析，公司可以对他们的产品进行不同的定位或者改变他们的目标受众。

我建议在你的投资组合中展示一个情绪分析项目，因为几乎所有的企业都有社交媒体存在，并且需要评估客户反馈。

## 预期寿命预测

数据集:[预期寿命数据集](https://www.kaggle.com/kumarajarshi/life-expectancy-who?ref=hackernoon.com)

教程:[寿命回归](https://www.kaggle.com/wrecked22/life-expectancy-regression)

语言:Python

在本项目中，您将根据教育程度、婴儿死亡人数、饮酒量和成人死亡率等变量来预测一个人的预期寿命。

我上面列出的情感分析项目是一个分类问题，这就是为什么我要在列表中添加一个回归问题。

在你的简历上展示各种各样的项目来展示你在不同领域的专长是很重要的。

## 乳腺癌分析

数据集:[乳腺癌数据集](https://www.kaggle.com/vishwaparekh/cluster-analysis-of-breast-cancer-dataset?select=data.csv)

教程:[乳腺癌数据集的聚类分析](https://www.kaggle.com/vishwaparekh/cluster-analysis-of-breast-cancer-dataset)

语言:Python

在本项目中，您将使用 K-means 聚类算法根据目标属性检测乳腺癌的存在。

K-means 聚类是一种无监督学习技术。

在你的投资组合中有聚类项目是很重要的，因为大多数真实世界的数据是没有标记的。

即使是公司收集的海量数据集，通常也没有训练标签。作为一名数据科学家，你可能需要使用无监督学习技术自己做标记。

# 结论

你需要展示展示各种技能的项目——包括数据收集、分析、可视化和机器学习。

在线课程不足以让你获得所有这些领域的技能。然而，你可以找到几乎所有你想做的项目的教程。

你所需要的只是 Python 的基础知识，你将能够跟随这些教程。

一旦你让所有的代码都工作起来，并且能够正确地遵循，你就可以复制这个解决方案，并且自己处理各种不同的项目。

请记住，如果你是数据科学领域的初学者，并且没有该学科的学位或硕士学位，那么展示项目组合非常重要。

组合项目是向潜在雇主展示你技能的最佳方式之一，尤其是在这个领域获得你的第一份入门级工作。

点击这里，了解我如何获得我的第一份数据科学实习[。](/data-science-projects-that-will-get-you-the-job-805065e7260)

> 迟早，胜利者是那些认为自己能行的人——保罗·图尼尔