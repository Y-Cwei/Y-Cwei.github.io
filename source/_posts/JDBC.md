---
title: JDBC
date: 2019-09-04 16:49:40
tags: JDBC
---

## 1. 概述
在Java中，数据库存取技术可分为如下几类：   

+ JDBC直接访问数据库
+ JDO技术（Java Data Object）
+ 第三方O/R工具，如Hibernate, Mybatis 等  

JDBC是java访问数据库的基石，JDO, Hibernate等只是更好的封装了JDBC。
#### 1. 什么是JDBC？
JDBC(Java Database Connectivity)是一个**独立于特定数据库管理系统（DBMS）、通用的SQL数据库存取和操作的公共接口**（一组API），定义了用来访问数据库的标准Java类库，使用这个类库可以以一种标准的方法、方便地访问数据库资源
JDBC为访问不同的数据库提供了一种**统一的途径** ，为开发者屏蔽了一些细节问题。
JDBC的目标是使Java程序员使用JDBC可以连接任何**提供了JDBC驱动程序**的数据库系统，这样就使得程序员无需对特定的数据库系统的特点有过多的了解，从而大大简化和加快了开发过程。  
如果没有JDBC，那么Java程序访问数据库时是这样的：  
![没有JDBC时，Java程序访问数据库时图形描述](/img/mysql/01.png)  
使用JDBC后，Java程序访问数据库时就变成了如下形式：  
![有JDBC时，Java程序访问数据库时图形描述](/img/mysql/02.png)  

结论：  
JDBC是SUN公司提供一套用于数据库操作的接口API，Java程序员只需要面向这套接口编程即可。
不同的数据库厂商，需要针对这套接口，提供不同实现。不同的实现的集合，即为不同数据库的驱动。

#### 2. JDBC API  
JDBC API是一系列的接口，它统一和规范了应用程序与数据库的连接、执行SQL语句，并到得到返回结果等各类操作。声明在java.sql与javax.sql包中。  
![JDBC API](/img/mysql/03.png) 


#### 3. JDBC程序编写步骤
1. 注册驱动（加载驱动）
2. 获取连接对象
3. 获取命令对象
4. 编写sql语句
5. 执行sql语句
6. 获取结果集
7. 处理结果
8. 释放资源

如图所示：  
![JDBC程序编写步骤](/img/mysql/04.png)  

## 2. 获取数据库连接

#### 1. 引入JDBC驱动程序  

驱动程序由数据库提供商提供下载。 [点击下载MySQL的驱动下载](http://dev.mysql.com/downloads/)
如何在Java Project项目应用中添加数据库驱动jar：  
代码如下：  

	public class JDBCDemo {
		public static void main(String[] args) throws SQLException{
			// 1.注册驱动
			DriverManager.registerDriver(new com.mysql.jdbc.Driver());
			// 2.获取连接对象
			Connection connection = DriverManager.getConnection("jdbc:mysql://localhost:3306/myemployees", "root", "root");
			// 3.获取命令对象
			Statement statement = connection.createStatement();
			// 4.编写sql语句
			String sql = "select * from employees where employee_id = 101";
			// 5.执行sql语句，获取结果集
			ResultSet resultSet = statement.executeQuery(sql);
			if (resultSet.next()) {
				// 6.处理逻辑
				String firstName = resultSet.getString("first_name");
				String lastName = resultSet.getString("last_name");
				System.out.println("first_name:"+firstName+", last_name:"+lastName);
			}
			// 8.释放资源
			resultSet.close();
			statement.close();
			connection.close();
		}
	}

## 
