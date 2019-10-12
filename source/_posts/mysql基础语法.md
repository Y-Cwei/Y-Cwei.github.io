---
title: mysql基础语法
date: 2019-09-03 17:32:15
tags: mysql
---

### SQL语句分类
+ DDL(Data Definition Language):数据定义语言，用来定义数据库对象：库、表、列等。功能：创建、删除、修改库和表结构。
+ DML(Data Manipulation Language):数据操作语言，用来定义数据库记录:增、删、改表记录。
+ DCL(Data Control Language):数据控制语言，用来定义访问权限和安全级别。
+ DQL(Data Query Language):数据查询语言，用来查询记录。也是本章学习的重点。
<!--more-->


### SQL数据中的属性类型
+ TINYINT:1字节，小整数值。
+ SMALLINT:2字节，大整数值。
+ MEDIUMINT:3字节，大整数值。
+ INT或INTEGER:4字节，整型,大整数值。
+ FLOAT:单精度浮点数值。
+ DOUBLE(5,2):双精度浮点型数值，参数表示该浮点型数值最多有5位，其中必须有2位小数。
+ DECIMAL(M,D):小数值,参数表示该数值最多有M位，其中必须有D位小数。
+ CHAR:字符型，固定长度字符串类型:char(255)。你存入一个a字符，虽然a只占一个字符，但是它会自动给你加254个空格凑成255个长度。即数据的长度不足指定长度，它会补足到指定长度。
+ VARCHAR:可变长度字符串类型：- varchar(65535),你存入的数据多长它就是多长。它会抽出几个字节来记录数据的长度。
+ TEXT(CLOB):mysql独有的数据类型，字符串类型。
+ BLOB:字节类型。
+ YEAR:年份值，格式为:YYYY
+ DATA:日期类型，格式为:yyyy-MM-dd。
+ TIME:时间类型，格式为:hh:mm:ss。
+ TIMESTAMP:时间戳类型，格式为上面二者的综合。
+ DATETIME:混合日期和时间值，格式为:YYYYMMDD HHMMSS.

### SQL语句详解
+ 当然首先需要再命令行中输入mysql -uroot -p来进入mysql。
+ 注意:
	+ MySQL语法不区分大小写，但是建议在写关键字时用大写。
	+ 每一条语句后面以分号结尾。

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

		ysql> create database if not exists mydb;
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

		mysql> create table tb1(
		    -> id INT,
		    -> name VARCHAR(20)
		    -> );
		Query OK, 0 rows affected (0.02 sec)
		 
		mysql> 

   
+ 删除数据表 `drop table (table_name);`

+ 查看表信息 `show create table (table_name) [\G];`
	
		mysql> show create table tb1 \G
		*************************** 1. row ***************************
		       Table: tb1
		Create Table: CREATE TABLE tb1 (
		  id int(11) DEFAULT NULL,
		  name varchar(20) DEFAULT NULL
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
		  
			mysql> alter table tb1 add age INT;
			Query OK, 0 rows affected (0.02 sec)
			Records: 0  Duplicates: 0  Warnings: 0
			
			mysql> desc tb1;
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
		
		mysql> alter table tb1 add gender char(1) after name;
		Query OK, 0 rows affected (0.02 sec)
		Records: 0  Duplicates: 0  Warnings: 0
		 
		mysql> desc tb1;
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


+ 添加多列
		
		mysql> alter table tb1 add(aaa int, bb int, cc int);
		Query OK, 0 rows affected (0.01 sec)
		Records: 0  Duplicates: 0  Warnings: 0
		 
		mysql> desc tb1;
		+--------+-------------+------+-----+---------+-------+
		| Field  | Type        | Null | Key | Default | Extra |
		+--------+-------------+------+-----+---------+-------+
		| id     | int(11)     | YES  |     | NULL    |       |
		| name   | varchar(20) | YES  |     | NULL    |       |
		| gender | char(1)     | YES  |     | NULL    |       |
		| age    | int(11)     | YES  |     | NULL    |       |
		| aaa    | int(11)     | YES  |     | NULL    |       |
		| bb     | int(11)     | YES  |     | NULL    |       |
		| cc     | int(11)     | YES  |     | NULL    |       |
		+--------+-------------+------+-----+---------+-------+
		7 rows in set (0.00 sec)
		 
		mysql> 


+ 删除数据表结构 `alter table (table_name) drop '(column_name)';`

		mysql> alter table tb1 drop aa;
		ERROR 1091 (42000): Can't DROP 'aa'; check that column/key exists
		mysql> alter table tb1 drop aaa;
		Query OK, 0 rows affected (0.00 sec)
		Records: 0  Duplicates: 0  Warnings: 0
		mysql> desc tb1;
		+--------+-------------+------+-----+---------+-------+
		| Field  | Type        | Null | Key | Default | Extra |
		+--------+-------------+------+-----+---------+-------+
		| id     | int(11)     | YES  |     | NULL    |       |
		| name   | varchar(20) | YES  |     | NULL    |       |
		| gender | char(1)     | YES  |     | NULL    |       |
		| age    | int(11)     | YES  |     | NULL    |       |
		| bb     | int(11)     | YES  |     | NULL    |       |
		| cc     | int(11)     | YES  |     | NULL    |       |
		+--------+-------------+------+-----+---------+-------+
		6 rows in set (0.00 sec)
		mysql> 


+ 删除多列 `alter table (table_name) drop bb,drop cc;`

		mysql> alter table tb1 drop bb,drop cc;
		Query OK, 0 rows affected (0.01 sec)
		Records: 0  Duplicates: 0  Warnings: 0
		 
		mysql> desc tb1;
		+--------+-------------+------+-----+---------+-------+
		| Field  | Type        | Null | Key | Default | Extra |
		+--------+-------------+------+-----+---------+-------+
		| id     | int(11)     | YES  |     | NULL    |       |
		| name   | varchar(20) | YES  |     | NULL    |       |
		| gender | char(1)     | YES  |     | NULL    |       |
		| age    | int(11)     | YES  |     | NULL    |       |
		+--------+-------------+------+-----+---------+-------+
		4 rows in set (0.00 sec)


+ 向数据表中添加数据 `insert [INTO] (table_name) (column1,column2) values (value1,value2)`
		
		mysql> insert into tb1 values(1,'rose','m',18);
		Query OK, 1 row affected (0.00 sec)


+ 一次添加多行数据

		mysql> insert into tb1 (id,name) values (2,'zhangsan'),(3,'lisi');
		Query OK, 2 rows affected (0.00 sec)
		Records: 2  Duplicates: 0  Warnings: 0
		 
		mysql> select * from tb1;
		+------+----------+--------+------+
		| id   | name     | gender | age  |
		+------+----------+--------+------+
		|    1 | rose     | m      |   18 |
		|    2 | zhangsan | NULL   | NULL |
		|    3 | lisi     | NULL   | NULL |
		+------+----------+--------+------+
		3 rows in set (0.00 sec)
		 
		mysql> 


+ 通过set添加数据
	
		insert into tb1 name='zhangsan',age=30;


+ 更新数据

		update tb1 set age=age+1 [where name='rose'];


+ 删除数据

		delete from tb1 where name='rose';


### 查看数据表中的数据
> select * from tb1;

+ 查询指定列

		select name from students;


+ 给查询取别名 as是可选

		select name as 姓名,s_id 学号 from students;


+ 排序

		-- 升序
		select * from stu_detail order by age (asc);
		-- 降序
		select * from stu_detail order by age desc;


+ 限制显示的数量

		-- 显示前2个
		select * from stu_detail limit 2;
		-- 显示第3个开始的4个数据
		select * from stu_detail limit 2,4;


+ 数据分组

		select d_id,count(d_id) from students group by d_id;
		-- 条件分组
		select d_id,count(d_id) from students group by d_id having d_id > 3;
		也可以使用别名
		select d_id as 学院ID,count(d_id) from students group by d_id having 学院ID > 3;


+ 查询结果处理

		-- 取最大值
		select max(age) from stu_detail;
		-- 去平均值
		select avg(age) from stu_detail;
		-- 去最小值
		select min(age) from stu_detail;


+ 子查询

		select * from stu_detail where age > (select avg(age) from stu_detail);


+ 关联查询

		select * from students,department where d_id=id;
		select * from students inner join department where d_id=id;
		select * from students inner join department on d_id=id;
		-- 左连接
		students的数据都能显示
		student d_id没有department对应的，也能显示 不能用where 必需用on
		select * from students left join department on d_id = id;
		-- 右连接
		department的数据都能显示
		select * from students right join department on d_id = id;


### 事务

+ 关联的SQL语句一起执行

		start transaction 开始事务
		commit 提交事务
		rollback 回滚事务，放弃修改
		
		
---	

### mysql常见问题
#### 1. insert into 插入报错
执行 `insert into 数据表名 (列1,列2) values("列1要插入的值","列2要插入的值");` 命令,报错,报错信息如下:  
![insert into报错信息](/img/mysql/api/01.png)  
出现此问题原因为已建立的表无法插入中文字符串  
使用 `show create table 表名` 查询创建表信息命令查询可知,表默认的字符编码为latin1字符集,查询结果如下   
![show create table 表名](/img/mysql/api/02.png)    
或者使用 `show full fields from 表名; ` 查询表详细信息,查询结果如下:  
![show full fields 表名](/img/mysql/api/03.png)  
因为数据表中的内容为latin1字符集，查询资料可知，latin1字符集为8bit，这说明它是不能表示中文的，故而当然会报改错：  

**解决方法如下:**  
修改字段的字符集,将其修改为 utf8 字符集即可;修改字段的字符集语法为 `alter table 表名 change 列名 列名 列类型 character set utf8; `,如下:  
![修改字符集](/img/mysql/api/04.png)  
如上修改之后,在执行 `insert into ...` 插入命令即可;

> 扩展:  
> 1. 修改数据库字符集命令 `alter database 数据库名 character set utf8;`.   
> 2. 修改数据表字符集命令 `alter table 数据表名 character set utl8;` 


