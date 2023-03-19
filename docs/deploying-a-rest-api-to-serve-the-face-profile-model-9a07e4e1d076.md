# 部署 REST API 来服务人脸轮廓模型

> 原文：<https://towardsdatascience.com/deploying-a-rest-api-to-serve-the-face-profile-model-9a07e4e1d076?source=collection_archive---------33----------------------->

## 经过相当长时间的搜索，我们已经确定了几个产品，可以用来缓解将深度学习模型部署到生产中所需的提升。由于我们的最终应用程序并不明确(尽管我们有几个想法)，生产化该模型的最佳方法是将其部署为 REST 服务，该服务可以独立于生成每个模型的数据科学生命周期过程进行维护，并且对于每个数据科学家来说都足够简单。轻松部署 REST 端点的能力是模型随时间快速迭代的关键促成因素。Ubiops 是一个为深度学习服务提供托管服务的产品，运行在 Kubernetes 上，大大简化了创建和维护 REST 服务的过程。

![](img/3fbcad900861fc79d94cf685a6f6edc5.png)

来自托管面部轮廓模型的 REST 端点的输出。(图片由作者提供，原文:Schniter，Eric，& Shields，Timothy。(2020).参与者面临重复的囚徒困境[数据集]。芝诺多。[http://doi.org/10.5281/zenodo.4321821](http://doi.org/10.5281/zenodo.4321821)

# 介绍

在本文中，我们使用 IPython Jupyter 笔记本来部署一个带有 [Ubiops](https://ubiops.com/) 的 REST 端点。本演示中使用的模型是我们在[2020–10–12](https://blog.prcvd.ai/research/perception/image/2020/10/12/Training-a-Face-Segmentation-Model-for-Automatic-Skin-Tone-Detection.html)博客中开发的面部轮廓。该模型是一个面部轮廓，旨在作为一种工具来控制表型属性的影响，这些表型属性可以从肖像风格的图像(有脸的图片)中观察到。REST 端点将支持底层模型的持续开发，独立于基于它构建的应用程序。发送的大部分字节将是来自经过训练的 Unet 分段模型的权重，大小为 165MB，经过 pickle 压缩。演示的最后一步展示了开发客户如何使用终端来获取面部特征。

通常使用的 UbiOps 的一些替代方案是 AWS Lambda 或其他类似的无服务器产品，自托管的“有状态”Flask 应用程序，以及在 MLOps 旗帜下销售的其他有前途的解决方案，如 Streamlit，Seldon，FastAPI，特别是提供“无服务器 GPU”产品的 Algorithmia。模型服务的关键问题是:它需要低成本，因为我们正在引导一个产品。它需要提供一个编程 API 和围绕模型发布和消费的常识性行为。最后，它需要使用 Jupyter 笔记本作为源

**本教程的代码可以在这里找到:** [**Github Repo**](https://github.com/free-soellingeraj/prod-mod-face-profile)

# 部署现有模型

自述文件对用于部署此端点的不同工件有更全面的描述。这些步骤可以在 Ubiops 文档和 [Ubiops 食谱](https://github.com/UbiOps/cookbook)中找到。这个例子略有不同，因为一个需求是将笔记本作为代码的记录系统。在 cookbook 教程中，笔记本代码由 python 模块填充，而在本教程中，python 模块是使用[Fastai-nb dev](https://nbdev.fast.ai/)项目从笔记本构建的。基本模式是:

![](img/9db89bed35c125e53b3de5b0f87ae9b4.png)

从带有 Ubiops 的 Jupyter 笔记本电脑部署。(*图片作者*)

## **高级功能**

实施包括几个步骤。首先，我们创建了`Deployment`类。`Deployment`类的实现结构是由 Ubiops 定义的，最好的地方是在[指南](https://github.com/UbiOps/cookbook)中。`Deployment`类成为端点的入口点，它的所有依赖项都可以在`deployment_package`中得到考虑。我们压缩`deployment_package`，然后使用 Ubiops API 注册部署请求的输入和输出。最后，我们将使用 API 将您的`deployment_package` zip 存档推送到 Ubiops。下面点击了高点，但是完整的代码在[**Github Repo**](https://github.com/free-soellingeraj/prod-mod-face-profile)**中找到。**

## **部署类**

```
import sys; import os
from pathlib import Path
import io
import jsonimport skimage
from skimage import color
import matplotlib.pyplot as plt
import numpy as npfrom prcvd.img.core import (
    TrainedSegmentationModel, MaskedImg
)
from prcvd.img.face import (
    FacialProfile
)
from prcvd.core import json_to_dictclass Deployment:    
    def __init__(self, base_directory:str, context):
        """
        Initialisation method for the deployment. It can for 
        example be used for loading modules that have to be kept in
        memory or setting up connections. Load your external model 
        files (such as pickles or .h5 files) here.
        :param str base_directory: absolute path to the directory 
            where the deployment.py file is located
        :param dict context: a dictionary containing details of the 
            deployment that might be useful in your code.
            It contains the following keys:
                - deployment (str): name of the deployment
                - version (str): name of the version
                - input_type (str): deployment input type, either 
                    'structured' or 'plain'
                - output_type (str): deployment output type, either 
                    'structured' or 'plain'
                - language (str): programming language the deployment 
                    is running
                - environment_variables (str): the custom environment 
                    variables configured for the deployment.
        You can also access those as normal environment variables via os.environ
        """
        print("Loading face segmentation model.")
        self.basedir = Path(base_directory)
        self.mod_fp = self.basedir/'model1.pkl'
        self.output_classes = [
            'Background/undefined', 'Lips', 'Eyes', 'Nose', 'Hair', 
            'Ears', 'Eyebrows', 'Teeth', 'General face', 'Facial hair',
            'Specs/sunglasses'
        ]
        with open(Path(self.basedir)/'ubiops_output_spec.json') as f:
            self.output_spec = json.load(f)self.output_mapping = {
            obj['name']: obj['id']
            for obj
            in self.output_spec
        }
        self.size = 224
        self.model = TrainedSegmentationModel(
            mod_fp=self.mod_fp, 
            input_size=self.size,
            output_classes=self.output_classes
        )
```

上面，我们将模型加载到内存中。Ubiops 警告说，这一步发生在端点空闲一段时间之后，所谓的冷启动可能需要一些时间。`base_directory`是`deployment_package`的根级别，您可以看到，当`Deployment`被实例化时，我们加载了该文件夹中的几个文件。下一部分是定义调用`model.predict`的`Deployment.request`。

```
def request(self, data, attempt=1):
        """
        Method for deployment requests, called separately for each individual request.
        :param dict/str data: request input data. In case of deployments with structured data, a Python dictionary
            with as keys the input fields as defined upon deployment creation via the platform. In case of a deployment
            with plain input, it is a string.
                - img: str, file path
                - sampling_strategy: str, 'use_all' | ...
                - align_face: bool, yes/no apply face alignment
                - num_attempts: int, max attempts before failure (sometimes face alignment fails)

        :return dict/str: request output. In case of deployments with structured output data, a Python dictionary
            with as keys the output fields as defined upon deployment creation via the platform. In case of a deployment
            with plain output, it is a string. In this example, a dictionary with the key: output.
        """
        img = MaskedImg()
        img.load_from_file(data['img'])

        try:
            profile = FacialProfile(
                model=self.model, 
                img=img, 
                sampling_strategy=data['sampling_strategy'], 
                align_face=data['align_face']
            )

        except:
            if not attempt > data['num_attempts']:
                return self.request(
                    data=data,
                    attempt=attempt+1,
                )
            else:
                return None, None## .. make/save the plot .. (see github for full code)

        row = profile.get_profile()
        row['model_id'] = str(self.mod_fp) # for ubiops output type str
        row = {**row, **{'img': str(outfp)}}

        return {self.output_mapping[k]: v for k,v in row.items()}
```

`model.predict`使用用户提供的输入来计算面部轮廓，并将其与原始图像一起可视化(见报告)。可以将文件保存到当前工作目录，Ubiops 会负责清理它们。另一方面，如果您需要保存作为输入提交的图像，您需要将该代码添加到请求中。

## 创建部署包

发送到 Ubiops 的是一个包含`Deployment`类和依赖项的压缩文件。如果您有任何私有依赖项，例如私有回购中不在 PiPy 上的包，您可以手动将它们添加到`deployment_package`中。下面的部分是在笔记本中完成的，但也可以从命令行完成。

**打包部署类**

下一部分可以从笔记本或命令行运行。它将笔记本代码(`mod`)生成的模块转换为 Ubiops 私有依赖。必要的部分是`setup.py`和`mod`文件夹，其中包含特定于该部署的 python 模块。我们删除了`.git`目录和笔记本，因为我们想最小化`deployment_package`的大小。

```
cp -r .. ../../prod-mod-face-profile-cp
rm -rf ../../prod-mod-face-profile-cp/deployment_package/
rm -rf ../../prod-mod-face-profile-cp/*.zip
rm -rf ../deployment_package/libraries
mkdir ../deployment_package/libraries
mv ../../prod-mod-face-profile-cp ../deployment_package/libraries/prod-mod-face-profile
rm -rf ../deployment_package/libraries/prod-mod-face-profile/.git/
```

**获取任何其他私有依赖关系**

我们需要获取任何其他私有库来打包这个部署。这个库包含了所有操纵经过训练的深度学习模型的代码，以及计算人脸轮廓的代码。

```
rm -rf ../deployment_package/libraries/prcvd
cd ../deployment_package/libraries && git clone [@github](/{user}:{pw}<a href=).com/prcvd/prcvd.git">https://{user}:{pw}[@github](http://twitter.com/github).com/prcvd/prcvd.git
cd ../deployment_package/libraries/prcvd && git checkout {branch}
rm -rf ../deployment_package/libraries/prcvd/.git/
```

> 提示:删除模块不需要的文件。
> 
> 警告:为了在 git repo 中不提交保存的冗余文件的情况下工作，我们不提交`deployment_package`和其他大型工件，如`*pkl`、`*pth`和`*zip`。更多详情见`.gitignore`底部

## 部署

您可以使用 Ubiops API 部署`deployment_package.zip`。第一部分是定义一个“部署”,如下面的代码所示。其主要部分是定义名称和数据类型输入以及来自`Deployment.request`的输出。我选择在单独的文件中管理它们，因为除了在这里定义部署之外，它们还会在几个地方使用。这是允许的 Ubiops 数据类型列表。我选择将输出中的每一项都作为单独的输出对象。在连接到 Ubiops Python API 之前，创建一个登录和项目是很重要的。

**配置 Ubiops API 连接**

这一步需要您用 Ubiops UI 手动获取一个 [api ke](https://ubiops.com/docs/organizations/service-users/) y，并创建一个新项目。一旦您将 api 密匙存储在安全的地方，您就可以连接了。

```
client = ubiops.ApiClient(
    ubiops.Configuration(
        api_key={'Authorization': os.getenv('API_TOKEN')}, 
        host='[https://api.ubiops.com/v2.1'](https://api.ubiops.com/v2.1'))
)
api = ubiops.CoreApi(client)deployment_template = ubiops.DeploymentCreate(
    name='my_deployment',
    description='description',
    input_type='structured',
    output_type='structured',
    input_fields=[
        ubiops.DeploymentInputFieldCreate(
            name=str(obj['name']),
            data_type=obj['data_type']['value'])
        for obj in input_spec
    ],
    output_fields=[
        ubiops.DeploymentOutputFieldCreate(
            name=str(obj['id']),
            data_type=obj['data_type']['value'])
        for obj in output_spec
    ],
    labels={'demo': 'mod'}
)
```

一旦部署存在，就会创建一个部署`Version`，在这里定义集群细节(例如，最大实例数、内存、空闲时间等..)，如下:

```
version_template = ubiops.VersionCreate(
    version='v1',
    language='python3.6',
    memory_allocation=3000,
    minimum_instances=0,
    maximum_instances=1,
    maximum_idle_time=1800 # = 30 minutes
)
```

最后，我们可以用下面的代码将`deployment_package.zip`推送到 UbiOps:

```
file_upload_result = api.revisions_file_upload(
    project_name='face-profile',
    deployment_name='my_deployment',
    version='v1',
    file=zipfp
)
```

值得查看 Github 中的笔记本，其中包含完整的部署代码。在那里，它展示了如何利用 Ubiops API 生成的一些异常来做一些基本的，但自动升级的事情。

> 提示:`deployment_name`和`version`标识符可以通过笔记本中的代码来控制。

**检查部署**

可以使用 API 来检查部署的状态，这需要反复询问状态。或者，用户可以在 Ubiops UI 中监控状态和日志。部署的日志如下所示:

![](img/a74d8e0561067d08387f4ffd19f80a76.png)

Ubiops 登录用户界面(*作者图片*)

# Python 请求客户端

一旦模型被部署，它简化了使用，因为现在不需要在本地加载模型，我们只需要调用一个 REST 端点。这就避免了安装依赖的问题，在我的经验中，安装依赖是阻止我的模型被他们的目标用户使用的最令人沮丧的问题。呼叫可以在一行中完成，如下所示:

```
client = UbiOpsClient(
    project_name='face-profile', 
    deployment_name='my_deployment', 
    deployment_version='v1',
    input_spec='./ubiops_input_spec.json',
    output_spec='./ubiops_output_spec.json',
    api_key=os.getenv('API_TOKEN'), 
    api_host=os.getenv('HOST')
)#  >> out
{'0000': (52, 47, 49),
 '0001': (133, 86, 112),
 '0002': (179, 144, 159),
 '0003': (33, 28, 34),
 '0004': (106, 10, 34),
 '0005': (118, 102, 117),
 '0006': None,
 '0007': (144, 122, 132),
 '0008': None,
 '0009': None,
 '0010': (97, 87, 107),
 '0011': (76, 72, 92),
 '0012': (196, 163, 176),
 '0013': (145, 122, 133),
 '0014': (188, 157, 166),
 '0015': 3.4990597042834075,
 '0016': 6,
 '0017': -0.0,
 '0018': 1.596774193548387,
 '0019': 99.0,
 '0020': 62.0,
 '0021': 18828,
 '0022': 0.018748672190354792,
 '0023': 0.04110898661567878,
 '0024': 0.30359039728064585,
 '0025': 0.0014871468026343743,
 '0026': 0.013702995538559592,
 '0027': 0.0,
 '0028': 0.2953579774803484,
 '0029': 0.0,
 '0030': 0.0,
 '0031': 0.006107924367962609,
 '0032': 0.0053112385808370514,
 '0033': 0.06161036753770979,
 '0034': 0.09693010410027618,
 '0035': 0.15604418950499258,
 '0036': '/home/fortville/code/prod-mod-face-profile/deployment_package/model1.pkl',
 '0037': '/home/fortville/code/prod-mod-face-profile/deployment_package/tmp.jpeg'}
```

查看模型的输入/输出

![](img/685800556f9dde6b0715a51b0b53c730.png)

在右边，看到输入图像。在左边，可以看到输出。可以将手动编码器绘制的线条(黑色实线和黑色虚线)与 PRCVD 绘制的线条进行比较。AI 算法(实心红和实心绿)(*作者图片*)

> 提示:在过去，使用不同的技术，我将“请求客户端”代码保存在它自己的模块中。请求者通常是拥有不同权限的数据科学家，而不是创建部署的工程师。

# 结论

总的来说，UbiOps 实现了简化 REST API 服务部署的承诺，并且提供了一个相当慷慨的免费层。UbiOps 接受单个部署类，并提供一个简单的目录结构来支持其库依赖关系。在本教程中，我们将讨论 jupyter 笔记本电脑和 UbiOps 云之间持续集成的概念验证。用户可以在 jupyter 笔记本中以半自动方式进行更改，包括更改模型重量、测试这些更改和部署。通常编写部署类的文件通过从笔记本创建的模块导入而成为标准文件。如果没有这个特性，用户将需要在笔记本中维护代码的一个版本(例如“原型”)，并在生产中在 python 模块中维护另一个版本(或者只是一个 python 文件)。就像我用过的其他生产技术，比如 AWS Lambda，有一个学习曲线，我必须对工具进行部分逆向工程，这需要我稍微重新思考我的原型设计的几个方面。

未来的工作包括集成更多的 Github 动作，这样,`deployment_package.zip`的创建和测试可以由新的 PR commit 或合并到主分支来触发。事实上，Ubiops 最近有一篇文章描述了如何与 github actions 集成:[这里](https://ubiops.com/docs/tutorials/git-sync/)

本教程中的所有代码都可以在[这里](https://github.com/free-soellingeraj/prod-mod-face-profile)找到