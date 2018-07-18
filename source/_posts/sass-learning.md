---
title: Sass Learning
date: 2017-09-28 10:50:18
tags:
    - code
    - note
    - scss
---

学习sass时整理的笔记，记录下来，方便随时巩固。

## 前言

* 页面常用css处理方式
* 当前存在哪些不方便的地方
* 如果改进？

## 引入

* 什么是css预处理器

> 通俗的说，CSS 预处理器用一种专门的编程语言，进行 Web 页面样式设计，然后再编译成正常的 CSS 文件，以供项目使用。

* 常见css预处理器有哪些
    * sass|scss
    * less
    * stylus
    * ...
    
* 各有什么不同
* 为什么用css预处理器
    * 更加简洁
    * 适应性更强
    * 可读性更佳
    * 更易于代码维护和更新

## 介绍
* 了解sass的两种不同格式：.sass和.scss；
* 了解编译环境，最简单的就是在dos下的sass直接编译，或者用compass，gulp、webpack、UEB等等；
* 了解各种编译环境的操作命令和各种参数；

## 安装
* sass安装需要依赖ruby，所以先要安装ruby，在官网下载直接next，完成后在ruby目录下打开任务窗，键入`ruby -v`查看是否安装成功。
* 如果ruby安装成功，直接使用gem的方式安装sass，` gem install sass`  默认是用墙外的程序，如果安装失败，可以使用淘宝的镜像安装；键入`sass -v`查看是否安装成功。
* 顺便可以把compass安装，` gem install compass`   ，后期可能会用到。

## 入门

* 注释：//和/**/
    * // XXXXXXXXXXXXXXX
    * //----------------------------
    * /* XXXXXXXXXXXXXXX
    * *************************/

* 语法
* 变量
    * 普通变量：`$bgColor:#fff;         body {background-color:$bgColor;}`
    * 默认变量：`$bgColor:#fff !default;         body {background-color:$bgColor;}`
    * 特殊变量：#{$variables} --进阶
    * 多值变量：list和map -- 高级

* 嵌套
    * 选择器嵌套
    * 属性嵌套
    * 伪类、伪元素嵌套

* 选择器(&)
    * &

* 编译
    * sass
    * compass
    * kola
    * gulp
    * webpack
    * UEB
    * ...

* 编码规范
    * 遵循css的规范
    * 拒绝使用ID
    * 避免不必要的嵌套和多层嵌套



## 进阶

* 运算
    * 加法(+)：不允许不同单位间相加；
    * 减法(-) ：同上；
    * 乘法(*) ：只允许出现一个单位；
    * 除法(/) ：同上；要用()括起来；
    * 颜色运算：`p {color: #010203 + #040506;}  p {color: #112233 * 2;}`
    * 字符运算：`$content: "Hello" + "" + "Sass!";`

* 混合
    * 定义：@mixin
    * 调用：@include
    * 常规
    * 带参数
    * 带多个参数
    * 结合函数或者循环的片段
    * 优势与不足

* 继承
    * 调用：@extend
    * 优势与不足

* 占位
    * 定义：%
    * 调用：@extend
    * 优势与不足
 
* 导入：@import '';
* &  or  @at-root #{&} 。&和#{&}区别：&代表源选择器，也当作一种标签选择器，可以继承，但只能放在开始位置；#{&} 可以引用父（引用父选择器）和插值，可以嵌套，也可以放在任何位置。

## 高级

* 值列表
    * nth函数（nth function） 可以直接访问值列表中的某一项；
    * join函数（join function） 可以将多个值列表连结在一起；
    * append函数（append function） 可以在值列表中添加值；
    * @each规则（@each rule） 则能够给值列表中的每个项目添加样式。

* 函数
* 条件判断
    * 三目判断

* 循环
    * @for循环
    * @each循环
    * list循环
    * map数据循环

## 常见问题
* gbk和utf-8
* 中文路径的问题

## 参考内容
* [SASS用法指南](http://www.ruanyifeng.com/blog/2012/06/sass.html)
* [CSS预处理器——Sass、LESS和Stylus实践【未删减版】](http://www.w3cplus.com/css/css-preprocessor-sass-vs-less-stylus-2.html)
* [Sass和Compass入门](http://www.cnblogs.com/yizihan/p/4427900.html)



