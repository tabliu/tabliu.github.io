---
title: JavaScript学习笔记（26）- DOM基本操作
date: 2019-04-30 14:20:10
tags:
  - code
  - javascript
  - note
---

## 什么是正则表达式

> 正则表达式（英语：Regular Expression，在代码中常简写为 regex、regexp 或 RE）使用单个字符串来描述、匹配一系列符合某个句法规则的字符串搜索模式。

### 查看滚动条的滚动距离

#### `window.pageYOffset/pageXOffset`

> 标准写法，高级浏览器和 IE9 以上支持。

```js
console.log(window.pageYOffset);
console.log(window.pageXOffset);
```

#### `document.body/documentElement.scrollLeft/scrollTop`

> IE8 及以下浏览器兼容写法
> 在不同版本 IE 浏览器兼容性比较混乱，但同一浏览器只具有一种方法
> 利用这特写，通过两种写法的值相加来取值

封装：求滚动条的滚动距离

```js
function getScrollOffset () {
  if (window.pageXOffset) {
    return {
      x: window.pageXOffset,
      y: window.pageYOffset
    }
  } else {
    return {
      x: document.body.scrollLeft + document.documentElement.scrollLeft,
      y: document.body.scrollTop + document.documentElement.scrollTop
    }
  }
}

console.log(getScrollOffset());
```

### 查看可视区域窗口尺寸

> 可视区域就是编写的 HTML 文档可以看到的部分，不含菜单栏、地址栏和控制台等容器部分的区域。

#### `window.innerWidth/innerHeight`

> 可视区域的宽高，包括滚动条宽高；
> 标准写法，高级浏览器和 IE9 以上支持。

```js
console.log(window.innerWidth);
console.log(window.innerHeight);
```

#### `document.documentElement.clientWidth/clientHeight`

> 标准模式下，任意浏览器都兼容

#### `document.body.clientWidth/clientHeight`

> 适用于怪异模式下的浏览器

插入说明：`<!DOCTYPE html>` 与怪异/混杂模式

DOCTYPE：浏览器的文档模型，添加后可以根据不同类型的文档模型来进行页面渲染；

怪异/混杂模式：为了兼容低版本浏览器下开发的页面在高版本浏览器能正常访问。去掉 DOCTYPE 即为怪异模式，向后兼容。

应用：封装浏览器视口尺寸的方法

```js
function getViewportOffset () {
  if (0 && window.innerWidth) {
    return {
      w: window.innerWidth,
      h: window.innerHeight
    }
  } else {
    if (document.compatMode === 'BackCompat') {
      return {
        w: document.body.clientWidth,
        h: document.body.clientHeight
      }
    } else {
      return {
        w: document.documentElement.clientWidth,
        h: document.documentElement.clientHeight
      }
    }
  }
}
console.log(getViewportOffset());
```

### 查看元素的几何尺寸

#### `getBoundingClientRect()`

> 该方法返回一个对象，包括：`top, right, bottom, left width, height`。`left` 和 `top`代表左上角距离视口左上角的坐标；`right` 和 `bottom` 代表右下角距离视口左上角的坐标；
> `width` 和 `height` 属性在老版本 IE 浏览器未实现；
> 返回的结果不是实时的。

#### `dom.offsetWidth/offsetHeight`

> 返回 `dom` 元素盒模型包含的宽高

#### `dom.offsetTop/offsetLeft`

> 返回 `dom` 元素的位置；
> 忽略自身的定位属性，返回的定位位置是距离**有定位父级元素**的值。也就是说：对于无定位父级的元素，返回相对文档的坐标；对于有定位父级的元素，返回相对于最近的有定位的父级元素的坐标。

#### `dom.offsetParent`

> 返回 `dom` 元素最近有定位的父级元素；如没有则返回 `body`；`body.offsetParent` 返回 `null`。

应用：求任意一个元素在文档中的坐标

```js
function getElementPosition () {

}
```

本节完。
