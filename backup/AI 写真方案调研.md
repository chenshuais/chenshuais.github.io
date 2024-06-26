
## 一、换脸方案

> 核心技术都是使用 [roop](https://github.com/s0md3v/roop) 进行换脸，StableDiffusion 上也有对应的插件：[https://github.com/s0md3v/sd-webui-roop](https://github.com/s0md3v/sd-webui-roop)

整体大概流程：
`Gmeek-html<img src="https://blog-image.ian2018.cn/images/c30b25dc6c72bf1769b8be626f5b67729750943d.png">`


### 1.1 文生图 + 换脸

由 StableDiffusion 生成一张写真图，然后使用 roop 插件进行换脸。

#### StableDiffusion 关键参数预览：

`Gmeek-html<img src="https://blog-image.ian2018.cn/images/9405f6c5fea44303de3f27d416f24e7946f8be3f.png">`


```json
{
    "prompt":"提示词",
    "negative_prompt":"反向提示词",
    "sampler_index":"采样算法",
    "steps":50,
    "batch_size":1,
    "cfg_scale":7,
    "width":512,
    "height":683,
    "override_settings":{
        "sd_model_checkpoint":"大模型名字"
    },
    "alwayson_scripts":{
        // 使用 roop 插件
        "roop":{
            "args":[
                "base64 人脸图片",
                true, // 是否启用 roop
                "0", // 人脸下标（替换第几个人脸，多个用逗号分隔）
                "inswapper_128.onnx（换脸模型路径）",
                "CodeFormer", // 面部修复
                1, // 面部修复强度
                "放大算法",
                1, // 放大倍率
                1, // 放大强度
                false, // 替换原图片
                true // 替换生成后的图片
            ]
        }
    }
}
```

#### 生成效果示例：
`Gmeek-html<img src="https://blog-image.ian2018.cn/images/a180cbb320a0e9698e0a06984874e3a4dc0e51ee.png">`

#### 耗时情况：

在 4090 的显卡上，一张图的总耗时**大概在 23 秒左右**，其中换脸大概需要 7 秒。

#### 优点：

- 所需数据少，只需要一张人脸图片即可
- 生成时间短
- 技术部署难度较低，安装并使用 StableDiffusion roop 插件即可

#### 缺点：

- 对人脸的要求比较高，清晰的正脸效果会比较好
- 侧脸的效果不是很好
- 有些脸型会受到 SD 生成的底图的影响

#### 影响效果关键点：

- 人脸图片的质量

  - 上传图片之前可以先检测一下是不人脸
- SD 大模型的选择

  - 亚洲面孔比较好的模型：[https://www.liblibai.com/modelinfo/9470cdf92e33f5ad72cc2f40f834a69a](https://www.liblibai.com/modelinfo/9470cdf92e33f5ad72cc2f40f834a69a)
  - 西方面孔比较好的模型：[https://civitai.com/models/4201/realistic-vision-v51](https://civitai.com/models/4201/realistic-vision-v51)
- SD 提示词和各种参数

### 1.2 预设图片 + 换脸

流程和上面类似，只不过不通过 SD 生图了，直接使用预设好的图片进行换脸。

#### 预设图来源：

目前可能的有以下几种

- 设计师设计
- 通过 SD 或者 MJ 生成图片，然后交由设计师调整
- 无版权的写真相关图片

#### roop sd webui api 参数预览：

[https://github.com/s0md3v/sd-webui-roop/pull/214](https://github.com/s0md3v/sd-webui-roop/pull/214) roop 插件最近的更新中也已经提供了换脸的接口。

`Gmeek-html<img src="https://blog-image.ian2018.cn/images/cb281d09a5b3c823a17b8706d434ebf0adb6c8af.png">`

```json
{
    "source_image":"base64 人脸图片",
    "target_image":"base64 底图",
    "face_index":[0], // 人脸下标（替换第几个人脸）
    "scale":1, // 放大倍率
    "upscale_visibility":1, // 放大强度
    "face_restorer":"CodeFormer", // 面部修复
    "restorer_visibility":1, // 面部修复强度
    "model":"inswapper_128.onnx" // 换脸模型名字
}
```

返回的结果是一个 json：

```json
{
    "image":"base64 图片"
}
```

#### 生成效果示例：

`Gmeek-html<img src="https://blog-image.ian2018.cn/images/f15d933eed9735b66002bec63fa5d41f8f5cd4bd.png">`

#### 耗时情况：

在 4090 显卡上，**大概需要 7 秒左右**。

#### 优缺点：

缺点和 1.1 一样，优点是更快了，省去了 SD 生图的时间。

#### 影响效果关键点：

- 预设底图的质量
- 人脸图片的质量

### 1.3 图生图 + 换脸

将预设图片通过 SD 进行图生图，然后进行换脸。

`Gmeek-html<img src="https://blog-image.ian2018.cn/images/88e6ae54fb91e1a7f28c8325fe5bb369ec95324b.png">`

#### 1.3.1 roop StableDiffusion 关键参数预览：

```json
{
    "init_images":["底图 base64"],
    "resize_mode":0,
    "denoising_strength":0.4,
    "prompt":"提示词",
    "seed":-1,
    "batch_size":1,
    "steps":20,
    "cfg_scale":7,
    "width":512,
    "height":512,
    "negative_prompt":"NSFW",
    "override_settings":{
        "sd_model_checkpoint":"大模型"
    },
    "sampler_name":"采样名字",
    "alwayson_scripts":{
        "roop":{
            "args":[
                "人脸 base64",
                true,
                "0",
                "stable-diffusion-webui/models/roop/inswapper_128.onnx",
                "GFPGAN",
                1,
                "None",
                1,
                1,
                false,
                true
            ]
        }
    }
}
```

#### 1.3.2 FaceSwapLab StableDiffusion 关键参数预览：

```python
data = {
    # 模版底图
    "init_images": [get_base64_image(img_path)],
    "resize_mode": 0,
    "denoising_strength": 0.45,
    "prompt": "",
    "seed": -1,
    "batch_size": 4,
    "steps": 20,
    "cfg_scale": 7,
    "width": width,
    "height": height,
    "negative_prompt": "",
    "override_settings": {
        "sd_model_checkpoint": "deliberate_v2.safetensors [9aba26abdf]"
    },
    "save_images": False,
    "sampler_name": "DPM++ SDE Karras",
    "alwayson_scripts": {
        "faceswaplab": {
            "args": [
                # ============> Face 1 <============
                # Reference （人脸照片）
                get_base64_image('./test_img/t1.png'),
                # Face Checkpoint (precedence over reference face)
                None,
                # Batch Sources Images（待融合的人脸照片，可以有多张）
                f"{get_base64_image('./test_img/t1.png')},{get_base64_image('./test_img/t2.webp')},{get_base64_image('./test_img/t3.jpg')}",
                # Blend Faces ((Source|Checkpoint)+References = 1)
                True,
                # Enable
                True,
                # Same Gender
                False,
                # Sort by size (larger>smaller)
                True,
                # Check similarity
                True,
                # Compute similarity
                True,
                # Min similarity
                0.5,
                # "Min reference similarity"
                0.2,
                # Target face : Comma separated face number(s)
                "0",
                # Reference source face : start from 0
                0,
                # Swap in source image (blended face)
                True,
                # Swap in generated image
                True,
                # Denoising strenght
                0,
                # Inpainting prompt use [gender] instead of men or woman
                "Portrait of a [gender]",
                # Inpainting negative prompt use [gender] instead of men or woman
                "blurry",
                # Inpainting steps
                20,
                # Inpainting Sampler
                "Euler a",
                # sd model (experimental)
                None,
                # Inpainting seed
                0,
                # Restore Face
                "None",
                # Restore visibility
                1,
                # codeformer weight
                1,
                # Upscaler
                "",
                # Use improved segmented mask (use pastenet to mask only the face)
                False,
                # Use color corrections
                False,
                # sharpen face
                False,
                # Upscaled swapper mask erosion factor, 1 = default behaviour.
                1,
                # Denoising strenght
                0,
                # Inpainting prompt use [gender] instead of men or woman
                "Portrait of a [gender]",
                # Inpainting negative prompt use [gender] instead of men or woman
                "blurry",
                # Inpainting steps
                20,
                # Inpainting Sampler
                "Euler a",
                # sd model (experimental)
                None,
                # Inpainting seed
                0,

                # ============> Face 2 <============
                # Reference
                None,
                # Face Checkpoint (precedence over reference face)
                None,
                # Batch Sources Images
                None,
                # Blend Faces ((Source|Checkpoint)+References = 1)
                True,
                # Enable
                False,
                # Same Gender
                False,
                # Sort by size (larger>smaller)
                False,
                # Check similarity
                False,
                # Compute similarity
                False,
                # Min similarity
                0,
                # "Min reference similarity"
                0,
                # Target face : Comma separated face number(s)
                "1",
                # Reference source face : start from 0
                0,
                # Swap in source image (blended face)
                False,
                # Swap in generated image
                True,
                # Denoising strenght
                0,
                # Inpainting prompt use [gender] instead of men or woman
                "Portrait of a [gender]",
                # Inpainting negative prompt use [gender] instead of men or woman
                "blurry",
                # Inpainting steps
                20,
                # Inpainting Sampler
                "Euler a",
                # sd model (experimental)
                None,
                # Inpainting seed
                0,
                # Restore Face
                "None",
                # Restore visibility
                1,
                # codeformer weight
                1,
                # Upscaler
                "",
                # Use improved segmented mask (use pastenet to mask only the face)
                False,
                # Use color corrections
                False,
                # sharpen face
                False,
                # Upscaled swapper mask erosion factor, 1 = default behaviour.
                1,
                # Denoising strenght
                0,
                # Inpainting prompt use [gender] instead of men or woman
                "Portrait of a [gender]",
                # Inpainting negative prompt use [gender] instead of men or woman
                "blurry",
                # Inpainting steps
                20,
                # Inpainting Sampler
                "Euler a",
                # sd model (experimental)
                None,
                # Inpainting seed
                0,

                # ============> Face 3 <============
                # Reference
                None,
                # Face Checkpoint (precedence over reference face)
                None,
                # Batch Sources Images
                None,
                # Blend Faces ((Source|Checkpoint)+References = 1)
                True,
                # Enable
                False,
                # Same Gender
                False,
                # Sort by size (larger>smaller)
                False,
                # Check similarity
                False,
                # Compute similarity
                False,
                # Min similarity
                0,
                # "Min reference similarity"
                0,
                # Target face : Comma separated face number(s)
                "2",
                # Reference source face : start from 0
                0,
                # Swap in source image (blended face)
                False,
                # Swap in generated image
                True,
                # Denoising strenght
                0,
                # Inpainting prompt use [gender] instead of men or woman
                "Portrait of a [gender]",
                # Inpainting negative prompt use [gender] instead of men or woman
                "blurry",
                # Inpainting steps
                20,
                # Inpainting Sampler
                "Euler a",
                # sd model (experimental)
                None,
                # Inpainting seed
                0,
                # Restore Face
                "None",
                # Restore visibility
                1,
                # codeformer weight
                1,
                # Upscaler
                "",
                # Use improved segmented mask (use pastenet to mask only the face)
                False,
                # Use color corrections
                False,
                # sharpen face
                False,
                # Upscaled swapper mask erosion factor, 1 = default behaviour.
                1,
                # Denoising strenght
                0,
                # Inpainting prompt use [gender] instead of men or woman
                "Portrait of a [gender]",
                # Inpainting negative prompt use [gender] instead of men or woman
                "blurry",
                # Inpainting steps
                20,
                # Inpainting Sampler
                "Euler a",
                # sd model (experimental)
                None,
                # Inpainting seed
                0,

                # ============> Global Post-Processing <============
                # Restore Face
                "GFPGAN",
                # Restore visibility
                1,
                # codeformer weight
                1,
                # Upscaler
                "R-ESRGAN 4x+",
                # Upscaler scale
                1,
                # Upscaler visibility (if scale = 1)
                1,
                # Enable/When
                "After Upscaling/Before Restore Face",
                # Denoising strenght
                0,
                # Inpainting prompt use [gender] instead of men or woman
                "Portrait of a [gender]",
                # Inpainting negative prompt use [gender] instead of men or woman
                "blurry",
                # Inpainting steps
                20,
                # Inpainting Sampler
                "Euler a",
                # sd model (experimental)
                None,
                # Inpainting seed
                0
            ]
        }
    }
}
```

## 二、训练 LoRA 模型

需要用户上传 20 - 30 张左右的照片，然后训练成用户的人脸 LoRA。

#### 所用到的技术：

- 人脸检测：python opencv [https://github.com/kipr/opencv/blob/master/data/haarcascades/haarcascade_frontalface_default.xml](https://github.com/kipr/opencv/blob/master/data/haarcascades/haarcascade_frontalface_default.xml)
- 图片预处理（将图剪裁成 512*512）：通过 SD webui 的插件 api 实现 [https://github.com/AUTOMATIC1111/stable-diffusion-webui/blob/68f336bd994bed5442ad95bad6b6ad5564a5409a/modules/textual_inversion/preprocess.py#L10](https://github.com/AUTOMATIC1111/stable-diffusion-webui/blob/68f336bd994bed5442ad95bad6b6ad5564a5409a/modules/textual_inversion/preprocess.py#L10)
- 图片打 tag：[https://github.com/toriato/stable-diffusion-webui-wd14-tagger](https://github.com/toriato/stable-diffusion-webui-wd14-tagger)
- LoRA 训练脚本：[https://jihulab.com/Akegarasu/lora-scripts](https://jihulab.com/Akegarasu/lora-scripts)

#### 大致训练流程：
`Gmeek-html<img src="https://blog-image.ian2018.cn/images/4bd4665d10a0dccd05894c27a3f1351f54e5ca33.png">`


#### 生成效果示例：

训练参数：

- 大模型：[https://civitai.com/models/4201/realistic-vision-v51](https://civitai.com/models/4201/realistic-vision-v51)
- 图片数量：20
- 每张图学习次数：16
- 学习轮数：15
- batch size：2

结果测试提示词（使用的是数据源图片 03 的 tag 提示词）：

```
RAW photo, subject, 8k uhd, dslr, soft lighting, high quality, film grain, male focus, 1boy, jacket, shirt, black shirt, realistic, open clothes, facial hair, looking at viewer, open jacket, blurry background, short hair, solo,  <lora:ou_musk-0000X:Y>
```

结果 XY 对比（X 是第几个模型，Y 是 LoRA 权重）：

`Gmeek-html<img src="https://blog-image.ian2018.cn/images/35e839e2071741bd538f41082bcab40f8d74d5d9.png">`


#### 耗时情况：

20 张图片、每张图片学习 16 次、epochs 15、batch size 2 等参数，总训练步数 2400，在 4090 显卡上**大概 7.6 分钟**左右。

#### 影响效果关键点：

- 上传图片的质量
- 给图片打 tag 的准确性
- 训练参数（repeats、epochs、unet_lr、text_encoder_lr 等等）

#### 技术难点：

- 给图片打 tag 的准确性
- 不同图片的训练参数可能会不同，需要找到一个平衡点
- 训练后的模型一般会有多个，不一定是最后一个效果最好，最后选用哪个技术上不太好判定

  - 不过这个可以在产品上解决，训练结束后，用每个模型给用户生成一张图，让用户选择用哪个

## 三、训练 LoRA 模型 + 换脸

先通过训练好 LoRA 生成一张底图，然后使用 roop 插件换脸。

这种方案是上面两种的结合，如果 LoRA 模型训练的好，再加上人脸质量好，最后达到的效果就会很好。

但是难点也是 LoRA 训练这一步。

<!-- ##{"timestamp":1692930151}## -->