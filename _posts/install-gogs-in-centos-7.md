---
title: Centos linux 7安装gogs及自动部署
date: 2018-06-17
type: "前端工具"
tags: [gogs]
---

#### 我为什么使用gogs？
* 需要在自己的服务器搭建git服务；
* 简单方便地安装git服务；
* 只需使用基本的git服务；

关于gogs的详细介绍可参看[官方简介](https://gogs.io/docs/intro)。


### 安装步骤：
本文介绍最简单的二进制安装方式，且前提是已经安装Linux和Mysql的情况下。简单来说就是上传压缩包和解压，然后启动gogs服务进行一些配置就可以了，具体如下：

#### 1. 在gogs官网下载 [64位的TAR.GZ压缩包](https://dl.gogs.io/0.11.53/gogs_0.11.53_linux_amd64.tar.gz) ，并上传到服务器的 `/home` （或者其他）目录下；
<!--more-->

#### 2. 在终端进入到服务器的 `/home` 目录下，运行以下命令解压刚上传的文件（或ftp工具解压也可以）；
```
tar zxf gogs_0.11.53_linux_amd64.tar.gz
```

#### 3. 在终端进入解压后的目录例如`/home/gogs`，并执行`./gogs web`命令来运行gogs服务；
```
cd /home/gogs
./gogs web
```
gogs默认使用3000的端口，如果被占用则执行`./gogs web -port 3001`。

#### 4. 浏览器输入`http://yourServerIp:3000/install`来安装gogs(端口3000为实际使用的端口，`install`为默认安装页面的路径)，在安装页面需要填写数据库、gogs相关配置，以mysql为例：
* 为gogs添加mysql数据库；
* 配置mysql；
* 配置gogs相关信息；
* 配置其他信息；

完成以上配置并提交成功后，gogs就可以运行起来了。

#### 5. 在刚安装的gogs路径下，找到`/home/gogs/scripts/systemd/gogs.service` 这个systemd服务模版文件复制一份或下到本地；

注：此文件为Systemd服务模板文件，通过修改该文件来配置守护进程，以让gogs后台运行，因为`./gogs web`方式如果关闭终端，gogs服务就停了；

#### 6. 修改`gogs.service`文件，需要修改里面的 `User`、`Group`、`WorkingDirectory`、`ExecStart` 和 `Environment` 为相对应的值。其中 `WorkingDirectory` 为 gogs 实际安装路径根目录。
```
User=root
Group=root
Environment=USER=root HOME=/home/git
```
注：需要注意配置用户和群组，配置错误会导致服务启动失败。本文使用的`User`和`Group`都是root，建议单独创建一个`git`用户。

#### 7. 将修改好的`gogs.service`文件上传到`/etc/systemd/system`下，并执行以下命令来激活gogs。
```
sudo systemctl enable gogs
```

#### 8. 执行以下命令来后台启动gogs，如果启动成功则gogs安装完成。
```
sudo systemctl start gogs
```

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


### 自动部署：
通过git的钩子(hook)来实现：提交代码后，直接将代码更新到你的nginx目录，以达到自动部署。

#### 1. 在gogs网站上创建一个仓库，例如创建好的地址是`http://git.yoursite.com/user/blog.git`；


#### 2. 打开终端并执行以下命令来进入刚创建的仓库的`hooks`目录下，然后找到`post-update.sample`文件，并复制一份到相同目录下，将其改名为`post-update`。
```
cd /home/gogs-repositories/user/blog.git/hooks
```
`/home/gogs-repositories`目录为`/home/gogs/custom/conf/app.ini`里[repository]的`ROOT`配置，也就是gogs仓库的根目录。

#### 3. 修改`post-update`文件为如下并保存；
```
#!/bin/sh

unset GIT_DIR
DeployPath=/www/wwwroot/blog/  #此目录为服务器页面目录
cd $DeployPath
git init
git remote add origin /home/gogs-repositories/user/blog.git
git clean -df
git pull origin master
```
以上的几行命令的意思是待git更新完成后，就进入nginx的blog应用目录下，然后清空该目录并拉取最新的代码。

#### 4. 终端执行以下命令为`post-update`文件设置执行权限；
```
chmod +x post-update
```

#### 5. 修改`blog.git`仓库下的任意文件并提交代码，然后检查nginx目录下的文件，应该会更新成功，则自动部署成功。


#### 6. hexo的自动部署需修改hexo根目录下`_config.yml`文件里的`deploy`部分；
```
deploy:
  type: git
  repo: http://git.yoursite.com/user/blog.git
  branch: master
```
修改`repo`和`branch`就可以了，一定要注意修改`branch`为你的正确的branch，然后执行`hexo d`应该会自动部署到刚才设置的nginx目录下了。



### 官方常见问题：
[https://gogs.io/docs/intro/faqs](https://gogs.io/docs/intro/faqs)


#### 常用gogs命令：
停止gogs:
```
sudo systemctl stop gogs
```

启动gogs:
```
sudo systemctl start gogs
```

检查gogs运行状态:
```
sudo systemctl status gogs -l
```
