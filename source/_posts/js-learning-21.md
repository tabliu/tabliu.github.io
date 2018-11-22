---
title: JavaScript学习笔记（21）- ES5严格模式
date: 2018-10-09 10:06:00
tags:
  - code
  - javascript
  - note
---

> JavaScript学习笔记。本笔记是基于在腾讯课堂《[Web前端开发之JavaScript精英课堂【渡一教育】](https://ke.qq.com/course/231577)》课程学习过程中记录的一些提纲和关键点。
> 强烈推荐想要进行js入门学习来听听，尤其是前面姬成讲的基础知识点。

## 前述

> 现在浏览器调用的js方法是基于`ES3.0`的和`ES5.0`的新增方法。
> 如果`ES3.0`的和`ES5.0`产生冲突时，使用`ES3.0`的方法；

## 严格模式的定义

> `ES3.0`的和`ES5.0`产生冲突时，使用`ES5.0`的方法来解决，这种模式就是`ES5.0`的严格模式；
> 使用严格模式不在兼容`ES3.0`的一些不规则语法，使用全新的`ES5.0`规范。

## 严格模式的使用

在逻辑代码的最顶端加入字符串：`"use strict"`，表示启用`ES5.0`的严格模式。

在严格模式下，一些方法不能被使用。比如：

```js
"use strict"

function test() {
console.log(arguments.callee);
}

test();
```

执行代码会报错：

> js_test29.html:17 Uncaught TypeError: 'caller', 'callee', and 'arguments' properties may not be accessed on strict mode functions or the arguments objects for calls to them

### 严格模式的两种使用方式

* 全局严格模式：在整段代码的最顶端添加`"use strict"`；
* 局部函数内严格模式：在该函数代码的最顶端添加`"use strict"`，推荐使用。

严格模式的使用会向下兼容，不会对不兼容的浏览器产生影响。

## 严格模式的规则

### `with`方法不可用

```js
"use strict"

with(document) {
  write('abc');
}
```

### `arguments.callee`方法不可用

### `func.caller`方法不可用

### 变量赋值前必须声明

### 局部的`this`必须被赋值使用

预编译时，局部定义的`this`不再指向`window`，默认指向空，所以必须被赋值使用，而且`this`指向所赋值；如果没有赋值使用，则报错。

在全局里`this`还是指向`window`。

### 拒绝使用重复的参数和属性名

本节完。
