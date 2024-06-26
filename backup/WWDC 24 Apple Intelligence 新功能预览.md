# Apple Intelligence

> AI 相关的能力仅支持 iPhone 15 Pro 及以上的机型
>
> https://developer.apple.com/apple-intelligence/

`Gmeek-html<img src="https://blog-image.ian2018.cn/images/03cac3f41a5d033b8a3bed66c5404607b0faa37c.png">`

## Writing Tools
> 参考文档：
>
> https://developer.apple.com/videos/play/wwdc2024/10168/
>
> https://www.youtube.com/watch?v=79PkqsA_zk8

`Gmeek-html<img src="https://blog-image.ian2018.cn/images/d4717848a1428549fc383812053238cdca0b2a54.png">`

### 表现形式：
- 系统键盘上的工具栏
- 长按文本弹出的上下文菜单中

`Gmeek-html<img src="https://blog-image.ian2018.cn/images/6a8d39e5b10a2e0c4aa7585fd183f5c17c7fec5a.png">`

### 适用于文本类型：
- 可编辑的文本：会直接在原文本上修改
- 不可编辑的文本：会弹出系统弹窗面板展示结果

### 支持的能力：
- 文本校对
- 文本重写
- 摘要总结
- 将文本转换为列表或者表格

### 开发者能做的：
- 默认应用内使用 `UITextView` 或者 `NSTextView` 的文本 UI 都支持 Writing Tools 功能。
- 设置应用内的文本是否支持 Writing Tools 功能（完整功能、受限的功能、不支持）。
- 设置应用内支持的文本类型（纯文本、富文本、表格）

## Image Playground

> 目前官方还没有放出相关文档

`Gmeek-html<img src="https://blog-image.ian2018.cn/images/d699a82925cf24c3b3b18c720947ca22270f5f7c.png">`

图像游乐场提供了一种易于使用的体验，让用户可以在 消息、备忘录、Keynote、Pages 等应用中创建有趣、充满玩味的图片。

通过使用图像游乐场 API，可以将这种体验添加到应用中，使用户能够快速利用应用内的上下文创建令人愉悦的图片。而且，由于图片完全在设备上生成，无需开发或托管自己的模型，用户即可在应用中享受创建新图片的乐趣。

## Genmoji

> 参考文档：
>
> https://developer.apple.com/videos/play/wwdc2024/10220/
>
> https://www.youtube.com/watch?v=D58kVm2h4-w

`Gmeek-html<img src="https://blog-image.ian2018.cn/images/6d001238492d302c9fffc31f069cbbd3b84e4b2c.png">`

Genmoji 开辟了全新的交流方式，让用户可以创建与任何时刻相匹配的新表情符号。虽然表情符号是以文本形式表示的，Genmoji 则是以内嵌图片的形式表示。如果您使用标准的 UI 框架来渲染文本字段，您可以轻松支持 Genmoji。在其他情况下，Genmoji 可以通过 AttributedString 来支持，这是一种用于表示带有图形的富文本的数据类型。

### 开发者能做的：
- 只能是做适配，以支持在应用内显示这种用户自定义的 emoji 表情。

## Siri with App Intents

> 参考文档：
>
> https://developer.apple.com/videos/play/wwdc2024/10133/
>
> https://www.youtube.com/watch?v=Lb89T7ybCBE
>
> https://developer.apple.com/documentation/AppIntents/making-your-app-s-functionality-available-to-siri

`Gmeek-html<img src="https://blog-image.ian2018.cn/images/b611b9651fd8b33dc6a1c6ebc3039351ebe5d605.png">`

### 表现形式：
- 通过新的 Siri AI 模型处理，获取到用户的意图，可以直接操作应用提供的功能。
- 我理解类似于 ChatGPT 的 function calling 功能。
`Gmeek-html<img src="https://blog-image.ian2018.cn/images/ec5014b0cee8e21b89a4dacd011c16dec67923fc.png">`
- 苹果一共设计了 12 中特定的意图（目前只开放了 Photos 和 Mail）
`Gmeek-html<img src="https://blog-image.ian2018.cn/images/71dc5dae6e1d38f9587c6aae71e3ade15f1184d4.png">`

### 开发者能做的：
- 根据新的框架，让应用支持 Siri 的调用，以实现上面 12 个意图功能。
- 例如：Photos 的创建相册、打开图片、搜索图片等。
<!-- ##{"timestamp":1718175491}## -->