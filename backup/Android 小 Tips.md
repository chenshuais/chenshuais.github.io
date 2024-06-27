# Android 对非 SDK 调用限制

从 Android 9 ( API 28 ) 开始，对非 SDK 接口限制

https://juejin.cn/post/6844903903037751309

https://developer.android.com/distribute/best-practices/develop/restrictions-non-sdk-interfaces?hl=zh-cn

* greylist 无限制，可以正常使用。

* blacklist 无论什么版本的手机系统，使用这些api，系统将会抛出错误。

* greylist-max-o 受限制的灰名单。APP运行在 版本<=8.0的系统里 可以正常访问，targetSDK>8.0且运行在>8.0的手机会抛出异常。

* greylist-max-p 受限制的灰名单。APP运行在 版本<=9.0的系统里 可以正常访问，targetSDK>9.0且运行在>9.0的手机会抛出异常。


# Android 11 的包可见性

在 Android 11 中，使用 intent 匹配时，需要在清单文件中添加 <queries> 标签，否则 queryIntentActivities() 方法将返回空列表。

https://developer.android.com/training/basics/intents/package-visibility-use-cases?hl=zh-cn#grant-uri-access

分享等操作，推荐使用系统的 Sharesheet 

```java
Intent intent = Intent.createChooser(sendIntent, null);
```

https://developer.android.com/training/sharing/send?hl=zh-cn#why-to-use-system-sharesheet


<!-- ##{"timestamp":1616653252}## -->