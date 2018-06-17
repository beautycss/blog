---
title: Centos linux 7安装gogs
date: 2018-06-17
type: "前端工具"
tags: [gogs]
---

#### 我为什么使用gogs？
* 需要在自己的服务器搭建git服务；
* 需要简单方便地安装；
* 只需要使用基本的git服务；

详情的了解可参看[官方简介](https://gogs.io/docs/intro)。


### 安装步骤：
本文介绍最简单的二进制安装方式，且前提是已经安装Linux和Mysql的情况下。简单来说就是上传压缩包和解压，然后启动gogs服务进行一些配置就可以了，具体如下：

#### 1. 在gogs官网下载 [64位的TAR.GZ压缩包](https://dl.gogs.io/0.11.53/gogs_0.11.53_linux_amd64.tar.gz) ，并上传到服务器的 `/home` （或者其他）目录下；


#### 2. 在终端进入到服务器的 `/home` 目录下，运行以下命令解压刚上传的文件（或ftp工具解压也可以）；
```
tar zxf gogs_0.11.53_linux_amd64.tar.gz
```
<!--more-->

#### 3. 在终端进入解压后的目录例如`/home/gogs`，并执行`./gogs web`命令来运行gogs服务；
```
cd /home/gogs
./gogs web
```
gogs默认使用3000的端口，如果被占用则执行`./gogs web -port 3001`。

#### 4. 执行以下2条命令，为gogs创建一个名为`git`的用户。
```
sudo adduser git
su git
```

#### 5. 浏览器输入`http://yourServerIp:3000/install`来安装gogs(端口3000为实际使用的端口，`install`为默认安装页面的路径)，在安装页面需要填写数据库、gogs相关配置，以mysql为例：
* 为gogs添加mysql数据库；
* 配置mysql；
* 配置gogs相关信息；
* 配置其他信息；

完成以上配置并提交成功后，gogs就可以运行起来了。

#### 6. 在刚安装的gogs路径下，找到`/home/gogs/scripts/systemd/gogs.service` 这个systemd服务模版文件复制一份或下到本地；

注：此文件为Systemd服务模板文件，通过修改该文件来配置守护进程，以让gogs后台运行，因为`./gogs web`方式如果关闭终端，gogs服务就停了；

#### 7. 修改`gogs.service`文件，需要修改里面的 `User`、`Group`、`WorkingDirectory`、`ExecStart` 和 `Environment` 为相对应的值。其中 `WorkingDirectory` 为 gogs 实际安装路径根目录。
```
User=git
Group=git
Environment=USER=git HOME=/home/git
```
注：需要注意配置用户和群组，本文使用的`User`是git，`Group`也是git。配置错误会导致服务启动失败。

#### 8. 将修改好的文件`gogs.service`上传到`/etc/systemd/system`下，并执行`sudo systemctl enable gogs` 命令来激活gogs。

#### 9. 执行`sudo systemctl start gogs`来后台启动gogs，如果启动成功则gogs安装完成。

如果刚才已经通过`./gogs web`启动了，先退出再执行本步骤。


### gogs配置：
安装完gogs后，在安装目录下会多出来一个文件，本文的路径是`/home/gogs/custom/conf/app.ini`，通过修改该文件可以自定义配置。

### 域名绑定：
#### 1. 参照以下配置nginx并重启；
```
server {
    listen 80;
    server_name git.yourdomain.com;

    location / {
        proxy_pass http://localhost:3000;
    }
}
```

#### 2. 修改`/home/gogs/custom/conf/app.ini`里`[server]`下面的`ROOT_URL`为上面nginx配置的域名，并重`gogs服务就可以了；


### 官方常见问题：
[https://gogs.io/docs/intro/faqs](https://gogs.io/docs/intro/faqs)
