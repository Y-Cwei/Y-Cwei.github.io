---
title: mysql基础语法.md
date: 2019-09-03 17:32:15
tags: mysql
---

### mysql用户管理：
+ 首先登录到MySQL 
	
		mysql -u root -p

+ 远程登录服务

		[root@lear user1]# mysql -h10.211.55.16 -P3306 -uclass -p
		Enter password: 
		Welcome to the MySQL monitor.  Commands end with ; or \g.
		Your MySQL connection id is 5
		Server version: 5.1.73 Source distribution
### 数据库
<!--more-->
+ 查看数据库 `show databases;`

		mysql> show databases;
		+--------------------+
		| Database           |
		+--------------------+
		| information_schema |
		| mysql              |
		| test               |
		+--------------------+
		3 rows in set (0.00 sec)
		 
		mysql> 

+ 查看当前的数据库 `select database();`  
+ 进入数据库 `use mysql;`   
如果不使用数据库，则当前的数据库为空NULL，use之后，数据库不为空。
		
		mysql> select database();
		+------------+
		| database() |
		+------------+
		| NULL       |
		+------------+
		1 row in set (0.00 sec)
		 
		mysql> use mysql;
		Reading table information for completion of table and column names
		You can turn off this feature to get a quicker startup with -A
		 
		Database changed
		mysql> select database();
		+------------+
		| database() |
		+------------+
		| mysql      |
		+------------+
		1 row in set (0.00 sec)
		 
		mysql> 

+ 创建数据库 `create database [if not exists] database_name;`  
如果数据库存在，创建会报错，加一个判断，当数据库不存在的时候再创建

		ysql> create database if not exists `mydb`;
		Query OK, 1 row affected, 1 warning (0.00 sec)
		 
		mysql> show databases;
		+--------------------+
		| Database           |
		+--------------------+
		| information_schema |
		| mydb               |
		| mysql              |
		| test               |
		+--------------------+
		4 rows in set (0.00 sec)
		 
		mysql> 
+ 删除数据库 `drop database if exists database_name;`

		mysql> show databases;
		+--------------------+
		| Database           |
		+--------------------+
		| information_schema |
		| mydb               |
		| mysql              |
		| test               |
		+--------------------+
		4 rows in set (0.00 sec)
 
		mysql> create database if not exists temp;
		Query OK, 1 row affected (0.00 sec)
		 
		mysql> show databases;
		+--------------------+
		| Database           |
		+--------------------+
		| information_schema |
		| mydb               |
		| mysql              |
		| temp               |
		| test               |
		+--------------------+
		5 rows in set (0.00 sec)
		 
		mysql> drop database if exists temp;
		Query OK, 0 rows affected (0.00 sec)
		 
		mysql> show databases;
		+--------------------+
		| Database           |
		+--------------------+
		| information_schema |
		| mydb               |
		| mysql              |
		| test               |
		+--------------------+
		4 rows in set (0.00 sec)
		 
		mysql> 

### 数据表
+ 显示数据库表 `show tables;`  
默认显示当前数据库的表  
`show tables from mysql;` 查看 `mysql` 指定数据库的表  

		mysql> use mydb;
		Reading table information for completion of table and column names
		You can turn off this feature to get a quicker startup with -A
		 
		Database changed
		mysql> show tables;
		+----------------+
		| Tables_in_mydb |
		+----------------+
		| tb1            |
		+----------------+
		1 row in set (0.00 sec)
		 
		mysql> show tables from mysql;
		+---------------------------+
		| Tables_in_mysql           |
		+---------------------------+
		| columns_priv              |
		| db                        |
		| event                     |
		| func                      |
		| general_log               |
		| help_category             |
		| help_keyword              |
		| help_relation             |
		| help_topic                |
		| host                      |
		| ndb_binlog_index          |
		| plugin                    |
		| proc                      |
		| procs_priv                |
		| servers                   |
		| slow_log                  |
		| tables_priv               |
		| time_zone                 |
		| time_zone_leap_second     |
		| time_zone_name            |
		| time_zone_transition      |
		| time_zone_transition_type |
		| user                      |
		+---------------------------+
		23 rows in set (0.00 sec)
		 
		mysql> 

+ 创建数据表  
INT 整数类型  
VARCHAR 变长字符，参数为最大字符限制  

		mysql> create table `tb1`(
		    -> `id` INT,
		    -> `name` VARCHAR(20)
		    -> );
		Query OK, 0 rows affected (0.02 sec)
		 
		mysql> 

+ 删除数据表 `drop table (table_name);`

+ 查看表信息 `show create table (table_name) [\G];`
	
		mysql> show create table `tb1` \G
		*************************** 1. row ***************************
		       Table: tb1
		Create Table: CREATE TABLE `tb1` (
		  `id` int(11) DEFAULT NULL,
		  `name` varchar(20) DEFAULT NULL
		) ENGINE=MyISAM DEFAULT CHARSET=utf8
		1 row in set (0.00 sec)
		 
		mysql> 

+ 查看表结构 `describe (table_name);` 或者 `show columns from tb1;`

		mysql> describe tb1;
		+-------+-------------+------+-----+---------+-------+
		| Field | Type        | Null | Key | Default | Extra |
		+-------+-------------+------+-----+---------+-------+
		| id    | int(11)     | YES  |     | NULL    |       |
		| name  | varchar(20) | YES  |     | NULL    |       |
		+-------+-------------+------+-----+---------+-------+
		2 rows in set (0.00 sec)
		mysql> 

+ 修改表结构 `alter table tb1 add age INT;`

		
		mysql> alter table `tb1` add `age` INT;
		Query OK, 0 rows affected (0.02 sec)
		Records: 0  Duplicates: 0  Warnings: 0
		 
		mysql> desc `tb1`;
		+-------+-------------+------+-----+---------+-------+
		| Field | Type        | Null | Key | Default | Extra |
		+-------+-------------+------+-----+---------+-------+
		| id    | int(11)     | YES  |     | NULL    |       |
		| name  | varchar(20) | YES  |     | NULL    |       |
		| age   | int(11)     | YES  |     | NULL    |       |
		+-------+-------------+------+-----+---------+-------+
		3 rows in set (0.00 sec)
		 
		mysql> 
+ 修改表名称 `alter table table1 rename to teacher;`  
+ 修改列名称 `alter table table1 change name user_name varchar(10) not null;`
指定位置添加
		
		mysql> alter table `tb1` add `gender` char(1) after `name`;
		Query OK, 0 rows affected (0.02 sec)
		Records: 0  Duplicates: 0  Warnings: 0
		 
		mysql> desc `tb1`;
		+--------+-------------+------+-----+---------+-------+
		| Field  | Type        | Null | Key | Default | Extra |
		+--------+-------------+------+-----+---------+-------+
		| id     | int(11)     | YES  |     | NULL    |       |
		| name   | varchar(20) | YES  |     | NULL    |       |
		| gender | char(1)     | YES  |     | NULL    |       |
		| age    | int(11)     | YES  |     | NULL    |       |
		+--------+-------------+------+-----+---------+-------+
		4 rows in set (0.00 sec)
		mysql> 