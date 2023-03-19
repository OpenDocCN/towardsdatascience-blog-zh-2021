# 如何使用 Airflow 集群策略和任务回调来跟踪元数据

> 原文：<https://towardsdatascience.com/airflows-best-kept-secrets-2a7acf59d13b?source=collection_archive---------32----------------------->

## 如何使用气流的两个最保守的秘密来监控你的狗

![](img/942673b7aa0acd2b428c7884f01afd38.png)

图像通过 Shutterstock 在许可下传输到 Databand。图片由 Honor Chan 编辑。

检测气流任务中的问题所需的时间是一个主要问题。这有一个很大的原因。你没有写每一条狗。很难找到一种可扩展的方式来[使你的平台可维护](https://databand.ai/blog/a-data-observability-model-for-data-engineers/)当有数百条管道[用逻辑编写时，如果没有适当的文档，你可能无法理解](https://databand.ai/blog/building-data-pipelines/)。如果你不了解管道是如何工作的，那么你就无法找到一种方法来跟踪关于重要的[数据质量](https://databand.ai/blog/what-is-good-data-quality-for-data-engineers/) &管道问题的&警报。这是个大问题。

虽然这听起来像是一个绝望的情况，但它不是——它只是一个具有挑战性的情况。让我们将这一挑战简化为三个移动部分:

1.  收集所有运算符对象
2.  收集执行元数据
3.  基于上述元数据设置警报和[监控仪表板](https://databand.ai/blog/everyday-data-engineering-monitoring-airflow-with-prometheus-statsd-and-grafana/)

有很多不同的方法可以解决这些问题。您可以使用开源工具和代码构建自己的解决方案，也可以使用托管解决方案来集中管理您的元数据、监控和警报。

但是对于本文，您将学习如何使用 Airflow 的集群策略和任务回调来实现 1 & 2，并且您将有一种方法来监视:

*   任务的工期
*   任务状态
*   您的任务与之交互的数据集的数据质量。

至于第三点，我们将在另一篇文章中解决。

# 用气流集群策略包装您的运营商

从哪里开始？您的第一个想法可能是创建一些包装操作符对象或装饰符，以添加到团队正在使用的当前操作符中，但是对于一个大型组织来说，将您的解决方案实现到每个操作符会花费大量时间。

幸运的是，airflow 提供了一个解决方案——集群策略允许您通过向 Airflow 设置添加一个`policy`来在不同的阶段操纵 Dag 和操作符。Airflow 公开了 3 个策略，每个策略都是 airflow 将在不同阶段加载和调用的功能:

*   `task_policy`–加载时将调用任何`Operator`。
*   `dag_policy`–将在任何`DAG`加载时被调用。
*   `task_instance_mutation_hook`–将在任务执行之前调用任何`TaskInstance`。

对于我们的情况，任何策略都可能是实现的很好的候选，因此我们将很快回来。

# 使用 Airflow 任务回调收集事件元数据

你需要解决的第二个问题是:如何捕捉不同的事件进行报道？

有两条途径可以做到这一点。

第一种选择非常简单；您可以包装任务的`execute`方法，并添加与原始执行相关的新功能。这里有一个你如何做到这一点的例子:

在上面的例子中，我们使用任务策略来强制用我们的`time_task_duraiton`记录器包装我们所有的气流任务。`task_policy`是气流寻找用来包装每个操作符的函数，`task_time_duration`负责计算任务的持续时间。实现这一点将使跟踪任务持续时间成为可能。

另一个选择是使用 Airflow 任务回调。我说的是 Airflow 在任务实例运行过程中调用不同事件的回调函数。

一个不使用任务回调的解决方案，比如上面的，可能满足你的需求，但是我认为在很多情况下这不是最好的选择。如果您想要构建一个更易维护、更可靠、更可伸缩的数据平台，使用 Airflow 任务回调是理想的解决方案。

为什么？它对用户代码的干扰较小。操纵`execute`方法(读作:用户代码)会影响[用户的逻辑](https://databand.ai/blog/everyday-data-engineering-tips-on-developing-production-pipelines-in-airflow/)并导致我们意外的行为。根据经验，作为平台工程师，您应该避免这种情况，并尽可能确保用户代码按预期执行。

那么，什么是气流任务回调？以下是所有的回调及其作用:

*   `pre_execute` -正好在`execute`方法运行之前运行。
*   `post_execute` -在`execute`方法运行后立即运行，并且只有在没有错误的情况下。
*   `on_failure_callback` -如果执行中出现错误且任务失败，则运行
*   `on_success_callback` -如果执行过程中没有错误并且任务成功，则运行。
*   `on_retry_callback` -如果执行中出现错误并且任务设置为重试，则运行。
*   `on_kill` -如果 execute 方法超时，就在引发超时错误之前或获得 SIGTERM 之后运行。

我们将在接下来的例子中使用其中的一些。

# 跟踪任务持续时间

你的 DAG 有过比预期时间短的时候吗？可能平时跑两个小时的时候跑了两分钟？这个例子将探索如何解决这个问题。

在这个例子中，我创建了两个函数— `pre_duration`和`post_duration`，它们一起记录任务的运行持续时间。使用`pre_execute`和`post_execute`回调来捕捉任务的开始和结束。

现在，您可以添加通用函数`wrap`和`add_policy`来减少添加多个策略的样板文件。

将这段代码放在`$AIRFLOW_HOME/config/airflow_local_settings.py`下，这样 Airflow 会找到您的策略并加载它。

运行 DAG 后，日志文件中的输出应如下所示:

# 跟踪任务状态

当一项任务失败一次，并不一定意味着有大问题。因此，开发运维人员或数据运维人员配置其气流环境以便在发生这种情况时重新运行任务是非常常见的。这样做的问题是:您需要知道该任务是否超过了可接受的重试阈值，以便在需要时可以快速修复问题。

因此，下面是如何使用相同的逻辑来收集任务状态的元数据:

# 使用您的新工具跟踪气流中的数据运行状况

正如大多数 Airflow 用户可能知道的那样，可能发生的最可怕的事情是，您的 Airflow UI 显示为全绿色，但随后数据并没有以您期望的形式交付。这意味着数据本身正在破裂——而不是管道——而 Airflow 没有一种直接的方法来跟踪这一点。

在数据质量方面有很多盲点。您需要了解从以前的 Dag 中读取卷时的异常情况，数据提供商可以在不通知您的情况下更改他们的 API，从而导致有问题的模式更改，并且您需要验证数据集中的数据健康状况(例如`null`值的比率太高)。

Airflow 集群策略和回调可以帮助您跟踪类似的数据质量指标。

在下面的例子中，我们假设您想要开始跟踪`Operators`对他们正在使用的数据(读或写)的影响

以下是您想了解的一些数据信息:

*   **操作类型** —读或写。
*   **操作时间戳** —跟踪我们的数据有多新。
*   **受操作影响的行数和列数**

给定您正在寻找的元数据，有许多方法可以做到这一点。为了简单起见，我假设我们的 DAG 遵循这种模式:

*   返回值是操作符输出文件的路径
*   我们使用 Xcom 来同步返回值
*   接受输入值的操作符将 Xcom 传递给名为“path”的参数

以下是 DAG 的样子，可以为您提供一些背景信息:

关于`python_callables`的细节并不重要，您需要记住的唯一事情是，当函数将数据写入输出文件时，文件路径就是返回值。

现在，让我们来看看这些记录数据帧形状的跟踪函数:

在本例中，`check_input`和`check_output`尝试从 csv 加载数据帧，并记录包含列和行计数的`shape`

并将这些功能添加到我们的策略中:

# 气流元数据跟踪准备就绪！

综上所述:

1.  您了解了“气流群集策略”以及我们如何使用它来跟踪系统中的每个`Operator`。
2.  从我们的`Operators`中了解了任务的回调、它们何时执行以及如何使用它们来收集执行数据

现在，由你来[实现你自己的跟踪系统](https://databand.ai/blog/everyday-data-engineering-monitoring-airflow-with-prometheus-statsd-and-grafana/)。这将是以后的一篇文章，但是我会给你留下一些你可能同时想要收集的数据的想法:

1.  从`SnowflakeOperator`、`PostgresOperator`或任何带有 [sqlparse](https://pypi.org/project/sqlparse/) 的 SQL 操作符中提取有关查询的信息。
2.  关于每一列的统计信息—空值的数量、百分比、直方图等等。使用 [pydeequ](https://pypi.org/project/pydeequ/) 或[大期望](https://pypi.org/project/great-expectations/)来验证操作员的输出
3.  跟踪您与****交互的数据的模式，这样您就可以确保知道它何时发生变化。
4.  收集系统指标—内存、cpu 使用率。尝试 [filprofiler](https://pypi.org/project/filprofiler/) 提取内存使用情况。
5.  运行时跟踪`[template_fields](<https://airflow.apache.org/docs/apache-airflow/stable/howto/custom-operator.html#templating>)`值

让我们知道您可能感兴趣的其他[跟踪功能](https://databand.ai/platform)！

## 想了解我和我的团队正在构建的数据可观察性平台的更多信息吗？在 [Databand.ai](http://databand.ai) 找到我们。