> 由于技术发展太快，该文档具有实效性，调研时间为 2023.3.21。


## 所有模型

### GPT-4

GPT-4 是一个大型多模态模型（目前只支持文本输入输出，将来会支持图像输入），**由于其更广泛的常识和高级推理，它可以更准确地解决难题**。GPT-4 针对聊天进行了优化，但也适用于传统的完成任务。

对于许多基本任务，GPT-4 和 GPT-3.5 模型之间的差异不大。但在更复杂的推理情况下，GPT-4 的能力更强，准确率也高很多，但响应速度要比 GPT-3.5 慢许多。

训练数据截至 2021 年 9 月。也就是说他不知道这之后发生的事情。

缺点是目前还没有全量开放，只有在白名单内的账户才能使用，而且价格也更贵。

定价：

Tip: 1k tokens 大约等于 750 个单词

[what-are-tokens-and-how-to-count-them](https://help.openai.com/en/articles/4936856-what-are-tokens-and-how-to-count-them)

| **Model**   | **Prompt**        | **Completion**    |
| ----------- | ----------------- | ----------------- |
| 8K context  | $0.03 / 1K tokens | $0.06 / 1K tokens |
| 32K context | $0.06 / 1K tokens | $0.12 / 1K tokens |

### GPT-3.5

GPT-3.5 模型可以理解并生成自然语言或代码。也针对聊天进行了优化，但也适用于传统的完成任务。

训练数据截至 2021 年 9 月。

缺点对于一些问题准确率不是很高。

优点是便宜，响应速度快。

定价：

| **Model**     | **Usage**          |
| ------------- | ------------------ |
| gpt-3.5-turbo | $0.002 / 1K tokens |

### GPT-3

GPT-3 模型可以理解和生成自然语言。这些模型被更强大的 GPT-3.5 所取代。但是，原始 GPT-3 基本模型（`davinci`、`curie`、`ada` 和 `babbage`）是当前唯一可用于微调的模型。也就是说这些模型可以根据自己的数据来训练微调。

训练数据截至 2019 年 10 月。

定价：

| **Model** | **特点**             | **Usage**           |
| --------- | -------------------- | ------------------- |
| ada       | GPT-3 系列里速度最快 | $0.0004 / 1K tokens |
| babbage   | -                    | $0.0005 / 1K tokens |
| curie     | -                    | $0.0020 / 1K tokens |
| davinci   | 能力更强，质量也更高 | $0.0200 / 1K tokens |

对于微调模型的价格：

| **Model** | **Training**        | **Usage**           |
| --------- | ------------------- | ------------------- |
| ada       | $0.0004 / 1K tokens | $0.0016 / 1K tokens |
| babbage   | $0.0006 / 1K tokens | $0.0024 / 1K tokens |
| curie     | $0.0030 / 1K tokens | $0.0120 / 1K tokens |
| davinci   | $0.0300 / 1K tokens | $0.1200 / 1K tokens |

### ~~Codex~~

Codex 是基于 GPT-3 模型的，可以用来理解和生成代码，它的训练数据包含自然语言和 GitHup 数十亿行公共代码。

该模型最擅长 Python，精通 JavaScript、Go、Perl、PHP、Ruby、Swift、TypeScript、SQL 和 Shell 等十几种语言。

训练数据截至 2021 年 6 月。

~~目前还在有限测试中，在这期间~~**免费**~~使用~~。最新消息，Open AI 于 2023 年 3 月 23 停止该模型，建议使用 GPT-3.5。

### Embeddings

Embeddings 是文本的数字表示，可用于衡量两段文本之间的相关性。Embeddings 可用于搜索、聚类、推荐、异常检测和分类任务。

定价：

| **Model** | **Usage**           |
| --------- | ------------------- |
| ada       | $0.0004 / 1K tokens |

### Moderation

Moderation 旨在检查内容是否符合 OpenAI 的[使用政策](https://openai.com/policies/usage-policies)。这些模型提供了查找以下类别内容的分类功能：仇恨、仇恨/威胁、自残、性、性/未成年人、暴力和暴力/图片。

目前也在**免费**使用阶段。

### DALL·E

DALL·E 是一个人工智能系统，可以根据自然语言的描述创建逼真的图像和艺术作品。支持在提示的情况下创建具有特定大小的新图像、编辑现有图像或创建用户提供的图像的变体的能力。

定价：

| **Resolution** | **Price**      |
| -------------- | -------------- |
| 1024×1024      | $0.020 / image |
| 512×512        | $0.018 / image |
| 256×256        | $0.016 / image |

### Whisper

Whisper 是一种通用的语音识别模型。它在不同音频的大型数据集上进行训练，也是一个多任务模型，可以执行多语言语音识别以及语音翻译和语言识别。

定价：

| **Model** | **Usage**                              |
| --------- | -------------------------------------- |
| Whisper   | $0.006 / minute (四舍五入到最接近的秒) |

## 支持的 API

### Chat 聊天

`POST https://api.openai.com/v1/chat/completions`

聊天对话，市面上主流的 ChatGPT 应用都是基于这个实现。

支持的模型：

- gpt-4
- gpt-4-0314
- gpt-4-32k
- gpt-4-32k-0314
- gpt-3.5-turbo
- gpt-3.5-turbo-0301

主要可调参数：

- message：对话列表，可以指定系统消息和对话记录，如：

```python
messages=[
        {"role": "system", "content": "You are a helpful assistant."},
        {"role": "user", "content": "Who won the world series in 2020?"},
        {"role": "assistant", "content": "The Los Angeles Dodgers won the World Series in 2020."},
        {"role": "user", "content": "Where was it played?"}
]
```

可以通过设置不同的 system 消息，来营造各种特定的场景。

- temperature：使用什么采样温度，介于 0 和 2 之间。值越高输出内容越随机，反之越稳定。
- top_p：代替 temperature，称为核采样，其中模型考虑具有 top_p 概率质量的标记的结果。所以 0.1 意味着只考虑构成前 10% 概率质量的标记。temperature 和 top_p 设置一个即可，不能两个都设置。

### Completions 内容补全

`POST https://api.openai.com/v1/completions`

根据提示词补全内容，例如：输入提示词「传统网络是」，它就会补全这个句子。感觉这个能实现的，Chat 都可以实现。

支持的模型：

- text-davinci-003
- text-davinci-002
- text-curie-001
- text-babbage-001
- text-ada-001
- davinci
- curie
- babbage
- ada
- code-davinci-002
- code-cushman-001

主要可调参数：

- prompt：提示词，给出一段提示，它会补全后面的内容（包括代码）
- suffix：后缀，当要在中间补全内容时使用，如「小猫很 [insert] 但是也很可爱」，“小猫很” 是 prompt，“但是也很可爱” 就是 suffix
- temperature：和上面一样，采样温度，介于 0 和 2 之间。值越高输出内容越随机，反之越稳定。
- top_p：和上面一样，代替 temperature，称为核采样。temperature 和 top_p 设置一个即可，不能两个都设置。
- stop：结束标记，停止生成的标记，返回的补全内容不会包含此字符。

### Edits 修改内容

`POST https://api.openai.com/v1/edits`

通过给定的内容和修改命令，对内容进行修改。如输入：「What day of the wek is it?」，修改命令为：「修改拼写错误」，它就会返回正确拼写的句子：「What day of the week is it?」

支持的模型：

- text-davinci-edit-001
- code-davinci-edit-001

主要可调参数：

- input：待修改的内容。
- instruction：修改命令。
- temperature：和上面一样，采样温度，介于 0 和 2 之间。值越高输出内容越随机，反之越稳定。
- top_p：和上面一样，代替 temperature，称为核采样。temperature 和 top_p 设置一个即可，不能两个都设置。

### Embeddings 嵌入向量转换

`POST https://api.openai.com/v1/embeddings`

将给定的文本转换为数字向量。

支持的模型：

- text-embedding-ada-002
- text-search-ada-doc-001

主要可调参数：

- input：输入的内容。

### Moderations 内容审核

`POST https://api.openai.com/v1/moderations`

检查内容是否符合 Open AI 的政策，如暴力、色情等内容。

支持的模型：

- text-moderation-stable
- text-moderation-latest

主要参数：

- input：待检查的内容。

返回例子：

```json
{
  "id": "modr-5MWoLO",
  "model": "text-moderation-001",
  "results": [
    {
      "categories": {
        "hate": false,
        "hate/threatening": true,
        "self-harm": false,
        "sexual": false,
        "sexual/minors": false,
        "violence": true,
        "violence/graphic": false
      },
      "category_scores": {
        "hate": 0.22714105248451233,
        "hate/threatening": 0.4132447838783264,
        "self-harm": 0.005232391878962517,
        "sexual": 0.01407341007143259,
        "sexual/minors": 0.0038522258400917053,
        "violence": 0.9223177433013916,
        "violence/graphic": 0.036865197122097015
      },
      "flagged": true
    }
  ]
}
```

### Images 生成/修改图片

#### 创建图片

`POST https://api.openai.com/v1/images/generations`

根据提示词和图片分辨率，生成对应的图片

支持的模型：

- DALL·E

主要参数：

- prompt：提示词，就是要生成什么样的图片。
- n：生成几张图。
- size：图片的分辨率大小。

返回例子：

[https://oaidalleapiprodscus.blob.core.windows.net/private/org-XRmzS8bsR...](https://oaidalleapiprodscus.blob.core.windows.net/private/org-XRmzS8bsRXK0Bv3bm3ZWG2pV/user-ugLS0njlcPKNCv7yd2f45jWN/img-CMU6Clg6A7oxjDquFPlSCXvf.png?st=2023-03-22T06%3A08%3A55Z&se=2023-03-22T08%3A08%3A55Z&sp=r&sv=2021-08-06&sr=b&rscd=inline&rsct=image/png&skoid=6aaadede-4fb3-4698-a8f6-684d7786b067&sktid=a48cca56-e6da-484e-a814-9c849652bcb3&skt=2023-03-21T15%3A59%3A24Z&ske=2023-03-22T15%3A59%3A24Z&sks=b&skv=2021-08-06&sig=Ye5WMUlSDjfct%2B1lZyzSF3tNo7C1z5pjwXc2lIAlguc%3D)

```json
{
  "created": 1679468935,
  "data": [
    {
      "url": "https://oaidalleapiprodscus.blob.core.windows.net/private/org-XRmzS8bsRXK0Bv3bm3ZWG2pV/user-ugLS0njlcPKNCv7yd2f45jWN/img-CMU6Clg6A7oxjDquFPlSCXvf.png?st=2023-03-22T06%3A08%3A55Z&se=2023-03-22T08%3A08%3A55Z&sp=r&sv=2021-08-06&sr=b&rscd=inline&rsct=image/png&skoid=6aaadede-4fb3-4698-a8f6-684d7786b067&sktid=a48cca56-e6da-484e-a814-9c849652bcb3&skt=2023-03-21T15%3A59%3A24Z&ske=2023-03-22T15%3A59%3A24Z&sks=b&skv=2021-08-06&sig=Ye5WMUlSDjfct%2B1lZyzSF3tNo7C1z5pjwXc2lIAlguc%3D"
    },
    {
      "url": "https://oaidalleapiprodscus.blob.core.windows.net/private/org-XRmzS8bsRXK0Bv3bm3ZWG2pV/user-ugLS0njlcPKNCv7yd2f45jWN/img-Y2l5d1lVHmseGapwqPKuHiEE.png?st=2023-03-22T06%3A08%3A55Z&se=2023-03-22T08%3A08%3A55Z&sp=r&sv=2021-08-06&sr=b&rscd=inline&rsct=image/png&skoid=6aaadede-4fb3-4698-a8f6-684d7786b067&sktid=a48cca56-e6da-484e-a814-9c849652bcb3&skt=2023-03-21T15%3A59%3A24Z&ske=2023-03-22T15%3A59%3A24Z&sks=b&skv=2021-08-06&sig=AMR27FWMhMus1y0xI6H0f6mjOnI5kpMf8K0l89GaY1U%3D"
    }
  ]
}
```

#### 编辑图片

`POST https://api.openai.com/v1/images/edits`

在给定的原始图片的基础上创建或修改图片。

支持的模型：

- DALL·E

主要参数：

- image：原始图片文件，必须是 PNG 格式的且必须是方形的，并且文件最大为 4 MB，如果没有传 mask，该图片还需要带透明度。
- mask：可选附加图片文件，必须带透明度，完全透明的区域表示图片应该被修改的位置，并且文件最大为 4 MB，且于 image 分辨率相同。
- prompt：提示词，就是要怎么修改图片。
- n：生成几张图。
- size：图片的分辨率大小。

#### 生成图片变体

`POST https://api.openai.com/v1/images/variations`

创建给定图片的变体。

支持的模型：

- DALL·E

主要参数：

- image：原始图片文件，必须是 PNG 格式的且必须是方形的，并且文件最大为 4 MB。
- n：生成几张图。
- size：图片的分辨率大小。

### Audio 语音转文字

#### 语音转文字

`POST https://api.openai.com/v1/audio/transcriptions`

将语音转换成所使用的语言文字。支持的语言如下，但有些语种的错误率高于 50%：

| 语言         | 英文        |
| ------------ | ----------- |
| 英语         | english     |
| 中文         | chinese     |
| 德语         | german      |
| 西班牙语     | spanish     |
| 俄语         | russian     |
| 韩语         | korean      |
| 法语         | french      |
| 日语         | japanese    |
| 葡萄牙语     | portuguese  |
| 土耳其语     | turkish     |
| 波兰语       | polish      |
| 加泰罗尼亚语 | catalan     |
| 荷兰语       | dutch       |
| 阿拉伯语     | arabic      |
| 瑞典语       | swedish     |
| 意大利语     | italian     |
| 印度尼西亚语 | indonesian  |
| 印地语       | hindi       |
| 芬兰语       | finnish     |
| 越南语       | vietnamese  |
| 希伯来语     | hebrew      |
| 乌克兰语     | ukrainian   |
| 希腊语       | greek       |
| 马来语       | malay       |
| 捷克语       | czech       |
| 罗马尼亚语   | romanian    |
| 丹麦语       | danish      |
| 匈牙利语     | hungarian   |
| 泰米尔语     | tamil       |
| 挪威语       | norwegian   |
| 泰语         | thai        |
| 乌尔都语     | urdu        |
| 克罗地亚语   | croatian    |
| 保加利亚语   | bulgarian   |
| 立陶宛语     | lithuanian  |
| 拉丁语       | latin       |
| 毛利语       | maori       |
| 马拉雅拉姆语 | malayalam   |
| 威尔士语     | welsh       |
| 斯洛伐克语   | slovak      |
| 泰卢固语     | telugu      |
| 波斯语       | persian     |
| 拉脱维亚语   | latvian     |
| 孟加拉语     | bengali     |
| 塞尔维亚语   | serbian     |
| 阿塞拜疆语   | azerbaijani |
| 斯洛文尼亚语 | slovenian   |
| 卡纳达语     | kannada     |
| 爱沙尼亚语   | estonian    |
| 马其顿语     | macedonian  |
| 布列塔尼语   | breton      |
| 巴斯克语     | basque      |
| 冰岛语       | icelandic   |
| 亚美尼亚语   | armenian    |
| 尼泊尔语     | nepali      |
| 蒙古语       | mongolian   |
| 波斯尼亚     | bosnian     |
| 哈萨克语     | kazakh      |
| 阿尔巴尼亚语 | albanian    |
| 斯瓦希里语   | swahili     |
| 加利西亚语   | galician    |
| 马拉地语     | marathi     |
| 旁遮普语     | punjabi     |
| 僧伽罗语     | sinhala     |
| 高棉语       | khmer       |
| 绍纳语       | shona       |
| 约鲁巴语     | yoruba      |
| 索马里语     | somali      |
| 南非荷兰语   | afrikaans   |
| 奥克语       | occitan     |
| 格鲁吉亚语   | georgian    |
| 白俄罗斯语   | belarusian  |
| 塔吉克语     | tajik       |
| 信德语       | sindhi      |
| 古吉拉特语   | gujarati    |
| 阿姆哈拉语   | amharic     |
| 意第绪语     | yiddish     |
| 老挝语       | lao         |
| 乌兹别克斯坦语 | uzbek    |
| 法罗语       | faroese     |
| 海地克里奥尔语 | haitian creole |
| 普什图语     | pashto      |
| 土库曼语     | turkmen     |
| 尼诺斯克语   | nynorsk     |
| 马耳他语     | maltese     |
| 梵语         | sanskrit    |
| 卢森堡语语   | luxembourgish |
| 缅甸语       | myanmar     |
| 西藏语       | tibetan     |
| 他加禄语     | tagalog     |
| 马达加斯加语 | malagasy    |
| 阿萨姆语     | assamese    |
| 鞑靼语       | tatar       |
| 夏威夷语     | hawaiian    |
| 林加拉语     | lingala     |
| 豪萨语       | hausa       |
| 巴什基尔语   | bashkir     |
| 爪哇语       | javanese    |
| 巽他语       | sundanese   |


支持的模型：

- whisper-1

主要参数：

- file：录音文件，支持的格式有：mp3, mp4, mpeg, mpga, m4a, wav, webm，文件最大为 25 MB。
- prompt：提示语，用于指导模型的风格或继续上一个音频片段的可选文本。需要和音频语言相匹配。
- language：音频的语言， **ISO-639-1** 格式，有助于提高准确率和速度。
- temperature：采样温度，介于 0 和 1 之间。值越高输出内容越随机，反之越稳定。

#### 语音翻译

`POST https://api.openai.com/v1/audio/translations`

目前只能翻译成英文。

支持的模型：

- whisper-1

主要参数：

- file：录音文件，支持的格式有：mp3, mp4, mpeg, mpga, m4a, wav, webm，文件最大为 25 MB。
- prompt：提示语，用于指导模型的风格或继续上一个音频片段的可选文本。需要是英语。
- temperature：采样温度，介于 0 和 1 之间。值越高输出内容越随机，反之越稳定。

## 速率限制

有两种衡量方式：

- **RPM**（每分钟请求数）
- **TPM**（每分钟令牌数）

|                       | **TEXT & EMBEDDING**    | **CHAT**               | **CODEX**          | **EDIT**            | **IMAGE**       | **AUDIO** |
| --------------------- | ----------------------- | ---------------------- | ------------------ | ------------------- | --------------- | --------- |
| 免费用户              | •20 RPM•150,000 TPM     | •20 RPM•40,000 TPM     | •20 RPM•40,000 TPM | •20 RPM•150,000 TPM | 50 images / min | 50 RPM    |
| 按量付费(前 48 小时)  | •60 RPM•250,000 TPM*    | •60 RPM•60,000 TPM*    | •20 RPM•40,000 TPM | •20 RPM•150,000 TPM | 50 images / min | 50 RPM    |
| 按量付费 (后 48 小时) | •3,500 RPM•350,000 TPM* | •3,500 RPM•90,000 TPM* | •20 RPM•40,000 TPM | •20 RPM•150,000 TPM | 50 images / min | 50 RPM    |

GPT-4 另外限制

| **gpt-4/gpt-4-0314**  | **gpt-4-32k/gpt-4-32k-0314** |
| --------------------- | ---------------------------- |
| • 200 RPM• 40,000 TPM | • 400 RPM• 80,000 TPM        |
<!-- ##{"timestamp":1679380929}## -->