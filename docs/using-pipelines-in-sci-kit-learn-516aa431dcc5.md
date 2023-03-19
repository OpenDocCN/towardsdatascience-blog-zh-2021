# 停止一步一步地构建你的模型。利用管道实现流程自动化！

> 原文：<https://towardsdatascience.com/using-pipelines-in-sci-kit-learn-516aa431dcc5?source=collection_archive---------32----------------------->

## 在 Sci-kit 学习中使用管道

![](img/06118f93505a9d59dd517963f19fe771.png)

照片由 [Rodion Kutsaev](https://unsplash.com/@frostroomhead?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

# 为什么是管道？

当我开始在 Sklearn 中构建模型时，我会将每个预处理步骤分解到它的单元或代码块中。这是一个很好的开始方式，因为您可以很容易地将这些步骤分解成可读的块。然而，虽然它更容易阅读，但缺乏可重复性。下一次用新数据表示模型时，在对新数据运行模型之前，您必须运行转换数据的所有步骤，这带来了许多问题。例如，如果在一次热编码过程中创建了新的列，数据的维度可能会发生变化。

答案？[管道](https://scikit-learn.org/stable/modules/generated/sklearn.pipeline.Pipeline.html#sklearn.pipeline.Pipeline)

# 管道解剖

首先，像往常一样，导入需要运行这个脚本。

```
from sklearn.pipeline import Pipeline
from sklearn.compose import make_column_selector as selector
from sklearn.compose import ColumnTransformer
from sklearn. pre-processing import MinMaxScaler
from sklearn. pre-processing import OneHotEncoder
from sklearn.ensemble import RandomForestRegressor
```

# 柱式变压器

接下来是一个制作列转换器的函数。我更喜欢通过一个函数来调用它，以使代码更加可重用。

列转换器允许您将任意数量的预处理步骤合并到一个转换器中。在下面的例子中，我们有一个用于数字列的`MinMaxScaler`和一个用于分类值的`OneHotEncoder`。您可以在这些步骤中包括来自 Sklearn 的任何转换器。

他们的文档中有一个很好的例子:
[混合类型的柱式变压器](https://scikit-learn.org/stable/auto_examples/compose/plot_column_transformer_mixed_types.html#sphx-glr-auto-examples-compose-plot-column-transformer-mixed-types-py)

除了演示数字列和分类列之外，还展示了[列选择器](https://scikit-learn.org/stable/modules/generated/sklearn.compose.make_column_selector.html)，它允许您根据不同的标准选择列。通过`selector(dtype_exclude="object")`选择数字列。下面演示了通过名称选择列作为一个简单的 python 列表。您可以将这些精选的样式与您提供的不同变压器相结合。此外，您在下面命名您的变压器，如`num`和`cat; see`，以便稍后在您的 fit 模型中识别。

```
def make_coltrans():
    column_trans = ColumnTransformer(transformers=
            [('num', MinMaxScaler(), selector(dtype_exclude="object")),
             ('cat', OneHotEncoder(dtype='int', handle_unknown='ignore'), ['CAT_FIELD_ONE', 'CAT_FIELD_TWO'])],
            remainder='drop')

    return column_trans
```

# 管道

在创建列转换器之后，现在创建管道是一个非常简单的步骤。您所需要做的就是根据您通常采取的步骤的逻辑顺序对管道序列进行排序。这里我们有两个步骤，列转换器和分类器。我们在下面的列转换器中将这些步骤命名为`prep`和`clf`。

```
def create_pipe(clf):
    '''Create a pipeline for a given classifier.  
       The classifier needs to be an instance
       of the classifier with all parameters needed specified.'''

    # Each pipeline uses the same column transformer.  
    column_trans = make_coltrans()

    pipeline = Pipeline([('prep',column_trans),
                         ('clf', clf)])

    return pipeline
```

# 创建和拟合模型

最后，我们可以创建分类器的实例，并将其传递给上面创建管道的函数。

```
# Create the classifier instance and build the pipeline.
clf = RandomForestClassifier(random_state=42, class_weight='balanced')
pipeline = create_pipe(clf)

# Fit the model to the training data
pipeline.fit(X_train, y_train)
```

# 摘要

上面演示了设置管道的简单性。第一次这样做时，与独立完成每一步相比，可能会有些困惑。好处是，每当您想要将它应用到一个新模型，或者更好的是，针对您的 fit 模型运行新数据时，所有的数据转换都会自动发生。

如果你喜欢阅读这样的故事，并想支持我成为一名作家，可以考虑报名成为一名媒体成员。一个月 5 美元，让你可以无限制地访问成千上万篇文章。如果你使用[我的链接](https://medium.com/@broepke/membership)注册，我会赚一小笔佣金，不需要你额外付费。