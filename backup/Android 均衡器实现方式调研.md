# 方案：

## 1 使用系统 Media API 实现
开源项目代表：https://github.com/harjot-oberai/MusicDNA，2.7 K star，最近更新 2019.5.31

### 1.1 原理
使用系统 API 去设置各种参数。
- 重低音（BassBoost），强度范围 0 - 1000
- 环绕音（Virtualizer），强度范围 0 - 1000
- 均衡器（Equalizer）
- 混响（PresetReverb）
- 频谱（Visualizer），一般用于做动画

设置方式：
1. 结合 `MediaPlayer` 给指定的音频设置参数。
2. 不指定任何音频，直接给系统设置。

### 1.2 知识点
#### 均衡器每个频率的作用：
- 20Hz - 60Hz 部分
    
    这一段提升能给音乐强有力的感觉，给人很响的感觉，如雷声。是音乐中强劲有力的感觉。如果提升过高，则又会混浊不清，造成清晰度不佳，特别是低频响应差和低频过重的音响设备。
- 60Hz - 250Hz 部分

    这段是音乐的低频结构，它们包含了节奏部分的基础音，包括基音、节奏音的主音。它和高中音的比例构成了音色结构的平衡特性。提升这一段可使声音丰满，过度提升会发出隆隆声。衰减这两段会使声音单薄。
- 250Hz - 2KHz 部分

    这段包含了大多数乐器的低频谐波，如果提升过多会使声音像电话里的声音。如把600Hz和1kHz过度提升会使声音像喇叭的声音。如把3kHz提升过多会掩蔽说话的识别音，即口齿不清，并使唇音“mbv”难以分辨。如把1kHz和3kHz过分提升会使声音具有金属感。由于人耳对这一频段比较敏感，通常不调节这一段，过分提升这一段会使听觉疲劳。
- 2KHz - 4kHz 部分

    这段频率属中频，如果提升得过高会掩盖说话的识别音，尤其是3kHz提升过高，会引起听觉疲劳。
- 4kHz - 5KHz 部分

    这是具有临场感的频段，它影响语言和乐器等声音的清晰度。提升这一频段，使人感觉声源与听者的距离显得稍近了一些；衰减5kHz，就会使声音的距离感变远；如果在5kHz左右提出升6dB，则会使整个混合声音的声功率提升3dB。
- 6kHz - 16kHz 部分

    这一频段控制着音色的明亮度，宏亮度和清晰度。一般来说提升这几段使声音宏亮，但不清晰，不可能会引起齿音过重，衰减时声音变得清晰，但声音不宏亮。

> 一般使用系统 API 设置的只支持 60Hz  230Hz  910Hz  3.6KHz  14KHz 这几个频率，可以通过这个特征去判断竞品用的是系统 API 还是其他的。

#### 混响：
房间内产生的声音会向多个方向传播。听者首先听到来自声源本身的直达声。之后，会听到附近墙壁、天花板和地板反射的声音引起的离散回声。当声波经过越来越多的反射后到达时，单个反射变得无法区分，并且听者会听到随时间衰减的连续混响。混响对于模拟听众的环境至关重要。它可用于音乐应用程序以模拟在各种环境中播放的音乐，或在游戏中使听者沉浸在游戏环境中。
系统 API 提高了 6 种预设效果
- [PRESET_LARGEHALL](https://developer.android.com/reference/android/media/audiofx/PresetReverb#PRESET_LARGEHALL)：代表适合完整管弦乐队的大型大厅的混响预设
- [PRESET_LARGEROOM](https://developer.android.com/reference/android/media/audiofx/PresetReverb#PRESET_LARGEROOM)：代表适合现场表演的大房间的混响预设
- [PRESET_MEDIUMHALL](https://developer.android.com/reference/android/media/audiofx/PresetReverb#PRESET_MEDIUMHALL)：代表中型大厅的混响预设
- [PRESET_MEDIUMROOM](https://developer.android.com/reference/android/media/audiofx/PresetReverb#PRESET_MEDIUMROOM)：代表十米或更短的中等房间的混响预设
- [PRESET_PLATE](https://developer.android.com/reference/android/media/audiofx/PresetReverb#PRESET_PLATE)：代表传统板式混响合成的混响预设
- [PRESET_SMALLROOM](https://developer.android.com/reference/android/media/audiofx/PresetReverb#PRESET_SMALLROOM)：代表一个不到 5 米长的小房间的混响预设

## 2 VLC 
一个开源的媒体播放器：https://www.videolan.org/

下载地址：https://play.google.com/store/apps/details?id=org.videolan.vlc

Android 端项目地址：https://github.com/videolan/vlc-android 1.3 K star，最近更新 2021.11.9

发现：
- VLC 只对视频支持了均衡器设置，音频没用做。
- 它没用使用系统 API，使用的是自己编写的 native 方法。详见 `EqualizerFragment`，项目依赖了  `org.videolan.android:libvlc-all`

## 3 FFmpeg
使用起来比较复杂，具体可以看下面的文章

https://www.jianshu.com/p/0ad6e9526487

https://www.jianshu.com/p/d7e8a9139350

https://trac.ffmpeg.org/wiki/Projects

# 结论
使用系统 API 的方式比较合适，因为这种方案既可以满足基本需求，使用起来又很简单，也可以结合 ExoPlayer 使用。
<!-- ##{"timestamp":1636439357}## -->