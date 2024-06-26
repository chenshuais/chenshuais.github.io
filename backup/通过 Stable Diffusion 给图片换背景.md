# 方案一：rembg + img2img Inpaint

> 参考 https://stable-diffusion-art.com/change-background/

## 1. 下载安装 rembg 插件
https://github.com/AUTOMATIC1111/stable-diffusion-webui-rembg

## 2. 生成蒙版图

`Gmeek-html<img src="https://blog-image.ian2018.cn/images/0765daa81db8d036a0060ba8ad908d01359b7a4f.png">`

## 3. 使用图生图重绘背景
`Gmeek-html<img src="https://blog-image.ian2018.cn/images/6b03c44dd15fa6574b9453a225f134ea28de0270.png">`
还可以增加 ControlNet 的深度控制，可以避免出现奇怪的东西（多只胳膊之类的），但会导致出图结构受限：
`Gmeek-html<img src="https://blog-image.ian2018.cn/images/3bbf161c6bef99b787524780ff7d16990dafab0d.png">`

# 方案二：Segment Anything 
## 1. 下载安装 Segment Anything  插件
https://github.com/continue-revolution/sd-webui-segment-anything

## 2. 提取相关蒙版图
切换到 img2img Tab，找到 Segment Anything 插件，参数如下：
`Gmeek-html<img src="https://blog-image.ian2018.cn/images/dce3cdbc03593af9f0a829b610a811fc79e7ba0c.png">`
这里可以直接将第 7 不勾选上，这样就不用再保存蒙版图了。

Segment Anything 还可以提取其他物体，比如衣服、帽子之类的，可以用来换装。

## 3. 还是用图生图重绘

`Gmeek-html<img src="https://blog-image.ian2018.cn/images/dee056fea8ad3b926640ca80653f98e13cd410a6.png">`

<!-- ##{"timestamp":1705316951}## -->