---
title: node.js学习笔记之入门
date: 2019-04-15 20:51:35
tags: Node.js
---

### 1. Node.js介绍  
#### 1.1 Node.js是什么？
+ Node.js® is a JavaScript runtime built on Chrome's V8 JavaScript engine.
	+ node.js 不是一门语言
	+ node.js 不是库，不是框架
	+ node.js 是一个javascript运行时环境
	+ 简单来讲，Node.js可以解析和执行js代码
	<!-- more -->
	+ 以前只有浏览器可以解析之行javascript代码
	+ 也就是说现在的javascript可以完全脱离浏览器来运行，一切都归功于Node.js
	+ 构建于 Chrome 的 V8 引擎之上
		+ 代码只是具有特定格式的字符串而已
		+ 引擎可以认识它，可以帮你去解析和执行
		+ Google Chrome 的 V8 引擎是目前公认解析 JavaScript 最快的
		+ Node.js 之所以很快的原因之一是：Node.js的作者，将Google Chrome 中的 V8 引擎移植了出来，开发了一个独立的 javaScript 运行时环境
+ Node.js uses an event-driven, non-blockling I/O model that makes it leightweight and efficient
	+ event-driven 时间驱动
	+ non-blocking I/O model 非阻塞I/O模型（异步）
	+ lightweidht and efficient 轻量和高效

+ Node.js package ecosystem, npm is the largest ecosystem of open source libraiies in the word
	+ npm 是世界上最大的开源生态系统
	+ 绝大多数的javascript相关的包都放在了npm上，这样做的目的是让开发人员更方便的去开发使用它
	+ `npm install jquery`  

	
#### 1.2 	浏览器中的js和Node.js中的js的区别
+ 浏览器中的javascript
	+ EcmaScript
		+ 基本语法
		+ if
		+ var
		+ function
		+ Object
		+ Array
	+ BOM
	+ DOM
+ Node.js中的javascript
	+ 没有BOM,DOM(浏览器不操作BOM,DOM)
	+ EcmaScript
	+ 在Node这个javaScript执行环境中为javaScript提供了一些服务器级别的操作API
		+ 例如文件的读写
		+ 网络服务的构建
		+ 网络通信
		+ http服务器
		+ 等的处理...