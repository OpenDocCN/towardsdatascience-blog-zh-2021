# 更好的带 Katib 的 ML 模型

> 原文：<https://towardsdatascience.com/better-ml-models-with-katib-6c0dbbbe401f?source=collection_archive---------29----------------------->

## Kubernetes 上的自动机器学习超参数调整

![](img/4e9ab0e84a13afbe5a487319e82b796a.png)

由 [Markus Gjengaar](https://unsplash.com/@markus_gjengaar?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 在 [Unsplash](https://unsplash.com/s/photos/piano?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 拍摄

在机器学习中，超参数是用户定义的值，在训练期间保持固定。超参数的例子是 k-means 聚类中的`k`值、学习速率、批量大小或神经网络中的隐藏节点数。

虽然现在有一些技术依赖于在我们的模型学习时改变这些值(例如，Leslie Smith 的单周期策略、模拟退火等)。)，这些不是可学习的参数，例如神经网络的权重。

然而，超参数可以极大地影响由训练过程生成的模型的质量以及算法的时间和存储器要求。因此，必须调整超参数，以获得给定问题的最佳配置。

我们可以手动或自动调整模型的超参数。在大多数情况下，手工执行这项任务是不切实际的。因此，自动超参数调整机制将有助于在合理的时间范围内发现每个超参数的最佳值。

许多系统自动完成这一过程。这个故事考察了 [Katib](https://github.com/kubeflow/katib) ，这是一个开源的、框架无关的、云原生的、可扩展的、生产就绪的超参数调优系统。但是为什么我们需要另一个超参数优化引擎呢？我们已经有了众多的选择: [Optuna](https://optuna.org/) ，[远视](http://hyperopt.github.io/hyperopt/)， [NNI](https://nni.readthedocs.io/en/stable/) ，[维齐尔](https://cloud.google.com/ai-platform/optimizer/docs/overview)，或者[雷](https://docs.ray.io/en/latest/tune/)。所以，首先，让我们看看 Katib 做了什么来改善现实世界中的事情。

> [Learning Rate](https://www.dimpo.me/newsletter?utm_source=article&utm_medium=medium&utm_campaign=ml_with_katib&utm_term=mlops) 是为那些对 AI 和 MLOps 的世界感到好奇的人准备的时事通讯。你会在每周五收到我关于最新人工智能新闻和文章的更新和想法。订阅[这里](https://www.dimpo.me/newsletter?utm_source=article&utm_medium=medium&utm_campaign=ml_with_katib&utm_term=mlops)！

# 动机

要在生产环境中部署机器学习系统，我们必须同时满足数据科学家和 DevOps 工程师的需求。这并不容易，因为环境的这两个用户很少说同一种语言。

数据科学家对开发尽可能精确的 ML 模型感兴趣；因此，要调整模型的超参数，它们的工作方式如下:

1.  使用数据集的一个样本，在他们的本地机器上试验 HP 调优
2.  将实验转移到更大的计算环境中，在那里可以利用专用硬件的能力，如 GPU 或 TPU
3.  比较并可视化结果
4.  在团队内部跟踪、版本化和共享结果

另一方面，DevOps 工程师管理基础设施，确保平台保持健康和高度可用。我们可以将开发运维工程师的职责总结如下:

1.  执行资源高效型部署
2.  支持多个用户，通过动态资源分配在同一系统中执行不同的操作
3.  实时升级系统，不会给用户带来任何停机时间
4.  通过日志记录监控系统并及时做出决策

定义了背景之后，我们需要我们的系统来工作，让我们看看 Katib 如何应对这些挑战。

## Katib 优势

Katib 是唯一一款能够在本地数据中心或私有/公有云中作为托管服务运行的惠普调优系统。这是真实的原因有很多:

*   **多租户:** Katib 支持多租户，这有利于跨团队协作。维齐尔也支持多租户；然而，Vizier 是一个专有系统。
*   **分布式训练:** Katib 支持分布式训练(如参数服务器、RingAllReduce 等。).像 Optuna 和 HyperOpt 这样的流行框架缺乏对分布式训练的支持。
*   **Cloud-native:** Katib 是 Kubernetes 准备好的。这使得它非常适合云原生部署。射线调谐和 NNI 也支持 Kubernetes，但需要额外的努力来配置。
*   **可扩展性:** Katib 很容易扩展，提供了一个模块化接口，允许搜索算法和数据存储定制。
*   **NAS:** Katib 是仅有的两个支持神经架构搜索的框架之一，另一个是 NNI。

因此，作为一个支持多租户的分布式容错系统，Katib 同时满足了数据科学家和系统管理员的需求。

# 系统工作流程

现在让我们检查一个典型的工作流，用户将遵循这个工作流与 Katib 进行交互。在本例中，我们将使用贝叶斯优化创建一个 HP 调优作业。

1.  用户可以创建 YAML 规范并使用客户端工具(如`kubectl`)提交，也可以通过编程方式与 Katib 通信。这里，为了清楚起见，我们将做前者。因此，让我们创建一个`Experiment`的 YAML 规格。

```
apiVersion: "kubeflow.org/v1beta1"
kind: Experiment
metadata:
  namespace: kubeflow
  name: bayesianoptimization-example
spec:
  objective:
    type: maximize
    goal: 0.99
    objectiveMetricName: Validation-accuracy
    additionalMetricNames:
      - Train-accuracy
  algorithm:
    algorithmName: bayesianoptimization
    algorithmSettings:
      - name: "random_state"
        value: "10"
  parallelTrialCount: 3
  maxTrialCount: 12
  maxFailedTrialCount: 3
  parameters:
    - name: lr
      parameterType: double
      feasibleSpace:
        min: "0.01"
        max: "0.03"
    - name: num-layers
      parameterType: int
      feasibleSpace:
        min: "2"
        max: "5"
    - name: optimizer
      parameterType: categorical
      feasibleSpace:
        list:
          - sgd
          - adam
          - ftrl
  trialTemplate:
    primaryContainerName: training-container
    trialParameters:
      - name: learningRate
        description: Learning rate for the training model
        reference: lr
      - name: numberLayers
        description: Number of training model layers
        reference: num-layers
      - name: optimizer
        description: Training model optimizer (sdg, adam or ftrl)
        reference: optimizer
    trialSpec:
      apiVersion: batch/v1
      kind: Job
      spec:
        template:
          spec:
            containers:
              - name: training-container
                image: torch-mnist:v1beta1
                command:
                  - "python3"
                  - "/opt/torch-mnist/mnist.py"
                  - "--batch-size=64"
                  - "--lr=${trialParameters.learningRate}"
                  - "--num-layers=${trialParameters.numberLayers}"
                  - "--optimizer=${trialParameters.optimizer}"
            restartPolicy: Never
```

一个`Experiment`是一个定制的 Kubernetes 资源(CRD ),指的是一个完整的用户任务。例如，这里的`Experiment`命令 Katib 运行一个贝叶斯优化任务。目标是通过调整学习率或优化器选择等参数来最大化验证的准确性。它将并行运行`3`试验，如果没有达到目标(即验证精度> = `0.99`)，它将在`12`试验后终止。

> 在这种情况下，我们对自己创建的 MNIST 数据集使用 PyTorch 解决方案。

2.这将触发一个`Suggestion`规范的创建。`Suggestion`是另一个 Kubernetes CRD，它请求创建基于优化算法的`x`平行试验。因此，对于我们的示例，它将生成以下规范:

```
spec:
  algorithmName: bayesianoptimization
  requests: 3
```

3.接下来，基于搜索算法，部署相关服务。在我们的例子中，每个实验都将部署一个贝叶斯优化服务。

4.然后`Suggestion`控制器用来自算法服务的结果更新`Suggestion`规范。

```
spec:
  algorithmName: bayesianoptimization
  requests: 3
status:
  suggestionCount: 3
  suggestions:
    - name: bayesianoptimization-run-1
      parameterAssignments:
      - name: --lr
        value: "0.013884981186857928"
      - name: --num-layers
        value: "3"
      - name: --optimizer
        value: adam
    - name: bayesianoptimization-run-2
      parameterAssignments:
      - name: --lr
        value: "0.024941501303260026"
      - name: --num-layers
        value: "4"
      - name: --optimiz
        value: sgd
    - name: bayesianoptimization-run-3
      ...
```

5.我们现在准备产生多个`Trials`。如果您要将模型训练代码重构为一个函数，那么`Trial`将执行这个函数，并将超参数建议作为参数传递。这句话几乎是真的。确切发生的情况是，我们的`Experiment`定义中的`trialTemplate`被转换成一个`runSpec`，其中建议替换了模板变量:

```
runSpec: |-
  apiVersion: batch/v1
  kind: Job
  metadata:
    name: bayesian-run-1
    namespace: kubeflow
  spec:
    template:
      spec:
        containers:
        - name: bayesian-run-1
          image: katib-mnist-example
          command:
          - "python"
          - "/classification/train_mnist.py"
          - "--batch-size=64"
          - "--lr=0.013884981186857928"
          - "--num-layers=3"
          - "--optimizer=adam"
```

6.最后，`Trial`控制器读取`Trial`规格并产生相应的`TrialJobs`。超参数作为命令行参数传递给`TrialJobs`。

7.`Trial`控制器监控`TrialJobs`并更新`Trial`状态中的相关字段。当底层作业完成其运行时，`Trial`也被标记为完成。度量被报告给度量存储，并且最佳客观度量被记录在`Trial`状态中。

8.最后但同样重要的是，`Experiment`控制器读取所有试验的状态，并尝试验证我们是否达到了目标。如果`Experiment`已经达到其目标，那么它被标记为完成。如果没有，它将继续运行，直到达到`Experiment`规格中指定的最大试验次数。

如果你认为工作量很大，让 [Kale](https://github.com/kubeflow-kale/kale) 为你处理所有这些步骤。请看下面的故事:

</hyperparameter-tuning-should-not-be-part-of-your-ml-code-44c49e80adb6>  

# 结论

在这个故事中，我们描述了什么是超参数，以及为什么优化模型的超参数配置很重要。我们看到这是一个很难手工完成的任务。因此，我们研究了 Katib 如何帮助我们实现这一过程的自动化。

Katib 是迄今为止唯一一个在云世界中如此自然出现的超参数优化系统。投入其中，让它为您处理惠普优化的日常工作。

> [学习率](https://www.dimpo.me/newsletter?utm_source=article&utm_medium=medium&utm_campaign=ml_with_katib&utm_term=mlops)是为那些对 AI 和 MLOps 的世界感到好奇的人准备的时事通讯。你会在每周五收到我关于最新人工智能新闻和文章的更新和想法。在这里订阅！

# 关于作者

我的名字是 [Dimitris Poulopoulos](https://www.dimpo.me/?utm_source=article&utm_medium=medium&utm_campaign=ml_with_katib&utm_term=mlops) ，我是一名为 [Arrikto](https://www.arrikto.com/) 工作的机器学习工程师。我曾为欧洲委员会、欧盟统计局、国际货币基金组织、欧洲央行、经合组织和宜家等主要客户设计和实施过人工智能和软件解决方案。

如果你有兴趣阅读更多关于机器学习、深度学习、数据科学和数据运算的帖子，请在 Twitter 上关注我的 [Medium](https://towardsdatascience.com/medium.com/@dpoulopoulos/follow) 、 [LinkedIn](https://www.linkedin.com/in/dpoulopoulos/) 或 [@james2pl](https://twitter.com/james2pl) 。此外，请访问我的网站上的[资源](https://www.dimpo.me/resources/?utm_source=article&utm_medium=medium&utm_campaign=ml_with_katib&utm_term=mlops)页面，这里有很多好书和顶级课程，开始构建您自己的数据科学课程吧！

所表达的观点仅代表我个人，并不代表我的雇主的观点或意见。