---
title: react native之IOS端打包
date: 2017-05-02
type: "react-native"
tags: [React Native]
---

#### 1. 进入你的react native工程目录：
```
cd /Users/xiaoming/rnApp
```

#### 2. 运行以下代码生成bundle文件，生成的目录和名字可以自定义，以下代码生成文件到 `ios`目录下，名字叫 `rnApp.bundle`：
``` 
react-native bundle --entry-file index.ios.js --platform ios --bundle-output ios/rnApp.bundle
```

<!--more-->


#### 3. 在Xcode里左侧的项目文件夹，右键 `Add Files to...` ，将刚才生成的两个文件添加到工程里。


#### 4. 在Xcode里打开 `AppDelegate.m` 文件，找到代码 `jsCodeLocation = [[RCTBundleURLProvider sharedSettings]`，在其下方添加以下代码 ：
```
jsCodeLocation = [[NSBundle mainBundle] URLForResource:@"rnApp" withExtension:@"bundle"];
```
注意：代码里的 `@"rnApp"` 和 `@"bundle"`，与之前生成的文件 `rnApp.bundle` 对应。


#### 5. 在Xcode里点击运行，使用模拟器测试app能否正常运行。

