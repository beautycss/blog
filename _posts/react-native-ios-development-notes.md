---
title: react native之IOS端开发笔记
date: 2017-03-06
type: "react-native"
tags: [React Native]
---

#### 建议使用或升级到0.4.0版本。版本升级：
```
react-native upgrade
```

#### 0.4.0及以上版本的RN插件安装：
``` 
react-native link
```
此命令给所有已安装的native插件链接原生依赖

<!--more-->

#### 允许访问网络：
在Xcode中打开IOS工程的`Info.plist`文件，在 `App Transport Security Settings` 下添加 `Allow Arbitrary Loads` 属性，值为 `true`
```
Allow Arbitrary Loads
true
```

#### 允许访问相机：
在Xcode中打开IOS工程的`Info.plist`文件，在最下添加 `Privacy - Camera Usage Description` 属性，值可以填获取相机权限的提示信息，也可以为空
```
Privacy - Camera Usage Description
```

#### 允许访问相册库：
在Xcode中打开IOS工程的`Info.plist`文件，在最下添加 `Privacy - Photo Library Usage Description` 属性，值可以填获取访问相册权限的提示信息，也可以为空
```
Privacy - Photo Library Usage Description
```

#### 支持的屏幕方向设置：
在Xcode里点击 `General` 标签， 在 `Device Orientation` 可以选择app支持的屏幕方向。

![屏幕方向设置](/assets/images/react-native-ios/orientation.png)


#### 启动屏设置：

1. 在Xcode里点击 `General` 标签， 在 `Launch Images Source` 填入 `LaunchImage`，下面的 `Launch Screen File` 留空：

![启动屏设置-使用LaunchImage](/assets/images/react-native-ios/launch-setting-1.png)

2. 进入Xcode，点击左侧 `Images.xcassets` ，然后根据右侧提示的尺寸，做好图片后，对应的拖进去就设置好了启动屏图片。

![启动屏设置-上传图片](/assets/images/react-native-ios/launch-setting-2.png)


