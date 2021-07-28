# DBUtils

帮助开发者完成数据的封装（将结果集映射成Java对象或对象集合），简化开发



### 步骤：

1.导入jar包，下载地址（http://commons.apache.org/proper/commons-dbutils/download_dbutils.cgi）

2.代码实现

```JAVA
//使用c3p0连接数据库
ComboPooledDataSource dataSource=new ComboPooledDataSource("c3p0");			
Connection connection= dataSource.getConnection();
//创建对象
QueryRunner queryRunner=new QueryRunner();							  
String sql="select * from abc";
//将数据进行封装（student为自定义类）
List<student> list=queryRunner.query(connection,sql,new BeanListHandler<>(student.class));
//遍历结果
for (student a:list
     ) {
    System.out.println(a.getId()+a.getName());
}
connection.close();
```

QueryRunner：SQL语句的操作对象，可以设置查询结果集的封装策略，线程安全

**构造方法：**

1.QueryRunner()：创建一个与数据库无关的QueryRunner对象，后期再操作数据库的会后，需要手动给一个Connection对象，它可以手动控制事务

2.QueryRunner(DataSource ds)：创建一个与数据库关联的queryRunner对象，后期再操作数据库的时候，不需要Connection对象，自动管理事务。（ DataSource：缓冲池对象）

构造函数与增删改查方法的组合：               

- QueryRunner()

​           update(Connection conn, String sql, Object... params)

​           query(Connection conn, String sql, ResultSetHandler<T> rsh, Object... params)		

-  QueryRunner(DataSource ds)     

​           update(String sql, Object... params)

​           query(String sql, ResultSetHandler<T> rsh, Object... params)



### ResultSetHandler 接口（由DBUtils框架提供）

（假设 user 为自定义类，且 user 内各属性必须与数据库表中各字段所对应）

DBUtils给我们提供了10个ResultSetHandler实现类，分别是：

1.ArrayHandler：将查询结果的第一行数据，保存到Object数组中

2.ArrayListHandler：将查询的结果，每一行先封装到Object数组中，然后将数据存入List集合

**3.BeanHandler：将查询结果的第一行数据，封装到user对象（常用）**

**4.BeanListHandler：将查询结果的每一行封装到（所需要）对象，然后再存入List集合（常用）**

5.ColumnListHandler：将查询结果的指定列的数据封装到List集合中

**6.MapHandler：将查询结果的第一行数据封装到map结合（key=列名，value=列值）常用）**

**7.MapListHandler：将查询结果的每一行封装到map集合（key=列名，value=列值）,再将map集合存入List集合（常用）**

8.BeanMapHandler：将查询结果的每一行数据，封装到User对象，再存入map集合中（key=列名，value=列值）

9.KeyedHandler：将查询的结果的每一行数据，封装到map1（****key==列名，value==列值** **），然后将map1集合（有多个）存入map2集合（只有一个）

10.ScalarHandler：封装类似count、avg、max、min、sum......函数的执行结果



另外，我们也可以自定义类来实现ResultSetHandler接口







