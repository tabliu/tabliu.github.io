---
title: JavaScript学习笔记（5）- 类型转换
date: 2018-07-02 18:10:00
tags:
  - code
  - javascript
  - note
---

> JavaScript学习笔记。本笔记是基于在腾讯课堂《[Web前端开发之JavaScript精英课堂【渡一教育】](https://ke.qq.com/course/231577)》课程学习过程中记录的一些提纲和关键点。
> 强烈推荐想要进行js入门学习来听听，尤其是前面姬成讲的基础知识点。


## 类型转换

### 显式类型转换

* Number(mix)：把数据类型转换为数字。包括任何类型：`Number => Number`、`String => Number or NaN`、`Boolean => 0 or 1`、`Null => 0`、`Undefined => NaN`；
* ParseInt(string, radix)：把字符串根据基数转换为十进制整型；没有基数按照十进制转。
* ParseFloat(string)：把字符串转换为浮点型。
* String(mix)：
* Boolean()：
* toString()：`Undefined`和`Null`不能使用；
* toString(radix)：把目标进制根据指定基数进行转换；

### 隐式类型转换

* isNaN()：先把指定类型通过`Number(mix)`进行转换，然后和`NaN`进行比对：`Number(mix) => NaN`；
* "++" "--"或者"+" "-"：都会转换为`Number`类型；
* "+"：左右只要有一个类型是`String`就会转换为`String`；
* "-" "*" "/" "%"：转换为`Number`；
* ">" "<" ">=" "<="：`Undefined \Null` 特殊；
* "&" "||" "!"：`Boolean`类型转换；
* "=="   "!=="：  `NaN == NaN => false`；
* "==="   "!=="：绝对等于或者不等于；

### typeof

* 返回6种类型：`String`、`Number`、`Object`、`Boolean`、`Function`和`Undefined`；
* 有且只有使用`typeof`加未定的变量不会报错。
