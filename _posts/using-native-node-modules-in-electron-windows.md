---
title: Electron之在Windows下使用原生NodeJS模块
date: 2017-08-05
type: "Electron"
tags: [electron]
---

需要使用 `ffi` 和 `ref` 两个模块来读取DLL文件。


#### 1. 修改npm配置文件`.npmrc`，添加以下代码，以便安装`electron`；
```
registry=https://registry.npm.taobao.org
sass_binary_site=https://npm.taobao.org/mirrors/node-sass/
phantomjs_cdnurl=http://npm.taobao.org/mirrors/phantomjs
ELECTRON_MIRROR=http://npm.taobao.org/mirrors/electron/
```
该文件一般是在你的用户目录下，比如`C:\Users\YourUsername`下面。如果没有，就新建这个文件。以上代码设置了npm的源以及electron的镜像，设置electron镜像为国内，才能快速成功的安装electron。
<!--more-->

#### 2. 全局安装`electron`；
```
npm install electron -g
```
建议全局安装Electron。

```
npm install --arch=ia32 electron -g
```
如果需要安装32位，则使用`--arch=ia32`。

#### 3. 全局安装`electron-packager`，electron打包用；
```
npm install electron-packager -g
```

#### 4. 全局安装`electron-rebuild`，之后编译用；
```
npm install --save-dev electron-rebuild -g
```
为了方便我这里是全局安装。

#### 5. 命令行工具进入electron根目录，安装`ffi` 和 `ref` 两个模块；
```
npm install ffi
npm install ref
```

#### 6. 在electron根目录下运行`electron-rebuild`，以编译依赖的模块；
```
electron-rebuild
```

#### 7. electron根目录下的`package.json`里面打包配置，需要设置为64位的，否则调用NodeJS模块报错；
```
"build" : "electron-packager ./projectFolder newFolder --overwrite --platform=win32 --arch=x64 --out=dist --icon=projectFolder/icon.ico --electron-version=1.7.5"
```

#### 8. 运行打包后，需拷贝根目录的`node_modules`文件夹到生成的app下的`resources`目录下；


#### 9. 拷贝根目录下的`main.js`到生成的app下的`resources`目录下；

#### 10. 运行生成好的app，正常调用表示成功。