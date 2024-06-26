测试 markdown：
```
# 回答内容如下：

Python中可以使用replace()函数来替换字符串。它接受两个参数，第一个参数是要被替换的子字符串，第二个参数是替换后的新字符串。以下是一个示例：

    ```python
    string = "Hello World"
    new_string = string.replace("World", "Python")
    print(new_string)
    ```

输出结果为：`Hello Python`

在这个例子中，我们将字符串"Hello World"中的"World"替换为"Python"。

* replace()函数返回一个新的字符串，原始字符串不会被改变。
    
```

## Android 

### 方案一、现成的支持 markdown 格式的自定义 view 的库

#### 1. [MarkdownView-Android](https://github.com/mukeshsolanki/MarkdownView-Android)

- 支持自定义样式，但是是通过修改 css 的形式，修改成本比较高
- 使用 webview 实现
- 不支持列表刷新，高度不好测量，不支持列表展示
`Gmeek-html<img src="https://blog-image.ian2018.cn/images/b852f752028bf28cf6df2c43f7ea43aab3affe41.png">`

#### 2. [Markwon](https://github.com/noties/Markwon)

- 支持自定义样式
- 不是 webview 实现，将 markdown 转化为 Android 中的 Spannable，可以创建丰富的文本样式，从而实现更灵活和定制化的文本显示效果，例如在富文本编辑器、聊天应用中显示不同用户的消息样式等
- 支持较多属性
- 支持列表刷新
- 限制：
  - 长按时对于 markdown 语法每一个标签下的文字只能分块选择
`Gmeek-html<img src="https://blog-image.ian2018.cn/images/d97d00d83df0be017dcc8a399f374e26b7ce6caa.png">`

### 方案二、解析 markdown 的库，自己去实现怎么展示

#### 1. [commonmark-java](https://github.com/commonmark/commonmark-java)

commonmark-java 是一个用于解析和渲染 Markdown 文本的Java库。它实现了 CommonMark 规范，可以将Markdown 文本转换为 HTML 或 AST（抽象语法树）等其他格式。它提供了各种功能，包括解析 Markdown 文本、自定义渲染、语法扩展等, 上述的 Markwon 就是基于此拓展了渲染功能

## iOS

### 方案一、现成的支持 markdown 格式的自定义 view 的库

#### 1. [MarkdownView](https://github.com/keitaoouchi/MarkdownView)

- 支持自定义样式，但是是通过修改 css 的形式，修改成本比较高
- 使用 webview 实现
- Webview 高度不好测量，列表刷新不太好实现
- 由于内部使用 webview，所以支持大部分的 markdown 语法
`Gmeek-html<img src="https://blog-image.ian2018.cn/images/b5b0712a8148ecfb29dd01cdb9cacfd038464c32.png">`

#### 2. [SwiftyMarkdown](https://github.com/SimonFairbairn/SwiftyMarkdown)
- 支持自定义样式
- 不是 webview 实现，原理就是将 markdown 格式转换成 attributedString，这样就可以直接设置给 Label，兼容性好
- 支持列表刷新，改造成本比较低
- 但是在刷新的时候文字的样式可能会跳动
- 由于 attributedString 本身样式就有限，所以支持的 markdown 语法较少，如下：
```
*italics* or _italics_
**bold** or __bold__
~~Linethrough~~Strikethroughs. 
`code`

# Header 1

or

Header 1
====

## Header 2

or

Header 2
---

### Header 3
#### Header 4
##### Header 5 #####
###### Header 6 ######

        Indented code blocks (spaces or tabs)

[Links](http://voyagetravelapps.com/)
![Images](<Name of asset in bundle>)

[Referenced Links][1]
![Referenced Images][2]

[1]: http://voyagetravelapps.com/
[2]: <Name of asset in bundle>

> Blockquotes

- Bulleted
- Lists
        - Including indented lists
                - Up to three levels
- Neat!

1. Ordered
1. Lists
        1. Including indented lists
                - Up to three levels
```
`Gmeek-html<img src="https://blog-image.ian2018.cn/images/0f378ae3cfd93c1dc5a3cfd9ddda07879db3b30c.png">`

<video width="320" height="680"   controls poster="https://blog-image.ian2018.cn/images/ae89321dd645e6809634ba8fe961731da0601014.png">
    <source src="https://blog-image.ian2018.cn/images/23179ed0b683c78e88569d347664426981319712.mov" type="video/mp4">
</video>

### 方案二、解析 markdown 的库，自己去实现怎么展示

#### 1. [swift-markdown](https://github.com/apple/swift-markdown)

完全解析成 markdown 的各个内容，可以自己去实现每个内容该怎么展示。

代码块的话，可以使用 https://github.com/JohnSundell/Splash 这个库，该库可以将代码进行高亮，转换成 attributedString ，大概效果如下：
`Gmeek-html<img src="https://blog-image.ian2018.cn/images/14f0ae43b7e0b3af2f41f40c80be9fe183598508.png">`

#### 2. [Ink](https://github.com/JohnSundell/Ink)

该库是将 markdown 内容转换成 html，展示的话还需要自己用 webview
<!-- ##{"timestamp":1688006431}## -->