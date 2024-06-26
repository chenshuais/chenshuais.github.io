## 特殊图形所需设计素材
- 一张特殊图形的透明度遮罩（如下图中第二张图的样式，被剪裁的部分是透明的，图片需要是正方形的）
`Gmeek-html<img src="https://blog-image.ian2018.cn/images/73aed981c0f66138f30b00f901c542a4317c0c8d.png">`

## 限制

### 支持的布局：
- `FrameLayout`
- `LinearLayout`
- `RelativeLayout`
- `GridLayout`

### 支持的控件：
- [`AnalogClock`](https://developer.android.com/reference/android/widget/AnalogClock)（模拟时钟）
- `Button`
- [`Chronometer`](https://developer.android.com/reference/android/widget/Chronometer)（计时器）
- `ImageButton`
- `ImageView`
- `ProgressBar`
- `TextView`
- [`ViewFlipper`](https://developer.android.com/reference/android/widget/ViewFlipper)（多页管理控件，可以用作图片轮播）
- `ListView`
- `GridView`
- [`StackView`](https://developer.android.com/reference/android/widget/StackView)（堆叠视图，可以做多图切换）
- [`AdapterViewFlipper`](https://developer.android.com/reference/android/widget/AdapterViewFlipper)（多个 View 切换，切换时可以有渐隐的动画效果）

在 Android 12 上，增加了对 CheckBox 等控件的支持：
- `CheckBox`
- `Switch`
- `RadioButton`


## Android 12 更新

> 从 Android 推出至今，AppWidget 的 API 基本就没有什么大的变化，从 2012 年到 2021 年更是只有一个 Android 版本包含了对 AppWidget API 的更新 --- [Google Android 开发者](https://juejin.cn/post/7051439506817286181#heading-2)

[Android 12上焕然一新的小组件：美观、便捷和实用](https://juejin.cn/post/6968851189190377480#heading-7)

- 增加 Widget 圆角设置
- 增加主题颜色变化
- 增加 Widget 动态预览，在添加 Widget 时，可以看到真实的效果
- 支持响应式布局，可以根据 Widget 的大小选择不同的布局
- 增加 targetCellWidth/targetCellHeight 配置，来直接指定 Widget 在桌面占用的 cell 大小（2 * 2 等）
- 简化数据绑定，新增 setRemoteAdapter API
- 支持 Compose 编写 Widget：[Jetpack Glance](https://juejin.cn/post/7042468014251311112)，不过也不能随心所欲，支持的控件都差不多，也是那几种


## 结论
- 支持的布局和控件有限，不能播放视频 （[官方文档](https://developer.android.com/guide/topics/appwidgets)）
- 动画可以支持帧动画、和一些旋转、位移、缩放、透明度等简单动画，不过需要采用特殊手段，会比较耗电（[Android-自定义桌面小部件【搞定小米MIUI小部件】 - 掘金](https://juejin.cn/post/7048623673892143140)， [AppWidget 添加动画效果](https://www.jianshu.com/p/228a8ad91829)）
- 可以通过剪裁图片，来显示一些特殊形状的图

<!-- ##{"timestamp":1644549811}## -->