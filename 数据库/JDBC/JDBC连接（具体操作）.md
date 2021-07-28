# 	JDBC连接数据库

 步骤：

1.下载 jar 包

2.导入 jar 包（放在lib文件下并在Libraries中添加）

3.执行连接语句

4.操作数据库

5.断开数据库



### 连接语句

1.mysql：

```JAVA
Class.forName("com.mysql.jdbc.Driver");//8.0以上版本为 "com.mysql.cj.jdbc.Driver"		
String url="jdbc:mysql://localhost:3306/demo?useUnicode=true&characterEncoding=UTF-8";//后缀解决乱码
String user="root";
String password="228228";
Connection connection= DriverManager.getConnection(url,user,password);
```



2.oracle：

```JAVA
Class.forName("oracle.jdbc.OracleDriver");		
String url="jdbc:oracle:thin:@localhost:1521:SIMS";
String user="SYSTEM";
String password="Manager123";
Connection connection= DriverManager.getConnection(url,user,password);
```







### jar 包下载

1. mysql：https://dev.mysql.com/downloads/connector/j/

![1612434391722](C:\Users\86185\AppData\Roaming\Typora\typora-user-images\1612434391722.png)

![1612434477578](C:\Users\86185\AppData\Roaming\Typora\typora-user-images\1612434477578.png)



2.oracle：https://www.oracle.com/database/technologies/appdev/jdbc-downloads.html











