今年的 I/O 大会和往常一样，不过最有特色的就是 [Adventure](https://adventure.withgoogle.com/io) 这个线上会场，和摩尔庄园差不多，开发者可以在这里逛各个会场，还能全球的开发者聊天，钓鱼等等。

`Gmeek-html<img src="https://blog-image.ian2018.cn/images/06ab53e56d71d26e7192c5f432b6bff576da664e.png">`

好了，回到主题上来，[Jetpack](https://developer.android.com/jetpack) 发展到今天，已经包含了 100 多个库、工具和指南，内容已经非常丰富，现在开发项目几乎都会使用 Jetpack，毕竟相比于其他开源项目，Jetpack 背靠 Google 还是更靠谱。

下面介绍一下今年 I/O 大会 Jetpack 相关的内容。

# 一、Room

[Room](https://developer.android.com/training/data-storage/room) 是一个数据库工具，它在 SQLite 之上提供了抽象层，让开发者可以通过注解轻松完成数据库的开发工作。
`Gmeek-html<img src="https://blog-image.ian2018.cn/images/92accfb40571876567033954686f17ee78e58e02.png">`

Room 的更新点：

- **稳定支持 [KSP](https://kotlinlang.org/docs/ksp-overview.html)**：在 Room 2.4 中，已经稳定支持 KSP 处理注解，并且完全支持 Kotlin 1.6。之前使用的是 [KAPT](https://kotlinlang.org/docs/kapt.html) 处理注解，导致拖慢了编译速度。KSP 和 KAPT 相比，编译速度提高了 2 倍。
- **用 Kotlin 重写**：从 Room 2.5 开始使用 Kotlin 完成重写，同时仍然与 Java 编写的版本二进制兼容。
- **对 Paging 3.0 的内置支持**：允许 Room 查询返回 `PagingSource` 对象。
- **支持多映射关系查询**：可以直接执行 JOIN 查询，无需额外定义数据结构：
    ```kotlin
    @Query("SELECT * FROM Artist JOIN Song ON Artist.artistName = Song.songArtistName")
    fun getArtistToSongs(): Map<Artist, List<Song>>
    ```
- **支持数据库自动升级**：Database 注解增加 `autoMigrations` 参数，可以用于声明要从哪个版本升级到哪个版本，当 Room 需要执行有关表和列修改的信息时，还可以使用 `@AutoMigration` 注解的 spec 参数传入。
    ```kotlin
    @Database(
        version = 2,
        autoMigrations = {
            @AutoMigration(from = 1, to = , spec = MyMigration::class)
        }
    )
    abstract class MyDatabase : RoomDatabase()
    ```

# 二、Paging

[Paging](https://developer.android.com/topic/libraries/architecture/paging/v3-overview) 是一个分页加载库，应用程序数据可以在 RecylerViews 或 Compose LazyColumns 中分页加载。
`Gmeek-html<img src="https://blog-image.ian2018.cn/images/fc0a4ccd04ef6499f0106edf32563b43ff74df4f.png">`

Paging 更新点：

- **支持 Rx 和 Guava**：Paging 3.1 为 Rx 和 Guava 集成带来了稳定的支持，这为 Paging 对 Kotlin 协程的使用提供了 Java 版替代方案。
- **增加新的返回类型 `LoadResult.Invalid`**：改进了对无效竞争条件的处理。
- **增加了 `onPagesPresented` 和 `addOnPagesUpdateListener` API**：改进了对空页面的无操作加载的处理。
- **针对 Paging 3 添加更简单的 Codelab**：https://developer.android.com/codelabs/android-paging-basics，帮助开发者加快 Paging 3 的上手速度。

# 三、Navigation

[Navigation](https://developer.android.com/guide/navigation) 是一个页面导航框架，一般用于单 Activity 的架构中。

更新点：

- **提供了 [Navigation Compose](https://developer.android.com/reference/kotlin/androidx/navigation/compose/package-summary) 组件**：可以集成到 Jetpack Compose 中，可以使 Composable 函数作为程序导航目的地。
- **改进对多回退栈的支持**：NavigationUI 现在会自动保存和恢复弹出目标的状态，这意味着开发者可以支持多个回退栈（如多个 Tab 在切换时会保存或恢复当前 Tab 的回退栈），而无需更改任何代码。
- **增加对大屏幕的支持**：在 `AbstractListDetailFragment` 中提供了双窗格布局的预构建，此 Fragment 使用 SlidingPaneLayout 来管理由子类管理的列表窗格和使用 `NavHostFragment` 的详细信息窗格。
- **全部用 Kotlin 重写**：Navigation 所有组件都已经在 Kotlin 中重写，并使用泛型改进了类的可空性。

# 四、DataStore

[DataStore](https://developer.android.com/topic/libraries/architecture/datastore) 是一个数据存储库，旨在代替 `SharedPreferences` ，可以解决其大部分问题。

今年 DataStore 没有更新点，主要就是推荐大家用起来，提供了一系列视频和文章，帮助开发者学习和使用 DataStore：[MAD Skills（现代 Android 开发技能）: DataStore](http://goo.gle/mad-skills-datastore)。

# 五、JankStats

[JankStats](https://developer.android.com/studio/profile/jankstats) 是一个新的库，可以帮助开发者跟踪和分析应用 UI 中的性能问题，生成有关丢弃的渲染帧报告 "jank"。目前还是 alpha 版本。

更新点：

- **兼容 API 16**：JankStats 基于 Android 平台 API [FrameMetrics](https://developer.android.com/reference/android/view/FrameMetrics) 之上，但可以兼容到 API 16。
- **提供 UI 上下文**：JankStats 可以追踪 UI 和用户的当前状态，以便开发者可以了解问题发生的时间和用户当时在做什么。
- **报告回调**：在每一帧中，JankStats 都会通过侦听器收到有关该帧的信息的通知，包括该帧完成所用的时间、是否被视为卡顿以及该帧期间的 UI 上下文。

# 六、Baseline Profiles

[Baseline Profiles](https://developer.android.com/studio/profile/baselineprofiles) 库可以优化应用首次启动速度。其主要是通过预编译的方式来提高启动速度，Baseline Profiles 允许应用和库向 Android Runtime 提供有关代码路径使用情况的 metadata，它使用这些 metadata 来确定提前编译的优先级。

配置文件数据最终会作为 `baseline.prof` 文件放入 APK 中，然后在安装时使用改文件对应用程序及其静态链接库代码进行部分预编译，加快应用加载速度。
`Gmeek-html<img src="https://blog-image.ian2018.cn/images/8571f51147c57009c96c9a6915fa522a1553453a.png">`

- 在 Jetpack 的一些流行库中，已经使用 Baseline Profiles，例如 Fragments 和 Compose 等。
- 如果要创建应用自己的 Baseline Profiles 配置文件，需要使用 [Macrobenchmark](https://developer.android.com/studio/profile/baselineprofiles)。

# 七、Macrobenchmark

[Macrobenchmark](https://developer.android.com/studio/profile/macrobenchmark) 是一个基准测试库，兼容到 API 23，可以将 Jetpack 的基准测试覆盖范围扩展到更复杂的用例，包括应用启动和集成 UI 操作（如 RcyclerView 或运行动画），帮助开发者更好的了解应用性能。此外，Macrobenchmark 还可以用于生成 Baseline Profiles。

新加的实验功能：

- 使用 `TraceSectionMetric` 进行基于自定义跟踪的时序测量，它允许开发者对特定代码部分进行基准测试。
- `AudioUnderrunMetric` 可以检测音频缓冲区欠载，帮助了解音频卡顿。
- 增加 `BaselineProfileRule` ，它可以生成配置文件，以帮助上面提到的运行时优化。


# 八、Tracing

[Tracing](https://developer.android.com/jetpack/androidx/releases/tracing) 也是一个性能分析库，它可以将跟踪事件写入系统缓冲区，然后就可以使用 [Systrace 和 Perfetto](https://developer.android.google.cn/topic/performance/tracing) 等工具进行分析了。此库代替了已弃用的 `androidx.core.os.TraceCompat` 类。

更新点：

- Tracing 1.1 支持在 API 14 的非调试版本中进行分析，类似于在 API 29 中添加的 `<profileable>` 清单标记。


# 九、新的 WindowManager

新的 [WindowManager](https://developer.android.com/jetpack/androidx/releases/window) 库通过提供支持低至 API 14 的通用 API 服务，帮助开发者调整应用程序以适应新的设备外形，如可折叠设备。

- 初始版本针对可折叠设备用例，包括查询影响内容应该如何显示的物理属性。未来的版本将扩展到更多的显示类型和窗口功能。
- Jetpack 的 [`SlidingPaneLayout`](https://developer.android.com/guide/topics/ui/layout/twopane) 组件已经更新为使用 `WindowManager` 的智能布局 API 来实现，从而避免将内容放置在被遮挡的区域，如折叠屏跨物理铰链。


# 十、DragAndDrop

[DragAndDrop](https://developer.android.com/jetpack/androidx/releases/draganddrop) 是一个处理 [View 拖放功能](https://developer.android.com/guide/topics/ui/drag-drop)的库，有助于简化拖放功能的实现。
`Gmeek-html<img src="https://blog-image.ian2018.cn/images/a70457c69f2b8997e1276284d8ecca634fe5842f.gif">`
- 支持新的外形尺寸和窗口模式。
- 接受来自其应用程序内部和外部的拖放数据，并具有一致的放置目标可供性。
- 最低支持 API 24。

# 十一、AppCompat

[AppCompat](https://developer.android.com/jetpack/androidx/releases/appcompat) 一个老库了，它可以在低版本 API 上使用新 API 的 UI 特性（很多都符合 Material Design 准则）。

更新点：

- 从 1.8 开始集成了 Emoji2 库，为 API 14 及以上的 AppCompat 支持的所有基于文本的视图带来了对新表情符号的默认支持。
- 将[自定义语言环境选择](https://developer.android.com/about/versions/13/features/app-languages)从 API 33 向后移植到 API 14。


# 十二、Annotation

[Annotation](https://developer.android.com/jetpack/androidx/releases/annotation) 库提供了大家熟悉的注解，如 `@NonNull` 与 lint 检查配对。

更新点：

- 正在迁移到 Kotlin。
- 增加 `@ReturnThis`：确保相应方法的替换方法必须返回相同的实例（适用于构建器等）。
- 增加 `@OpenForTesting`：标记为 “open” 的 Kotlin 类和方法可以添加此注解，并且 lint 将确保此类仅在单元测试中子类化（并且仅替换方法）。
- 增加 `@DeprecatedSinceApi`：表示带注解的方法（或类/字段）是某个平台 API 的向后移植库的一部分，从指定的 API 级别开始将不再需要相应方法。
- 增加 `@EmptySuper`：表示相应方法被定义为空方法，因此在替换该方法时不需要调用它（实际上，您不应调用该方法，例如，它可以包含向后兼容性检查）。


# 十三、Jetpack Compose

[Compose](https://developer.android.com/jetpack/compose) 是一个新的 UI 框架，特色就是声明式 UI。现在已经是稳定版本，在 Play 商店中排名前 1000 的应用中，已经有 100 多个正在使用 Compose。

## Compose 主要更新点：

- **嵌套滚动优化**：当在视图系统中嵌入可组合滚动的 `CoordinatorLayout` 时，开发者现在可以确保它们的滚动行为是可互操作，这让可折叠工具栏的设置更加容易。可以通过实验 API `rememberNestedScrollInteropConnection` 传递给 nestedScroll 修饰符来选择加入此行为。
- **[可下载字体](https://developer.android.com/jetpack/compose/text#downloadable-fonts)**：开发者可以使用 Compose 的新 API 来异步访问 Google 字体，甚至可以定义备用字体，而无需任何复杂的设置。
- **懒布局**：增加了一个新的实验性 API `LazyLayout`，它允许开发者实现自定义的惰性布局。
- **字体填充**：通过可自定义的参数 `includeFontPadding`，使文本可以更精确的对齐，建议开发者设置为 false。
`Gmeek-html<img src="https://blog-image.ian2018.cn/images/0791b8933fa07a9ba1b475665e7dc88d8971462e.png">`
- **文字放大镜**：Android 文本提供了一个放大镜小部件，可以更轻松地选择文本。Compose 现在也支持文本放大镜。
`Gmeek-html<img src="https://blog-image.ian2018.cn/images/d3f98130b33a412ff8d54f69cc41e1865e20ee1f.png">`
- **窗口插图**：[Accompanist](https://google.github.io/accompanist/) 中的 Insets 库已经升级到 Compose Foundation 库中。
    - Accompanist 是一个类似实验室的环境，适用于新的 Compose API。我们使用它来帮助填补 Compose 工具包中的已知空白，试验新的 API，并收集有关开发 Compose 库的开发经验的见解。这些库的目标是将它们上游到官方工具包中，此时它们将被弃用并从 Accompanist 中删除。
- **窗口尺寸等级**：为了更容易设计、开发和测试可调整大小的布局，发布了窗口大小类 [`material3-window-size-class`](https://developer.android.com/jetpack/androidx/releases/compose-material3) 作为 Material 3 库集的一部分，它们现在在新库中以 alpha 形式提供。
- **支持 Wear OS**：Compose for Wear OS 进入 Beta 版，意味着它的功能完整且 API 稳定。

## Android Studio Compose 工具更新点：

- 从 Android Studio Dolphin 开始，开发者可以使用 `Layout Inspector` 检查 recomposition 的频率，大量的 recomposition 可以帮助开发者指向可以优化方向。此外，Android Studio Electric Eel 现在包括一个 recomposition 荧光笔，这是一个用于查看哪些 composables 项何时 recomposition 的视觉辅助工具。
`Gmeek-html<img src="https://blog-image.ian2018.cn/images/eb13b85a4c58590bb4f0854844cbc598611fe6ef.png">`
- Android Studio Electric Eel（在 Canary 中）带来了 [LiveEdit](https://developer.android.com/jetpack/compose/tooling#live-edit)，可以自动更新 Preview 对象，并将代码更改部署到模拟器或设备上，从而实时查看可组合项更新后的效果。
`Gmeek-html<img src="https://blog-image.ian2018.cn/images/fc0e3429f6bf3964f991852258928ea15e952a1d.gif">`


> 参考资料：
>
> [2022 Google I/O Jetpack 的新动态](https://io.google/2022/program/c7710536-a86d-42a6-b43e-e69ae76dbb34/intl/zh/)
>
> [Google I/O 2022：Jetpack 的新功能 - 掘金- 恋猫de小郭](https://juejin.cn/post/7097029239731388446)
> 
> [What's new in Jetpack Compose](https://android-developers.googleblog.com/2022/05/whats-new-in-jetpack-compose.html)


<!-- ##{"timestamp":1653829997}## -->