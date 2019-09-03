---
title: java基础入门
date: 2019-06-10 22:03:36
tags: JAVA
---

### 1. JAVA基础介绍
#### 1.1 java面向对象编程
> 面向对象编程的三大特性 
> > 封装：  
> > 继承：  
> > 多态：  
> > （抽象）：

+ java程序要素介绍：
	1. java源文件以“.java”为扩展名。源文件的基本组成部分是类（class），如本类中的 PersonInfo 类
	2. 一个源文件中最多只能有一个 public 类。其它类的个数不限，如果源文件包含一个 public 类，则文件名必须按该雷鸣命名。
	<!--more-->
	3. Java应用程序的执行文件入口是 main() 方法。它有固定的书写格式：**public static void main(String[] args){...}**
	4. Java语言严格区分大小写。
	5. Java方法由一条条语句构成，每个语句以“;”结束。
	6. 打括号是成对出现的，缺一不可。
			
			/**
			 *  public: 公共的，公开的
			 *  class: 类的定义
			 *  PersonInfo: 类名
			 *  类是java程序最基本的单位
			 *  类 = 类头（类签名） + 类体
			 *  public class PersonInfo: 类名
			 *  { }: 类体，类后面的一对 {} 及其中的内容
			 *  main: 主方法，程序的入口方法
			 *  主类: 包含主方法的类就是主类
			*/
			public class PersonInfo {
			    /**
			     *  方法: 完成某种特定功能的单位
			     *  public static: 修饰符
			     *  void: 无返回值
			     *  main: 方法名
			     *  （String[] args）: 参数
			     *  方法名后面的一对 {} 及其中的内容称为方法体
			     *  方法 = 方法签名 + 方法体
			     */
			    public static void main (String[] args) {
			        /**
			         *  语句: java程序的最小执行单位，语句必须以分号 ; 结尾
			         */
			        System.out.println("姓名： 张三");
			        System.out.println("性别： 男");
			        System.out.println("籍贯： 河北");
			        System.out.println("住址： 河北省保定市");
			    }
			}

+ 开发一个java程序的步骤：

	1. 在目录下创建一个文本文件，文件名是 公共类名.java，编辑这个文件
	2. 命令行下使用 javac 公共类名.java，编译源文件，产生多个.class文件，文件数量取决于 源文件中方法的个素
	3. 执行这个程序，java 主类名

	
+ 执行过程：
	1. java 创建JVM
	2. 	由ClassLoader加载主类
	3. 执行主类中的主方法
	4. 主方法执行完毕后，销毁JVM.进程结束
	
	