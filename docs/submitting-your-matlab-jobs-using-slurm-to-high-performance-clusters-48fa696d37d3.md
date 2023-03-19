# 使用 Slurm 向高性能集群提交您的 MATLAB 作业

> 原文：<https://towardsdatascience.com/submitting-your-matlab-jobs-using-slurm-to-high-performance-clusters-48fa696d37d3?source=collection_archive---------18----------------------->

## 关于使用 SLURM 向高性能集群提交作业的教程

![](img/bd1296633f6037ce135e4fb4d6c8e39e.png)

作者使用 MATLAB 脚本创建的图像

在我的[上一篇文章](https://medium.com/swlh/how-to-submit-r-code-jobs-using-pbs-to-high-performance-clusters-ed82d0a578c7)中，我写了使用 PBS 作业调度器向高性能集群(HPC)提交作业以满足我们的计算需求。但是，并非所有 HPC 都支持 PBS 作业。最近，我所在的机构还决定为其新安装的集群使用另一种叫做 [Slurm 的作业调度器。](https://public.confluence.arizona.edu/display/UAHPC/Puma+Quick+Start)

<https://medium.com/swlh/how-to-submit-r-code-jobs-using-pbs-to-high-performance-clusters-ed82d0a578c7>  

从它的文档来看，Slurm 是一个开源的、容错的、可伸缩的集群管理和作业调度器 Linux 集群。作为一个集群工作负载管理器，Slurm 有三个关键功能:

1.  它在一段时间内将对资源的访问分配给用户，以便他们可以执行他们的计算任务。
2.  它提供了一个框架来启动、执行和监控一组已分配节点上的工作(通常是并行作业)。
3.  它通过管理挂起工作的队列来仲裁资源的争用。

在本文中，我将演示如何向支持 Slurm 作业调度器的 HPC 节点提交 MATLAB 作业。

你一定想知道为什么 MATLAB 毕竟不是开源的。对此有大量的理由。首先，与大多数 python 包不同，MATLAB 支持真正的并行性——除了那些实际上用 C++实现并且只提供 python 级别的包装器的包。MATLAB 对分布式计算有很好的支持，底层实现确实是用 C++实现的，速度非常快。此外，MATLAB 提供现成的算法来支持控制工程、数字信号处理、机器人等 python 社区仍然落后的领域中的应用程序。如果您是大型组织(如大学)的一部分，那么您的组织可能拥有 MATLAB 的机构许可。

# 为工作提交编写 Slurm 脚本

Slurm 提交脚本有两个部分:(1)资源请求(2)作业执行

脚本的第一部分指定了节点数量、最大 CPU 时间、最大 RAM 量、是否需要 GPU 等。作业将请求运行计算任务。

脚本的第二部分指定加载哪些模块、加载哪些数据文件以及运行哪些程序或代码。

# 一个工作实例

我给出了一个工作示例，我想利用 MATLAB 提供的分布式计算工具箱并行计算一些东西。为此，我可以使用`parfor`代替传统的信号内核`for`。由于我想运行这段代码的 HPC 系统有 94 个内核，我将指定创建 94 个工作线程，这样就可以创建 94 个并行作业。

让我们开始吧，首先，在`/home/u100/username`目录下创建一个名为`minimal_parfor.m`的新文件:

```
L = linspace(0.000001, 200, 800000);
index = 1:length(L);formatOut = 'yyyy_mm_dd_HH_MM_SS_FFF';
dt = datestr(now,formatOut);datafolder  = "/home/u100/username";
outputfile = datafolder + "/"+"SIN_" + dt + ".csv";
if ~isfile(outputfile)
        fprintf("SIN Output file doesn't exist .. creating\n");
        fileID = fopen(outputfile,'w');
        fprintf(fileID,'L,Sin(L)\n');
        fclose(fileID);
else
        fprintf("SIN Output file exists .. not creating\n");
end**parpool(94);**
**parfor** ii = index S = sin(L(ii)); fileID = fopen(outputfile,'a');
    fprintf(fileID,'%.10f,%.10f\n', ...
        L(ii),S);
    fclose(fileID);end
```

正如我们所看到的，我使用`parpool`请求 94 个工人。此外，我正在并行计算一个数字的 sin 值，并将它们保存在一个文件中。我更喜欢将数据保存在文件中而不是存储在变量中的方法，因为我可以在执行仍在运行时查看文件。`parfor`随机选择指定的 94 个索引，运行 94 个并行作业。因此，我不会发现索引被顺序写入文件。但是，我总是可以在以后对它们进行排序。

第二步，我在`/home/u100/username`目录下创建一个 SLURM 文件，并将其命名为`minimal.slurm`:

```
#!/bin/bash# --------------------------------------------------------------
### PART 1: Requests resources to run your job.
# --------------------------------------------------------------
### Optional. Set the job name
#SBATCH --job-name=minimal_parfor### Optional. Set the output filename.
### SLURM reads %x as the job name and %j as the job ID
#SBATCH --output=%x-%j.out
#SBATCH --error=%x-%j.err### REQUIRED. Specify the PI group for this job
#SBATCH --account=manager### Optional. Request email when job begins and ends### Specify high priority jobs
#SBATCH --qos=user_qos_manager# SBATCH --mail-type=ALL### Optional. Specify email address to use for notification
# SBATCH --[mail-user=user@email.edu](mailto:mail-user=rahulbhadani@email.arizona.edu)### REQUIRED. Set the partition for your job.
#SBATCH --partition=standard### REQUIRED. Set the number of cores that will be used for this job.
#SBATCH --ntasks=1
#SBATCH --cpus-per-task=94### REQUIRED. Set the memory required for this job.
#SBATCH --mem=450gb### REQUIRED. Specify the time required for this job, hhh:mm:ss
#SBATCH --time=200:00:00# --------------------------------------------------------------
### PART 2: Executes bash commands to run your job
# --------------------------------------------------------------### SLURM Inherits your environment. cd $SLURM_SUBMIT_DIR not needed
pwd; hostname; dateecho "CPUs per task: $SLURM_CPUS_PER_TASK"
### Load required modules/libraries if needed
module load matlab/r2020b### This was recommended by MATLAb through technical support
ulimit -u 63536 cd $PWD
matlab -nodisplay -nosplash -softwareopengl < /home/u100/username/mininal_parfor.m > /home/u100/username/out_mininal.txt
date
~
```

`minimal.slurm`是一个 bash 脚本，它指定了在 HPC 中请求的资源以及如何执行 MATLAB 作业。我使用命令`SBATCH — cpus-per-task=94`指定了 94 个 CPU，这样当 MATLAB 通过 parpool 请求 94 个工作线程时，它就可以使用了。此外，我请求 450 GB 的 RAM，当我的作业开始运行时它将是可用的。

要将作业提交给 HPC，请键入

```
sbatch minimal.slurm
```

要获取已提交作业的状态，您可以键入:

```
sacct
```

或者

```
squeue | grep username
```

一旦工作开始、完成或由于任何原因终止，我希望在我的 slurm 文件中的指定电子邮件地址上收到一封电子邮件。

我希望这篇文章对使用 SLURM 提交工作的人有所帮助，他们希望使用 MATLAB 的并行计算工具箱

  

有关 MATLAB 分布式计算工具箱的更多详细信息，请查看

## **参考文献**

1.  [https://slurm.schedmd.com/quickstart.html](https://slurm.schedmd.com/quickstart.html)
2.  [https://www . mathworks . com/help/parallel-computing/par pool . html](https://www.mathworks.com/help/parallel-computing/parpool.html)

注意:本文绝不是对 MATLAB 或 Mathworks 的任何产品的邀约。作者与 Mathworks 或相关实体没有任何关系。