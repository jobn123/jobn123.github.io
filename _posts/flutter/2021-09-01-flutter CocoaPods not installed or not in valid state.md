---
layout:     post
title:      "flutter CocoaPods not installed or not in valid state"
subtitle:   " 🏷 "
date:       2021-09-01 12:00:00
author:     "Hiz"
header-img: "img/flutter.jpg"
catalog: true
tags:
    - Flutter
---

# 前言

今天拉取下代码之后，在ios模拟器上死活运行不起来。然后报了一个`
CocoaPods not installed or not in valid state`的错误。

# 尝试解决

其实我电脑上是安装了`CocoaPods`了的。 运行`flutter doctor -v` 之后也是显示一切正常。这就使我更纳闷了。

```shell
 ✘ eleven@elevendeMacBook-Pro  ~  flutter doctor -v
[✓] Flutter (Channel stable, 2.2.3, on macOS 11.4 20F71 darwin-x64, locale
    zh-Hans-CN)
    • Flutter version 2.2.3 at /Users/eleven/flutter-2.0.3
    • Framework revision f4abaa0735 (9 weeks ago), 2021-07-01 12:46:11 -0700
    • Engine revision 241c87ad80
    • Dart version 2.13.4
    • Pub download mirror https://pub.flutter-io.cn
    • Flutter download mirror https://storage.flutter-io.cn

[✓] Android toolchain - develop for Android devices (Android SDK version 30.0.3)
    • Android SDK at /Users/eleven/Library/Android/sdk
    • Platform android-30, build-tools 30.0.3
    • Java binary at: /Applications/Android
      Studio.app/Contents/jre/jdk/Contents/Home/bin/java
    • Java version OpenJDK Runtime Environment (build
      1.8.0_242-release-1644-b3-6915495)
    • All Android licenses accepted.

[✓] Xcode - develop for iOS and macOS
    • Xcode at /Applications/Xcode.app/Contents/Developer
    • Xcode 12.5.1, Build version 12E507
    • CocoaPods version 1.10.2

[✓] Chrome - develop for the web
    • Chrome at /Applications/Google Chrome.app/Contents/MacOS/Google Chrome

[✓] Android Studio (version 4.1)
    • Android Studio at /Applications/Android Studio.app/Contents
    • Flutter plugin can be installed from:
      🔨 https://plugins.jetbrains.com/plugin/9212-flutter
    • Dart plugin can be installed from:
      🔨 https://plugins.jetbrains.com/plugin/6351-dart
    • Java version OpenJDK Runtime Environment (build
      1.8.0_242-release-1644-b3-6915495)

[✓] VS Code (version 1.58.2)
    • VS Code at /Applications/Visual Studio Code.app/Contents
    • Flutter extension version 3.25.0

[✓] Connected device (2 available)
    • iPhone 11 (mobile) • A04E2BA0-EC69-481D-967F-5069866571D9 • ios
      • com.apple.CoreSimulator.SimRuntime.iOS-14-5 (simulator)
    • Chrome (web)       • chrome                               • web-javascript
      • Google Chrome 92.0.4515.159

• No issues found!
```

但是我还是尝试了卸载然后重新安装`cocoapods`，不起作用。后来找了一大圈。发现是vscode的问题。把flutter的扩展删除，然后再重新安装，就可以了。