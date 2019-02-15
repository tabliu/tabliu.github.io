---
title: ES6学习笔记（4）- 增强字面量
date: 2018-12-07 11:10:00
tags:
  - code
  - javascript
  - note
  - ES6
---

> ES6中提供了一种全新的增强字面量的方法，接下来简单的介绍下关于增强字面量的使用。

## 模版增强字面量

在日常开发中，字符串的使用随处可见，但在使用过程中经常会有各种限制，比如同一字符串不能折行，不能直接调用已定义的变量等。

ES6中针对这些做了改进，用两个反引号(`)进行标识，在(`)内，可以添加字符串，`html`标签或者变量。

### 可以替代常规字符串的定义方式

```js
// 常规
let str1 = 'hello world!'

console.log(str);

// ES6
let str2 = `hello world!`

console.log(str);
```

### 模版字符串内可以任意换行

```js
let html1 = '<div class="wrap"><h1>我是标题</h1><p>我是内容</p></div>';

console.log(html1);

let html2 = `<div class="wrap">
  <h1>我是标题</h1>
  <p>我是内容</p>
</div>`;

console.log(html2);
// <div class="wrap">
//   <h1>我是标题</h1>
//   <p>我是内容</p>
// </div>
```

使用增强模版字符串，(`)内的换行和空格都会被保留。

### 模版字符串内可加入变量占位符

模版字符串内的变量占位符允许将任何有效的 JS 表达式嵌入到模板字面量中，并将其结果输出为字符串的一部分。

变量占位符由 `${`起始，`}`结束，中间允许放入任意的 JS 表达式，可以是简单的变量，运算符或者函数等。

```js
let str = `hello world!`
let html = `<div class="wrap">
  <h1>我是标题</h1>
  <p>我想大声的喊：${str}</p>
</div>`;

console.log(html);
// <div class="wrap">
//   <h1>我是标题</h1>
//   <p>我想大声的喊：hello world!</p>
// </div>
```

## 对象、数组增强字面量

### 定义方法

我们经常用到的数组或者对象的字面量定义就是一种增强方式，展示直观并可以缩减代码。

```js
let arr1 = new Array();
let arr2 = []; //字面量定义
let obj1 = new Object();
let obj2 = {}; //字面量定义
```

### 属性和方法的增强

针对对象中的属性，如果属性名和值相同，可以只保留一个即可；

```js
let app = new Vue({
  el: '#app',
  router: router,
  template: '<App/>',
  components: { App }
});
```

其中的 `router` 则可以进行简写：

```js
let app = new Vue({
  el: '#app',
  router, // router: router,
  template: '<App/>',
  components: { App }
});
```

对于对象中的方法，也可以进行相应简写，将`: function`省略。比如 `Vue`框架中就采用了这样的形式：

```js
export default {
  name: 'app',
  data () {  // data: function () { ... }
    return {
      ...
    }
  },
  mounted () { // mounted: function () { ... }
    ...
  },
  created () { // created: function () { ... }
    ...
  }
}
```

下面看一个实例：

```js
function CreateBookShop(inventory) {
  return {
    inventory: inventory,
    inventoryValue: function () {
      return this.inventory.reduce(function (total, book) {
        return total + book.price;
      }, 0);
    },
    priceForTitle: function (title) {
      return this.inventory.find(function(book) {
        return book.title === title;
      }).price;
    }
  }
}

let inventroy = [{
    title: 'Vue',
    price: 10
  },
  {
    title: 'Angular',
    price: 15
  },
]

let bookShop = CreateBookShop(inventroy);

console.log(bookShop.inventoryValue());
console.log(bookShop.priceForTitle('Vue'));
```

通过字面量增强可以对代码进行简化如下：

```js
function CreateBookShop(inventory) {
  return {
    inventory,
    inventoryValue () {
      return this.inventory.reduce((total, book) => total + book.price, 0);
    },
    priceForTitle (title) {
      return this.inventory.find(book => book.title === title).price;
    }
  }
}
```

本节完。
