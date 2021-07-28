# SpringBoot

SpringBoot是整合Spring技术栈的一站式框架

SpringBoot是简化Spring技术栈的快速开发脚手架

能快速创建出生产级别的Spring应用



优点：

- 创建独立Spring应用
- 内嵌web服务器
- 自动starter依赖，简化构建配置
- 自动配置Spring以及第三方功能
- 提供生产级别的监控、健康检查及外部化配置
- 无代码生成、无需编写XML



缺点：

- 人称版本帝，迭代快，需要时刻关注变化

- 封装太深，内部原理复杂，不容易精通

# 特点

## 1.依赖管理

- 父项目做依赖管理

- 开发导入starter场景启动器

- 无需关注版本号，自动版本仲裁

- 可以修改默认版本号

```xml
在当前项目里面重写配置
    <properties>
        <mysql.version>5.1.43</mysql.version>
    </properties>
```



## 2.自动配置

- 自动配好Tomcat
- 自动配好SpringMVC
- 自动配好Web常见功能，如：字符编码问题
- 默认的包结构

主程序**所在包及其下面的所有子包**里面的组件都会被默认扫描进来

想要改变扫描路径:

1.@SpringBootApplication(scanBasePackages=**"com.atguigu"**)

2.使用@ComponentScan 指定扫描路径


```
@SpringBootApplication
等同于
@SpringBootConfiguration
@EnableAutoConfiguration
@ComponentScan("com.atguigu.boot")
```



# 入门实现

1、引入依赖（web场景）

```xml
<parent>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-parent</artifactId>
        <version>2.3.4.RELEASE</version>
</parent>
	
    <!-不同的场景引入不同的依赖->
    <dependencies>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-web</artifactId>
        </dependency>

</dependencies>
```

2、创建主程序

```java
/**
 * 主程序类
 * @SpringBootApplication：这是一个SpringBoot应用
 */
@SpringBootApplication
public class MainApplication {

    public static void main(String[] args) {
        SpringApplication.run(MainApplication.class,args);		//返回一个Ioc容器，可以选择接收
    }
}
```

# 相关注解

（@Component、@Controller、@Service、@Repository 于 Spring 一致）

1.**@Configuration**：告诉 SpringBoot 这是一个配置类 （ 配置文件），**配置类本身也是组件**

```java
@Configuration(proxyBeanMethods = false)
public class MyConfig {
}
```

proxyBeanMethods：

- true：对应 **Full** 模式，保证每个@Bean方法被调用多少次返回的组件都是**单例**的
- false：对应 **Lite** 模式，保证每个@Bean方法被调用多少次返回的组件都是**新创建**的

（组件依赖必须使用Full模式默认。其他默认是否Lite模式）

2.**@Bean**：给容器中添加组件。(没有 name 值)以方法名作为组件的id。返回类型就是组件类型。返回的值，就是组件在容器中的实例

```java
 @Bean("tom")				//有name值，所以使用name作为组件的id
 public Pet tomcatPet(){
        return new Pet("tomcat");
 }
```

3.**@ComponentScan**：指定要扫描的包

4.**@Import**：让容器中自动创建出某个类型的组件、默认组件的名字就是全类名

```java
@Import({User.class, DBHelper.class})//给容器中自动创建出这两个类型的组件、默认组件的名字就是全类名
```

5.**@Conditional**：条件装配，满足Conditional指定的条件，则进行组件注入，有很多子类

6.**@ImportResource**：映入原生的 XML 配置文件

```java
@ImportResource("classpath:beans.xml")
```

7.**@ConfigurationProperties**：读取配置文件（**只有在容器中的组件才能读取配置文件**）

**@EnableConfigurationProperties**：1、开启；2、注册到容器**(在其他的类上使用)**

```java
/**
 * 只有在容器中的组件，才会拥有SpringBoot提供的强大功能
 */
@Component
@ConfigurationProperties(prefix = "mycar")	//将配置文件中前缀为 mycar 的数据与属性相绑定
public class Car {
}
```

```java
@EnableConfigurationProperties（Car.class)
public class MyConfig {
}
//1、开启Car配置绑定功能
//2、把这个Car这个组件自动注册到容器中
```

（写在其他类上）@EnableConfigurationProperties = （写在本类上）@ConfigurationProperties + @Component

8.**@SpringBootConfiguration**：里面包含了@Configuration，代表当前是一个配置类或主程序类

9.**@EnableAutoConfiguration**：启用 SpringBoot 的自动配置机制，帮助 SpringBoot 应用将所有符合条件的@Configuration 配置都加载到当前 SpringBoot

SpringBoot默认会在底层配好所有的组件。但是如果用户自己配置了以用户的优先



**总结：**

- SpringBoot先加载所有的自动配置类  xxxxxAutoConfiguration
- 每个自动配置类按照条件进行生效，默认都会绑定配置文件指定的值。xxxxProperties，xxxProperties和配置文件进行了绑定
- 生效的配置类就会给容器中装配很多组件
- 只要容器中有这些组件，相当于这些功能就有了
- 定制化配置

- - 用户直接自己@Bean替换底层的组件
  - 用户去看这个组件是获取的配置文件什么值就去修改。

**xxxxxAutoConfiguration ---> 组件  --->** **xxxxProperties里面拿值  ----> application.properties**



# 小工具

（配置文件中debug=true开启自动配置报告）

dev-tools：（伪）动态刷新项目

Spring Initailizr：项目初始化向导，快速创建 SpringBoot 项目



