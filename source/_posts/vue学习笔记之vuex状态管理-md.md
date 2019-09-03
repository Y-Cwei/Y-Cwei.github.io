---
title: vue学习笔记之vuex状态管理
date: 2019-03-28 21:43:36
categories: vue
tags: [vue, vuex]
---

#### vue学习笔记之vuex状态管理
> vuex用于状态管理，使用如下  

##### 1. 项目中引入vuex

	npm i vuex -s
	
* 1.1 下载安装完成，修改项目src/main.js，引入 `vuex`   

		imoprt Vuex from 'vuex' // 引入Vuexœ
		
		Vue.use(Vuex) // 声明使用Vuex