# 使用 Elastic Horovod 和 Amazon EC2 Spot 实例进行经济高效的分布式培训

> 原文：<https://towardsdatascience.com/cost-efficient-distributed-training-with-elastic-horovod-and-amazon-ec2-spot-instances-599ea35c0700?source=collection_archive---------39----------------------->

## [理解大数据](https://towardsdatascience.com/tagged/making-sense-of-big-data)

## 根据员工系统的可用性动态调整您的培训课程

![](img/33e43713a67eab7c3dcf82a83484e9e5.png)

[梁杰森](https://unsplash.com/@ninjason?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上的照片

[**Horovod**](https://horovod.readthedocs.io/en/stable/index.html)**是一个流行的框架，用于在多个 GPU 工作人员和多个主机上运行分布式培训。[在这篇文章中，我将解释如何使用 Elastic Horovod 来降低分布式培训环境中的成本，并演示在](https://horovod.readthedocs.io/en/stable/elastic_include.html)[Amazon Elastic Compute Cloud(Amazon EC2)](https://aws.amazon.com/ec2/?ec2-whats-new.sort-by=item.additionalFields.postDateTime&ec2-whats-new.sort-order=desc)spot 实例上运行它所需的配置步骤。**

**本文包括四个部分。在第 1 部分中，我们首先描述如何使用 spot 实例来降低培训成本。在第 2 部分中，我们将讨论数据分布式训练，并介绍在多个 spot 实例上进行训练的挑战。在第 3 部分中，我们描述了 Elastic Horovod 解决这一挑战的方式，并阐述了解决方案的一些方面。在第 4 部分中，我们在 Amazon EC2 spot 实例上演示了一个弹性 Horovod 的例子。**

**我要讲的故事和我要分享的例子是基于 Gloo 0 . 21 . 2 版本的 Horovod、PyTorch 1.4 版本的 py torch 和 tensor flow 2.3 版本的。这些框架继续快速发展，一些 API 定义、使用流程和功能行为可能会发生变化。请务必重新评估我们将针对未来版本做出的任何假设和结论。**

****免责声明 1:** 这篇文章的目的是引起人们对这个有趣而重要的新特性的注意。不一定推荐使用它。是否集成弹性 Horovod 应由多种考虑因素决定，其中一些我们将在下面讨论。**

****免责声明 2:** 本帖绝不是为了取代[弹性 Horovod 文档](https://horovod.readthedocs.io/en/stable/elastic_include.html)。我们将多次参考文档，详细阐述一些微妙之处，扩展其中一个示例，并演示一个端到端的示例。但是，对于您自己的任何实现，官方文档都应该是您的参考来源。**

# **第 1 部分:使用 Spot 实例降低培训成本**

**现代 DNN 培训项目的主要挑战之一是管理培训成本。训练模型所需的硬件是昂贵的，考虑到我们经常需要运行多个训练实验，有时在多个设备上，不难看出成本会很快成为一个问题。**

**在云中进行培训时，降低成本的一个令人信服的方法是使用剩余云服务容量中的折扣计算引擎。在 AWS 中这些被称为 [**Amazon EC2 Spot 实例**](https://aws.amazon.com/ec2/spot/?cards.sort-by=item.additionalFields.startDateTime&cards.sort-order=asc) ，在 Google Cloud 中它们被称为 [**可抢占 VM 实例**](https://cloud.google.com/compute/docs/instances/preemptible) ，在微软 Azure 中它们被称为 [**低优先级 VM**](https://docs.microsoft.com/en-us/azure/batch/batch-low-pri-vms)。这些产品的具体细节往往会有所不同，但共同点是它们都为未使用的计算引擎提供了显著的折扣。**

**代价是，与[按需](https://aws.amazon.com/ec2/pricing/on-demand/)或[保留实例](https://aws.amazon.com/ec2/pricing/reserved-instances/pricing/)相反，这些机器可能不可用(例如在高峰使用期间)，即使成功获得，它们也可以在任何时候被抢占。因此，spot 实例可能不是时间关键型训练任务的好选择。但对于其他任务，它们可能会节省大量资金。**

**当然，需要考虑培训工作提前终止的可能性。没有人愿意花几个小时训练一个模型，只是为了在他们即将完成的时候让他们的训练机器被抢占。正如我在[上一篇文章](/6-development-habits-for-increasing-your-cloud-ml-productivity-becdc41eb289)中描述的，我们可以通过在我们的训练应用中开发 ***容错*** 来防止这种情况。具体来说，通过定期存储模型检查点，我们可以确保终止的训练会话可以从最后存储的检查点恢复，而不是从头开始训练。存储检查点的频率应该通过权衡存储检查点的计算开销和在终止的情况下必须从最后存储的检查点重新运行训练的开销来确定。以高频率存储检查点将减少终止后恢复的开销，但是将增加存储模型权重的开销。以低频率存储检查点减少了捕获模型权重所花费的时间，但是在过早终止的情况下增加了更长时间重新训练的风险。**

# **第 2 部分:通过数据分发加速训练**

**加速 DNN 开发的常用方法是执行**数据分布式训练**。在数据分布式训练中，我们在多个工作器(GPU)上并行训练我们的模型。多个工作者可以在单个设备上(具有多个 GPU)，或者在多个设备上。在每个训练步骤中，每个工人训练不同批次的样本，然后与所有其他工人分享其结果梯度。用 N 表示工人的数量，分布式培训的效果是，我们在每个培训步骤结束 N 批培训。如果配置正确，这可以将模型收敛所需的步骤数减少 n 倍。**

**越来越多的趋势是在越来越多的(GPU)工作人员和越来越多的设备上分布模型(尤其是大型模型)的训练。自然，多台昂贵机器的使用放大了在低成本现场实例上培训的潜在节省。然而，在这种情况下，我们现在需要考虑多个训练设备中的任何一个上的现场中断的可能性。(一个有趣的问题是，使用超过 1/N 训练步数的 N 个设备是否会增加我们对现场中断的整体脆弱性。另一个有趣的问题是，一个实例的局部中断是否会增加其他实例的局部中断的可能性。虽然很难预测现场中断或计算其可能性，但您可以采用一些策略来减少对多个现场中断的脆弱性。有关该主题的更多信息，请参见此处的。)**

**在许多分布式训练框架中，包括 Horovod，在单个设备的现场终止的情况下，整个训练会话将失败。因为在 N 个工作线程的情况下，点中断的开销要高得多，所以增加捕获检查点的频率是有意义的(如上所述)。但是捕获每个检查点的开销也要高得多，因为现在有 N 个工作线程在捕获检查点期间暂停。**

**有人可能想知道，在多个训练设备的情况下，当只有一个设备被中断时，我们为什么需要终止整个训练会话。为什么训练不能简单地在剩余的设备上继续？这就是**弹性 Horovod** 的用武之地。**

**<https://horovod.readthedocs.io/en/stable/elastic_include.html>  

# 第 3 部分:弹性卵形线

在 Horovod 版本 0.20 中引入的 Elastic Horovod 支持动态增减工作人员的数量，而不会中断培训过程。特别是，如果您正在运行多个 spot 实例，并且其中一个实例被中断，您将无需(从最新的检查点)重新启动培训会话，从而节省大量时间(和成本)开销。当同一个 spot 实例恢复活动时(如果它被配置为“[持久](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/spot-requests.html)”)，或者引入一个新的 spot 实例时，这也适用。在接下来的小节中，我们将讨论这个特性的一些方面。

## 有 Gloo 的 Horovod

弹性 Horovod 需要 [Gloo](https://horovod.readthedocs.io/en/stable/install_include.html#gloo) 控制器来协调 Horovod 进程之间的工作。这与更常见的不支持弹性 Horovod 的 MPI 控制器相反。关于 Gloo 的更多细节，参见 [Horovod 安装指南](https://horovod.readthedocs.io/en/stable/install_include.html#horovod-installation-guide)。

## 发现主机

为了支持动态伸缩，Elastic Horovod 需要一种发现主机设备的机制。这是通过用户定义的“主机发现脚本”提供的，该脚本被传递给 horovodrun 命令行。该脚本必须定义为输出可用主机的名称列表。更多详情见[此处](https://horovod.readthedocs.io/en/stable/elastic_include.html#running-with-horovodrun)。下面是一个非常简单的主机发现 bash 脚本的示例，它遍历一个预定义的 IP 地址列表，并只打印从其中接收到成功 ping 响应的地址:

```
#!/bin/bash
hostArray=(
  "10.0.1.20"
  "10.0.1.21"
  "10.0.1.22"
)
for host in ${hostArray[*]}; do
  if ping -c 1 $host &> /dev/null
  then
    echo $host
  fi
done
```

在实际场景中，主机发现脚本可能要复杂得多，并且将取决于您分配工作机的机制以及您如何为它们分配 IP 地址或主机名。

## PyTorch 示例

下面是一个如何使用 PyTorch 和 Elastic Horovod 创建一个简单的训练脚本的例子。该脚本基于这里提出的[模板](https://horovod.readthedocs.io/en/stable/elastic_include.html#elastic-pytorch)，添加了缺失的功能。我们突出显示了与弹性 Horovod 相关的代码部分。

```
import torch
import horovod.torch as hvd
import torchvision.models as models
import torch.nn as nn
import torch.optim as optimhvd.init()
torch.cuda.set_device(hvd.local_rank())
device = torch.device("cuda:0" if torch.cuda.is_available() else "cpu")
lr = 0.001
model = models.resnet50(pretrained=True)
model.to(device)
loss_optim = nn.CrossEntropyLoss()
optimizer = optim.SGD(model.parameters(), lr * hvd.size())
optimizer = hvd.DistributedOptimizer(optimizer)def get_random_batch():
    batch_size = 2
    data_in = torch.rand(batch_size, 3, 64, 64).to(device)
    target = torch.zeros(batch_size).long().to(device)
    return data_in, target# training loop must be wrapped by [@hvd](http://twitter.com/hvd).elastic.run
[**@hvd**](http://twitter.com/hvd)**.elastic.run** def train(**state**):
    for **state.epoch** in range(state.epoch, 100):
        print("Epoch", state.epoch)
        for **state.batch** in range(state.batch, 100):
            data, target = get_random_batch()
            optimizer.zero_grad()
            output = model(data)
            loss = loss_optim(output, target)
            loss.backward()
            optimizer.step()
        # commit the state at the end of each epoch
        # see documentation for how to choose frequency of 'commit'
        **state.commit()**
        **state.batch = 0****def on_state_reset():
    # adjust learning rate on reset
    for param_group in optimizer.param_groups:
        param_group['lr'] = lr * hvd.size()**# state object tracks and synchronizes state among the workers **state = hvd.elastic.TorchState(model, optimizer, batch=0, epoch=0)
state.register_reset_callbacks([on_state_reset])** train(state)
```

下面是如何使用 disover_host.sh bash 脚本运行 Elastic Horovod 的示例，每个主机设备包含 4 个 GPU(插槽),以及上面的 Python 脚本(我们将其命名为 train.py):

```
horovodrun -np **8** \
           --min-np **4** \
           --max-np **12** \
           --host-discovery-script **discover_hosts.sh** \
           --slots **4** \
           python **train.py**
```

在本例中，一旦有 8 名工人可用，培训就会开始，只要有 4 名工人可用，培训就会继续，并且在任何给定时间，培训的工人都不会超过 12 名。

## **弹性态物体**

培训工人之间的同步由[弹性状态对象](https://horovod.readthedocs.io/en/stable/elastic_include.html#modifying-the-training-script-with-state-synchronization)管理。该对象封装了需要在工作线程之间同步的所有变量，包括模型权重、优化器变量、epoch 和批号。

状态对象包括用于备份训练状态的提交功能。这是一个自动防故障装置，旨在防止由于某个工作线程意外崩溃而导致状态损坏的可能性。类似于上面讨论的关于选择检查点捕获频率的难题，决定提交训练状态对象的频率是在减少失败情况下从最后一次备份重新训练的开销和提交动作的开销之间的权衡。我们的目标应该是配置我们的环境，使意外崩溃的可能性极小，并将提交减少到最低限度。例如，我们可以依靠 [AWS 现场中断通知](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/spot-interruptions.html#spot-instance-termination-notices)来确保发现脚本以优雅的方式处理现场中断，并避免意外的连接故障。

**关于弹性 Horovod Keras API** 的重要提示:使用 TensorFlow Keras 训练时，弹性状态承诺通过[hvd . Elastic . commitstatecallback](https://horovod.readthedocs.io/en/stable/api.html)回调进行管理。注意这个回调的默认行为(如 [elastic keras 示例](https://horovod.readthedocs.io/en/stable/elastic_include.html#elastic-keras)所示)是在每批之后提交。这将大大降低你的训练速度。为了避免这种情况，在回调构造函数中指定一个 *batches_per_commit* 值。

## 重置回调函数

重置回调函数负责根据培训人员的添加或删除对模型配置进行更改。需要特别注意训练可能依赖于全局批处理大小的超参数，尤其是优化器参数。许多流行的优化器(如 [Adam](https://en.wikipedia.org/wiki/Stochastic_gradient_descent#Adam) )的性能取决于全局批处理大小的值。如果在训练期间全局批处理大小发生变化，而没有对优化器设置进行适当的调整，则训练收敛可能会受到负面影响。 [Elastic Horovod 文档](https://horovod.readthedocs.io/en/stable/elastic_include.html#practical-considerations-consistent-training)建议根据活动工作者的数量来调整学习率(如上面的 PyTorch 示例),或者使用对工作者数量变化不太敏感的优化器(如 Adasum)。另一个选项是修改每个工作线程的本地批处理大小，以便不改变全局批处理大小(假设 GPU 内存允许这样做，并且不会导致 GPU 资源利用不足)。但是**不能保证这些技术就足够了**。在调整弹性训练之前，您应该验证您的训练算法能够处理工人数量的变化，而不会损害收敛。

## 数据划分

执行多员工数据分布式培训时，需要做出的一个决定是如何在不同员工之间分配培训数据。在弹性训练场景中，这个决定有些复杂。如果您选择对数据进行分割，使每个工作人员在独立的数据子集上进行训练，那么您可能希望在每次工作人员数量发生变化时对数据进行重新分区，以确保每个数据样本都受到相同的关注。

通过定义每个工作者在整个数据集的随机洗牌上进行训练，可以避免随着拓扑的每次变化而更新数据集的需要。虽然这种策略中的数据采样结果可能与分片的情况有很大不同(例如，在训练过程中给定样本的外观不一定像分片的情况那样均匀分布)，但在许多情况下，它不会影响训练的收敛能力，尽管您可能希望在自己的模型上验证这一点。

## 主机排除策略

弹性 Horovod 包括一项相对严格的政策，将变得不响应的员工排除在外。(在撰写本文时，Horovod 源代码将此功能称为“黑名单”。)这可能会对训练环境造成限制，在训练环境中，主机可能会被中断，但稍后会以相同的 IP 地址或主机名恢复。在撰写本文时，有一个[开放拉取请求](https://github.com/horovod/horovod/pull/2483/files)旨在放松这一政策，这将有望进入即将到来的版本。在任何情况下，您都可能需要通过覆盖默认行为来自定义排除策略。

## 调度员的脆弱性

弹性 Horovod 将使您能够从任何主机系统的故障中恢复，除了使用 *horovodrun* 命令运行培训会话的主机。如果这台主机出现故障，整个培训课程将会终止。

为了增加系统的容错能力，你可以在一个不可抢占的设备上运行 *horovodrun* 命令，比如一个[按需 AWS EC2 实例](https://aws.amazon.com/ec2/pricing/on-demand/)。为了降低成本，您可以将该设备指定为专用调度程序，而不需要其自身的任何 GPU 工作程序。然而，这种方法有一个潜在的问题。当前 Horovod 代码中内置了一个假设，即所有设备的网络接口名称都是相同的。虽然在相同的 GPU 工作设备上确实经常是这种情况，但在非 GPU 设备上可能不是这样。我们将在下一节中看到这样的例子，我们将在 EC2 实例上演示弹性 Horovod。

有一个开放的[特性请求](https://github.com/horovod/horovod/pull/2482)来解决这个问题，所以希望这个问题会在未来的版本中得到解决。与此同时，您可以尝试重命名网络接口，或者尝试以下简单的解决方法(hack):打开 *launch_gloo_elastic* 函数中的*horovod/runner/gloo _ run . py .*创建一个 *dispatcher_nics* 列表以及现有的 *nics* 列表，并将此列表传递给后续的 *network.get_driver_ip* 调用，而不是如中所示的 *nics*

```
nics = get_common_interfaces(driver)
**dispatcher_nics=['ens5']**
server_ip = network.get_driver_ip(**dispatcher_nics**)
```

另一个要考虑的选项是将 GPU 设备的子集配置为按需*实例。虽然这种解决方案可能成本更高，但它具有额外的优势，即使在零现场实例可用性的情况下也能确保持续培训。*

## 弹性射线

[Ray](https://docs.ray.io/en/latest/ray-overview/index.html#gentle-intro) 是一个构建[分布式应用](https://docs.ray.io/en/latest/cluster/index.html)的流行框架，支持[启动云集群](https://docs.ray.io/en/latest/cluster/launcher.html#ref-automatic-cluster)和[自动集群扩展](https://docs.ray.io/en/latest/cluster/autoscaling.html)。Elastic Horovod 包括与 Ray 的集成，可以用来简化环境设置。参见[文档](https://horovod.readthedocs.io/en/stable/elastic_include.html#running-on-ray)了解如何轻松扩展您的脚本以使用 Ray 功能。

# 第 4 部分:Amazon EC2 Spot 实例上的弹性 Horovod

在本节中，我们将在 Amazon EC2 集群上演示一个端到端的弹性 Horovod 示例。特别感谢我的同事 Max Rabin 对这一部分的帮助。

有许多**不同的方式来配置和启动云集群，包括高级 API(比如 [Ray](https://docs.ray.io/en/latest/cluster/cloud.html#ref-cloud-setup) )。我们选择使用 boto3 Python API 来演示实例创建，但是该示例可以很容易地适用于其他方法。**

**步骤 1:创建 EC2 Spot 实例**

我们从[启动](https://docs.aws.amazon.com/quickstarts/latest/vmlaunch/step-1-launch-instance.html)三个 p2.xlarge spot 实例主机设备和一个 c5.xlarge 按需实例开始，它将充当我们的调度程序。我们选择了单个 GPU 设备(即插槽=1)，但在现实世界中，与主机数量更多、每个主机的插槽数量更少的情况相比，主机数量更少、每个主机的插槽数量更多(例如 p2.8xlarge)的情况下，您通常会获得更好的性能。

在下面的代码块中，您可以找到如何创建三个 GPU spot 实例的示例。要创建 dispatcher 实例，只需将 count 设置减少到 1，将实例类型更改为 *c5.xlarge，*，并删除 *InstanceMarketOptions* 设置。记下创建调用返回的实例 id，因为这些 id 可用于从命令行管理实例。

```
import boto3
ec2 = boto3.resource('ec2', region_name="us-east-1")
instances = ec2.create_instances(
    MaxCount=3, MinCount=3,
    ImageId='ami-072519eedc1730252',#replace with ML AMI of choice
    InstanceType='p2.xlarge',
    SubnetId='<subnet id>', # <-- fill this in
    IamInstanceProfile={'Arn':'<InstanceProfile>'}, <-- fill this in
    SecurityGroupIds=['<SecurityGroupIds>'], <-- fill this in
    InstanceMarketOptions={
        'MarketType': 'spot',
        'SpotOptions': {
            "SpotInstanceType": "persistent",
            "InstanceInterruptionBehavior": "stop"
        }
    }
 )
print(instances)
```

对于我们的图片 ID，我们选择了最新的 Ubuntu 18.04 [AWS 机器学习图片](https://aws.amazon.com/machine-learning/amis/)。下面是提取最新图像的命令行代码片段。

```
aws ec2 describe-images --owners amazon \
    --query 'Images[?Name!=`null`]|[?starts_with(Name,`Deep Learning AMI (Ubuntu 18.04)`) == `true`].[ImageId,Name,CreationDate]' \
    --output text | sort -k2 | tail
```

**第二步:SSH 设置**

Horovod 依赖于调度员和所有工人之间的无密码 SSH 通信。要设置这个，使用一个[记载的机制](https://docs.aws.amazon.com/quickstarts/latest/vmlaunch/step-2-connect-to-instance.html#sshclient)连接到 EC2 实例。在 dispatcher 上创建无密码的 SSH 密钥，并将公钥复制到每台主机设备上。有关详细信息，请参考下面的链接。

<https://linuxize.com/post/how-to-setup-passwordless-ssh-login/>  

在真实的场景中，这一步应该是自动化的。

**第三步:作为 Ubuntu 用户，激活虚拟环境并安装 Horovod**

需要在所有实例上执行此步骤。

```
source activate pytorch_p36
pip install --upgrade pip
HOROVOD_WITH_GLOO=1 pip install --no-cache-dir horovod
```

在 dispatcher 上运行 *ifconfig* 以查看网络接口的名称，并按照上一节所述更新*horovodrun/runner/gloo _ run . py*文件。

**步骤 4:配置 discover_hosts.sh 脚本**

将我们在上面创建的 discover_hosts.sh 复制到 dispatcher 实例上，并用三个主机实例的 IP 修改 hostArray。

确保该脚本是可执行的，并且其目录在$PATH 环境变量中。

**步骤 5:使用 horovodrun 进行 PyTorch 训练**

将上面的 PyTorch 脚本复制到每个主机设备，并运行以下测试:

**测试 1 —主机添加** : 停止三台主机中的一台。这可以通过 Amazon EC2 仪表板或使用 AWS CLI 来完成:

```
aws ec2 stop-instances --instance-ids i-<id1>
```

在有两个活动主机的调度程序上运行*horovudrun*:

```
horovodrun -np **2** --min-np **1** --max-np **3** --host-discovery-script **discover_hosts.sh** --slots **1** python **train.py**
```

在培训运行时，启动第三个 EC2 实例(从仪表板或 CLI)，验证培训会话是否识别了添加内容，并将第三个实例添加为培训主机。

```
aws ec2 start-instances --instance-ids i-<id1>
```

**测试 2 —主机移除:**由于我们无法确定现场终止的时间，我们将通过简单地停止其中一台主机来模拟。像以前一样，开始对所有三台主机进行训练，经过几个训练步骤后，停止其中一台主机。验证是否已识别出移除操作，以及培训会话是否能够仅使用剩余的两台主机继续进行。

请注意，我们刚刚执行的主机移除会被视为*不合适的*，并且会导致故障主机被添加到排除列表中。(如果您使用相同的 IP 重新启动同一台机器，它将不会被重新添加到相同的培训课程中。)在真实的场景中，应该增强主机发现机制，以确保优雅地处理现场中断*。*

***测试 3——将运行时间与非弹性 Horovod 进行比较:**您可能有理由希望验证增加的功能不会带来任何额外成本。为此，将运行时与非弹性运行进行比较:*

```
*horovodrun --gloo \
           -np 3 \
           -H server1:1,server2:1,server3:1 \
           python train.py*
```

***测试 4 —测量提交频率的影响:**使用弹性状态提交的频率来测量对训练运行时的影响。*

***第六步:清理***

*不要忘记在工作结束时[删除所有 EC2 实例](https://docs.aws.amazon.com/quickstarts/latest/vmlaunch/step-3-clean-up-instance.html)。*

*这可以通过 EC2 仪表板或使用 AWS CLI 来完成:*

```
*aws ec2 terminate-instances --instance-ids i-<id1> i-<id2> ...*
```

# *摘要*

*Elastic Horovod 是 Horovod 的一个引人注目的新特性，它支持在可抢占实例集群上进行训练，而没有在设备中断时必须从检查点重新开始训练的潜在开销。这种解决方案可以在对培训进度影响最小的情况下，显著降低培训成本。然而，Elastic Horovod 可能不是每个团队和每个项目的最佳解决方案。时间关键型任务通常最好在完全可靠的*按需*实例上运行，即使对于非关键型培训任务，您也应该验证收敛性不受动态变化的工作人员数量的影响。祝你好运！***