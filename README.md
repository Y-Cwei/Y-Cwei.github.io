1. 下载 `BlueLake` 主题

		$ git clone https://github.com/chaooo/hexo-theme-BlueLake.git themes/BlueLake

	+ 官网下载完需要改一些配置项，考虑修改需要一定时间，故而可以直接拷贝 localTheme 目录下的 BlueLake 到 themes 里即可。  
	
2. 下载依赖

		$ npm install hexo-renderer-jade@0.3.0 --save
		$ npm install hexo-renderer-stylus --save

3. 添加本地搜索

		$ npm install hexo-generator-json-content@2.2.0 --save