---
title: 前后端接口联调小记
date: 2018-06-25
type: "api"
tags: [api]
---

在项目开发过程中，前端与后端联调接口会碰到很多问题，比如规范、跨域等问题，因此做了一下总结，方前后端的同学参考。

## 接口规范：

#### 1. 给出固定的路径，比如`/xxapi`；

#### 2. 基本的数据结构；
```
data: null,
success: true,
message: '请求成功',
```
默认的数据结构应该至少有这3个属性，如果没有数据则`data`为`null`；`success`属性是方便前端判断响应结果是否为成功的状态，可能有时响应结果的错误有很多种情况，而前端首先需要知道是成功还是失败；如果失败，前端可将`message`显示给用户，比如登录页。

#### 3. 列表页的数据结构；
```
data: [
    list: [
        {id:1, title: '1'},
        {id:2, title: '2'},
        ...
    ],
    pagination: {
        total: 300,
        current: 1,
    }
],
success: true,
message: '请求成功',
```
total为记录总数，current为当前第几页，默认显示数前后端应商定好，比如20条，写到各自工程的配置参数。

#### 4. 详情页的数据结构；
```
data: [
    title: '标题',
    content: '内容',
    ...
],
success: true,
message: '请求成功',
```

## 跨域解决方案：

#### 1. 不同端口或不同的域名产生的跨域，错误如下：

```
Failed to load ... : No 'Access-Control-Allow-Origin' header is present on the requested resource. Origin 'http://...' is therefore not allowed access. If an opaque response serves your needs, set the request's mode to 'no-cors' to fetch the resource with CORS disabled.
```

解决方案：
```
Access-Control-Allow-Origin: *
Access-Control-Allow-Methods: GET,PUT,POST,DELETE,OPTIONS

```
如果后端是JAVA,一般是在开发环境服务器的nginx增加上面的配置。


```
Access-Control-Allow-Headers: Content-Type,*
```

```
Access-Control-Allow-Credentials: true
```

```
Access-Control-Expose-Headers: x-session-expired,x-session-noperms
```
