---
title: JavaScript学习笔记（22）- 初识DOM
date: 2018-10-10 14:06:00
tags:
  - code
  - javascript
  - note
---

> JavaScript学习笔记。本笔记是基于在腾讯课堂《[Web前端开发之JavaScript精英课堂【渡一教育】](https://ke.qq.com/course/231577)》课程学习过程中记录的一些提纲和关键点。
> 强烈推荐想要进行js入门学习来听听，尤其是前面姬成讲的基础知识点。

## DOM的定义

> DOM（Document Object Model），定义了表示和修改文档所需的方法。DOM对象即为宿主对象，由浏览器厂商定义，用来操作`HTML`和`XML`功能的一类对象的集合。也有人称DOM是对`HTML`以及`XML`的标准编程接口。

## DOM的基本操作

### document

> 代表整个文档的最顶级标签；
> `document`上有一些列方法；
> `html`代表整个文档的根标签。

### getElementById

> 通过访问元素标签`id`来获取标签的方法；
> 元素标签`id`在`IE8`以下的浏览器下不区分`id`大小写，而且也返回匹配`name`属性的元素；
> 通过`getElementById`获取的是唯一元素。

### getElementsByTagName

> 通过访问元素标签名来获取一组标签的方法。
> 获取的一组数据是以***类数组***的形式进行存储。

划重点：***获取的所有成组的数据基本都是类数组***。

该方法是`js`操作DOM中最主流最好用的方法之一，兼容性极强。

### getElementsByClassName

> 通过访问元素标签样式名来获取一组标签的方法。
> 有兼容性，`IE8`及以下浏览器不支持使用。

### getElementsByName

> 通过访问元素标签`name`值来获取一组标签的方法。
> 兼容性不强，只有部分标签的name可生效，不推荐使用。

### querySelector

> 通过访问css选择器来选择一个标签；
> 有兼容性，在`IE7`及以下浏览器不支持使用；

### querySelectorAll

> 通过访问css选择器来选择一组元素；
> 有兼容性，在`IE7`及以下浏览器不支持使用；

`querySelector`和`querySelectorAll`不是实时的，获取的只是数据的副本，一个静态数据。

## DOM的节点操作

### 遍历节点操作

* `parentNode`：标签的父节点，最顶端的父节点为`#document`；一个元素只有一个`parentNode`。
* `childNodes`：标签的子节点，返回一个类数组；只包含指定元素的下一级节点的集合，不包含子节点的子节点。
* `firstChild`：第一个子节点
* `lastChild`：最后一个子节点
* `nextSibling`：下一个兄弟节点
* `previousSibling`：上一个兄弟节点

### 遍历元素节点数

* `prantElement`：返回当前元素的父元素节点（`IE9`以下不兼容）
* `children`：只返回当前元素的子节点，兼容性强
* `node.childrenElementCount`：`===node.children.length`，返回当前元素子节点的个数。（`IE9`以下不兼容）
* `firstElementChild`：返回当前元素的第一个子节点（`IE9`以下不兼容）
* `lastElementSibling`：返回当前元素的最后一个子节点（`IE9`以下不兼容）
* `nextElementSibling`：返回当前元素的下一个兄弟节点（`IE9`以下不兼容）
* `previousElementSibling`：返回当前元素的上一个兄弟节点（`IE9`以下不兼容）

实例：下面`div`标签下有多少个节点

```html
<body>
  <div>
    <p></p>
    <span></span>
    <!-- this is a commet -->
  </div>

  <script>
    //在原型链上实现children()的方法
    Element.prototype.myChildren = function() {
      var nodes = this.childNodes;
      var len = nodes.length;
      var elements = {
        length: 0,
        push: Array.prototype.push,
        splice: Array.prototype.splice
      };
      for (let i = 0; i < nodes.length; i++) {
        const temp = nodes[i];
        if (temp.nodeType === 1) {
          elements.push(temp);
        }
      }
      // console.log(elements);
      return elements;
    };
    var div = document.getElementsByTagName('div')[0];
    console.log(div.myChildren());
  </script>
</body>
```

### 节点的属性

#### `nodeName`

> 元素节点的标签名，以大写形式表示，只读。
> 用来区分和判断节点。

* `Document`：`#document`；
* 元素节点：标签名（全大写）；
* 属性节点：属性名；
* 文本节点：`#text`；
* `Doctype`：`html`；

```js
console.log(document.nodeName); //"#document"
console.log(document.body.nodeName); //"BODY"
console.log(document.firstChild.nodeName); //"html"
```

#### `nodeValue`

> `Text`节点或者`Comment`节点的文本内容，可读写。
> 用来区分和判断节点的时候用。

* `Document`：`null`；
* 元素节点：`null`；
* 属性节点：属性值；
* 文本节点：文本内容；
* `Doctype`：`null`；

```js
console.log(document.nodeValue); //null
console.log(document.body.nodeValue); //null
console.log(document.firstChild.nodeValue); //null
```

#### `nodeType`

> 元素节点的类型，只读。
> 用来判断元素节点类型。

* 元素节点：对应的值为`1`
* 属性节点：对应的值为`2`
* 文本节点：对应的值为`3`
* 注释节点：对应的值为`8`
* Document：对应的值为`9`
* Doctype ：对应的值为`10`
* DocumentFragment：对应的值为`11`

```js
console.log(document.nodeType); //"9
console.log(document.body.nodeType); //1
console.log(document.firstChild.nodeType); //10
```

* `attributes`：元素节点的属性集合。

### 节点的方法

* `Node.hasChildNodes()`：判断元素节点是否包含子节点，返回`Boolean`。

## 综合练习

### 获取指定节点，并打印出该节点下所有的元素子节点(不可用children方法)

```html
<div>
  <ul>
    <li>1</li>
    <li>2</li>
    <li>3</li>
  </ul>
  <div>demo</div>
  <strong>111</strong>
</div>
<script>
  Element.prototype.myChildren = function () {
    var temp = {
        length: 0,
        push: Array.prototype.push,
        splice: Array.prototype.splice
      },
        child = this.childNodes,
        len = child.length;
    for (var i = 0; i < len; i++) {
      if (child[i].nodeType === 1) {
        temp.push(child[i]);
      }
    }
    return temp;
  }

  var div = document.getElementsByTagName('div')[0];
  console.log(div.myChildren());
</script>
```

### 封装一个方法，求指定元素的第n层的父节点

```html
<div>
  <strong>
    <span>
      <b>
        <i></i>
      </b>
    </span>
  </strong>
</div>
<script>
  function retParent(elem, n) {
    //先求一层，再求一层，递归调用；
    while (elem && n) {
      elem = elem.parentElement;
      n--;
    }
    return elem;
  }

  var i = document.getElementsByTagName('i')[0];
  console.log(retParent(i, 5));
</script>
```

### 封装一个方法：返回元素e的第n个兄弟元素节点。如果n为正数返回后面的兄弟元素；n为负数返回前面的；为0则返回自己。

```html
<div>
  <address></address>
  <strong></strong>
  <span></span>
  <b></b>
  <i></i>
  <p></p>
</div>
<script>
  function retSibling(e, n) {
    while (e && n) {
      if (n > 0) {
        if (e.nextElementSibling) {
          e = e.nextElementSibling;
        } else {
          for (e = e.nextSibling; e && e.nodeType != 1; e = e.nextSibling);
        }
        n --;
      } else {
        if (e.previousElementSibling) {
          e = e.previousElementSibling;
        } else {
          for (e = e.previousSibling; e && e.nodeType != 1; e = e.previousSibling);
        }
        n ++;
      }
    }
    return e;
  }

  var span = document.getElementsByTagName('span')[0];
  console.log(retSibling(span, 3)); //<p></p>
</script>
```

本节完。
