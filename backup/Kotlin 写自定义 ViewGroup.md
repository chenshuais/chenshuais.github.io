`Gmeek-html<img src="https://blog-image.ian2018.cn/images/2bba4a3bdad5d534b40c4ffa5281b2761d91b28a.png">`

Android 最近推行的 Compose ，有着 Kotlin 的加持，使写 UI 更加方便快速，不用担心布局嵌套，还是声明式 UI，那么 Compose 有这么多好处，原生写法还有 “出路” 吗？

今天给大家分享一种非传统的自定义 ViewGroup 写法，让你对自定义 ViewGroup 不再 “恐惧”，再借助 Kotlin ，我们用原生写法，也可以快速写出无嵌套的布局。



## 为什么要用自定义 ViewGroup

平时大家写 UI，都直接用 xml 去写布局，或多或少都会注意去避免布局嵌套，自从有了 ConstraintLayout，嵌套的情况就减少了许多，但用 ConstraintLayout 就能达到极致性能吗？整体上相较于其他 Layout ，性能确实[有所提升](https://cloud.tencent.com/developer/article/1365408)。但对于产品中具体的页面，可能就不会达到极致性能，因为 ConstraintLayout 要考虑的场景太多了，导致其逻辑很复杂，对于确定的页面来说，一个有 “针对性” 的自定义 ViewGroup ，是能够超越 ConstraintLayout 的，因为你只需要对一个页面负责即可，不用考虑那么全。

这里我所说的自定义 ViewGroup 就是用代码去写布局，不是写一个公共的控件让别人去使的那种。[Telegram](https://github.com/DrKLO/Telegram/) 的布局就全部用代码去写的。

那么我们平时为什么不去用代码写布局呢？

* 自定义 ViewGroup 太复杂了，什么 MeasureSpec 情况有一堆。
* 每次写都忘，还得去查，学了就忘
* 效率太低，我为了那点性能提升，没必要

总结一下，就是因为自定义 ViewGroup 比较难，还费事。以前用 Java 写代码确实会比较麻烦，但现在有了 Kotlin，也可以很优雅的去用代码写布局了。接下来，我就带大家来捋一下自定义 ViewGroup 的流程，然后用 Kotlin 去实现。



## 自定义 ViewGroup 要做什么

一个 ViewGroup 有哪几步，我想大家都知道，无非就是测量、布局、绘制，就这三步，测量就是把子 View 的大小测量一下，再算一下自己的大小；布局就是设置一下子 View 的位置；绘制对于 ViewGroup 来说一般不需要，无非就是在自己这画点什么东西。这么看也不是很难嘛，那么难的是哪里呢？我想就是因为下面这张表：

`Gmeek-html<img src="https://blog-image.ian2018.cn/images/d6cfd28d1c2d38c89b5e4eb097ac3f6215240320.png">`

就是测量的时候的各种模式，很多书上讲自定义 View 的时候都会给出这张表，其实这是作者自己总结出来的，Android 官网上是没有这些东西的。

现在让我们忘记上面这张表，就只看一下有几种测量模式：EXACTLY、AT_MOST、UNSPECIFIED，这三个英文意思已经很明确了。

* EXACTLY 就是精确的，就是你设置多少就是多少
* AT_MOST 就是最多能用多少，就是尽可能满足子 View
* UNSPECIFIED 就是不确定的，这种一般都是需要再次测量的，就比如 LinearLayout 使用 weight 

其中 UNSPECIFIED 对于我们用代码写布局这种情况，几乎就不会用到，这种就可以不用考虑，那么就只剩下两种模式了，总结一下就是 “View 实际多少就是多少” 和 "View 最多能用多少"，这么一想，是不是就没那么复杂了。光说大家估计也没有具体的概念，接下来上代码。



## 如何借助 Kotlin 提升写 ViewGroup 效率

接下来，让我们用 Kotlin 的扩展方法，来一步一步去完成自定义 ViewGroup 需要的东西。

测量的时候要传一个 MeasureSpec 对象，这个对象是根据宽高 Int 值和测量模式确定的，有了 Kotlin ，我们是不是可以直接给 Int 定义一个扩展方法，来获取这个 Int 值的 MeasureSpec 不就行了，来，看代码：

```kotlin
// EXACTLY 的测量模式
fun Int.toExactlyMeasureSpec() = MeasureSpec.makeMeasureSpec(this, MeasureSpec.EXACTLY)
// AT_MOST 的测量模式
fun Int.toAtMostMeasureSpec() = MeasureSpec.makeMeasureSpec(this, MeasureSpec.AT_MOST)
```

我们给一个控件设置宽高，一般都是给个具体的值，要么就是 MATCH_PARENT 或者 WRAP_CONTENT，那么我们是不是也这种常见的情况抽成一个方法呢？有了 Kotlin 我们可以直接在这个 View 上弄个扩展方法，来获取它的默认宽高：

```kotlin
// 获取 View 宽度的默认测量值
fun View.defaultWidthMeasureSpec(parent: ViewGroup): Int {
    return when (layoutParams.width) {
        // 如果是 MATCH_PARENT，就说明它要填满父布局，那就给它一个父布局宽度的精确值呗
        MATCH_PARENT -> parent.measuredWidth.toExactlyMeasureSpec()
        // 如果是 WRAP_CONTENT，就说明它满足自己的大小就行，那么我们应该尽可能提供剩余可用空间
        WRAP_CONTENT -> WRAP_CONTENT.toAtMostMeasureSpec()
        // 0 就是不确定的，这里我们有 UI 稿，就没有不确定的情况了，所以这里就不用考虑了
        0 -> throw IllegalAccessException("我不考虑这种情况 $this")
        // 最后就是具体的值了，那就给你具体的呗
        else -> layoutParams.width.toExactlyMeasureSpec()
    }
}
// 获取 View 高度的默认测量值，和上面获取宽度的原理一样
fun View.defaultHeightMeasureSpec(parent: ViewGroup): Int {
    return when (layoutParams.height) {
        MATCH_PARENT -> parent.measuredHeight.toExactlyMeasureSpec()
        WRAP_CONTENT -> WRAP_CONTENT.toAtMostMeasureSpec()
        0 -> throw IllegalAccessException("我不考虑这种情况 $this")
        else -> layoutParams.height.toExactlyMeasureSpec()
    }
}
```

好了，有了这些，我们再写自定义 ViewGroup 是不是就简单多了，我们测量一个控件，直接这些写就可以了：

```kotlin
textView.measure(textView.defaultWidthMeasureSpec(this), textView.defaultHeightMeasureSpec(this))
```

等等，这样写还是有点复杂，我们为什么不干脆再定义一个扩展方法，让 View 直接按默认的测量好了：

```kotlin
fun View.autoMeasure(parent: ViewGroup) {
    measure(
        this.defaultWidthMeasureSpec(parent),
        this.defaultHeightMeasureSpec(parent)
    )
}
```

这样下次使用就可以这样写了：

```kotlin
textView.autoMeasure(this)
```

是不是更简单了，到这，测量的基本代码差不多就写完了，顺便把布局的基础方法也写一下吧，布局就比较简单了，就是告诉子 View 的位置就好了。

```kotlin
// 设置 view 的位置
fun View.autoLayout(parent: ViewGroup, x: Int = 0, y: Int = 0, fromRight: Boolean = false) {
    // 判断布局是不是从右边开始
    if (!fromRight) {
        // 注意这里为什么用 measuredWidth 而不是用 width
        // 因为 width 是通过 mRight - mLeft 计算的，而这时它俩都没有被赋值，所以都是 0
        layout(x, y, x + measuredWidth, y + measuredHeight)
    } else {
        autoLayout(parent.measuredWidth - x - measuredWidth, y)
    }
}
```

我们其实可以把这些方法都写到一个类，以后写自定义 ViewGroup，直接继承它就可以了，就像下面这样：

```kotlin
 // 为了方便设置 dp sp，直接在这里声明了扩展属性
val Int.dp
    get() = TypedValue.applyDimension(
        TypedValue.COMPLEX_UNIT_DIP, this.toFloat(), 
        Resources.getSystem().displayMetrics
    ).toInt()
val Float.sp
    get() = TypedValue.applyDimension(
        TypedValue.COMPLEX_UNIT_SP, this,
        Resources.getSystem().displayMetrics
    )

abstract class CustomViewGroup(context: Context) : ViewGroup(context) {

    // 方便获取带 Margin 的宽高
    protected val View.measuredWidthWithMargins get() = measuredWidth + marginStart + marginEnd
    protected val View.measuredHeightWithMargins get() = measuredHeight + marginTop + marginBottom

    protected fun Int.toExactlyMeasureSpec() = MeasureSpec.makeMeasureSpec(this, MeasureSpec.EXACTLY)
  
    protected fun Int.toAtMostMeasureSpec() = MeasureSpec.makeMeasureSpec(this, MeasureSpec.AT_MOST)

    protected fun View.defaultWidthMeasureSpec(parent: ViewGroup): Int {
        return when (layoutParams.width) {
            MATCH_PARENT -> parent.measuredWidth.toExactlyMeasureSpec()
            WRAP_CONTENT -> WRAP_CONTENT.toAtMostMeasureSpec()
            0 -> throw IllegalAccessException("我不考虑这种情况 $this")
            else -> layoutParams.width.toExactlyMeasureSpec()
        }
    }

    protected fun View.defaultHeightMeasureSpec(parent: ViewGroup): Int {
        return when (layoutParams.height) {
            MATCH_PARENT -> parent.measuredHeight.toExactlyMeasureSpec()
            WRAP_CONTENT -> WRAP_CONTENT.toAtMostMeasureSpec()
            0 -> throw IllegalAccessException("我不考虑这种情况 $this")
            else -> layoutParams.height.toExactlyMeasureSpec()
        }
    }

    protected fun View.autoMeasure() {
        measure(
            this.defaultWidthMeasureSpec(this@CustomViewGroup),
            this.defaultHeightMeasureSpec(this@CustomViewGroup)
        )
    }

    protected fun View.autoLayout(x: Int = 0, y: Int = 0, fromRight: Boolean = false) {
        if (!fromRight) {
            layout(x, y, x + measuredWidth, y + measuredHeight)
        } else {
            autoLayout(this@CustomViewGroup.measuredWidth - x - measuredWidth, y)
        }
    }
}
```



## 写个自定义 ViewGroup 试试

`Gmeek-html<img src="https://blog-image.ian2018.cn/images/3afa2ec5945a1469af18d637e5c965e1a4eb13e3.png">`

就以计算器界面为例吧。上面👆是通过 ConstraintLayout 实现的，看看有哪些控件，1 个 EditText，17 个 Button。我们来试试用自定义 ViewGroup 来简单复刻一下，直接上代码吧：

```kotlin
class CalculatorLayout(context: Context) : CustomViewGroup(context) {
    // 我们可以直接这样在把控件 new 出来，设置一些属性，这样我们还省去了 findViewById，而且不用担心空指针
    val etResult = AppCompatEditText(context).apply {
        typeface = ResourcesCompat.getFont(context, R.font.comfortaa_regular)
        setTextColor(ResourcesCompat.getColor(resources, R.color.white, null))
        background = null
        textSize = 65f
        gravity = Gravity.BOTTOM or Gravity.END
        maxLines = 1
        isFocusable = false
        isCursorVisible = false
        setPadding(16.dp, paddingTop, 16.dp, paddingBottom)
        layoutParams = LayoutParams(LayoutParams.MATCH_PARENT, LayoutParams.WRAP_CONTENT)
        // 注意，这里直接 add 不会触发 onMeasure 这些流程，可以放心 add
        addView(this)
    }
    // 数字键盘后面的背景
    val keyboardBackgroundView = View(context).apply {...}

    class NumButton(context: Context, text: String, parent: ViewGroup) : AppCompatTextView(context) {
        init {
            setText(text)
            gravity = Gravity.CENTER
            background =
                ResourcesCompat.getDrawable(resources, R.drawable.ripple_cal_btn_num, null)
            layoutParams =
                MarginLayoutParams(LayoutParams.WRAP_CONTENT, LayoutParams.WRAP_CONTENT).apply {
                    leftMargin = 2.dp
                    rightMargin = 2.dp
                    topMargin = 6.dp
                    bottomMargin = 6.dp
                }
            isClickable = true
            setTextAppearance(context, R.style.StyleCalBtn)
            parent.addView(this)
        }
    }
  
    val btn0 = NumButton(context, "0", this)
    ...

    init {
        // 给自己设置个背景
        background = ResourcesCompat.getDrawable(resources, R.drawable.shape_cal_bg, null)
    }

    override fun onMeasure(widthMeasureSpec: Int, heightMeasureSpec: Int) {
        super.onMeasure(widthMeasureSpec, heightMeasureSpec)
        // 先算一下数字按钮的大小
        val allSize =
            measuredWidth - keyboardBackgroundView.paddingLeft - keyboardBackgroundView.paddingRight -
                    btn0.marginLeft * 8
        val numBtSize = (allSize * (1 / 3.8)).toInt()
        // 计算操作按钮的大小
        val operatorBtWidth = (allSize * (0.8 / 3.8)).toInt()
        val operatorBtHeight = (numBtSize * 4 + btn0.marginTop * 6 - btnDel.marginTop * 8) / 5

        // 再计算数字盘的高度
        val keyboardHeight =
            keyboardBackgroundView.paddingTop + keyboardBackgroundView.paddingBottom +
                    numBtSize * 4 + btn0.marginTop * 8

        // 最后把高度剩余空间都给 EditText
        val editTextHeight = measuredHeight - keyboardHeight

        // 测量背景
        keyboardBackgroundView.measure(
            measuredWidth.toExactlyMeasureSpec(),
            keyboardHeight.toExactlyMeasureSpec()
        )

        // 测量按钮
        btn0.measure(numBtSize.toExactlyMeasureSpec(), numBtSize.toExactlyMeasureSpec())
        ...
        btnDiv.measure(operatorBtWidth.toExactlyMeasureSpec(), operatorBtHeight.toExactlyMeasureSpec())
        ...

        // 测量 EditText
        etResult.measure(
            measuredWidth.toExactlyMeasureSpec(),
            editTextHeight.toExactlyMeasureSpec()
        )

        // 最后设置自己的宽高
        setMeasuredDimension(measuredWidth, measuredHeight)
    }

    override fun onLayout(changed: Boolean, l: Int, t: Int, r: Int, b: Int) {
        // 好了，测量都完了，就一个个放吧
        // 先放 EditText
        etResult.autoLayout()

        // 把背景放上
        keyboardBackgroundView.autoLayout(0, etResult.bottom)

        // 开始放按钮吧
        btn7.let {
            it.autoLayout(
                keyboardBackgroundView.paddingLeft + it.marginLeft,
                keyboardBackgroundView.top + keyboardBackgroundView.paddingTop + it.marginTop
            )
        }
        btn8.let {
            it.autoLayout(
                btn7.right + btn7.marginRight + it.marginLeft,
                btn7.top
            )
        }
        btn9.let {
            it.autoLayout(
                btn8.right + btn8.marginRight + it.marginLeft,
                btn7.top
            )
        }
        ...
    }
}
```

Ok，以上就完成了自定义 ViewGroup，我们可以直接这样用了：

```kotlin
override fun onCreate(savedInstanceState: Bundle?) {
    super.onCreate(savedInstanceState)
    val contentView = CalculatorLayout(this)
    setContentView(contentView)
    
    // 不用 findViewById 和 ktx插件、ViewBinding 这些东西，直接用就可以
    contentView.btnDel.setOnClickListener {
        contentView.etResult.setText("")
    }
}
```

看看最终的对比效果：

`Gmeek-html<img src="https://blog-image.ian2018.cn/images/eac7aa7435f613da784a1ab8ab2af6545a3cd845.png">`

看上去还算可以，怎么样，是不是用 Kotlin 代码写布局也不是很复杂，缺点就是不能在 Android Studio 上预览（其实是可以显示预览框的，只要在自定义 View 的构造方法加上 AttributeSet 参数就可以了，不过不一定能显示成功，反正我这预览的还是灰色的）。


## 结束了？

这就完了？是不是感觉还少点，如果我在 xml 里设置了 style 怎么办？设置 style 可是减少重复代码的常用手段，那么怎么在代码里设置 style 呢？

据我所知，Android 官方是没有提供设置 style 的 API 的，那么就没法用 style 了吗？当然不是，我们可以换个思路。

我们用 style 是为了减少相同样式 View 的重复代码，将重复代码抽成 style 达到复用的效果，其实我们上面的那种写法就已经达到了这个目的，就是 `NumButton` 那个类，我们把重复的属性设置都放到了这个类里，看，这样是不是已经满足设置 style 的需求了。


什么？你就要用 style。好吧，那就再教大家一个小技巧来用代码设置 style。

大家记不记得 View 的构造方法中有一个是四个参数的：

```java
/**
 * ...
 * @param defStyleAttr An attribute in the current theme that contains a
 *        reference to a style resource that supplies default values for
 *        the view. Can be 0 to not look for defaults. 
 *        当前主题中的一个属性，包含对为视图提供默认值的样式资源的引用。可以为 0 以不查找默认值
 * @param defStyleRes A resource identifier of a style resource that
 *        supplies default values for the view, used only if
 *        defStyleAttr is 0 or can not be found in the theme. Can be 0
 *        to not look for defaults.
 *        为视图提供默认值的样式资源的资源标识符，仅在 defStyleAttr 为 0 或在主题中找不到时使用。可以为 0 以不查找默认值。
 * ...
 */
public View(context: Context, attrs: AttributeSet?, defStyleAttr: Int, defStyleRes: Int)
```

着重看一下后两个参数的意思，其中第四个参数 defStyleRes 不就是我们要找的设置 style 的地方嘛。那么我们想要给 View 设置 style 可以这样写：

```kotlin
// 注意，这里的第三个参数传 0 ，原因上面源码注释里已经写了
val btn0 = Button(context, null, 0, R.style.StyleCalBtn)
```

这样我们写的 style 就能用了。但是... 还有一个问题，有的 View 就没给提供四个参数的构造方法咋办？就比如 `AppCompatButton` ，像这种情况我们怎么设置 style 呢？这时候就需要看一下 View 的第三个参数 defStyleAttr 的含义了。

defStyleAttr 的解释当前主题中的一个属性 attr，这个属性里有默认 style 的引用，那么，我们把这个默认 style 的引用换成我们自己的不就达到了设置 style 的目的了嘛。

先看一下 `AppCompatButton` 的 attr 名字叫什么：

```java
public AppCompatButton(@NonNull Context context, @Nullable AttributeSet attrs) {
    this(context, attrs, R.attr.buttonStyle);
}
```

根据 `AppCompatButton` 的源码可以知道，它的 attr 叫 buttonStyle，然后我们就可以自定义一个 theme，把 buttonStyle 的值给覆盖掉：

```xml
<style name="Theme.CalBtn" parent="Theme.CustomViewGroup">
    <!-- 注意这里前面加了 android: -->
    <item name="android:buttonStyle">@style/StyleCalBtn</item>
</style>
```

那么我们怎么用这个主题呢？直接传到 View 的第三个参数上？这样不行，第三个参数要的是一个 attr，不能传 theme。那怎么办呢？

我们可以从 Context 上做手脚，通过 `ContextThemeWrapper` 给 Context 再包装一层：

```kotlin
fun Context.toTheme(@StyleRes style: Int) = ContextThemeWrapper(this, style)
```

这样 View 在通过 `context.obtainStyledAttributes()` 方法获取属性值的时候，就可以加载我们设置的主题了。

新的写法就可以这样：

```kotlin
// 注意，这里不能只传 context ，如果只传 context 它内部会走 (context, null, 0) 这个构造，导致设置的 style 无效
val btn0 = AppCompatButton(context.toTheme(R.style.Theme_CalBtn), null, android.R.attr.buttonStyle)
```

这下可以用代码设置 style 了吧。


## 真的结束了

虽然与 xml 的书写相比，确实有些麻烦，但熟练之后，我感觉都差不多，还能帮助我们对自定义 View 这块更加熟悉，感兴趣的小伙伴可以在项目不忙的时候先试试这种写法。当然，也可以试试 Compose，其实 Compose 最终也是一个 ViewGroup ，可以看看 `AndroidComposeView` ，它最终也是会添加到 DecorView 上。

最后感谢纯纯写作的作者 [drakeet](https://github.com/drakeet) 大神的分享。

Demo 地址：[https://github.com/IAn2018cs/CustomViewGroup](https://github.com/IAn2018cs/CustomViewGroup)

<!-- ##{"timestamp":1630931940}## -->