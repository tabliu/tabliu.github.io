---
title: Nodejs 学习笔记(1) - 初识Nodejs
date: 2018-09-26 16:30:00
tags:
  - code
  - nodejs
  - note
---

## Nodejs定义

> Nodejs 是一个基于 `Chrome V8` 引擎的 `JavaScript` 运行环境。
> Nodejs 使用了一个事件驱动、非阻塞式 `I/O` 的模型，使其轻量又高效。
> Nodejs 的包管理器 `npm` 是全球最大的开源生态系统。

## Nodejs的特点

* 使用 `JavaScript`；
* 速度非常的快(V8引擎 & non-block)；
* Nodejs的包管理器 `npm`，是全球最大的开源生态系统。

## JavaScript引擎(Engines)

* 电脑根本不识别也不理解javaScript代码；
* JavaScript引擎的作用就是让电脑识别JS代码。

## 模块(Module)

> 在Nodejs中，文件和模块是一一对应的（每个文件被视为一个独立的模块）。
> Nodejs中有一个简单的模块加载系统。

### 模块的定义

### Node事件模块

#### Events

> 大多数`Node.js`核心API都是采用惯用的异步事件驱动架构（fs/http）；
> 所有能触发事件的对象都是EventEmitter类的实例。
> 事件流程：引入模块 -> 创建EventEmitter对象 -> 注册事件 -> 触发事件。
