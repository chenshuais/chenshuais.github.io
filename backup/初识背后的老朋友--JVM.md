一直在学Java基础，光知道怎么用了也不行，也得了解一下它的内部长什么样子。接下来的一段时间，打算去探索一下JVM。本人对这块知识还是小白，如有不对的地方还望大佬们指正。

首先，我们先来缕一下JDK、JRE、JVM 之间的关系。下面看一下Java8的结构图：
`Gmeek-html<img src="https://blog-image.ian2018.cn/images/c8c72550b7e5f4a92543e65ae98d71ed3a34555b.png">`

通过这张图，我们可以得出以下关系：
`Gmeek-html<img src="https://blog-image.ian2018.cn/images/69327f171e8aae507776a41e43e6248aca6c657d.png">`

通过这个图可以知道，平时我们使用的API都是JVM在支持。

接下来让我们了解一下Java虚拟机产品的发展。
Sun Classic VM：Sun公司开发，第一款商用虚拟机，不过现在已经淘汰。
Exact VM：Sun公司开发，采用准确式内存管理，存活时间很短，后被HotSpot取代。
HotSpot VM：Sun公司开发，采用热点代码探测技术，它是Sun JDK和Open JDK中所带的虚拟机，也是目前使用范围最广的Java虚拟机，称霸武林。
KVM：Sun公司开发，简单、轻量、高度可移植，本来打算用于手机平台，后来Android平台出现了。

JRockit：由BEA公司开发，后被Oracle收购。号称是世界上最快的Java虚拟机，主要用于服务端。
J9：由IBM公司开发，类似于HotSpot，主要用于IBM自己的产品。

Azul VM：特定硬件平台专有的虚拟机，属于高性能虚拟机。
Liquid VM：也属于高性能虚拟机，它不需要操作系统的支持。

Dalvik VM：Android平台的虚拟机，执行.dex文件。
Microsoft JVM：由微软开发，只能运行在Windows平台上，但是是Windows平台上性能最好的虚拟机。后来被Sun公司指控，要求其撤下该产品。

好了，今天先了解这么多吧，下次我们再继续。

`Gmeek-html<img src="https://blog-image.ian2018.cn/images/dc233332bca947f4950e7975a2f597093ae33e88.png">`


<!-- ##{"timestamp":1531572121}## -->