# Spring MVC

Spring MVC 是⽬前主流的实现 MVC 设计模式的企业级开发框架，Spring 框架的⼀个⼦模块，⽆需整 

合，开发起来更加便捷



## MVC 设计模式

Model + View +Controller 

Controller 接收客户请求，调用 Model 生成业务数据，传递给 View

Spring MVC 对这一过程进行了封装



## 核心组件

- DispatcherServlet：前置控制器，是整个流程控制的核心，控制其他组件的执行，进行统一调度，相当于总指挥
- Handler：处理器，完成具体的业务逻辑，相当于 Servlet 或 Action
- HandlerMapping：DispatcherServlet 接收到请求之后，通过 HandlerMapping 将不同的请求映射到不同的 Handler
- HandlerInterceptor：处理器拦截器，是一个接口，如果需要完成拦截处理，可以实现该接口
- HandlerExecutionChain：处理器执行链，包含 Handler 和 HandlerInterceptor （系统自带的，如果需要，可额外添加）
- HandlerAdapter：处理器适配器，在 Handler 执行业务方法之前自动完成一些操作（表单数据的验证、数据类型的转换、封装数据等），开发者只需要将注意力集中在业务逻辑上即可，DispatcherServlet 通过 HandlerAdapter 调用不同的 Handler 
- ModelAndView：装载了模型数据和视图信息，作为 Handler 的处理结果，返回给 DispatcherServlet
- ViewResolver：视图解析器，DispatcherSevlet 通过 ViewResolver 将逻辑视图解析为物理视图，最终将渲染结果响应给客户端



## 工作流程

![1617094373036](C:\Users\86185\AppData\Roaming\Typora\typora-user-images\1617094373036.png)

1. DispatcherServlet 接收客户端请求
2. DispatcherServlet 调用 HandlerMapping，根据不同的请求映射到不同的 Handler
3. HandlerMapping 将 Handler 和 HandlerInterceptor 以 HandlerExecutionChain 的形式一并返回给 DispatcherServlet
4. DispatcherServlet 通过 HandlerAdapter 自动完成一些准备工作（例如数据封装等），然后调用 Handler 的方法完成业务逻辑处理
5. Handler 返回一个 ModelAndView 给 DispatcherServlet
6. DispatcherServlet 将获取的 ModelAndVIew 对象传给 ViewResolver，将逻辑视图解析为物理视图 View
7. DispatcherServlet 根据 View 进行视图渲染（将模型数据 Model 填充到视图 View 中） 
8. DispatcherSerlvet 将渲染后的结果响应给客户端

Spring MVC 流程非常复杂，但实际开发中很简单，大部分底层代码都被封装了，我们只要完成一些配置文件即可，此外还要处理 Handler 和 View



## 实现步骤

1.添加依赖

```xml
<dependency>
  <groupId>org.springframework</groupId>
  <artifactId>spring-webmvc</artifactId>
  <version>5.3.4</version>
</dependency>
```

2.在 Web.xml 中配置 DispatcherServlet

```xml
<!DOCTYPE web-app PUBLIC
 "-//Sun Microsystems, Inc.//DTD Web Application 2.3//EN"
 "http://java.sun.com/dtd/web-app_2_3.dtd" >

<web-app>
  <display-name>Archetype Created Web Application</display-name>
  <servlet>
    <servlet-name>dispatcherServlet</servlet-name>
    <servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
    <init-param>
      <param-name>contextConfigLocation</param-name>
      <param-value>classpath:springmvc.xml</param-value>
    </init-param>
  </servlet>
  <servlet-mapping>
    <servlet-name>dispatcherServlet</servlet-name>
    <url-pattern>/</url-pattern>
  </servlet-mapping>
</web-app>
```

3.配置 springmvc.xml 

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:context="http://www.springframework.org/schema/context"
       xmlns:mvc="http://www.springframework.org/schema/mvc"
       xmlns:component="http://www.springframework.org/schema/context"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
http://www.springframework.org/schema/beans/spring-beans.xsd
http://www.springframework.org/schema/context
http://www.springframework.org/schema/context/spring-context.xsd
http://www.springframework.org/schema/mvc
http://www.springframework.org/schema/mvc/spring-mvc-3.2.xsd">

    <!--自动扫描-->
    <context:component-scan base-package="com.dzx"/>

    <!--配置视图解析器-->
    <bean class="org.springframework.web.servlet.view.InternalResourceViewResolver">
        <property name="prefix" value="/"/>
        <property name="suffix" value=".jsp"/>
    </bean>
</beans>
```

4.创建 Handler

```java
package com.dzx.Handler;

import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.RequestMapping;

@Controller
@RequestMapping("/test")
public class HelloHandler {

    @RequestMapping("/hello")
    public String sayHello(){
        return "test";
    }
}
```

5.根据 Handler 返回的结果创建对应的物理视图（例如 JSP文件）



## Spring MVC 注解

核心注解：

- **@RequestMapping** 

将 URL 请求与业务方法进行映射，在 Handler **类**或**方法**上进行标注，其中在方法定义处添加相当于加一层路径

常用属性：

| 属性名 | 描述                                           |
| ------ | ---------------------------------------------- |
| value  | 指定 URL 请求的实际地址，是默认属性            |
| method | 指定请求方式的类型（GET、POST、PUT、DELET等）  |
| params | 指定请求中必须包含某些参数，否则不可调用该方法 |

```java
@RequestMapping(value = "/hello",method = RequestMethod.GET,params = {"name","id=10"})
public String sayHello(){
    return "test";
}
```

上述请求中必须包含 name 和 id 两个参数，且 id 的值必须等于10 才能正确访问

组合注解：可以省略 method，简化代码

@GetMapping：匹配 GET 方式的请求

@PostMapping：匹配 POST 方式的请求

@PutMapping：匹配 PUT 方式的请求

@DeleteMapping：匹配 DELETE 方式的请求



- **@Controller**

在类定义处添加，将该类交给 IoC 容器来管理，同时使其成为一个**控制器**，可以同时处理多个请求

为保证 Spring 能找到控制器类，需要在 springmvc.xml 中添加扫描器

```xml
<!--自动扫描-->
<context:component-scan base-package="com.dzx"/>
```



## 参数类型和返回类型

请求处理方法的参数类型：

HttpServletRequest、HttpServletResponse、HttpSession等，以及@pathVariable、@RequestParam、@RequestBody等注解

请求处理方法的返回类型：ModelAndView、Model、Map、View、String、void等

常见的返回类型有 ModelAndView、String、void，其中 ModelAndView 类型中可以添加 Model 数据，并指定视图；String 类型的返回值可以跳转视图，但不能携带数据；而 void 类型主要在异步请求时使用，只返回数据，不会跳转视图

redirect 重定向

```java
return "redirect:/test.jsp";   
```

forward 请求转发

```java
return "forward:/test.jsp";    //return "test";
```

Spring MVC **默认是以请求转发**的方式响应 JSP 



## 乱码问题

如果出现中⽂乱码问题，只需在 web.xml 添加 Spring MVC ⾃带的过滤器即可

```xml
<filter>
  <filter-name>encodingFilter</filter-name>
  <filter-class>org.springframework.web.filter.CharacterEncodingFilter</filter-class>
<init-param>
  <param-name>encoding</param-name>
  <param-value>UTF-8</param-value>
</init-param>
</filter>
<filter-mapping>
  <filter-name>encodingFilter</filter-name>
  <url-pattern>/*</url-pattern>
</filter-mapping>
```

处理 @ResponseBody 中⽂乱码，在 springmvc.xml 中配置消息转换器

```xml
<mvc:annotation-driven>
 	<!-- 消息转换器 -->
 	<mvc:message-converters register-defaults="true">
 	<bean class="org.springframework.http.converter.StringHttpMessageConverter">
 		<property name="supportedMediaTypes" value="text/html;charset=UTF-8"></property>
 	</bean>
	</mvc:message-converters>
</mvc:annotation-driven>
```





## 数据绑定

Spring MVC 会根据客户端请求参数的不同，将请求消息中的信息以一定的方式转换并绑定到控制器类的方法参数中

- **简单数据类型(基本数据类型+包装类)** 

```java
@RequestMapping("/base")
@ResponseBody
public String test(@RequestParam(value = "user_id",required = true,defaultValue ="10") Integer id){
	return id+"";
}
```

包装类可以接收 null，基本数据类型不能接收 null

@RequestBody：Spring MVC 会直接将业务方法的返回值响应给客户端，如果不加该注解，Spring MVC 会将业务方法返回值传递给 DispatcherServlet，再由 DispatcherServlet 调用 ViewResolver 对返回值进行解析，映射到一个 JSP 资源

@RequestParam：对请求中的参数进行定义

value：指形参的别名，将 HTTP 请求中名为 user_id 的值传给参数 id，如果只使用 value，可以省略不写属性名

required：指定参数是否必须拥有，默认为 ture

defaultValue：默认值，表示请求中没有指定参数时的默认值

- **Java Bean（POJO）类型**

```java
@AllArgsConstructor
@Data
@NoArgsConstructor
public class User {
    private long id;
    private String name;
    private int age;
}
@Data
public class Address {
    private String name;
}

@RequestMapping("/pojo")
public String pojo(User user){
    System.out.println(user);
    return "test";
}
```

```jsp
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<html>
<body>
        <form action="/test/pojo" method="post">
            id：     <input type="text" name="id"/><br>
            name：   <input type="text" name="name"/><br>
            age：    <input type="text" name="age"/><br>
            Address：<input type="text" name="address.name"/><br>
            <input type="submit" value="提交"/>
        </form>
</body>
</html>
```

form 表单内各元素的 name 属性值必须要与绑定的 POJO 类中的属性名一致，这样才能自动将数据绑定，否则后台接收的参数值为 null

**注意：** 如果类的属性是基本数据类型，直接使用对应的属性名，例如 id、name、age，如果类的属性是 POJO 类，则使用【对象.属性】的方式，其中【对象】要于类的属性名一致，例如 address.name 

- **数组**

请求：http://localhost:8080/test/str?name=1&name=2&name=3（参数名要与数组名一致）

```java
@RequestMapping("/str")
@ResponseBody
public String str(String name[]){
    String s = Arrays.toString(name);
    return s;
}
```

@RestController：将业务方法的返回值直接响应给客户端

@Controller：将每一个业务方法的返回值都交给视图解析器进行解析

**@RestController = @Controller + @RequestBody**  （不进行视图解析，直接将返回值响应给客户端）

- **List**

Spring MVC 不支持 List 类型的直接转换，需要对 List 集合进行包装

```java
@Data
public class UserList {
    private List<User> users;
}

@RequestMapping("/list")
@ResponseBody
public String list(UserList userList){
      StringBuffer stringBuffer = new StringBuffer();
    for (User u :userList.getUsers()
    ) {
            stringBuffer.append(u);
    }
    return stringBuffer.toString();
}
```

```jsp
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<html>
<body>
        <form action="/test/list" method="post">
            1id：     <input type="text" name="users[0].id"/><br>
            1name：   <input type="text" name="users[0].name"/><br>
            2id：     <input type="text" name="users[1].id"/><br>
            2name：   <input type="text" name="users[1].name"/><br>
            3id：     <input type="text" name="users[2].id"/><br>
            3name：   <input type="text" name="users[2].name"/><br>
            <input type="submit" value="提交"/>
        </form>
</body>
</html>
```

- **Map**

Spring MVC 不支持 Map 类型的直接转换，需要对 Map 集合进行包装

```java
@Data
public class UserMap {
    private Map<String,User> userMap;
}

 @RequestMapping("/map")
    @ResponseBody
    public String map(UserMap userMap){
        StringBuffer stringBuffer = new StringBuffer();
        for (String key:userMap.getUserMap().keySet()){
            User user = userMap.getUserMap().get(key);
            stringBuffer.append(user);
        }
        return stringBuffer.toString();
    }
```

```jsp
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<html>
<body>
        <form action="/test/map" method="post">
            1id：     <input type="text" name="userMap['a'].id"/><br>
            1name：   <input type="text" name="userMap['a'].name"/><br>
            2id：     <input type="text" name="userMap['b'].id"/><br>
            2name：   <input type="text" name="userMap['b'].name"/><br>
            3id：     <input type="text" name="userMap['c'].id"/><br>
            3name：   <input type="text" name="userMap['c'].name"/><br>
            <input type="submit" value="提交"/>
        </form>
</body>
</html>
```

- **JSON**

客户端发送 JSON 格式的数据时，直接通过 Spring MVC 绑定到业务方法的形参中

解决 Spring MVC 无法加载静态资源的办法有三种：（在 Web.xml 中进行配置）

1.使用 < mvc:default-servlet-handeler /> 标签，一般 Web 服务器默认的 Servlet 名称为 "default"

2.激活 Tomcat 默认的 Servlet 来处理静态文件

```xml
<servlet-mapping>
 	<servlet-name>default</servlet-name>
 	<url-pattern>*.js</url-pattern>
</servlet-mapping>
<servlet-mapping>
 	<servlet-name>default</servlet-name>
 	<url-pattern>*.css</url-pattern>
</servlet-mapping>
```

3.使用 < mvc:resorces />标签

```xml
<mvc:resources location="/js/" mapping="/js/**" />
```

location：用于定位需要访问的本地静态资源文件路径，具体到某个文件夹

mapping：匹配静态资源全路径，其中 "/**" 表示文件夹及其子文件下的某个具体文件 



步骤：

- Spring MVC 中的 JSON 和 JavaBean 的转换需要借助于 fastjson，pom.xml 引入相关依赖

```xml
 <dependency>
      <groupId>com.alibaba</groupId>
      <artifactId>fastjson</artifactId>
      <version>1.2.32</version>
    </dependency>
<dependency>
  <groupId>com.fasterxml.jackson.core</groupId>
  <artifactId>jackson-annotations</artifactId>
  <version>2.12.3</version>
</dependency>
<dependency>
  <groupId>com.fasterxml.jackson.core</groupId>
  <artifactId>jackson-core</artifactId>
  <version>2.12.3</version>
</dependency>
<dependency>
  <groupId>com.fasterxml.jackson.core</groupId>
  <artifactId>jackson-databind</artifactId>
  <version>2.12.3</version>
</dependency>
```

springmvc.xml 添加 fastjson 配置。

```xml
<mvc:annotation-driven>
    <!-- 消息转换器 -->
    <mvc:message-converters register-defaults="true">
        <bean class="org.springframework.http.converter.StringHttpMessageConverter">
            <property name="supportedMediaTypes" value="text/html;charset=UTF-8"></property>
        </bean>
        <!-- 配置fastjson -->
        <bean class="com.alibaba.fastjson.support.spring.FastJsonHttpMessageConverter4"></bean>
    </mvc:message-converters>
</mvc:annotation-driven>
```



- 添加 js 文件

![1618835490873](C:\Users\86185\AppData\Roaming\Typora\typora-user-images\1618835490873.png)

- 相关代码

```jsp
<%--
 Created by IntelliJ IDEA.
 User: southwind
 Date: 2019-03-14
 Time: 10:35
 To change this template use File | Settings | File Templates.
--%>
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<html>
<head>
    <title>Title</title>
    <script type="text/javascript" src="js/jquery-3.3.1.min.js"></script>
    <script type="text/javascript">
        $(function(){
            var user = {
                "id":1,
                "name":"张三"
            };
            $.ajax({
                url:"/data/json",
                data:JSON.stringify(user),
                type:"POST",
                contentType:"application/json;charset=UTF-8",
                dataType:"JSON",
                success:function(data){
                    alert(data.id+"---"+data.name);
                }
            })
        });
    </script>
</head> <body>
</body>
</html>
```

```java
@RequestMapping(value = "/data")
......
    
@RequestMapping("/json")
@ResponseBody
public User json(@RequestBody User user){
    System.out.println(user);
    return user;
}
```



## Sping MVC 模型数据解析

JSP 四大作用域对应的内置对象：pageContext、request、session、application。

模型数据的绑定是由 ViewResolver 来完成的，实际开发中，我们需要**先添加模型数据**，**再交给 ViewResolver 来绑定**。 

Spring MVC 提供了以下几种方式添加模型数据：

- Map
- Model
- ModelAndView
- @ModelAttribute
- @SessionAttribute



> **将模型数据绑定到 request 对象**

1.Map

将数据**存储到 Map** 中，Map 将数据绑定到 **request** 对象，然后**直接在 JSP 页面访问 request 对象的数据池**（通过 **key**值） 

```java
@RequestMapping("/MAP")
public String MAP(Map<String,User> map){
    User user = new User();
    user.setId(1);
    user.setName("dzx");
    map.put("dzx",user);
    return "view";
}
```

```jsp
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<%@ page isELIgnored="false" %>
<html>
<head>
    <title>Title</title>
</head>
<body>
    ${requestScope.dzx}
</body>
</html>
```



2.Model

将数据**存储到 Model** 中，Model 将数据绑定到 **request** 对象，然后**直接在 JSP 页面访问 request 对象的数据池** 

```java
@RequestMapping("/model")
public String model(Model model){
    User user = new User();
    user.setId(2);
    user.setName("zw");
    model.addAttribute("people",user);			//key-value	
 	//model.addAttribute(user);				   //object
    return "view";
}
```

```jsp
<%--
  Created by IntelliJ IDEA.
  User: 86185
  Date: 2021/4/19
  Time: 21:08
  To change this template use File | Settings | File Templates.
--%>
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<%@ page isELIgnored="false" %>
<html>
<head>
    <title>Title</title>
</head>
<body>
  
    ${requestScope.people}   //如果 model 使用了 key-value 形式存储,就是用 request.key 去访问
    -------------------
    ${requestScope.user}	//如果 model 使用 object 形式存储，就直接访问 request.object 去访问，或者							 说 key 与 value 相等（名字）
</body>
</html>
```



3.ModelAndView

核心思想：

将数据**存储到 ModelAndView** 中，ModelAndViewl 将数据绑定到 **request** 对象，然后**直接在 JSP 页面访问 request 对象的数据池** 

ModelAndView 类包含一个 view 成员变量

**其中我们需要**：

1.创建 ModelAndView 对象

2.**对 view 进行赋值**

3.将数据添加到 ModelAndView 对象中

由于 ModelAndView 有多个不同的构造函数且对 view 的赋值也有多个方法，因此造成有许多不同的方式来解析ModelAndView

```java
@RequestMapping("/modelAndView")
public ModelAndView modelAndView(){
    User user = new User();
    user.setId(1L);
    user.setName("张三");
    ModelAndView modelAndView = new ModelAndView();
    modelAndView.addObject("user",user);
    modelAndView.setViewName("view");
    return modelAndView;
}

@RequestMapping("/modelAndView2")
public ModelAndView modelAndView2(){
    User user = new User();
    user.setId(1L);
    user.setName("张三");
    ModelAndView modelAndView = new ModelAndView();
    modelAndView.addObject("user",user);
    View view = new InternalResourceView("/view.jsp");
    modelAndView.setView(view);
    return modelAndView;
}

@RequestMapping("/modelAndView3")
public ModelAndView modelAndView3(){
    User user = new User();
    user.setId(1L);
    user.setName("张三");
    ModelAndView modelAndView = new ModelAndView("view");
    modelAndView.addObject("user",user);
    return modelAndView;
}

@RequestMapping("/modelAndView4")
public ModelAndView modelAndView4(){
    User user = new User();
    user.setId(1L);
    user.setName("张三");
    View view = new InternalResourceView("/view.jsp");
    ModelAndView modelAndView = new ModelAndView(view);
    modelAndView.addObject("user",user);
    return modelAndView;
}

@RequestMapping("/modelAndView5")
public ModelAndView modelAndView5(){
    User user = new User();
    user.setId(1L);
    user.setName("张三");
    Map<String,User> map = new HashMap<>();
    map.put("user",user);
    ModelAndView modelAndView = new ModelAndView("view",map);
    return modelAndView;
}

@RequestMapping("/modelAndView6")
public ModelAndView modelAndView6(){
    User user = new User();
    user.setId(1L);
    user.setName("张三");
    Map<String,User> map = new HashMap<>();
    map.put("user",user);
    View view = new InternalResourceView("/view.jsp");
    ModelAndView modelAndView = new ModelAndView(view,map);
    return modelAndView;
}

@RequestMapping("/modelAndView7")
public ModelAndView modelAndView7(){
    User user = new User();
    user.setId(1L);
    user.setName("张三");
    ModelAndView modelAndView = new ModelAndView("view","user",user);
    return modelAndView;
}

@RequestMapping("/modelAndView8")
public ModelAndView modelAndView8(){
    User user = new User();
    user.setId(1L);
    user.setName("张三");
    View view = new InternalResourceView("/view.jsp");
    ModelAndView modelAndView = new ModelAndView(view,"user",user);
    return modelAndView;
}
```



4.HttpServletRequest

使用 HttpServletRequest request ，通过 request.setAttribute() 将数据存储到 request 数据池中

```java
@RequestMapping("/request")
public String request(HttpServletRequest request){
    User user = new User();
    user.setId(1L);
    user.setName("张三");
    request.setAttribute("user",user);
    return "view";
}
```



5.@ModelAttribute

被@ModelAttribute注释的方法会在此 controller 每个方法执行前被执行

定义一个方法，加上注解，表示该方法专门用来**返回要存储到 request 对象数据池中的对象**

```java
//3中不同的定义方式

//直接返回对象
@ModelAttribute
public User getUser(){
    User user = new User();
    user.setId(1L);
    user.setName("张三");
    return user;
}

//使用 Map
@ModelAttribute
public void getUser(Map<String,User> map){
    User user = new User();
    user.setId(1L);
    user.setName("张三");
    map.put("user",user);
}

//使用 Model
@ModelAttribute
public void getUser(Model model){
    User user = new User();
    user.setId(1L);
    user.setName("张三");
    model.addAttribute("user",user);
}
```

**业务方法中无需再处理模型数据**

```java
@RequestMapping("/modelAttribute")
public String modelAttribute(){
    return "view";
}
```



总结

无论是哪种模型，都会**自动将数据存储到 request 对象的数据池中**，因此我们只需要**将数据存储到模型中**即可



> **将模型数据绑定到 session 对象**

1.直接使用原生的 Servlet API

2.@SessionAttribute

方式一：

```java
@SessionAttributes(value = {"user","address"})
public class ViewHandler {
}
```

对于 ViewHandler 中的所有业务方法，只要向 request 中添加了 key = "user"、key = "address" 的对象时，Spring MVC 会自动将该数据添加到 session 中，保存 key 不变

方式二：

```java
@SessionAttributes(types = {User.class,Address.class})
public class ViewHandler {
}
```

对于 ViewHandler 中的所有业务方法，只要向 request 中添加了数据类型是 User 、Address 的对象时，Spring MVC 会自动将该数据添加到 session 中，保存 key 不变



> **将模型数据绑定到 application 对象**

直接使用 application 对象

```java
@RequestMapping("/application")
public String application(HttpServletRequest request){
    ServletContext application = request.getServletContext();
    User user = new User();
    user.setId(1L);
    user.setName("张三");
    application.setAttribute("user",user);
    return "view";
}
```



## Spring MVC 自定义数据转换器

数据转换器是指将客户端 HTTP 请求中的参数转换为业务方法中定义的形参，自定义表示开发者可以自主设计转换的方式，HandlerApdter 已经提供了通用的转换，String 转 int，String 转 double，表单数据的封装等，但是在特殊的业务场景下，HandlerAdapter 无法进行转换，就需要开发者自定义转换器

客户端输入 String 类型的数据 "2019-03-03"，自定义转换器将该数据转为 Date 类型的对象

- 实现 **Conveter** 接口

```java
package com.dzx.Handler;

import org.springframework.core.convert.converter.Converter;
import java.text.ParseException;
import java.text.SimpleDateFormat;
import java.util.Date;


/**
 * 自定义日期转化器
 * 将用户输入的 String 类型转化为 Date 型
 * 2019-03-03  ->  Date
 */
public class DateConverter implements Converter<String, java.util.Date>{

    @Override
    public Date convert(String s) {
        //转换格式
        SimpleDateFormat simpleDateFormat = new SimpleDateFormat("yyyy-MM-dd");
        Date date = null;
        System.out.println("1111111111111111111111111111111");
        try {
            date = simpleDateFormat.parse(s);
        } catch (ParseException e) {
            e.printStackTrace();
        }
        return date;
    }
}
```

- Springmvc.xml 配置

```xml
<!--打开自定义转换器服务-->
<!--一个XML 只需要一个<mvc:annotation-driven-->
<mvc:annotation-driven conversion-service="conversionService">	
<!--配置自定义转换器-->
<bean id="conversionService" class="org.springframework.context.support.ConversionServiceFactoryBean">
    <property name="converters">
        <list>
            <bean class="com.dzx.Handler.DateConverter">
            </bean>
        </list>
    </property>
</bean>
```

- 控制器

```java
//自定义数据转换器
@RequestMapping("/date")
@ResponseBody
public String dateConverter(Date date){
    System.out.println(date);
    return date.toString();
}
```

传入参数 2012-01-01 会自动转换成 Date 型



## REST

Representational State Transfer，资源表现层状态转换，是目前比较主流的一种互联网软件架构，它结构清晰、标准规范、易于理解、便于扩展

```java
//REST风格
@RequestMapping("/rest/{id}")
@ResponseBody
public Integer rest(@PathVariable("id") int user_id){
    return user_id;
}
```

将变量用路径的方式传递，必须配合 **@PathVariable** 使用





## 拦截器（过滤器）

实现 HandlerInterceptor 接口​	

```java
public class Interceptor1 implements HandlerInterceptor {

    //在控制器方法前执行
    public boolean preHandle(HttpServletRequest request, HttpServletResponse response, Object handler) throws Exception {
        System.out.println("执行方法前1...");
        return true;
    }

    //在控制器方法调用之后，解析视图之前执行
    public void postHandle(HttpServletRequest request, HttpServletResponse response, Object handler, ModelAndView modelAndView) throws Exception {
        System.out.println("执行方法后1...");
    }

    //在整个请求完成后执行，及视图渲染结束后执行
    public void afterCompletion(HttpServletRequest request, HttpServletResponse response, Object handler, Exception ex) throws Exception {
        System.out.println("渲染结束后1...");
    }
}
```

拦截器的配置:

```xml
<!--配置拦截器-->
<mvc:interceptors>
    <!--直接设置在<mvc:interceptors>下的拦截器，默认拦截所有请求-->
    <bean class="com.dzx.Handler.Interceptor1"></bean>
    <!--拦截器配置-->
    <mvc:interceptor>
        <!--拦截路径-->
        <mvc:mapping path="/**"/>	拦截所有请求
        <!--不拦截的路径-->
        <mvc:exclude-mapping path="/date"/>	  拦截以 date 结尾的请求
        <!--设置在<mvc:interceptor>下的拦截器，拦截/放行指定路径-->
        
    </mvc:interceptor>
</mvc:interceptors>
```

< mvc:interceptor >中  必须按照 <mvc:mapping > 、<mvc:exclude-mapping > 、< bean >的顺序来配置，否则报错



**多个拦截器的拦截过程**

![1619691965939](C:\Users\86185\AppData\Roaming\Typora\typora-user-images\1619691965939.png)

有点类似于栈，拦截器的顺序于配置的顺序有关（pre 的时候按**正序**，（post、after）其余的时候按**倒序**）





## 文件上传与下载

- **文件上传**

方式一：使用原生的IO流

方式二：使用 MVC 包装好的东西

```java
 //文件上传
    @PostMapping("/upload")
    public String upload(MultipartFile file, HttpServletRequest request){
        if(file.getSize()>0){
            //方式一：使用封装好的IO流
            //获取上传文件名
            String filename = file.getOriginalFilename();
            //获取上传文件的路径+参数
            String path =request.getServletContext().getRealPath("./file/");

            try {
                File desfile = new File(path,filename);
                //进行上传
                file.transferTo(desfile);
                request.setAttribute("path",path);
            } catch (IOException e) {
                e.printStackTrace();
            }
```

配置文件

```xml
<!-- 配置上传组件 -->
<bean id="multipartResolver" class="org.springframework.web.multipart.commons.CommonsMultipartResolver"></bean>
```



- **文件下载**

```java
//文件下载
@RequestMapping("/download/{name}")
public ResponseEntity<byte[]> download(@PathVariable("name") String filename,HttpServletRequest request)throws IOException{
    filename+=".png";
    //指定资源的路径
    String path = request.getServletContext().getRealPath("./file/");
    //创建文件对象
    File file = new File(path,filename);
    //设置响应头
    HttpHeaders httpHeaders = new HttpHeaders();
    //通知浏览器以下载的方式打开文件
    httpHeaders.setContentDispositionFormData("attachment",filename);
    //定义以流的形式下载返回文件数据
    httpHeaders.setContentType(MediaType.APPLICATION_OCTET_STREAM);
    //使用 Spring MVC 框架的 ResponseEntity 对象封装返回下载数据
    System.out.println(path+filename);
    return new ResponseEntity<byte[]>(FileUtils.readFileToByteArray(file),httpHeaders, HttpStatus.OK);
}
```











​																							