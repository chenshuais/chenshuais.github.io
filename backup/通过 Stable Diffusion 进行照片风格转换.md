> 经过几次测试，其中图生图的效果会比较好
>
> ControlNet 使用 3 层控制：Canny + Depth + IP-Adapter

## 用到的模型下载
### 底模：
https://civitai.com/api/download/models/302806

### ControlNet：
将其放到 `stable-diffusion-webui/extensions/sd-webui-controlnet/models` 或者 `stable-diffusion-webui/models/ControlNet` 下
- Canny：
  - https://huggingface.co/lllyasviel/sd_control_collection/resolve/main/sai_xl_canny_256lora.safetensors?download=true
- Depth：
  - https://huggingface.co/lllyasviel/sd_control_collection/resolve/main/sai_xl_depth_128lora.safetensors?download=true
- IP-Adapter Face：
  - https://huggingface.co/h94/IP-Adapter/resolve/main/sdxl_models/ip-adapter-plus-face_sdxl_vit-h.safetensors?download=true


## 参数详情：

### 图生图参数：
Stable Diffusion checkpoint：dreamshaperXL_turboDpmppSDE.safetensors [676f0d60c8]

`Gmeek-html<img src="https://blog-image.ian2018.cn/images/fbebebdd321dcdc173131becdbd50065e0f4a825.png">`

### ControlNet 参数：

`Gmeek-html<img src="https://blog-image.ian2018.cn/images/126b0ed20d343c23dd28b248a95e362d61958b6e.png">`
`Gmeek-html<img src="https://blog-image.ian2018.cn/images/e4bda592201f46be6ad68585a302221d2c9067f2.png">`
`Gmeek-html<img src="https://blog-image.ian2018.cn/images/9bdf5fa060ac6c3ebc6184dd7757c82cd0eefddb.png">`

## 结果展示：

### 动画风格，Style：SAI Anime

`Gmeek-html<img src="https://blog-image.ian2018.cn/images/72a2f6b2f2f4220c959eacd09304661b38c5484b.png">`

`Gmeek-html<img src="https://blog-image.ian2018.cn/images/5619a8bd240c0b28aa16029e7da1e86f606a112e.png">`

### 3D 风格，Style：SAI 3D Model，SAI Disney Model

`Gmeek-html<img src="https://blog-image.ian2018.cn/images/96d873e4c573ef84d44d19ef3bd6ba694b3a20ae.png">`

`Gmeek-html<img src="https://blog-image.ian2018.cn/images/0e4e761661bd4ff8575b00e8cb70ae020ef80421.png">`

### 复古游戏风，Style：Game Retro Game

`Gmeek-html<img src="https://blog-image.ian2018.cn/images/935c2ca2d8bf33f56bd16279bcb981f0e942a792.png">`

`Gmeek-html<img src="https://blog-image.ian2018.cn/images/3b7bf90b822424333ae703ce82147571dbfe0317.png">`

## API 参数

```python
url = 'http://127.0.0.1:7860/sdapi/v1/img2img'
img_path = './test_img/t4.png'
data = {
    "init_images": [get_base64_image(img_path)],
    "resize_mode": 0,
    "denoising_strength": 0.7,
    "prompt": "(anime artwork), (anime style), key visual, vibrant, studio anime, highly detailed",
    "negative_prompt": "photo, deformed, black and white, realism, disfigured, low contrast",
    "seed": -1,
    "batch_size": 1,
    "n_iter": 1,
    "steps": 6,
    "cfg_scale": 2,
    "width": 1024,
    "height": 1280,
    "override_settings": {
        "sd_model_checkpoint": "dreamshaperXL_turboDpmppSDE.safetensors [676f0d60c8]"
    },
    "save_images": False,
    "sampler_name": "DPM++ SDE Karras",
    "alwayson_scripts": {
        "controlnet": {
            "args": [
                {
                    "module": "canny",
                    "model": "sai_xl_canny_256lora [566f20af]",
                    "processor_res": 512,
                    "weight": 0.8,
                    "guidance_start": 0.0,
                    "guidance_end": 1.0,
                    "threshold_a": 100,
                    "threshold_b": 200,
                    "control_mode": 0
                },
                {
                    "module": "depth_midas",
                    "model": "sai_xl_depth_128lora [f5ddde75]",
                    "processor_res": 512,
                    "weight": 0.3,
                    "guidance_start": 0.2,
                    "guidance_end": 0.6,
                    "control_mode": 0
                },
                {
                    "module": "ip-adapter_clip_sdxl_plus_vith",
                    "model": "ip-adapter-plus-face_sdxl_vit-h [c60d7d48]",
                    "processor_res": 512,
                    "weight": 0.3,
                    "guidance_start": 0.3,
                    "guidance_end": 0.8,
                    "control_mode": 0
                }
            ]
        }
    }
}
```
<!-- ##{"timestamp":1703228951}## -->