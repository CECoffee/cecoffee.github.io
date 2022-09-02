---
layout: post
title: "Android TileService的使用"
subtitle: "实现Android 7.0+的QuickSettings"
date: 2022-09-02
categories: Android
tags:
  - Android
  - Kotlin
author: Moxi
---

## 1. 前言

> 最近换上了 Clash For Magisk，虽然说有了 DashBoard，但还是感觉不如 Class For Android 来的方便，尤其是控制中心的一键启动和关闭，所以就想着自己写一个，于是就有了这篇文章。

# 2. TileService 引入

> Android 7.0+ Sdk 24+

从字面上可以看出 `TileService` 本质上是一个 `Service`

所以我们需要在 AndroidManifest.xml 中注册

```xml
<service
    android:name=".QuickSettingsService"
    android:icon="@drawable/ic_logo_service"
    android:label="@string/tile_label_run"
    android:permission="android.permission.BIND_QUICK_SETTINGS_TILE"
    android:exported="true">
    <intent-filter>
        <action android:name="android.service.quicksettings.action.QS_TILE" />
    </intent-filter>
</service>
```

- `android:name=".QuickSettingsService"`: 指定 TileService 的类名
- `android:icon="@drawable/ic_logo_service"`: 指定显示的的图标 最好是透明
- `android:label="@string/tile_label_run"`: 指定显示的标题(未激活状态)
- `android:permission="android.permission.BIND_QUICK_SETTINGS_TILE"`: 指定权限
- `android:exported="true"`: 指定是否导出(未知用途)
- `action android:name="android.service.quicksettings.action.QS_TILE"`: 指定 Action

实现长按打开对应Activity
```xml
.MainActivity
<intent-filter>
    <action 
    android:name="android.service.quicksettings.action.QS_TILE_PREFERENCES" />
</intent-filter>
```

# 3. TileService 实现

```kotlin
// QuickSettingsService.kt
class QuickSettingsService : TileService() {  // 继承 TileService
    override fun onClick() { // 重载onClick方法 实现监听
        super.onClick()
        // 当点击时，执行的代码
    }
}
```

如果我们要实现获取当前选中状态
```kotlin
override fun onClick() { 
    qsTile.state = Tile.STATE_ACTIVE // 激活状态
    qsTile.state = Tile.STATE_INACTIVE // 关闭状态
    // Tips: 可以通过 `qsTile.state` 获取/设置当前状态
    // 系统会自动对图标进行颜色渲染
}
```

如果我们要实现更换Icon
```kotlin
qsTile.icon = Icon.createWithResource(this, R.drawable.ic_logo_service) 
// 更换图标
```

如果我们要实现更换标题
```kotlin
qsTile.label = getString(R.string.tile_label_run)
// 更换标题
```
最后别忘了
```kotlin
qsTile.updateTile()
```

[Dashboard-2](https://github.com/moxidev/DashBoard-2)
##  参考
[TileService-Android](https://developer.android.com/reference/android/service/quicksettings/TileService)
