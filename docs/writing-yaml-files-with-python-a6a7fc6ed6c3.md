# 用 python 编写 YAML 文件

> 原文：<https://towardsdatascience.com/writing-yaml-files-with-python-a6a7fc6ed6c3?source=collection_archive---------0----------------------->

## 使用 ruamel.yaml 注释配置文件

*本文期望读者对* [*python 编程语言*](https://docs.python.org/3/) *有一个基本的了解。*

![](img/56cb0c26116a90d83247360882a03099.png)

https://pxhere.com/en/photo/492303

Yaml 文件已经存在了二十多年了。它们对于显示数据结构非常有用，并且是配置文件的一种流行格式。从 [AWS CloudFormation](https://aws.amazon.com/cloudformation/) 、[公共工作流语言](https://www.commonwl.org/user_guide/)到[家庭助手](https://www.home-assistant.io/) ⁴，yamls 是互联网配置文件的基本支柱。它们比其他格式(如 [JSON](https://en.wikipedia.org/wiki/JSON) )更容易阅读，支持注释并易于版本控制——试着在两个略有不同的 JSON 文件上运行`git merge`,你会明白我的意思。尽管如此，当涉及到用我们钟爱的语言 python 编写 yaml 文件时，我们的选择相当有限。只有 [PyYAML](https://pyyaml.org/wiki/PyYAMLDocumentation) 和 [ruamel.yaml](https://yaml.readthedocs.io/en/latest/) 在 [yaml 一致性测试矩阵](https://matrix.yaml.io/)中有注册表，其中只有 ruamel.yaml 支持处理注释。

如果用户可能需要在稍后阶段手动修改配置文件，那么能够将注释写入配置文件是非常有用的。另一个优势是 enum 支持，通过向 yaml 文件添加注释，用户可以看到某个设置的所有可用选项，而不必滚动文档来找到它们！更好的是，配置文件中的注释可以让用户包括到文档的链接，在那里他们可以阅读配置文件的那个部分！

因此，让我们开始通过 python 编写一些 yaml

## 一个示例 yaml 文件:

```
employees:
  - name: Jeffrey Bezos
    job title: CEO
    annual salary (USD): 1000000000000
  - name: John Smith
    job title: factory worker
    annual salary (USD): 20000
```

这个 yaml 文件相当于 python 中的一个字典，只有一个关键字*“*employees ”,包含两个元素的列表。
嵌套列表中的每个元素包含三个相同的关键字:“姓名”、“职位”和“年薪(美元)”。

yaml 文件很酷的一点是能够使用“#”符号对它们进行注释

```
employees:
  # Start with CEO
  - name: Jeffrey Bezos  # Goes by Jeff
    job title: CEO # ...entrepreneur, born in 1964... 
    annual salary (USD): 1000000000000  # This is too much
  # List of factory workers below
  - name: John Smith
    job title: factory worker
    annual salary (USD): 20000  # Probably deserves a raise
```

## 安装 ruamel.yaml

下面我们将尝试用 ruamel.yaml 将这个 yaml 文件加载到 python 中。如果你需要安装 ruamel.yaml，我们将使用版本`0.17.4`，这里有一些命令可以帮助你。

安装选项 1:

```
pip install ruamel.yaml==0.17.4
```

安装选项 2:

```
conda install -c conda-forge ruamel.yaml==0.17.4
```

## 将 ruamel 导入 python 并加载 yaml 文件

```
# Imports
from ruamel.yaml.main import round_trip_load as yaml_loademployees_dict = yaml_load("""
employees:
  # Start with CEO
  - name: Jeffrey Bezos  # Goes by Jeff
    job title: CEO # / Entrepreneur, born in 1964...
    annual salary (USD): 1000000000000  # This is too much
  # List of factory workers below
  - name: John Smith
    job title: factory worker
    annual salary (USD): 20000  # Probably deserves a raise
""")print(employees_dict)
print(f"Type: {type(employees_dict}")
```

输出:

```
ordereddict([('employees', [ordereddict([('name', 'Jeffrey Bezos'), ('job title', 'CEO'), ('annual salary (USD)', 1000000000000)]), ordereddict([('name', 'John Smith'), ('job title', 'factory worker'), ('annual salary (USD)', 20000)])])])
Type: <class 'ruamel.yaml.comments.CommentedMap'>
```

酷！这很简单。现在让我们试着从头开始构建一个 yaml 文件！

# 从头开始构建 yaml 文件

## 让我们首先设置我们的导入和全局变量

```
# Regular imports
from copy import deepcopy

# Yaml loaders and dumpers
from ruamel.yaml.main import \
    round_trip_load as yaml_load, \
    round_trip_dump as yaml_dump

# Yaml commentary
from ruamel.yaml.comments import \
    CommentedMap as OrderedDict, \
    CommentedSeq as OrderedList

# For manual creation of tokens
from ruamel.yaml.tokens import CommentToken
from ruamel.yaml.error import CommentMark# Globals
# Number of spaces for an indent 
INDENTATION = 2 
# Used to reset comment objects
tsRESET_COMMENT_LIST = [None, [], None, None]
```

并使用`OrderedDict` 和`OrderedList`属性创建一个购物清单。

```
shopping_list = OrderedDict({
    "Shopping List": OrderedDict({
        "eggs": OrderedDict({
            "type": "free range",
            "brand": "Mr Tweedy",
            "amount": 12
        }),
        "milk": OrderedDict({
            "type": "pasteurised",
            "litres": 1.5,
            "brands": OrderedList([
                "FarmFresh",
                "FarmHouse gold",
                "Daisy The Cow"
            ])
        })
    })
})# Dump the yaml file
print(yaml_dump(shopping_list, preseve_quotes=True))
```

输出:

```
Shopping List:
  eggs:
    type: free range
    brand: Mr Tweedy
    amount: 12
  milk:
    type: pasteurised
    litres: 1.5
    brands:
      - FarmFresh
      - FarmHouse gold
      - Daisy The Cow
```

创建我们的数据结构是相当乏味的。我们希望写入 yaml 的数据结构也不太可能是这种格式。它更有可能是标准的字典和/或列表格式。没关系，我们可以用`dump`和`load`来代替，将标准字典和列表格式转换成我们的 OrderedList ( `CommentedMap`)和 OrderedList ( `CommentedSeq`)。

```
# Original object
shopping_list = {
    "Shopping List": {
        "eggs": {
            "type": "free range",
            "brand": "Mr Tweedy",
            "amount": 12
        },
        "milk": {
            "type": "pasteurised",
            "litres": 1.5,
            "brands": [
                "FarmFresh",
                "FarmHouse gold",
                "Daisy The Cow"
            ]
        }
    }
}

# To yaml object
shopping_list = yaml_load(yaml_dump(shopping_list), preseve_quotes=True)# Show object type
print(type(shopping_list))
```

输出:

```
<class 'ruamel.yaml.comments.CommentedMap'>
```

完美！

## 向 yaml 文件添加标题

我们可以使用`yaml_set_start_comment`属性在地图对象上添加注释

```
shopping_list.yaml_set_start_comment("Shopping Lists for date: "
                                     "23 Oct 2021")
print(yaml_dump(shopping_list, 
                indent=INDENTATION, 
                block_seq_indent=INDENTATION))
```

输出:

```
# Shopping Lists for date: 21 Oct 2021
Shopping List:
  eggs:
    type: free range
    brand: Mr Tweedy
    amount: 12
  milk:
    type: pasteurised
    litres: 1.5
    brands:
      - FarmFresh
      - FarmHouse gold
      - Daisy The Cow
```

## 在嵌套键前添加注释

假设我们真的不想忘记鸡蛋，让我们在鸡蛋键上方写上“请不要忘记鸡蛋！”。由于`shopping_list`对象的‘购物清单’属性也是一个`OrderedDict`或`OrderedList`，我们可以在`Shopping List`属性上使用`yaml_set_comment_before_after_key`方法。

```
shopping_list.get("Shopping List").\
    yaml_set_comment_before_after_key(key="eggs",
                                      before="Please don't forget "
                                             "eggs!")
print(yaml_dump(shopping_list, 
                indent=INDENTATION, 
                block_seq_indent=INDENTATION))
```

输出:

```
# Shopping Lists for date: 21 Oct 2021
Shopping List:
#  Please don't forget eggs!
  eggs:
    type: free range
    brand: Mr Tweedy
    amount: 12
  milk:
    type: pasteurised
    litres: 1.5
    brands:
      - FarmFresh
      - FarmHouse gold
      - Daisy The Cow
```

呃，这可不理想！那个评论应该缩进。让我们删除它，并确保下次使用`indent`参数。要删除评论，我们需要重置`OrderedDict`的`ca`属性中的*蛋*键，然后重试。

## 删除注释对象

```
shopping_list.get("Shopping List").ca.\
    items["eggs"] = deepcopy(RESET_COMMENT_LIST)
print(yaml_dump(shopping_list, 
                indent=INDENTATION, 
                block_seq_indent=INDENTATION))
```

输出:

```
# Shopping Lists for date: 21 Oct 2021
Shopping List:
  eggs:
    type: free range
    brand: Mr Tweedy
    amount: 12
  milk:
    type: pasteurised
    litres: 1.5
    brands:
      - FarmFresh
      - FarmHouse gold
      - Daisy The Cow
```

让我们用`indent`参数(设置为 2 个空格)再试一次

```
object_depth = 1 # Shopping List

shopping_list.get("Shopping List").\
    yaml_set_comment_before_after_key(
        key="eggs",
        before="Don't forget the eggs",
        indent=object_depth*INDENTATION
    )print(yaml_dump(shopping_list, 
                indent=INDENTATION, 
                block_seq_indent=INDENTATION))
```

输出:

```
# Shopping Lists for date: 21 Oct 2021
Shopping List:
  # Don't forget the eggs
  eggs:
    type: free range
    brand: Mr Tweedy
    amount: 12
  milk:
    type: pasteurised
    litres: 1.5
    brands:
      - FarmFresh
      - FarmHouse gold
      - Daisy The Cow
```

## 在键后添加注释

我们也可以在一个键后添加注释

```
object_depth = 2 # Shopping List -> Eggs
shopping_list.get("Shopping List").\
    yaml_set_comment_before_after_key(
        key="eggs",
        after="Please don't forget eggs!",
        after_indent=object_depth*INDENTATION
    )
print(yaml_dump(shopping_list, 
                indent=INDENTATION, 
                block_seq_indent=INDENTATION))
```

输出:

```
# Shopping Lists for date: 21 Oct 2021
Shopping List:
  # Don't forget the eggs
  eggs:
    # Please don't forget eggs!
    type: free range
    brand: Mr Tweedy
    amount: 12
  milk:
    type: pasteurised
    litres: 1.5
    brands:
      - FarmFresh
      - FarmHouse gold
      - Daisy The Cow
```

## 让我们给一个键添加一个行尾注释

礼貌地提醒一下，两升牛奶太多了！

```
shopping_list.get("Shopping List").\
    get("milk").\
    yaml_add_eol_comment(
        key="litres",
        comment="2 litres is too much milk!"
    )print(yaml_dump(shopping_list, 
                indent=INDENTATION, 
                block_seq_indent=INDENTATION))
```

输出:

```
# Shopping Lists for date: 21 Oct 2021
Shopping List:
  # Don't forget the eggs
  eggs:
    # Please don't forget eggs!
    type: free range
    brand: Mr Tweedy
    amount: 12
  milk:
    type: pasteurised
    litres: 1.5  # 2 litres is too much milk!
    brands:
      - FarmFresh
      - FarmHouse gold
      - Daisy The Cow
```

## 在列表中添加注释

请注意，*农家金*和*奶牛雏菊*是最后的选择。通过在列表的农舍金牌项目的索引上使用 before 参数。对于列表中的注释，仅支持 before 参数。

```
object_depth = 3 # Shopping List -> Milk -> Brands
shopping_list.get("Shopping List").\
    get("milk").\
    get("brands").\
    yaml_set_comment_before_after_key(
        key=1, # "Farmhouse gold" is the second item in the list
        before="Last Resorts",
        indent=object_depth*INDENTATION
    )
print(yaml_dump(shopping_list, 
                indent=INDENTATION, 
                block_seq_indent=INDENTATION))
```

输出:

```
# Shopping Lists for date: 21 Oct 2021
Shopping List:
  # Don't forget the eggs
  eggs:
    type: free range
    brand: Mr Tweedy
    amount: 12
  milk:
    type: pasteurised
    litres: 1.5  # 2 litres is too much milk!
    brands:
      - FarmFresh
      # Last Resorts
      - FarmHouse gold
      - Daisy The Cow
```

## 向列表中的项目添加行尾注释

```
object_depth = 3 # Shopping List -> Milk -> Brands
shopping_list.get("Shopping List").\
    get("milk").\
    get("brands").\
    yaml_add_eol_comment(
        key=1, 
        comment="Too creamy"
    )
print(yaml_dump(shopping_list, 
                indent=INDENTATION, 
                block_seq_indent=INDENTATION))
```

输出:

```
TypeError: 'CommentToken' object is not iterable
```

哦不！这是一个[已知的 bug](https://sourceforge.net/p/ruamel-yaml/tickets/404/) ，我们将不得不使用 brands 对象的`ca`属性将其添加到列表中。

## ca 属性的细分:

让我们先看看我们在处理什么:

```
print(shopping_list.get("Shopping List").get("milk").get("brands").ca)
```

输出:

```
Comment(
  start=None,
  items={
    1: [None, [CommentToken('# Last Resorts\n', col: 6)], None, None]
  })
```

在`CommentedSeq`中只使用了 items 属性值中的前两个元素。第一个元素是 eol 注释，而第二个元素是 before 注释列表。对于`CommentedMap`，第一个元素代表“开始注释”，第二个元素是之前注释，第三个是 eol 注释，第四个是之后注释。本文的*资源*部分对此进行了总结。

## 手动添加 CommentToken 对象

我们需要将项目的第一个元素设置为我们想要的注释。
手动添加一个`CommentToken`对象时，我们以`\ #`为前缀，以一个新的行字符`\n`结束。
我们用一个最小的`CommentMark`对象初始化`start_mark`参数，并将`end_mark`参数设置为`None`。

```
shopping_list.get("Shopping List").\
    get("milk").\
    get("brands").ca.\
    items[1][0] = CommentToken(value=" # Too creamy\n",
                               start_mark=CommentMark(0),
                               end_mark=None)
print(yaml_dump(shopping_list, 
                indent=INDENTATION, 
                block_seq_indent=INDENTATION))
```

输出:

```
# Shopping Lists for date: 21 Oct 2021
Shopping List:
  # Don't forget the eggs
  eggs:
    type: free range
    brand: Mr Tweedy
    amount: 12
  milk:
    type: pasteurised
    litres: 1.5  # 2 litres is too much milk!
    brands:
      - FarmFresh
      # Last Resorts
      - FarmHouse gold  # Too creamy
      - Daisy The Cow
```

# 变通办法概述

由此可以看出 ruamel.yaml 还有一定的提升空间:

1.  在使用`yaml_set_comment_before_after_key`方法的 before 或 after 参数时，需要设置缩进。这也意味着在创建注释时需要知道嵌套有多深。
2.  如果在列表中的元素上已经存在 before 注释，则不能使用`yaml_add_eol_comment`方法。
3.  在我写这篇文章的时候，ruamel 的最新版本(0.17.16)有一个 bug，使得许多输出省略了许多注释。
4.  ruamel.yaml 文档中没有关于将注释写入 yaml 文件的内容。

## 对我的发现的小小咆哮

互联网最重要的配置格式结构之一怎么会没有一个来自世界上最流行的编程语言之一的完全受支持的解析器呢？我确信一定有成千上万的 python 开发者和我有着同样的想法。

在任何人急于发表评论之前，你应该向开发商 have⁵ ⁶ ⁷.提出这个问题这样做，我可以假设为什么在我之前有这么多人从来没有烦恼过。ruamel.yaml 由 [SourceForge](https://sourceforge.net/projects/ruamel-yaml/) 托管，当你访问这个网站时，你会觉得你已经进入了 90 年代(从 UX 的背景来看，90 年代是一个糟糕的地方)。与 GitHub 不同，SourceForge 不允许你在创建后编辑问题。界面笨拙，搜索算法在搜索现有问题时过于敏感(什么都会出现！)而且通知服务是古老的——它不是一个通知标签，你会在一个新的评论被添加到你的问题(包括你自己的)时收到一封电子邮件，或者你可以订阅一个 RSS 提要。

虽然 PyYAML 似乎已经重新获得了 maintenance⁸，但是当前的开发人员仍然处于一个尴尬的境地，许多软件依赖于 PyYAML 而没有指定 version⁹.更新代码以合并/导入 ruamel.yaml 中完成的许多工作，如支持注释、YAML 1.2 格式或其他常规改进，会有破坏向后兼容性的风险，因此会导致许多用户代码中断。对于那些没有对依赖项进行版本控制的人来说，这是一个很好的教训，但是我不确定每个人都这样认为。

我个人不知道正确的解决方案是什么，像 PyYAML2 这样的新名称可以继承 ruamel.yaml 的工作，同时保持对使用 set PyYAML 的人的向后兼容性？更多支持 ruamel.yaml？

这篇文章绝不是对 PyYAML 或 ruamel.yaml 的维护者的攻击。我一点也不羡慕他们，我真诚地希望更多的支持在路上。我还完全允许复制/编辑这段代码，并将其用作在 python 中处理 yaml 的文档。至少，我希望这篇文章至少对那些希望将有限的、粗糙的注释写入 yaml 文件的开发人员有用，并避免其他人通过反复试验对源代码进行逆向工程。

## 资源:

stack overflow:[https://stack overflow . com/questions/40704916/how-to-insert-a-comment-line-to-YAML-in-python-using-ruamel-YAML](https://stackoverflow.com/questions/40704916/how-to-insert-a-comment-line-to-yaml-in-python-using-ruamel-yaml)

my binder . org:[https://my binder . org/v2/gist/Alexis wl/7 Fe 1 B3 a 90d 86313 c 9785992 BD 0 ce 6 e 19/HEAD](https://mybinder.org/v2/gist/alexiswl/7fe1b3a90d86313c9785992bd0ce6e19/HEAD)

ruamel.yaml 文档:[https://yaml.readthedocs.io/en/latest/](https://yaml.readthedocs.io/en/latest/)

注释标记对象结构:

*   CommentedMap:
    0:用`yaml_set_start_comment`方法设置的注释。
    1:用`before`参数从`yaml_set_comment_before_after_key`方法设置的注释。
    2:来自`yaml_set_eol_comment`
    的注释集 3:来自带`after`参数的`yaml_set_comment_before_after_key`方法的注释集。
*   CommentedSeq:
    0:从`yaml_set_eol_comment`
    设置的注释 1:从带有`before`参数的`yaml_set_comment_before_after_key`方法设置的注释。
    2:未使用
    3:未使用

## 参考资料:

[1]:[https://yaml.org/about.html](https://yaml.org/about.html)
【2】:[https://docs . AWS . Amazon . com/AWS cloudformation/latest/user guide/template-formats . html](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/template-formats.html)
【3】:[https://www . commonwl . org/user _ guide/02-1st-example/index . html](https://www.commonwl.org/user_guide/02-1st-example/index.html)
【4】:[https://www.home-assistant.io/docs/configuration/yaml/](https://www.home-assistant.io/docs/configuration/yaml/)
【5】:[https://sourceforge.net/p/ruamel-yaml/tickets/400/](https://sourceforge.net/p/ruamel-yaml/tickets/400/)