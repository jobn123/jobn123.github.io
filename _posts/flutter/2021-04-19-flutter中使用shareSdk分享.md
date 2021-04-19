---
layout:     post
title:      "flutter中使用shareSdk，进行微信、微博、qq分享"
subtitle:   " 🏷 "
date:       2021-04-19 12:00:00
author:     "Hiz"
header-img: "img/flutter.jpg"
catalog: true
tags:
    - Flutter
---

# 前置条件

去各自平台的开放平台申请创建app.
[微信开放平台](https://open.weixin.qq.com/cgi-bin/index?t=home/index&lang=zh_CN)、[QQ互联](https://connect.qq.com/manage.html#/mobileinfo/mobile/101944353)、[微博开放平台](https://open.weibo.com/)、

# 注册MobTech账户

[MobTech](https://www.mob.com/developer/register),创建app,并接入shareSDK

在分享设置里面，找到微信，qq，微博填入在各自平台申请的appid、appsecrect、App UniversalLink。

![image-20210419144935781](https://gitee.com/inkkk0516/typora/raw/master/image-20210419144935781.png)

# 安装shareSdk

* **在pubspec.yaml文件中加入依赖**,并安装

  ```yaml
  dependencies:
    sharesdk_plugin:
  ```

* 在需要分享的页面导入sdk

  ```yaml
  import 'package:sharesdk_plugin/sharesdk_plugin.dart';
  ```

* 在项目工程的Info.plist中如图增加**MOBAppKey** 和 **MOBAppSecret** 两个字段

  ![image-20210419145738358](https://gitee.com/inkkk0516/typora/raw/master/image-20210419145738358.png)

  这两个字段是在MobTech创建好应用之后生成的。

# **配置对应平台的URL Scheme**

* 微信的URL Scheme的配置就是微信开放平台注册应用获取得到的AppID

* 微博的URL Scheme的配置就是微博开放平台注册应用获取到的Appkey，并在前面加wb前缀，格式为wb+AppKey；

* QQ的URL Scheme的配置就是腾讯开放平台注册应用获取到的AppID，并且转成十六进制，另外在前面加QQ前缀，格式为：QQ＋AppId的16进制（如果appId转换的16进制数不够8位则在前面补0）

  ![image-20210419150327798](https://gitee.com/inkkk0516/typora/raw/master/image-20210419150327798.png)

# 配置各平台白名单

![image-20210419150718736](https://gitee.com/inkkk0516/typora/raw/master/image-20210419150718736.png)

# 使用shareSdk

```dart
import 'package:sharesdk_plugin/sharesdk_plugin.dart';

@override
void initState() {
  super.initState();
  ShareSDKRegister register = ShareSDKRegister();
  register.setupWechat("wx84cfe5545",
      "d4b2cf8c89b9220445f4aeb3", "https://www.xxx.cn/app/");
  register.setupSinaWeibo("366393434405104", "a79168add356cbaecdfec3",
      "http://www.sharesdk.cn", "https://www.xxxx.cn/app/");
  register.setupQQ("10194443434353", "713b67709f2485aa0f13d640c8");
  SharesdkPlugin.regist(register);
  SharesdkPlugin.addRestoreReceiver(_onEvent, _onError);
}

@override
Widget build(BuildContext context) {
  return Scaffold(
    appBar: AppBar(
      title: const Text('分享测试'),
    ),
    body: ListView(
      padding: EdgeInsets.fromLTRB(0.0, 30.0, 0.0, 0.0),
      children: <Widget>[
        _creatRow("分享到微信", "分享图片到微信", shareToWechat, context),
        _creatRow("分享到QQ", "测试自定义参数", shareQQCustom, context),
        _creatRow("分享到新浪微博", "测试自定义参数", shareSinaCustom, context),
      ],
    ),
  );
}

// 分享到微信会话
// 如果直接发朋友圈将 ShareSDKPlatforms.wechatSession 改成 ShareSDKPlatforms.wechatTimeLine
void shareToWechat(BuildContext context) {
  SSDKMap params = SSDKMap()
    ..setGeneral(
        "title",
        "text",
        [
          "http://download.sdk.mob.com/web/images/2019/07/30/14/1564468183056/750_750_65.12.png"
        ],
        "http://download.sdk.mob.com/web/images/2019/07/30/14/1564468183056/750_750_65.12.png",
        null,
        "http://pic28.photophoto.cn/20130818/0020033143720852_b.jpg",
        "http://pic28.photophoto.cn/20130818/0020033143720852_b.jpg",
        "http://i.y.qq.com/v8/playsong.html?hostuin=0&songid=&songmid=002x5Jje3eUkXT&_wv=1&source=qq&appshare=iphone&media_mid=002x5Jje3eUkXT",
        "http://i.y.qq.com/v8/playsong.html?hostuin=0&songid=&songmid=002x5Jje3eUkXT&_wv=1&source=qq&appshare=iphone&media_mid=002x5Jje3eUkXT",
        null,
        SSDKContentTypes.image);
  // 如果直接发朋友圈改成 ShareSDKPlatforms.wechatTimeLine
  SharesdkPlugin.share(ShareSDKPlatforms.wechatSession, params,
      (SSDKResponseState state, Map userdata, Map contentEntity,
          SSDKError error) {
    showAlert(state, error.rawData, context);
  });
}

// 分享到QQ空间
// 如果想直接分享到qq会话将ShareSDKPlatforms.qZone改成ShareSDKPlatforms.qq
void shareQQCustom(BuildContext context) {
  SSDKMap params = SSDKMap()
    ..setQQ(
        "textx",
        "title",
        "http://m.93lj.com/sharelink?mobid=ziqMNf",
        null,
        null,
        null,
        null,
        "",
        "http://wx4.sinaimg.cn/large/006tkBCzly1fy8hfqdoy6j30dw0dw759.jpg",
        null,
        null,
        "http://m.93lj.com/sharelink?mobid=ziqMNf",
        null,
        null,
        SSDKContentTypes.webpage,
        ShareSDKPlatforms.qZone);
  // 直接分享到qq会话改成ShareSDKPlatforms.qq
  SharesdkPlugin.share(ShareSDKPlatforms.qZone, params,
      (SSDKResponseState state, Map userdata, Map contentEntity,
          SSDKError error) {
    showAlert(state, error.rawData, context);
  });
}

// 分享到新浪微博
void shareToSina(BuildContext context) {
  SSDKMap params = SSDKMap()
    ..setGeneral(
        "title",
        "text",
        ["http://wx3.sinaimg.cn/large/006nLajtly1fpi9ikmj1kj30dw0dwwfq.jpg"],
        "http://wx3.sinaimg.cn/large/006nLajtly1fpi9ikmj1kj30dw0dwwfq.jpg",
        null,
        "http://www.mob.com/",
        "http://wx4.sinaimg.cn/large/006WfoFPly1fw9612f17sj30dw0dwgnd.jpg",
        "http://i.y.qq.com/v8/playsong.html?hostuin=0&songid=&songmid=002x5Jje3eUkXT&_wv=1&source=qq&appshare=iphone&media_mid=002x5Jje3eUkXT",
        "http://f1.webshare.mob.com/dvideo/demovideos.mp4",
        null,
        SSDKContentTypes.auto);

  SharesdkPlugin.share(ShareSDKPlatforms.sina, params,
      (SSDKResponseState state, Map userdata, Map contentEntity,
          SSDKError error) {
    showAlert(state, error.rawData, context);
  });
}
```

**其它更多平台分享**

[官方demo](https://github.com/MobClub/ShareSDK-For-Flutter/blob/master/README_CN.md)

# 参考

[flutter接入shareSDK文档](https://www.mob.com/wiki/detailed?wiki=ShareSDK_for_Flutter&id=14)

[ios使用shareSDK接入分享](https://blog.csdn.net/pipimob/article/details/79149898)