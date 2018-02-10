---
title: Linux下安装NodeJS
date: 2017-11-01
type: "nodejs"
tags: [nodejs]
---


#### 1. 推荐在淘宝镜像（也可在nodejs官网）下载 [`node-v8.9.0-linux-x64.tar.gz`](http://npm.taobao.org/mirrors/node/latest-v8.x/node-v8.9.0-linux-x64.tar.gz) ，并上传到服务器的 `/usr/local` 目录下;
我这里以`8.9.0`版本为例，且使用的linux服务器为64位的。


#### 2. 在终端进入到服务器的 `/usr/local` 目录下，运行以下命令解压文件（或ftp工具解压也可以）；
```
tar zxf node-v8.9.0-linux-x64.tar.gz
```
<!--more-->

#### 3. 重命名文件夹，我这里改名为`node`，也可以使用ftp工具直接重命名；
```
mv node-v8.9.0-linux-x64 node
```

#### 4. 进入`node/bin`目录下看看是否有`node`及`npm`两个文件，没有的话重新解压看看；
```
cd node/bin
```

#### 5. 建立软链；
```
ln -s /usr/local/node/bin/node /usr/local/bin/node
ln -s /usr/local/node/bin/npm /usr/local/bin/npm
```
这样就可以将nodejs设置为全局访问了。

#### 6. 查看node及npm版本号，有的话则安装成功；
```
node -v
npm -v
```

#### 7. 附上查看linux版本的命令。
```
cat /proc/version
```
