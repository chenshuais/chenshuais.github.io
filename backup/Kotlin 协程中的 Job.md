`Gmeek-html<img src="https://blog-image.ian2018.cn/images/86ff272f7ad726ad95ce92b6d9bb8137b55e2ea1.png">`

通过之前的分享，我们已经知道了 Kotlin 的协程是什么，也知道了挂起函数。今天就一块来学习一下什么是 **Job**。

## Job 从哪来

平时我们使用协程，大部分都是直接 `launch` 一下就完了，很少的时候会用一下 `async`。

```kotlin
launch {
}
async { 
}
```

如果用过 `async` 的话，会发现 IDE 会有黄色的提醒，告诉你返回值 **Deferred** 没有使用。
`Gmeek-html<img src="https://blog-image.ian2018.cn/images/cce0c7273f0b100ff9f5957cd51b7f5ae9386dc8.png">`

这个 **Deferred** 其实就是 **Job**，它是一个继承 **Job** 的接口。
`Gmeek-html<img src="https://blog-image.ian2018.cn/images/0f510aed293051cab6c231e17e9073fc6c46552c.png">`

其实不止 `async` 有返回值，`launch` 也是有返回值的，而 `launch` 的返回值直接就是一个 **Job**。
`Gmeek-html<img src="https://blog-image.ian2018.cn/images/4cff61b074c930a018498356a653c3262c27f8f1.png">`

我们再来看一下 **Job** 的源码的定义（这里我删除了一些注释和内部 API）：

```kotlin
public interface Job : CoroutineContext.Element {
    
    // ------------ state query ------------

    public val isActive: Boolean

    public val isCompleted: Boolean

    public val isCancelled: Boolean

    // ------------ state update ------------

    public fun start(): Boolean

    public fun cancel(cause: CancellationException? = null)

    // ------------ parent-child ------------

    public val children: Sequence<Job>

    // ------------ state waiting ------------

    public suspend fun join()

    // ------------ low-level state-notification ------------

    public fun invokeOnCompletion(handler: CompletionHandler): DisposableHandle
    
}
```
**Job** 的源码注释还是写的很清楚的，还贴心的做了划分。

从源码的注释我们可以大概知道，**Job** 大致分了五个部分：
* 状态的查询
* 状态的更新
* 父子关系
* 状态的等待
* 状态监听

这个状态其实就是协程的状态。

到这里我们已经知道了 **Job** 从哪来的，那么这个 **Job** 到底有什么用呢？接下来我们用几个例子来试一下。

## Job 有什么用

`Gmeek-html<img src="https://img.soogif.com/fCnkbyO30VyAxkMkMQMGzwWadCVCafE7.gif?scope=mdnice">`
在写例子之前，先做一下前置准备：

1. 给 IDEA 设置一下 VM 参数：`-Dkotlinx.coroutines.debug`，这样打印出来的线程名字中就包含了协程的信息。

`Gmeek-html<img src="https://blog-image.ian2018.cn/images/3a1ff93948b2260c5a0476ec4108ee40a7a22024.png">`


2. log 打印方法：
```kotlin
/**
 * 打印 Job 的状态信息，就是源码中看到的 state query 那部分内容
 */
fun Job.log() {
    logX(
        """
        isActive = $isActive
        isCancelled = $isCancelled
        isCompleted = $isCompleted
    """.trimIndent()
    )
}

/**
 * 控制台输出带协程信息的 log
 */
fun logX(any: Any?) {
    println(
        """
================================
$any
Thread:${Thread.currentThread().name}
================================""".trimIndent()
    )
}
```

### 一、测试通过 Job 查看协程状态

```kotlin
fun test1() = runBlocking {
    val job = launch {
        delay(1000L)
    }
    job.log()             // ①
    delay(1500L)          // ②
    job.log()             // ③
}
```
打印结果：
```
================================
isActive = true
isCancelled = false
isCompleted = false
Thread:main @coroutine#2
================================
================================
isActive = false
isCancelled = false
isCompleted = true
Thread:main @coroutine#2
================================
```
上面代码中我们创建了一个协程，在里面做了一个延迟 1 秒钟的任务，然后马上通过 **Job log** 查看了一下当前协程的状态，发现 **isActive** 是 true，其他都是 false，说明当前协程属于活跃状态。

然后等了 1.5 秒后，再次通过 **Job log** 查看协程的状态，发现 **isActive** 变成了 false，并且 **isCompleted** 变成了 true。

通过这些我们大概就能知道 **isActive** 和 **isCompleted** 的状态啥时候会改变了吧，运行协程时 **isActive** 就是 true，等协程运行完成了，**isCompleted** 就变成了 true。

### 二、测试通过 Job 更新协程状态

#### 1. **Job** 的 `start` 方法

要想使用 `start`，我们首先得改一下 `launch` 的启动方式：
```kotlin
fun test2() = runBlocking {
    //                           变化在这里
    //                               ↓
    val job = launch(start = CoroutineStart.LAZY) {
        logX("Coroutine start!")
        delay(1000L)
    }
    delay(1000L)          // ①
    job.log()
    job.start()           // ②
    job.log()
    delay(1500L)          // ③
    job.log()
}
```
打印结果：
```
================================
isActive = false
isCancelled = false
isCompleted = false
Thread:main @coroutine#2
================================
================================
isActive = true
isCancelled = false
isCompleted = false
Thread:main @coroutine#2
================================
================================
Coroutine start!
Thread:main @coroutine#3
================================
================================
isActive = false
isCancelled = false
isCompleted = true
Thread:main @coroutine#2
================================
```
上面代码我们在创建协程的时候将 start 参数设置成了 **LAZY**，这样创建协程时就不会启动了。

然后延迟了 1 秒钟，查看了一下协程的状态，会发现 **isActive** 是 false，**isCompleted** 也是 false。

当我们调用了 Job 的 `start` 方法后，再次查看状态，**isActive** 变成了 true，并且协程中的任务也打印了 “Coroutine start!”，说明调了 `start` 方法之后会把协程激活。

又等了 1.5 秒后，协程也正常结束了。

#### 2. **Job** 的 `cancel` 方法：

```kotlin
fun test4() = runBlocking {
    val job = launch(start = CoroutineStart.LAZY) {
        logX("Coroutine start!")
        delay(1000L)
        logX("Coroutine end!")
    }
    delay(500L)
    job.log()
    job.start()
    job.log()
    delay(500L)           // ①
    job.cancel()          // ②
    job.log()             // ③
    delay(2000L)          // ④
    job.log()             // ⑤
}
```
打印结果：
```
================================
isActive = false
isCancelled = false
isCompleted = false
Thread:main @coroutine#2
================================
================================
isActive = true
isCancelled = false
isCompleted = false
Thread:main @coroutine#2
================================
================================
Coroutine start!
Thread:main @coroutine#3
================================
================================
isActive = false
isCancelled = true
isCompleted = false
Thread:main @coroutine#2
================================
================================
isActive = false
isCancelled = true
isCompleted = true
Thread:main @coroutine#2
================================
```
我们还是创建了一个懒加载的协程，启动它 500 毫秒之后，调用了 **Job** 的 `cancel` 方法，这时去查看协程的状态，会发现 **isActive** 变成了 false，**isCancelled** 变成了 true。

等又过了 2 秒，我们再次查看协程的状态，发现 **isCompleted** 也变成了 true，并且最后协程任务中的 “Coroutine end!” 也没有打印出来。说明 `cancel` 方法会把协程任务取消掉。

到这里我们是不是发现 **Job** 也有个生命周期，没错，Job 的源码注释中就已经说明了：

`Gmeek-html<img src="https://blog-image.ian2018.cn/images/02d149556182e6f048337afa6517fb40ab000c88.png">`

`Gmeek-html<img src="https://blog-image.ian2018.cn/images/617ccf573494e8efa04ebfd5d14927932b39cf00.png">`

从上面我们就能清晰的看到整个 **Job** 的生命周期。

但是有个奇怪的点，上面代码执行结果出现了 **isCancelled** 和 **isCompleted** 同时都为 true 的情况，这在图中的生命周期是没有体现的。

这是因为，协程认为由于某种原因取消的协程，也仍然是一种“结束状态”。所以流程图当中的 New、Active、Completing、Cancelling、Completed、Cancelled 这些状态，其实都是 **Job** 内部私有的状态，而 **Job** 对外暴露出的 **isCompleted** 并不是与其一一对应的。

Android 中的 `lifecycleScope` 就是在 onDestroy 的时候，通过 **Job** 将协程 cancel 掉了，这才有了和 Activity/Fragment 生命周期绑定的 scope，所以大家创建协程的时候，尽量都用这个 scope，不要用全局的，除非这个任务就是需要全局运行的。

### 三、测试通过 Job 等待协程运行

#### 1. Job 的 join

前面几个例子我们是都知道协程中的任务执行了多久，然后去查看协程的状态。但实际开发中我们一般都是不能提前知道的，就比如进行网络请求，这时候我们就可以用 **Job** 的 `join` 方法了。

```kotlin
fun test5() = runBlocking {
    suspend fun download() {
        // 模拟下载任务
        val time = (Random.nextDouble() * 1000).toLong()
        logX("Delay time: = $time")
        delay(time)
    }

    val job = launch {
        logX("Coroutine start!")
        download()
        logX("Coroutine end!")
    }
    job.log()
    job.join()      // 等待协程执行完毕
    job.log()
}
```
打印结果：
```
================================
isActive = true
isCancelled = false
isCompleted = false
Thread:main @coroutine#2
================================
================================
Coroutine start!
Thread:main @coroutine#3
================================
================================
Delay time: = 456
Thread:main @coroutine#3
================================
================================
Coroutine end!
Thread:main @coroutine#3
================================
================================
isActive = false
isCancelled = false
isCompleted = true
Thread:main @coroutine#2
================================
```
`join` 是个挂起函数，所以执行到 join 那里会把代码挂起，等协程运行完成后，后面的代码才会继续执行。

另外我们还可以通过给 **Job** 添加一个通知回调的方式，来知道协程运行完成，这可以使用 **Job** 的 `invokeOnCompletion`：

```kotlin
fun test6() = runBlocking {
    suspend fun download() {
        // 模拟下载任务
        val time = (Random.nextDouble() * 1000).toLong()
        logX("Delay time: = $time")
        delay(time)
    }

    val job = launch {
        logX("Coroutine start!")
        download()
        logX("Coroutine end!")
    }
    job.log()
    job.invokeOnCompletion {
        job.log()   // 协程结束以后就会调用这里的代码
    }
    job.join()      // 等待协程执行完毕
}
```
打印结果：
```
================================
isActive = true
isCancelled = false
isCompleted = false
Thread:main @coroutine#2
================================
================================
Coroutine start!
Thread:main @coroutine#3
================================
================================
Delay time: = 170
Thread:main @coroutine#3
================================
================================
Coroutine end!
Thread:main @coroutine#3
================================
================================
isActive = false
isCancelled = false
isCompleted = true
Thread:main @coroutine#3
================================
```
可以看到效果是一样的，这样我们就能在代码其他地方来监听协程的运行了。

但是需要注意的是，协程被 `cancel` 了也同样会回调：
```kotlin
fun test6() = runBlocking {
    suspend fun download() {
        // 模拟下载任务
        val time = (Random.nextDouble() * 1000).toLong()
        logX("Delay time: = $time")
        delay(time)
    }

    val job = launch {
        logX("Coroutine start!")
        download()
        logX("Coroutine end!")
    }
    job.log()
    job.invokeOnCompletion {
        job.log()   // 协程结束以后就会调用这里的代码
    }
    job.cancel()    // 取消协程
}
```
打印结果：
```
================================
isActive = true
isCancelled = false
isCompleted = false
Thread:main @coroutine#2
================================
================================
isActive = false
isCancelled = true
isCompleted = true
Thread:main @coroutine#3
================================
```

#### 2. Deferred 的 await

**Deferred** 继承了 **Job** 但只比 **Job** 多了一个 `await` 方法：
`Gmeek-html<img src="https://blog-image.ian2018.cn/images/21f6a4af8d7d547f837157d606f3a0a33d2d4c15.png">`

和 `join` 类似，`await` 也是可以等待协程运行完成，只不过 `await` 是有返回值的，可以拿到协程的运行结果：
```kotlin
fun test7() = runBlocking {
    suspend fun download(): String {
        // 模拟下载任务
        val time = (Random.nextDouble() * 1000).toLong()
        logX("Delay time: = $time")
        delay(time)
        return "download result!"
    }
    
    val deferred = async {
        logX("Coroutine start!")
        val result = download()
        logX("Coroutine end!")
        return@async result
    }
    val result = deferred.await()
    println("Result = $result")
    logX("Process end!")
}
```
打印结果：
```
================================
Coroutine start!
Thread:main @coroutine#3
================================
================================
Delay time: = 263
Thread:main @coroutine#3
================================
================================
Coroutine end!
Thread:main @coroutine#3
================================
Result = download result!
================================
Process end!
Thread:main @coroutine#2
================================
```

`await` 也是一个挂起函数，通过 `await` 我们拿到了 `async` 协程的运行结果。这里可以再回忆一下挂起函数的运行模型：
`Gmeek-html<img src="https://blog-image.ian2018.cn/images/845ae9cde2593556965e11f079cdf3146bb1cea2.gif">`

通过上面几个例子我们应该已经知道了 **Job** 有什么用了吧，其实 **Job** 就是用来控制协程的，也就是 **Job** 是协程的句柄。这里可以类比遥控器和空调的关系：
* 空调遥控器可以监测空调的运行状态；Job 也可以监测协程的运行状态；
* 空调遥控器可以操控空调的运行状态，Job 也可以简单操控协程的运行状态。

那么现在 **Job** 还剩最后一块「parent-child」，下面我们来看看这个父子关系到底是什么？

## 结构化并发

**Job** 中的 `children` 其实是和结构化并发有关的，这也是协程的另一大特色。那么什么是结构化并发呢？从字面上看就是有结构的并发（废话文学了🙄），还是看几个例子吧：
```kotlin
fun test8() = runBlocking {
    val parentJob: Job
    var job1: Job? = null
    var job2: Job? = null
    var job3: Job? = null
    parentJob = launch {
        job1 = launch {
            delay(1000L)
        }
        job2 = launch {
            delay(3000L)
        }
        job3 = launch {
            delay(5000L)
        }
    }
    delay(500L)
    parentJob.children.forEachIndexed { index, job ->
        when (index) {
            0 -> println("job1 === job is ${job1 === job}")
            1 -> println("job2 === job is ${job2 === job}")
            2 -> println("job3 === job is ${job3 === job}")
        }
    }
    parentJob.join() // 这里会挂起大约5秒钟
    logX("Process end!")
}
```
打印结果：
```
job1 === job is true
job2 === job is true
job3 === job is true
================================
Process end!
Thread:main @coroutine#2
================================
```
上面代码中我们在一个 launch 块中又创建了 3 个 launch，得到了 parentJob 和 job 1-3，然后比较了 parentJob 的 children 和 job 1-3 是否相等。从结果上看，它们就是同一个对象，也就是 launch 创建的协程是有父子关系的，它们结构大概是下面这样：
`Gmeek-html<img src="https://blog-image.ian2018.cn/images/18b29a89ce5307f9a9e28ddf5a262e5c2e038175.png">`

还有 parentJob 执行 join 的时候大概挂起了 5 秒，而子协程中最长的任务就是 5 秒，这说明父协程会等所有子协程全部完成后，才会结束：
`Gmeek-html<img src="https://blog-image.ian2018.cn/images/08a9b255f24248d65e4e90828fc1ffaebc492437.gif">`



然后我们再试试 cancel 的情况：
```kotlin
fun test9() = runBlocking {
    val parentJob: Job
    var job1: Job? = null
    var job2: Job? = null
    var job3: Job? = null
    parentJob = launch {
        job1 = launch {
            logX("Job1 start!")
            delay(1000L)
            logX("Job1 done!") // ①，不会执行
        }
        job2 = launch {
            logX("Job2 start!")
            delay(3000L)
            logX("Job2 done!") // ②，不会执行
        }
        job3 = launch {
            logX("Job3 start!")
            delay(5000L)
            logX("Job3 done!")// ③，不会执行
        }
    }
    delay(500L)
    parentJob.children.forEachIndexed { index, job ->
        when (index) {
            0 -> println("job1 === job is ${job1 === job}")
            1 -> println("job2 === job is ${job2 === job}")
            2 -> println("job3 === job is ${job3 === job}")
        }
    }
    parentJob.cancel() // 变化在这里
    logX("Process end!")
}
```
打印结果：
```
================================
Job1 start!
Thread:main @coroutine#4
================================
================================
Job2 start!
Thread:main @coroutine#5
================================
================================
Job3 start!
Thread:main @coroutine#6
================================
job1 === job is true
job2 === job is true
job3 === job is true
================================
Process end!
Thread:main @coroutine#2
================================
```
上面代码我们调用了 parentJob 的 `cancel`，会发现直到整个程序结束了 job 1-3 也不会执行结束。说明父协程取消了，连带所有的子协程也会跟着取消：
`Gmeek-html<img src="https://blog-image.ian2018.cn/images/be55bf136e50c207113f77878f5e10f81db62c3d.gif">`

到这差不多能理解什么是结构化并发了吧，就是带有结构和层级的并发。接下来我们举几个比较实用的例子。

## 实际应用

需求大概是这样的：一个照片详情页，有三个接口，分别是获取照片详情信息、获取照片点赞列表、获取照片评论列表，这三个接口之前没有耦合，只有三个接口全都获取到信息才能进行 UI 展示。

我们先看一下一般写法：
```kotlin
fun test10() = runBlocking {
    suspend fun getPhotoInfo(): String {
        delay(1000L) // 模拟耗时操作
        return "photo info"
    }
    suspend fun getPhotoLikeList(): String {
        delay(1000L) // 模拟耗时操作
        return "photo like list"
    }
    suspend fun getPhotoCommentList(): String {
        delay(1000L) // 模拟耗时操作
        return "photo comment list"
    }

    val deferred = async {
        val results = mutableListOf<String>()
        // measureTimeMillis 是 kotlin 里一个统计执行时间的方法
        val time = measureTimeMillis {
            results.add(getPhotoInfo())
            results.add(getPhotoLikeList())
            results.add(getPhotoCommentList())
        }
        println("Cost Time: $time")
        results
    }
    
    println("开始展示 UI: ${deferred.await()}")
}
```
打印结果：
```
Cost Time: 3015
开始展示 UI: [photo info, photo like list, photo comment list]
```
我们写了 3 个挂起函数模拟 3 个接口的请求，然后直接在协程里就去分别请求了，大概消耗 3 秒才结束。

然后我们看一下优化后的：
```kotlin
fun test11() = runBlocking {
    suspend fun getPhotoInfo(): String {
        delay(1000L) // 模拟耗时操作
        return "photo info"
    }
    suspend fun getPhotoLikeList(): String {
        delay(1000L) // 模拟耗时操作
        return "photo like list"
    }
    suspend fun getPhotoCommentList(): String {
        delay(1000L) // 模拟耗时操作
        return "photo comment list"
    }

    val deferred = async {
        val results: List<String>
        // measureTimeMillis 是 kotlin 里一个统计执行时间的方法
        val time = measureTimeMillis {
            // 里面也用 async 并行请求
            val result1 = async { getPhotoInfo() } 
            val result2 = async { getPhotoLikeList() }
            val result3 = async { getPhotoCommentList() }
            results = listOf(result1.await(), result2.await(), result3.await())
        }
        println("Cost Time: $time")
        results
    }
    
    println("开始展示 UI: ${deferred.await()}")
}
```
打印结果：
```
Cost Time: 1022
开始展示 UI: [photo info, photo like list, photo comment list]
```
可以看到消耗的时间变成 1 秒，大大提升了运行效率。

所有当异步任务之间没有互相依赖时，可以通过这样的结构化并发来优化代码运行效率。实际上，`async` 最常见的使用场景就是与挂起函数结合，优化并发。

## 结束了

到这我们应该已经清楚了 **Job** 是什么，能干什么用了。主要就是「**监控控制协程运行**」和处理「**结构化并发**」。

最后说一下 cancel 的一个坑，看下面代码猜运行结果：
```
fun test12() = runBlocking {

    suspend fun fetchAll() {
        withContext(Dispatchers.IO) {
            var i = 0
            while (true) {
                i++
                logX("fetch i: $i")
            }
        }
    }

    val job = launch {
        logX("coroutine start!")
        fetchAll()
        logX("coroutine end!")
    }

    delay(500L)

    job.cancel()

    logX("Process end!")
}
```

这段代码其实是不会结束的，因为 fetchAll 里没有处理协程 cancel 的情况，所有会一直运行。因为 cancel 只是改变了状态，取消的话还需要协程内部去配合。

之前那些例子能取消，是因为那里面用了 `delay` 这个方法，是 `delay` 方法里处理了 cancel，并抛出了一个取消的异常，所以协程才能结束。

解决办法就是在协程里加上当前状态的判断：
```kotlin
fun test12() = runBlocking {

    suspend fun fetchAll() {
        withContext(Dispatchers.IO) {
            var i = 0
            while (isActive) { // 变化在这里
                i++
                logX("fetch i: $i")
            }
        }
    }

    val job = launch {
        logX("coroutine start!")
        fetchAll()
        logX("coroutine end!")
    }

    delay(500L)

    job.cancel()

    logX("Process end!")
}
```
这样我们自己定义的挂起函数也能响应 cancel 了。

---
最最后再推荐一下极客时间的《Kotlin 编程第一课》，上面的内容都是参考的这门课程。


<!-- ##{"timestamp":1689731820}## -->