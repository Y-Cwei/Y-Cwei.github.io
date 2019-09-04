1. 下载 `BlueLake` 主题

		$ git clone https://github.com/chaooo/hexo-theme-BlueLake.git themes/BlueLake

	+ 官网下载完需要改一些配置项，考虑修改需要一定时间，故而可以直接拷贝 localTheme 目录下的 BlueLake 到 themes 里即可。  
	+ 使用 mklink 建立 themes 和 localTheme 符号链接，保证对 thems 目录下的文件的修改能同步到 localThem 里。  
	+ 语法：  

			rmdir /S E:\githubPages\noteStudy\localTheme
			mklink /D E:\githubPages\noteStudy\localTheme E:\githubPages\noteStudy\themes
			
			注： windows7 需要以管理员身份运行
	
2. 下载依赖

		$ npm install hexo-renderer-jade@0.3.0 --save
$ npm install hexo-renderer-stylus --save

3. 添加本地搜索

		$ npm install hexo-generator-json-content@2.2.0 --save