# 在 TensorFlow 服务中使用预先训练的 Huggingface 模型

> 原文：<https://towardsdatascience.com/use-pre-trained-huggingface-models-in-tensorflow-serving-d2761f7e69f6?source=collection_archive---------15----------------------->

## 将数千个社区 NLP 模型投入生产

![](img/41ddc6736f665081e0a16bf03cedac19.png)

克里斯托夫·高尔在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

**HuggingFace** 简化了 NLP，只需几行代码，你就拥有了一个完整的管道，能够执行从情感分析到文本生成的任务。作为一个预训练模型的中心，加上它的开源框架 **Transformers，**我们过去做的许多艰苦工作都得到了简化。这使我们能够编写能够解决复杂的 NLP 任务的应用程序，但缺点是我们不知道幕后发生了什么。尽管**拥抱脸**和**变形金刚**执行了这种惊人的简化，但我们可能希望从所有代码中提取一些抽象，并简单地使用许多可用的预训练模型中的一个。在这篇文章中，我们将学习如何使用 TensorFlow 服务中的许多预训练模型之一，这是一种将机器学习模型投入生产的流行服务。

我将使用这个 [Distilbert](https://huggingface.co/distilbert-base-uncased-finetuned-sst-2-english?text=I+like+you.+I+love+you) 预训练模型进行情感分析，它将预测给定文本是正面还是负面的。不幸的是，Transformers 没有直接导出到 TensorFlow Serve 的功能，因此我们必须做一些工作来实现我们的目标。首先，我们需要安装 Tensorflow、Transformers 和 NumPy 库。

```
pip install transformers
pip install tensorflow
pip install numpy
```

在第一部分代码中，我们将从 Transformers 加载模型和标记器，然后以正确的格式将其保存在磁盘上，以便在 TensorFlow Serve 中使用。

```
from transformers import TFAutoModelForSequenceClassification
import tensorflow as tfMAX_SEQ_LEN = 100model = TFAutoModelForSequenceClassification.from_pretrained("distilbert-base-uncased-finetuned-sst-2-english")callable = tf.function(model.call)
concrete_function = callable.get_concrete_function([tf.TensorSpec([None, MAX_SEQ_LEN], tf.int32, name="input_ids"), tf.TensorSpec([None, MAX_SEQ_LEN], tf.int32, name="attention_mask")])model.save('saved_model/distilbert/1', signatures=concrete_function)
```

首先，我们用 TFAutoModelForSequenceClassification 加载模型。加载变形金刚模型的一个关键方面是选择正确的类。由于我们使用预训练模型进行情感分析，我们将使用 TensorFlow 的加载器(这就是为什么我们导入了 **TF** AutoModel 类)进行序列分类。如果你不确定加载什么类，只需检查模型卡或“在变形金刚中使用”页面上的“拥抱脸模型”信息，以确定使用哪个类。

在代码片段中，我们声明了一个签名函数，它是 TensorFlow Serve 所必需的。这个函数对于向 TF Serve 中的可服务模型声明我们的数据的输入形状是必要的，它由变量 *MAX_SEQ_LEN* 定义。在这种情况下，我已经定义了模型将接受两个输入，两个大小为 200 的列表，分别用于 input_ids(我们从 tokenizer 获得的 id)和 attention_mask(如果输入序列长度小于最大输入序列长度时使用)。记住我们为每个输入声明的名称也很重要，因为它是我们稍后将在发送给模型的 HTTP 请求中定义的参数。在执行这几行之后，我们应该在工作目录中有一个包含以下文件的新目录:

如果你还没有[安装 TensorFlow Serve](https://github.com/tensorflow/serving/blob/master/tensorflow_serving/g3doc/docker.md) (我推荐和 Docker 一起使用)现在就做吧。安装完成后，我们可以从 Docker 中提取映像，并使用加载了以下命令的 Distilbert 模型开始运行我们的服务:

```
docker run -p 8501:8501 --mount type=bind, source=/PATH_TO_YOUR_DIRECTORY/saved_models/distilbert, target=/models/distilbert -e MODEL_NAME=distilbert -t tensorflow/serving
```

这将导致 TensorFlow Serve 在端口 8501 加载 Distilbert(模型文件必须位于用数字命名的目录下，否则 TF Serve 不会加载模型),我们可以通过 HTTP 请求该端口。在下一节中，我将展示如何对我们的加载模型进行预测。

```
import requests
from transformers import AutoTokenizer, AutoConfigtokenizer = AutoTokenizer.from_pretrained("distilbert-base-uncased-finetuned-sst-2-english")text = "I like you. I love you"
encoded_input = tokenizer(text, pad_to_max_length=MAX_SEQ_LEN, max_length=MAX_SEQ_LEN)#TF Serve endpoint
url = "http://localhost:8501/v1/models/distilbert:predict"

payload={"instances": [{"input_ids": encoded_input['input_ids'], "attention_mask": encoded_input['attention_mask']}]}print(payload)
>> { "input_ids": [101, 1045, 2066, 2017, 1012, 1045, 2293, 2017,  102, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0], "attention_mask": [1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0] }headers = {
  'Content-Type': 'application/json'
}

response = requests.request("POST", url, headers=headers, data=json.dumps(payload))

print(json.loads(response.text)['predictions'])
>>[[-4.2159214, 4.58923769]]
```

我们首先从 transformers 调用模型标记器，将输入文本转换成 id 列表及其相应的注意掩码，两者都有填充，以获得发送到我们加载的模型所需的格式。令牌化后，我们对 TensorFlow 执行 POST 请求，使用正确的有效负载提供服务，在这里我们获得呈现的值。但是等等，这个模型不是应该对文本中的情感进行分类吗？嗯，我们还缺少最后一步，将 [**softmax**](https://machinelearningmastery.com/softmax-activation-function-with-python/) 函数应用于我们的结果向量。

```
import numpy as npdef softmax(x):e_x = np.exp(x - np.max(x))
    return e_x / e_x.sum(axis=0)print(softmax(json.loads(response.text)['predictions'][0]))
>>[0.0001 0.9999]
```

得到的向量是输入文本为负或正的概率。在这种情况下，0.0001 的概率为负，0.9999 的概率属于正类。如果我们将结果与用变压器加载管道的输出进行比较，我们可以看到两者是相同的。

总结这篇文章，我们使用了许多 huggingface 预训练模型中的一个，并将其加载到 TensorFlow Serve 上，这样我们就可以发出 HTTP 请求，并轻松地将这些模型扩展到生产中。我们还学习了如何使用模型记号化器向我们加载的模型发出请求，以及如何将 softmax 函数应用到我们的结果中以获得所需的值。在我的下一篇文章中，我将展示我们如何通过编写我们的记号赋予器来将自己从 transformers 库中完全抽象出来，并使用它来请求我们加载的模型。喜欢随时提出建议或改进意见，希望这篇文章能帮助你实现你的目标。