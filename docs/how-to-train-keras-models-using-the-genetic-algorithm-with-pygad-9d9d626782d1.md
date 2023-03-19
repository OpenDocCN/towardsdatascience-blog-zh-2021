# 如何用 PyGAD 遗传算法训练 Keras 模型

> 原文：<https://towardsdatascience.com/how-to-train-keras-models-using-the-genetic-algorithm-with-pygad-9d9d626782d1?source=collection_archive---------19----------------------->

[PyGAD](https://pygad.readthedocs.io) 是一个开源的 Python 库，用于构建遗传算法和训练机器学习算法。它提供了广泛的参数来定制遗传算法，以处理不同类型的问题。

PyGAD 拥有自己的模块，支持构建和训练神经网络(NNs)和卷积神经网络(CNN)。尽管这些模块运行良好，但它们是在 Python 中实现的，没有任何额外的优化措施。这导致即使是简单的问题也需要相对较长的计算时间。

从[PyGAD](https://pygad.readthedocs.io)2 . 8 . 0(2020 年 9 月 20 日发布)开始，一个名为`kerasga`的新模块支持训练 Keras 模型。尽管 Keras 是用 Python 构建的，但速度很快。原因是 Keras 使用 TensorFlow 作为后端，TensorFlow 高度优化。

本教程讨论如何使用 [PyGAD](https://pygad.readthedocs.io/) 训练 Keras 模型。讨论内容包括使用顺序模型或函数式 API 构建 Keras 模型、构建 Keras 模型参数的初始群体、创建合适的适应度函数等等。

您也可以跟随本教程中的代码，并在来自 [ML Showcase](https://ml-showcase.paperspace.com/projects/genetic-algorithm-with-pygad) 的渐变社区笔记本上免费运行它。

完整的教程大纲如下:

*   开始使用 [PyGAD](https://pygad.readthedocs.io)
*   `pygad.kerasga`模块
*   使用 [PyGAD](https://pygad.readthedocs.io) 训练 Keras 模型的步骤
*   确定问题类型
*   创建一个 Keras 模型
*   实例化`pygad.kerasga.KerasGA`类
*   准备培训数据
*   损失函数
*   适应度函数
*   生成回调函数(可选)
*   创建一个`pygad.GA`类的实例
*   运行遗传算法
*   健身与世代图
*   有关已定型模型的统计信息
*   回归的完整代码
*   使用 CNN 分类的完整代码

让我们开始吧。

![](img/d1073f3bfac3308ed91d17249eb6ae85.png)

图片来自 Unsplash:[https://unsplash.com/photos/zz_3tCcrk7o](https://unsplash.com/photos/zz_3tCcrk7o)

# PyGAD 入门

要开始本教程，安装 PyGAD 是必不可少的。如果您已经安装了 PyGAD，检查`__version__`属性以确保根据下一个代码至少安装了 PyGAD 2.8.0。

```
import pygadprint(pygad.__version__)
```

可以从 [PyPI (Python 包索引)](https://pypi.org/project/pygad)获得，然后可以使用`pip`安装程序进行安装。确保安装 PyGAD 2.8.0 或更高版本。

```
pip install pygad>=2.8.0
```

![](img/116a213e4248f221fda5fad7f0793261.png)

[PyGAD 标志](https://pygad.readthedocs.io)

安装完成后，您就可以开始了。阅读文档[阅读文档](https://pygad.readthedocs.io):[pygad . readthedocs . io](https://pygad.readthedocs.io)。该文档包括一些示例。

下一段代码解决了一个简单的优化线性模型参数的问题。

```
import pygad
import numpyfunction_inputs = [4,-2,3.5,5,-11,-4.7] # Function inputs.
desired_output = 44 # Function output.def fitness_func(solution, solution_idx):
    output = numpy.sum(solution*function_inputs)
    fitness = 1.0 / (numpy.abs(output - desired_output) + 0.000001)
    return fitnessnum_generations = 100
num_parents_mating = 10
sol_per_pop = 20
num_genes = len(function_inputs)ga_instance = pygad.GA(num_generations=num_generations,
                       num_parents_mating=num_parents_mating,
                       fitness_func=fitness_func,
                       sol_per_pop=sol_per_pop,
                       num_genes=num_genes)ga_instance.run()ga_instance.plot_result()
```

# `pygad.kerasga`模块

从 PyGAD 2.8.0 开始，引入了一个名为`kerasga`的新模块。它的名字是 KerasGgenic**A**算法的简称。该模块提供以下功能:

*   使用`KerasGA`类构建解决方案的初始群体。每个解决方案都包含 Keras 模型中的所有参数。
*   使用`model_weights_as_vector()`功能将 Keras 模型的参数表示为染色体(即 1D 向量)。
*   使用`model_weights_as_matrix()`功能从染色体中恢复 Keras 模型的参数。

`pygad.kerasga`模块有一个名为`KerasGA`的类。这个类的构造函数接受两个参数:

1.  `model`:Keras 车型。
2.  `num_solutions`:群体中解的数量。

基于这两个参数，`pygad.kerasga.KerasGA`类创建了 3 个实例属性:

1.  `model`:对 Keras 模型的引用。
2.  `num_solutions`:群体中解的数量。
3.  `population_weights`:保存模型参数的嵌套列表。该列表在每一代之后更新。

假设 Keras 模型保存在`model`变量中，下一段代码创建一个`KerasGA`类的实例，并将其保存在`keras_ga`变量中。`num_solutions`参数被赋值为 10，这意味着群体有 10 个解。

构造函数创建一个长度等于`num_solutions`参数值的列表。列表中的每个元素在使用`model_weights_as_vector()`函数转换成 1D 向量后，为模型的参数保存不同的值。

基于`KerasGA`类的实例，初始群体可以从`population_weights`属性返回。假设模型有 60 个参数，有 10 个解，那么初始种群的形状就是`10x60`。

```
import pygad.kerasgakeras_ga = pygad.kerasga.KerasGA(model=model,
                                 num_solutions=10)initial_population = keras_ga.population_weights
```

下一节总结了使用 PyGAD 训练 Keras 模型的步骤。每一个步骤都将在单独的章节中讨论。

# 使用 PyGAD 训练 Keras 模型的步骤

使用 PyGAD 训练 Keras 模型的步骤总结如下:

*   确定问题类型
*   创建一个 Keras 模型
*   实例化`pygad.kerasga.KerasGA`类
*   准备培训数据
*   损失函数
*   适应度函数
*   生成回调函数(可选)
*   创建一个`pygad.GA`类的实例
*   运行遗传算法

接下来的小节将讨论这些步骤。

# 确定问题类型

问题类型(分类或回归)有助于准备以下内容:

1.  损失函数(用于构建适应度函数)。
2.  Keras 模型中的输出图层。
3.  训练数据。

对于回归问题，损失函数可以是平均绝对误差、均方误差或本页[中列出的另一个函数](https://keras.io/api/losses/regression_losses)，该页总结了回归的 Keras 损失函数:[keras.io/api/losses/regression_losses](https://keras.io/api/losses/regression_losses)。

对于分类问题，损失函数可以是二进制交叉熵(对于二进制分类)、分类交叉熵(对于多类问题)，或者在[本页](https://keras.io/api/losses/probabilistic_losses)中列出的另一个函数，其总结了 Keras 分类损失函数:[keras.io/api/losses/probabilistic_losses](https://keras.io/api/losses/probabilistic_losses/)。

输出层中的激活函数根据问题是分类还是回归而不同。对于分类问题，与回归的**线性**相比，它可能是 **softmax** 。

如果问题是回归，那么每个样本的输出相对于分类问题中的类标签是一个连续的数。

总之，确定问题的类型以便正确选择训练数据和损失函数是至关重要的。

# 创建一个 Keras 模型

构建 Keras 模型有 [3 种方式](https://keras.io/api/models):

1.  [时序模型](https://keras.io/guides/sequential_model)
2.  [功能 API](https://keras.io/guides/functional_api)
3.  [模型子类化](https://keras.io/guides/model_subclassing)

PyGAD 支持使用顺序模型和函数式 API 构建 Keras 模型。

## 顺序模型

对于顺序模型，这里有一个构建 Keras 模型的例子。简单地说，使用`tensorflow.keras.layers`模块创建每一层。然后，创建一个`tensorflow.keras.Sequential`类的实例。最后，使用`add()`方法将图层添加到模型中。

```
import tensorflow.kerasinput_layer  = tensorflow.keras.layers.Input(3)
dense_layer1 = tensorflow.keras.layers.Dense(5, activation="relu")
output_layer = tensorflow.keras.layers.Dense(1, activation="linear")model = tensorflow.keras.Sequential()
model.add(input_layer)
model.add(dense_layer1)
model.add(output_layer)
```

请注意，输出层的激活函数是`linear`，这意味着问题是回归。对于一个分类问题，函数可以是`softmax`。在下一行中，输出层有 2 个神经元(每个类 1 个),它使用`softmax`激活函数。

```
output_layer = tensorflow.keras.layers.Dense(2, activation="linear")
```

## 功能 API

对于功能性 API 案例，每一层通常都是作为顺序模型案例创建的。每一层，或者输入层，都被用作一个接受前一层作为参数的函数。最后，创建了一个`tensorflow.keras.Model`类的实例，它接受输入和输出层作为参数。

```
input_layer  = tensorflow.keras.layers.Input(3)
dense_layer1 = tensorflow.keras.layers.Dense(5, activation="relu")(input_layer)
output_layer = tensorflow.keras.layers.Dense(1, activation="linear")(dense_layer1)model = tensorflow.keras.Model(inputs=input_layer, outputs=output_layer)
```

创建 Keras 模型后，下一步是使用`KerasGA`类创建 Keras 模型参数的初始群体。

# 实例化`pygad.kerasga.KerasGA`类

通过创建一个`pygad.kerasga.KerasGA`类的实例，就创建了一个 Keras 模型参数的初始群体。下一段代码将前一节中创建的 Keras 模型传递给`KerasGA`类构造函数的`model`参数。

```
import pygad.kerasgakeras_ga = pygad.kerasga.KerasGA(model=model,
                                 num_solutions=10)
```

下一节将创建用于训练 Keras 模型的训练数据。

# 准备培训数据

基于问题的类型(分类或回归)，准备训练数据。

对于有 1 个输出的回归问题，这里有一个随机生成的训练数据，其中每个样本有 3 个输入。

```
import numpy
​
# Data inputs
data_inputs = numpy.array([[0.02, 0.1, 0.15],
                           [0.7, 0.6, 0.8],
                           [1.5, 1.2, 1.7],
                           [3.2, 2.9, 3.1]])
​
# Data outputs
data_outputs = numpy.array([[0.1],
                            [0.6],
                            [1.3],
                            [2.5]])
```

对于 XOR 这样的二元分类问题，下面是它的训练数据。每个样本有 2 个输入。准备输出，以便输出层有 2 个神经元，每个类一个。

```
import numpy
​
# XOR problem inputs
data_inputs = numpy.array([[0, 0],
                           [0, 1],
                           [1, 0],
                           [1, 1]])
​
# XOR problem outputs
data_outputs = numpy.array([[1, 0],
                            [0, 1],
                            [0, 1],
                            [1, 0]])
```

下一节讨论回归和分类问题的损失函数。

# 损失函数

损失函数因问题类型而异。本节讨论 Keras 的`tensorflow.keras.losses`模块中用于回归和分类问题的一些损失函数。

## 回归

对于回归问题，损失函数包括:

*   `tensorflow.keras.losses.MeanAbsoluteError()`
*   `tensorflow.keras.losses.MeanSquaredError()`

查看[本页](https://keras.io/api/losses/regression_losses)了解更多信息。

下面是一个计算平均绝对误差的例子，其中`y_true`和`y_pred`代表真实输出和预测输出。

```
mae = tensorflow.keras.losses.MeanAbsoluteError()
loss = mae(y_true, y_pred).numpy()
```

## 分类

对于分类问题，损失函数包括:

*   `tensorflow.keras.losses.BinaryCrossentropy()`:二元分类。
*   `tensorflow.keras.losses.CategoricalCrossentropy()`:多级分类。

查看[本页](https://keras.io/api/losses/probabilistic_losses)了解更多信息。

下面是一个计算二元类熵的例子:

```
bce = tensorflow.keras.losses.BinaryCrossentropy()
loss = bce(y_true, y_pred).numpy()
```

基于损失函数，根据下一部分准备适应度函数。

# 适应度函数

分类或回归问题的损失函数都是最小化函数。遗传算法的适应度函数是最大化函数。因此，适应值是作为损失值的倒数来计算的。

```
fitness_value = 1.0 / loss
```

用于计算模型的适应值的步骤如下:

1.  从 1D 向量恢复模型参数。
2.  设置模型参数。
3.  做预测。
4.  计算损失值。
5.  计算适应值。
6.  返回适应值。

## 回归适合度

下一段代码构建了完整的适应度函数，它与 PyGAD 一起处理回归问题。PyGAD 中的 fitness 函数是一个常规的 Python 函数，它必须接受两个参数。第一个表示要计算适应值的解。另一个参数是群体内解的指数，这在某些情况下可能是有用的。

传递给适应度函数的解是 1D 向量。为了从这个向量恢复 Keras 模型的参数，使用了`pygad.kerasga.model_weights_as_matrix()`。

```
model_weights_matrix = pygad.kerasga.model_weights_as_matrix(model=model, weights_vector=solution)
```

一旦参数被恢复，那么它们就被`set_weights()`方法用作模型的当前参数。

```
model.set_weights(weights=model_weights_matrix)
```

基于当前参数，模型使用`predict()`方法预测输出。

```
predictions = model.predict(data_inputs)
```

预测输出用于计算损失值。平均绝对误差被用作损失函数。

```
mae = tensorflow.keras.losses.MeanAbsoluteError()
```

因为损失值可能是 **0.0** ，那么最好像`0.00000001`一样给它加上一个小值，避免在计算适应值时跳水归零。

```
solution_fitness = 1.0 / (mae(data_outputs, predictions).numpy() + 0.00000001)
```

最后，返回适应值。

```
def fitness_func(solution, sol_idx):
    global data_inputs, data_outputs, keras_ga, model
​
    model_weights_matrix = pygad.kerasga.model_weights_as_matrix(model=model,
                                                                 weights_vector=solution)
​
    model.set_weights(weights=model_weights_matrix)
​
    predictions = model.predict(data_inputs)

    mae = tensorflow.keras.losses.MeanAbsoluteError()
    solution_fitness = 1.0 / (mae(data_outputs, predictions).numpy() + 0.00000001)
​
    return solution_fitness
```

## 二元分类的适合度

对于二进制分类问题，这里有一个适用于 PyGAD 的适应度函数。假设分类问题是二元的，它计算二元交叉熵。

```
def fitness_func(solution, sol_idx):
    global data_inputs, data_outputs, keras_ga, model
​
    model_weights_matrix = pygad.kerasga.model_weights_as_matrix(model=model,
                                                                 weights_vector=solution)
​
    model.set_weights(weights=model_weights_matrix)
​
    predictions = model.predict(data_inputs)

    bce = tensorflow.keras.losses.BinaryCrossentropy()
    solution_fitness = 1.0 / (bce(data_outputs, predictions).numpy() + 0.00000001)
​
    return solution_fitness
```

下一节将构建一个在每代结束时执行的回调函数。

# 生成回调函数(可选)

对于每一代，遗传算法对解进行改变。在每一次生成完成后，可以调用一个回调函数来计算一些关于最新达到的参数的统计数据。

这一步是可选的，仅用于调试目的。

生成回调函数实现如下。在 PyGAD 中，这个回调函数必须接受一个引用遗传算法实例的参数，通过该参数可以使用`population`属性获取当前群体。

在这个函数中，一些信息被打印出来，比如当前的代数和最佳解的适应值。这种信息使用户能够通过遗传算法的进展来更新。

```
def callback_generation(ga_instance):
    print("Generation = {generation}".format(generation=ga_instance.generations_completed))
    print("Fitness    = {fitness}".format(fitness=ga_instance.best_solution()[1]))
```

# 创建一个`pygad.GA`类的实例

使用 PyGAD 训练 Keras 模型的下一步是创建一个`pygad.GA`类的实例。这个类的构造函数接受许多参数，这些参数可以在[文档](https://pygad.readthedocs.io/en/latest/README_pygad_ReadTheDocs.html#init)中找到。

下一段代码通过使用该应用程序中最少的参数传递来实例化`pygad.GA`类，这些参数是:

*   `num_generations`:世代数。
*   `num_parents_mating`:要交配的亲本数量。
*   `initial_population`:Keras 模型参数的初始群体。
*   `fitness_func`:健身功能。
*   `on_generation`:生成回调函数。

请注意，在`KerasGA`类的构造函数中，群体中的解的数量先前被设置为 10。因此，要交配的亲本数量必须少于 10 个。

```
num_generations = 250
num_parents_mating = 5
initial_population = keras_ga.population_weights
​
ga_instance = pygad.GA(num_generations=num_generations, 
                       num_parents_mating=num_parents_mating, 
                       initial_population=initial_population,
                       fitness_func=fitness_func,
                       on_generation=callback_generation)
```

下一部分运行遗传算法来开始训练 Keras 模型。

# 运行遗传算法

`pygad.GA`类的实例通过调用`run()`方法来运行。

```
ga_instance.run()
```

通过执行这个方法，PyGAD 的生命周期按照下图开始。

![](img/9ae61bc8e3c162d269e9e2ac04fbb75d.png)

[PyGAD 生命周期](https://pygad.readthedocs.io)。版权归作者所有。

下一节讨论如何对训练好的模型得出一些结论。

# 健身与世代图

使用`pygad.GA`类中的`plot_result()`方法，PyGAD 创建了一个图形，显示了适应值是如何逐代变化的。

```
ga_instance.plot_result(title="PyGAD & Keras - Iteration vs. Fitness", linewidth=4)
```

# 有关已定型模型的统计信息

`pygad.GA`类有一个名为`best_solution()`的方法，它返回 3 个输出:

1.  找到最佳解决方案。
2.  最佳解决方案的适应值。
3.  群体中最佳解决方案的索引。

下一段代码调用`best_solution()`方法并输出最佳解决方案的信息。

```
solution, solution_fitness, solution_idx = ga_instance.best_solution()
print("Fitness value of the best solution = {solution_fitness}".format(solution_fitness=solution_fitness))
print("Index of the best solution : {solution_idx}".format(solution_idx=solution_idx))
```

下一段代码从最佳解决方案中恢复 Keras 模型的权重。基于恢复的权重，该模型预测训练样本的输出。您还可以预测新样本的输出。

```
# Fetch the parameters of the best solution.
best_solution_weights = pygad.kerasga.model_weights_as_matrix(model=model,
                                                              weights_vector=solution)
model.set_weights(best_solution_weights)
predictions = model.predict(data_inputs)
print("Predictions : \n", predictions)
```

假设使用的损失函数是平均绝对误差，下一个代码计算它。

```
mae = tensorflow.keras.losses.MeanAbsoluteError()
abs_error = mae(data_outputs, predictions).numpy()
print("Absolute Error : ", abs_error)
```

接下来的部分列出了使用 PyGAD 构建和训练 Keras 模型的完整代码。

# 回归的完整代码

对于一个使用平均绝对误差作为损失函数的回归问题，这里是它的完整代码。

```
import tensorflow.keras
import pygad.kerasga
import numpy
import pygad
​
def fitness_func(solution, sol_idx):
    global data_inputs, data_outputs, keras_ga, model
​
    model_weights_matrix = pygad.kerasga.model_weights_as_matrix(model=model,
                                                                 weights_vector=solution)
​
    model.set_weights(weights=model_weights_matrix)
​
    predictions = model.predict(data_inputs)
​
    mae = tensorflow.keras.losses.MeanAbsoluteError()
    abs_error = mae(data_outputs, predictions).numpy() + 0.00000001
    solution_fitness = 1.0 / abs_error
​
    return solution_fitness
​
def callback_generation(ga_instance):
    print("Generation = {generation}".format(generation=ga_instance.generations_completed))
    print("Fitness    = {fitness}".format(fitness=ga_instance.best_solution()[1]))
​
input_layer  = tensorflow.keras.layers.Input(3)
dense_layer1 = tensorflow.keras.layers.Dense(5, activation="relu")(input_layer)
output_layer = tensorflow.keras.layers.Dense(1, activation="linear")(dense_layer1)
​
model = tensorflow.keras.Model(inputs=input_layer, outputs=output_layer)
​
weights_vector = pygad.kerasga.model_weights_as_vector(model=model)
​
keras_ga = pygad.kerasga.KerasGA(model=model,
                                 num_solutions=10)
​
# Data inputs
data_inputs = numpy.array([[0.02, 0.1, 0.15],
                           [0.7, 0.6, 0.8],
                           [1.5, 1.2, 1.7],
                           [3.2, 2.9, 3.1]])
​
# Data outputs
data_outputs = numpy.array([[0.1],
                            [0.6],
                            [1.3],
                            [2.5]])
​
num_generations = 250
num_parents_mating = 5
initial_population = keras_ga.population_weights
​
ga_instance = pygad.GA(num_generations=num_generations, 
                       num_parents_mating=num_parents_mating, 
                       initial_population=initial_population,
                       fitness_func=fitness_func,
                       on_generation=callback_generation)
ga_instance.run()
​
# After the generations complete, some plots are showed that summarize how the outputs/fitness values evolve over generations.
ga_instance.plot_result(title="PyGAD & Keras - Iteration vs. Fitness", linewidth=4)
​
# Returning the details of the best solution.
solution, solution_fitness, solution_idx = ga_instance.best_solution()
print("Fitness value of the best solution = {solution_fitness}".format(solution_fitness=solution_fitness))
print("Index of the best solution : {solution_idx}".format(solution_idx=solution_idx))
​
# Fetch the parameters of the best solution.
best_solution_weights = pygad.kerasga.model_weights_as_matrix(model=model,
                                                              weights_vector=solution)
model.set_weights(best_solution_weights)
predictions = model.predict(data_inputs)
print("Predictions : \n", predictions)
​
mae = tensorflow.keras.losses.MeanAbsoluteError()
abs_error = mae(data_outputs, predictions).numpy()
print("Absolute Error : ", abs_error)
```

代码完成后，下一张图显示适应值在增加，这是一个好迹象，因为 Keras 模型正在正确学习。

![](img/8f8453074838dffaec9520dfbfec9462.png)

图片来自 [PyGAD](https://pygad.readthedocs.io) 文档。

以下是关于训练模型的更多细节。请注意，预测值接近正确值。平均相对误差为 0.018。

```
Fitness value of the best solution = 54.79189095217631
Index of the best solution : 0
Predictions : 
[[0.11471477]
 [0.6034051 ]
 [1.3416876 ]
 [2.486804  ]]
Absolute Error :  0.018250866
```

# 使用 CNN 分类的完整代码

下一个代码使用 Keras 构建一个卷积神经网络，用于对 80 幅图像的数据集进行分类，其中每幅图像的大小为`100x100x3`。注意，使用分类交叉熵是因为数据集有 4 个类。

可以从以下链接下载培训数据:

1.  [dataset _ inputs . npy](https://github.com/ahmedfgad/NumPyCNN/blob/master/dataset_inputs.npy):[https://github . com/ahmedfgad/NumPyCNN/blob/master/dataset _ inputs . npy](https://github.com/ahmedfgad/NumPyCNN/blob/master/dataset_inputs.npy)
2.  [dataset _ outputs . npy](https://github.com/ahmedfgad/NumPyCNN/blob/master/dataset_outputs.npy):[https://github . com/ahmedfgad/NumPyCNN/blob/master/dataset _ outputs . npy](https://github.com/ahmedfgad/NumPyCNN/blob/master/dataset_outputs.npy)

```
import tensorflow.keras
import pygad.kerasga
import numpy
import pygad
​
def fitness_func(solution, sol_idx):
    global data_inputs, data_outputs, keras_ga, model
​
    model_weights_matrix = pygad.kerasga.model_weights_as_matrix(model=model,
                                                                 weights_vector=solution)
​
    model.set_weights(weights=model_weights_matrix)
​
    predictions = model.predict(data_inputs)
​
    cce = tensorflow.keras.losses.CategoricalCrossentropy()
    solution_fitness = 1.0 / (cce(data_outputs, predictions).numpy() + 0.00000001)
​
    return solution_fitness
​
def callback_generation(ga_instance):
    print("Generation = {generation}".format(generation=ga_instance.generations_completed))
    print("Fitness    = {fitness}".format(fitness=ga_instance.best_solution()[1]))
​
# Build the keras model using the functional API.
input_layer = tensorflow.keras.layers.Input(shape=(100, 100, 3))
conv_layer1 = tensorflow.keras.layers.Conv2D(filters=5,
                                             kernel_size=7,
                                             activation="relu")(input_layer)
max_pool1 = tensorflow.keras.layers.MaxPooling2D(pool_size=(5,5),
                                                 strides=5)(conv_layer1)
conv_layer2 = tensorflow.keras.layers.Conv2D(filters=3,
                                             kernel_size=3,
                                             activation="relu")(max_pool1)
flatten_layer  = tensorflow.keras.layers.Flatten()(conv_layer2)
dense_layer = tensorflow.keras.layers.Dense(15, activation="relu")(flatten_layer)
output_layer = tensorflow.keras.layers.Dense(4, activation="softmax")(dense_layer)
​
model = tensorflow.keras.Model(inputs=input_layer, outputs=output_layer)
​
keras_ga = pygad.kerasga.KerasGA(model=model,
                                 num_solutions=10)
​
# Data inputs
data_inputs = numpy.load("dataset_inputs.npy")
​
# Data outputs
data_outputs = numpy.load("dataset_outputs.npy")
data_outputs = tensorflow.keras.utils.to_categorical(data_outputs)
​
num_generations = 200
num_parents_mating = 5
initial_population = keras_ga.population_weights
​
ga_instance = pygad.GA(num_generations=num_generations, 
                       num_parents_mating=num_parents_mating, 
                       initial_population=initial_population,
                       fitness_func=fitness_func,
                       on_generation=callback_generation)
​
ga_instance.run()
​
ga_instance.plot_result(title="PyGAD & Keras - Iteration vs. Fitness", linewidth=4)
​
# Returning the details of the best solution.
solution, solution_fitness, solution_idx = ga_instance.best_solution()
print("Fitness value of the best solution = {solution_fitness}".format(solution_fitness=solution_fitness))
print("Index of the best solution : {solution_idx}".format(solution_idx=solution_idx))
​
# Fetch the parameters of the best solution.
best_solution_weights = pygad.kerasga.model_weights_as_matrix(model=model,
                                                              weights_vector=solution)
model.set_weights(best_solution_weights)
predictions = model.predict(data_inputs)
# print("Predictions : \n", predictions)
​
# Calculate the categorical crossentropy for the trained model.
cce = tensorflow.keras.losses.CategoricalCrossentropy()
print("Categorical Crossentropy : ", cce(data_outputs, predictions).numpy())
​
# Calculate the classification accuracy for the trained model.
ca = tensorflow.keras.metrics.CategoricalAccuracy()
ca.update_state(data_outputs, predictions)
accuracy = ca.result().numpy()
print("Accuracy : ", accuracy)
```

下图显示了适应值是如何逐代演变的。只要适应度值增加，那么就增加代数，以达到更好的精度。

![](img/a0dfbf2f5572f7510cb6783f1ac0e342.png)

图片来自 [PyGAD](https://pygad.readthedocs.io) 文档。

以下是有关已训练模型的一些信息。

```
Fitness value of the best solution = 2.7462310258668805
Categorical Crossentropy :  0.3641354
Accuracy :  0.75
```

**本文原载于** [**Paperspace 博客**](https://blog.paperspace.com/train-keras-models-using-genetic-algorithm-with-pygad) **。你可以在渐变** **上免费运行我的教程的代码** [**。**](https://gradient.paperspace.com/)

# 结论

本教程讨论了如何使用名为 [PyGAD](https://pygad.readthedocs.io) 的 Python 3 库，使用遗传算法来训练 Keras 模型。Keras 模型可以使用顺序模型或函数式 API 来创建。

使用`pygad.kerasga`模块，创建 Keras 模型权重的初始群体，其中每个解决方案为模型保存一组不同的权重。这个种群随后按照 [PyGAD](https://pygad.readthedocs.io) 的生命周期进化，直到所有世代完成。

由于 Keras 后端 TensorFlow 的高速特性， [PyGAD](https://pygad.readthedocs.io) 可以在合理的时间内训练复杂的架构。