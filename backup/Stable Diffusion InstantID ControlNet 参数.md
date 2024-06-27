> [!TIP]
> sd-webui 版本： v1.7.0-408-g94d4b3c8
> controlnet 版本：v1.1.439

## 用到的模型下载

### 底模：
https://civitai.com/api/download/models/302806
### ControlNet：
将其放到 `stable-diffusion-webui/models/ControlNet` 下
- instant_id_face_embedding 预处理器所需模型：
  - https://huggingface.co/InstantX/InstantID/resolve/main/ip-adapter.bin?download=true
  - 下载后需要重命名成 ：`ip-adapter_instant_id_sdx.bin`
- instant_id_face_keypoints 预处理器所需模型：
  - https://huggingface.co/InstantX/InstantID/resolve/main/ControlNetModel/diffusion_pytorch_model.safetensors?download=true
  - 下载后需要重命名成 ：`control_instant_id_sdxl.safetensors`

## 任意风格：

### SD-WebUI 参数
类型：文生图/图生图

底模：dreamshaperXL_sfwTurboDpmppSDE.safetensors [3a22504d8a]

提示词 & 负向提示词：可以直接使用 Styles

采样器：DPM++ SDE Karras

步数：6

CFG Scale：2

### ControlNet 参数

这一步是提取人脸信息
`Gmeek-html<img src="https://blog-image.ian2018.cn/images/01798863202fc45eac5330d73ec6b1b3c7447c32.png">`

这一步是提取目标图片中人脸的位置（一般是放模板图）
`Gmeek-html<img src="https://blog-image.ian2018.cn/images/b08051af9cf1042dbf20fe09699d63251a9bd1ea.png">`

### API 参数

```python
url = 'http://127.0.0.1:7860/sdapi/v1/txt2img'
img_path = './test_img/t4.png'
data = {
    "prompt": "1man",
    "styles": [
        "SAI Anime"
    ],
    "seed": -1,
    "batch_size": 1,
    "n_iter": 1,
    "steps": 6,
    "cfg_scale": 2,
    "width": 1024,
    "height": 1024,
    "override_settings": {
        "sd_model_checkpoint": "dreamshaperXL_sfwTurboDpmppSDE.safetensors [3a22504d8a]"
    },
    "save_images": False,
    "sampler_name": "DPM++ SDE Karras",
    "alwayson_scripts": {
        "controlnet": {
            "args": [
                {
                    "input_image": get_base64_image(img_path),
                    "module": "instant_id_face_embedding",
                    "model": "ip-adapter_instant_id_sdx [eb2d3ec0]",
                    "processor_res": 512,
                    "weight": 1,
                    "guidance_start": 0.0,
                    "guidance_end": 1.0,
                    "control_mode": 0
                },
                {
                    "input_image": get_base64_image(img_path),
                    "module": "instant_id_face_keypoints",
                    "model": "control_instant_id_sdxl [c5c25a50]",
                    "processor_res": 512,
                    "weight": 1,
                    "guidance_start": 0,
                    "guidance_end": 1,
                    "control_mode": 0
                }
            ]
        }
    }
}
```

<!-- ##{"timestamp":1706576591}## -->