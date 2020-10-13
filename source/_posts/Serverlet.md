---
title: Servlet
date: 2019-10-17 10:49:22
tags: JavaWEB
---
### 1. 什么是Servlet22?
+ Servlet是Sun公司制定的一套技术标准，包含与Web应用相关的一系列接口，是Web应用实现方式的宏观解决方案。而具体的Servlet容器负责提供标准的实现。
+ Servlet作为服务器端的一个组件，它的本意是“服务器端的小程序”。Servlet的实例对象由Servlet容器负责创建；Servlet的方法由容器在特定情况下调用；Servlet容器会在Web应用卸载时销毁Servlet对象的实例。
+ 简单可以理解为  Servlet就是用来处理客户端的请求的.

#### Servlet使用如下
##### 1. 源代码使用
1. src/ 下创建一个类,继承 Servlet,创建步骤如下:  
![创建Servlet](/img/javaweb/servlet/1.png)  
![创建Servlet](/img/javaweb/servlet/2.png)  
![创建Servlet](/img/javaweb/servlet/3.png)  
2. 编辑刚创建生成的 HelloServlet.java 文件,编辑service方法,因为此方法是用来处理请求及用户响应的方法  
![创建Servlet](/img/javaweb/servlet/4.png)  
![创建Servlet](/img/javaweb/servlet/5.png)  
编辑完后,Servlet会对 `<url-pattern>` 中映射的地址提供服务  
3. 项目上右键,选择 Run As -> Run On Server 
4. 打开的浏览器后拼接 `<url-pattern>` 中映射的地址 TestUrl,回车访问,eclipse的Console视图出现我们事先声明打印的 "请求已收到...." 提示文字,同时浏览器即出现servlet提供的响应"Response Success!",至此,基础的使用servlet就完成了.  
![创建Servlet](/img/javaweb/servlet/7.png)  
![创建Servlet](/img/javaweb/servlet/6.png)  
##### 2. 直接 new servlet 使用   
![创建Servlet](/img/javaweb/servlet/8.png)  
如果new的时候没有servlet选项,则使用如下方式添加servlet即可  
![创建Servlet](/img/javaweb/servlet/9.png)  
![创建Servlet](/img/javaweb/servlet/10.png)  
创建的java文件如下   
![创建Servlet](/img/javaweb/servlet/11.png)  
同时自动注册Servelet  
![创建Servlet](/img/javaweb/servlet/12.png)  
即可对url-pattern中映射的地址提供服务.

