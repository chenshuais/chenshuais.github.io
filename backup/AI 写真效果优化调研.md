## 模版新方案

不再使用图生图的方式，而是采用 **模版 ****tag**** 反推** + **文生图** + **ControlNet Pose 控制姿势** + **换脸**

该方案是为了解决图生图方案导致的画面不真实的问题，采用合适的大模型，也能顺带缓解换脸丑的问题（因为模型生成的脸型都比较好看，相当于先做了一次美颜，然后换脸）

### SD 具体实施步骤

#### 一、使用 [wd14-tagger](https://github.com/picobyte/stable-diffusion-webui-wd14-tagger) 插件对模版图片进行 tag 反推

`Gmeek-html<img src="https://blog-image.ian2018.cn/images/374886d1e7c58b556feb8bd7ad8fa410b88d1c9b.png">`

#### 二、生成 ControlNet 骨架图

这一步也可以直接放文生图里，这里提前生成可以不用进行预处理，减少一点生成时间。

`Gmeek-html<img src="https://blog-image.ian2018.cn/images/07dd6eacdddc76601351eabc10f3f717c70e3b99.png">`

直接点击文生图的生成按钮，结果的第二张就是骨架图：

`Gmeek-html<img src="https://blog-image.ian2018.cn/images/f5227e0edeb5a2679d1658692ec920fcc7f6e5ad.png">`

#### 三、文生图 + ControlNet** **+ FaceSwapLab

##### 这里用了摄影写实风格的大模型：[majicMIX realistic](https://civitai.com/models/43331?modelVersionId=176425)，文生图参数如下：

Checkpoint: **majicMIX_v7_realistic**

Sampler: **Euler a**

Steps: **20**

Hires upscaler: **ESRGAN 4x**

Hires upscale: **2**

Hires steps: **15**

Hires denoising strength: **0.25**

Width: 512

Height: 700

CFG Scale: 7

**clip skip: 2 （全局设置，本地 sd 上已经设置好了）**

Prompt: 1girl, solo, flower, long hair, realistic, blonde hair, dress, lips, blue eyes, wavy hair, bouquet, blurry, looking away, looking to the side, head wreath, upper body, blurry background, breasts, white dress, nose, holding

Negative prompt: (worst quality:2), (low quality:2), (normal quality:2), lowres, watermark,  nsfw, cartoon, painting, illustration, bad body, ugly

##### 打开 ControlNet，直接使用骨架图

`Gmeek-html<img src="https://blog-image.ian2018.cn/images/2197a870079060df0c91c95df1f8280c1be91c4b.png">`


##### 设置 FaceSwapLab 换脸参数，缓解锯齿问题

`Gmeek-html<img src="https://blog-image.ian2018.cn/images/5a249fb39145c92811587ae748fde439b8077cf5.png">`
`Gmeek-html<img src="https://blog-image.ian2018.cn/images/83716e54f2dfd5be41701b6623c79497649fc2c8.png">`
`Gmeek-html<img src="https://blog-image.ian2018.cn/images/7f7a2608adf16859343db94b50df2a7e5d414dcb.png">`


#### 四、结果预览

女性：

`Gmeek-html<img src="https://blog-image.ian2018.cn/images/83be71478b38be54235064aa8ca09ccc9dfa9736.png">`


`Gmeek-html<img src="https://blog-image.ian2018.cn/images/8b33799523e5f74e15057ea883cb590d0a0a6509.png">`
`Gmeek-html<img src="https://blog-image.ian2018.cn/images/9aa61184103ea13e98e29346135138c0ca26d8de.png">`


男性（模型更偏向女性，这里在提示词里加了 性别控制的 embedding [https://civitai.com/models/89709?modelVersionId=95532](https://civitai.com/models/89709?modelVersionId=95532)）：

`Gmeek-html<img src="https://blog-image.ian2018.cn/images/0b174a3e870af9185dab545d30d743044f6f00c4.png">`


`Gmeek-html<img src="https://blog-image.ian2018.cn/images/1488b6bb2b4fbccc46d01a0db293f08a2e1061cf.png">`
`Gmeek-html<img src="https://blog-image.ian2018.cn/images/5eea42131de903cb815c64bfaa50d7f646f9b6a0.png">`


#### 五、细节调整 LoRA 测试

可以增加图像的细节，看起来更真实

LoRA：[https://civitai.com/models/58390/detail-tweaker-lora-lora](https://civitai.com/models/58390/detail-tweaker-lora-lora)

`Gmeek-html<img src="https://blog-image.ian2018.cn/images/ed792f809622e7e911c6acc358acbbc9346d9d12.png">`


#### 六、其他大模型测试：

##### 使用 [deliberate_v2](https://tensor.art/models/596158958383661078) 模型测试：

Checkpoint: **deliberate_v2**

Sampler: **DPM++ SDE Karras**

Steps: **25**

Hires upscaler: none

Width: 512

Height: 700

CFG Scale: 7

`Gmeek-html<img src="https://blog-image.ian2018.cn/images/f7fa20fad9f1e505ff9e5b227600b03e9bf799b4.png">`


##### 使用 [deliberate_v3](https://tensor.art/models/634494689980086052) 模型测试：

Checkpoint: **deliberate_v3**

Sampler: **DPM++ SDE Karras**

Steps: **25**

Hires upscaler: none

Width: 512

Height: 700

CFG Scale: 7

`Gmeek-html<img src="https://blog-image.ian2018.cn/images/ffa4d3f0ba6c0f0cd3f7b9a80cccd559aa30b6dc.png">`


##### 使用 [reliberate_v2](https://tensor.art/models/623683531446469158) 模型测试：

Checkpoint: **reliberate_v20**

Sampler: **DPM++ 2M**

Steps: **24**

Hires upscaler: none

Width: 512

Height: 700

CFG Scale: 6.5

`Gmeek-html<img src="https://blog-image.ian2018.cn/images/025b67f4c23ac839dff00a6ddab47865e2d2b9a3.png">`


##### 使用 [realistic-vision-v51](https://civitai.com/models/4201/realistic-vision-v51) 模型测试：

Checkpoint: **realisticVisionV51_v51VAE**

Sampler: **DPM++ SDE Karra**

Steps: **25**

Hires upscaler: **4x-UltraSharp**

Hires upscale: **2**

Hires steps: **10**

Hires denoising strength: **0.25**

Width: 512

Height: 700

CFG Scale: 7

Prompt: RAW photo, subject, 8k uhd, dslr, soft lighting, high quality, film grain, Fujifilm XT3, solo, 1boy, realistic, male focus, necktie, formal, suit, blue background, brown hair, upper body, blue necktie, shirt, brown eyes, looking to the side, white shirt, simple background, collared shirt, jacket, blue jacket, short hair, facial hair, closed mouth

Negative prompt: (deformed iris, deformed pupils, semi-realistic, cgi, 3d, render, sketch, cartoon, drawing, anime), text, cropped, out of frame, worst quality, low quality, jpeg artifacts, ugly, duplicate, morbid, mutilated, extra fingers, mutated hands, poorly drawn hands, poorly drawn face, mutation, deformed, blurry, dehydrated, bad anatomy, bad proportions, extra limbs, cloned face, disfigured, gross proportions, malformed limbs, missing arms, missing legs, extra arms, extra legs, fused fingers, too many fingers, long neck, BadDream

`Gmeek-html<img src="https://blog-image.ian2018.cn/images/ab71be42d06a52b0f72c18873542d775191df215.png">`

##### 使用 [a-zovya-photoreal](https://civitai.com/models/57319/a-zovya-photoreal) 模型测试：

Checkpoint: **a-zovya-photoreal**

Sampler: **DPM++ SDE Karra**

Steps: **25**

Hires upscaler: none

Width: 512

Height: 700

CFG Scale: 7

Prompt: solo, 1boy, realistic, male focus, necktie, formal, suit, blue background, brown hair, upper body, blue necktie, shirt, brown eyes, looking to the side, white shirt, simple background, collared shirt, jacket, blue jacket, short hair, facial hair, closed mouth, 200mm zoom lens f/1.4 (masterpiece:1.2) (photorealistic:1.2) (bokeh) (best quality) (detailed skin:1.3) (intricate details) (8k) (HDR) (analog film) (canon d5) (cinematic lighting) (sharp focus)

Negative prompt: nsfw, (monochrome) (bad hands) (disfigured) (grain) (Deformed) (poorly drawn) (mutilated) (lowres) (deformed) (dark) (lowpoly) (CG) (3d) (blurry) (duplicate) (watermark) (label) (signature) (frames) (text)

`Gmeek-html<img src="https://blog-image.ian2018.cn/images/4b2c98adf25fdad0dcd171cb18faa77594a393a0.png">`


##### 使用 [dreamshaper](https://civitai.com/models/4384/dreamshaper) （可以生成一些奇幻色彩的）模型测试：

Checkpoint: **dreamshaper**

Sampler: **DPM++ SDE Karra**

Steps: **30**

Hires upscaler: none

Width: 512

Height: 700

CFG Scale: 9

Prompt: (masterpiece), (extremely intricate:1.3), (realistic), solo, 1boy, realistic, male focus, necktie, formal, suit, blue background, brown hair, upper body, blue necktie, shirt, brown eyes, looking to the side, white shirt, simple background, collared shirt, jacket, blue jacket, short hair, facial hair, closed mouth

Negative prompt: BadDream, (UnrealisticDream:1.3)

`Gmeek-html<img src="https://blog-image.ian2018.cn/images/19d83b9554085d203f58136baf81aae1d09bfcc7.png">`


## 文生图写真改进方案

### 针对半身照的效果好，全身照的效果差

使用 [ADetailer](https://github.com/Bing-su/adetailer) 插件对面部进行重绘

#### SD 参数：

Checkpoint: **deliberate_v2**

Sampler: **DPM++ SDE Karra**

Steps: **25**

Hires upscaler: **none**

Width: **512**

Height: **700**

CFG Scale: **3.5**

Seed: **1226343406**

Prompt: **Man,handsome,mixed race,travel,(full body), natural skin texture, 24mm, 4k textures, soft cinematic light, adobe lightroom , photo lab , hdr, intricate, elegant, highly detailed, sharp focus, soothing tones, insane details, intricate details, hyperdetailed, low contrast, dim colors, exposure blend, hdr, (solo), [lora:aiphoto_v2:0.99](lora:aiphoto_v2:0.99)**

Negative prompt: **nsfw,cartoon, painting, illustration, (worst quality),  (low quality),  (normal quality),bad body,ugly**

#### 开启 ADetailer 插件

`Gmeek-html<img src="https://blog-image.ian2018.cn/images/ff4b38dc405fde7658bd46243a27afaa2216c9a4.png">`


#### 结果对比

`Gmeek-html<img src="https://blog-image.ian2018.cn/images/23da4e22d787d0bba0138eb530128f357dfa59a5.png">`

### 针对手部经常出现畸形

#### 方案一

在负向提示词增加手部的 embedding：[badhandv4](https://civitai.com/models/16993/badhandv4-animeillustdiffusion)

其他参数一样，ADetailer 1st 开启面部重绘模型

Negative prompt: **nsfw,cartoon, painting, illustration, (worst quality),  (low quality),  (normal quality),bad body,ugly, ****badhandv4**

这种会对画风内容有些影响，下面可以看到相同的参数，加了 embedding 导致了人物和服装都发生了变化。

`Gmeek-html<img src="https://blog-image.ian2018.cn/images/365e10f5ba0437b46e2aab83a94b9ef7867dcac8.png">`


#### 方案二

用 [ADetailer](https://github.com/Bing-su/adetailer) 对手部进行重绘，把 badhandv4 放到重绘时的反向提示词中

`Gmeek-html<img src="https://blog-image.ian2018.cn/images/0a720c1c61eca5d0c827ff5f21b452ea60d32b8c.png">`


`Gmeek-html<img src="https://blog-image.ian2018.cn/images/57218ccf942b44b21e5edfe359bdec031746e9fd.png">`


### 针对性别问题

使用性别控制的 embedding [https://civitai.com/models/89709?modelVersionId=95532](https://civitai.com/models/89709?modelVersionId=95532) 修正一下。这块就需要服务端根据提示词过滤一下是否有男性相关的，有的话再拼接一下 embedding 提示词。

#### SD 参数：

Checkpoint: **deliberate_v2**

Sampler: **DPM++ SDE Karra**

Steps: **25**

Hires upscaler: **none**

Width: **512**

Height: **700**

CFG Scale: **3.5**

Seed: **1553387558**

Prompt: **Man, natural skin texture, 24mm, 4k textures, soft cinematic light, adobe lightroom , photo lab , hdr, intricate, elegant, highly detailed, sharp focus, soothing tones, insane details, intricate details, hyperdetailed, low contrast, dim colors, exposure blend, hdr, (solo), [lora:aiphoto_v2:0.99](lora:aiphoto_v2:0.99)**

Negative prompt: **nsfw,cartoon, painting, illustration, (worst quality),  (low quality),  (normal quality),bad body,ugly, ****GS-DeFeminize-neg**

#### 结果对比：

`Gmeek-html<img src="https://blog-image.ian2018.cn/images/782b8c01a5add81964dfc441e403cc31dd434afa.png">`


#### Gender Slider 性别滑块使用方法：

如果想让角色人物更加男性化，加入如下提示词：

| 正面：                         | 负面：                            |
| ------------------------------ | --------------------------------- |
| GS-Boyish（男孩）              | GS-DeFeminize-Neg（移除女性性征） |
| GS-Masculine（粗犷的成熟男性） |                                   |
| 权重一般为 0.2~1.3             | 权重一般为 1.0~1.3                |

如果想让角色人物变得更年轻/年老，加入如下提示词：

| 正面：                   | 负面：                             |
| ------------------------ | ---------------------------------- |
| GS-Girlish（可爱的女孩） | GS-DeMasculate-Neg（移除男性性征） |
| GS-Womanly（成熟女性）   |                                    |
| 权重一般为 0.1~1.3       | 权重一般为 1.0~1.3                 |

### 针对偶尔出的图脸部是不完整的问题

这个单通过改提示词不太好弄，可以用 ControlNet 控制一下姿势，就能避免出一些奇怪的图，但这样姿势就不能随机了，就是固定提供的那几个。


<!-- ##{"timestamp":1698811291}## -->