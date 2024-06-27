FCM 有[两种消息类型](https://firebase.google.com/docs/cloud-messaging/concept-options#notifications_and_data_messages)：

- **通知消息****：**又叫显示消息，由 FCM sdk 处理。
- **数据消息：**由客户端处理。

## Android 上接收消息

https://firebase.google.com/docs/cloud-messaging/android/receive

`Gmeek-html<img src="https://blog-image.ian2018.cn/images/f9fbe68f4aa7dfa5d1b1d2fb383575f57df07d8f.png">`


### 对于通知消息：

- **当应用处于前台时**：不会弹出系统通知，交由应用的 `FirebaseMessagingService` 中 `onMessageReceived` 回调处理。
- **当应用处于后台时**：直接弹出系统通知。

### 对于数据消息：

- **当应用处于前台时**：交由应用的 `FirebaseMessagingService` 中 `onMessageReceived` 回调处理。
- **当应用处于后台时**：交由应用的 `FirebaseMessagingService` 中 `onMessageReceived` 回调处理。

### 对于既有通知又有数据的消息：

- **当应用处于前台时**：不会弹出系统通知，交由应用的 `FirebaseMessagingService` 中 `onMessageReceived` 回调处理。
- **当应用处于后台时**：直接弹出系统通知，数据会在 `Intent` 的 `extras` 中，可以在主 Activity 的 onCreate/onNewIntent 中获取。

### onMessageReceived 处理限制

需要在 20 秒内完成处理。

### 消息生命周期

https://firebase.blog/posts/2019/02/life-of-a-message

https://stackoverflow.com/questions/47398812/is-it-possible-to-receive-fcm-push-notifications-when-app-is-killed

以下情况会导致消息无法收到：

- 消息过期：最长 28 天
- 如果消息设置为可折叠，会被新的消息替代
- 设备超过一个月未上线
- 设备处于休眠模式
- **应用在省电策略中在受限状态**
- **应用在设置页点击了强行停止（可能还有某些手机厂商对上滑这种操作也执行了强行停止，那么在这种手机上就不会收到推送了）**
- **Android 13 申请通知权限被拒绝（低于 13 默认是有通知权限的，13 上需要应用主动申请）**
- **应用被卸载**

**本地测试现象：如果手机有谷歌服务，****将应用上滑杀死****，也能收到推送，包括****通知消息****和数据消息。**

### 发送的通知样式

https://firebase.google.com/docs/cloud-messaging/send-message

#### 修改通知左上角的小图标

`Gmeek-html<img src="https://blog-image.ian2018.cn/images/545c898db8b1b81d15046a1eb2599d7f8129b3f1.png">`


#### 修改通知右边的大图标

`Gmeek-html<img src="https://blog-image.ian2018.cn/images/232213bb635c597776cf3db718e88655ed83d76d.png">`


#### 附 Android 通知元素说明

`Gmeek-html<img src="https://blog-image.ian2018.cn/images/3bd667a0715452f7e413e43b95b69122428027cd.png">`


## iOS 上接收消息

可以通过 APNs 渠道发送，消息接收更可靠。

<!-- ##{"timestamp":1689909684}## -->