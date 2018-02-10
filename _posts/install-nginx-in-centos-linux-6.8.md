---
title: Centos linux 6.8安装nginx
date: 2017-03-03
type: "前端工具"
tags: [nginx]
---


#### 1. 在nginx官网下载 [`1.10.3.tar.gz`](http://nginx.org/download/nginx-1.10.3.tar.gz) ，并上传到服务器的 `/usr/local` 目录下：


#### 2. 在终端进入到服务器的 `/usr/local` 目录下，运行以下命令解压文件（或ftp工具解压也可以）：
```
tar zxf nginx-1.10.3.tar.gz
```
<!--more-->

#### 3. 安装c++工具和库：
```
yum install gcc gcc-c++ ncurses-devel perl
```

#### 4. 安装PCRE库：
```
yum -y install pcre-devel
```

#### 5. 安装openssl库：
```
yum -y install openssl openssl--devel
```

#### 6. 安装zlib库：
```
yum install -y zlib-devel
```

#### 7. 进入解压的目录，并运行以下配置代码：
```
cd ./nginx-1.10.3
./configure --prefix=/usr/local/nginx
```

#### 8. 编译及安装nginx：
```
make
make instal
```

#### 9. 启动nginx命令：
```
/usr/local/nginx/sbin/nginx
```

#### 10. 重启nginx命令：
```
/usr/local/nginx/sbin/nginx -s reload
```

#### 11. 建立一个软链：
```
ln -s /usr/local/nginx/sbin/nginx /bin/nginx
```
之后就可以运行 `nginx -s reload` 来重启nginx了。

