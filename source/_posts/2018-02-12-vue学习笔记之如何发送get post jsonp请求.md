---
title: vue发送get post jsonp请求
date: 2018-02-12 19:13:13
tags: vue
---
#### 1. 使用 [vue-resource](https://github.com/pagekit/vue-resource) 发送get、 post、 jsonp请求
> 官方已不再维护

[github提供的vue-ersource请求测试地址](https://github.com/typicode/json-server)

+ 使用 vue-resource 发送 get 请求

<!--more-->

	this.$http.get('/someUrl').then(response => {
		// get body data
		console.log(response.body)

	}, error => {
		// error callback

	})

+ 使用 vue-resource 发送 post 请求

		this.$http.post('/someUrl', {body}, {config}).then(response => {
			// 默认发起的 post 请求，默认没有表单格式，所以有的服务器处理不了
			// 通过查阅文档发现，通过 post 方法的第三个参数，{ emulateJSON：true }设置提交的内容为普通表单数据格式
			console.log(response.body)
	
		}, error => {
			// error callback
		})
	
	
+ 使用 vue-resource 发送 jsonp 请求

		this.$http.jsonp('/someUrl', {config}).then(response => {
			console.log(response.body)
	
		}, error => {
			// error callback
		})