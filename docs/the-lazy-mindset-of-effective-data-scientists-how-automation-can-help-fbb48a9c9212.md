# 高效数据科学家的懒惰心态:自动化如何有所帮助

> 原文：<https://towardsdatascience.com/the-lazy-mindset-of-effective-data-scientists-how-automation-can-help-fbb48a9c9212?source=collection_archive---------48----------------------->

## 在数据科学中利用 CI/CD 和自动化

![](img/8b8f90b1cdd58fadca9d9ce4a2c18f3b.png)

照片来自 [Unsplash](https://unsplash.com/) 上的[普里西拉·杜·普里兹](https://unsplash.com/@priscilladupreez)

让代码和自动化为你工作。如果您经常发现自己一遍又一遍地编写类似的代码，那么是时候考虑自动化任务或分析了。也许你正在开始一夜之间运行类似的分析，或者在每次运行时用不同的输入参数比较模型结果，但是你在有效地完成这些任务吗？我通常希望在我的日常工作中改进两个方面:(1)使用 CI/CD 的标准代码检查，以及(2)使用自动化作业执行分析。

在数据科学领域工作时，我发现我经常在许多不同的笔记本上编写代码，并与团队中不同的数据科学家共享。如果我正在做的任务开始变得重复，或者其他人做一些非常相似的事情，那么我就后退一步，看看这个过程。我看看能不能利用 CI/CD 管道、脚本或自动化作业来帮助工作，这样我就可以做到事半功倍。

# 用于代码检查的 CI/CD 管道

CI/CD 管道是我在工作流程中经常使用的东西。您可以设计这些管道来执行日常任务，并与数据科学家共享结果。除了单元测试之外，您还可以使用 CI/CD 管道。您可以从中开始工作，在分析变更期间执行性能指标，开始库的部署，等等。

作为一名开发人员和数据科学家，我寻找方法来简化我的团队的流程，并使这些标准检查更容易运行。当我打开一个 pull 请求时，管道开始运行验证、例行检查等等，以验证代码是否按预期运行并产生正确的结果。这些管道加速了验证代码变更和分析变更有效性的过程。因此，当在同行评审中检查分析概念时，主要检查已经自动完成。剩下我要做的就是检查代码中的逻辑错误、低效和文档。

在考虑新管道时，我经常问自己的问题是:

```
- Can the task be run in less than 6 hours, which is our pipeline's requirement? - Can the result of the pipeline be easily shared during a pull request? How will the outcome be used to validate the code during a pull request? - Does the task aid in deciding the pull request or release of the artifacts after the pull request is merged? Such as deploying documentation, a website, or a library. - Do you have common artifacts that you need to release during or after a pull request or merge? How can you automate those releases using pipelines?
```

# 常见任务的作业自动化

如果我不创建一个管道来帮助我的任务自动化，我会在创建和自动化标准笔记本电脑和工作中寻找效率。我每小时、每晚或每周为我的团队运行许多作业，并且只在我需要更新或者某个作业警告我失败时才检查代码。自动化这些任务使我能够在后台单独运行时专注于其他工作。当您查看本周的任务时，是否有任何领域可以纳入自动化作业？

我创建的一些作业没有按计划运行。这些作业要么等待某人按下按钮，要么等待另一个进程来触发它们。代码将在我需要时运行任务，并且我可以根据需要单击一个按钮来重新运行它。例如，每月一次，我需要为另一个团队生成分析工件。在不同的时间，我可能需要也可能不需要代码，所以我创建了一个作业，只需单击一个按钮就可以根据需要运行。制作我需要的工件并打包它们只需要 5 分钟，而不是每次都重写代码。

如果一个人没有开始工作，那么另一个进程正在开始它。通常，我的团队将这些作业设置为由另一个作业或管道启动。触发后，作业将运行并创建结果。

当我创造就业机会时，我倾向于考虑的事情是:

```
- Is the task run every so often and can live in a notebook, unscheduled job, or manual pipeline when you need to rerun it? - Is the task run on a schedule and placed into an automated job with that schedule? If yes, then create an automated job that takes in the necessary input. - What inputs are needed for that job or could be changed by the user? These inputs you can pass into the job when it is started. When you include inputs, consider items that you may want to change later even if you are not changing the input value now. For example, the number of files to read in. You may want to keep it at 200 now but change it later, so it should be input. - Is the task, something that is run after another action has been completed? You could create a job that is started when the previous item has been completed, such as having a CI/CD pipeline that kicks off an automated job, such as using Azure Pipelines to kick off jobs in Databricks.
```

# 最后的想法

让代码和自动化为你工作。如果您发现自己重复运行类似的任务和分析，确定是否有一种方法可以建立一个管道或自动化作业来处理这些工作。自动化程度越高，流程中出现人为错误的机会就越少。

***您如何使用管道或工作来帮助您的流程？***

如果你想阅读更多，看看我下面的其他文章吧！

</top-3-challenges-with-starting-out-as-a-data-scientist-705757a6fc09>  </why-you-need-a-data-science-mentor-in-2021-f2ca7372c7a7>  </4-things-i-didnt-know-about-being-a-team-lead-in-data-science-1f96293cb8aa> 