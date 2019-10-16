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

##### 3.1 导入jdbc的jar包
驱动程序由数据库提供商提供下载。 [点击下载MySQL的驱动下载](http://dev.mysql.com/downloads/)   
1. 新建lib文件夹,用于存放第三方jar包,将 *mysql-connector-java-5.1.44-bin.jar* jar包粘贴到新建的lib文件夹内   
![倒入jar包](/img/mysql/jdbc/01.png) 
2. 选中lib文件夹下的 *mysql-connector-java-5.1.44-bin.jar*包, 右键 Build Path -> Add to Build Path(因为已经创建过,所以此处无 Add to Build Path 按钮)   
![Build Path](/img/mysql/jdbc/02.png)
3. 此时根目录下会自动生成一个 Referenced Libraries 目录,内含有小奶瓶状的 *mysql-connector-java-5.1.44-bin.jar*包,即代表导入成功   
![小奶瓶](/img/mysql/jdbc/03.png)
4. 准备工作完成,使用jdbc连接操作数据库.

##### 3.2 使用jdbc查询数据库数据
![查询](/img/mysql/jdbc/04.png)

##### 3.3 使用jdbc向数据库中添加数据
![添加](/img/mysql/jdbc/05.png)

##### 3.4 使用jdbc向数据库中删除数据
![删除](/img/mysql/jdbc/06.png)

##### 3.5 使用jdbc修改数据库中的数据
![修改](/img/mysql/jdbc/07.png)

> 注意: excute返回值问题  
> boolean execute() throws SQLException在此 PreparedStatement 对象中执行 SQL 语句，该语句可以是任何种类的 SQL 语句。一些特别处理过的语句返回多个结果，execute 方法处理这些复杂的语句；executeQuery 和 executeUpdate 处理形式更简单的语句。 execute 方法返回一个 boolean 值，以指示第一个结果的形式。必须调用 getResultSet 或 getUpdateCount 方法来检索结果，并且必须调用 getMoreResults 移动到任何后面的结果返回：如果第一个结果是 ResultSet 对象，则返回 true；如果第一个结果是更新计数或者没有结果，则返回 false，意思就是如果是查询的话返回true，如果是更新或插入的话就返回false了；execute()返回的是一个boolean值,代表两种不同的操作啊,getResultSet()返回的是结果集,而getUpdateCount()返回的是更新的记数。


## 2.JDBC中数据库连接池的使用
#### 1. 数据库连接池的必要性
##### 不使用数据库连接池存在的问题:  
1. 普通的JDBC数据库连接使用 DriverManager 来获取，每次向数据库建立连接的时候都要将 Connection 加载到内存中，再验证IP地址，用户名和密码(得花费0.05s～1s的时间)。需要数据库连接的时候，就向数据库要求一个，执行完成后再断开连接。这样的方式将会消耗大量的资源和时间。数据库的连接资源并没有得到很好的重复利用.若同时有几百人甚至几千人在线，频繁的进行数据库连接操作将占用很多的系统资源，严重的甚至会造成服务器的崩溃。  
2. 对于每一次数据库连接，使用完后都得断开。否则，如果程序出现异常而未能关闭，将会导致数据库系统中的内存泄漏，最终将导致重启数据库。  
3. 这种开发不能控制被创建的连接对象数，系统资源会被毫无顾及的分配出去，如连接过多，也可能导致内存泄漏，服务器崩溃。
为解决传统开发中的数据库连接问题，可以采用数据库连接池技术（connection pool）。  

数据库连接池的基本思想就是为数据库连接建立一个“缓冲池”。预先在缓冲池中放入一定数量的连接，当需要建立数据库连接时，只需从“缓冲池”中取出一个，使用完毕之后再放回去。数据库连接池负责分配、管理和释放数据库连接，它允许应用程序重复使用一个现有的数据库连接，而不是重新建立一个。连接池的最大数据库连接数量限定了这个连接池能占有的最大连接数，当应用程序向连接池请求的连接数超过最大连接数量时，这些请求将被加入到等待队列中。  
![连接池](/img/mysql/jdbc/08.jpg)

##### 数据库连接池技术的优点：  
1. 资源重用   
	由于数据库连接得以重用，避免了频繁创建，释放连接引起的大量性能开销。在减少系统消耗的基础上，另一方面也增加了系统运行环境的平稳性。  
2. 更快的系统反应速度   
	数据库连接池在初始化过程中，往往已经创建了若干数据库连接置于连接池中备用。此时连接的初始化工作均已完成。对于业务请求处理而言，直接利用现有可用连接，避免了数据库连接初始化和释放过程的时间开销，从而减少了系统的响应时间
3. 新的资源分配手段   
	对于多应用共享同一数据库的系统而言，可在应用层通过数据库连接池的配置，实现某一应用最大可用数据库连接数的限制，避免某一应用独占所有的数据库资源
4. 统一的连接管理，避免数据库连接泄露  
	在较为完善的数据库连接池实现中，可根据预先的占用超时设定，强制回收被占用连接，从而避免了常规数据库连接操作中可能出现的资源泄露
	
#### 2. 多种开源的数据库连接池
JDBC 的数据库连接池使用 javax.sql.DataSource 来表示，DataSource 只是一个接口，该接口通常由服务器(Weblogic, WebSphere, Tomcat)提供实现，也有一些开源组织提供实现：  
1. DBCP 是Apache提供的数据库连接池，速度相对c3p0较快，但因自身存在BUG，Hibernate3已不再提供支持   
2. C3P0 是一个开源组织提供的一个数据库连接池，速度相对较慢，稳定性还可以   
3. Proxool 是sourceforge下的一个开源项目数据库连接池，有监控连接池状态的功能，稳定性较c3p0差一点   
4. BoneCP 是一个开源组织提供的数据库连接池，速度快   
5. Druid 是阿里提供的数据库连接池，据说是集DBCP 、C3P0 、Proxool 优点于一身的数据库连接池，但是速度不知道是否有BoneCP快     

DataSource 通常被称为数据源，它包含连接池和连接池管理两个部分，习惯上也经常把 DataSource 称为连接池  
注意：  
1. 数据源和数据库连接不同，数据源无需创建多个，它是产生数据库连接的工厂，因此整个应用只需要一个数据源即可。
2. 当数据库访问结束后，程序还是像以前一样关闭数据库连接：conn.close(); 但conn.close()并没有关闭数据库的物理连接，它仅仅把数据库连接释放，归还给了数据库连接池。  

#### 3.Druid（德鲁伊）数据源
 Druid是阿里巴巴开源平台上一个数据库连接池实现，它结合了C3P0、DBCP、Proxool等DB池的优点，同时加入了日志监控，可以很好的监控DB池连接和SQL的执行情况，可以说是针对监控而生的DB连接池，据说是目前最好的连接池。
 
#### 4.使用Druid连接池
##### 4.1 导入Druid的jar包
![导入Druid的jar包](/img/mysql/jdbc/08.png)
##### 4.2 使用Druid连接池
###### 4.2.1 代码配置连接
![代码配置](/img/mysql/jdbc/09.png) 
###### 4.2.2 使用 `druid.properties` 配置文件
1. `src/` 下新建 `druid.properties` 文件,内容为  
![druid.properties配置](/img/mysql/jdbc/10.png) 
2. 使用配置文件连接池连接数据库  
![使用配置文件连接池连接数据库](/img/mysql/jdbc/11.png) 

###### 4.2.3 提取连接数据库的方法
使用druid,每次连接数据库,都要重复一下步骤:  
1. 加载properties配置文件  
2. 加载驱动 
3. 从连接池中获取数据源对象  
4. 获取连接对象  
5. 关闭连接    
五个步骤,故可将此部分复用代码提取到一个工具类中,便于复用  

提取方法:  
1. src/ 下新建一个名为utils的 Java Project 项目,用于存放工具类方法;  
2. new Class JDBCUtils 工具类,代码如下:  


	package com.richinfo.team.utils;
	/**
	 * 工具类:
	 *   获取连接对象
	 */
	import java.io.IOException;
	import java.io.InputStream;
	import java.sql.Connection;
	import java.sql.ResultSet;
	import java.sql.SQLException;
	import java.sql.Statement;
	import java.util.Properties;
	
	import javax.sql.DataSource;
	
	import com.alibaba.druid.pool.DruidDataSourceFactory;
	
	public class JDBCUtils {
	
		static DataSource dataSource;
		static {
			try {
				// 1. 加载配置文件
				Properties properties = new Properties();
				InputStream inputStream = JDBCUtils.class.getClassLoader().getResourceAsStream("druid.properties");
				properties.load(inputStream);
				
				// 2. 加载驱动
				String driver = properties.getProperty("driver");
				Class.forName(driver);
				
				// 3. 获取数据源对象
				dataSource = DruidDataSourceFactory.createDataSource(properties);
				
			} catch (Exception e) {
				// TODO Auto-generated catch block
				e.printStackTrace();
			}
		}
		
		// 获取连接对象
		public static Connection getConnection() throws SQLException {
			return dataSource.getConnection();
		}
		// 释放资源
		public static void close(ResultSet resultSet,Statement statement, Connection connection) {
			if (resultSet != null) {
				try {
					resultSet.close();
				} catch (SQLException e) {
					// TODO Auto-generated catch block
					e.printStackTrace();
				}
			}
			if (statement != null) {
				try {
					statement.close();
				} catch (SQLException e) {
					// TODO Auto-generated catch block
					e.printStackTrace();
				} 
			}
			if (connection != null) {
				try {
					connection.close();
				} catch (SQLException e) {
					// TODO Auto-generated catch block
					e.printStackTrace();
				}
			}
		}
	}
	
封装好工具类后即可通过工具类获取连接对象	
1. 新建 JdbcByUtils 测试类,代码如下:
	
		package com.rinchinfo.team.moa;
	
		import static org.junit.Assert.*;
		
		import java.sql.Connection;
		import java.sql.ResultSet;
		import java.sql.Statement;
		
		import org.junit.Test;
		
		import com.richinfo.team.utils.JDBCUtils;
		
		public class DruidByUtils {
	
			@Test
			public void testInsert() throws Exception {
				// 1. 从JDBCUtils工具类里获取连接对象
				Connection connection = JDBCUtils.getConnection();
				// 2. 创建命令对象
				Statement statement = connection.createStatement();
				// 3.1 编写sql语句
				String insertSql = "insert into account (uname, pwd) values ('旺旺', 'pwd@999'),('Tom', 'pwd@888')";
				// 4.1 执行sql语句,获取结果集
				int resultSet = statement.executeUpdate(insertSql);
				if (resultSet != 0) {
					System.out.println("插入成功!");
				} else {
					System.out.println("TMD,插入失败了...");
				}
				// 3.2 编写查询的sql语句
				String selectQuery = "select * from account";
				// 4.2 执行sql语句,获取结果集
			 	ResultSet resultSet1 = statement.executeQuery(selectQuery);
			 	while (resultSet1.next()) {
					String unameString = resultSet1.getString("uname");
					String pwdString = resultSet1.getString("pwd");
					System.out.println("姓名: "+unameString+"\t"+"密码: "+pwdString);
				}
			 	// 5. 关闭连接
			 	JDBCUtils.close(resultSet1, statement, connection);
			}
		}
	
	输出结果如下:
	插入成功!
	姓名: 小明	密码: pwd@54321
	姓名: 小红	密码: pwd@112222222
	姓名: 旺旺	密码: pwd@999
	姓名: Tom	密码: pwd@888
	
	
##### 4.3 案例:实现登录功能
	package com.rinchinfo.team.moa;
	
	import static org.junit.Assert.*;
	
	import java.sql.Connection;
	import java.sql.ResultSet;
	import java.sql.Statement;
	import java.util.Scanner;
	
	import org.junit.Test;
	
	import com.richinfo.team.utils.JDBCUtils;
	
	public class LoginTest {
		
		@Test
		public void login() throws Exception {
			// 获取用户输入
			Scanner scanner = new Scanner(System.in);
			System.out.println("请输入用户名");
			String uname = scanner.nextLine();
			System.out.println("请输入密码");
			String upwd = scanner.nextLine();
			Connection connection = null;
			Statement statement = null;
			ResultSet resultSet = null;
			try {
				connection = JDBCUtils.getConnection();
				statement = connection.createStatement();
				String loginSqlString = "select * from account where uname='"+ uname +"' and pwd='"+ upwd +"'";
				resultSet = statement.executeQuery(loginSqlString);
				if (resultSet.next()) {
					System.out.println("登录成功!");
				} else {
					System.out.println("登录失败!");
				}
			} catch (Exception e) {
				// TODO: handle exception
				e.printStackTrace();
			} finally {
				// TODO: handle finally clause
				JDBCUtils.close(resultSet, statement, connection);
			}
			
		}
	}
	
![登录结果](/img/mysql/jdbc/12.png) 

##### 4.4 sql注入场景
> SQL注入  
> SQL 注入是利用某些系统没有对用户输入的数据进行充分的检查，而在用户输入数据中注入非法的 SQL 语句段或命令，从而利用系统的 SQL 引擎完成恶意行为的做法。对于 Java 而言，要防范 SQL 注入，只要用 PreparedStatement 取代 Statement 就可以了。

如上登录场景,因为我们无法控制用户的输入,当用户输入如下用户名时  
![sql注入](/img/mysql/jdbc/13.png)  
依然能登录成功,分析拼接的sql语句为:  

	select * from account where uname="xxx" or 1=1 #" and pwd="ssssssss"
此条sql语句因为有 `or 1=1` 为 `true` 的存在,所以会将数据库中的数据都查询出来,如图:  
![sql注入](/img/mysql/jdbc/14.png)   
故无论密码如何输入,均能登录成功;

##### 4.5 使用PreparedStatement防止sql注入
PreparedStatement概述  
可以通过调用 Connection 对象的 preparedStatement(String sql) 方法获取 PreparedStatement 对象
PreparedStatement 接口是 Statement 的子接口，它表示一条预编译过的 SQL 语句  
1. PreparedStatement 对象所代表的 SQL 语句中的参数用问号(?)来表示，调用 PreparedStatement 对象的 setXxx() 方法来设置这些参数. setXxx() 方法有两个参数，第一个参数是要设置的 SQL 语句中的参数的索引(从 1 开始)，第二个是设置的 SQL 语句中的参数的值  
2. ResultSet executeQuery()执行查询，并返回该查询生成的 ResultSet 对象。  
3. int executeUpdate()：执行更新，包括增、删、该 

用法如下:   

	@Test
	public void prepareLogin() throws Exception {
		// 获取用户输入
		Scanner scanner = new Scanner(System.in);
		System.out.println("请输入用户名");
		String uname = scanner.nextLine();
		System.out.println("请输入密码");
		String upwd = scanner.nextLine();
		Connection connection = null;
		PreparedStatement statement = null;
		ResultSet resultSet = null;
		try {
			connection = JDBCUtils.getConnection();
			// 使用PreparedStatement
			// statement = connection.createStatement();
			// String loginSqlString = "select * from account where uname='"+ uname +"' and pwd='"+ upwd +"'";
			statement = connection.prepareStatement("select * from account where uname = ? and pwd = ?");
			// 赋值
			// 第一个参数为第一个?的位置,第二个参数为?占位符的实参,遇到特殊字符会转译,
			// 会将第二个参数全部赋值给对应形参,如 xxx' or 1=1 # 会全部当作用户名处理,不会出现sql注入的情况
			statement.setString(1, uname);
			statement.setString(2, upwd);
			resultSet = statement.executeQuery();
			if (resultSet.next()) {
				System.out.println("登录成功!");
			} else {
				System.out.println("登录失败!");
			}
		} catch (Exception e) {
			// TODO: handle exception
			e.printStackTrace();
		} finally {
			// TODO: handle finally clause
			JDBCUtils.close(resultSet, statement, connection);
		}
	}

登录结果为:  
![sql注入](/img/mysql/jdbc/15.png) 

## 3. 批量处理
使用场景:  
当一次性插入很多条数据时,可以使用批量处理  
好处:  
减少执行次数,提高执行效率
常用方法:  
addBatch();  
executeBatch();  
clearBatch();  
如场景,一次性插入5000条数据,代码如下:  

	@Test
	public void prepareLogin() throws Exception {
		// 获取用户输入
		Scanner scanner = new Scanner(System.in);
		System.out.println("请输入用户名");
		String uname = scanner.nextLine();
		System.out.println("请输入密码");
		String upwd = scanner.nextLine();
		Connection connection = null;
		PreparedStatement statement = null;
		ResultSet resultSet = null;
		try {
			connection = JDBCUtils.getConnection();
			statement = connection.prepareStatement("select * from account where uname = ? and pwd = ?");
			for (int i=1; i<=5000; i++) {
				statement.setString(1, uname);
				statement.setString(2, upwd);
				// 将sql语句添加到批量处理中
				statement.addBatch();
				if (i%1000==0) {
					// 执行批量处理
					statement.executeBatch();
					// 清空批量处理中的语句
					statement.clearBatch();
				}	
			}
			if (resultSet.next()) {
				System.out.println("登录成功!");
			} else {
				System.out.println("登录失败!");
			}
		} catch (Exception e) {
			// TODO: handle exception
			e.printStackTrace();
		} finally {
			// TODO: handle finally clause
			JDBCUtils.close(resultSet, statement, connection);
		}
	}
 
## 事物
	
	@Test
	public void test(){
		Connection  connection = null;
		PreparedStatement statement = null;
		try {
			connection = JDBCUtils.getConnection();
			// 设置自动提交为false  手动开启事务
			connection.setAutoCommit(false);
			statement = connection.prepareStatement("update accout set money = ? where uname = ?");
			statement.setDouble(1, 500);
			statement.setString(2, "小明");
			statement.executeUpdate();
			statement.setDouble(1, 1500);
			statement.setString(2, "大明");
			statement.executeUpdate();
			
			// 提交事务
			connection.commit();
		} catch (SQLException e) {
			// TODO Auto-generated catch block
			e.printStackTrace();
			try {
				// 如果出新问题,回滚代码
				connection.rollback();
			} catch (SQLException e1) {
				// TODO Auto-generated catch block
				e1.printStackTrace();
			}
		}finally {
			JDBCUtils.close(null, statement, connection);
		}
	
	}
 
