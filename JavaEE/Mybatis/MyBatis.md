# MyBatis

ORMapping：Object Relationship Mapping 对象关系映射

对象：面向对象

关系：关系型数据库

映射：Java 到数据库的映射，开发者可以通过面向对象的思想来管理数据库



### 实现步骤

1.准备工作：

- 在 pom.xml 中导入相关依赖（jar 包）

```xml
<dependency>
    <groupId>org.mybatis</groupId>
    <artifactId>mybatis</artifactId>
    <version>3.5.6</version>
</dependency>
<!-- 根据数据库 -->
<dependency>
    <groupId>mysql</groupId>
    <artifactId>mysql-connector-mxj</artifactId>
    <version>5.0.12</version>
</dependency>
```

- 创建表以及于表相对应的实体类
- 创建 MyBatis 的配置文件 **config.xml**，文件名可自定义

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE configuration PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-config.dtd">
<configuration>
    <!--配置MyBatis运行环境-->
    <environments default="devloping">
        <environment id="devloping">
            <!--配置JDBC事务管理-->
            <transactionManager type="JDBC"></transactionManager>
            <!--POOLED配置JDBC数据库连接池-->
            <dataSource type="POOLED">
                <property name="driver" value="com.mysql.jdbc.Driver"/>
                <property name="url" value="jdbc:mysql://localhost:3306/demo?useUnicode=true&amp;characterEncoding=UTF-8"/>
                <property name="username" value="root"/>
                <property name="password" value="228228"/>
            </dataSource>
        </environment>
    </environments>
</configuration>
```

2：实现方式

- 使用原生接口
- 通过 Mapper 代理实现自定义接口（推荐）

> 使用原生接口

 1.MyBatis 需要开发者自己写 SQL 语句，存放在 **Mapper.xml** 文件中（名字自定义），实际开发中，会为每个实体类创建对应的 Mapper.xml 文件，定义、管理该对象数据的 SQL

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<mapper namespace="Mapper">
    <select id="find" resultType="com.dzx.exmple.myBatis.User">
        select * from abc
    </select>
</mapper>
```

常见属性/标签：

namespace：标识空间名字，用于查找，通常设置为文件所在包+文件名的形式

insert/select/update/delete 标签：执行相关操作

id：是实际调用 MyBatis **方法**时需要用到的参数

parameterType：参数的数据类型

resultType：返回的结果类型

2.在 MyBatis 的配置文件 config.xml 中注册 Mapper.xml，放在<configuration>中

```xml
<mappers>
    <mapper resource="Mapper.xml"></mapper>		//resources:文件所在位置(例子是存放在resources中)
</mappers>
```

3.调用 MyBatis 的原生接口执行操作（查询）

```java
package com.dzx.exmple.myBatis;

import org.apache.ibatis.session.SqlSession;
import org.apache.ibatis.session.SqlSessionFactory;
import org.apache.ibatis.session.SqlSessionFactoryBuilder;

import java.io.InputStream;
import java.util.List;

public class Test {
    public static void main(String[] args) {
        //加载MyBatis配置文件
        InputStream inputStream = Test.class.getClassLoader().getResourceAsStream("config.xml");
        SqlSessionFactoryBuilder sqlSessionFactoryBuilder = new SqlSessionFactoryBuilder();
        SqlSessionFactory sqlSessionFactory = sqlSessionFactoryBuilder.build(inputStream);
        SqlSession sqlSession = sqlSessionFactory.openSession();
        //调用Mapper.xml中的SQL（Mapper的namespace+SQL标签的id属性）
        String statement = "Mapper.find";
        List<User> list = sqlSession.selectList(statement);
        //遍历
        for (User user:list){
            System.out.println(user);
        }
    }
}
```

**statement：设置为 <mapper> 的 namespace + statement 标签（select 、update、delete、insert）对应的 id 属性**

> 通过Mapper代理实现自定义接口

1.自定义接口，定义相关业务的方法

```java
package com.dzx.exmple.myBatis;

public interface UserRepository {
    public int insert(User user);//增
    public int delete(long id);  //删
    public int update(User user);//改
    public User find(long id);   //查
}
```

2.创建与接口对应的 **Mapper.xml** 文件，定义与接口方法对应的 **SQL** 语句

statement 标签可根据业务需求选择 insert、delete、update、select

MyBatis 会根据规则自动创建**接口实现类的代理对象**

规则：

- Mapper.xml 中 namespace 为接口的**全类名**
- Mapper.xml 中 statement 的 id 为接口中对应的**方法名**
- Mapper.xml 中 statement 的 parameterType 和接口中对应方法的**参数类型**一致
- Mapper.xml 中 statement 的 resultType 和接口中对应方法的**返回类型**一致

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<mapper namespace="com.dzx.exmple.myBatis.UserRepository">
    
    <!--insert、update、delete返回类型只能是int型，所以可以省略不写-->
    <insert id="insert" parameterType="com.dzx.exmple.myBatis.User">
        insert into abc(id,name) value (#{id},#{name})
    </insert>

    <delete id="delete" parameterType="long">
        delete abc from abc where id=#{id}
    </delete>

    <update id="update" parameterType="com.dzx.exmple.myBatis.User">
        update abc set name=#{name} where id=#{id}
    </update>

    <select id="find" parameterType="long" resultType="com.dzx.exmple.myBatis.User">
        select * from abc where id=#{id}
    </select>
</mapper>
```

关于 parameterType 和 resultType 的参数类型：

1.基本数据类型：直接写，例如 parameterType="long"

2.封装类型：java.lang 包+封装类，例如 parameterType="java.lang.Long"

3.java bean：类的全类名，例如 parameterType="com.dzx.exmple.myBatis.User"

**当参数有多个时可以不写，一般值用 #{属性名} 表示**

3.在 MyBatis 配置文件 config.xml 中注册对应的 Mapper.xml

```xml
<!--注册Mapper.xml-->
<mappers>
    <mapper resource="MapperUser.xml"></mapper>
</mappers>
```

4.调用接口的代理对象完成业务操作

```java
package com.dzx.exmple.myBatis;

import org.apache.ibatis.session.SqlSession;
import org.apache.ibatis.session.SqlSessionFactory;
import org.apache.ibatis.session.SqlSessionFactoryBuilder;

import java.io.InputStream;
import java.util.List;

public class Test {
    public static void main(String[] args) {
        //加载MyBatis配置文件
        InputStream inputStream = Test.class.getClassLoader().getResourceAsStream("config.xml");
        SqlSessionFactoryBuilder sqlSessionFactoryBuilder = new SqlSessionFactoryBuilder();
        SqlSessionFactory sqlSessionFactory = sqlSessionFactoryBuilder.build(inputStream);
        SqlSession sqlSession = sqlSessionFactory.openSession();

        //获取实现接口的代理对象
        UserRepository userRepository = sqlSession.getMapper(UserRepository.class);

        //查
        System.out.println(userRepository.find(1));
        //增
        User user =new User(14,"DZX");
        userRepository.insert(user);
        sqlSession.commit();
        //改
        user.setName("邓志雄");
        userRepository.update(user);
        sqlSession.commit();
        //删
        userRepository.delete(14);
        sqlSession.commit();
    }
}
```

**注意：在执行 insert、updata、delete操作时要进行 commit，select不用**



### 及联查询

- 一对一：在任意一方引入对方主键作为外键
- 一对多：在”多“的一方，添加”一“的一方的主键作为外键
- 多对多：产生中间关系表，引入两张表的主键作为外键，两个主键成为联合主键或使用新的字段作为主键

**什么是延迟加载？** 

延迟加载也叫懒加载、惰性加载，使用可以提高程序的运行效率，比如一个用户查询操作要先访问 user 表，再访问address 表，但是如果我们只需要用户表中的东西，那么就不用去访问 address 表了，这就是延迟加载，即在某些特定的情况下去访问特定的表，在其他情况下可以不访问某些表，从一定程度上减少了 java 与数据库的交互次数

启用方式：

- （全局）在 MyBatis 配置文件中设置<setting> （Mybatis 默认不开启延迟加载）

- ```
  <settings>
      <!-- 打印SQL-->
      <setting name="logImpl" value="STDOUT_LOGGING" />
      <!-- 开启延迟加载 -->
      <setting name="lazyLoadingEnabled" value="true"/>
  </settings>
  ```

  （局部）在<association>或者<collection>中设置 fetchType 属性（lazy，eager），**（1）如果 MyBatis 中设置了延迟加载，那么 <association>或者<collection>中 fetchType 默认为 lazy，要使用延迟加载就可以不用进行设置了，但是如果不使用延迟加载将属性设置为 eager 即可**；**（2）如果 MyBatis 中没有设置延迟加载，<association>或者<collection>中 fetchType 默认为 eager，要使用延迟加载需将属性值改为 lazy**

以下代码：查询 name 时，第一次查询 user 表就可以查询到 name，但是还是执行了子查询去查了 address 表，使用延迟加载后，就只需要查询 user 表，不用查询 address 表了，因为 MyBatis 中没有设置，所以默认是 eager，因此我们需要将属性值设为 lazy

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN" "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<mapper namespace="AddressMapper">
<select id="find" parameterType="long" resultType="com.dzx.exmple.spring.Address">
    select * from address where id = #{id}
</select>
</mapper>
```

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<mapper namespace="com.dzx.exmple.myBatis.UserRepository">
    <select id="find" parameterType="long" resultMap="FindMap">
        select * from abc where id=#{id}
    </select>
    <resultMap id="FindMap" type="com.dzx.exmple.myBatis.User">
        <id property="id" column="id"></id>
        <result property="name" column="name"></result>
        <association property="address" column="addressid" javaType="com.dzx.exmple.spring.Address" select="AddressMapper.find" fetchType="lazy"/>//设置延迟加载
    </resultMap>
</mapper>
```

```java
public class Test3 {
    public static void main(String[] args) {
        InputStream inputStream = Test.class.getClassLoader().getResourceAsStream("config.xml");
        SqlSessionFactoryBuilder sqlSessionFactoryBuilder = new SqlSessionFactoryBuilder();
        SqlSessionFactory sqlSessionFactory = sqlSessionFactoryBuilder.build(inputStream);
        SqlSession sqlSession = sqlSessionFactory.openSession();
        UserRepository userRepository = sqlSession.getMapper(UserRepository.class);
        System.out.println(userRepository.find(2).getName());
        sqlSession.close();
    }
}
```



##### 一对一

在本类中定义对方类型的对象，如A类中定义B类类型的属性b，或者B类中定义A类类型的属性a

User 中定义 Address 类，根据 id 查询 User，用 <association>（存在 rusultmap 中） 来处理

<association>标签属性：

- property：指定映射到的实体类**对象属性**，与表字段一一对应
- column：指定表中对应的字段
- javaType：指定映射到实体对象**属性的类型**
- select：指定引入嵌套查询的子SQL语句，该属性用于关联映射中的嵌套查询
- fetchType：指定在关联查询中是否启用延迟加载，值为 lazy(延迟加载)、eager（立即加载），详细见上

使用方式：

```xml
<!--方式一：嵌套查询 -->（将字段（column）的值作为子查询的参数）
<association property="address" column="addressid" javaType="com.dzx.exmple.spring.Address" select="AddressMapper.find" />

<!--方式二：嵌套结果-->（将上一次查询出来的结果进行对照，把字段的值赋给对应的对象属性）
<association property="address" javaType="com.dzx.exmple.spring.Address" fetchType="eager">
        <id property="id" column="addressid"></id>
        <result property="context" column="context"></result>
</association>
```

具体代码：

```java
package com.dzx.exmple.myBatis;

import com.dzx.exmple.spring.Address;
import lombok.AllArgsConstructor;
import lombok.Data;
import lombok.NoArgsConstructor;

@AllArgsConstructor
@NoArgsConstructor
@Data
public class User {
    private int id;
    private String name;
    private Address address;
}
```

```java
package com.dzx.exmple.spring;

import lombok.AllArgsConstructor;
import lombok.Data;
import lombok.NoArgsConstructor;

@AllArgsConstructor
@NoArgsConstructor
@Data
public class Address {
    private int id;
    private String context;
}
```

AddressMapper.xml

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN" "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<mapper namespace="AddressMapper">
<select id="find" resultType="com.dzx.exmple.spring.Address">
    <！--根据addressid 查询地址内容-->
    select * from address where id = #{id}
</select>
</mapper>
```

UserMapper.xml

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<mapper namespace="com.dzx.exmple.myBatis.UserRepository">
    <select id="find" parameterType="long" resultMap="FindMap">
        select * from abc where id=#{id}
    </select>
    <resultMap id="FindMap" type="com.dzx.exmple.myBatis.User">
        <id property="id" column="id"></id>
        <result property="name" column="name"></result>
        
        <!--一对一：association使用select属性引入另外一条SQL语句-->
        <association property="address" column="addressid" javaType="com.dzx.exmple.spring.Address" select="AddressMapper.find"/>
    </resultMap>
</mapper>
```

config.xml

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE configuration PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-config.dtd">
<configuration>

    <settings>
        <!-- 打印SQL-->
        <setting name="logImpl" value="STDOUT_LOGGING" />
        <!-- 开启延迟加载 -->
        <setting name="lazyLoadingEnabled" value="true"/>
    </settings>
    <!--配置MyBatis运行环境-->
    <environments default="devloping">
        <environment id="devloping">
            <!--配置JDBC事务管理-->
            <transactionManager type="JDBC"></transactionManager>
            <!--POOLED配置JDBC数据库连接池-->
            <dataSource type="POOLED">
                <property name="driver" value="com.mysql.jdbc.Driver"/>
                <property name="url" value="jdbc:mysql://localhost:3306/demo?useUnicode=true&amp;characterEncoding=UTF-8"/>
                <property name="username" value="root"/>
                <property name="password" value="228228"/>
            </dataSource>
        </environment>
    </environments>

    <!--注册Mapper.xml-->
    <mappers>
        <mapper resource="UserMapper.xml"></mapper>
        <mapper resource="AddressMapper.xml"></mapper>
    </mappers>
</configuration>
```



##### 一对多

一个A类类型对应多个B类类型的情况，需要在A类中以**集合**的方式引入B类类型的对象，在B类中定义A类类型的属性a

1个 Classes 对应 多个 Student，用<collection> 处理

<collection> 标签属性：

- property：指定映射到的实体类**对象属性**，与表字段一一对应
- column：指定表中对应的字段
- ofType：List列表中**泛型的类型**
- select：指定引入嵌套查询的子SQL语句，该属性用于关联映射中的嵌套查询
- fetchType：指定在关联查询中是否启用延迟加载，值为 lazy(延迟加载)、eager（立即加载），详细见上

```xml
<!--方式一：嵌套查询-->
<collection property="students" column="id" ofType="com.dzx.exmple.myBatis.Student" select="com.dzx.exmple.myBatis.UserRepository.find"/>


<!--方式二：嵌套结果-->
<collection property="students" ofType="com.dzx.exmple.myBatis.Student">
        <id property="id" column="id"></id>
        <result property="name" column="name"></result>
</collection>
```

关键代码：使用嵌套结果，查询学生显示班级，查询班级显示所有的学生

```java
package com.dzx.exmple.myBatis;

import lombok.Data;

@Data
public class Student {
    private long id;
    private String name;
    private Classes classes;
}
```

```java
package com.dzx.exmple.myBatis;

import lombok.Data;
import java.util.List;

@Data
public class Classes {
    private long id;
    private String cname;
    private List<Student> students;
}
```

StudentMapper.xml

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<mapper namespace="com.dzx.exmple.myBatis.UserRepository">
    <select id="find" parameterType="long" resultMap="StudentMap">
        select * from student s,classes c where s.id = #{id} and c.id=s.cid
    </select>
    <resultMap id="StudentMap" type="com.dzx.exmple.myBatis.Student">
        <id property="id" column="id"></id>
        <result property="name" column="name"></result>
        <association property="classes" column="cid" javaType="com.dzx.exmple.myBatis.Classes">
            <id property="id" column="cid"/>
            <result property="cname" column="cname"/>
        </association>
    </resultMap>
</mapper>
```

ClassMapper.xml

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<mapper namespace="com.dzx.exmple.myBatis.ClassFind">
    <select id="Findbyid" parameterType="long" resultMap="ClassMap">
        select s.id,s.name,c.id as cid,c.cname from student s,classes c where c.id=s.cid and c.id=#{id}
    </select>

    <resultMap id="ClassMap" type="com.dzx.exmple.myBatis.Classes">
        <id property="id" column="cid"></id>
        <result property="cname" column="cname"/>
        <collection property="students" column="id" ofType="com.dzx.exmple.myBatis.Student">
                <id property="id" column="id"></id>
                <result property="name" column="name"></result>
        </collection>
    </resultMap>

</mapper>
```



##### 多对多

在A类中定义B类类型的集合，在B类中定义A类类型的集合

多个客户与多个商品之间的关系

```java
package com.dzx.exmple.myBatis;

import lombok.Data;

import java.util.List;

@Data
public class Custom {
    private long id;
    private String name;
    private List<Product> products;
}
```

```java
package com.dzx.exmple.myBatis;

import lombok.Data;

import java.util.List;

@Data
public class Product {
    private long id;
    private String name;
    private List<Custom> customs;
}
```

```java
package com.dzx.exmple.myBatis;

public interface ProductRepository {
    public Product findById(long id);
}
```

```java
package com.dzx.exmple.myBatis;

public interface CustomRepository {
    public Custom findById(long id);
}
```

ProductMapper.xml

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<mapper namespace="com.dzx.exmple.myBatis.ProductRepository">
    <select id="findById" resultMap="promap">
        select c.id as cusid,c.name as cname,b.*,p.id as proid,p.name as pname from custom c,product p,buy b where p.id = #{id} and b.cid=c.id and b.pid=p.id
    </select>
    <resultMap id="promap" type="com.dzx.exmple.myBatis.Product">
        <id property="id" column="proid"></id>
        <result property="name" column="pname"></result>
        <collection property="customs" ofType="com.dzx.exmple.myBatis.Custom">
            <id property="id" column="cusid"></id>
            <result property="name" column="cname"></result>
        </collection>
    </resultMap>
</mapper>
```

CustomMapper.xml

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<mapper namespace="com.dzx.exmple.myBatis.CustomRepository">
    <select id="findById" resultMap="custommap">
        select c.id as cusid,c.name as cname,b.*,p.id as proid,p.name as pname from custom c,product p,buy b where c.id = #{id} and b.cid=c.id and b.pid=p.id
    </select>
    <resultMap id="custommap" type="com.dzx.exmple.myBatis.Custom">
        <id property="id" column="cusid"></id>
        <result property="name" column="cname"></result>
        <collection property="products" ofType="com.dzx.exmple.myBatis.Product">
            <id property="id" column="proid"></id>
            <result property="name" column="pname"></result>
        </collection>
    </resultMap>
</mapper>
```

#### 总结

一对一，其中一方使用 association

一对多，一使用 collection ，多使用 association

多对多，双方都使用 collection

**总而言之，如果是非集合，用 association，如果是集合，用 collection**



### 逆向工程

MyBatis 框架需要：实体类、自定义 Mapper 接口、Mapper.xml 

传统上这三个组件需要开发者手动创建，而**逆向工程可以帮助我们自动创建这些东西**

**MyBatis Generator，简称 MBG**，是一个专门服务于 MyBatis 框架的代码生成器，可以自动生成实体类、自定义 Mapper 接口、Mapper.xml ，支持基本的CRUD操作，但是一些复杂的SQL需要开发者自己完成

实现步骤：

- **添加依赖**

```xml
<dependency>
<groupId>org.mybatis.generator</groupId>
<artifactId>mybatis-generator-core</artifactId>
<version>1.3.2</version>
</dependency>
```

- **创建 MBG 配置文件 generatorConfifig.xml**

1、jdbcConnection：配置数据库连接信息。 

2、javaModelGenerator：配置 JavaBean 的⽣成策略。 

3、sqlMapGenerator 配置 SQL 映射⽂件⽣成策略。 

4、javaClientGenerator：配置 Mapper 接⼝的⽣成策略。 

5、table 配置⽬标数据表：（tableName：表名，domainObjectName：JavaBean 类名）。 

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE generatorConfiguration
        PUBLIC "-//mybatis.org//DTD MyBatis Generator Configuration 1.0//EN"
        "http://mybatis.org/dtd/mybatis-generator-config_1_0.dtd">
<generatorConfiguration>
    <context id="testTables" targetRuntime="MyBatis3">
        <jdbcConnection
                driverClass="com.mysql.jdbc.Driver"
                connectionURL="jdbc:mysql://localhost:3306/demo?useUnicode=true&amp;characterEncoding=UTF-8"
                userId="root"
                password="228228"
        ></jdbcConnection>
        <javaModelGenerator targetPackage="com.dzx.exmple.demo"
                            targetProject="./src/main/java"></javaModelGenerator>
        <sqlMapGenerator targetPackage="/"
                         targetProject="./src/main/resources"></sqlMapGenerator>
        <javaClientGenerator type="XMLMAPPER"
                             targetPackage="com.dzx.exmple.demo" targetProject="./src/main/java">
        </javaClientGenerator>
        <table tableName="abc" domainObjectName="com.dzx.exmple.myBatis.User"></table>
    </context>
</generatorConfiguration>
```

- 创建 Generator 执行类


```java
package com.dzx.exmple.myBatis;


import org.mybatis.generator.api.MyBatisGenerator;
import org.mybatis.generator.config.Configuration;
import org.mybatis.generator.config.xml.ConfigurationParser;
import org.mybatis.generator.exception.InvalidConfigurationException;
import org.mybatis.generator.exception.XMLParserException;
import org.mybatis.generator.internal.DefaultShellCallback;

import java.io.File;
import java.io.IOException;
import java.sql.SQLException;
import java.util.ArrayList;
import java.util.List;

public class Main {
    public static void main(String[] args) {
        List<String> warings = new ArrayList<String>();
        boolean overwrite = true;
        String genCig = "/generatorConfig.xml";
        File configFile = new File(Main.class.getResource(genCig).getFile());
        ConfigurationParser configurationParser = new
                ConfigurationParser(warings);
        Configuration configuration = null;
        try {
            configuration = configurationParser.parseConfiguration(configFile);
        } catch (IOException e) {
            e.printStackTrace();
        } catch (XMLParserException e) {
            e.printStackTrace();
        }
        DefaultShellCallback callback = new DefaultShellCallback(overwrite);
        MyBatisGenerator myBatisGenerator = null;
        try {
            myBatisGenerator = new
                    MyBatisGenerator(configuration,callback,warings);
        } catch (InvalidConfigurationException e) {
            e.printStackTrace();
        }
        try {
            myBatisGenerator.generate(null);
        } catch (SQLException e) {
            e.printStackTrace();
        } catch (IOException e) {
            e.printStackTrace();
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
    }
}
```

大多数代码直接用，少些地方需要修改数据信息

详见：https://blog.csdn.net/qiaqia609/article/details/80956266?utm_medium=distribute.pc_relevant.none-task-blog-baidujs_title-0&spm=1001.2101.3001.4242



### MyBatis缓存

使用缓存可以减少 java 与数据库的交互次数，提高程序的运行效率。比如查询 id =1的对象时，第一次查之后会自动将该对象保存到缓存中，下一次查询时，直接从缓存中取出对象即可，无需再次访问数据库

分类：

- **一级缓存：SqlSession 级别，默认开启，并且不能关闭**

操作数据库时需要创建 SqlSession 对象，在对象中有一个 HashMap 用于存储缓存数据，不同的 SqlSession 之间缓存数据区域互不影响

一级缓存的作用域是 SqlSession 范围的，当同一个 SqlSession 执行多次相同的 SQL 语句时，第一次执行完毕后会将结果保存到缓存中，之后的查询会直接从缓存中获取

![1616076289044](C:\Users\86185\AppData\Roaming\Typora\typora-user-images\1616076289044.png)

​						    （相同同的SqlSession执行相同的SQL语句，执行了一次）

![1616076544128](C:\Users\86185\AppData\Roaming\Typora\typora-user-images\1616076544128.png)

​							（不同的SqlSession执行相同的SQL语句，执行了两次）

**需要注意的是，如果 SqlSession 执⾏了 DML 操作（insert、update、delete），MyBatis 必须将缓存清空以保证数据的准确性**

- **二级缓存：Mapper 级别，默认关闭，可以开启**

使用二级缓存时，多个 SqlSession 使用同一个 Mapper 的 SQL 语句操作数据库，得到的数据会存在二级缓存区，同样是使用 HashMap 进行数据存储，相比于一级缓存，二级缓存的范围更大，多个 SqlSession 可以共用二级缓存，二级缓存是跨 SqlSession 的

二级缓存是多个 SqlSession 共享的，其作用域是 **Mapper 的同一个 namespace** ，不同的 SqlSession 多次执行相同的 namespace 下的 SQL 语句，且参数也相等，则第一次执行成功后会将数据保存到二级缓存中，下一次执行可直接从二级缓存中取出数据



**！！！注意：为解决二级缓存失效的问题，第一次 SqlSession 执行后必须要提交！！！**

（原因：第一次必须先将数据提交到二级缓存区，下一次其他的 SqlSession 才能在缓存区取数据 ）



**方式一：MyBatis 自带的二级缓存** 

1.在 MyBatis 配置文件中开启二级缓存

```xml
<settings>
    <!-- 开启二级缓存 -->
    <setting name="cacheEnabled" value="true"/>
</settings>
```

2.在对应的 Mapper.xml 文件中 <mapper>标签里配置二级缓存

```xml
<cache/>
```

3.**相关的**实体类实现 Serializable（序列化）接口

```java
//查询操作需要用到 User 和 Address 两个实体类，所以两个都要实现序列化接口
public class User implements Serializable {
    private int id;
    private String name;
    private Address address;
}
public class Address implements Serializable {
    private int id;
    private String context;
}
```

结果：

![1616080069077](C:\Users\86185\AppData\Roaming\Typora\typora-user-images\1616080069077.png)

两个不同的 SqlSession 执行相同的 namespace 下的 SQL 语句，且参数也相等，SQL语句只执行了一次



**方式二：ehcache 二级缓存**

1.添加相关依赖

```xml
<dependency>
	<groupId>org.mybatis</groupId>
	<artifactId>mybatis-ehcache</artifactId>
	<version>1.0.0</version>
</dependency>
<dependency>
	<groupId>net.sf.ehcache</groupId>
	<artifactId>ehcache-core</artifactId>
	<version>2.4.3</version>
</dependency>
```

2.添加 ehcache.xml

```xml
<ehcache>
    <diskStore/>
    <defaultCache
            maxElementsInMemory="1000"
            maxElementsOnDisk="10000000"
            eternal="false"
            overflowToDisk="false"
            timeToIdleSeconds="120"
            timeToLiveSeconds="120"
            diskExpiryThreadIntervalSeconds="120"
            memoryStoreEvictionPolicy="LRU">
    </defaultCache>
</ehcache>
```

3.在 MyBatis 配置文件中配置二级缓存

```xml
<settings>
    <!-- 开启二级缓存 -->
    <setting name="cacheEnabled" value="true"/>
</settings>
```

4.对应的 Mapper.xml 中配置二级缓存

```xml
<cache type="org.mybatis.caches.ehcache.EhcacheCache">
    <!-- 缓存创建之后，最后⼀次访问缓存的时间⾄缓存失效的时间间隔 -->
    <property name="timeToIdleSeconds" value="3600"/>
    <!-- 缓存⾃创建时间起⾄失效的时间间隔 -->
    <property name="timeToLiveSeconds" value="3600"/>
    <!-- 缓存回收策略，LRU表示移除近期使⽤最少的对象 -->
    <property name="memoryStoreEvictionPolicy" value="LRU"/>
</cache>
```

**5.实体类不需要实现序列化接口**

结果：

![1616080823587](C:\Users\86185\AppData\Roaming\Typora\typora-user-images\1616080823587.png)

与方式一结果一致，方式二相较于方式一，实体类不用实现接口，但需要添加依赖和配置 ehcache.xml，**切记第一个SqlSession 要提交**



### 动态 SQL

使⽤动态 SQL 可简化代码的开发，减少开发者的⼯作量，程序可以⾃动根据业务参数来决定 SQL 的组成

标签：

- if：判断语句，用于单条件分支判断
- choose（when、otherwise）：相当于 switch（case、default），用于多条件分支判断
- where、trim、set：辅助元素，一般用于处理SQL拼装、特殊字符问题
- foreach：循环语句，常用于 in 语句等列举条件中
- bind：从 OGNL 表达式中创建一个变量，并将其绑定到上下文，常用于模糊查询

**< if > 元素**

```xml
<if test="判断表达式">
	//sql
</if>
```

**< choose >、< when > 、< otherwise >元素**

```xml
<choose>
	<when test="表达式">
		//sql
	</when>
	......
	<otherwise>
		//sql
	</otherwise>
</choose>
```

**< where >元素**

```xml
<where>
	...
</where>
```

只有<where>之内的条件成立时，才会加入 where 关键字，否则不会添加，如果 where 后面加了 "and" 或者"or" ，and 和 or 会被自动删除

**< trim >元素**

```xml
<trim prefix="where" perfixOverrides="and">
	...
</trim>
```

<trim>作用时除去一些特殊的字符串

prefix：添加在 sql 语句的前缀

suffix：添加在 sql 语句的后缀

perfixOverrides：除去 sql 语句前面的特殊字符，该特殊字符由 perfixOverrides 指定

suffixOverrides：除去 sql 语句后面的特殊字符，该特殊字符由 suffixOverrides 指定

**< set >**

```xml
<set>
	...
</set>
```

<set>元素会动态前置 set 关键字（在每句满足条件的 sql 前添加 set），同时会消除 sql 语句中最后一个多余的逗号

如果<set>元素内包含的内容都为空，会报错，因此要保证传入的字段**不能都为空**

**< foreach >元素**

```xml
select * from t_table where id in
<foreach item="id" index="index" collection="list" open="(" separator="," close=")">
	#{id}
</foreach>
```

item：循环中当前的元素

index：当前元素在集合的位置下标

collection：传递过来的参数类型（首字母小写），它可以是 array、list（collection）、map等等

open 和 close：配置的是以什么符号将这些集合元素包装起来

separator：各个元素的间隔符