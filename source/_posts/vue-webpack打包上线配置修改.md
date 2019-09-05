---
title: vue+webpack打包上线配置修改
date: 2019-09-05 10:32:57
tags: vue
---

#### 1. 打包后的项目运行空白问题
打包上线后，访问页面，发现页面空白，因为我们部署的时候，我们项目在服务器的相对目录上，二打包后生成的文件，资源为根目录，故而找不到文件， `config/index.js ` 做如下调整即可：

	// 修改 assetsPublicPath 的值 '/' -> './'
	assetsPublicPath: './', // 修改前为 assetsPublicPath: '/',
	// 修改 productionSourceMap 的值 true -> false
	productionSourceMap: false, // 修改前为 productionSourceMap: true,
	
修改结果如图:  
![运行空白问题](/img/vue/01.png)

#### 2. css中使用 background: url() 引入图片不显示
打包上线后，css中使用background: url() 引入的图片路径错误，不显示，调整 `build/utils.js` 文件如下：

	// 修改前
	return ExtractTextPlugin.extract({
	  use: loaders,
	  fallback: 'vue-style-loader'
	})

	// 修改后
	return ExtractTextPlugin.extract({
	  use: loaders,
	  fallback: 'vue-style-loader',
	  publicPath: '../../' // 新增背景图片阈值
	})

修改后：  
![css中使用 background: url() 引入图片不显示问题](/img/vue/02.png)

#### 3. 打包上线
配置修改完毕，执行 `npm run build` 对项目进行上线前打包处理，当执行npm run build后，会在项目根目录生成一个dist文件，dist目录内文件为打包上线的最终文件。  

注： 当 npm run build 命令执行完毕，会有一个如下的tip提示：  
![打包后提示](/img/vue/03.png)
该tip意在告诉开发人员，打包生成的文件，要放到服务器上才能正常访问，无法直接通过浏览器来访问打包后的页面，若想自行测试，可通过node中的express来测试打包项目是否正常！

#### 4. express如何测试打包文件
+ `npm i express –g` ,执行当前命令，会下载最新版express4.0版本，该版本已将express的命令工具分离，还需下载安装对应的命令工具，键入:
`npm i express-generator –g` 下载，或 `npm i express@版本号 -g` 下载低版本express即可。  
+ 下载完成，使用express创建一个工程： express yourprojectname(自定义项目名)
+ 进入项目主目录： cd yourprojectname 
+ 下载安装所需依赖： npm install 
+ 启动程序： npm start 
> 注： 当命令行出现 node ./bin/www 证明程序已经开始启动，稍等不报错即可通过http://localhost:3000来访问当前项目public目录下的index.html文件，但开发者似无感知，可通过修改bin/www文件来给开发者一个成功启动程序的日志提示，修改方式如下：  

		// server.listen(port);
		server.listen(port,function(){
			console.log('Your application is running here:   http://localhost:'+port);
		});  


+ 修改完成，当执行npm start程序启动成功后，控制台会打印输出 `Your application is running here:  http://localhost:3000` ，表明程序启动成功。

+ 将打包好的dist文件，拷贝到此项目public目录下，启动程序，通过： `http://localhost:3000/dist` 即可访问打包的文件，可自测打包文件是否完整。

