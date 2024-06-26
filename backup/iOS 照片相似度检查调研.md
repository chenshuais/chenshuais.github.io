## 照片相似度检查

参考文章：
> [OpenCV 相似度算法](https://juejin.cn/post/6969457808378953735)
>
> [苹果机器学习判断图片相似度](https://cloud.tencent.com/developer/article/2277725)
>
> [OpenCV 相似度算法2](https://www.cnblogs.com/yangyuxiaozi/p/7419349.html)
>
> [OpenCV](https://medium.com/@hdpoorna/integrating-opencv-to-your-swift-ios-project-in-xcode-and-working-with-uiimages-4c614e62ac88)

参考项目：
> https://github.com/coderFrankenstain/WJSimilarPhotos
>
> https://github.com/ZYHshao/MachineLearnDemo
>
> https://github.com/ameingast/cocoaimagehashing
>
> https://github.com/ameingast/cocoaimagehashing/issues/18
>
> https://github.com/yeatse/opencv-spm

### 图片匹配算法

`Gmeek-html<img src="https://blog-image.ian2018.cn/images/d42335aa1006b6f1f2f0443fccd625ce6761f61a.png">`
`Gmeek-html<img src="https://blog-image.ian2018.cn/images/fa7395e7c8313e9eb9f8605eb889d7fc8f5dc525.png">`

### 图片相似度检测方案

#### 方案一、通过 OpenCV 实现

识别算法
1. 利用 PHASH 算法（感知哈希算法，通常用于图片相似度识别）对比图片轮廓相似度。
2. 利用图片的 HSV 直方图，对比图片颜色上的相似度。
3. 利用 ORB 特征 算法，对比图片的细节特征相似度。
4. 将几种相似度算法结果取一个 hash 值，最终通过 hash 值来判定两张图相似。

`Gmeek-html<img src="https://blog-image.ian2018.cn/images/dc6cbd33532220636d3f540e20e9e794832a6408.png">`

匹配耗时

平均一张图 25 毫秒左右
- phash 阈值为 14（小于该值的图片被判断为相似）
- histogram 阈值为 0.5（大于等于该值的图片被判断为相似）
- orb 阈值为 0.173（大于等于该值的图片被判断为相似）

#### 方案二、利用苹果的 Vision 框架，通过机器学习对比图片相似度

识别算法
1. 使用 VNGenerateImageFeaturePrintRequest 创建图片特征分析请求。
2. 得到特征数据对象，调用 computeDistance 计算特征差距，从而对比相似度。

`Gmeek-html<img src="https://blog-image.ian2018.cn/images/74f86b80ae18ade251b1f9892e6378dba5932fd8.png">`

匹配耗时

平均一张图 122 毫秒左右
- 相似差异阈值为：0.55（小于该值的图片被判断为相似）

## 图片质量对比，找出最优的

> 参考文章
>
> https://yulingtianxia.com/blog/2018/11/30/Photo-Assessment/

通过对比图片的饱和度、亮度、模糊度、颜色分布来给图片打分。

## 视频压缩

> 参考文章
>
> [传统视频压缩](https://www.jianshu.com/p/35fc959cef17)
>
> [自定义视频压缩](https://mp.weixin.qq.com/s?__biz=MjM5MDI3MjA5MQ%3D%3D&mid=2697267472&idx=2&sn=8260857e7fc9201bad8decaf04b0ebe9&chksm=8376f624b4017f32c18186ac4c30ce3e0a22b2ca3d82089c857d5d7ed4c26e6e433f0ac44e5c&mpshare=1&scene=23&srcid=0904cbU12nkK96nGMZwKCHVf%23rd)
>
> [自定义视频压缩 2](https://blog.csdn.net/cai_huaer/article/details/131572817)
>
> https://github.com/arthenica/ffmpeg-kit

- 方案一：通过系统 API AVFoundation 进行压缩
- 方案二：自定义压缩
- 方案二：使用 FFmpeg 压缩
<!-- ##{"timestamp":1714461540}## -->