---
title: JDBC
date: 2019-09-04 16:49:40
tags: jdbc
---

#### 1. 概述
在Java中，数据库存取技术可分为如下几类：   

+ JDBC直接访问数据库
+ JDO技术（Java Data Object）
+ 第三方O/R工具，如Hibernate, Mybatis 等  

JDBC是java访问数据库的基石，JDO, Hibernate等只是更好的封装了JDBC。
#### 2. 什么是JDBC？
JDBC(Java Database Connectivity)是一个**独立于特定数据库管理系统（DBMS）、通用的SQL数据库存取和操作的公共接口**（一组API），定义了用来访问数据库的标准Java类库，使用这个类库可以以一种标准的方法、方便地访问数据库资源
JDBC为访问不同的数据库提供了一种**统一的途径** ，为开发者屏蔽了一些细节问题。
JDBC的目标是使Java程序员使用JDBC可以连接任何**提供了JDBC驱动程序**的数据库系统，这样就使得程序员无需对特定的数据库系统的特点有过多的了解，从而大大简化和加快了开发过程。
如果没有JDBC，那么Java程序访问数据库时是这样的：
![没有JDBC时，Java程序访问数据库时图形描述](/img/mysql/01.png)
