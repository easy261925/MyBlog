---
title: react-native 踩坑及心得体会（持续更新）
date: 2018-11-07 17:16:08
tags:
---

# react-native 踩坑及心得体会

### 遇到啥写啥

- h.config not found ....

```
  cd node_modules/react-native/third-party/glog-0.3.5
  ./configure
  make
  make install
```

- android 键盘顶开底部导航 (createBottomTabNavigator)

修改 android/app/src/AndroidManifest `android:windowSoftInputMode` 的值为 `stateAlwaysHidden|adjustPan`

- 使用 `antd-mobile`

`npm i` 之后 `react-native run-ios` 会报关于 `react-dom` 的错误，删除 `node_modules` 并使用 `yarn` 即可解决

- vscode 快速打包 android APK 任务配置
```
{
  "version": "2.0.0",
  "tasks": [
    {
      "label": "build react-native android apk",
      "type": "shell",
      "command": "cd android/ && ./gradlew clean && ./gradlew assembleRelease && cd app/build/outputs/apk/release/ && open ."
    }
  ]
}
```

每次打包按下 `cmd + shift + p` + `Run Task` + `build react-native android apk` 一直回车即可
