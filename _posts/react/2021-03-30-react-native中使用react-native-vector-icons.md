---
layout:     post
title:      "React-Native中使用react-native-vector-icons"
subtitle:   "📎"
date:       2021-03-30 12:00:00
author:     "Hiz"
header-img: "img/react.jpg"
catalog: true
tags:
    - React
    - React-Native
---

# 前言

[react-native-vector-icons](https://github.com/oblador/react-native-vector-icons) 是为react-native打造的一个Icon库。它包含了常见的`AntDesign`、`FontAwesome`、`Ionicons`等

# 安装

```shell
npm install --save react-native-vector-icons
```

# 使用

```jsx
import Icon from 'react-native-vector-icons/FontAwesome'

<View style={styles.container}>
  <TouchableWithoutFeedback onPress={() => navigation.toggleDrawer()}>
    <Icon
      name="bars"
      color="white"
      size={25}
    />
  </TouchableWithoutFeedback>
  <TouchableWithoutFeedback onPress={() => navigation.navigate('Search')}>
    <Icon
      name="search"
      color="white"
      size={25}
    />
  </TouchableWithoutFeedback>
</View>
```

安装完成后是无法直接使用的，在ios模拟器中显示？

# 配置xcode

* 双击 项目ios目录下的xxx.xcodeproj,将项目在xcode中打开,新建一个group,并取名为Fonts

<img src="https://gitee.com/inkkk0516/typora/raw/master/image-20210330134411125.png" alt="image-20210330134411125" style="zoom:50%;" />

* 将项目中使用的字体文件拖入到Fonts中，只需将你再项目中使用的文件拖入即可，无需全部都放进去。勾选下图中框起来的部分，字体可以在node_modules/react-native-vector-icons/Fonts中查找

  <img src="https://gitee.com/inkkk0516/typora/raw/master/image-20210330135027682.png" alt="image-20210330135027682" style="zoom:50%;" />

* 点击Info.plist 添加刚才拷贝的字体

  <img src="https://gitee.com/inkkk0516/typora/raw/master/image-20210330135225433.png" alt="image-20210330135225433" style="zoom:50%;" />

重新运行项目即可。

# 问题

> Error: Multiple commands produce

在 target -> Build phase > Copy Bundle Resource 中找到info.plist，并将其移除





![Apr-02-2021 14-52-24](/Users/eleven/Downloads/Apr-02-2021 14-52-24.gif)