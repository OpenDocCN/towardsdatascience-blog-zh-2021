# Python 的 Boto3 简介

> 原文：<https://towardsdatascience.com/introduction-to-pythons-boto3-c5ac2a86bb63?source=collection_archive---------6----------------------->

## AWS SDK 简单易行

![](img/a9e77c57c8d05fc1783bdb566a2fd3c1.png)

图片来自 Unsplash.com，作者: [@azizayad](https://unsplash.com/@azizayad)

自从在一家可以访问 AWS 和云的初创公司工作以来，我不得不学习一些新工具。这是加入创业公司最有趣的事情之一！一开始，我很自然地开始在我的本地机器上做数据科学。然而，随着数据资产的建立和新人的加入，我不得不做出改变。我开始利用我们的 AWS 云环境。更具体地说，我需要访问亚马逊 S3。我一步一步摸索着 stackoverflow，浏览了几个教程。最后我学到了很多。我想我应该分享一些我一路走来学到的东西，希望能帮助别人。

在这篇文章中，我将解释一些关于 [Boto3](https://boto3.amazonaws.com/v1/documentation/api/latest/index.html) 、python 的 AWS SDK 和亚马逊 S3 的高级概念。我将提供[一步一步的 python 代码](https://github.com/aviolante/amazon_s3_boto3_blog_post)来做一些基本的，但是在尝试遍历和访问 S3 的信息时非常有用的事情。让我们跳进来吧！

## **首先要做的事情**

1.  你需要安装 Boto3: `pip install boto3`。
2.  接下来，您需要在本地机器上一个名为“aws”的隐藏文件夹中创建一个凭证文件。从技术上讲，您可以直接传递您的凭证，而不是创建一个文件，但我不建议这样做。

*   打开一个终端，`cd`进入这样的目录路径(这是 Mac 用的):`/Users/your_name`。
*   创建一个名为“aws”的隐藏文件夹:`mkdir .aws`。
*   `cd`进入新的隐藏文件夹，用你喜欢的任何文本编辑器创建一个空白文件。我用 vim，所以对我来说只是`vim credentials`。
*   进入文件后，插入以下项目并保存:

```
[default]
aws_access_key_id = your_access_key
aws_secret_access_key = your_secret_access_key
region = your_region
```

您可以在 AWS 文档中找到如何找到[您的访问密钥](https://docs.aws.amazon.com/powershell/latest/userguide/pstools-appendix-sign-up.html)或[特定区域代码](https://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/Concepts.RegionsAndAvailabilityZones.html)。一旦你安装了 Boto3 并且设置了你的证书，你就可以连接到 S3 了。

## **低水平和高水平 API**

Boto3 中有几个不同的 API 与 AWS 接口。分别是:`client()`和`resource()`。您需要知道的是，客户端是 AWS 的一个*"低级接口，其方法与服务 API 的映射接近 1:1 "*，资源是一个*"比服务客户端发出的原始、低级调用更高级别的抽象"*【1】*。*要了解更多信息，这个 [stackoverflow 答案](https://stackoverflow.com/questions/42809096/difference-in-boto3-between-resource-client-and-session)给出了每个 API 的详细描述。对于这篇文章，我将使用`resource()`，因为它很好地抓住了基本要素，但如果你认为自己需要更多，那么较低层次的`client()`可能是最好的。如果你想在下面的代码中使用`client()`，你需要做一些小的调整。据我所见，这两个 API 之间只有细微的差别，至少我在这里展示的是这样。

## 亚马逊 S3 桶系统

一个目录系统，就像你的本地机器，是一个层次系统，你可能有文件夹或者子文件夹的深层目录路径，比如:`/Users/your_name_most_likely/folder/sub_folder1/sub_folder2/...`。亚马逊 S3 不使用目录系统。它是一个完全扁平的结构，有桶和物体。对象存储在一个桶中。当您查看 bucket 中的对象时，就像我们下面将要看到的，它看起来像一个目录或文件夹系统，但它只是一个单一的路径或文件夹对象。看起来像这样的原因是因为 S3 使用所谓的前缀和分隔符作为组织工具。因此，根据[文档](https://docs.aws.amazon.com/AmazonS3/latest/dev/ListingKeysHierarchy.html)，您可能想要创建对象路径，如下图所示，使用前缀`North America/USA/`来限制结果并只返回 2 个对象路径。这就是在存储桶和对象级别进行组织的方式。这需要一点时间来适应，但过一会儿感觉就一样了。

```
Europe/France/Nouvelle-Aquitaine/Bordeaux
North America/Canada/Quebec/Montreal
North America/USA/Washington/Bellevue
North America/USA/Washington/Seattle
```

## Boto3 中的一些基本步骤和命令

*   连接到 Amazon s3

只要上面的凭证文件已经创建，您就应该能够连接到您的 S3 对象存储。

```
import boto3s3_client = boto3.resource('s3')
```

*   创建和查看存储桶

创建存储桶时，有许多内容可以配置(位置约束、读访问、写访问等)，您可以[使用客户端 API](https://boto3.amazonaws.com/v1/documentation/api/latest/reference/services/s3.html?highlight=create_bucket#S3.Client.create_bucket) 来实现。在这里，我们仍然使用上面代码块中的高级 API `resource()`。创建新时段后，现在让我们查看 S3 所有可用的时段。一定要注意[水桶命名限制](https://docs.aws.amazon.com/AmazonS3/latest/dev/BucketRestrictions.html)(对所有蛇案粉丝表示歉意！).

```
# create a bucket with given name
andre_bucket = s3_client.create_bucket(Bucket='andre.gets.buckets')# view buckets in s3
>>> for bucket in s3_client.buckets.all():
...     print(bucket.name)andre.gets.buckets
data.staging.us.east.1
data.lake.us.east.1
website.images.production
website.images.staging
```

你可以看到新创建的桶`andre.gets.buckets`(小篮球为你双关！).

*   检视时段内的物件

现在我们已经创建了一个桶，让我们向其中添加一些对象，然后查看特定桶中的所有对象。

```
# point to bucket and add objects
andre_bucket.put_object(Key='andre/added/a/new/object1')
andre_bucket.put_object(Key='andre/added/a/new/object2')
andre_bucket.put_object(Key='andre/added/a/new/object3')# view objects within a bucket
>>> for obj in andre_bucket.objects.all():
...     print(obj.key)andre/added/a/new/object1
andre/added/a/new/object2
andre/added/a/new/object3
```

真棒。我们所有的新对象都在我们的存储桶中找到。现在，让我们面对现实吧！让我们从 S3 桶上传、下载和删除一些数据。

*   上载、下载和删除对象

让我们上传我桌面上的一个 CSV 文件。然后，我们将再次查看存储桶中的对象，以查看是否显示新数据。

```
# upload local csv file to a specific s3 bucket
local_file_path = '/Users/andreviolante/Desktop/data.csv'
key_object = 'andre/added/a/new/object/data.csv'

andre_bucket.upload_file(local_file_path, key_object)>>> for obj in andre_bucket.objects.all():
...     print(obj.key)andre/added/a/new/object/data.csv
andre/added/a/new/object1
andre/added/a/new/object2
andre/added/a/new/object3
```

成交。我们刚刚将一个数据文件上传到了 S3 桶`andre_bucket`中，并将其组织在了前缀`andre/added/a/new/object`下，这对于你来说可能是更有意义的事情。

现在，让我们下载相同的 CSV 文件，以展示其功能。我仍然将`key_object`作为我的数据所在的“目录”。

```
# download an s3 file to local machine
filename = 'downloaded_s3_data.csv'

andre_bucket.download_file(key_object, filename)
```

太棒了。现在，我可以在桌面上看到一个名为`downloaded_s3_data.csv`的新文件。

最后，让我们删除其中一些对象。有两种方法可以真正做到这一点:1)删除特定对象或 2)删除存储桶中的所有对象。我们会双管齐下。

```
# delete a specific object
andre_bucket.Object('andre/added/a/new/object2').delete()>>> for obj in andre_bucket.objects.all():
...     print(obj.key)andre/added/a/new/object/data.csv
andre/added/a/new/object1
andre/added/a/new/object3
```

我们刚刚移除了对象`andre/added/a/new/object2`。现在，让我们把剩下的去掉。

```
# delete all objects in a bucket
andre_bucket.objects.delete()>>> for obj in andre_bucket.objects.all():
...     print(obj.key)>>>
```

一个很好的单一行删除所有对象内的桶！现在，由于我们有一个空桶，让我们展示如何删除一个桶。

```
# delete specific bucket
andre_bucket.delete()>>> for bucket in s3_client.buckets.all():
...     print(bucket.name)data.staging.us.east.1
data.lake.us.east.1
website.images.production
website.images.staging
```

相当简单。这是一个重要的过程，因为**除非桶为空**，否则不能删除桶。如果尝试，您将看到如下错误:`The Bucket you tried to delete is not empty`。

## 结论

恭喜你。我们成功地使用 Boto3(用于 AWS 的 Python SDK)来访问亚马逊 S3。简单回顾一下，我们连接到亚马逊 S3，遍历桶和对象，创建桶和对象，上传和下载一些数据，然后最终删除对象和我们的桶。这些有帮助的日常命令应该让你快速启动并运行 S3，同时给你足够的知识去谷歌其他任何东西。快乐云穿越！

## 参考

1.  [客户](https://boto3.amazonaws.com/v1/documentation/api/latest/guide/clients.html)和[资源](https://boto3.amazonaws.com/v1/documentation/api/latest/guide/resources.html)文档
2.  Boto3 文档[此处](https://boto3.amazonaws.com/v1/documentation/api/latest/index.html)
3.  [Git Repo](https://github.com/aviolante/amazon_s3_boto3_blog_post) 及所有附带代码
4.  查看[我的其他故事](https://medium.com/@violante.andre)！