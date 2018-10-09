---
title: JavaScript学习笔记（19）- 类数组
date: 2018-08-16 09:29:00
tags:
  - code
  - javascript
  - note
---

> JavaScript学习笔记。本笔记是基于在腾讯课堂《[Web前端开发之JavaScript精英课堂【渡一教育】](https://ke.qq.com/course/231577)》课程学习过程中记录的一些提纲和关键点。
> 强烈推荐想要进行js入门学习来听听，尤其是前面姬成讲的基础知识点。

## 类数组的定义

> 具有数组属性和方法的一组数据的集合。
> 通常是一个对象的形式。

## 类数组的组成

* 属性要为索引（数字）属性；
* 必须有`length`属性；
* 最好加上`push`方法的属性；

```javascript

var obj = {
  "0" : 'a',
  "1" : 'b',
  "2" : 'c',
  "length" : 3,
  "push" : Array.prototype.push,
  "splice" : Array.prototype.splice
}

Arrary.prototype.push = function(target) {
  this[this.length] = target;
  this.length ++;
}
```

看一个实例：

```javascript

var obj = {
  "2" : 'a',
  "3" : 'b',
  "length" : 2,
  "push" : Array.prototype.push
}

obj.push('c');
obj.push('d');
console.log(obj); //obj{2: "c", 3: "d", length: 4, push: Array.prototype.push}
```

类数组的关键点是使用`length`的特性来取值和赋值并改变`length`的值。

## 类数组的应用

> 具有数组和对象的特性，可以把数组和对象的属性都拼在一起拿来用。
> 并不是所有的方法都可以用，如果要使用数组的方法，需要手动添加进来。

```javascript
var obj = {
  "0" : 'a',
  "1" : 'b',
  "3" : 'c',
  name : 'abc',
  age : 123,
  sex : 'male',
  length : 3,
  push : Array.prototype.push,
  splice : Array.prototype.splice
};

console.log(obj);
```

## 课后作业

### 封装一个`type`方法，能够准确的判断各个目标对象的类型

```javascript
//typeof([]) --> array
//typeof({}) --> object
//typeof(function) --> function
//typeof(new Number()) --> number Object
//typeof(123) --> number

```

基本思路：

1. 先区分原始值和引用值；
2. 针对引用值中的`object`类型进行区分；

```javascript
function type(target) {
  var tof = typeof(target),
      toStr = Object.prototype.toString.call(target),
      tempObj = {
        "[object Array]" : 'array',
        "[object Object]" : 'object',
        "[object Boolean]" : 'boolean - object',
        "[object Date]" : 'date - object',
        "[object Number]" : 'number - object',
        "[object RegExp]" : 'regExp - object',
        "[object String]" : 'string - object'
      };

  if (target === null) {
    return 'null';
  } else if (tof == 'object') {
    return tempObj[toStr];
  } else {
    return tof;
  }
}
```

这里一定要注意`==`和`===`的区别使用。

### 数组去重，要求在原型链上编程

```javascript
Array.prototype.unique = function() {

}

var arr = [1,1,2,3,3,'a','b','a',undefined];
console.log(arr.unique()); //arr[1,2,3,'a','b',undefined]

```

思路：利用对象的一个特性：`同一个属性名不可以多个同名属性`。

1. 新建一个数组，把数组的各个值当作对象的属性名并随意赋值；
2. 判断数组中的属性是否有值，如果是`undefined`则说明没有值，进行赋值后再逐个去重；
3. 把去重后的对象的属性名形成一个全新的数组并返回。

```javascript
Array.prototype.unique = function () {
  var temp = {},
      arr = [],
      len = this.length;
  for (var i = 0; i < len; i++) {
    if (!temp[this[i]]) {
      temp[this[i]] = 'flag';
      arr.push(this[i]);
    }
  }
  return arr;
}

var arr = [1,1,2,3,3,'a','b','a',undefined];
console.log(arr.unique()); //arr[1,2,3,'a','b',undefined]
```

本题中一定要理解`this`的作用和和`this[i]`的使用。

### 一个字符串由[a-z]组成，请找出该字符串第一个只出现一次的字母

### 字符串去重

思路：利用之前完成的数组去重的方法，先把字符串转成数组，去重后再转回字符串。

```js
Array.prototype.unique = function () {
    var temp = {},
        arr = [],
        len = this.length;
    for (var i = 0; i < len; i++) {
        if (!temp[this[i]]) {
            temp[this[i]] = 'flag';
            arr.push(this[i]);
        }
    }
    return arr;
}

function strUnique(str) {
    // var arr = str.split("");
    // var temp = arr.unique();
    // var str = temp.join("");
    return str.split("").unique().join("");
}

var str = 'dfasdafsfdsfsfdsf';
strUnique(str); //'dfas'
```

本节完。
