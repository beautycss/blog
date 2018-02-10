---
title: mac os使用mkdocs生成文档及解决中文搜索问题
date: 2017-01-23 17:00
type: "前端工具"
tags: [mkdocs, markdown]
---

mkdocs是一款根据markdown文件生成文档站点的工具，用来生成项目的文档说明非常专业和方便，官网地址是：[http://www.mkdocs.org/](http://www.mkdocs.org/)

## 安装及使用`mkdocs`
使用mkdocs需要安装python和pip，由于mac os自带了`python`，因此直接从安装pip开始。

#### 1. 安装`pip`的简单方法：
``` 
sudo easy_install pip
```
<!--more-->

#### 2. 安装 `mkdocs` 工具：
```
sudo pip install mkdocs
```
说明：建议前面加一个sudo，碰到过报错问题使用此方法解决了

#### 3. 查看`mkdocs`的版本，以检查是否安装成功：
```
mkdocs --version
```

#### 4. 用`mkdocs`生成一个工程：
```
mkdocs new my-project
cd my-project
```

#### 5. 终端里输入`mkdocs serve`，运行刚刚创建的工程：
```
mkdocs serve
```

#### 6. 在浏览器中输入 [http://127.0.0.1:8000/](http://127.0.0.1:8000/) 访问：
```
http://127.0.0.1:8000/
```

#### 7. 配置`mkdocs`：
```
site_name: bootstrap使用文档              # 站点名称及页面标题
pages:                                  # 页面列表
    - 首页: index.md                     # 页面标题及左侧显示的名称：对应的md文件
    - 起步: start.md                     # 页面标题及左侧显示的名称：对应的md文件
theme: readthedocs                      # 此主题为蓝灰主题，默认为官方亮蓝色主题 
site_dir: api                           # 生成的文件夹名称，默认为site
# extra_css: [extra/extra.css]          # 额外的css文件，需要放在docs目录下跟md文件一起
```

官方主题：
![http://www.mkdocs.org/img/screenshot.png](http://www.mkdocs.org/img/screenshot.png)

readthedocs主题：
![http://www.mkdocs.org/img/readthedocs.png](http://www.mkdocs.org/img/readthedocs.png)



#### 7. 终端里输入`mkdocs build`，生成文档站点：
```
mkdocs build
```
生成完的站点包括完整的css,js,fonts,html等文件/目录。

#### 8. 部署文档站点：
mkdocs没有提供部署功能，自己可以发布到git站点或自己的空间/主机上去。


## 解决中文搜索问题
由于mkdocs生成json文件时将汉字转成了ascii码字符，且mkdocs使用的搜索插件lunr.js本身不支持中文，因此解决中文搜索问题就是要解决这两个问题。

#### 1. 打开Finder，使用快捷键`command+shift+g`，输入以下路径，进入mkdocs的目录：
```
/Library/Python/2.7/site-packages/mkdocs/
```
注意系统的python版本，我这使用的是2.7

#### 2. 用代码编辑器打开`search.py`文件，搜索`generate_search_index`，修改后的如下：
```
return json.dumps(page_dicts, sort_keys=True, ensure_ascii=False, indent=4)
```
添加了`ensure_ascii=False, `

#### 3. 保存文件，如果权限问题无法保存，就另存到其他目录下，再拷贝覆盖`mkdocs`目录下的`search.py`文件；

#### 4. 打开github站点 [https://github.com/codepiano/lunr.js](https://github.com/codepiano/lunr.js) 下载或克隆该修改版的lunr.js项目到本地；

#### 5. 将下载或克隆到本地的lunr.js项目里的`lunr.min.js`文件，拷贝覆盖到以下目录里的同名文件：
```
/Library/Python/2.7/site-packages/mkdocs/assets/search/mkdocs/js
```
此目录仍然为mkdocs工具的目录，覆盖官方工具里的同名文件，以后每次生成站点就直接好用了，也可以先备份官方使用的`lunr.min.js`文件再覆盖它

#### 6. 进入文档的目录，使用`mkdocs build`重新生成站点；

#### 7. 查看生成好的文档目录下`mkdocs/search_index.json`文件，确认里面的中文可以正常显示，非ascii码字符，说明修改官方文件成功；

#### 8. 将生成好的文档站点上传更新后，清除浏览器缓存，输入中文进行搜索，出来结果即表示成功



感谢 [让mkdocs支持中文搜索](http://wyqbailey.github.io/2015/08/13/make-mkdocs-support-chinese.html) 这篇博文的指点，该博文中提供的修改版lunr.js已经不支持中文，使用上面本文提供的支持中文。

