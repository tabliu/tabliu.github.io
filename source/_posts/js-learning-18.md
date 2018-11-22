---
title: JavaScript学习笔记（18）- 数组
date: 2018-08-14 08:18:00
tags:
  - code
  - javascript
  - note
---

> JavaScript学习笔记。本笔记是基于在腾讯课堂《[Web前端开发之JavaScript精英课堂【渡一教育】](https://ke.qq.com/course/231577)》课程学习过程中记录的一些提纲和关键点。
> 强烈推荐想要进行js入门学习来听听，尤其是前面姬成讲的基础知识点。

## 数组的定义

> 所谓数组，是有序的元素序列。
> 若将有限个类型相同的变量的集合命名，那么这个名称为数组名。

以上定义来源于百度百科。

## 数组的定义方法

### 字面量定义

```javascript
var arr = [];
var arr1 = [1,2,,,3,4] //稀松数组，length为6
```

### 系统构造式

```javascript
var arr = new Array(); //定义一个空数组
var arr1 = new Array(3); //定义一个包含3个元素的数组，元素为空
var arr2 = new Array(1,2,3); //定义一个包含元素1，2，3的数组
```

数组用到的所有方法全来源于`Array.prototype`。

使用`new Array()`方法创建数组`()`里可以直接传参数，但当参数只有一位且为数字时，表示为数组的长度`length`为该数字，而不是该数组第一位。

如果参数只有一位则***必须为正整数***。

```javascript
var arr = [6]; //arr.lenght = 1
var arr1 = new Array(6); //arr.lenght = 6
```

## 数组的属性

数组的常用属性是`length`，表示数组的长度。

```javascript
var arr = [1, 2, 3, 4];
console.log(arr.length); //arr.lenght = 4
var arr1 = new Array(6);
console.log(arr1.length); //arr.lenght = 6
```

`length`属性，既可以通过`arr.length`来读取，也可以改变赋值，从而改变原数组。

```javascript
var arr = [1, 2, 3, 4];
console.log(arr.length); //arr.lenght = 4
arr.length = 6;
console.log(arr); //arr变成一个新数组，相当于 arr = [1, 2, 3, 4,,,]
arr.length = 0;
console.log(arr); //arr变成一个空数组，相当于 arr = []
arr.length = 6;
console.log(arr); //arr变成一个6位的空值数组，相当于 arr = new Array(6)
```

## 数组的操作

### 数组的读

```javascript
var arr = [];
console.log(arr[10]); //undefined
```

数组的读操作在正常情况下基本不会报错，除非执行了其不存在的方法。

### 数组的写

```javascript
var arr = [];
arr[10] = 'abc';
console.log(arr); // arr[,,,,,,,,,'abc']
console.log(arr.length); //11
```

总结：

> 数组不可以溢出读，返回为`undefined`；可以溢出写。

## 数组的常用方法

> 分为es3.0、es5.0和es6.0方法。

### 改变原数组的方法

#### push方法

> 在数组的最后位置添加数组。

```javascript
var arr = [];
arr.push(1,2,3); //arr[1,2,3]
Array.prototype.push = function() {
  for(i = 0; i < arguments.length; i ++) {
    this[this.length] = arguments[i];
  }
  return this.length;
}
```

模拟实现`push()`的方法：

```javascript
var arr = [];
Array.prototype.push = function() {
  for(i = 0; i < arguments.length; i ++) {
    this[this.length] = arguments[i];
  }
  return this.length;
}

arr.push(1,1,2,2);
```

#### pop方法

> 对数组的最后一位进行剪切操作。

```javascript
var arr = [1,2,3,4,5];
arr.pop(); //arr[1,2,3,4]
```

`pop()`方法是对数组最后一位进行剪切并返回原数组的长度，不支持添加参数，只能剪切一位。

#### shift方法

> 从数组的第一位开始进行删除操作。

```javascript
var arr = [1,2,3,4,5];
arr.shift(); //arr[2,3,4,5]
```

#### unshift方法

> 从数组的第一位开始进行添加操作，和`push()`方法类似，只是从第一位开始操作。

```javascript
var arr = [1,2,3,4,5];
arr.unshift(-1,0); //arr[-1,0,1,2,3,4,5]
```

#### reverse方法

> 对数组进行翻转操作并返回数组。

```javascript
var arr = [1,2,3,4,5];
arr.reverse(); //arr[5,4,3,2,1]
```

#### splice方法

> 对数组进行切片操作并返回截取数据组成的新数组。
> 参数使用：arr.splice(从第几位开始, 截取多少的长度, 在切口处添加新的数据)

```javascript
var arr = [1,1,2,2,3,3];
arr.splice(1,2); //[1,2]
console.log(arr); //arr[1,2,3,3]
```

如果不截取数据则在第二位添加`0`；从第三位可以添加无数个参数都视为添加的新数据。

```javascript
var arr = [1,1,2,2,3,3];
arr.splice(1,2); //[1,2]
console.log(arr); //arr[1,2,3,3]
arr.splice(3,1); //[3]
arr.splice(3,0,4,5,6); //[]
console.log(arr); //arr[1,2,3,4,5,6]
```

我们知道`splice()`方法第一个参数表示是从第几位开始，这个参数必须是整数且不能越界，但允许是负数，***负数值表示该数组的倒数第几位***。

其计算方法位：

```javascript
pos += pos >= 0 ? 0 : pos + this.length;
```

#### sort方法

> 对数组进行排序并返回数组。

```javascript
var arr = [3,0,4,5,6];
arr.sort(); //[0,3,4,5,6]
```

`sort()`方法默认是按照升序进行排列，但排序是按照ASKII码的大小进行比较。

```javascript
var arr = [1,2,4,0,10,5];
arr.sort(); //[0,1,10,2,4,5]
```

`sort()`方法预留接口可以自定义方法实现想要的功能。

自定义方法的说明：

* `function(){}`中必须要传入两个参数；
* 看方法的返回值：
  1、当返回值为***负数*** 时，那么***前面的数放在前面***；
  2、当返回值为***正数*** 时，那么***后面的数放在前面***；
  3、当返回值为***0*** 时，那么***该位置数不动***。

```javascript
var arr = [1,2,4,0,10,5];
arr.sort(function(a, b) {
  // 默认升序
  if (a > b) {
    return 1;
  } else {
    return -1;
  }

  // 降序
  if (a < b) {
    return 1;
  } else {
    return -1;
  }

  //简写为
  return a - b; // 升序
  return b - a; // 降序
});
```

练习：

给一个有序的数组乱序排列

```javascript
var arr = [1,2,3,4,5,6,7,8];
arr.sort(function(a, b) {
  return Math.random() - 0.5;
});
```

根据数组中对象的属性值进行排序

```javascript
var deng = {
  name : 'deng',
  age : 40,
  sex : 'male'
};
var cheng = {
  name : 'cheng',
  age : 18,
  sex : 'male'
};
var fang = {
  name : 'fang',
  age : 20,
  sex : 'female'
};

var arr = [deng, cheng, fang];

arr.sort(function(a, b) {
  return a.age - b.age;
});
console.log(arr);
```

根据数组中字符串的长度进行排序

```javascript
var arr = ['abss大多数', 'sj范德萨', 'djsfdsf大水法范德萨'];

arr.sort(function(a, b) {
  return a.length - b.length;
});
console.log(arr); //['sj范德萨', 'abss大多数', 'djsfdsf大水法范德萨']
```

根据数组中字符串的字节长度进行排序

```javascript
function getCharLength(str) {
 var num = str.length;
 for(var i = 0; i < str.length; i ++) {
   if(str.charCodeAt(i) > 255) {
     num ++;
   }
 }
 return num;
}

var arr = ['abss大多数', 'sj范德萨太帅了！！', 'djsfdsweeef大水法'];

arr.sort(function(a, b) {
  return getCharLength(a) - getCharLength(b);
});
console.log(arr); //["abss大多数", "djsfdsweeef大水法", "sj范德萨太帅了！！"]
```

### 不改变原数组的方法

#### concat方法

> 连接多个数组并返回一个新数组。

```javascript
var arr = [3,0,4,5,6];
var arr1 = [1,3,4,5,66,3];
arr.concat(arr1); //[3, 0, 4, 5, 6, 1, 3, 4, 5, 66, 3]
```

#### slice方法

> 截取数组的若干位，并将截取的数据返回一个新数组。
> 可以添加参数，且不同参数具有不同的意义。

两个参数：`slice(从该位开始截取, 截取到该位)`

```javascript
var arr = [3,0,4,5,6];
arr.slice(1,3); //[0,4]
```

一个参数：`slice(从该位开始截取一直截取到最后一位)`

```javascript
var arr = [3,0,4,5,6];
arr.slice(1); //[0,4,5,6]
```

没有参数：`slice()`从开始位截取到最后。

```javascript
var arr = [3,0,4,5,6];
arr.slice(); //[3,0,4,5,6]
```

#### join方法

> 通过给定的连接方式将数组的每一位连接起来并返回。
> 须有一个字符串的参数，没有连接方式用空串`''`或者`""`，如果不加默认以`,`方式进行连接。

```javascript
var arr = [3,0,4,5,6];
arr.join('-'); //'3-0-4-5-6'
```

`join()`方法返回的是一个字符串，在字符串里提供了一个`split()`方法可以将字符串通过给定的方式进行拆分组成一个新数组。

```javascript
var arr = [3,0,4,5,6];
var str = arr.join('-'); //'3-0-4-5-6'
var arr1 = str.split('-');
console.log(arr1); // ["3", "0", "4", "5", "6"]
```

练习：对多个字符串进行连接

```javascript
var str1 = 'tencent',
    str2 = 'alibaba',
    str3 = 'baidu';
var arr = [str1, str2, str3]
var strFanal = arr.join('/');
console.log(strFanal); //'tencent/alibaba/baidu'
```

本节完。
