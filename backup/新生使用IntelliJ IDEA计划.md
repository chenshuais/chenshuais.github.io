# 1.为什么要使用 IntelliJ IDEA

Google 早在2015年中期，就宣布结束对 Eclipse ADT（安卓开发工具）的支持，并推荐开发者使用 Google 专门为开发者定制的 Android Studio ，而且提供了将项目从 Eclipse 转移到 Android Studio 的方法。但是开发者们都习惯了使用 Eclipse 开发，不想再去学习使用一个新的工具，所以 Eclipse 还残存了一段时间。在今年11月2日，Google 宣布彻底停止对 Eclipse 的支持，也就意味着，我们虽然还可以打开并使用 Eclipse ，但不能再下载最新的 SDK（和 jdk 差不多）等 Android  开发工具了，在这个技术飞速发展的时代，这是一件非常可怕的事情。所以，对于咱们安卓开发者来说，咱们就不再学习使用 Eclipse 了。

现在咱们都还在学 java 基础，而 Android Studio 是开发安卓的工具，如果以后直接使用 Android Stdio，可能大家会不好接受，所以咱们现在需要一个过渡工具，那就是 IntelliJ IDEA 。因为 Android Studio 是基于 IntelliJ 开发的，它们的使用方法也是大同小异，学会了使用 IntelliJ，那么使用Android Studio 也就没什么问题了。

# 2.介绍 IntelliJ IDEA

IntelliJ IDEA 是一个集成开发工具，可以用来写 java，咱们现在使用的还是 EditPlus 加 cmd，先写好java文件，然后再自己编译，运行。而 IntelliJ IDEA 都帮我们完成了，我们只需要写java代码，然后点击运行就可以了，这样所有的工作都可以在一个工具上完成。并且，IntelliJ IDEA 有好多快捷键和代码提示，这样大大节省了开发时间。

IntelliJ IDEA 官网的介绍：IntelliJ IDEA 主要用于支持 Java、Scala、Groovy 等语言的开发工具，同时具备支持目前主流的技术和框架，擅长于企业应用、移动应用和 Web 应用的开发。

> IntelliJ IDEA 官网：https://www.jetbrains.com/idea/


> 云盘下载地址： 链接：http://pan.baidu.com/s/1hsqLGv6
> 
> 密码：x3ga

# 3.安装教程

IntelliJ IDEA 的安装是非常简单的，几乎一直点 Next 即可。

### 欢迎界面
`Gmeek-html<img src="https://blog-image.ian2018.cn/images/83b563ad5f98e7020645fd671932221e0253f821.png">`

### 同意他的要求
`Gmeek-html<img src="https://blog-image.ian2018.cn/images/96062945ba00ae992e9c664301f77252fa6b83ae.png">`

### 选择安装路径
`Gmeek-html<img src="https://blog-image.ian2018.cn/images/d85d14948e28460a909856fe46f41e049eaf25aa.png">`

### 是否创建桌面图标和选择关联文件
`Gmeek-html<img src="https://blog-image.ian2018.cn/images/095450307bacdf91a9891d33e238da5b6956dd44.png">`

> 上图标记 1 表示在桌面上创建一个快捷图标，建议勾选上
> 
> 上图标记 2 表示关联 Java 和 Groovy 文件，意思是以后打开这类文件都用IntelliJ IDEA 打开，建议都不要勾选，
> 因为 IntelliJ IDEA占内存比较大，关联上之后再打开就这类文件就比较慢

### 直接点击安装
`Gmeek-html<img src="https://blog-image.ian2018.cn/images/9dfdfecdc7e44af9dc217ea293adc4f2d37fcb6f.png">`

### 安装过程
`Gmeek-html<img src="https://blog-image.ian2018.cn/images/1a7ce81c5ae8a2eaf40f1f942fd8205730910014.png">`

### 安装完成
`Gmeek-html<img src="https://blog-image.ian2018.cn/images/3f7532c86d975604ed8c76a137fe321d7f0a35e3.png">`
> 上图中的复选框代表现在就运行 IntelliJ IDEA

# 4.初次打开 IntelliJ IDEA

### 导入配置文件
`Gmeek-html<img src="https://blog-image.ian2018.cn/images/a8a2505cf7b055a85b8a48aec5d7862da553da32.png">`

> 上图第一个单选按钮表示 IntelliJ IDEA 识别到计算机上有 IntelliJ IDEA 旧版本的旧配置，如果选择了该选项，则 
> IntelliJ IDEA 将自动把旧版本的配置文件转移到新版本的配置文件目录上。**如果首次安装一般是没有该选项的。**
> 
> 上图第二个单选按钮表示你可以指定 IntelliJ IDEA 导入你计算机上存在其他目录的 IntelliJ IDEA 配置文件目录，如果你有的话。
> 
> 上图第三个单选按钮表示你没有任何早期版本的 IntelliJ IDEA 配置，你不导入任何配置，让 IntelliJ IDEA 生成一份新的配置。**这里我们选择这个**

### 选择主题
`Gmeek-html<img src="https://blog-image.ian2018.cn/images/39a94f09d17306b3efe2a6b269cfeb6999b16f78.png">`
> 这里就是选择一下 IntelliJ IDEA 的主题，现在流行的是第二个Darcula。选择后点击Next。

### 扩展功能
`Gmeek-html<img src="https://blog-image.ian2018.cn/images/0cc7c50f2b949053cf05f30f3366140a5ba9ffe1.png">`
> 这里是显示了IntelliJ IDEA的一些扩展功能，可以选择是否启用。
> 
> 我们这里保持默认即可，直接点击 Start using IntelliJ IDEA

### 启动界面
`Gmeek-html<img src="https://blog-image.ian2018.cn/images/0f6546bb95a2bee628b2a3f92ec62251f5fb2972.png">`

# 5.新建工程

### 点击 Create New Project
`Gmeek-html<img src="https://blog-image.ian2018.cn/images/4b61094fed3ca35071d9e735da936b1bcd719aca.png">`

`Gmeek-html<img src="https://blog-image.ian2018.cn/images/f7d4c045ab8c2e5f63c229e6ad2aec64b4238bac.png">`
> 如上图标注 1 所示，选择Java
> 
> 如上图标注 2 所示，如果此时 IntelliJ IDEA 还没有配置任何一个 SDK 的话，可以点击New...选择JDK，这时候会自动找出JDK所在位置
> 
> 如上图标注 3 所示，选择好 JDK 之后，点击Next进入下一步。

### JDK 位置
`Gmeek-html<img src="https://blog-image.ian2018.cn/images/6ebe7f5e484d4794805c72443facb5c918293dff.png">`

### 自动生成main方法
`Gmeek-html<img src="https://blog-image.ian2018.cn/images/072514172a197ea5fa12d935fd281b0e1090d381.png">`

### 工程的名字和位置
`Gmeek-html<img src="https://blog-image.ian2018.cn/images/35860c913aeecb3ce9ed1d83c9060992188788fc.png">`
> 如上图标注 1 所示，这是工程的名字，也就是文件夹的名字
> 
> 如上图标注 2 所示，这是工程所在位置，可以自己更改
> 
> 如上图标注 3 所示，这是 java 文件所在的包名，也就是 java 所在文件夹的名字，
> 
> 例：com.company 就是 com文件夹下的company文件下的 java 文件。
> 
> 如上图标注 4 所示，完成


### IntelliJ IDEA 编辑界面
`Gmeek-html<img src="https://blog-image.ian2018.cn/images/8582429589f03102eeab6b6d040cac8925698d0a.png">`
> 到这里，我们就成功创建了一个java工程，在这里我们看到了熟悉的 java 代码。
> 
> 点击上面的运行按钮，就可以运行程序了。

### 查看运行结果
`Gmeek-html<img src="https://blog-image.ian2018.cn/images/ef2b2e28a83b5a27252a4e0b607236a62a5b8091.png">`

# 6.快捷键

`Gmeek-html<img src="https://blog-image.ian2018.cn/images/1fe308d754ed5be64455b591021a82bb883ce1a0.png">`
> 这些快捷键大家没必要一下都记住，等需要的时候再去查，多用几次就记住了。

# 7.详细使用IntelliJ IDEA教程

> 极客学院：http://wiki.jikexueyuan.com/project/intellij-idea-tutorial/

> 云盘离线文档： 链接：http://pan.baidu.com/s/1gfp1KcV
>
> 密码：o1el

# 8.适应学习

现在咱们看的视频都是使用 Eclipse 教学的，但知识的内容都一样，只是开发工具不同而已，所以咱们要学会适应学习，遇到问题先尝试自己解决，当解决不了的时候再去问老成员。这样做不是烦你们问我们问题，而是希望你们能够学会解决问题的方法，毕竟我们不能一直跟着你们，有时候还是需要靠你们自己去解决的。但是当自己解决不了的时候，一定要问我们，不要不好意思。（虽然有时候我们也不会，但可以一起讨论解决，三个臭皮匠顶个诸葛亮嘛）


<!-- ##{"timestamp":1478413137}## -->