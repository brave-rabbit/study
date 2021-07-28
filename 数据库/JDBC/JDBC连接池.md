# JDBC开发流程

- 加载驱动 （只需要加载一次）			      ------Class.forName(drive);
- 建立数据库连接（获取Connection）                    ------connection=DriverManager.getConnection(url,user,password);
- 执行SQL语句（操作Statement/PreparedStatement）
- 处理结果集（操作ResultSet，查询时需要）
- 断开连接、释放资源



传统方式下，执行增删改查时需要获取 connection对象，执行完后将对象释放。即每次执行操作时都要向数据库申请新的 connection 对象，也就是说一个 connection 对象执行一个操作就被释放了，那么100个操作就要申请100个对象，造成了资源的浪费，为解决这一问题，我们需要利用到**数据库连接池**



### 数据库连接池

基本思想：为数据库创建一个缓冲池，预先向缓冲池放入一定数量的连接对象，当申请数据库连接（connection）时，会优先从缓冲池中取对象，**用完之后再放回缓冲池**，供下一次使用。允许程序重复使用同一个数据库连接对象，且不需要重新创建新的对象，做到了资源的重复利用

【当缓冲池连接对象的数量不够时，新的请求就会进入等待队列，等待其他线程释放连接】



### 连接池的实现

由  javax.sql.DataSource 接⼝来完成的，DataSource 是 Java 官⽅提供的接⼝，使⽤的时候开发者并不需要自己来实现该接⼝，可以使⽤第三⽅的⼯具，C3P0 是⼀个常⽤的第三⽅实现，实际开发中直接使⽤ C3P0 即可完成数据库连接池的操作。

一般情况步骤如下：

![1612793814507](C:\Users\86185\AppData\Roaming\Typora\typora-user-images\1612793814507.png)



（使用IDEA情况下）

1.导入 c3p0-0.9.5.5.jar 及以上 jar 包（可能需要mchange-commons-java-0.2.19.jar----用来解决DBUtils连接超时问题）

2.代码实现以及基础参数设置

```java
//使用JAVA代码设置缓冲池参数
try{
ComboPooledDataSource dataSource=new ComboPooledDataSource();   //创建缓冲池
dataSource.setDriverClass("com.mysql.jdbc.Driver");             //设置Driver
dataSource.setJdbcUrl("jdbc:mysql://localhost:3306/demo");      //设置url
dataSource.setUser("root");                                     //设置用户名
dataSource.setPassword("228228");                               //设置密码
//设置缓冲池参数（最大连接数、最小连接数......）等操作
Connection connection=dataSource.getConnection();               //从缓冲池获取连接对象
//执行各种操作
connection.close();                                             //将对象返回到缓冲池中（没有释放）
}catch(.....){
    ...
}
```

可以通过缓冲池对象 dateSource 调用各种方法设置缓冲池的参数

实际开发中改写JAVA代码需要重新编译，很麻烦，所以一般我们用 XML 文件来实现对缓冲池参数的设置

**需要注意：**

1.XML文件名必须是  c3p0-config.xml

2.new ComboPooledDataSource()，传⼊的参数必须是 c3p0-confifig.xml 中 named-config 标签的 name 属性值，不传参数使用<default-config></default-config>里的设置

```JAVA
ComboPooledDataSource dataSource=new ComboPooledDataSource("c3p0");
```

- c3p0-config.xml 配置文件

```xml
<?xml version="1.0" encoding="UTF-8"?>
<c3p0-config>
	 <named-config name="c3p0">
 	<!-- 指定连接数据源的基本属性 -->
 	<property name="driverClass">com.mysql.jdbc.Driver</property>
    <property name="jdbcUrl">jdbc:mysql://localhost:3306/demo</property>
    <property name="user">root</property>
    <property name="password">228228</property>
         
 <!-- 若数据库中连接数不⾜时, ⼀次向数据库服务器申请多少个连接 -->
 	<property name="acquireIncrement">5</property>
 <!-- 初始化数据库连接池时连接的数量 -->
 	<property name="initialPoolSize">20</property>
 <!-- 数据库连接池中的最⼩的数据库连接数 -->		//当连接数为最小时就视为0个，应该进行补充（临界值）
 	<property name="minPoolSize">2</property>
 <!-- 数据库连接池中的最⼤的数据库连接数 -->
 	<property name="maxPoolSize">40</property>
 	</named-config>
</c3p0-config>
```

3.IDEA中 c3p0-config.xml 文件放在 resources 文件夹下

![1612793498206](C:\Users\86185\AppData\Roaming\Typora\typora-user-images\1612793498206.png)





