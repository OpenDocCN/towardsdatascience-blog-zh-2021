# TransparentPath:一个管理 Google 云存储路径的 Python 包

> 原文：<https://towardsdatascience.com/transparentpath-a-python-package-to-manage-paths-on-google-cloud-storage-ae25a15a44d5?source=collection_archive---------14----------------------->

> 你是否习惯了 pathlib 的 Path 对象，在使用 GCSFileSystem 对象时感到沮丧？透明路径包是为你做的。

![](img/96bfa1f812ac22e8c574e732c6ccc265.png)

由[凯尔·苏杜](https://unsplash.com/@ksudu94?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

# 它有什么用途，如何使用它

当我第一次开始使用谷歌云平台(GCP)时，我面临着以下困难:用 Python 代码轻松地从/向谷歌云存储(GCS)读取和写入文件。对于本地文件，我习惯于使用 [Pathlib](https://docs.python.org/3/library/pathlib.html) 库，通过重载 *_truediv__* 以允许 bash 式的路径创建，并通过实现许多有用的方法，如 *glob* 、 *ls* 、 *unlink* …可以直接在 path 对象上调用，这使得使用路径变得非常简单和直观。Python 中允许在 GCS 上使用路径的类是 gcsfs 包中的 G [CSFileSystem](https://gcsfs.readthedocs.io/en/latest/) ，它没有所有这些便利的特性，因为主对象不是文件而是文件系统。

因此，我创建了 [TransparentPath](https://pypi.org/project/transparentpath/) 对象，包装在 GCSFileSystem **和** LocalFileSystem 中，允许使用与 PathLib 相同的逻辑处理 Path 对象，但是在 GCS 上。我添加了对 LocalFileSystem 的支持，这样，仅通过这个类，就可以在相同的代码中轻松地使用远程和本地文件。

这里有一个小例子，说明最初使用 Pathlib 开发的本地工作代码如何被重构为使用 GCSFileSystem，然后如何使用 TransparentPath 开发它。代码在包含 **csv** 和 **parquet** 文件的目录中循环，读取包含的数据帧，将它们连接起来并保存在一个新文件中。

## 使用 Pathlib:

```
from pathlib import Path
import pandas as pd
first_path = Path*(*"foo"*)* / "bar"

dfs = *[]* dic = None

for subpath in first_path.glob*(*"*"*)*:
    if subpath.suffix == ".csv":
        df = pd.read_csv*(*subpath, index_col=0, parse_dates=True*)* dfs.append*(*df*)* elif subpath.suffix == ".parquet":
        df = pd.read_parquet*(*subpath*)* dfs.append*(*df*)* df = pd.concat*(*dfs*)* second_path = first_path.parent / "concatenated.csv"
df.to_csv*(*second_path*)*
```

## **使用 GCSFileSystem** :

```
from gcsfs import GCSFileSystem
import pandas as pd

dfs = *[]* dic = None

fs = GCSFileSystem*()* first_path = "gs://bucket_name/foo/bar"

for subpath in fs.glob*(*"/".join*([*first_path, "*"*]))*:
    if subpath.endswith*(*".csv"*)*:
        df = pd.read_csv*(*"".join*([*"gs://", subpath*])*, index_col=0, parse_dates=True*)* dfs.append*(*df*)* elif subpath.endswith*(*".parquet"*)*:
        df = pd.read_parquet*(*"".join*([*"gs://", subpath*]))* dfs.append*(*df*)* df = pd.concat*(*dfs*)* second_path = "gs://bucket_name/foo/concatenated.csv"
df.to_csv*(*second_path*)*
```

## **使用透明路径**:

```
from transparentpath import TransparentPath as Path
import pandas as pd

Path.set_global_fs*(*"gcs", bucket="bucket_name"*)* first_path = Path*(*"foo"*)* / "bar"

dfs = *[]* dic = None

for subpath in first_path.glob*(*"*"*)*:
    df = subpath.read*(*index_col=0, parse_dates=True*)* dfs.append*(*df*)* df = pd.concat*(*dfs*)* second_path = *(*first_path.parent / "concatenated.csv"*)* second_path.write*(*df*)*
```

如果将行 *Path.set_global_fs("gcs "，bucket="bucket_name")* 注释掉，所有操作都在本地进行，而不是在 gcs 上进行，这意味着**您可以轻松地从本地文件切换到远程文件**。在这个例子中，GCS 上的树与项目目录中的树相同，这简化了代码。但即使不是这样，在代码的开头用一个单独的 *if* 语句来指定代码是在本地运行还是在 GCS 上运行，就可以定义一个可以在任何地方使用的根路径。

此外，请注意 *gs://* 没有出现，并且在创建路径时没有给出 bucket 名称。文件系统由 *set_global_fs* 设置，允许定义由所有后续路径共享的桶。您仍然可以使用下面两行中的任何一行在不同的存储桶上创建路径:

```
first_path = Path*(*"gs://other_bucket_name/foo"*)* / "bar"
first_path = Path*(*"foo", bucket="other_bucket_name"*)* / "bar"
first_path = Path*(*"other_bucket_name/foo"*)* / "bar"
```

# 资格证书

将自动检测 GC 的凭据，优先级顺序如下:

*   环境变量 GOOGLE_APPLICATION_CREDENTIALS 已设置并指向有效的*。json* 文件。
*   您安装了有效的 Cloud SDK。在这种情况下，您可能会看到警告: *UserWarning:您的应用程序已经使用来自 Google Cloud SDK 的最终用户凭证进行了身份验证，没有配额项目。由你来决定如何处理它。*
*   运行代码的机器本身就是一台 GCP 机器

请注意，关联帐户需要有权列出项目中的存储桶，因为 TransparentPath 将尝试检查项目中是否存在 *set_global_fs* 中所需的存储桶。

# 可用的方法

## 文件系统、路径库和字符串

文件系统对象中所有可用的方法都存在于透明路径中。区别在于不需要指定应用它们的路径:

```
fs.glob*(*"/".join*([*first_path, "*"*])  # GCSFileSystem
path.glob("*")  # TransparentPath*
```

*Pathlib* 中所有可用的方法都存在于 TransparentPath 中:

```
path.parent
path.name
path.suffix
...
```

*str* 中可用的所有方法都存在于 TransparentPath 中:

```
>>> "foo*" in (TransparentPath("gs://bucket_name") / "foo")
True*
```

为了使这些方法可用，TransparentPath 不实现它们。当调用该类未知的方法时，它将通过以下方式检查该方法是否已知:

*   文件系统(GCS 或本地，取决于路径)
*   Pathlib
*   潜艇用热中子反应堆（submarine thermal reactor 的缩写）

并使用带有适当参数的此方法。例如，当使用 *glob* 时，它不是透明路径的一个方法，该类首先为它的文件系统对象查找它是否存在。它有，所以它使用它。 *glob* 也存在于 *Pathlib* 中，但是由于该方法最早是在文件系统中发现的，所以永远不会使用。

## 阅读和写作

在 TransparentPath 中读写 Pandas 数据帧和系列真的很容易:

```
p = Path("gs://bucket_name/file.csv")
df = path.read(index_col=0, parse_dates=True)
p = Path("gs://bucket_name/file.parquet")
p.write(df)
```

根据文件后缀，该类调用适当的 pandas 读取方法。支持与熊猫一起阅读的格式有 **csv** 、**拼花**、 **hdf5** 、 **xlsx** 、 **xls** 、 **xlsm** 。你也可以在字典中读到一个 json。如果后缀都不是，这个类就认为文件包含纯文本，并使用文件系统的 *open* 方法读取它。

内置的 *open* 被该类重载，以允许在 TransparentPaths 对象或以“gs://”开头的字符串上使用它:

```
# Both commands will have the same effect
with open("gs://bucket_name/file.txt", "r") as f:
    ...with open(Path("gs://bucket_name/file.txt"), "r") as f:
    ...
```

透明路径也可以处理 Dask 数据帧:

```
import pandas as pd
import dask.dataframe as dd
df_dask = dd.from_pandas*(* pd.DataFrame*(* columns=*[*"foo", "bar"*]*,
        index=*[*"a", "b"*]*,
        data=*[[*1, 2*]*, *[*3, 4*]]
    )*,
    npartitions=1
*)* 
pfile = Path("gs://bucket_name/file.parquet")
pfile.write*(*df_dask*)  # detects that the object is Dask dataframe* df_dask = pfile.read*(*use_dask=True*)  # need to tell to use Dask*
```

## 复制和移动

*cp* 和 *mv* 方法可用，将对文件和目录都有效:

```
p1 = Path("foo/", bucket="bucket_name", fs="gcs")
p2 = Path("foo/", fs="local")
p1.cp(p2)  # Copies the item from GCS to local
```

## 结论

该软件包仍然是新的，并定期更新。稳定版通过`pip install transparentpath`提供给 Python 3.8 及以上版本，测试版通过`pip install transparentpath-nightly`提供。任何问题都可以在 project [Github](https://github.com/Advestis/transparentpath) 页面上提交。在这里，您还可以找到一个自述文件，其中包含更多关于如何使用该类的信息和示例。

## 关于我们

Advestis 是一家欧洲合同研究组织(CRO ),对统计学和可解释的机器学习技术有着深刻的理解和实践。Advestis 的专长包括复杂系统的建模和时间现象的预测分析。

*领英*:[https://www.linkedin.com/company/advestis/](https://www.linkedin.com/company/advestis/)