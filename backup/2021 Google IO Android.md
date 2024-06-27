## Android 12 [新功能预览](https://android-developers.googleblog.com/2021/03/android-12-developer-preview-2.html)

### 界面上巨大的更新
- 新的设计语言 Material NEXT
- 扩展了调色板，以适配 Android 12 的新设计。能从壁纸中提取颜色的仅限于 Pixel 手机。
`Gmeek-html<img src="https://blog-image.ian2018.cn/images/b022b2583e5bd84c33a8eb94ce2da4127d172fa9.png">`

### Widget 
- 在 Android 12 上移除了过去添加 Widget 的方式
- 提供了全新的 API 来构建 Widget
- 增加了 Widget 的使用场景，如 Google 助理上互动

### Launch animations 
- https://developer.android.com/about/versions/12/features/splash-screen
- 应用冷启动动画，在 Android 12 中默认就有，展示应用 icon 放大的动画。
- 也可以自定义动画，通过 `Activity.getSplashScreen()` 来修改动画属性

### Notifications 
- 又重新设计了通知模版 ，颜色、背景、字体等
- 如果是用的系统默认的样式，则不需要进行修改，但是如果用的是自定义的样式，则需要进行很多调整，因为在新版本中已经完全弃用自定义样式了，提供了在固定模版内自定义的方式。
- 在 Android 12 上，通知必须有启动 Activity 的 Intent，否则通知则无法正常工作。

### Toast 
- 对 Toast 增加了所属应用的 icon
- 减少了 Toast 内容文字的长度
- 限制了弹出速率

### Picture in Picture 
- 调整了画中画的切换动画，并提供了相应的 API 使用交叉淡入淡出的效果
- 在调整画中画大小时，会无缝适配视频内容

### 新增模糊处理 API
- `imageView.setRenderEffect(RenderEffect.createBlurEffect(20f, 20f, SHADER_TILE_MODE))`
- 不只是 View ，也可以设置屏幕背景的模糊度，通过 style 或者 WindowManager API 设置。
`Gmeek-html<img src="https://blog-image.ian2018.cn/images/678be2d2533b967e2bf2c46f468bcbe902fc7115.png">`
- 其实在 Android 第一个版本就有模糊的功能，只不过当时只有一款手机的硬件支持，所以采用了将背景调暗的效果。现在随着时代的进步，手机的性能大幅提高，现在已经到了使用模糊的成熟时机。

### 更新了按钮的水波纹点击效果
- 增加了闪光效果，会在 Android 12 上默认开启
`Gmeek-html<img src="https://blog-image.ian2018.cn/images/3aae19121c6f16ea8143a9a685b07260e49d0542.png">`

### 更新滑动的边缘效果 https://developer.android.com/about/versions/12/overscroll
- 将之前的发光效果改成了类似于 iOS 的拉伸弹性效果，会在 Android 12 上默认开启。
`Gmeek-html<img src="https://blog-image.ian2018.cn/images/2ca2125d0755cd478ec8d4e95a8f39871a25cbcd.png">`
- 提供了 API 可以控制这种效果

### 图像 
- 新增了对 AVIF 格式的图片支持，AVIF 相比于 JPEG 图像质量更好，磁盘占用更小。

### 视频 
- 为不支持的视频编码格式，增加了自动转码功能，可以在 Manifest 中注册，设置支持哪些格式，不支持哪些格式。也可以使用 Media API 来处理。
`Gmeek-html<img src="https://blog-image.ian2018.cn/images/65694939e574c77f62ec850c13743bec3ead6cdd.png">`

### 音频
- 增加了根据音频产生相匹配的震动效果功能，但是并不是所有设备都支持，需要判断 `isAvailable()`
`Gmeek-html<img src="https://blog-image.ian2018.cn/images/69b27024225c59d0855438b38253dc1da744bee3.png">`
- 可以应用到游戏中，提取音频中的脚步声雨声等音轨，然后同步震动效果

### 隐私权限
- 相机和录音权限
  - 当应用在使用这两个权限时，系统会状态栏上提示用户有应用在访问相机或者麦克风，并且可以在下拉快捷面板上查看是哪个应用，并可以直接在这里取消授权
- 新的蓝牙权限
  - 有了这个权限，就可以扫描蓝牙设备，而不是像之前，需要申请位置权限才可以使用。
- 位置权限
  - 区分了精确位置权限和大概位置权限
- 剪切板
  - 只有键盘和前台应用可以访问剪切板数据，并且前台应用在访问剪切板时，系统会弹出 Toast 告诉用户应用在读取剪切板数据。
`Gmeek-html<img src="https://blog-image.ian2018.cn/images/fa58d4d242d41773c9c8d07507872f14f6742d78.png">`
- 前台通知限制
  - 如果通知的时间出现不到 10 s，则通知不会出现
  - 不再允许从后台启动前台服务，推荐做法是使用 WorkManager
`Gmeek-html<img src="https://blog-image.ian2018.cn/images/466312132c0b4bfe852d9dd09b4487682790e2f5.png">`
- 经常不使用的应用对权限回收
  - 在 Android 12 上，不仅会回收权限，还会清除缓存数据，休眠应用

### 拖拽、复制粘贴、键盘 Stickers 三个 API 合并
- Android 12 之前的样子
`Gmeek-html<img src="https://blog-image.ian2018.cn/images/c67b79543b61b8da35b63e6864cf3207145b216f.png">`
- 合并之后
`Gmeek-html<img src="https://blog-image.ian2018.cn/images/628efd5d0a3edfa546281fd20891acee51dd9cb6.png">`
  - 不过目前还没有全面上线该功能，目前只为 `AppCompatEditText` 启用了，并且只能通过依赖 Platform 来支持，AndroidX 还没有上线

### 性能测试的库 
- [Macrobenchmark](https://developer.android.com/studio/profile/macrobenchmark)，可以测试启动、列表滚动等的性能

### Wear 手表系统 
- 发布了新的手表系统

### Android Tools 
- new development tools 
  - 数据库 Database Inspector
  - Work Manager Inspector （可以查看任务的执行状态）
- new design tools 
  - Compose 预览功能
- gradle plugin

### Jetpack 
- 推出稳定版 
  - CameraX
  - Paging 3
  - Hilt
- α & β 新版本 
  - DataStore
  - WorkManager 2.7
  - Navigation
  - Marobenchmark

### Jetpack Compose 
- 声明式 UI 库


## Jetpack [Compose](https://developer.android.com/jetpack/compose)

即将发布 1.0 稳定版本

### 什么是 Compose
- Compose 是一个完全基于 Kotlin 编写的声明式 UI 工具库，可以简单快捷的构建 UI
- 如果要使用 Compose ，必须要用 Kotlin 编写

### 声明式 UI 和命令式 UI 的区别
- 命令式，状态的更新，需要通过代码去控制刷新界面，从而保持状态和 UI 的同步
`Gmeek-html<img src="https://blog-image.ian2018.cn/images/25abe4bfca27f420e230156b04941246d44fbaa3.png">`
- 声明式，UI 可以根据状态自动更新，开发者只需要管理好 UI 的状态
  - Compose 当状态变化时，会去创建新的 UI 对象，不过他会跳过没有改变的元素，使其效率更高
`Gmeek-html<img src="https://blog-image.ian2018.cn/images/49438857005ef09a1eaf359fd9482bd511fd6cb0.png">`

### Compose 代码长什么样
```kotlin
@Composable
fun MassageList(messages: List<String>) {
  Column {
    messages.forEach { message ->
    	Text(text = message)
    }
  }
}
```
- 上面就是创建一个字符串列表界面
- 带有 @Composable 注解的函数，为了区分其他普通函数，函数名字推荐首字母大写。
- 该函数没有返回值，不过这样也会直接创建 UI

### 与 LiveData 结合

`Gmeek-html<img src="https://blog-image.ian2018.cn/images/8bd65640e9662dd07b54d8bbc17b1d59ccc3f703.png">`
- 不需要自己手动设置 Observe，Compose 编译器会自动读取状态，并在状态变化时更新 recomposable

### 在项目中使用

- 支持逐步替换
`Gmeek-html<img src="https://blog-image.ian2018.cn/images/b72b9076445a0bd15cb9dd1819887e9dc4abf215.png">`
- View 里使用 Compose
- Compose 里使用 View

### Android Studio 工具

- 支持对 Compose 预览
`Gmeek-html<img src="https://blog-image.ian2018.cn/images/aae5cf7d5d44e542f48d394d73a16b78a9e3deea.png">`

### 其他
- Compose 不会多次测量，从而使嵌套布局更加高效
- 自定义布局更加简单，只需要写一个 layout 函数就可以测量和摆放
- 动画更加简单 `animateDpAsState`
- 支持无障碍
- 提供更方便的测试 API，甚至测试动画
- 支持 Kotlin 特性，如协程等
- 对其他 Jetpack 库的支持

## Android TV & Google TV

- 播放下一个的 API
- 投屏设备转移
- 增加了遥控器模拟器
- Firebase Test 支持 TV 测试

<!-- ##{"timestamp":1624969877}## -->