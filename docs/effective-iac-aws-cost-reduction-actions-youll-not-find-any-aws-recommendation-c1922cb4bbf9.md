# 你在任何 AWS 建议中都找不到的有效 IaC AWS 成本降低措施

> 原文：<https://towardsdatascience.com/effective-iac-aws-cost-reduction-actions-youll-not-find-any-aws-recommendation-c1922cb4bbf9?source=collection_archive---------31----------------------->

## 如何在不购买保留实例的情况下大幅降低 EC2 成本。

![](img/b73fc0043191775db044d6e4b4cfa294.png)

照片由[视觉故事||米歇尔](https://unsplash.com/@micheile?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 拍摄

**简介**

当开发一个新项目时，在 AWS 上从头开始建立云架构会很快导致成本快速增加。从第一天起，了解服务成本并采取节约成本的措施是至关重要的。

通常，客户希望尽可能地灵活，但由于需求快速变化，为他们的计算或数据库服务利用保留实例定价是不可行的。

在本文中，我将解释**如何在不购买保留实例的情况下大幅降低 EC2 成本**。

# 大小合适的实例

在讨论 AWS 服务的不同定价模式之前，有必要看一下通常由两种 AWS 服务提供的适当调整建议:

*   成本管理/合适的规模建议[2]
*   计算优化器[3]

考虑这些服务的建议**可以从一开始就防止过度供应**。

# 尽可能对所有非生产环境使用 Spot 实例

通常，在应用程序在生产环境中投入使用之前，会有一些部署阶段(开发、测试、UAT、试运行等)。).对于非生产环境，只要有可能就使用点实例被证明是有效的，尤其是当您使用无状态、容错的工作负载时([1])。

使用 spots 时，你要预料到它们可能会被打断。在选择实例类型之前，可以通过查看 AWS 的[“spot Instance Advisor”](https://aws.amazon.com/de/ec2/spot/instance-advisor/)[2】来评估 Spot 实例被中断的概率，该向导显示了与按需实例相比的节省，以及 Spot 实例被中断的概率，具体取决于地区。

欧盟-中欧-1(法兰克福)现场顾问 2021–08–05

上表显示了中断的可能性，并比较了从 AWS 文档[5]中收集的 3 种实例类型(m5.large、m4.large、c5.xlarge)的不同定价模式(按需、现货、预留 1 年全额预付)的降价情况。

使用/创建 spot 实例与相应的 terraform 资源有各种不同的方式:

*   单点实例/ `aws_spot_instance_request` [6]
*   现货舰队请求/`aws_spot_fleet_request`【7】
*   使用混合实例策略/ `aws_autoscaling_group` [8]自动缩放组

使用自动缩放组并设置`descired_capacity`、`min_size`和`max-size`不仅可以确保 spot 实例在被中断时被重新请求，还可以让我们非常容易地实现另一个节省成本的措施，我们将在下一节中看到如何实现。

通过用`on_demand_base_capacity=0`、`on_demand_percentage_above_base_capacity=0`配置`aws_autoscaling_group` —资源中的`instances_distribution`块，可以确保只使用点实例。将`spot_allocation_strategy`设置为`lowest-price`将进一步从定义的实例池中请求最便宜的实例类型。

```
instances_distribution {
      on_demand_base_capacity                  = 0
      on_demand_percentage_above_base_capacity = 0
      spot_allocation_strategy                 = "lowest-price"
    }
```

# 在非办公时间关闭所有非生产环境的实例

除了通过使用 spots 实现节约之外，在用户不工作的时间或节假日实施计划停机也是可行的。

> "为什么要为根本没有使用的资源支付租金？"

对于 ASG 管理的实例，如使用具有混合实例策略的启动模板的方法所描述的，这可以通过自动缩放组的“预定动作”功能来实现[10]。

通过使用 terra form-resource`aws_autoscaling_schedule`【9】，您可以通过在 cron 表达式定义的特定时间设置所需的 ASG 大小参数来缩放 ASG。

让我们使用所描述的方法，将非办公时间定义为

*   周一至周五:20:00–06:00
*   星期六，星期日:00:00–24:00

一个实例的月成本比较

比较不同的成本节约选项，我们会发现将现场实例和计划关闭相结合明显优于标准保留实例定价(1 年全额预付)。

# 结论

AWS 文档包含许多优化成本的信息和文档，通常非常受欢迎。可以理解的是，并不是每种方法都被 AWS 正式推荐，但是在实践中证明是非常有效的。

我展示了一种方法，它结合了 spot 实例的使用，并使用 ASG 调度操作在非办公时间关闭它们，与按需定价相比，这大大降低了成本。

如果您在降低 AWS 基础设施成本方面需要更多帮助，请联系我们。

# **参考文献**

[1][https://docs . AWS . Amazon . com/white papers/latest/cost-optimization-leveling-ec2-spot-instances/when-to-use-spot-instances . html](https://docs.aws.amazon.com/whitepapers/latest/cost-optimization-leveraging-ec2-spot-instances/when-to-use-spot-instances.html)

[2][https://docs . AWS . Amazon . com/AWS count billing/latest/about v2/ce-right sizing . html](https://docs.aws.amazon.com/awsaccountbilling/latest/aboutv2/ce-rightsizing.html)

[3]https://aws.amazon.com/de/compute-optimizer/

[https://aws.amazon.com/de/ec2/spot/instance-advisor/](https://aws.amazon.com/de/ec2/spot/instance-advisor/)

[5][https://AWS . Amazon . com/de/ec2/pricing/reserved-instances/pricing/#](https://aws.amazon.com/de/ec2/pricing/reserved-instances/pricing/#)

[6][https://registry . terraform . io/providers/hashi corp/AWS/latest/docs/resources/spot _ instance _ request](https://registry.terraform.io/providers/hashicorp/aws/latest/docs/resources/spot_instance_request)

[7][https://registry . terraform . io/providers/hashi corp/AWS/latest/docs/resources/spot _ fleet _ request](https://registry.terraform.io/providers/hashicorp/aws/latest/docs/resources/spot_fleet_request)

[8][https://registry . terra form . io/providers/hashi corp/AWS/latest/docs/resources/auto scaling _ group # mixed-instances-policy-with-spot-instances-and-capacity-rebalance](https://registry.terraform.io/providers/hashicorp/aws/latest/docs/resources/autoscaling_group#mixed-instances-policy-with-spot-instances-and-capacity-rebalance)

[9][https://registry . terraform . io/providers/hashi corp/AWS/latest/docs/resources/auto scaling _ schedule](https://registry.terraform.io/providers/hashicorp/aws/latest/docs/resources/autoscaling_schedule)

[10][https://docs . AWS . Amazon . com/auto scaling/ec2/user guide/schedule _ time . html](https://docs.aws.amazon.com/autoscaling/ec2/userguide/schedule_time.html)