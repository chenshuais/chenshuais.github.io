# 1 Google Cloud - Cloud Vision API - SafeSearch

https://cloud.google.com/vision/docs/detecting-safe-search?hl=en

## 1.1 检测方式

REST 或者客户端 SDK。
可以一次性检查图片在以下 5 个种类的可能性：
- adult（成人）
- spoof（欺诈）
- medical（医疗）
- violence（暴力）
- racy（恶意内容）

[检测的几种结果](https://cloud.google.com/vision/docs/reference/rpc/google.cloud.vision.v1#google.cloud.vision.v1.Likelihood)


| 类型 | 含义 |
|--------|--------|
| UNKNOWN | 未知的可能性。|
| VERY_UNLIKELY | 这是极不可能的。|
| UNLIKELY | 不太可能。|
| POSSIBLE | 有可能的。|
| LIKELY | 这有可能。|
| VERY_LIKELY |这很有可能。|


提供了两种检测方式：
1. 将本地图片转成 Base64 编码格式，请求 API 接口，获取图片的可能性。
2. 直接检查远端图片，如 Google Cloud Storage 中的图片，或者其他的图片 URL 地址。

## 1.2 价格

https://cloud.google.com/vision/pricing?hl=en

`Gmeek-html<img src="https://blog-image.ian2018.cn/images/08f72e5ebc602e2a11aab9f39a550b6af6b42430.png">`

## 1.3 使用

- 到 GCP 上启用 Cloud Vision API 服务
- 获取凭据中的 [API 密钥](https://cloud.google.com/docs/authentication/api-keys)

# 2 Microsoft - Content Moderator

https://azure.microsoft.com/en-us/services/cognitive-services/content-moderator/

## 2.1 检测方式

[Cognitive Services APIs Reference](https://westus.dev.cognitive.microsoft.com/docs/services/57cf753a3f9b070c105bd2c1/operations/57cf753a3f9b070868a1f66c)

API 接口方式，域名支持按地区访问。请求可以是图片的 URL 地址，或者图片源文件（MIEI types）。

## 2.2 价格

https://azure.microsoft.com/en-us/pricing/details/cognitive-services/content-moderator/

根据不同地区定价，以 East US 为例。

`Gmeek-html<img src="https://blog-image.ian2018.cn/images/0668d5b7c4ed4f749841ec8b139dde479e17fa5e.png">`

# 3 AWS - Amazon Rekognition Image

https://docs.aws.amazon.com/rekognition/latest/dg/moderation.html

## 3.1 检测方式

https://docs.aws.amazon.com/rekognition/latest/dg/procedure-moderate-images.html

需要先上传到 S3 ，然后使用它的客户端 SDK 去检测图片的敏感标签。

## 3.2 价格

https://aws.amazon.com/rekognition/pricing/?nc1=h_ls

可以先免费试用一年，每月有 5000 张图片检测。

根据不同地区定价，以 East US 为例。

`Gmeek-html<img src="https://blog-image.ian2018.cn/images/6e99299f8b02261e83d5783a1760ebac0ad2b49b.png">`

# 4 腾讯云 - 图片内容安全 / 数据万象图片审核

## 4.1 图片内容安全

https://cloud.tencent.com/document/product/1125

### 4.1.1 检测方式

接口支持按地域访问，支持境外地区，如北美、欧洲等地。

API 接口形式，可以传图片 Base64 编码，或者图片的 URL 地址。可以返回图片的标签和置信度打分。
- Normal（正常）
- Porn（色情）
- Abuse（谩骂）
- Ad（广告）
- Custom（自定义图片）
- 其他恶意图片

### 4.1.2 价格

https://cloud.tencent.com/document/product/1125/37108

采用预付费方式，超出套餐后按量计费（¥25 / 万张），可以重复购买套餐。

`Gmeek-html<img src="https://blog-image.ian2018.cn/images/cb68a386c87259921cebaa76c058999f6adbf673.png">`

## 4.2 数据万象图片审核

https://cloud.tencent.com/document/product/460

### 4.2.1 检测方式

需要开通对象存储服务，然后在控制后台打开图片审核功能，在上传到 Bucket 时，会自动审核图片。后台还可以设置检测类型、置信度回调接口等配置。

也可以使用 API 接口检测 Bucket 中的图片。

检测到敏感图片后，该资源无法公开访问。

### 4.2.2 价格

https://cloud.tencent.com/document/product/460/6970

分为按量计费和预付费两种。预付费方式只适用中国大陆地区。

`Gmeek-html<img src="https://blog-image.ian2018.cn/images/fbcd747ec46b166914d05c15495d1231f33b1a2e.png">`

流量费用，分境内和境外，以下为境外费用。

`Gmeek-html<img src="https://blog-image.ian2018.cn/images/58510f052a857848e9b10df8354a43541a7a76c8.png">`


# 5 Alibaba Cloud - Content Moderation

https://www.alibabacloud.com/product/content-moderation

## 5.1 检测方式

https://www.alibabacloud.com/help/zh/doc-detail/70292.htm?spm=a2c63.p38356.b99.35.12dd7cab0805Ta

有 API 和 SDK 两种方式。只能传图片的 URL 地址。有同步和异步两种方式，可以指定检测类型：
- porn：图片智能鉴黄
- terrorism：图片暴恐涉政
- ad：图文违规
- qrcode：图片二维码
- live：图片不良场景
- logo：图片logo

## 5.2 价格

https://www.alibabacloud.com/zh/product/content-moderation/pricing#producttabimage-0

按量计费，每日计算。每个账户首月每日有 3000 张图片检测额度，超出后收费。

`Gmeek-html<img src="https://blog-image.ian2018.cn/images/76302881b99bd3ccce7bb6bc985c98a900a07dcc.png">`

# 6 百度 - 图像审核

https://ai.baidu.com/tech/imagecensoring

## 6.1 检测方式

https://ai.baidu.com/ai-doc/ANTIPORN/ek3h6x90n

https://ai.baidu.com/ai-doc/ANTIPORN/jk42xep4e

API 接口方式，支持传图片 Base64 编码和图片 URL 地址。可以自定义审核策略（在后台配置），默认策略如下：
- 百度官方违禁图库
- 色情识别（默认违规标签包含：一般色情、卡通色情、SM、低俗、儿童裸露、艺术品色情、性玩具）
- 暴恐识别（默认违规标签包含：警察部队、血腥、尸体、爆炸火灾、杀人、暴乱、暴恐人物、军事武器、暴恐旗帜、血腥动物或动物尸体、车祸）
- 政治人物识别

## 6.2 价格

https://ai.baidu.com/ai-doc/ANTIPORN/lk3h6x7if

按审核策略使用了哪些规则收费。每次请求各项服务累加计算，如果用了色情、暴恐、旗帜标志三个，则每千张收费 0.25 + 0.38 + 0.56 = 1.19 。

`Gmeek-html<img src="https://blog-image.ian2018.cn/images/1d8ed3b77aded58f92ca03f6407583783acdb4f5.png">`

# 7 七牛云

## 7.1 检测方式

https://developer.qiniu.com/censor/5588/image-censor

API 接口方式，支持图片 Base64 编码和图片 URL。可以分类型检测。
- pulp（图片鉴黄）
- terror（图片鉴暴恐）
- politician（图片敏感人物识别）
- ads（图片广告识别）

## 7.2 价格

https://developer.qiniu.com/censor/4833/censor-price

按检测类型收费，使用了多种类型则进行累加计算，如果四种都检测，则每千张收费 0.85 + 0.85 + 0.85 + 0.85 = 3.4。

`Gmeek-html<img src="https://blog-image.ian2018.cn/images/3b609b40a79b177bef336b3bb239fb849db4ee02.png">`

<!-- ##{"timestamp":1616490743}## -->