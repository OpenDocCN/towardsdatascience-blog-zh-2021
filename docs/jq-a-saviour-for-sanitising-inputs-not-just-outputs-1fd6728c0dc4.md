# jq:净化输入而不仅仅是输出的救星

> 原文：<https://towardsdatascience.com/jq-a-saviour-for-sanitising-inputs-not-just-outputs-1fd6728c0dc4?source=collection_archive---------23----------------------->

有很多例子和教程使用 [jq](https://github.com/stedolan/jq/blob/master/README.md) 来处理 JSON 输出或快速访问 JSON 主体中的项目。我在下面的参考资料中链接了几个我最喜欢的。

然而，当需要 json 作为 CLI 输入时，很少有人知道它的好处。这种用例的一个主要例子是与 AWS CLI 交互，我将在本文中向您展示这一点。

![](img/25dadf84ceb1e2cd0182e7c1da3df6d6.png)

图片来源:作者本人

## 一个典型的程序员问题:

假设我们想使用`aws ssm send-command`工具向 ec2 实例提交一个命令。当`ssh`不可用时，这可能特别有用。

> 通常情况下，端口 22 (ssh)是不开放的，或者至少非常局限于少数几个 IP 地址。这篇 [AWS 文章](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/authorizing-access-to-an-instance.html)总结了无限制 ssh 访问的安全风险

为了通过 [ssm](https://docs.aws.amazon.com/systems-manager/latest/userguide/ssm-agent.html) 发送命令，我们可以使用以下代码:

```
instance_id="i-123456789"
aws ssm send-command \
  --document-name "AWS-RunShellScript" \
  --targets "Key=InstanceIds,Values=${instance_id}" \
  --parameters "{'commands':['echo foo']}"
```

简单对吗？

不完全是这样，命令被发送给超级用户 *sudo，*但是在许多用例中，我们很可能希望提交给非管理用户，比如 *ec2-user。*我们可以通过在`su "-" "ec2-user" -c '$command’`中包装命令来做到这一点——因为我们已经在 parameters 参数上使用了双引号，`$command`周围的单引号不会意外地将它写成“文字”。

然而，我们仍然有可能在这里遇到一些不足之处。假设我们正在编写一个从 stdin 获取命令的函数，我们可能希望像下面这样编写一行程序，并将其保存为`submit_to_ec2_instance.sh`。

```
#!/usr/bin/env bash: '
Simple script that submits the command string from stdout in
to the instance id (sysarg 1)
'# The first input, should start with 'i-'
instance_id="$1" # Send our command from stdin to $instance_id
aws ssm send-command \
  --document-name "AWS-RunShellScript" \
  --targets "Key=InstanceIds,Values=${instance_id}" \
  --parameters "{'commands':[\"su - ec2-user -c '$(</dev/stdin)'\"}"
```

如果用户决定用

```
echo "sleep '5'; echo 'done here'" | \
submit_to_ec2_instance.sh i-123456789
```

那么我们实例 id 上的`su`函数将接收以下参数:

*   `-`
*   `ec2-user`
*   `-c`
*   `sleep 5; echo done`
*   `here`

哦，亲爱的..这里的最后两个参数应该通过单引号合并成一个参数。好像我们在 stdin 周围的单引号和用户在他们命令里的单引号冲突了！

> 命令行上的参数由空格分隔，但可以通过使用单引号、双引号或转义空格来解释为一个参数。
> 在这种情况下，单引号提前结束。`… -c 'sleep 5; echo 'done here''`被错误拆分。解释器无法区分用户指定的内部单引号和 shell 脚本函数中设置的外部单引号。

## 一系列“不可行”的变通办法

以下每一条都是可行的，但至少有一条警告:

1.  **告诉用户不要使用单引号**

但是如果他们不总是能控制输入呢？他们可能会盲目地将另一个命令的输出解析成这个命令？

2.**使用类似** `**sed**` **的工具来确保我们将用户输入中的所有** `**'**` **都变成了** `**\'**`

这是一个好主意，但是如果他们已经转义了单引号，我们需要确保转义的单引号是双重转义的……编写一个类似于 [shlex 的](https://docs.python.org/3/library/shlex.html)解析器会变得非常复杂和耗时。

3.**回到 stdin 前后的转义双引号**

然后，用户需要转义任何双引号。转义引号也需要双转义。同样，我们要么期望用户转义引号，要么编写另一个 shlex 工具。

啊呀，仅仅一个命令就变得复杂了！

## jq 来救援了！

让我们回到起点，考虑将`--parameters` arg 作为字符串表示中的 json 对象。在下面的参考资料中，我们注意到 jq 对于查询 json 输出和简单的数据整理非常有用。对于生成 json 输入，它也非常有用。下面我们将使用`jq`和`<<<` bash [here-string](https://tldp.org/LDP/abs/html/x17837.html) 来初始化一个 json 对象，使用`jq`填充它，它将为我们完成所有的转义处理，然后将它设置为我们的`--parameters`值。

让我们从最基本的 jq 输入开始:

```
$ jq <<< "{}"
{}
```

现在让我们用`my_arg`和`my_val`创建一个最小的 json 对象

```
$ jq '."my_arg"="my_val"' <<< {}
{
  "my_arg": "my_val"
}
```

> 一个小警告是 *jq* 建议在它的过滤字符串周围使用单引号。为了在这个字符串中获得我们的命令行变量，我们需要使用`--arg`参数。

让我们使用 jq *arg* 参数来尝试获取一个 json 对象，它看起来类似于 aws ssm send-command *参数* CLI 值的预期值。使用方括号`[]`是因为*命令*实际上是命令列表。

```
$ command="sleep '5'; echo 'done here'"
$ jq \
  --arg key "commands" \
  --arg value "$command" \
  '.[$key]=[$value]' <<< {}
{
  "commands": [
    "sleep '5'; echo 'done here'"
  ]
}
```

整洁！该输出将所有内容放入一个参数中，如上所述，这将在 *sudo* 帐户上运行，而不是在 *ec2-user* 帐户上运行。为了给我们的*值*参数添加前缀，我们可以使用`+`过滤器命令来插入我们的前缀。

```
$ command="sleep '5'; echo 'done here'"
$ jq \
  --arg key "commands" \
  --arg value "$command" \
  '.[$key]=["su - ec2-user -c " + $value]' <<< {}
{
  "commands": [
    "su - ec2-user -c sleep '5'; echo 'done here'"
  ]
}
```

我们很接近了！这仍然有之前的引用问题，幸运的是我们可以使用`tojson`方法来避免`command`中的任何双引号。

> jq 过滤器字符串中的`\()`相当于 shell 的`$()`命令调用。

```
$ command="sleep '5'; echo 'done here'"
$ jq \
  --arg key "commands" \
  --arg value "$command" \
  '.[$key]=["su - ec2-user -c " + "\($value | tojson)"]' <<< {}
{
  "commands": [
    "su - ec2-user -c \"sleep '5'; echo 'done here'\""
  ]
}
```

吼吼！让我们也添加几个已经转义的引号，看看 jq 对它们做了什么

```
$ command="sleep '5'; echo 'done here' \"with escaped quotes\""
$ jq \
  --arg key "commands" \
  --arg value "$command" \
  '.[$key]=["su - ec2-user -c " + "\($value | tojson)"]' <<< {}
{
  "commands": [
    "su - ec2-user -c \"sleep '5'; echo 'done here' \\\"with escaped quotes\\\"\""
  ]
}
```

太棒了！我们现在可以这样写我们的`submit_to_ec2_instance.sh`

```
**#!/usr/bin/env bash** : '
Takes in command from stdin and an instance-id positional argument
Submits command to ec2-user on instance-id
'

# Step one -> Collect the instance id
instance_id="$1"  # The first input, should start with 'i-'

# Escape quotes from command, set up parameter arg as json string
# This should evaluate to (with escape quotes as necessary)
# {
#   "commands": [
#     "su - ec2-user -c "my-command arg1 arg2..."
#   ]
# }
parameter_arg="$(jq --raw-output \
  --arg key "commands" \
  --arg value "$(</dev/stdin)" \
  '.[$key]=["su - ec2-user -c " + "\($value | tojson)"]' <<< {})"

# Now run the aws ssm send-command function with our instance id
aws ssm send-command \
  --document-name "AWS-RunShellScript" \
  --targets "Key=InstanceIds,Values=${instance_id}" \
  --parameters "${parameter_arg}"
```

## 我应该什么时候使用这个？

理想情况下，如果我们经常在 CLI 上与 AWS 交互，我们会在 python 脚本中使用类似于 [boto3](https://boto3.amazonaws.com/v1/documentation/api/latest/index.html) 的模块来组织我们的参数。然而，在这种模块和高级语言可能不容易获得的情况下，上述代码是有用的。

这种情况的一个例子可能是外部 CI/CD 执行器，如 [GitHub Actions](https://github.com/features/actions) 或 [TravisCI](https://travis-ci.org/) ，在这种情况下，我们需要在每次执行代码时安装高级语言(如 python)和任何所需的模块。通过使用类似上面的简单 shell 脚本，CI/CD 可以获得一个全新的 ubuntu 安装，安装 *jq* 到 *apt-get* ，然后运行一个快速 bash 脚本将我们的请求提交给我们想要的端点。

另一个用例可能是将这个脚本设置为您的`.bashrc`中的一个简单函数，而您对 python 和 boto3 的可访问性可能取决于当前的 *venv* 或 *conda* 环境。这里，使用 jq 会更合适，因为 bash 函数总是可以工作的——假设 jq 已经被全局安装。

当事情变得越来越复杂时，应该从 bash 这样的简单脚本语言转向 python 这样的高级语言。高级语言可能会提高你的代码可读性，节省你编写和维护代码的时间，并且在捕捉/处理错误方面做得更好。

## 资源

*   启发了这篇文章的 StackOverflow 答案
*   [jq 手册](https://stedolan.github.io/jq/tutorial/)
*   Jonathan Cook 的 jq 教程,深入研究了 json 输入的整理