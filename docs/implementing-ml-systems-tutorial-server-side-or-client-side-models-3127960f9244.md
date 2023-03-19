# 实现 ML 系统教程:服务器端还是客户端模型？

> 原文：<https://towardsdatascience.com/implementing-ml-systems-tutorial-server-side-or-client-side-models-3127960f9244?source=collection_archive---------17----------------------->

## 编写小型 Flask 服务器与编写 WebDNN 浏览器模型

![](img/1bc62270cbbb76d1bd90d3b7885eef42.png)

Krzysztof Kowalik 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上的照片

开发机器学习模型很有趣，也很好，但开发完之后，你可能会考虑将它们部署到应用程序中来使用它们。问题是你应该把它们放在客户端(可能是手机)还是应该把模型放在服务器上，把数据发送给服务器，然后把结果拿回来？在这篇文章中，我将讨论这两种方法的利弊，以及这两种方法需要什么。

## 服务器端模型

服务器端机器学习模型是最广泛使用的 ML 系统，因为其架构实现起来更简单。例如，您在手机(客户端)上运行了一个对图像进行分类的应用程序。用户拍了一张照片，这张照片被翻译成字节，手机用这些字节向服务器发送 POST 请求。服务器对这些字节运行模型的预测功能，并将带有预测的 JSON 响应发送回移动设备。这里没什么特别的。

现在，如果您以这种方式实施您的 ML 系统，您将面临以下一些挑战:

1.  **延迟:**如果您的应用需要密集的最大似然预测，如实时视频对象检测，您可能会在不充足的时间内获得结果，这将使用户体验非常不愉快。
2.  **成本:**典型的 web 应用程序不需要配备高端 GPU 的昂贵机器来快速运行预测。然而，如果你在服务器上托管模型，你将需要一个具有强大 GPU 的服务器，这是非常昂贵的。例如，我看到过这样的例子，在 AWS EC2 实例上托管一个图像字幕模型的成本大约是每天 24 美元(每年大约 9000 美元)。这与典型的 web 应用程序相比非常昂贵。

因此，尽管这种方法可能更容易实现，但也有一些缺点需要考虑。当然，这取决于具体情况，因此测试您的系统并检查这些缺点是否真的会导致问题是值得的。

如果您对以这种方式实现 ML 系统感兴趣，我建议尝试 Python Flask 服务器。它们非常容易实现和开始使用。你可以在这里查看我的教程[关于构建一个使用 Flask 的 ML messenger 聊天机器人。您实际上是从运行服务器的样板代码开始，然后添加一个如下所示的预测函数:](/how-to-build-a-ml-powered-image-translation-messenger-chatbot-in-less-than-24-hours-1bf9d7b88f8b)

```
#Client sends a POST request with the image url[@app](http://twitter.com/app).route('/predict', methods=['POST'])
def predict(): if request.method == 'POST': data = json.loads(request.data)
    url = data[0]["payload"]["url"]
    image = requests.get(url)  # Get the image from the url sent by the client
    img = Image.open(BytesIO(image.content))
    result = predictor_api.make_prediction(url)
    return jsonify({'result': result})
```

## 客户端模型

![](img/94e50d4ca2f7273f0fe921c19680db62.png)

照片由[帕诺斯·萨卡拉基斯](https://unsplash.com/@meymigrou?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

这是事情开始变得更有趣的地方。有几种方法可以做到这一点(与服务器端模型不同)，客户端模型更多的是一个正在进行的研究基础。两种最典型的客户端是浏览器或微控制器。我们将讨论浏览器，因为它们是更可能的客户端选项。

客户端 ML 模型当然比服务器端模型便宜得多，并且理论上它们应该具有更低的延迟，因为你不会向另一个服务器发送和接收请求，你将在一个地方(客户端)做所有的事情。然而，实际上由于硬件限制，延迟实际上会更大。而且，它们可能会变得更加难以实现，因为你需要进行大量的优化，以便 ML 系统能够顺利运行，这就是我们将要在这里讨论的。

在移动设备/浏览器上部署 ML 模型的主要问题是由于硬件和集成的限制。本质上，大多数在浏览器上运行的 web 应用程序通常不需要像预测那样的计算密集型操作(除了 web 应用程序上的游戏)。然而，最近有大量的优化被引入，使得客户端 ML 模型变得更加可能。

其中几个例子是:

1.  [**WebGL**](https://www.khronos.org/webgl/) **(浏览器实现**[**OpenGL**](https://www.opengl.org//)**):**web GL 在 2011 年已经发布**(所以其并不算太新)，但是对于 ML 机型来说还是挺有用的。WebGL 是一个 Javascript API，用于允许在浏览器上运行的任何应用程序平滑地交互和使用 GPU，通常用于渲染游戏中的图形和物理交互**
2.  [**Web Assembly**](https://webassembly.org/)**:**Web Assembly 大约出现在 **4 年前**你大概能从关键词“Assembly”中感觉到它与编译和底层代码有关。WebAssembly 引入了一种将 web 应用程序编译成更紧凑的二进制格式的新方法，这种方法从本质上减小了它们的大小，并允许它们在浏览器上更流畅地运行。它通过定义一个以二进制格式<https://github.com/WebAssembly/design/blob/master/BinaryEncoding.md>**存储的[抽象语法树](https://github.com/WebAssembly/design/blob/master/AstSemantics.md#abstract-syntax-tree-semantics) (AST)来实现这一点。在“Javascript 的 nitrous boost”之前已经调用过了。**
3.  **[**WebGPU**](https://www.w3.org/TR/webgpu/) :这本质上是在 WebGL 上的升级。WebGPU 是一个正在进行的项目，用于升级 WebGL 以优化浏览器上的 GPU 操作。简而言之，WebGPU 是一种新标准，它试图通过将标准分解为更多模块化的着色器组件来提高 WebGL 的核心性能，这些组件可以并发运行以获得更好的性能。**
4.  **[**WebDNN**](https://mil-tokyo.github.io/webdnn/)**:**WebDNN 是在浏览器上运行深度神经网络最好的开源库之一。WebDNN 的第一个组件是 Python API，它将模型转换成图形。然后，这个图被移交给第二个组件，该组件是一个 JS API，它将这个图移植成兼容的格式，以便在浏览器上运行(适用于 WebGL、Web Assembly 等..)**

**WebDNN 其实挺有意思的。我想研究一下它的使用和设置有多简单，所以下一节将是一个关于如何操作的小教程。第一个组件是 python API，它按照上面的讨论转换模型。请注意，这不是一个精确的一步一步的教程，这里的主要目的是大致演示如何做到这一点，因为一个精确的教程可能需要一篇完整的文章，如果你想让我这样做，请在评论中告诉我。**

**步骤 Python API:**

```
# - Imports
from keras.applications import resnet50
from webdnn.frontend.keras import KerasConverter
from webdnn.backend import generate_descriptor# 1\. load model
model = resnet50.ResNet50(include_top=True, weights='imagenet')# 2\. Convert model to a graph
graph = KerasConverter(batch_size=1).convert(model)
exec_info = generate_descriptor("webgpu,webgl,webassembly,fallback", graph)# 3\. Save the graph  & weights
exec_info.save("./output")
```

**来源: [WebDNN 文档](https://github.com/mil-tokyo/webdnn)**

**步骤 JavaScript API:**

```
let runner, image, probabilities;
// Step 1: load the modelasync function init() {
    // Initialize descriptor runner
    runner = await WebDNN.load('./output');
    image = runner.inputs[0]; 
    probabilities = runner.outputs[0];
}// Step2: define a function that  runs predicitionsasync function run() {
    // Set the value into input variable.
    image.set(await WebDNN.Image.getImageArray('./input_image.png'));

    // Run
    await runner.run();// Show the result
    console.log('Output', WebDNN.Math.argmax(probabilities));
}
```

**来源: [WebDNN 文档](https://github.com/mil-tokyo/webdnn)**

**这看起来一点都不漫长也不复杂！在典型的客户端 ML web 应用程序中，您还需要一个额外的步骤，就是将模型集成到 chrome 扩展中。第一部分是在浏览器中持续运行的后台脚本，第二部分是注入每个运行页面的内容脚本。内容脚本负责将数据发送到后台脚本，以便在模型上运行预测。这实际上很容易实现。您需要一个函数来发送数据(例如，当一个按钮被点击时)，一个函数来监听/接收数据。**

**这里有一个很好的例子:**

```
// content script-1, sends a message
$('img').each(function() {
 var img = $(this);
 chrome.extension.sendMessage({
   msg: "run_model",
   image: img[0].currentSrc
 });
};// background script-1, listens to the message and runs the prediction
chrome.extension.onMessage.addListener(
 function(request, sender, sendResponse){
  if (request.msg == "run_model") {
   chrome.tabs.query({active: true, currentWindow: true},
    function(tabs) {
     run(request.image, tabs[0].id)
    }
   )
  }
 }
)// background script-2, send predictions back
chrome.tabs.sendMessage(
 tab_id,
 {result: results}, function (response) {}
);// content-script-2, receive model output
chrome.extension.onMessage.addListenser(
 function(request, sender, sendResponse){
  var imgid = request.imgid;
  var result = request.result
  // use output here
 }
)
```

**来源: [Github 示例项目](https://github.com/milhidaka/chainer-image-caption/blob/master/webdnn/index.js)**

**就是这样！我希望你喜欢这篇文章，并学到了一些东西。客户端模型是 ML 中一个有趣的领域，你可以期待我在将来发表更多这样的文章。**

**欢迎订阅我的时事通讯:**

**https://artisanal-motivator-8249.ck.page/5524b8f934**