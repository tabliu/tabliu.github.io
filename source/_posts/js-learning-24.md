---
title: JavaScript学习笔记（24）- 日期对象-Date()
date: 2018-11-22 09:56:00
tags:
  - code
  - javascript
  - note
---

> JavaScript学习笔记。本笔记是基于在腾讯课堂《[Web前端开发之JavaScript精英课堂【渡一教育】](https://ke.qq.com/course/231577)》课程学习过程中记录的一些提纲和关键点。
> 强烈推荐想要进行js入门学习来听听，尤其是前面姬成讲的基础知识点。

## 日期对象的定义

> 系统提供的返回当天的日期和时间的方法；
> 包含了一些列方法；

```js
var date = new Date();
console.log(date); //Thu Nov 22 2018 09:59:56 GMT+0800 (中国标准时间)
```

注释：`Date()`对象会自动把当前日期和时间保存为其初始值

## 日期对象的属性

### constructor

> 返回对创建此对象的`Date()`函数的引用。

### prototype

> 使您有能力向对象添加属性和方法。

## 日期对象的方法

### Date()

> 返回当日的日期和时间；
> 返回类型是字符串。

```js
console.log(Date()); //"Thu Nov 22 2018 09:59:56 GMT+0800 (中国标准时间)"
```

本节完。
