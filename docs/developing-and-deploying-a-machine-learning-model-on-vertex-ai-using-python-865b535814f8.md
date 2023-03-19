# 使用 Python 在顶点人工智能上开发和部署机器学习模型

> 原文：<https://towardsdatascience.com/developing-and-deploying-a-machine-learning-model-on-vertex-ai-using-python-865b535814f8?source=collection_archive---------7----------------------->

## 编写让您的 MLOps 团队满意的培训渠道

编写让您的 MLOps 团队满意的 ML 管道:遵循模型代码和 Ops 代码之间清晰的责任分离。本文将向您展示如何做到这一点。

![](img/2ca4f118cfd6778d7f9d66e9b68dd581.png)

普里西拉·杜·普里兹在 [Unsplash](https://unsplash.com/s/photos/grateful?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上的照片

## 为什么要分开责任？

在我之前关于 Vertex AI 的两篇文章中，我向您展示了如何[使用 web 控制台创建和部署 AutoML 模型](/giving-vertex-ai-the-new-unified-ml-platform-on-google-cloud-a-spin-35e0f3852f25)，以及如何获取您以某种方式训练的 TensorFlow 模型并[将其部署到 Vertex AI](/how-to-deploy-a-tensorflow-model-to-vertex-ai-87d9ae1df56) 。但是这两种方法都不能真正扩展到数百个模型和大型团队。

当您使用 Google Cloud web 控制台创建 AutoML 模型时，您会得到一个可以监控的端点，并且可以在其上设置持续评估。如果你发现模型在漂移，自动根据新数据重新训练它是很困难的——你不会想在凌晨 2 点醒来使用用户界面来训练模型。如果您可以仅使用代码来训练和部署模型，那就更好了。对于您的 MLOps 团队来说，代码自动化要容易得多。

将您在 Jupyter 笔记本中训练的 TensorFlow 模型部署到 Vertex AI 也有同样的问题。再培训将是困难的，因为运营团队将不得不设置所有的运营、监控和调度，而这些都是非常笨重且完全非最小化的。

对于再培训来说，整个过程——从数据集创建到培训再到部署——由代码驱动要好得多。做到这一点，您的运营团队将感谢您让他们的工作变得简单，因为他们清楚地将模型代码与运营代码分开，并且用 Python 而不是笔记本来表达所有内容。

如何在 Vertex AI 中获得这种分离是我在本文中要展示给你的。

## 在 Python 文件中完成

Jupyter 笔记本非常适合开发，但是我强烈建议不要将这些笔记本直接投入生产(是的，我确实知道 [Papermill](https://papermill.readthedocs.io/en/latest/) )。

我建议您将最初的原型模型代码转换成 Python 文件，然后在其中继续所有的开发。扔掉朱庇特笔记本。您将从一个临时笔记本中调用提取(和维护)的 Python 代码，以供将来试验。

你可以在[https://github . com/Google cloud platform/data-science-on-GCP/tree/edition 2/09 _ vertexai](https://github.com/GoogleCloudPlatform/data-science-on-gcp/tree/edition2/09_vertexai)中看到我的例子。请参见文件 model.py 和 train_on_vertexai.py，并使用它们进行后续操作。

## 编写模型. py

文件 model.py 包含了我的 Jupyter 笔记本中所有的 Keras 模型代码( [flights_model_tf2.ipynb](https://github.com/GoogleCloudPlatform/data-science-on-gcp/blob/edition2/09_vertexai/flights_model_tf2.ipynb) 在同一个 GitHub 目录下)。不同之处在于它是可执行的，笔记本的大部分代码被提取到一个名为 train_and_evaluate.py 的函数中:

```
def train_and_evaluate(train_data_pattern, eval_data_pattern, test_data_pattern, export_dir, output_dir):
    ... train_dataset = read_dataset(train_data_pattern, train_batch_size)
    eval_dataset = read_dataset(eval_data_pattern, eval_batch_size, tf.estimator.ModeKeys.EVAL, num_eval_examples) model = create_model()
    history = model.fit(train_dataset,
                        validation_data=eval_dataset,
                        epochs=epochs,
                        steps_per_epoch=steps_per_epoch,
                        callbacks=[cp_callback]) # export
    logging.info('Exporting to {}'.format(export_dir))
    tf.saved_model.save(model, export_dir)
```

有三点需要注意:

1.  数据从分别用于训练、验证和测试数据集的由 train_data_pattern、eval_data_pattern 和 test_data_pattern 指定的 URIs 中读取。
2.  模型创建代码提取到一个名为 create_model 的函数中
3.  模型被写出到 export_dir，任何其他中间输出被写入 output_dir。

数据模式和输出目录在 model.py 中从环境变量中获得:

```
 OUTPUT_DIR = 'gs://{}/ch9/trained_model'.format(BUCKET)
    OUTPUT_MODEL_DIR = os.getenv("AIP_MODEL_DIR")
    TRAIN_DATA_PATTERN = os.getenv("AIP_TRAINING_DATA_URI")
    EVAL_DATA_PATTERN = os.getenv("AIP_VALIDATION_DATA_URI")
    TEST_DATA_PATTERN = os.getenv("AIP_TEST_DATA_URI")
```

这非常重要，因为它是你的代码和 Vertex AI 之间的契约，是所有自动发生的事情所需要的。

然而，您可能需要在 Vertex AI 之外运行这段代码(例如，在开发期间)。在这种情况下，不会设置环境变量，因此所有变量都是 None。寻找这种情况，并将它们设置为您的开发环境中的值:

```
 if not OUTPUT_MODEL_DIR:
        OUTPUT_MODEL_DIR = os.path.join(OUTPUT_DIR,
                                        'export/flights_{}'.format(time.strftime("%Y%m%d-%H%M%S")))
    if not TRAIN_DATA_PATTERN:
        TRAIN_DATA_PATTERN = 'gs://{}/ch9/data/train*'.format(BUCKET)
    if not EVAL_DATA_PATTERN:
        EVAL_DATA_PATTERN = 'gs://{}/ch9/data/eval*'.format(BUCKET)
```

这些文件可能非常小，因为它们仅用于开发。实际的生产运行将在顶点 AI 内部运行，在那里将设置环境变量。

一旦你写完 model.py，确保它能正常工作:

```
python3 model.py  --bucket <bucket-name>
```

现在，您已经准备好从顶点 AI 管道中调用它了。

## 编写培训管道

训练管道(参见 train_on_vertexai.py)需要在代码中做五件事:

1.  在 Vertex AI 中加载托管数据集
2.  设置培训基础结构以运行 model.py
3.  运行 model.py，并传入托管数据集。
4.  找到要将模型部署到的端点。
5.  将模型部署到端点

**1。托管数据集**

这就是如何加载一个表格数据集(有图像、文本等选项。数据集，对于 BigQuery 中的表格数据):

```
data_set = aiplatform.TabularDataset.create(
        display_name='data-{}'.format(ENDPOINT_NAME),
        gcs_source=['gs://{}/ch9/data/all.csv'.format(BUCKET)]
)
```

请注意，我传入了*所有*的数据。Vertex AI 将负责将数据分成训练、验证和测试数据集，并将其发送给训练程序。

**2。培训设置**

接下来，创建一个训练作业，传入 model.py、训练容器映像和服务容器映像:

```
model_display_name = '{}-{}'.format(ENDPOINT_NAME, timestamp)
job = aiplatform.CustomTrainingJob(
        display_name='train-{}'.format(model_display_name),
        script_path="model.py",
        container_uri=train_image,
        requirements=[],  # any extra Python packages
        model_serving_container_image_uri=deploy_image
)
```

(关于为什么要给模型分配时间戳名称，请参见[如何将 TensorFlow 模型部署到顶点 AI](/how-to-deploy-a-tensorflow-model-to-vertex-ai-87d9ae1df56)

**3。运行培训作业**

运行作业包括在某些硬件上的托管数据集上运行 model.py:

```
model = job.run(
        dataset=data_set,
        model_display_name=model_display_name,
        args=['--bucket', BUCKET],
        replica_count=1,
        machine_type='n1-standard-4',
        accelerator_type=aip.AcceleratorType.NVIDIA_TESLA_T4.name,
        accelerator_count=1,
        sync=develop_mode
    )
```

**4。寻找终点**

我们想要部署到一个预先存在的端点(阅读见[如何部署一个 TensorFlow 模型到顶点 AI](/how-to-deploy-a-tensorflow-model-to-vertex-ai-87d9ae1df56) 了解什么是端点)。因此，找到一个现有端点，否则创建一个:

```
 endpoints = aiplatform.Endpoint.list(
        filter='display_name="{}"'.format(ENDPOINT_NAME),
        order_by='create_time desc',
        project=PROJECT, location=REGION,
    )
    if len(endpoints) > 0:
        endpoint = endpoints[0]  # most recently created
    else:
        endpoint = aiplatform.Endpoint.create(
            display_name=ENDPOINT_NAME, project=PROJECT, location=REGION
        )
```

**5。部署型号**

最后，将模型部署到端点:

```
model.deploy(
        endpoint=endpoint,
        traffic_split={"0": 100},
        machine_type='n1-standard-2',
        min_replica_count=1,
        max_replica_count=1
    )
```

就是这样！现在，您有了一个 Python 程序，您可以随时运行它来重新训练和/或部署训练好的模型。当然，MLOps 人员通常不会大规模替换模型，而是只向模型发送一小部分流量。他们可能还会在 Vertex AI 中设置对端点的监控和持续评估。但是你让他们很容易做到这一点。

## 代码中的端到端自动 ML

如果我想使用 AutoML 而不是我的定制培训工作，上面的管道会有什么变化？嗯，我不需要我自己的模型。因此，我将使用 AutoML 来代替 CustomTrainingJob。

设置和运行培训作业(上面的步骤 3 和 4)现在变成了:

```
def train_automl_model(data_set, timestamp):
    # train
    model_display_name = '{}-{}'.format(ENDPOINT_NAME, timestamp)
    job = aiplatform.AutoMLTabularTrainingJob(
        display_name='train-{}'.format(model_display_name),
        optimization_prediction_type='classification'
    )
    model = job.run(
        dataset=data_set,
        target_column='ontime',
        model_display_name=model_display_name,
        budget_milli_node_hours=(300 if develop_mode else 2000),
        disable_early_stopping=False
    )
    return job, model
```

这是唯一的变化！管道的其余部分保持不变。当我们说你有一个 ML 开发的统一平台时，这就是我们的意思。

事实上，您可以将 ML 框架类似地更改为 PyTorch 或 sklearn 或 XGBoost，就 MLOps 人员而言，只有很小的更改。在我的 train_on_vertexai.py 中，我使用命令行参数在自定义 Keras 代码和 AutoML 之间切换。

## 以非默认方式拆分数据

默认情况下，Vertex AI 会对数据进行部分拆分(80%用于训练，10%用于验证和测试)。如果你想控制分裂怎么办？有几个选项可用(基于时间等。).

假设您想要向数据集中添加一个控制拆分的列，您可以在创建数据时执行此操作:

```
CREATE OR REPLACE TABLE dsongcp.flights_all_data ASSELECT
  IF(arr_delay < 15, 1.0, 0.0) AS ontime,
  dep_delay,
  taxi_out,
  ...
  **IF (is_train_day = 'True',
      IF(ABS(MOD(FARM_FINGERPRINT(CAST(f.FL_DATE AS STRING)), 100)) < 60, 'TRAIN', 'VALIDATE'),
      'TEST') AS data_split**
FROM dsongcp.flights_tzcorr f
...
```

基本上，有一个我称为 data_split 的列接受值 TRAIN、VALIDATE 或 TEST。因此，托管数据集中的每一行都被分配给这三个拆分之一。

然后，当我训练作业时(无论是自定义模型还是 automl)，我指定预定义的拆分列是什么:

```
model = job.run(
        dataset=data_set,
        # See [https://googleapis.dev/python/aiplatform/latest/aiplatform.html#](https://googleapis.dev/python/aiplatform/latest/aiplatform.html#)
        **predefined_split_column_name='data_split',**
        model_display_name=model_display_name,
```

就是这样！Vertex AI 将负责剩下的工作，包括将所有必要的元数据分配给正在训练的模型。

一句话:随着越来越多的 it 变得自动化管理，MLOps 变得越来越容易。通过在你的代码中遵循清晰的职责分离，你可以了解这一点。

尽情享受吧！

## 更多关于 Vertex AI 的阅读:

1.  [给谷歌云上的新统一 ML 平台 Vertex AI 一个旋转](/giving-vertex-ai-the-new-unified-ml-platform-on-google-cloud-a-spin-35e0f3852f25) :
    我们为什么需要它，无代码 ML 培训到底有多好，所有这些对数据科学工作意味着什么？
2.  [如何将 TensorFlow 模型部署到 Vertex AI](/how-to-deploy-a-tensorflow-model-to-vertex-ai-87d9ae1df56) :在 Vertex AI 中使用保存的模型和端点
3.  [使用 Python 在 Vertex AI 上开发和部署机器学习模型](https://medium.com/@lakshmanok/developing-and-deploying-a-machine-learning-model-on-vertex-ai-using-python-865b535814f8):编写让你的 MLOps 团队满意的训练管道
4.  [如何在 Vertex AI 中为超参数调整构建 MLOps 管道](https://lakshmanok.medium.com/how-to-build-an-mlops-pipeline-for-hyperparameter-tuning-in-vertex-ai-45cc2faf4ff5) :
    为超参数调整设置模型和协调器的最佳实践