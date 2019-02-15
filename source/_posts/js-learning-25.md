---
title: JavaScript学习笔记（25）- 正则表达式
date: 2018-07-05 10:10:10
tags:
  - code
  - javascript
  - note
---

> ES6中提供了一种全新的增强字面量的方法，接下来简单的介绍下关于增强字面量的使用。

## 正则表达式的定义

## 正则表达式的属性

### `g`

> 全局匹配

```js
let reg = /^a/g; //^ 以给定的代码开头
let str1 = 'abc123'

console.log(reg.test(str1)) // true
console.log(reg.test(str1)) // false
```

## 正则表达式常用方法

### test()

> 返回正则表达式匹配的结果；
> 如果匹配以数组形式返回，不匹配返回`null`。

```js
let reg = /^a/; //^ 以给定的代码开头
let str1 = 'abc123'
let str2 = '123abc'

console.log(reg.test(str1)) // true
console.log(reg.test(str2)) // false
```

### exea()

> 返回正则表达式匹配的结果；
> 如果匹配以数组形式返回，不匹配返回`null`。

```js
let reg = /^a/; //^ 以给定的代码开头
let str1 = 'abc123'
let str2 = '123abc'

console.log(reg.exec(str1)); // ["a", index: 0, input: "abc123", groups: undefined]
console.log(reg.exec(str2)); // `null`
```

## 和正则表达式结合的方法

### replace()

> 属于字符串方法；
> 根据给定的正则规则将匹配结果替换为指定值。

```js
/**
 * 应用：将中划线链接的字符串转化为小驼峰式
 * 'get-element-by-id' => 'getElementById'
 */

let str = 'get-element-by-id';
let reg = /-\w/g;

str.replace(reg, function($) {
  console.log($);
  return $.slice(1).toUpperCase();
}); //第一个是规则，第二个是要替换的内容
```

本节完。
