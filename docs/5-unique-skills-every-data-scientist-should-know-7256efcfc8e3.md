# 每个数据科学家都应该知道的 5 项独特技能

> 原文：<https://towardsdatascience.com/5-unique-skills-every-data-scientist-should-know-7256efcfc8e3?source=collection_archive---------25----------------------->

## 意见

## 沟通是关键…还有其他商业技巧和建议

![](img/196b73ba610e643cedf1d254720835a9.png)

凯利·西克玛在[Unsplash](https://unsplash.com/s/photos/sticky-note?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)【1】上的照片。

# 目录

1.  介绍
2.  为利益相关方减少 DS 术语
3.  不要过度承诺
4.  与软件工程师建立关系
5.  主 SQL 优化
6.  *Git* 用 Git
7.  摘要
8.  参考

# 介绍

作为数据科学家或未来的数据科学家，我们可能会看到一些同样的技能被认为是重要的，这是事实；然而，我想提出五项独特的技能和/或建议，希望你能从这些例子中受益，并在你的职业生涯中应用它们。下面的技能将包括与利益相关者合作，以及一些编程技巧和建议。如果您想了解关于这五种独特的数据科学技能的更多信息，请继续阅读。

# 为利益相关者减少数据科学术语

![](img/eb73f869c014bceccd1fa198770530d2.png)

照片由[萨米·威廉姆斯](https://unsplash.com/@sammywilliams?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)在[Unsplash](https://unsplash.com/s/photos/confused?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)【2】上拍摄。

如果你决定受雇于一家数据科学更面向客户的公司，这项技能非常重要。顾客主要有两种意思。第一种是外部类型，包括数据科学客户端。第二类是公司利益相关者，根据你的具体工作，你可能会与他们有更多的互动。他们通常由产品经理、业务分析师甚至其他软件工程师组成。你必须向这些人解释你复杂的数据科学模型，让他们明白易懂。

为了在一家公司启动一个数据科学项目，您需要减少数据科学专用术语的数量，以便与您合作的人能够理解您所采用的概念和方法。

> 下面是一个向产品经理简化行话的例子:

*   **坏**—“*我们需要使用 XGBoost 机器学习算法，为我们的最终用户将均方根误差降低 12.68%*”

在这种情况下，产品经理可能不理解 XGBoost 是什么意思(*除非你进一步解释它*)，但这个想法仍然不是向你的利益相关者教授数据科学(*大多数人不知道 RMSE，即使解释了，理解起来仍然很棘手——即使对数据科学家来说*)，而是总结模型对业务的影响。

*   **好的**—“*我们将使用一种新的算法，这种算法有几个好处，最主要的是它将减少我们的预测误差，这将在下个季度节省 X 笔钱*

这个例子要好得多，因为算法的名称对于涉众来说通常并不重要(*除非是一个软件工程师在努力部署你的模型*)。这个例子也强调了模型对业务的影响。虽然错误指标可能有用，但通常它们最终会被转化为业务已经习惯理解的 KPI ( *关键性能指标*)指标。

# 不要过度承诺

![](img/e93087e81d71b3368a7b2d376f33db19.png)

Andrew Petrov 在[Unsplash](https://unsplash.com/s/photos/promise?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)【3】上拍摄的照片。

作为数据科学家，我们可能会为我们的开发准确率达到 96%而感到兴奋，并想在屋顶上大声喊出来。虽然这是一个很好的结果，但我们希望确保该结果能够反映生产中实际可用的数据，以及我们最关心的特定数据组。也许对于我们更关心数据的组，准确率接近 92%,像边缘情况，准确率更重要。

> 以下是第一次向利益相关者展示结果时需要考虑的一些事情:

*   测试周期是多长？
*   我们期望不同的测试周期具有相同或不同的精度/误差吗？
*   这些数据在生产环境中可用吗？
*   我们的训练规模在生产中可以一样吗？
*   我们真的能在生产中如此频繁地预测吗？
*   这种型号的成本与节省成本相比是多少？
*   我们能经常训练这种模型吗？
*   当我们的预测缺少数据时会发生什么？
*   当异常事件导致特定时间段或组的准确性显著下降时，会发生什么？
*   测试，测试，测试！确保这些预测是稳定的。

正如你所看到的，当把结果传达给利益相关者时，有相当多的事情需要考虑，以上只是其中的一些考虑。

# 与软件工程师建立关系

![](img/d3af0167ab34cb0d26dbd398e90248f6.png)

图片由[黑客资本](https://unsplash.com/@hackcapital?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)在[Unsplash](https://unsplash.com/s/photos/software?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)【4】上拍摄。

这项技能可能更多的是一种建议，但对数据科学家来说仍然非常重要。当然，如果你的背景不是软件工程，这可能是更值得考虑的事情。例如，许多数据科学家不是来自软件工程或编码/编程背景，而是具有金融、地球科学、统计或数学背景。这些领域在某些方面是有益的，但数据科学家往往会在围绕机器学习算法和数据科学模型部署的复杂编码过程中遇到困难。如果这是你，那么明智的做法可能是找一个你觉得合适的人来讨论如何创建更高效的代码，或者找一个可以批准你的拉请求的人，而不是另一个数据科学家。

> 以下是与软件工程师一起工作的一些好处:

*   更好的理解面向对象编程( *OOP* )。
*   可以更快地部署模型。
*   可以更快地与业务整合。
*   如果需要的话，可以让另一个人仔细检查你的代码——第二双眼睛。

记住这一点很重要，是的，数据科学的一些角色涉及所有过程，包括所有涉及的软件工程或编程，因此这项技能或技巧对于以前只专注于在本地构建模型的数据科学家来说更是如此。

# 主 SQL 优化

![](img/1e633551f96d5f3b097a4c5ced6b57f3.png)

卡斯帕·卡米尔·鲁宾在[Unsplash](https://unsplash.com/s/photos/sql?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)【5】上的照片。

通常，就像 Python 等典型语言中的一般编程一样，数据科学家可能会努力优化 SQL 查询。这个技巧是理解在哪里和什么应用到你的查询，以便它可以更快地运行(*和正确地*)，这可以使测试模型更快(*例如—* *如果你经常更新你的查询)*并且使生产中的训练更有效。

> 以下是 SQL 优化技术的一些示例:

*   用日期过滤
*   使用分类数据过滤
*   删除排序
*   执行内部联接

当然，还有更多优化查询的方法，但是上面是一些可以显著提高查询运行时间的简单方法。

# *Git* 用 Git

![](img/55bd50fada12f87c871fbdd5466b54f2.png)

在[un splash](https://unsplash.com/s/photos/github?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)【6】上由 [Yancy Min](https://unsplash.com/@yancymin?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 拍摄的照片。

当我们在学术环境中学习数据科学时，有时 Git 和/或 GitHub 实践会被遗忘。有许多常见的命令需要熟悉，这样您就可以快速更新您的模型代码，还有一些更独特的命令将在下面介绍。下面的链接是一个很好的例子，里面有很多不同类型的 git 命令，比如撤销更改、重写 git 历史、Git 分支和远程存储库等等[7]:

<https://www.atlassian.com/git/tutorials/atlassian-git-cheatsheet>  

> 以下是一些独特的 Git 命令:

```
* git status* git diff* git branch* git commit* git fetch* git remote add* git reset — hard* git rebase -i <base>
```

# 摘要

上面提到的技巧和建议是我从自己身上受益的，希望你也能受益。我们已经讨论了如何与公司中的其他利益相关者合作，以及如何创建更高效的代码。总而言之，这些只是每个数据科学家都应该知道的一些技能，对你来说可能是独一无二的和新的。

> 作为总结，以下是每个数据科学家都应该知道的五大技能:

```
* Reducing DS Jargon for Stakeholders* Do Not Overpromise* Build a Relationship with a Software Engineer* Master SQL Optimization** Git* with Git
```

感谢您的阅读！如果您有任何问题，请联系我，如果您有任何经验，同意或不同意上面看到的技能，请在下面评论。你还能想到哪些每个数据科学家都应该知道的技能或建议？

*我不隶属于上述任何一家公司。*

# 参考

[1]Kelly sik kema 在 [Unsplash](https://unsplash.com/s/photos/sticky-note?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上拍摄的照片，(2021)

[2]Sammy Williams 在 [Unsplash](https://unsplash.com/s/photos/confused?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上拍摄的照片，(2021)

[3]安德鲁·彼得罗夫在 [Unsplash](https://unsplash.com/s/photos/promise?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上拍摄的照片，(2021)

[4]图片由[黑客之都](https://unsplash.com/@hackcapital?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)在[Unsplash](https://unsplash.com/s/photos/software?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)(2018)拍摄

[5]照片由[卡斯帕·卡米尔·鲁宾](https://unsplash.com/@casparrubin?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)在[Unsplash](https://unsplash.com/s/photos/sql?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)(2017)上拍摄

[6]照片由 [Yancy Min](https://unsplash.com/@yancymin?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 在[Unsplash](https://unsplash.com/s/photos/github?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)(2019)上拍摄

[7] Atlassian， [Git 备忘单](https://www.atlassian.com/git/tutorials/atlassian-git-cheatsheet)，(2021)