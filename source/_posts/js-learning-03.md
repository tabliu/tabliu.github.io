---
title: JavaScript学习笔记（3）- 运算符
date: 2018-06-30 16:10:00
tags:
  - code
  - javascript
  - note
---

> JavaScript学习笔记。本笔记是基于在腾讯课堂《[Web前端开发之JavaScript精英课堂【渡一教育】](https://ke.qq.com/course/231577)》课程学习过程中记录的一些提纲和关键点。
> 强烈推荐想要进行js入门学习来听听，尤其是前面姬成讲的基础知识点。

## 运算操作符

* "+"

  * 数学+运算
  * 字符串链接
  * 任何类型 + 字符串的结果都是字符串

* "-"、"*"、"/"、"%"、"="、"()"
* 优先级："()"最高，"="最弱
* "++"、"--"、"+="、"-="、"/="、"*="、%="
* Infinity  、 NaN

赋值的顺序：自右向左；计算的顺序：自左向右。

## 比较运算符

* ">"、"<"、">="、"<="、"=="、"!="
* 比较结果为`boolean`值；
* 字符串比较的是按位`ASCII`码的`Unicode`值的大小；
* `NaN`不等于任何东西，包括自己。

```js
console.log(2 > 123); //false
console.log('2' > '123'); //true
```

## 逻辑运算符

* "&&"、"||"、"!"
* undefined、null、NaN、""、0、false ==> false
* 先看第一表达式转化为布尔值的结果，如果结果为真，那么它会看第二表达式转化为布尔值的结果；然后如果只有两个表达式的话，只看到第二个表达式，就可以返回该表达式的值了。
* && 结果为假输出假 || 结果为真输出真。

```js
var n;
console.log(n == null); //true;
console.log(11 == '11'); //true;
console.log(0 == false); //true;
```

## 全等/不全等比较：`===`和`!==`

> 不带隐式转化的比较；
> 不希望在比较运算的时候进行隐式转化就需要用到全等或者不全等。

```js
var n;
console.log(n === null); //false;
```

## `isNaN`

> `isNaN(num)`函数用于检查其参数是否是非数字值，返回布尔值；
> 如果`num`是特殊的非数字值 NaN（或者能被转换为这样的值 ）返回的值就是`true`；如果`num`是其他值则返回f`alse`；
> 如果`num`的值不是数字类型，会隐式转换为数字类型再比较；
> isNaN(num)函数通常用于检测`parseFloat()`和`parseInt()`的结果，以判断它们表示的是否是合法的数字。当然也可以用`isNaN()`函数来检测算数错误，比如用`0`作除数的情况。

```js
isNaN(NaN); // true
isNaN('true'); //true
isNaN(undefined); //true
isNaN({}); // true
isNaN(null); //false
isNaN(12); // false
isNaN(false); //false
isNaN(true);// false

// strings
isNaN('12'); // false: "12"、 “12”将转换为一个数字12，所以不是一个NAN；
isNaN('12.12'); // false: "12.12" 将被转换为 12.12 ，所以不是一个 NaN
isNaN("12ABC"); // true: 字符串'12ABC' parseInt('12ABC')方法后为 12，但是这里是使用Number()方法， Number('12ABC')得到的结果为NaN
isNaN(''); // false: 空字符串将被转换为数字0，0不是一个NAN，所以为false
isNaN(' '); // false: 一个空格的字符串被转换为数字0，0不是一个NAN，所以为false

// dates
isNaN(new Date()); // false
isNaN(new Date().toString()); // true // 这里使用了toString()方法转为字符串，所以非常肯定的得出它是一个NAN;
```

> 综述：从上面的案例可以看出，`isNAN()`方法括号内的内容如果是字符串数据类型，或者布尔类型，将会使用`Number()`方法转换数字类型，如果转换后为`NAN`则返回`true`，反之则返回`false`；


参考: [http://www.cnblogs.com/dreamworker6/p/7382131.html](http://www.cnblogs.com/dreamworker6/p/7382131.html)
