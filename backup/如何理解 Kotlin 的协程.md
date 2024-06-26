`Gmeek-html<img src="https://blog-image.ian2018.cn/images/3efa3eb9d8217fb232eebea4d0745a1c8fee7fb9.png">`

说起 Kotlin，它和 Java 对比最大的优势应该就是协程了。

## 什么是协程

对于 Java 语言的开发者来说，协程这个概念应该很陌生，因为一般学习计算机专业的都只知道进程、线程，从来都没有协程的概念，没有之前经验的类比，学习 Kotlin 的协程就很摸不着头脑。

`Gmeek-html<img src="https://img.soogif.com/W6xpQJJ5arKfZ9OR6E0vWOMRQzNIYvGR.gif?scope=mdnice">`

其实协程这个概念很早之前就有了，在 1967 的 Simula 67 编程语言中就已经支持了协程，不过在之后的几十年里都没有推广开来，直到 2012 年左右，C# 才支持了协程，之后 JavaScript、Python 等语言也支持了协程。

`Gmeek-html<img src="https://blog-image.ian2018.cn/images/6b6f7b8464f9ef004fa1a075435b6ecc55b4de2a.png">`


Kotlin 的协程是在 2017 年加入的，2019 年才正式发布，到现在也才三年多，学习资料少，再加上 Kotlin 协程易学难精，贸然在生产环境中使用协程，可能会遇到各种各样的问题，所以，Kotlin 在业界的普及率并不高。

说了这么多，那么协程到底是什么呢？协程能做什么呢？其实协程和线程一样，都是用来做异步、并发的。

开始的时候相信大家都听说过这样一种说法，「协程是更加轻量的线程」，说协程是运行在线程之上的。后来大家又说 Kotlin 协程其实就是一个「封装线程的框架」，将线程池进一步封装，本质上还是线程。那么这两种说法哪个才是正确的呢？其实这两种说法都可以说是正确的，只不过它俩针对的角度不同。

* 「协程是更加轻量的线程」是针对广义上的协程，是对程序当中的协程概念更加贴切的解释。
* 「封装线程的框架」则是针对其底层实现的。

这就好比一个产品经理和一个程序员，产品经理提需求说：“我想要一个轻量的线程，它能运行在线程上，你去做吧”，程序员说：“~~这需求做不了~~ 我给你用线程模拟一个吧”。

`Gmeek-html<img src="https://img.soogif.com/6AhLdk2FINR4kRvxCj3dpQxGZBdIZtpO.gif?scope=mdnice">`


对于我们刚刚学习 Kotlin 协程来说，「封装线程的框架」作为第一印象即可，把它理解成「协程是更加轻量的线程」对我们学习协程来说更有帮助，学习都是一个循序渐进的过程，我们得先知道它的用法、运作模型，然后再去探索它的实现原理，这样才能更好的理解 Kotlin 的协程，如果我们一上来就去啃那些底层的实现，硬拿线程去类比解释，是很困难的。

接下来用一段代码来说明一下「协程是更加轻量的线程」这一概念。

Kotlin 的协程没有集成到标准库中，需要单独依赖：

```gradle
dependencies {
    implementation("org.jetbrains.kotlinx:kotlinx-coroutines-core:1.6.0")
    // Android
    implementation("org.jetbrains.kotlinx:kotlinx-coroutines-android:1.6.0")
}
```

普通线程写法：

```kotlin
fun getResult1(): String {
    println("Start getResult1, Thread:${Thread.currentThread().name}")
    Thread.sleep(1000)
    println("End getResult1, Thread:${Thread.currentThread().name}")
    return "Result1"
}

fun getResult2(): String {
    println("Start getResult2, Thread:${Thread.currentThread().name}")
    Thread.sleep(1000)
    println("End getResult2, Thread:${Thread.currentThread().name}")
    return "Result2"
}

fun getResult3(): String {
    println("Start getResult3, Thread:${Thread.currentThread().name}")
    Thread.sleep(1000)
    println("End getResult3, Thread:${Thread.currentThread().name}")
    return "Result3"
}

fun main() {
    val startTime = System.currentTimeMillis()
    val result1 = getResult1()
    val result2 = getResult2()
    val result3 = getResult3()
    val results = listOf(result1, result2, result3)
    val time = System.currentTimeMillis() - startTime

    println("Time: $time")
    println("results: $results")
}
```

打印结果：

```
Start getResult1, Thread:main
End getResult1, Thread:main
Start getResult2, Thread:main
End getResult2, Thread:main
Start getResult3, Thread:main
End getResult3, Thread:main
Time: 3033
results: [Result1, Result2, Result3]
```

协程写法：

```kotlin
suspend fun getResult1(): String {
    println("Start getResult1, Thread:${Thread.currentThread().name}")
    delay(1000)
    println("End getResult1, Thread:${Thread.currentThread().name}")
    return "Result1"
}

suspend fun getResult2(): String {
    println("Start getResult2, Thread:${Thread.currentThread().name}")
    delay(1000)
    println("End getResult2, Thread:${Thread.currentThread().name}")
    return "Result2"
}

suspend fun getResult3(): String {
    println("Start getResult3, Thread:${Thread.currentThread().name}")
    delay(1000)
    println("End getResult3, Thread:${Thread.currentThread().name}")
    return "Result3"
}

fun main() {
    runBlocking {
        val startTime = System.currentTimeMillis()
        val result1 = async { getResult1() }
        val result2 = async { getResult2() }
        val result3 = async { getResult3() }
        val results = listOf(result1.await(), result2.await(), result3.await())
        val time = System.currentTimeMillis() - startTime

        println("Time: $time")
        println("results: $results")
    }
}
```

打印结果：

```
Start getResult1, Thread:main
Start getResult2, Thread:main
Start getResult3, Thread:main
End getResult1, Thread:main
End getResult2, Thread:main
End getResult3, Thread:main
Time: 1014
results: [Result1, Result2, Result3]
```

可以看出都是这两种写法都是在 main 线程上运行的，但协程的写法却能在单线程上并发执行，这种现象用「协程其实还是线程」去想就不好理解了。

如果我们把协程理解成是一种轻量的线程就很好解释了，因为协程可以运行在线程上，就可以多个协程同时执行了。

`Gmeek-html<img src="https://blog-image.ian2018.cn/images/357c4f7e9031dd8c68ea798f2ade7b811bc1eb40.png">`

我们给 IDEA 设置一下 VM 参数：`-Dkotlinx.coroutines.debug`，这样打印出来的线程名字中就包含了协程的信息。

`Gmeek-html<img src="https://blog-image.ian2018.cn/images/3a1ff93948b2260c5a0476ec4108ee40a7a22024.png">`

然后再次运行上面协程写法的代码，打印结果如下：

```
Start getResult1, Thread:main @coroutine#2
Start getResult2, Thread:main @coroutine#3
Start getResult3, Thread:main @coroutine#4
End getResult1, Thread:main @coroutine#2
End getResult2, Thread:main @coroutine#3
End getResult3, Thread:main @coroutine#4
Time: 1015
results: [Result1, Result2, Result3]
```

这下就更清楚了，可以看到上面协程写法的代码其实在 main 线程上运行了三个协程。

## 什么是协程的非阻塞

协程对比线程的另一个特点就是非阻塞，举个栗子

`Gmeek-html<img src="https://img.soogif.com/fCnkbyO30VyAxkMkMQMGzwWadCVCafE7.gif?scope=mdnice">`

```kotlin
fun main() {
    repeat(2) {
        Thread.sleep(1000)
        println("Print-1, Thread:${Thread.currentThread().name}")
    }

    repeat(2) {
        Thread.sleep(900)
        println("Print-2, Thread:${Thread.currentThread().name}")
    }
}
```

打印结果：

```
Print-1, Thread:main
Print-1, Thread:main
Print-2, Thread:main
Print-2, Thread:main
```

可以看到在同一个线程中，普通线程会阻塞后面的运行，代码是一行一行执行的。再来看看协程的表现是什么样的：

```kotlin
fun main() {
    runBlocking {
        launch {
            repeat(2) {
                delay(1000)
                println("Print-1, Thread:${Thread.currentThread().name}")
            }
        }

        launch {
            repeat(2) {
                delay(900)
                println("Print-2, Thread:${Thread.currentThread().name}")
            }
        }
    }
}
```

打印结果：

```
Print-2, Thread:main @coroutine#3
Print-1, Thread:main @coroutine#2
Print-2, Thread:main @coroutine#3
Print-1, Thread:main @coroutine#2
```

同样是一个线程，协程就不会阻塞 Print-2 的执行。如果我们再把上面的代码修改一下：

```kotlin
fun main() {
    runBlocking {
        launch {
            repeat(2) {
                // delay(1000)
                // 改成用 Thread sleep
                Thread.sleep(1000)
                println("Print-1, Thread:${Thread.currentThread().name}")
            }
        }

        launch {
            repeat(2) {
                // delay(1000)
                // 改成用 Thread sleep
                Thread.sleep(1000)
                println("Print-2, Thread:${Thread.currentThread().name}")
            }
        }
    }
}
```

再看一下打印结果：

```
Print-1, Thread:main @coroutine#2
Print-1, Thread:main @coroutine#2
Print-2, Thread:main @coroutine#3
Print-2, Thread:main @coroutine#3
```

这次表现就和线程一样了，这是因为协程中的非阻塞其实只是语言层面的，要实现协程的非阻塞，靠的是 `suspend` 关键字，我们来看一下 delay 函数的函数签名：

```kotlin
public suspend fun delay(timeMillis: Long)
```

只有标记了 `suspend` 的函数才能实现非阻塞的效果。那么如何理解 `suspend` 呢？

suspend 的意思就是挂起，非阻塞就是当一个任务需要等待的时候，先把它挂起来，不影响后面的任务执行，等它等待完成时，再恢复这个任务。看下面这动图会更清楚些：

`Gmeek-html<img src="https://blog-image.ian2018.cn/images/f89c9ec3bf6b868e899df7bafe444409c721986f.gif">`

`Gmeek-html<img src="https://blog-image.ian2018.cn/images/47f1218fd7707ccf006ebeab9668e69f5210c3d9.gif">`

可以想象被标记了 `suspend` 的函数比普通函数多了一个 “挂钩”，这样这个任务就能被 “调度中心” 挂起来，等需要的时候再放下了继续执行。


## 你需要知道的

说了这么多，其实并没有讲 Kotlin 协程的原理，只是分享了一下初学者怎么去理解 Kotlin 的协程，先建立一个思维模型，这样在以后学习 Kotlin 的协程时，也会更好理解。

上面这些内容只是想表达以下几点：

* Kotlin 的协程是一套独立于标准库的框架，它封装了 Java 的线程，对开发者暴露了协程的 API。
* 程序中的协程可以理解为一种轻量的线程。
* 一个线程中可以有多个协程。
* 协程通过**挂起和恢复**的能力，实现了非阻塞的效果。

最后推荐一下极客时间的《Kotlin 编程第一课》，上面的内容都是参考的这门课程。
<!-- ##{"timestamp":1648517700}## -->