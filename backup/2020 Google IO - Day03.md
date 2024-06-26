2020 Google 开发者大会 - 线上

# Day03 - Flutter | Web | Material Design


## 1. Flutter性能进展及着色器编译预热

flutter在过去一年中的性能优化进展：

<img src="https://blog-image.ian2018.cn/images/5e89bdafd2fc9159e3153133a9f3235e085e4454.png" style="zoom:50%;" />

### 1.1 性能测试

<img src="https://blog-image.ian2018.cn/images/f5f4f65a17ff5b88cd6973c4d3ebcaa28a7f6541.png" style="zoom:50%;" />

#### 1.1.1 能耗测试

* 能耗 ≈ 速度，但是能耗 ≠ 速度
* 速度快可以导致能耗高
* 速度慢也有可能不耗能，如cpu上的任务被别的IO或GPU任务阻塞，进行长时间的等待，但这个时候并没有多大的能耗

目前只加入了iOS的能耗测试工具，Android的会在以后增加。



#### 1.1.2 在Timeline中查看CPU/GPU使用率

在用 `flutter run --profile --track-skia` 命令启动App后，打开 Observatory 的监听URL，进入 timeline

<img src="https://blog-image.ian2018.cn/images/783ee8c4c9b9549c53e18832147068793bdba5a3.png" style="zoom:50%;" />

现在App里触发一段动画，之后在timeline中点击Refresh加载该动画的timeline，选取一段时间线，就可以看到此时的CPU、内存和GPU的使用率。

<img src="https://blog-image.ian2018.cn/images/44b23241ce11c6bf218e8daa850cacaaa7f4c002.png" style="zoom:50%;" />

除了手动查看timeline，还可以利用FlutterDriver集成测试来自动获取CPU、GPU的使用率的统计信息。在集成测试之前调用 `enableFlutterDriverExtension()` 来支持Driver集成测试。

```dart
void main() {
  test('perf test', () async {
    // 先建立电脑和手机的连接
    FlutterDriver driver = await FlutterDriver.connect();
    // 等待3s，为了更好的看到测试初始的状态
    await Future<void>.delayed(const Duration(seconds: 3));
    // 收集timeline进行性能测试
    final Timeline timeline = await driver.traceAction(() async {
      // 点击 Button 播放动画
      await driver.tap(find.byType('Button'));
      // 等待3s，获得这3s的性能数据
      await Future<void>.delayed(const Duration(seconds: 3));
    });
    // 将timeline的性能数据 输出到 前缀为perf的json文件中
    final TimelineSummary summary = TimelineSummary.summarize(timeline);
    summary.writeSummartToFile('perf', pretty: true);
    driver.close();
  });
}
```

通过运行 `flutter drive --profile -t xxx/xxx/dart` 运行集成测试。



### 1.2 SkSL 着色器编译预热

使用SkSL预热，可以将最慢帧的渲染速度提升2-5倍，消除着色器编译卡顿。



#### 1.2.1 如何确定着色器编译卡顿

着色器编译卡顿是：一个Flutter App第一次渲染某类动画时有明显的卡顿，之后再渲染同样的动画就没有卡顿了，这很有可能就是着色器编译卡顿。

要进一步确定是否为编译器着色卡顿，可以使用命令 `flutter run --profile --trace-skia` 运行App后，进入Observatory 的timeline，查看raster thread，查看一帧的Draw函数耗时，如果有长时间的 GrGLProgramBuilder::finalize，就意味着有着色器编译卡顿。

<img src="https://blog-image.ian2018.cn/images/7a101897685eb7092eb1c709358db8f11316a71a.png" style="zoom:50%;" />



#### 1.2.2 使用SkSL预热降低卡顿

手动方式：

* 在运行App时，加入 cache-sksl 的参数

  ```
  flutter run --profile --cache-sksl
  ```

* 然后操作App，产生有卡顿的动画，然后回调命令界面，按？键可以得到所有命令。其中 M 可以将捕捉到的SkSL着色器进行导出。按下M后得到一个json文件，然后关闭程序。

  <img src="https://blog-image.ian2018.cn/images/5d874bdd76b5119f920f6309b17032aff99f54ed.png" style="zoom:50%;" />

* 然后使用 bundle-sksl-path 参数和刚刚得到的json文件路径，build一个包含SkSL着色器预热的App

  ```
  flutter build apk --bundle-sksl-path xxx/xxx/flutter_01.sksl.json --profile
  ```

* 最后通过 use-application-binary 运行这个新的App，再次查看timeline，可以看到卡顿已经消失了

  ```
  flutter run --profile --use-application-binary xxx/xxx/app-profile.apk --track-skia
  ```



自动化方式：

通过集成测试自动化生成SkSL着色器文件。

* 编写好测试文件

* 执行以下命令，可以将运行当中遇到的所有着色器输出到json文件中

  ```
  flutter drive --profile -t xxx/xxx.dart --cache-sksl --write0sksl-on-exit flutter_01.sksl.json
  ```

之后的步骤就和手动的一样了，使用`bundle-sksl-path`命令build一个SkSL预热的apk。



#### 1.2.3 更多内容

https://flutter.dev/docs/perf/rendering/shader



## 2. 新的 Flutter 开发工具功能：内存和包体积调试工具

默认情况下，只要安装了Flutter环境，就已经安装了DevTools工具。

Dart开发工具新功能：

* 内存调试
* 内存测试
* 包体积调试

Dart开发工具的功能：

* 布局检查（Inspector）
* 性能调试（Performance）
* 内存调试（Memory）
* 网络调试（Network）
* 包体积调试（App Size）
* 调试器（Debugger）
* 日志（Logging）

### 2.1 内存调试

内存调试器功能：

* 时间窗格（Dart和Android内存）
* 手动和自动快照（snapshot）和垃圾回收（GC）
* 内存分析
* 内存堆分配累加器（Heap Allocation Accumulators）
* 通过命令行界面将内存统计信息导出到json文件

### 2.2 内存测试

内存测试的方法：

* 通过ADB交互直接进行内存测试
* Dart开发工具内存测试
* iOS内存测试

[更多详细信息](https://github.com/flutter/flutter/wiki/How-to-write-a-memory-test-for-Flutter)

### 2.3 包体积调试

包体积调试功能：

* 可视化应用程序的总大小，包括功能级别的Dart AOT快照
* 分析快照和应用包（APK，IPA等）
* 分析快照或应用程序包（APK，IPA等）的差异
* 查看软件包级别的应用大小归因数据



## 3. Pigeon 与 Flutter 混合工程

 Pigeon工具库可以帮助简化Flutter在混合开发中，原生与Flutter代码之间的函数调用与数据共享，减少模版代码，使编写插件更简单。

使用 MethodChannels 和 Pigeon 对比：

<img src="https://blog-image.ian2018.cn/images/943c46329ac031f58fead5f9bbceca1217cff2b7.png" style="zoom:50%;" />

可以看到 MethodChannels的方法高度依赖与字符串检查和类型转换，但在Pigeon中就可以像编写原生方法那样去实现。



### 3.1 Pigeon的运行流程

<img src="https://blog-image.ian2018.cn/images/b9142f6823969ebdf1262667da812debfbe95a2b.png" style="zoom:50%;" />

首先声明好接口定义，然后运行Pigeon工具，它可以为不同平台生成代码，这些代码都会使用MethodChannels的方法，省去了平台通道的样板代码。

### 3.2 Pigeon 使用方法

* 在 `pubspec.yaml` 中的 `dev_dependencies` 下添加 Pigeon 插件依赖，然后运行 `flutter pub get` 安装。

  ```
  dev_dependencies:
  	flutter_test:
  		sdk: flutter
  	pigeon: ^0.1.3
  ```

* 一般会在项目根目录下新建一个pigeons的文件夹，用来放Pigeon的接口定义文件

* 在pigeons文件下创建一个dart文件，然后声明接口定义

  ```dart
  import 'package:pigeon/pigeon.dart';
  
  // 需要的数据类
  class Version {
    String string;
  }
  
  // 声明Flutter调用原生的方法，（如果原生要调Flutter的方法，可以用@FlutterApi的注解）
  @HostApi()
  abstract class ExampleApi {
    Version getPlatformVersion();
  }
  ```

* 使用Pigeon生成不同平台的代码

  ```
  flutter pub run pigeon \
    --input pigeons/xxx.dart \ #指定输入为刚刚编写的pigeon文件，以下都是设置不同的输出选项
    --dart_out lib/xxxxx.dart \ #dart
    --objc_header_out ios/Runner/xxxxx.h \ #ios
    --objc_source_out ios/Runner/xxxxx.m \ #ios
    --java_out ./android/app/src/main/java/dev/flutter/pigeon/xxxxx.java \ #android
    --java_package "dev.flutter.pigeon" #指定android包名
  ```

* 开始使用

  * 在原生代码中实现刚声明的接口

    ```kotlin
    import dev.flutter.pigeon.xxxxx
    
    class PigeonPlugin: FlutterPlugin, xxxxx.ExampleApi {
      private lateinit var channel : MethodChannel
    
      override fun onAttachedToEngine(@NonNull flutterPluginBinding: FlutterPlugin.FlutterPluginBinding) {
        xxxxx.ExampleApi.setup(flutterPluginBinding.binaryMessenger, this)
      }
    
      override fun onDetachedFromEngine(@NonNull binding: FlutterPlugin.FlutterPluginBinding) {
        xxxxx.ExampleApi.setup(binding.binaryMessenger, null)
      }
    
      // 实现接口的方法
      override fun getPlatformVersion(): xxxxx.Version {
        var result = xxxxx.Version()
        result.string = "Android ${android.os.Build.VERSION.RELEASE}"
        return result
      }
    }
    ```

  * 在dart中调用原生方法

    ```dart
    import 'dart:async';
    
    import 'package:flutter/services.dart';
    import 'xxxxx.dart';
    
    class PigeonPlugin {
      static ExampleApi _apiInstance;
    
      static ExampleApi get _api {
        if (_apiInstance == null) {
          _apiInstance = ExampleApi();
        }
        return _apiInstance;
      }
    
      static Future<String> get platformVersion async {
        // 调用原生的方法
        Version version = await _api.getPlatformVersion();
        return version.string;
      }
    }
    ```

### 3.3 更多信息

* [视频示例代码](https://github.com/gaaclarke/GiantsA2A)

* [Pigeon文档](https://pub.flutter-io.cn/packages/pigeon)
* [Flutter混合编程文档](https://flutter.dev/docs/development/add-to-app)



## 4. 阿里巴巴如何在复杂业务场景中使用 Flutter

### 4.1 阿里中使用了Flutter的App：

<img src="https://blog-image.ian2018.cn/images/5dc633e618db6a028304797f74da46b47b5a7ae9.png" style="zoom:50%;" />



### 4.2 Flutter的技术优势

<img src="https://blog-image.ian2018.cn/images/03c9ad2f956ff5927547f59776ea92951b02349e.png" style="zoom:50%;" />



### 4.3 Flutter的适用场景


`Gmeek-html<img src="https://blog-image.ian2018.cn/images/90ba012494e4935d8545880a21e741e5353dd2af.png">`


### 4.4 融入客户端开发工作流

<img src="https://blog-image.ian2018.cn/images/800b167ea7e9cb8522caf12453b2b49bdf413cbd.png" style="zoom:50%;" />



### 4.5 接入现存技术积累

<img src="https://blog-image.ian2018.cn/images/0906e418d2ebcee0b869b4f464f4c2188042e4c3.png" style="zoom:50%;" />

其中导航栈管理和状态管理已经开源



### 4.6 实际案例：[解决多图长列表页面的内存问题](https://mp.weixin.qq.com/s/Mcfj3lRR8VJACxsjjIiVsA)

问题：

* 页面很长、图片很多，首屏时间太长
* 大量图片同时加载并生成纹理，内存飙升
* Sliver中每项Cell拆分粒度太大，单个Cell占用多屏，难以实现回收

解决方案：

* 拆分Cell，使每一项变得更小
* 根据坐标判断图片是否在屏幕内，添加通知图片懒加载和回收
* 提前获取图片宽高大小，减少布局和重绘
* 以图片为单位进行纹理回收，而不是以Sliver中的每项Cell为单位
* 通过外接原生图片库，实现共用本地缓存



### 4.7 Flutter[体系化建设](https://mp.weixin.qq.com/s/Xh18fq7CYUoPEnnL5eL5Fg)

思路：

<img src="https://blog-image.ian2018.cn/images/4cb2128c0688dda895f397548f4e5ae8fcd3626d.png" style="zoom:50%;" />

具体技术规划：

<img src="https://blog-image.ian2018.cn/images/1a78678fd8560554c5663b9642f5ee235ca5a5cc.png" style="zoom:50%;" />

各项任务之间的联系：

<img src="https://blog-image.ian2018.cn/images/a1708f3d0461ab645e43b5ef06a3a7000d76f018.png" style="zoom:50%;" />



### 4.8 社区贡献

开源项目：

* [flutter-boost](https://github.com/alibaba-flutter/flutter-boot) 核心解决了混合开发模式下的两个问题：flutter混合开发的工程化设计和混合栈
* [fish-redux](https://github.com/alibaba/fish-redux) 一个基于 Redux 数据管理的组装式 flutter 应用框架， 它特别适用于构建中大型的复杂应用
* [flutter-go](https://github.com/alibaba/flutter-go) 一个提供了各种常用的widget组件的库，目前暂停维护

书籍：

* [Flutter in action](https://developer.aliyun.com/article/720790)
* [Flutter 技术解析与实战](https://developer.aliyun.com/article/754410)
* [AliFlutter 体系化建设和实践](https://developer.aliyun.com/article/765471)



## 5. 在 Flutter 和 Android 中使用 Material Motion

[Motion](https://material.io/design/motion/understanding-motion.html#principles) 有助于使界面更具表现力、更易于使用。

设计原则：

`Gmeek-html<img src="https://blog-image.ian2018.cn/images/c7e34bd4c0274daf2cab591ebe48eb5fd9747c93.gif">`

### 5.1 Transition

Transition是一种可将同一界面中的不同状态连接的动画。让用户可以感知界面元素之间的关系。

[Material Motion指南](https://material.io/design/motion/the-motion-system.html)中介绍了四种模式，涵盖了最常用的Transition类型，使用Motion库可以轻松实现这些效果。

目前Motion库只支持Android和Flutter：

| 平台    | 状态                                                         |
| ------- | ------------------------------------------------------------ |
| Android | [**Available**](https://material.io/develop/android/theming/motion/) |
| iOS     | Not planned                                                  |
| Web     | Not planned                                                  |
| Flutter | [**Available**](https://pub.dev/packages/animations)         |



#### 5.1.1 Container Transform

可以在两个元素之间实现无缝连接。

`Gmeek-html<img src="https://blog-image.ian2018.cn/images/b8e22fd2fea932c8e8077a03e6da884cca9a1d29.gif">`

```kotlin
val transform = MaterialContainerTransform().apply {
  // 设置开始和结束View
  startView = fab
  endView = bottomToolbar

  // 确保容器转换仅在单个目标上运行
  addTarget(endView)

  // 添加运行路径
  pathMotion = MaterialArcMotion()

  // 移除蒙层颜色
  scrimColor = Color.TRANSPARENT
}

// 开始延迟执行，并设置开始和结束view的可见性
TransitionManager.beginDelayedTransition(container, transform)
fab.visibility = View.GONE
bottomToolbar.visibility = View.VISIBLE
```



#### 5.1.2 Shared axis

会使用淡化效果，并在转换时共用X轴、Y轴或Z轴。

`Gmeek-html<img src="https://blog-image.ian2018.cn/images/7fa1e073dfce2b029b15f54cf662dd55e81ebb92.gif">`

````kotlin
// 创建Motion实例，并设置要使用的轴
val sharedAxis = MaterialSharedAxis(MaterialSharedAxis.Y, true)

// 使用TransitionManager延迟开始
TransitionManager.beginDelayedTransition(container, sharedAxis)

// 设置开始和结束view的可见性
outgoingView.visibility = View.GONE
incomingView.visibility = View.VISIBLE
````



#### 5.1.3 Fade Through（淡出后淡入）

适用于元素之间的关系不太紧密的情况。

`Gmeek-html<img src="https://blog-image.ian2018.cn/images/0097b8664adda31eaecf2b1c3a15e0a689ce9d36.gif">`

```kotlin
// 创建淡出后淡入Motion实例
val fadeThrough = MaterialFadeThrough()

// 使用TransitionManager延迟开始
TransitionManager.beginDelayedTransition(container, fadeThrough)

// 切换两个试图的可见性
outgoingView.visibility = View.GONE
incomingView.visibility = View.VISIBLE
```



#### 5.1.4 Fade（淡化）

适用于进入或退出画面的界面元素，如对话框、菜单等。

`Gmeek-html<img src="https://blog-image.ian2018.cn/images/f9481ec474c61f8cbce1de805e34e8138efdd2bf.gif">`

```kotlin
// 创建Fade Motion实例
val materialFade = MaterialFade().apply {
  duration = 150L
}
// 使用TransitionManager延迟开始
TransitionManager.beginDelayedTransition(container, materialFade)
// 设置view的可见性
fab.visibility = View.VISIBLE
```



### 5.2 更多信息

[https://material.io/design/motion](https://material.io/design/motion)


<!-- ##{"timestamp":1606055477}## -->