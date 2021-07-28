# JDBC 简介

Java DataBase Connectivity 是⼀个独⽴于特定数据库的管理系统，是通⽤的 SQL 数据库存取和操作的公共接⼝。

 jdbc 定义了⼀组标准，为访问不同数据库提供了统⼀的途径。

![1612705288608](C:\Users\86185\AppData\Roaming\Typora\typora-user-images\1612705288608.png)





# JDBC的体系结构

jdbc 接口包括两个层面：

- 面向应用的API，供程序员调用						
- 面向数据库的API，供厂商开发数据库的驱动程序                  

![1612705589156](C:\Users\86185\AppData\Roaming\Typora\typora-user-images\1612705589156.png)

----JDBC API

提供者：Java官方

内容：供开发者调用的接口	（在 java.sql、javax.sql 包）

eg:

- DriverManager类	            加载		
- Connection 接口                  连接
- Statement 接口                    操作
- ResultSet 接口                     结果



----JDBC Driver Manager

提供者：Java官方

作用：管理不同的 JDBC 驱动



----JDBC驱动

提供者：数据库厂商

作用：负责连接不同的数据库





# 注意！！！

- 关于 Statement 与 PreparedStatement 

1.PreparedStatement 是Statement 的子类，提供了 SQL 占位符的功能（即用 ？表示数据）

```java
id=1；

//Statement开发
sql="delete from file where id="+id;
statement=connection.createStatement();
statement.executeUpdate(sql);

//PreparedStatement开发
sql="delete from file where id=?";
preparedStatement=connection.prepareStatement(sql);
preparedStatement.setInt(1,id);
preparedStatement.executeUpdate();
```



2.sql 语句放的顺序不同

Statement：是获得对象之后，再execute...中放入sql语句

PreparedStatement：是获取对象的时候就放入sql，之后execute...就不用了



3.使用 Statement 开发有两个问题

（1）需要频繁拼接	String 字符串，出错率较高

（2）存在 SQL 注入的风险。

【SQL注入：利⽤某些系统没有对⽤户输⼊的信息进⾏充分检测，在⽤户输⼊的数据中注⼊⾮法的 SQL 

语句，从⽽利⽤系统的 SQL 引擎完成恶意⾏为的做法】

```java
//比如检查密码是否正确时，用户输入 "123 or 1=1"，等式恒成立，检测功能失效
sql="select * from student where password="+password;
```



































