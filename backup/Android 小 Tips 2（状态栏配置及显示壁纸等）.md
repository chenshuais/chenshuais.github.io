
# 设置状态栏

```java
//设置状态栏字体颜色为白色
getWindow().getDecorView().setSystemUiVisibility(
        View.SYSTEM_UI_FLAG_LAYOUT_STABLE |
                View.SYSTEM_UI_FLAG_LAYOUT_FULLSCREEN);
//设置状态栏颜色为透明
getWindow().setStatusBarColor(Color.TRANSPARENT);
```

```java
//设置状态栏字体颜色为黑色
window.getDecorView().setSystemUiVisibility(
                                View.SYSTEM_UI_FLAG_LAYOUT_STABLE |
                                View.SYSTEM_UI_FLAG_LAYOUT_FULLSCREEN |
                                View.SYSTEM_UI_FLAG_LIGHT_STATUS_BAR);
//设置导航键颜色也为透明
window.setNavigationBarColor(Color.TRANSPARENT);                                
```

在 style 中配置

```xml
<item name="android:navigationBarColor" tools:targetApi="21">@android:color/transparent</item>
<item name="android:enforceNavigationBarContrast" tools:targetApi="29">false</item>
<item name="android:statusBarColor"  tools:targetApi="21">@android:color/transparent</item>
<item name="android:enforceStatusBarContrast" tools:targetApi="29">false</item>

<item name="android:windowTranslucentStatus">true</item>
<item name="android:windowTranslucentNavigation">true</item>
```

# 显示壁纸

```xml
<!-- 显示壁纸 -->
<item name="android:windowShowWallpaper">true</item>
<item name="android:windowBackground">@android:color/transparent</item>
```
<!-- ##{"timestamp":1619595112}## -->