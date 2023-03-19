# AI 配音过 Subs？用人工智能翻译和配音视频

> 原文：<https://towardsdatascience.com/how-to-translate-and-dub-videos-with-machine-learning-55204e55a960?source=collection_archive---------43----------------------->

![](img/b4feb71002471a9c77fe034cc994c345.png)

[unsplash.com](http://unsplash.com/)

除了为自己做饭和绕着房子走几圈，日本卡通(或者孩子们称之为“动画”)是我在隔离期间学会喜欢的东西。

不过，看动漫的问题是，如果不学习日语，你就要依赖人类翻译和配音员把内容翻译成你的语言。有时你得到了字幕(“subs”)，但没有配音(“dubs”)。其他时候，整季的节目根本没有被翻译，你只能坐在座位的边缘，只有维基百科的摘要和 90 年代的网络论坛带你穿越黑暗。

那你该怎么办？答案显然不是让计算机将一部电视节目的全部片段从日语转录、翻译和配音成英语。翻译是一门精细的艺术，不能自动化，需要人手的爱心触摸。此外，即使你确实使用机器学习来翻译视频，你也不能使用计算机来配音……我的意思是，谁会愿意听一整季的机器声音呢？那会很糟糕。只有真正的神经病才会想要那个。

因此，在这篇文章中，我将向你展示如何使用机器学习来转录、翻译视频，并将视频从一种语言转换为另一种语言，即“人工智能视频配音”它可能不会给你带来网飞质量的结果，但必要时你可以用它来本地化在线对话和 YouTube 视频。我们将从使用谷歌云的[语音转文本 API](https://cloud.google.com/speech-to-text) 将音频转换成文本开始。接下来，我们将使用[翻译 API](https://cloud.google.com/translate) 翻译该文本。最后，我们将使用[文本到语音转换 API](https://cloud.google.com/text-to-speech) 为翻译“配音”，根据文档，它产生的声音“像人一样”。

(顺便说一句，在你在评论中炮轰我之前，我应该告诉你，YouTube 将自动免费为你转录和翻译你的视频。所以你可以把这个项目当成你从头开始烘焙酸面团的新爱好:这是对 30 个小时的低效利用。)

# 人工智能配音的视频:它们通常听起来很棒吗？

在你开始这段旅程之前，你可能想知道你有什么期待。我们现实地期望从 ML 视频配音管道中达到什么质量？

这里有一个从英语自动翻译成西班牙语的例子(字幕也是自动生成的英语)。我没有对它进行任何调整或调节:

正如你所看到的，转录是体面的，但并不完美，同样的翻译。(忽略说话者有时语速太快的事实——稍后再谈。)总的来说，你可以很容易地从这个配音视频中了解事情的要点，但它并不完全接近人类的质量。

让这个项目比大多数项目更棘手(更有趣)的是，至少有三个可能的失败点:

1.  语音转文本 API 可能会错误地将视频从音频转录为文本
2.  翻译 API 可能会错误或笨拙地翻译该文本
3.  这些翻译可能会被文本到语音转换 API 读错

以我的经验来看，最成功的配音视频是那些在清晰的音频流中有一个人说话，并从英语配音成另一种语言的视频。这主要是因为英语的转录质量(语音到文本)比其他源语言高得多。

事实证明，从非英语语言配音更具挑战性。这是我最喜欢的节目之一《死亡笔记》的一段特别平淡无奇的日语到英语的配音:

死亡笔记的原始视频

如果你想把翻译/配音留给人类，好吧——我不能怪你。如果没有，请继续阅读！

# 构建人工智能翻译配音员

和往常一样，你可以在用机器学习 Github repo 制作的[中找到这个项目的所有代码。要自己运行代码，请按照自述文件配置您的凭证并启用 API。在这篇文章中，我将简单介绍一下我的发现。](https://github.com/google/making_with_ml/tree/master/ai_dubs)

首先，我们将遵循以下步骤:

1.  从视频文件中提取音频
2.  使用语音转文本 API 将音频转换为文本
3.  **将转录文本分割成句子/片段进行翻译**
4.  翻译文本
5.  生成翻译文本的语音音频版本
6.  **加速生成的音频，使其与视频中的原说话者一致**
7.  将新音频缝合到折叠音频/视频的顶部

我承认，当我第一次着手构建这个配音器时，我充满了傲慢——我所要做的就是将几个 API 插在一起，还有什么比这更简单的呢？但是作为一个程序员，所有的傲慢都必须受到惩罚，好家伙，我也受到了惩罚。

具有挑战性的部分是我上面加粗的部分，主要来自于必须将翻译与视频对齐。但一会儿我们会详细讨论这个问题。

# 使用谷歌云语音转文本 API

翻译视频的第一步是将音频转换成文字。为此，我使用了谷歌云的[语音转文本 API](https://daleonai.com/translate-dub-videos-with-ml?utm_source=blog&utm_medium=partner&utm_campaign=CDR_dal_aiml_ai-dubs_020221) 。这个工具可以识别 125 种语言的音频，但正如我上面提到的，英语的质量最高。对于我们的用例，我们希望启用一些特殊功能，比如:

*   [增强型](https://cloud.google.com/speech-to-text/docs/enhanced-models?utm_source=blog&utm_medium=partner&utm_campaign=CDR_dal_aiml_ai-dubs_020221)。这些是经过特定数据类型(“视频”、“电话呼叫”)训练的语音到文本模型，通常质量更高。当然，我们将使用“视频”模型。
*   脏话过滤器。这个标志防止 API 返回任何不良单词。
*   字时间偏移。这个标志告诉 API 我们希望转录的单词和说话者说这些单词的时间一起返回。我们将使用这些时间戳来帮助我们的字幕和配音与源视频对齐。
*   [语音适配](https://cloud.google.com/speech-to-text/docs/context-strength?utm_source=blog&utm_medium=partner&utm_campaign=CDR_dal_aiml_ai-dubs_020221)。通常，语音到文本转换最难处理的是不常用的单词或短语。如果您知道某些单词或短语可能会出现在您的视频中(即“梯度下降”、“支持向量机”)，您可以将它们以数组的形式传递给 API，这将使它们更有可能被转录:

```
client = speech.SpeechClient()  
# Audio must be uploaded to a GCS bucket if it's > 5 min
audio = speech.RecognitionAudio(uri="gs://path/to/my/audio.wav")

config = speech.RecognitionConfig(
  language_code="en-US"
  # Automatically transcribe punctuation 
  enable_automatic_punctuation=True,
  enable_word_time_offsets=True,
  speech_contexts=[
    # Boost the likelihood of recognizing these words:
    {"phrases": ["gradient descent", "support vector machine"], 
     "boost": **15**}
  ],
  profanity_filter=True,
  use_enhanced="video",
  model="video")

res = client.long_running_recognize(config=config, audio=audio).result()
```

API 将转录的文本连同单词级时间戳作为 JSON 返回。举个例子，我转录了[这个视频](https://youtu.be/o6nGn1euRjk)。你可以在[这个 gist](https://gist.github.com/dalequark/e983b929b6194adb49d00a9c55ae4e33) 里看到 API 返回的 JSON。输出还让我们进行快速的质量健全性检查:

*我实际说的:*

> *“软件开发者。我们的摇滚风格并不出名，对吧？或者说*是*我们？今天，我将向你展示我如何利用 ML 让自己变得更时尚，从有影响力的人那里获取灵感。”*

*API 以为我说了什么:*

> *“软件开发者。我们的摇滚和风格并不出名。我们还是今天的我们吗？我将向你展示我是如何利用 ml 从有影响力的人那里获取灵感来打造新的时尚潮流的。”*

根据我的经验，这是你在转录高质量英语音频时可以期待的质量。注意标点有点偏。如果你对观众理解视频的要点感到满意，这可能已经足够好了，尽管如果你说的是源语言，你可以很容易地自己手动修改脚本。

此时，我们可以使用 API 输出来生成(未翻译的)字幕。事实上，如果您用`-srt '标志运行我的脚本，它会为您做到这一点( [srt](https://blog.hubspot.com/marketing/srt-file#:~:text=An%20SRT%20file%20(otherwise%20known,the%20sequential%20number%20of%20subtitles.) 是隐藏字幕的一种文件类型):

```
python dubber.py my_movie_file.mp4 "en" outputDirectory **--srt** **--targetLangs** ["es"]
```

# 机器翻译

现在我们有了视频抄本，我们可以使用[翻译 API](https://daleonai.com/cloud.google.com/translate?utm_source=blog&utm_medium=partner&utm_campaign=CDR_dal_aiml_ai-dubs_020221) 来…嗯…翻译它们。

这是事情开始变得有点🤪。

我们的目标是这样的:我们希望能够翻译原始视频中的单词，然后在大致相同的时间点播放它们，以便我的“配音”声音与我的真实声音保持一致。

然而，问题是翻译不是逐字逐句的。从英语翻译成日语的句子可能语序混乱。它可能包含更少的词，更多的词，不同的词，或(如成语)完全不同的措辞。

解决这个问题的一个方法是翻译整个句子，然后试着调整这些句子的时间界限。但是即使这样也变得复杂了，因为你如何表示一个句子呢？在英语中，我们可以用标点符号来拆分单词，例如:

`"Hi! My name is Dale. What's up?" --> ["Hi", "My name is Dale", "What's up"]`

但标点符号因语言而异(英语中没有)，有些语言根本不用标点符号分隔句子。

另外，在现实生活中，我们通常不会用完整的句子说话。你知道吗？

另一个让翻译抄本变得困难的问题是，一般来说，你输入到翻译模型中的*越多*上下文，你就能期待更高质量的翻译。举个例子，如果我把下面的句子翻译成法语:

"我感到忧郁，但我也喜欢粉红色。"

我去拿翻译过来的:

"我喜欢蓝色，但我也喜欢玫瑰."

这是准确的。但是，如果我把这句话分成两部分(“我感到忧郁”和“但是我也喜欢粉红色”)，并分别翻译每一部分，我会得到:

“Je me sens triste，mais j'aime aussi le rose”，即“我感到悲伤，但我也喜欢粉红色。”

这就是说，我们在将文本发送到翻译 API 之前分割得越多，翻译的质量就越差(尽管在时间上与视频保持一致会更容易)。

最终，我选择的策略是，每当说话者停顿时间超过一秒钟时，就把所说的话分开。这是一个看起来像什么的例子:

```
{
        "en": "Software developers.",
        "start_time": **0.2**,
        "end_time": **1.5**,
    },
    {
        "en": "We're not known for our Rock and style. Are we",
        "start_time": **1.6**,
        "end_time": **4.4**,
    },
    {
        "en": "or are we",
        "start_time": **5**,
        "end_time": **6.2**,
    },
```

这自然导致了一些尴尬的翻译(即“或者我们”是一个奇怪的翻译片段)，但我发现它足够好。这里是代码中的逻辑。

侧栏:我还注意到，对于非英语语言，语音到文本 API 返回的时间戳的准确性明显较低，这进一步降低了非英语到英语配音的质量。

最后一件事。如果您已经知道希望如何翻译某些单词(例如，我的名字“Dale”应该总是简单地翻译为“Dale”)，您可以利用高级翻译 API 的“词汇表”功能来提高翻译质量。我在这里写了一篇关于那个[的博文](https://daleonai.com/improving-machine-translation-with-the-google-translation-api-advanced)。

碰巧的是，Google Cloud 正在开发一个新的 API 来处理翻译口语的问题。它被称为[媒体翻译 API](https://cloud.google.com/media-translation?utm_source=blog&utm_medium=partner&utm_campaign=CDR_dal_aiml_ai-dubs_020221) ，它直接在音频上运行翻译(即没有转录的文本中介)。我不能在这个项目中使用这个 API，因为它还没有返回时间戳(这个工具目前处于测试阶段)，但是我认为在未来的迭代中使用它会很棒！

# 文本到语音转换

现在开始有趣的部分——挑选电脑声音！如果你读过我的 [PDF 到有声读物转换器](https://daleonai.com/pdf-to-audiobook)，你就会知道我喜欢听起来滑稽的电脑声音。为了生成配音音频，我使用了谷歌云[文本到语音转换 API](https://daleonai.com/cloud.google.com/text-to-speech?utm_source=blog&utm_medium=partner&utm_campaign=CDR_dal_aiml_ai-dubs_020221) 。TTS API 可以用不同口音的不同语言生成许多不同的声音，你可以在这里找到并使用[来弹奏](https://cloud.google.com/text-to-speech/docs/voices?utm_source=blog&utm_medium=partner&utm_campaign=CDR_dal_aiml_ai-dubs_020221)。“标准”声音可能听起来有点，呃，*微小，*如果你知道我的意思，但是由高质量神经网络生成的 [WaveNet](https://deepmind.com/blog/article/wavenet-generative-model-raw-audio) 声音听起来很像人类。

在这里，我遇到了另一个我没有预见到的问题:如果计算机的声音比视频的原始扬声器慢得多，从而导致生成的音频文件太长，该怎么办？那么配音将不可能与源视频对齐。或者，如果译文比原文更加冗长，导致同样的问题，怎么办？

为了解决这个问题，我尝试了文本到语音转换 API 中的参数`speakingRate`。这允许您加快或减慢电脑声音:

```
# Instantiates a client
    client = texttospeech.TextToSpeechClient()

    # Set the text input to be synthesized
    synthesis_input = texttospeech.SynthesisInput(text="Hello World")

    voice = texttospeech.VoiceSelectionParams(
        language_code=languageCode, name=voiceName
    )

    # Select the type of audio file you want returned
    audio_config = texttospeech.AudioConfig(
        audio_encoding=texttospeech.AudioEncoding.MP3,
        # Speed up or slow down speaking
        speaking_rate=speakingRate 
    )

    # Perform the text-to-speech request on the text input with the selected
    # voice parameters and audio file type
    response = client.synthesize_speech(
        input=synthesis_input,
        voice=voice,
        audio_config=audio_config
    )
```

所以，如果电脑说一句话的时间比视频原声说话人说一句话的时间长，我就提高说话速度，直到电脑和人类花的时间差不多。

听起来有点复杂？代码如下所示:

```
**def** **speakUnderDuration**(text, languageCode, durationSecs, voiceName=None):
    """Speak text within a certain time limit.
    If audio already fits within duratinSecs, no changes will be made.

    Args:
        text (String): Text to be spoken
        languageCode (String): language code, i.e. "en"
        durationSecs (int): Time limit in seconds
        voiceName (String, optional): See https://cloud.google.com/text-to-speech/docs/voices

    Returns:
        bytes : Audio in wav format
    """
    # First, generate audio with speakingRate = 1
    baseAudio = speak(text, languageCode, voiceName=voiceName)

    # Save audio to a temporary file
    f = tempfile.NamedTemporaryFile(mode="w+b")
    f.write(baseAudio)
    f.flush()

    # Get the sample's duration
    baseDuration = AudioSegment.from_mp3(f.name).duration_seconds
    f.close()

    # How fast was the generated audio compared to the original?
    ratio = baseDuration / durationSecs

    # if the audio fits, return it
    **if** ratio <= **1**:
        **return** baseAudio

    # If the base audio is too long to fit in the segment, increase
    # the speaking rate
    ratio = round(ratio, **1**)
    # speakingRate must be <= 4
    **if** ratio > **4**:
        ratio = **4**
    **return** speak(text, languageCode, voiceName=voiceName, speakingRate=ratio)
```

这解决了音频与视频对齐的问题，但有时这意味着我配音中的电脑扬声器有点笨拙地快。但是这对 V2 来说是个问题。

# 值得吗？

你知道这句话吗，“玩愚蠢的游戏，赢得愚蠢的奖品？”感觉我在这里建立的每一个 ML 项目都是一种爱的劳动，但这一次，我喜欢我的愚蠢的奖励:能够生成无限数量的怪异，机器人，笨拙的动画配音，有时还有点像样。

点击这里查看我的结果:

*遵循*[*dale quark*](http://twitter.com/dalequark)*为万物 ML。最初发表于 2021 年 2 月 2 日 https://daleonai.com**[*。*](https://daleonai.com/translate-dub-videos-with-ml)*