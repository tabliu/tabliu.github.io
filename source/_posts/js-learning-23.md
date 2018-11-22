---
title: JavaScript学习笔记（23）- DOM的基本操作
date: 2018-11-21 16:00:00
tags:
  - code
  - javascript
  - note
---

> JavaScript学习笔记。本笔记是基于在腾讯课堂《[Web前端开发之JavaScript精英课堂【渡一教育】](https://ke.qq.com/course/231577)》课程学习过程中记录的一些提纲和关键点。
> 强烈推荐想要进行js入门学习来听听，尤其是前面姬成讲的基础知识点。

## 增加节点操作

### document.createElement()

> 创建元素节点；
> 创建后并不会添加到DOM中去，需要通过`appendChild()`方法来插入；

```js
var div = document.createElement('div');
div.innerHTML = 123;
document.body.appendChild(div);
```

### document.createTextNode()

> 创建文本节点；
> 创建后并不会添加到DOM中去，同样需要通过`appendChild()`方法来插入；

```js
var div = document.createElement('div');
var text = document.createTextNode('Hello World!');
div.appendChild(text);
document.body.appendChild(div);
```

### document.createComment()

> 创建注释节点；
> 创建后并不会添加到DOM中去，同样需要通过`appendChild()`方法来插入；

```js
var div = document.createElement('div');
var text = document.createTextNode('Hello World!');
var comment = document.createComment('this is comment');
div.appendChild(text);
div.appendChild(comment);
document.body.appendChild(div);
```

## 修改节点操作

### appendChild()方法

> 语法为：`PARENTNODE.appendChild(node)`，插入元素`node`到父级元素`PARENTNODE`的最后位置；
> 对DOM中已经存在的节点使用`appendChild()`方法相当于是剪切操作；

```js
var div = document.createElement('div');
var span = document.createElement('span');
var text = document.createTextNode('Hello World!');
var comment = document.createComment('this is comment');
document.body.appendChild(div);
div.appendChild(text);
div.appendChild(span);
span.appendChild(comment);
span.appendChild(text);
```

### insertBefore()方法

> 语法为：`PARENTNODE.insertBefore(a, b)`，相当于是`PARENTNODE` `inset` `a` `before` `b`；
> 对DOM中已经存在的节点使用`insertBefore()`方法也相当于是剪切操作，前提是指定的节点必须存在。

```js
var div = document.createElement('div');
var span = document.createElement('span');
var text = document.createTextNode('Hello World!');
var comment = document.createComment('this is comment');
document.body.appendChild(div);
div.appendChild(text);
div.insertBefore(span, text);
div.insertBefore(comment, span);
```

## 删除节点操作

### removeChild()方法

> 语法为：`parent.removeChild(child)`，相当于`parent`对`child`执行剪切操作。

```js
var div = document.createElement('div');
var span = document.createElement('span');
var text = document.createTextNode('Hello World!');
var comment = document.createComment('this is comment');
document.body.appendChild(div);
div.appendChild(text);
div.appendChild(span);
span.appendChild(comment);
span.appendChild(text);
div.removeChild(span);
```

### remove()方法

> 语法为：`child.remove()，这是`ES5`的新语法，对自身执行删除方法，返回`undefined`。

```js
var div = document.createElement('div');
var span = document.createElement('span');
var text = document.createTextNode('Hello World!');
var comment = document.createComment('this is comment');
document.body.appendChild(div);
div.appendChild(text);
div.appendChild(span);
span.appendChild(comment);
span.appendChild(text);
text.remove();
comment.remove();
div.remove();
```

## 替换节点操作

### replaceChild()方法

> 语法为：`PARENTNODE.replaceChild(new, origin)，相当于`PARENTNODE`用新节点`new`替换原有子节点`origin`；
> 完成后返回原有子节点`origin`，相当于把子节点`origin`进行了剪切操作。

```js
var div = document.createElement('div');
var span = document.createElement('span');
var text = document.createTextNode('Hello World!');
var comment = document.createComment('this is comment');
document.body.appendChild(div);
div.appendChild(text);
div.appendChild(span);
span.appendChild(comment);
span.appendChild(text);
var p = document.createElement('p');
div.replaceChild(p, span);
```

## 元素节点的属性

### innerHTMl()

> 改变元素标签的内容，其内容是`HTML`结构；
> 可读也可写。

```js
var div = document.createElement('div');
document.body.appendChild(div);
div.innerHTML = '123';
div.innerHTML += 'abc';
div.innerHTML += '<span style="color: #f00; font-size:20px;">红色</span>';
```

### innerText()

> 可以读写指定标签内的文本内容，包括标签所有内子标签的文本内容；
> 老版本火狐浏览器不兼容

```js
var div = document.createElement('div');
var p = document.createElement('p');
var span = document.createElement('span');
document.body.appendChild(div);
div.innerHTML = '123';
div.innerHTML += 'abc';
div.innerHTML += '<span style="color: #f00; font-size:16px;">红色</span>';
div.appendChild(p);
p.innerHTML = 'pppp';
p.appendChild(span);
span.innerHTML = 'span';
console.log(div.innerText); //123abc红色 ppppspan
div.innerText = 'div';
console.log(div.innerText); //div
```

注意：如果想要使用`innerText`属性，一定要确认是否包含子元素，否则会把所有元素内容的文本和子元素全部替换。

### textContent()

> 功能同`innerText`，火狐浏览器兼容，低版本IE浏览器不兼容。

## 元素节点的方法

### setAttribute()

> 写操作，给指定标签添加属性和属性内容。
> 可以自定义系统没有提供的属性

```js
var div = document.createElement('div');
var p = document.createElement('p');
var span = document.createElement('span');
document.body.appendChild(div);
div.setAttribute('id', 'demo');
div.setAttribute('class', 'class1');
console.log(div); //<div id="demo" class="class1"></div>
```

### getAttribute()

> 读操作，返回标签某个属性的属性值。
> 可以自定义系统没有提供的属性

```js
var div = document.createElement('div');
var p = document.createElement('p');
var span = document.createElement('span');
document.body.appendChild(div);
div.setAttribute('id', 'demo');
div.setAttribute('class', 'class1');
console.log(div.getAttribute('id')); //demo
console.log(div.getAttribute('class')); //class1
```

## 综合练习

### 封装一个`insertAfter()`方法，功能类似于`insertBefore()`

```html
<div>
  <p>123</p>
  <strong>strong</strong>
</div>
<script>
  Element.prototype.insertAfter = function (target, origin) {
    // var sibling = origin.nextElementSibling;
    // if (sibling == null) {
    //   this.appendChild(target);
    // } else {
    //   this.insertBefore(target, sibling);
    // }

    if (origin == this.lastElementChild) {
      this.appendChild(target);
    } else {
      this.insertBefore(target, origin.nextElementSibling)
    }
  }

  var div = document.getElementsByTagName("div")[0];
  var p = document.getElementsByTagName("p")[0];
  var strong = document.getElementsByTagName("strong")[0];
  var span = document.createElement('span');
  span.innerHTML = 'span';
  div.insertAfter(span, p);
  document.body.insertAfter(span, div);
</script>
```

### 对指定标签组进行标签逆序操作

```html
<div>
  <p>123</p>
  <strong>strong</strong>
</div>
<script>
  Element.prototype.insertAfter = function(target, origin) {
    // var sibling = origin.nextElementSibling;
    // if (sibling == null) {
    //   this.appendChild(target);
    // } else {
    //   this.insertBefore(target, sibling);
    // }

    if (origin == this.lastElementChild) {
      this.appendChild(target);
    } else {
      this.insertBefore(target, origin.nextElementSibling);
    }
  };

  Element.prototype.reverseElement = function() {
    var children = this.children;
    var len = children.length;
    for (var i = len - 2; i > -1; i --) {
      this.appendChild(children[i]);
    }
  };

  var div = document.getElementsByTagName('div')[0];
  var p = document.getElementsByTagName('p')[0];
  var strong = document.getElementsByTagName('strong')[0];
  var span = document.createElement('span');
  span.innerHTML = 'span';
  div.insertAfter(span, p);
  div.reverseElement();
</script>
```

本节完。
