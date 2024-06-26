> 文本扫描和文本 OCR，系统 API 已经有了很好的支持，效果好，开发成本低，缺点就是文档扫描的 UI 无法定制，只能使用系统的。

# 一、文档扫描

## 系统 API 

从 iOS 13 开始，苹果增加 VisionKit 系统框架，并且在 Vision 框架上增加了对 OCR 的支持，应用可以直接使用系统 API 开完成文档扫描和 OCR。

相关文章：
- https://developer.apple.com/documentation/visionkit/
- https://mistergrizzly.medium.com/scanning-documents-with-vision-and-visionkit-on-ios-13-913c0a6f9392

由 **VisionKit** 支持，使用简单，识别效果很好，但调起的是系统界面，所以没有办法定制 UI。


<video src="https://blog-image.ian2018.cn/images/a6f029cbc29e9ff85a322c4b2e4e7ad36d08d650.MP4" controls="controls" width="320" height="680"></video>


## 其他开源项目

### WeScan（基于系统的 Vision 框架实现，可以自定义 UI）
相关文章：
- https://github.com/WeTransfer/WeScan

效果没有 VisionKit 好，唯一优点就是能定制 UI。

<video src="https://blog-image.ian2018.cn/images/d52b3e6bd8e0778d34904517f7dc291cd8a9eaa1.mp4" controls="controls" width="320" height="680"></video>

# 二、文本 OCR 

## 系统 API
相关文章：
- https://developer.apple.com/documentation/vision/recognizing_text_in_images/

由 Vision 支持，使用简单，识别速度快，缺点是需要指定识别语言列表。

<video src="https://blog-image.ian2018.cn/images/a6deafb9be9cba0408ecf07de118b4bf7bdd35b7.mp4" controls="controls" width="320" height="680"></video>

## 其他开源项目
### Tesseract OCR（开源 SDK）
相关文章：
- https://github.com/tesseract-ocr/tesseract
- https://github.com/gali8/Tesseract-OCR-iOS
- https://github.com/SwiftyTesseract/SwiftyTesseract
- https://medium.com/@onmyway133/vision-in-ios-text-detection-and-tesseract-recognition-26bbcd735d8f

纯英文的效果一般，混合语言的就不行了，识别速度很慢，准确率也很低。

<video src="https://blog-image.ian2018.cn/images/c96a2e662c04026ca507b923dd6b86d4c8d44dcd.mp4" controls="controls" width="320" height="680"></video>

### Paddle 飞桨（百度开源 SDK）
相关文章：
- https://github.com/PaddlePaddle/Paddle-Lite
- https://www.paddlepaddle.org.cn/lite/develop/demo_guides/ios_app_demo.html
- https://www.paddlepaddle.org.cn/lite/develop/quick_start/release_lib.html
- https://github.com/Leonard-iOS/PaddleOCR

iOS 上暂未跑通，代码比较古老。

# 三、导出 PDF

## 系统 API
相关文章：
- https://developer.apple.com/documentation/pdfkit/pdfdocument
- https://stackoverflow.com/questions/73987290/make-a-pdf-document-by-image-using-pdfkit-in-ios-16

`Gmeek-html<img src="https://blog-image.ian2018.cn/images/6eaaa51c203be0a72a30130c8d8be3129479905e.png">`

## 其他开源项目
### PDFGenerator
相关文章：
- https://github.com/sgr-ksmt/PDFGenerator

`Gmeek-html<img src="https://blog-image.ian2018.cn/images/81bd0d5d5ca4454b555a29c836a714240e7aa621.png">`

### TPPDF
相关文章：
- https://github.com/techprimate/TPPDF

`Gmeek-html<img src="https://blog-image.ian2018.cn/images/f2a88477bb6d91c2e2671514f869efca63680fba.png">`


<!-- ##{"timestamp":1711963080}## -->