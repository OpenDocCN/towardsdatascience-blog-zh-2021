# 记录你的流量

> 原文：<https://towardsdatascience.com/log-your-flow-392d0d399127?source=collection_archive---------34----------------------->

## 利用有效的日志改进代码监控

![](img/b282bfb531874a22f1b9eace3ae4877d.png)

图片由艾丁·奥康纳在 [StockFreedom](http://stockfreedom.com) 上拍摄

您是否曾经遇到过 prod 中的一个错误，并且您打开了日志记录系统，却发现日志不足以指示性地跟踪发生了什么以及原因？
好吧，我可以向你保证这从来没有发生在我身上；-)

但是如果它发生了，我一定会吸取教训，让我的日志更加丰富，对未来更有指示性。

今天，我将与您分享我收集的关于如何使我们的日志对未来的调试尽可能有用的结论。愿你的产品永远不会有 bug！

# 使用指示性实体 id

这听起来可能有点琐碎，但有时会被忽略。
打开您的日志系统并交叉检查您可以捕捉到的不同提示可能会很乏味，只有这样您才能找到您正在寻找的实体，这样您才能开始调试真正的问题。

在微服务架构中，获取日志的相关实体 ID 并不总是可能的，但是如果可能的话，我强烈建议使用它。

# 记录主流程和子流程的开始和结束

假设我们有几个主要流程，每个主要流程由几个子流程或动作组成。

当一个错误发生时，我们想知道它发生在哪里，以及在错误之前/之后发生了什么。在每个主流程和子流程的开头和结尾添加日志将使日志对我们来说更加易读和清晰。我们不仅可以对事件的顺序有一个大致的了解，而且我们还可以知道一些事件是否应该发生，但最终没有发生——可能是因为抛出了一个异常。

这并不意味着我们要记录代码中发生的所有事情的开始和结束(例如每个函数)。我们将在本文后面讨论不要过度日志记录的必要性。

# 记录主要变量的相关变化

帐户/应用程序的状态是否发生了变化？也许从待定到批准，或者关闭？或者也许决策变量的值已经改变并影响了代码中的未来流程？

例如，如果一个实体在一种状态下开始一个流，而在另一种状态下结束这个流，那么记录这种变化是很有用的，并且可能会将其原因添加到您的日志中。也有可能在流程中，由于不同的原因，状态会发生几次变化，日志会告诉我们完整的故事——发生了什么，什么时候，为什么。

# 记录错误的原因

日志应该易于浏览——我们希望快速确定问题是什么以及问题发生在哪里，并且相对快速地完成。
当我们根据每条消息的严重程度对其进行分类时，我们可以过滤掉目前与我们无关的级别。
这一方面使我们能够记录非常具体的消息，另一方面避免在调试过程中读取太多数据。

为了让您熟悉不同的级别，并查看使用示例，我邀请您阅读我的附录文章— [日志级别](https://medium.com/@naomikriger/levels-of-logs-a261a0f291a2)。

# 保持一致的格式

当浏览日志以找到您需要的相关信息时，当您知道您期望的答案在哪里时，这样做更容易。此外，当特定流或子流的所有日志都一致时，您可以快速区分您的流的日志和那些也可能出现在您的搜索结果中但在其他不相关的流中生成的日志。这将使你能够迅速过滤掉它们。

# 对于下面的代码库，让我们在实践中看看这些技巧:

```
loans/
    __init__.py
    main_flows/
        __init__.py
        car_loan.py
        student_loan.py
    sub_flows/
        __init__.py
        auto_approval_eligibility_check.py
        car_loan_criteria_check.py
        technical_requirements_check.py
        student_loan_criteria_check.py
```

*子流*中的每个文件都有一个主类，这个主类有几个函数。一个*主流程*由几个子流程组成。
例如:

*   **学生 _ 贷款**用途*最低 _ 资格 _ 检查* & *学生 _ 贷款 _ 标准 _ 检查*
*   **车贷**用途*最小 _ 资格 _ 检查* & *汽车 _ 审批 _ 资格 _ 检查* & *车贷 _ 条件 _ 检查*

# 使用指示性实体 ID +记录主流程和子流程的开始和结束

```
**logger.info(f'**Applicant — {applicant_id} started StudentLoanApplication**')****logger.info(f'**Applicant — {applicant_id} started TechnicalRequirementsCheck**')****logger.info(f'**Applicant — {applicant_id} finished TechnicalRequirementsCheck**')****logger.info(f'**Applicant — {applicant_id} started StudentLoanCriteriaCheck**')****logger.info(f'**Applicant — {applicant_id} finished StudentLoanCriteriaCheck**')****logger.info(f'**Applicant — {applicant_id} finished StudentLoanApplication**')**
```

# 记录主要变量的相关变化

```
**logger.info(f'**Applicant — {applicant_id} started AutoApprovalEligibilityCheck**')****logger.info(f'**Applicant — {applicant_id} is qualified for student loan, application status changed to — {ApplicationStatus.APPROVED}**')****logger.info(f'**Applicant: {applicant_id} finished AutoApprovalEligibilityCheck**')**
```

# 记录预期错误的原因+使用相关的日志级别

```
**logger.info(f'**Applicant — {applicant_id} started MinimalEligibilityCheck**')****logger.debug(f'**Applicant — {applicant_id} started passed_technical_requirements check**')****logger.error(f'**Applicant — {applicant_id} application status is set to {ApplicationStatus.ERROR}. **'**
 'Student ID not found in the University’s database**')****logger.info(f'**Applicant — {applicant_id} finished MinimalEligibilityCheck**')**
```

# 不要做什么

## 不要写无意义的日志

指示性日志将为我们提供可以开始工作的信息——因此，除了提及发生了什么之外，我们还将知道实体 ID、发生更改的代码部分、更改的原因或者我们能够使用的其他信息。

这里有一个我们不应该做的例子-

```
**logger.info('**Application rejected**')**
```

更好的日志应该是这样的-

```
**logger.info(f'**Applicant — {applicant_id} financial balance in the past {MONTHS_TO_CHECK} months does not match the criteria for a student loan.
Application status updated to {APPLICATION.STATUS.REJECTED}**')**
```

## 不要污染原木

在具有足够指示性的日志和过于冗长的日志之间有一条细线，我们应该小心不要越过这条线。

描述性太强的日志示例包括:

*   提供完整的数据结构(字典、列表等)。)和用于在代码中做出特定决策的数据
*   写一条描述性太强的日志消息——同样的消息可以用更短的句子来写，有时太冗长=更麻烦、更不清晰

最终，日志应该清晰、简洁、易于浏览。
有时，我们“勉强接受”语法不完美的日志消息，以便让消息更短、阅读更快。对于浏览来说过于冗长的日志使用起来不够舒服。

优秀的日志将使我们能够快速发现问题，并知道调试什么以及如何调试。

这里有一个我们不应该做的例子-

```
**logger.info(f'**Applicant — {applicant_id} not qualified for student loan — average balance in the past {MONTHS_TO_CHECK} is too low. Daily bank balance as provided in bank statements is {daily_bank_balance}**')**
```

上面的例子也有问题，因为它暴露了客户的敏感信息。但是这里主要提供了一个例子，展示了用于计算单个值的完整列表/字典。

更好的日志应该是这样的-

```
**logger.info(f'**Applicant — {applicant_id} not qualified for student loan — average balance in the past {MONTHS_TO_CHECK} is too low. Average bank balance is {average_bank_balance}, minimal acceptable average balance is {min_accepatble_average_balance}**')**
```

## 不要过度记录

如果我们不得不在某个方向出错，我宁愿在太少的木头上放太多的木头。毕竟，试图调试一个错误并且缺少必要的数据是一件痛苦的事情。
然而，在添加新日志时，我们要记住日志应该是有意义和有用的。如果日志没有提供附加值，那么它主要是噪音，我们最好不要它。

> 日志应该读起来舒服和有用

![](img/51784907ce168f5dc7a03cc2ac4e34fb.png)

模因作者来自 [imgflip](https://imgflip.com/)