---
title: nginx服务器下解决字体跨域的问题
date: 2016-12-25
type: "前端工具"
tags: [nginx, 跨域]
#photos:
#- https://www.hdwallpapers.net/previews/starry-night-over-corsica-986.jpg
#- https://www.hdwallpapers.net/previews/water-plant-close-up-979.jpg
---


#### 1. 进入 `/usr/local/nginx/conf` 目录，打开 `nginx.conf` 文件，
```
/usr/local/nginx/conf
```

#### 2. 找到当前使用的server及要修改的域名处，增加以下代码并保存：

``` 
    location ~* \.(ttf|otf|eot|woff|woff2)$ {
        add_header Access-Control-Allow-Origin *;
    }
```
<!--more-->

#### 3. 在 `nginx.conf` 文件的同级目录，打开 `mime.types` 文件并找到以下代码：
```
    application/font-woff                 woff;
```

#### 4. 在其上方增加以下代码并保存：
```
    font/ttf                              ttf;
    font/otf                              otf;
    application/font-woff2                woff2;
```

#### 5. 重启nginx服务器：
```
    /usr/local/nginx/sbin/nginx -s reload
```

#### 6. 清除浏览器缓存并查看效果，完成。


