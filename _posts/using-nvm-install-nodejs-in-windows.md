---
title: 在Windows下使用NVM安装NodeJS
date: 2017-10-30
type: "nodejs"
tags: [nodejs]
---

如果要在同一台电脑上安装不同版本的nodejs来使用，需要用到nvm。


#### 1. 下载nvm安装包并安装，安装路径可自行设置。

#### 2. 打开你的nvm安装目录，安装nvm时设置的安装路径，以下为默认路径。
```
C:\Users\Administrator\AppData\Roaming\nvm
```

#### 3. 运行`install.cmd`，命令行里会提示安装路径，复制粘贴你的nvm安装路径即可。

<!--more-->

#### 4. 运行完成后，nvm目录下多了一个`setting.txt`文件，为nvm配置文件。

#### 5. 打开文件`setting.txt`，添加以下代码：
```
node_mirror: http://npm.taobao.org/mirrors/node/ 
npm_mirror: https://npm.taobao.org/mirrors/npm/
```
以上代码设置了镜像为国内，以便快速成功的安装nodejs。


#### 6. 安装nodejs，如果需指定nodejs位数需要在后面加，我这里根据需要安装的是32位，版本是8.3.0：
```
nvm install 8.3.0 32
```
这里指定了安装32位的nodejs，不指定则默认安装64位的。

#### 7. 安装好nodejs后，会提示用使用use来启动这个版本，输入以下命令来启用：
```
nvm use 8.3.0 32
```
使用use时也要带上安装的位数：32。


#### 8. 可以输入命令来查看使用的nodejs和npm版本，如果显示出来版本号表示该nodejs安装成功：
```
node -v
npm -v
```

#### 9. 为了便于快速安装npm模块，可以在用户目录建一个`.npmrc`文件，添加以下代码：
```
registry=https://registry.npm.taobao.org
```
该文件一般是在你的用户目录下，比如`C:\Users\YourUsername`下面。该配置设置了npm的源为淘宝的源，解决了默认访问官方的国外源慢的问题。

