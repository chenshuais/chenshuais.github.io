还是使用图生图，需要用到 ControlNet 插件，使用 [IP-Adapter](https://ip-adapter.github.io/) 对人脸进行控制，具体参数设置如下：

`Gmeek-html<img src="https://blog-image.ian2018.cn/images/e01db614b6ce1f6d27ecb99f6b7cdb62aaa2ef51.png">`

参数：

dreamshaperXL_turboDpmppSDE

Sampling method：DPM++ SDE Karras

Sampling steps：6

Width * Height：1024x1400

CFG Scale：2

Seed：2104314926

Denoising strength：0.4

ControlNet IP-Adapter Preprocessor：ip-adapter_clip_sdxl_plus_vith

ControlNet IP-Adapter Model：[ip-adapter-plus-face_sdxl_vit-h [c60d7d48]](https://huggingface.co/h94/IP-Adapter/resolve/main/sdxl_models/ip-adapter-plus-face_sdxl_vit-h.bin?download=true)

ControlNet Control Weight：0.75

ControlNet Ending Control Step：0.9

效果：
`Gmeek-html<img src="https://blog-image.ian2018.cn/images/72f0fee15808bbbd63b53d298d5199ffdbd3860d.png">`


<!-- ##{"timestamp":1702951291}## -->