---
title: 前后端接口联调小结
date: 2018-06-25
type: "api, cors"
tags: [api, cors]
---

在项目开发过程中，前端与后端联调接口会碰到很多问题，比如规范、跨域等问题，因此总结了一些规范和开发中碰到的问题，以及如何设置和获取自定义相应头字段，方便前后端的同学参考和进一步了解。

## 接口规范：

#### 1. 后端需给出固定的路径，比如`/xxapi`；

#### 2. 基本的数据结构；
```
data: null,
success: true,
message: '请求成功',
```
默认的数据结构应该至少有这3个属性，如果没有数据则 `data` 为 `null` ；`success`属性是方便前端判断响应结果是否为成功的状态，比如登录页需要响应给前端的错误信息有很多种，而前端首先需要知道是成功还是失败来进行逻辑编码；如果失败，前端可直接将`message`显示给用户。
<!--more-->

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

## 常见接口报错及解决方案：
在前端页面访问后端接口的时候，如果后端或服务器端未做一些设置，会造成页面访问接口失败，在浏览器的控制台会显示报错信息。本文针对一些常见的错误，列出了解决方案。

#### 1. 不同端口或不同的域名产生的跨域问题，错误如下：

```
Failed to load ... : No 'Access-Control-Allow-Origin' header is present on the requested resource. Origin 'http://...' is therefore not allowed access. If an opaque response serves your needs, set the request's mode to 'no-cors' to fetch the resource with CORS disabled.
```
出现这个错误是因为前端页面跟后端接口在不同的端口或IP或域名下面，也就是跨域。关于跨域你可能需要详细了解 [跨域资源共享 CORS](https://developer.mozilla.org/zh/docs/Web/HTTP/Access_control_CORS)。跨域产生的原因是浏览器遵循的基本安全策略 - [MDN web docs[浏览器的同源策略]](https://developer.mozilla.org/zh-CN/docs/Web/Security/Same-origin_policy)。

解决方案：
```
Access-Control-Allow-Origin: *
Access-Control-Allow-Methods: GET,PUT,POST,DELETE,OPTIONS
```
如果后端是Java项目，一般是在开发环境服务器的nginx配置文件中添加上面几项来统一配置，而不是在后端代码里配置。添加完后重启nginx，然后在浏览器的响应头里应该可以看到刚才配置的几项，说明配置成功。这里`Access-Control-Allow-Origin`设置为`*`表示允许所有的域，也可以根据具体的环境来设置具体的IP或域名。

#### 2. 请求头类型的错误：
```
Request header field Content-Type is not allowed by Access-Control-Allow-Headers in preflight response.
```
这个错误当前请求Content-Type的值不被支持。可能前端发送的请求头类型有`application/json`，这个请求涉及到预检请求，可参看 [HTTP访问控制（CORS）](https://developer.mozilla.org/zh/docs/Web/HTTP/Access_control_CORS)。

解决方案：
```
Access-Control-Allow-Headers: Content-Type,*
```
nginx配置文件中添加这项配置，表示接收前端的`Content-Type`。

#### 3. 携带cookie设置产生的问题，错误如下：

```
Failed to load ... : Response to preflight request doesn't pass access control check: The value of the 'Access-Control-Allow-Credentials' header in the response is '' which must be 'true' when the request's credentials mode is 'include'. Origin 'http://...' is therefore not allowed access.
```
出现这个错误是因为前端请求接口的时候在请求头里加了`credentials: 'include'`参数携带cookie信息，而后端未做接收cookie的设置。

解决方案：
```
Access-Control-Allow-Credentials: true
```
nginx配置文件中添加这项配置，表示接收前端携带的cookie信息。只有前后端都设置，跨域携带cookie请求才能成功。详细的了解可参看 [MDN web docs [Access-Control-Allow-Credentials]](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Headers/Access-Control-Allow-Credentials)


## 自定义响应头解决方案：

在独立的前端工程中，用户访问到的虽然是静态页面，而实际上现在的前端页面已经有了自己的路由、权限控制等等。

在独立的前端工程工程中同样会碰到一个问题：前端页面如何比较好的获取用户超时状态来退出登录？本文介绍使用自定义响应头字段来解决这个问题。

##### 1. 后端在响应头里增加属性，例如：
```
x-session-expired: 1
```
后端代码通过增加`x-session-expired`为`1`来告诉前端当前用户超时了。然后在浏览器的响应里应该能看到上面这个配置。但是前端在跨域请求情况下默认只能获取到响应头里的`Cache-Control`、`Content-Language`、`Content-Type`、`Expires`、`Last-Modified`、`Pragma`6个基本字段，如果要获取到刚增加的`x-session-expired`字段，需要进行下一步。


#### 2. 后端增加以下配置：
```
Access-Control-Expose-Headers: x-session-expired
```
该配置的作用是指定哪些首部可以作为响应的一部分暴露给外部，这样前端才能获取到上面一步添加的`x-session-expired`字段及其值。

#### 3. 前端在 `fetch` 请求的时候获取自定义响应头字段：
```
return fetch(url, newOptions)
    .then(checkStatus)
    .then(response => {
      const { dispatch } = store;
      const { headers } = response;
      if (headers && headers.has('x-session-expired')) {
        // 如果有响应头里有x-session-expired则退出
        dispatch({
          type: 'login/logout',
        });
        return;
      }
      return response.json();
    })
    .catch((e) => {
        ...
    });
```
可以看到上面代码的`headers.has('x-session-expired')`，前端通过has方法来获取响应头里是否有`x-session-expired`字段，来判断是退出还是解析数据。

至此，后端设置自定义响应头字段且前端获取该字段的问题解决了。该方法也同样适用于添加其他的响应头字段，解决无权限或其他问题。

#### 疑问：可能有同学会提出：超时情况下，后端返回不同的json数据来告诉前端用户超时不行吗？

- 超时或者无权限的情况下，后端应该是给前端响应不同的状态码的，比如401无权限、403访问被禁止。但是如果此时还要给前端响应数据，按照规范就是要返回200的状态码，所以这样肯定是不好的。
- 从上面第三步的前端代码可以看出，如果响应头有那个超时的字段`x-session-expired`字段就直接退出登录，不进行解析响应的数据了。在响应头没有那个字段的情况下，才去解析响应的数据。如果前端要对响应的数据进行判断，还需要增加额外的代码去解析响应的数据，并额外增加逻辑代码进行状态判断，这样有的错误在代码逻辑里判断，有的错误通过状态码判断显然是不好的。应该把错误统一放到响应的状态码来处理会更好一些。


## 知识要点：
关于简单请求和非简单请求、以及预检请求等概念，建议看下面列出的前2篇详细了解一下。关于浏览器的同源策略等，可以看第3篇详细了解。

- [HTTP访问控制（CORS）](https://developer.mozilla.org/zh/docs/Web/HTTP/Access_control_CORS)
- [跨域资源共享 CORS 详解](http://www.ruanyifeng.com/blog/2016/04/cors.html)
- [浏览器的同源策略](https://developer.mozilla.org/zh-CN/docs/Web/Security/Same-origin_policy)
- [Access-Control-Allow-Credentials](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Headers/Access-Control-Allow-Credentials)

