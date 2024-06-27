> [!TIP]
> Firebase 自动收集事件说明：https://support.google.com/firebase/answer/9234069?hl=zh-Hans
>
> user-engagement 说明：https://support.google.com/analytics/answer/11109416#user-engagement

## 手机调试

### 1. 打开安卓的 debugView，这样就能实时的看打点的上传情况了。

https://firebase.google.com/docs/analytics/debugview

```bash
# 打开远程调试事件
adb shell setprop debug.firebase.analytics.app PACKAGE_NAME
# 打开 firebase log
adb shell setprop log.tag.FA VERBOSE
adb shell setprop log.tag.FA-SVC VERBOSE
adb logcat -v time -s FA FA-SVC
```

### 2. 操作手机，观察 log

示例 1

`Gmeek-html<img src="https://blog-image.ian2018.cn/images/01c5007ff157906237ec110255de98ba498c4965.png">`

`Gmeek-html<img src="https://blog-image.ian2018.cn/images/f701344eaf3e765d5cd4c76268ead20d73ea2a6f.png">`

示例 2

`Gmeek-html<img src="https://blog-image.ian2018.cn/images/7bd11b71912d109e6b04fb8df9fb3f515ca83b1e.png">`

`Gmeek-html<img src="https://blog-image.ian2018.cn/images/27c650cb1ad8fd5e6d6b3c584caa064597a4e38a.png">`

## 初步结论

通过上面观察，推测 screen_view 事件里的 engagement_time_msec 是表示上一个页面的停留时间。

engagement_time_msec = 上一个页面 paused 的时间 - 上一个页面 resumed 的时间。

根据线上打点分析，发现 bigquery 收集上来的数据和手机上调试 log 的数据有不一样的行为。

在线上收集到的数据中，screen_view 中的 engagement_time_msec 参数代表的就是这个页面的停留时间。携带时机为当该页面没有发送 user_engagement 事件时，会有这个参数，一般情况是连续跳转多个页面的时候。（这里猜测是为了弥补一下没有发 user_engagement 事件吧，可以通过这个参数知道这个页面的停留时间😂）

还有一些情况是连续发送了多个 user_engagement 事件，这里应该是当前页面没有发生变化，操作行为：打开应用 -> (home 出去 -> 从最近任务中进入应用 -> home 出去 -> 从最近任务中进入应用) 以此循环。

## 最终结论

- screen_view 中的 engagement_time_msec 参数代表的就是这个页面的停留时间。
- screen_view 携带 engagement_time_msec 时机为当该页面没有发送 user_engagement 事件时，会有这个参数，一般情况是连续跳转多个页面的时候。
- 连续发送多个 user_engagement 事件，是因为当前页面没有发生变化，但离开了页面，操作行为：打开应用 -> (home 出去 -> 从最近任务中进入应用 -> home 出去 -> 从最近任务中进入应用) 以此循环。
- 本地是会记录每个 screen_view 和 user_engagement 的，并且是成对出现的，但上传到 bigquery 会特殊处理一下。

<!-- ##{"timestamp":1658456652}## -->