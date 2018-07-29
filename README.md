# ThemeTest
[UI与交互]沉浸式App启动页与Activity切换动画
----
看到饿了么的沉浸式启动Splash页,比全屏模式启动后进入Main,沉浸栏再出现过渡效果好很多.

具体实现代码如下:
values目录
```xml
<style name="App.Theme.Splash" parent="@style/Theme.AppCompat.Light.NoActionBar">
    <item name="android:windowBackground">@drawable/bg_splash</item>
    <item name="android:windowContentOverlay">@null</item>
    <item name="android:windowAnimationStyle">@style/Animation.Activity.Splash</item>
</style>
```

新建values-v19目录
```xml
<style name="App.Theme.Splash" parent="@style/Theme.AppCompat.Light.NoActionBar">
    <item name="android:windowBackground">@drawable/bg_splash</item>
    <item name="android:fitsSystemWindows">true</item>
    <item name="android:windowContentOverlay">@null</item>
    <item name="android:windowTranslucentStatus">true</item>
    <item name="android:windowAnimationStyle">@style/Animation.Activity.Splash</item>
</style>
```

新建values-v21目录
```xml
<style name="App.Theme.Splash" parent="@style/Theme.AppCompat.Light.NoActionBar">
    <item name="android:windowBackground">@drawable/bg_splash</item>
    <item name="android:fitsSystemWindows">true</item>
    <item name="android:windowContentOverlay">@null</item>
    <item name="android:windowDrawsSystemBarBackgrounds">true</item>
    <item name="android:statusBarColor">@android:color/transparent</item>
    <item name="android:windowAnimationStyle">@style/Animation.Activity.Splash</item>
</style>
```
---

然后就是Activity切换动画主要两种形式

1.布局设置
```xml
<style name="Animation.Activity.Common" parent="@android:style/Animation">
    <item name="android:activityOpenEnterAnimation">@anim/anim_right_in</item>
    <item name="android:activityOpenExitAnimation">@anim/anim_stay</item>
    <item name="android:activityCloseEnterAnimation">@anim/slide_left_in</item>
    <item name="android:activityCloseExitAnimation">@anim/anim_right_out</item>
    
    <item name="android:taskOpenEnterAnimation">@anim/anim_right_in</item>
    <item name="android:taskOpenExitAnimation">@anim/anim_stay</item>
    <item name="android:taskCloseEnterAnimation">@anim/slide_left_in</item>
    <item name="android:taskCloseExitAnimation">@anim/anim_right_out</item>
</style>
```
如果启动的activity运行在原来的task中，
那么使用animation activityOpenEnterAnimation/activityOpenExitAnimation;

如果结束的activity结束之后原来的task还存在，
那么使用activityCloseEnterAnimation/activityCloseExitAnimatio;

如果启动的activity运行在新的task中，
那么使用animation taskOpenEnterAnimation/taskOpenExitAnimation;

如果结束的activity结束之后原来的task将不存在，也即此activity为task最后的activity，
那么使用taskCloseEnterAnimation/taskCloseExitAnimation;

|属性| 效果|
| -----   | -----  | 
|activityOpenEnterAnimation |此Activity被打开时的入场动画|
|activityOpenExitAnimation  |此Activity打开其它Activity的退场动画|
|activityCloseEnterAnimation|被打开Activity关闭时,此Activity重新进入用户视野的入场动画|
|activityCloseExitAnimation |被打开Activity关闭时的退场动画|

经测试该形式的动画切换(具体看代码中Splash启动Main,再由Main关闭回到Splash)

总结如下(对不同场景下Activity动画切换更改有帮助):

此Activity被打开时的入场动画由该Activity决定
此Activity所打开的Activity的关闭动画也由该Activity决定
