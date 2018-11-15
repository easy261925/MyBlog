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

- 查看文件代码行数

进入到指定目录中执行 (.js即为查看所有js文件)

```
find . "(" -name "*.js"  ")" -print | xargs wc -l
```

- 关于 CocoaPods 配置

不知道哪一版本开始(当前0.57.4)，要在 `ios/Podfile` 里这样写了

```
# Uncomment the next line to define a global platform for your project
# platform :ios, '9.0'

target 'AppName' do
  # Uncomment the next line if you're using Swift or would like to use dynamic frameworks
  # use_frameworks!

  # Pods for AppName
 # 'node_modules'目录一般位于根目录中
  # 但是如果你的结构不同，那你就要根据实际路径修改下面的`:path`
  pod 'React', :path => '../node_modules/react-native', :subspecs => [
    'Core',
    'CxxBridge', # 如果RN版本 >= 0.47则加入此行
    'DevSupport', # 如果RN版本 >= 0.43，则需要加入此行才能开启开发者菜单
    'RCTText',
    'RCTNetwork',
    'RCTWebSocket', # 调试功能需要此模块
    'RCTAnimation', # FlatList和原生动画功能需要此模块
    # 在这里继续添加你所需要的其他RN模块
  ]
  # 如果你的RN版本 >= 0.42.0，则加入下面这行
  pod 'yoga', :path => '../node_modules/react-native/ReactCommon/yoga'

  # 如果RN版本 >= 0.45则加入下面三个第三方编译依赖
  pod 'DoubleConversion', :podspec => '../node_modules/react-native/third-party-podspecs/DoubleConversion.podspec'
  pod 'glog', :podspec => '../node_modules/react-native/third-party-podspecs/glog.podspec'
  pod 'Folly', :podspec => '../node_modules/react-native/third-party-podspecs/Folly.podspec'

end
```

```
  pod install
```
