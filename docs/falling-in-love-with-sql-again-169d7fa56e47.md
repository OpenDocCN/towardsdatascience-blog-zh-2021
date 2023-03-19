# 再次爱上 SQL

> 原文：<https://towardsdatascience.com/falling-in-love-with-sql-again-169d7fa56e47?source=collection_archive---------9----------------------->

## 通过利用公共表表达式的能力

![](img/f7b15093f1407e8edc4ac5cda82553a4.png)

乔恩·泰森在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上的照片

当我在 15 年前第一次学习 SQL 时，我惊讶于它是如此的有用和强大。我恋爱了。但是多年来，随着我开始编写更复杂的查询，我的代码开始变成一堆难以维护的连接和嵌套子查询。如果我需要编辑几周前写的一个查询，我的眼睛会变得呆滞，我会放弃理解它，我会从头开始写一个新版本。SQL 正在失去它的魅力。

然后，几年前令人惊奇的事情发生了。我在一个随机的博客上偶然发现了示例 SQL 代码，它使用了这个叫做公共表表达式的特性。SQL 又长又复杂，但易于阅读和理解。我立刻就被吸引住了。从那以后，我完全改变了思考和编写 SQL 的方式。

# 有哪些常见的表格表达式？

通用表表达式，或 **CTE** s，基本上是一种在查询顶部列出一个或多个选择表达式并在以后通过名称引用它们的方法，就像你处理一个表一样。它们在概念上类似于创建临时表，但是更容易使用并且开销更低。

```
WITH 
  cte1 as (SELECT …),
  cte2 as (SELECT …)
SELECT * FROM cte1 JOIN cte2
```

在这样一个虚构的例子中，很难完全理解 cte，所以让我们来看一个更真实的用例。

# 学生考试成绩示例

比方说，我想找到加利福尼亚州学生的平均最高数学考试分数。思考这个问题，我基本上需要做以下几个步骤:

1.  获取学生子集(加利福尼亚)
2.  获取测试分数的子集(数学)
3.  加入他们在一起，以获得所有加州学生的数学考试成绩
4.  获得每个学生的最高分
5.  取整体平均值

为了让这个变得更有趣一点，我将假设我们不直接存储学生的地理位置，而是必须加入他们的学校才能算出来。

我将回顾我解决这个问题的老方法(在我了解 cte 之前)和我的新的改进方法。

# 旧方法(没有 cte)

我将首先让加州所有的学生。就像我上面提到的，我们需要将学生加入他们的学校，以便确定他们的位置:

```
SELECT students.id 
FROM students 
JOIN schools ON (
  students.school_id = schools.id AND schools.state = 'CA'
)
```

然后，我可以加入数学考试的结果:

```
SELECT students.id**, test_results.score** 
FROM students 
JOIN schools ON (
  students.school_id = schools.id AND schools.state = 'CA'
)
**JOIN test_results ON (
  students.id = test_results.student_id
  AND test_results.subject = 'math'
)**
```

请注意，我们可以反过来做——从数学测试结果开始，加入加州学生。两者同样有效。至此，我已经掌握了所有加州学生的数学成绩。为了只选择每个学生的最高分，我可以添加一个最高分，并按以下方式分组:

```
SELECT students.id, **MAX(**test_results.score**) as score**
FROM students 
JOIN schools ON (
  students.school_id = schools.id AND schools.state = 'CA'
)
JOIN test_results ON (
  students.id = test_results.student_id
  AND test_results.subject = 'math'
)
**GROUP BY students.id**
```

现在，每个学生都有一行他们最好的数学成绩。最后，我将把它放在一个外部选择中，以获得总体平均值:

```
**SELECT AVG(score) FROM (**
  SELECT students.id, MAX(test_results.score) as score
  FROM students 
  JOIN schools ON (
    students.school_id = schools.id AND schools.state = 'CA'
  )
  JOIN test_results ON (
    students.id = test_results.student_id
    AND test_results.subject = 'math'
  )
  GROUP BY students.id
**) tmp**
```

我们完事了。最终结果还不算太糟，但是如果没有上下文，阅读和理解起来还是有点吓人。添加更多的嵌套级别、连接和条件只会让事情变得更糟。

值得注意的一件有趣的事情是，每一步都倾向于在 SQL 的开头和结尾添加代码——换句话说，逻辑是由内向外分层流动的。

# 新方法(使用 cte)

我会以同样的方式开始，选择加州的所有学生。为了使用公共表表达式，我给这个 SELECT 语句起了个名字，并把它添加到查询顶部的带有子句的**中:**

```
**WITH
  student_subset as (**
    SELECT students.id 
    FROM students 
    JOIN schools ON (
      students.school_id = schools.id AND schools.state = 'CA'
    ) **),**
```

现在我可以对考试成绩做同样的事情。与我以前的方法不同，我现在不担心连接。我只是为所有学生选择一个独立的数学考试成绩数据集。

```
WITH
  student_subset as (
    SELECT students.id 
    FROM students 
    JOIN schools ON (
      students.school_id = schools.id AND schools.state = 'CA'
    )
  ), **score_subset as (
    SELECT student_id, score 
    FROM test_results 
    WHERE subject = 'math'
  ),**
```

获得这两个独立的数据集后，我可以使用另一个 CTE 将它们连接在一起:

```
WITH
  student_subset as (
    SELECT students.id 
    FROM students 
    JOIN schools ON (
      students.school_id = schools.id AND schools.state = 'CA'
    )
  ),
  score_subset as (
    SELECT student_id, score 
    FROM test_results 
    WHERE subject = 'math'
  ), **student_scores as (
    SELECT student_subset.id, score_subset.score
    FROM student_subset 
    JOIN score_subset ON (
        student_subset.id = score_subset.student_id
    )
  ),**
```

现在，我需要将每个学生的最高分限制在一行，我也可以通过添加额外的 CTE 来做到这一点:

```
WITH
  student_subset as (
    SELECT students.id 
    FROM students 
    JOIN schools ON (
      students.school_id = schools.id AND schools.state = 'CA'
    )
  ),
  score_subset as (
    SELECT student_id, score 
    FROM test_results 
    WHERE subject = 'math'
  ),
  student_scores as (
    SELECT student_subset.id, score_subset.score
    FROM student_subset 
    JOIN score_subset ON (
        student_subset.id = score_subset.student_id
    )
  ), **top_score_per_student as (
    SELECT id, MAX(score) as score 
    FROM student_scores 
    GROUP BY id
  )**
```

最后，我取总平均值。因为这是最后一步，所以它是作为主查询在 WITH 子句之外完成的:

```
WITH
  student_subset as (
    SELECT students.id 
    FROM students 
    JOIN schools ON (
      students.school_id = schools.id AND schools.state = 'CA'
    )
  ),
  score_subset as (
    SELECT student_id, score 
    FROM test_results 
    WHERE subject = 'math'
  ),
  student_scores as (
    SELECT student_subset.id, score_subset.score
    FROM student_subset 
    JOIN score_subset ON (
        student_subset.id = score_subset.student_id
    )
  ),
  top_score_per_student as (
    SELECT id, MAX(score) as score 
    FROM student_scores 
    GROUP BY id
  ) **SELECT AVG(score) 
FROM top_score_per_student**
```

我们完事了。最终结果要冗长得多(27 行对 12 行)，但在许多方面弥补了这一点。

# cte 的好处

**逻辑流**—使用 cte，逻辑从上到下流动，而不是从内向外流动。这是一种更自然的思考和编写 SQL 的方式，尽管它需要一些时间来适应。

**可读性** —逻辑流程意味着您可以从顶部开始阅读使用 CTEs 的查询，并立即开始理解它。对于更传统的从内向外的流程，仅仅是找出从哪里开始阅读就要花费令人沮丧的时间。一旦你开始阅读，你将不得不不停地跳来跳去弄清楚发生了什么。随着查询变得越来越复杂，这变得越来越明显。

**文档** —将查询分成线性步骤的另一个副作用是，它为您提供了一个添加文档的自然位置。除了 CTE 名称(例如“top_score_per_student”)，在步骤之间添加行内 SQL 注释也很容易。没有 cte 提供的强制线性结构，这样做既困难又麻烦。

**轻松调试** —当结果看起来不正确时，您可以轻松地从上到下完成这些步骤，沿途验证数据，以确定问题发生的确切位置。如果没有 cte，您将经常发现自己在调查 bug 时对 SQL 进行了如此糟糕的处理，以至于当您最终找到 bug 时，需要花费同样长的时间来展开所有工作并返回到最初的查询。最糟糕的是，有时您的修复工作在剥离下来的被屠杀的 SQL 上，但由于某种原因而不是在您的原始查询上，您又回到了起点。

**可重用性**—多个查询通常共享一些通用的业务逻辑。例如，使用相同的“加州学生”定义，但查看出勤记录而不是考试成绩。使用 cte，这些定义中的每一个都是一个自包含的 SELECT 表达式，可以很容易地在查询之间复制/粘贴，而无需修改。

**性能** —您可能认为使用 cte 会导致查询变慢，但实际上大部分时间都不是这样。SQL 查询引擎非常聪明，通常会将这两种方法编译成相同的优化执行路径。以一种迂回的方式，cte 实际上可以提高性能，因为它们帮助您将优化工作的优先级集中在最广泛使用的表达式上。例如，如果您经常使用连接学生和学校的表达式来获取地理位置，那么创建一个新的物化视图来消除连接可能是值得的。

# 离别笔记

我找不到几年前第一次向我介绍常用表表达式的博客文章，但我永远感激它帮助我重新点燃了对 SQL 的热爱。希望这篇文章能为其他人做同样的事情。