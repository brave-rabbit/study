# 一、整合SQL数据库

导入驱动，驱动要与数据库的类型和版本相契合

```xml
<!--以MySQL为例-->
<dependency>
    <groupId>mysql</groupId>
    <artifactId>mysql-connector-java</artifactId>
    <version>5.1.47</version>
</dependency>
```





## 1、整合 JDBC 

JDBC 提供的数据源为 **HikariDataSource** （如果有自己的则优先使用自己的）



- 导入场景（依赖）

```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-data-jdbc</artifactId>
</dependency>
```



- 在配置文件中设置数据源的属性

以 **spring.datasource** 为前缀

```yml
spring:
  datasource:
    url: jdbc:mysql://localhost:3306/demo
    driver-class-name: com.mysql.jdbc.Driver
    username: root
    password: 228228
```



- 容器中存在  **JdbcTemplate** 实例，可以使用 **JdbcTemplate** 对数据库进行 crud

```java
class Boot05WebAdminApplicationTests {

    @Autowired
    JdbcTemplate jdbcTemplate;

    @Test
    void contextLoads() {

//        jdbcTemplate.queryForObject("select * from account_tbl")
//        jdbcTemplate.queryForList("select * from account_tbl",)
        Long aLong = jdbcTemplate.queryForObject("select count(*) from account_tbl", Long.class);
        log.info("记录总数：{}",aLong);
    }
}
```





## 2、整合 Druid 连接池

**Druid的简介**

Druid首先是一个数据库连接池。Druid是目前最好的数据库连接池，在功能、性能、扩展性方面，都超过其他数据库连接池，包括DBCP、C3P0、BoneCP、Proxool、JBoss DataSource。Druid已经在阿里巴巴部署了超过600个应用，经过一年多生产环境大规模部署的严苛考验。Druid是阿里巴巴开发的号称为监控而生的数据库连接池！



**Druid的功能**

1、替换DBCP和C3P0。Druid提供了一个高效、功能强大、可扩展性好的数据库连接池。

2、可以监控数据库访问性能，Druid内置提供了一个功能强大的StatFilter插件，能够详细统计SQL的执行性能，这对于线上分析数据库访问性能有帮助。

3、数据库密码加密。直接把数据库密码写在配置文件中，这是不好的行为，容易导致安全问题。DruidDruiver和DruidDataSource都支持PasswordCallback。

4、SQL执行日志，Druid提供了不同的LogFilter，能够支持Common-Logging、Log4j和JdkLog，你可以按需要选择相应的LogFilter，监控你应用的数据库访问情况。

5、扩展JDBC，如果你要对JDBC层有编程的需求，可以通过Druid提供的Filter机制，很方便编写JDBC层的扩展插件。



**所以Druid可以：**
1、充当数据库连接池。
2、可以监控数据库访问性能
3、获得SQL执行日志



Druid 提供的数据源为 **DruidDataSource**

Druid连接池

两种方式：

- 自定义
- 导入场景(Start)



**相关组件：**

- #### StatViewServlet

（1）提供监控信息展示的html页面**（访问请求 /druid/*）**

（2）提供监控信息的 JSON API



- #### StatFilter

（1）用于统计监控信息，如SQL监控、URI监控

（2）系统中所有filter：

| 别名          | Filter类名                                              |
| ------------- | ------------------------------------------------------- |
| default       | com.alibaba.druid.filter.stat.StatFilter                |
| stat          | com.alibaba.druid.filter.stat.StatFilter                |
| mergeStat     | com.alibaba.druid.filter.stat.MergeStatFilter           |
| encoding      | com.alibaba.druid.filter.encoding.EncodingConvertFilter |
| log4j         | com.alibaba.druid.filter.logging.Log4jFilter            |
| log4j2        | com.alibaba.druid.filter.logging.Log4j2Filter           |
| slf4j         | com.alibaba.druid.filter.logging.Slf4jLogFilter         |
| commonlogging | com.alibaba.druid.filter.logging.CommonsLogFilter       |





### 2.1 自定义方式

所有东西都需要手动配置

采用非SpringBoot配置文件的方式（旧的配置方式）

1、导入数据源依赖

```xml
<dependency>
    <groupId>com.alibaba</groupId>
    <artifactId>druid</artifactId>
    <version>1.1.17</version>
</dependency>
```

2、设置数据源（采用xml配置）

```xml
<bean id="dataSource" class="com.alibaba.druid.pool.DruidDataSource"
    destroy-method="close">
    <property name="url" value="${jdbc.url}" />
    <property name="username" value="${jdbc.username}" />
    <property name="password" value="${jdbc.password}" />
    <property name="maxActive" value="20" />
    <property name="initialSize" value="1" />
    <property name="maxWait" value="60000" />
    <property name="minIdle" value="1" />
    <property name="timeBetweenEvictionRunsMillis" value="60000" />
    <property name="minEvictableIdleTimeMillis" value="300000" />
    <property name="testWhileIdle" value="true" />
    <property name="testOnBorrow" value="false" />
    <property name="testOnReturn" value="false" />
    <property name="poolPreparedStatements" value="true" />
    <property name="maxOpenPreparedStatements" value="20" />
```

3、设置相关组件

- 设置 StatViewServlet

```xml
  <servlet>
    	<servlet-name>DruidStatView</servlet-name>
    	<servlet-class>com.alibaba.druid.support.http.StatViewServlet</servlet-class>
  </servlet>
  <servlet-mapping>
    	<servlet-name>DruidStatView</servlet-name>
    	<url-pattern>/druid/*</url-pattern>
  </servlet-mapping>
```

- 设置 StatFilter

```xml
需要给数据源中配置如下属性；可以允许多个filter，多个用，分割；如：
<property name="filters" value="stat,slf4j" />
```

```xml
设置 stat（slf4j也一样，就不多赘述了）
<bean id="stat-filter" class="com.alibaba.druid.filter.stat.StatFilter">
    <property name="slowSqlMillis" value="10000" />
    <property name="logSlowSql" value="true" />
</bean>
```





### 2.2 导入 Start 方式（推荐）

部分东西SpringBoot帮我们配置好了

采用SpringBoot配置文件的方式（新的配置方式）

1、导入场景（依赖于JDBC场景，**所以也需要导入 jdbc 场景**）

```xml
<dependency>
      <groupId>com.alibaba</groupId>
      <artifactId>druid-spring-boot-starter</artifactId>
      <version>1.1.17</version>
</dependency>
```

2、设置 druid 连接池的配置以及添加相关组件

```xml
spring:
  datasource:
    url: jdbc:mysql://localhost:3306/db_account
    username: root
    password: 123456
    driver-class-name: com.mysql.jdbc.Driver

    druid:
      aop-patterns: com.atguigu.admin.*  #监控SpringBean
      filters: stat,wall     # 底层开启功能，stat（sql监控），wall（防火墙）

      stat-view-servlet:   # 配置监控页功能
        enabled: true
        login-username: admin
        login-password: admin
        resetEnable: false

      web-stat-filter:  # 监控web
        enabled: true
        urlPattern: /*
        exclusions: '*.js,*.gif,*.jpg,*.png,*.css,*.ico,/druid/*'


      filter:
        stat:    # 对上面filters里面的stat的详细配置
          slow-sql-millis: 1000
          logSlowSql: true
          enabled: true
        wall:
          enabled: true
          config:
            drop-table-allow: false
```

 3、输入/druid/* 请求

![1622702296194](C:\Users\86185\AppData\Roaming\Typora\typora-user-images\1622702296194.png)





## 3、整合 Mybatis

SpringBoot 自动配置了 **SqlSessionFactorySqlSession**、 **SqlSessionTemplate 、SqlSession** 等实例



- 导入 mybatis 场景（**内部有 jdbc start，所以不需要我们导入 jdbc 场景了**）

```xml
<dependency>
    <groupId>org.mybatis.spring.boot</groupId>
    <artifactId>mybatis-spring-boot-starter</artifactId>
    <version>2.1.4</version>
</dependency>
```

- mybatis 配置在**application.yaml中**（或则使用全局配置文件 mybatis-config.xml）

```xml
配置mybatis
mybatis:
  config-location: classpath:mybatis/mybatis-config.xml #全局配置文件位置（使用全局配置文件）
  mapper-locations: classpath:mybatis/mapper/*.xml  # mapper映射文件位置
（也可以直接在这里对mabatis进行配置,配置前缀为 mybatis.configuration ）
```

- 编写 Mapper 接口时，使用@Mapper进行注解（或者使用在主程序上使用*@MapperScan*进行扫描）

- 映射 SQL 语句，可以**使用注解（简单语句）**，也可以**使用 mapper 映射文件（困难语句）**

```java
@Mapper
public interface BookMapper {

    @Select("select * from book where id = #{id}")
    public Book getById(int id);

    public List<Book> getAll();
}
```

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper
        PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<mapper namespace="com.dzx.boot.mapper.BookMapper">
    <select id="getAll" resultType="com.dzx.boot.pojo.Book">
        select * from  book
    </select>
</mapper>
```

### 3.1 数据操作

进行数据操作时，跟以前一样采用 mapper、service、serviceimpl 这套方式

```java
@Data
public class Book {

    private int id;
    private String name;
    private String author;
}


public interface BookService {
    public Book getById(int id);
    public List<Book> getAll();
}

@Component
public class BookServiceImpl implements BookService {
    
    @Autowired
    BookMapper bookMapper;
   
   @Override
    public List<Book> getAll() {
        List<Book> all = bookMapper.getAll();
        return all;
    }

    @Override
    public Book getById(int id) {
        Book book = bookMapper.getById(id);
        return book;
    }
}

@Controller
public class TestController {

    @Autowired
    BookServiceImpl bookService;

    @ResponseBody
    @RequestMapping("/bookid")
    public Book test2(@RequestParam("id") int id){
        Book book = bookService.getById(id);
        List<Book> all = bookService.getAll();
        System.out.println(all);
        return book;
    }
```





## 4、整合 MyBatis-Plus 

### 4.1 MyBatis 简介

MyBatis-Plus （简称 MP）是一个[MyBatis 的增强工具，在 MyBatis 的基础上只做增强不做改变，为简化开发、提高效率而生



特性：

- **无侵入**：只做增强不做改变，引入它不会对现有工程产生影响，如丝般顺滑
- **损耗小**：启动即会自动注入基本 CURD，性能基本无损耗，直接面向对象操作
- **强大的 CRUD 操作**：内置通用 Mapper、通用 Service，仅仅通过少量配置即可实现单表大部分 CRUD 操作，更有强大的条件构造器，满足各类使用需求
- **支持 Lambda 形式调用**：通过 Lambda 表达式，方便的编写各类查询条件，无需再担心字段写错
- **支持主键自动生成**：支持多达 4 种主键策略（内含分布式唯一 ID 生成器 - Sequence），可自由配置，完美解决主键问题
- **支持 ActiveRecord 模式**：支持 ActiveRecord 形式调用，实体类只需继承 Model 类即可进行强大的 CRUD 操作
- **支持自定义全局通用操作**：支持全局通用方法注入（ Write once, use anywhere ）
- **内置代码生成器**：采用代码或者 Maven 插件可快速生成 Mapper 、 Model 、 Service 、 Controller 层代码，支持模板引擎，更有超多自定义配置等您来使用
- **内置分页插件**：基于 MyBatis 物理分页，开发者无需关心具体操作，配置好插件之后，写分页等同于普通 List 查询
- **分页插件支持多种数据库**：支持 MySQL、MariaDB、Oracle、DB2、H2、HSQL、SQLite、Postgre、SQLServer 等多种数据库
  - **内置性能分析插件**：可输出 Sql 语句以及其执行时间，建议开发测试时启用该功能，能快速揪出慢查询
- **内置全局拦截插件**：提供全表 delete 、 update 操作智能分析阻断，也可自定义拦截规则，预防误操作



详细了解：https://baomidou.com/guide/





### 4.2 使用

- 导入场景（**包括了mybatis，所以mybatis场景不用导入**）

```xml
<dependency>
    <groupId>com.baomidou</groupId>
    <artifactId>mybatis-plus-boot-starter</artifactId>
    <version>3.4.1</version>
</dependency>
```

- 配置相关属性

yml文件中的配置前缀为：**mybatis-plus**

SqlSessionFactory 自动配置好，底层是容器中默认的数据源

**mapperLocations 自动配置好的，有默认值，classpath:/mapper/.xml**；任意包的类路径下的所有mapper文件夹下任意路径下的所有xml都是 sql 映射文件。  建议以后sql映射文件，放在 mapper下

容器中也自动配置好了 SqlSessionTemplate

- 编写 Mapper 接口

**继承 BaseMapper** ，**可以自动拥有一些 crud 方法（注意要写泛型，不然会报错）**

**@Mapper 标注接口**；也可以使用 @MapperScan批量扫描

- 调用增强对象（Mapper 或者 ServiceImpl 等）进行操作

```java
@Mapper
public interface BookMapper extends BaseMapper<Book> {

}

@Component
public class BookServiceImpl extends ServiceImpl<BookMapper,Book> implements BookService {

   //@Autowired
   //BookMapper bookMapper;   有需要会用到

    }
    
}

@Configuration
@Controller
public class TestController {

    @Autowired
    BookServiceImpl bookService;

    @ResponseBody
    @RequestMapping("/bookid")
    public  List<Book> test2(){
        List list = bookService.list();
        return list;
 }
```



### 4.3 分页功能

- 向容器中加入分页组件

```java
//Spring boot方式
@Configuration
@MapperScan("com.baomidou.cloud.service.*.mapper*")
public class MybatisPlusConfig {

    // 旧版
    @Bean
    public PaginationInterceptor paginationInterceptor() {
        PaginationInterceptor paginationInterceptor = new PaginationInterceptor();
        // 设置请求的页面大于最大页后操作， true调回到首页，false 继续请求  默认false
        // paginationInterceptor.setOverflow(false);
        // 设置最大单页限制数量，默认 500 条，-1 不受限制
        // paginationInterceptor.setLimit(500);
        // 开启 count 的 join 优化,只针对部分 left join
        paginationInterceptor.setCountSqlParser(new JsqlParserCountOptimize(true));
        return paginationInterceptor;
    }
    
    // 最新版
    @Bean
    public MybatisPlusInterceptor mybatisPlusInterceptor() {
        MybatisPlusInterceptor interceptor = new MybatisPlusInterceptor();
        interceptor.addInnerInterceptor(new PaginationInnerInterceptor(DbType.H2));
        return interceptor;
    }
    
}
```

- 在 Controller 中进行处理

```
@RequestMapping("/page/{pn}")
public String page(@PathVariable(value = "pn",required = false) Integer pn, Model model){

    //构造分页参数，pn为当前页数
    Page<Book> page = new Page<>(pn,2);

    //调用page进行分页
    Page<Book> pageinfo = bookService.page(page,null);
    model.addAttribute("pageinfo",pageinfo);
    return "list";
}
```

- 前端页面进行显示


```html
<!DOCTYPE html>
<html lang="en" xmlns:th="http://www.thymeleaf.org">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
</head>
<body>

<table>
    <th>#</th>
    <th>id</th>
    <th>name</th>
    <th>author</th>
    <tr th:each="book,state:${pageinfo.getRecords()}">
        <td th:text="${state.count}"></td>
        <td th:text="${book.getId()}"></td>
        <td th:text="${book.getName()}"></td>
        <td th:text="${book.getAuthor()}"></td>
    </tr>
</table>
当前第[[${pageinfo.getCurrent()}]]页
总共[[${pageinfo.getPages()}]]页
共[[${pageinfo.getTotal()}]]记录
<div>
//有待修改，没有错误校验
<a th:href="@{/page/1}" >首页</a>
<a th:href="@{/page/{pn}(pn=${pageinfo.getCurrent()}-1)}">上一页</a>
<a th:href="@{/page/{pn}(pn=${pageinfo.getCurrent()+1})}">下一页</a>
<a th:href="@{/page/{pn}(pn=${pageinfo.getPages()})}">尾页</a>
</div>
</body>
</html>
```





### 4.4 注意

1. 引入了mybatis-plus 就不需要 mybatis了，不然会相互影响，导致错误
2. 该写泛型的地方最好写上，不然可能会出错
3. **接口**继承 BaseMapper 和**实现类**继承 ServiceImpl 都可以获得 mybatis-plus 为我们提供的方法，我们只需要根据情况去使用接口和实现类即可
4. 除了mybatis-plus框架提供的方法，我们还可以使用自己的方法，但是记得要写对应的 mapper 映射文件
5. 也可以使用提供的分页功能以及各种插件









# 二、整合NoSQL数据库

























































