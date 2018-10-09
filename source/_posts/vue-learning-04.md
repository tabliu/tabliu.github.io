---
title: Vue 学习笔记(4) - 使用vue-resource进行http请求操作
date: 2018-09-26 09:30:00
tags:
  - code
  - vue
  - note
---

> Vue使用过程中，最重要的操作离不开数据操作，即http请求中常用的包括增、删、改、查等操作。
> Vue进行http请求操作有两种方式，引入`vue-resource` 或者 `axios`。

接下来主要对`vue-resource`做介绍。

## `vue-resource`的安装

```bash
npm install vue-resource --save-dev
```

安装后需要引入和使用：

```js
import VueResource form 'vue-resource'
Vue.use(VueResource)
```

## `vue-resource`的特点

1. 体积小。`vue-resource` 非常小巧，在压缩以后只有大约12KB，服务端启用gzip压缩后只有4.5KB大小，这远比jQuery的体积要小得多。

2. 支持主流的浏览器。和Vue.js一样，vue-resource除了不支持IE 9以下的浏览器，其他主流的浏览器都支持。

3. 支持`Promise API` 和 `URI Templates`。

4. 支持拦截器。

## `vue-resource`的使用

### 全局使用

```js
// 基于全局Vue对象使用http
Vue.http.get('url', [options]).then(successCallback, errorCallback);

Vue.http.post('url', [body], [options]).then(successCallback, errorCallback);
```

### 局部使用

常用于`$http`直接在`methods`或者`created`中定义。

```js
// 基于局部Vue文件使用http
this.$http.get('url', [options]).then(successCallback, errorCallback);

this.$http.post('url', [body], [options]).then(successCallback, errorCallback);
```

说明：

* 发送 `http`请求后，通过`then`方法相应处理结果并进行接收返回结果。
* `then`方法返回的结果有两个参数：一个是响应成功的回调方法，另一个是响应失败的回调。

`then`方法的书写形式有以下几种：

```js
//一般模式
this.$http.get('url', [options]).then(function(response) {
  console.log("请求响应成功！")
}, function(error) {
  console.log("请求响应失败！")
});

//ES6模式
this.$http.get('url', [options]).then((response) => {
  console.log("请求响应成功！")
}, (error) => {
  console.log("请求响应失败！")
});
```

还可以使用`catch`方法来响应和捕捉请求失败的异常：

```js
//catch模式
this.$http.get('url', [options]).then((response) => {
  console.log("请求响应成功！")
}).catch((error) => {
  console.log("请求响应失败！")
});
```

代码的`error`并不是包含在`then`方法中，而是在和`then`方法并列的`catch`方法里。

`catch`方法和`then`方法中`errorCallback`是不同的，`then`方法只在响应失败时调用，而`catch`方法是在整个请求到响应过程中，只要程序出错了就会被调用。

## `vue-resource`支持的HTTP方法

`vue-resource`的请求API是按照REST风格设计的，它提供了7种请求API：

```js
get(url, [options])
post(url, [body], [options])
delete(url, [options])
put(url, [body], [options])
patch(url, [body], [options])
head(url, [options])
jsonp(url, [options])
```

### options对象

发送请求时的options选项对象包含以下属性：

| 参数   | 类型        | 描述                                 |
| :----- | :---------- | :----------------------------------- |
| url | String | 请求的URL。可以是绝对地址，相对地址或者是字符串拼接地址 |
| method | String | 请求的HTTP方法，例如：'GET', 'POST'或其他HTTP方法 |
| body | Object, FormDataString | request body |
| params | Object | 请求的URL参数对象 |
| headers | Object | request header |
| timeout | Number | 单位为毫秒的请求超时时间 (0 表示无超时时间) |
| before | function(request) | 请求发送前的处理函数，类似于jQuery的beforeSend函数 |
| progress | function(event) | ProgressEvent回调处理函数 |
| emulateHTTP | Boolean | 发送PUT, PATCH, DELETE请求时以HTTP POST的方式发送，并设置请求头的X-HTTP-Method-Override |
| emulateJSON | Boolean | 将request body以application/x-www-form-urlencoded content type发送 |

### response对象

response对象包含以下属性：

| 方法   | 类型        | 描述                                 |
| :----- | :---------- | :----------------------------------- |
| text() | String | 以String形式返回response body |
| json() | Object | 以JSON对象形式返回response body |
| blob() | Blob | 以二进制形式返回response body |

| 属性   | 类型        | 描述                                 |
| :----- | :---------- | :----------------------------------- |
| ok | Boolean | 响应的HTTP状态码在200~299之间时，该属性为true |
| status | Number | 响应的HTTP状态码 |
| statusText | String | 响应的状态文本 |
| headers | Object | 响应头 |

### `get`请求

```js
this.$http.get('url', { params: { param1: 1, param2: 2 }})
  .then((response) => {
    console.log(response.body) //成功后返回数据
  }, (error) => {
    console.log(error) //失败返回错误信息
  })

```

说明：

* `url`：必须。可以是绝对地址，也可以是设置代理后的地址；
* `params`：非必须，需要向后台传的参数，没有则不填。

## 参考文档

* [Vue_VueResource](https://segmentfault.com/a/1190000007087934)
* [vue-resource插件使用](https://www.cnblogs.com/axl234/p/5899137.html)
