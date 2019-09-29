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
	
#### 2.axios请求超时的处理,超时重新发送

	// 重发次数
	axios.defaults.retry = 10
	// 间隔时间
	axios.defaults.retryDelay = 1000
	axios.interceptors.response.use(undefined, function axiosRetryInterceptor (err) {
	  var config = err.config
	  if (!config || !config.retry) return Promise.reject(err)
	  config.__retryCount = config.__retryCount || 0
	  if (config.__retryCount >= config.retry) {
	    return Promise.reject(err)
	  }
	  config.__retryCount += 1
	  var backoff = new Promise(function (resolve) {
	    setTimeout(function () {
	      resolve()
	    }, config.retryDelay || 1)
	  })
	  return backoff.then(function () {
	    return axios(config)
	  })
	})	
	
#### 3.axios设置请求超时,弹窗提示

	axios.defaults.timeout = 10 * 1000
	axios.interceptors.response.use(undefined, error => {
	  var originalRequest = error.config
	  if (error.code === 'ECONNABORTED' && error.message.indexOf('timeout') !== -1 && !originalRequest._retry) {
	    MessageBox.alert('网络请求超时,请稍后进行尝试', '移动OA提示')
	  }
	})	
	