---
title: Linux下安装n来管理多版本NodeJS
date: 2018-02-22
type: "nodejs"
tags: [nodejs]
---


在linux环境下已经安装了nodejs的情况下，需要升级或使用其它版本的nodejs，这时需要使用工具n来管理。


### 安装步骤：

#### 1. 已经安装了nodejs环境;


#### 2. 全局安装n；
```
npm install -g n
```
<!--more-->

#### 3. 输入命令n来查看是否安装成功，成功的话会出来命令提示列表；
```
n
```

#### 4. 安装新版的nodejs，这里以安装9.2.0版本为例；
```
n 9.2.0
```

#### 5. 查看新安装的node版本和npm版本；
```
node -v
npm -v
```

#### 6. 如果显示的版本为新的版本，说明安装成功。


### 错误1及解决步骤：

如果执行 `node -v` 显示的版本仍为旧版，说明node安装目录和n的默认路径不一致，需要把n的路径设置为当前node的目录；

#### 1. 先获取当前的node安装目录。
```
which node
```
比如获取到的是 `/usr/local/node`。如果获取到的路径是 `/usr/local/bin/node`，说明之前安装的node使用了软链，需要找到当前node的安装目录，例如`/usr/local/node`。

#### 2. 打开环境配置文件；
```
vim ~/.bash_profile
```

#### 3. 修改环境配置文件并保存；
```
export N_PREFIX=/usr/local/node #当前node的安装目录
export PATH=$N_PREFIX/bin:$PATH
```

#### 4. 执行source使修改生效；
```
source ~/.bash_profile
```


### 错误2及解决步骤：
如果出现 `n: command not found` 的错误，说明除了要解决路径不一致的问题，还要建立一个软链。

#### 1. 执行上面修改路径的4步。

#### 2. 找到n的安装目录，建立软链；
```
ln -s /usr/local/node/lib/node_modules/n/bin/n /usr/local/bin/n
```
