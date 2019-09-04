---
title: mysql之事务
date: 2019-09-04 14:55:11
tags: mysql
---

### 事务
事务属于 DTL 控制语言

#### 1. 什么是事务？为什么要用事务？   
+ 一个事务由一条或者多条sql语句构成，这一条或者多条sql语句要么全部执行成功，要么全部执行失败！  
+ 默认情况下，每条单独的sql语句就是一个单独的事务！ 


#### 2. 事务的四大特性（ACID）？  
+ **原子性（Atomicity）**：事务中所有操作是不可再分割的原子单位。事务中所有操作要么全部执行成功，要么全部执行失败。  
+ **一致性（Consistency）**：事务执行后，数据库状态与其它业务规则保持一致。如转账业务，无论事务执行成功与否，参与转账的两个账号余额之和应该是不变的。  
+ **隔离性（Isolation）**：隔离性是指在并发操作中，不同事务之间应该隔离开来，使每个并发中的事务不会相互干扰。  
+ **持久性（Durability）**：一旦事务提交成功，事务中所有的数据操作都必须被持久化到数据库中，即使提交事务后，数据库马上崩溃，在数据库重启时，也必须能保证通过某种机制恢复数据。 

##### 2.1 用法演示

	# 创建 db2 数据库
	CREATE DATABASE IF NOT EXISTS db2;
	# 使用 db2
	USE db2;
	# 查看使用的表
	SELECT DATABASE();	
	# 创建 account 表
	CREATE TABLE IF NOT EXISTS account(
		id INT PRIMARY KEY AUTO_INCREMENT,
		uname VARCHAR(16),
		pw VARCHAR(16),
		money DECIMAL(9,2)
	);
 	# 插入数据
	INSERT INTO account(uname, pw, money) VALUES('小琪', 'pwd12345', 2000), ('小洪', 'pwd12345', 5000);
	# 查看插入结果
	SELECT id AS 序号, uname 姓名, pw 密码, money 存款 from account;

	# 输出结果如下
	+----+-------+----------+---------+
	| id | uname | pw       | money   |
	+----+-------+----------+---------+
	|  1 | 小琪     | pwd12345 | 2000.00 |
	|  2 | 小洪     | pwd12345 | 5000.00 |
	+----+-------+----------+---------+
	2 rows in set (0.03 sec)


场景描述： 假如小琪想要给小洪转账1000元钱，代码描述如下

	# 1. 小琪账户减少1000元钱
	UPDATE account SET money=money-1000 WHERE uname='小琪';
	# 2. 小洪账户增加1000元钱
	UPDATE account SET money=money+1000 WHERE uname='小洪';
	# 查看转账结果
	SELECT id AS 序号, uname 姓名, pw 密码, money 存款 from account;

	# 输出结果如下
	+----+-------+----------+---------+
	| id | uname | pw       | money   |
	+----+-------+----------+---------+
	|  1 | 小琪     | pwd12345 | 1000.00 |
	|  2 | 小洪     | pwd12345 | 6000.00 |
	+----+-------+----------+---------+
	2 rows in set (0.03 sec)

#### 3. 事务的使用步骤：
如果在转账过程中，小琪账户减少了1000元钱，但是由于网络或者其它因素所致，小洪并没有收到小琪的转账1000元钱，违背了事务四大特性的一致性，所以必须要使用事务，来保证事务的一致性，事务使用如下：
	
	# 1. 开启事务
	START TRANSACTION;
	# 2. 编写事务的sql语句（增删改） UPDATE DELETE INSERT SELECT会自动提交数据
	UPDATE account SET money=money-1000 WHERE uname='小琪';
	UPDATE account SET money=money+1000 WHERE uname='小洪';
	# 3. 提交事务（无错误信息时）/ 回滚事务（又错误信息时）
	# ROLLBACK; # 回滚事务
	COMMIT; # 提交事务

使用事务，能很好的保证事务的一致性，有突发事件可以通过 ROLLBACK 回滚到初始状态；

**小技巧：**  

	# 查看当前数据库是否开启自动提交事务
	SHOW VARIABLES LIKE 'autocommit'; # autocommit	ON
	# 关闭自动提交事务
	SET autocommit=0;
	# 查看当前数据库是否开启自动提交事务
	SHOW VARIABLES LIKE 'autocommit'; # autocommit	OFF
	# 重新开启自动提交事务
	SET autocommit=1;
	# 查看当前数据库是否开启自动提交事务
	SHOW VARIABLES LIKE 'autocommit'; # autocommit	ON
	
	
	
	