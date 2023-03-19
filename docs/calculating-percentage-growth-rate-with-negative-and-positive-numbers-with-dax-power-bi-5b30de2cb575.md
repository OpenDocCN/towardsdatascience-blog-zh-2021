# 使用 DAX 幂 BI 计算负数和正数的增长率百分比

> 原文：<https://towardsdatascience.com/calculating-percentage-growth-rate-with-negative-and-positive-numbers-with-dax-power-bi-5b30de2cb575?source=collection_archive---------13----------------------->

## 简单但复杂的算术

## 计算百分比变化的常用方法(***【A-B/| B |****)在涉及负数的情况下不起作用，但我有一个更好的替代方法，可以让你的工作保持干净和敏捷。*

![](img/2cd2748dd27e8cbdde8fd57fa8e8dab1.png)

照片由[卢克·切瑟](https://unsplash.com/@lukechesser?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

用 **A-B/ABS(B)** 计算增长率似乎是大多数分析师的标准，我们经常忽略这样一个事实，即我们所依赖的简单公式并不是在所有情况下都是正确的。

我最近完成了一份报告，正在向团队展示结果，一名成员要求我延长报告的时间线，其中包括一些负增长期，我随意延长了过滤器，令我惊讶的是，结果显然是错误的。

我回到我的 DAX 公式，看看我是否将 absolute *ABS()* 应用于分母，是的！我做了，但我的结果仍然是错误的，这使我开始思考负数会如何影响我们的百分比增长率计算的输出。

为了更清楚起见，我读了几篇文章，这篇来自 Linkedin [上](https://www.linkedin.com/posts/ghalimi_relative-growth-activity-6755853400505749504-EanF/)[**Ismael**](https://www.linkedin.com/in/ghalimi/)**的帖子解释了相对增长率脱颖而出，阐明了我们在计算增长率时经常忽略的灰色区域。**

**做了一些谷歌搜索，我看到一些分析师使用不同的方法来解决这个问题，懒惰的方法通常涉及返回空白()或零，其中任何值小于零。另一种方法是创建一个回报 P 或 N(正或负)，以掩盖我们亲爱的公式“A-B/ABS(B)”的低效率。**

**在阅读了这么多关于如何解决这个问题的文章后，Excel 超级用户论坛来帮忙了。来自论坛的超级用户(塔赫尔·艾哈迈德)建议考虑不同的场景，并使用 If 函数来解决问题。**

**下面是我最后的 DAX 查询，它最终处理了所有可能影响百分比增长计算的场景，并为任何使用 DAX 的人解决了这个问题**

```
Net Value - Growth Rate MoM% =
VAR _PREV_MONTH =
    CALCULATE ( [Net Value], DATEADD ( 'Calendar Table'[Date], -1, MONTH ) )VAR _CURR_MONTH =
    CALCULATE ( [Net Value] )VAR _RESULT =
    IF (
        AND ( AND ( _CURR_MONTH < 0, _PREV_MONTH < 0 ), _CURR_MONTH < _PREV_MONTH ),
        ( ( _CURR_MONTH - _PREV_MONTH ) / _PREV_MONTH ) * -1,
        IF (
            AND ( AND ( _CURR_MONTH < 0, _PREV_MONTH < 0 ), _CURR_MONTH > _PREV_MONTH ),
            ( ( _CURR_MONTH - _PREV_MONTH ) / _PREV_MONTH ),
            IF (
                AND ( _CURR_MONTH < 0, _PREV_MONTH < 0 ),
                ( ( _CURR_MONTH - _PREV_MONTH ) / _PREV_MONTH ) * -1,
                IF (
                    AND ( _CURR_MONTH > 0, _PREV_MONTH < 0 ),
                    ( ( _CURR_MONTH - _PREV_MONTH ) / _PREV_MONTH ) * -1,
                    IF (
                        _PREV_MONTH < _CURR_MONTH,
                        ( ( _CURR_MONTH - _PREV_MONTH ) / _PREV_MONTH ),
                        IF (
                            _PREV_MONTH > _CURR_MONTH,
                            ( ( _CURR_MONTH - _PREV_MONTH ) / _PREV_MONTH ),
                            ABS ( ( ( _CURR_MONTH - _PREV_MONTH ) / _PREV_MONTH ) )
                        )
                    )
                )
            )
        )
    )RETURN
    _RESULT
```

**以下是对计算增长率百分比时预期的不同情景的简单解释，所有这些都用上述 DAX 公式进行了完美的处理(简单的说明归功于 [Taheer](https://superuser.com/questions/619559/any-trick-to-calculating-percentages-when-the-base-is-a-negative-number)**

1.  **A>0，B>0，A**
2.  **A>0，B>0，A>B**
3.  **一个<0, B> 0，|一个**
4.  **A <0, B> 0，|A|>B**
5.  **A>0，B <0, A**
6.  **A> 0，B<0, A>| B | 0**
7.  **A <0, B<0, |A|**
8.  **A<0, B<0, |A|> |B|**

**下一次，当您使用 DAX 在 Power BI 报告中计算增长率百分比时，请仔细考虑您的公式，并考虑利用上面的片段。**

**在 Linkedin 上关注我[以了解我的故事](https://www.linkedin.com/in/belloramon/)**

**这个片段在 GitHub 上——叉出来！**

**<https://github.com/raymonbell01/Calculate-Percentage-Growth-Rate-with-DAX/commit/d873ff809c10ba6ed8f2ea3cca4f42e17acc3254> **