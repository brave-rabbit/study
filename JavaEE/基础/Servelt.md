# Servelt：是一组接口，负责与客户端通信

功能：

（1）创建并返回基于客户请求的动态HTML页面；

（2）与数据库进行通信；



## 映射

网页不能直接访问Servlet（java）文件，需要通过映射去访问

- **基于XML文件的配置方式**

![1611117736157](C:\Users\86185\AppData\Roaming\Typora\typora-user-images\1611117736157.png)

servlet-name :命名，上下要一致

servlet-class：访问文件所在位置

url-pattern：替代名**（注意加“/”）**

- **基于注解的方式**

![1611120839221](C:\Users\86185\AppData\Roaming\Typora\typora-user-images\1611120839221.png)

在Servlet（java）文件中添加   @WebServlet("/替代名")



## 解决中文乱码问题

![1611119942000](C:\Users\86185\AppData\Roaming\Typora\typora-user-images\1611119942000.png)

setContentType:设置响应类型



![1612189801816](C:\Users\86185\AppData\Roaming\Typora\typora-user-images\1612189801816.png)

setCharacterEncoding：编码设置(**必须提前（在获取数据之前）设置好，否则会失效**)



​	

## Servlet的生命周期

1. 当浏览器访问Servlet时,Tomcat会查询当前Servlet对象是否存在.若存在，执行（3），若不存在，执行（2）.

2. 执行无参构造函数，调用 init 方法完成初始化操作

3. 调用 service 方法完成逻辑操作

4. 关闭Tomcat时，调用 destory 方法，释放对象

无参构造函数（1次）-->init（1次）-->service（N次）-->destory（1次）



## ServletConfig（用来描述Servlet的基本信息）

```
@Override
public void init(ServletConfig config) {
    System.out.println(config.getServletName());
    System.out.println(config.getInitParameter("username"));
    System.out.println(config.getInitParameter("password"));
    Enumeration<String> es=config.getInitParameterNames();
    while(es.hasMoreElements()) {
        System.out.println(es.nextElement());
    }
}
```

- getServletName()：返回Servlet的name（在设置映射的时候设置的name），没设置的话返回全类名

![1611140394951](C:\Users\86185\AppData\Roaming\Typora\typora-user-images\1611140394951.png)

![1611140663687](C:\Users\86185\AppData\Roaming\Typora\typora-user-images\1611140663687.png)

- getInitParameter(String key)：返回Key对应的value
- getInitParameterNames()：返回所有参数名（复数形式）
- getServletContext()：返回ServletContext对象     **ServletContext（整个Servlet的管理者）**



## ServletContext（用于描述Web）

```java
@Override
public void init(ServletConfig servletConfig) throws ServletException {
    ServletContext servletContext = servletConfig.getServletContext();
    System.out.println(servletContext.getContextPath());
    System.out.println(servletContext.getRealPath("/"));
    System.out.println(servletContext.getServerInfo());
    System.out.println("init...");
}
```



### ServletConfig和ServletContext的区别：

​	ServletConfig作用于某个Servlet实例，每个Servlet都有对应的ServletConfig，ServletContext作用于整个Web应用，一个Web应用对应一个ServletContext，多个Servlet实例对应一个ServletContext 

​	一个局部对象，一个全局对象



## Servlet的层次结构

Servlet->GenericServlet->HttpServlet

HTTP常见的四种请求类型:

GET				读取

POST			保存

PUT				修改

DELETE			删除

GenenricServlet实现Servlet接口，同时为它的子类屏蔽不常用的方法，只需要重写service方法

HttpServlet继承GenericServlet，在service方法中根据不同类型的请求调用不同的方法（GET用doGet、POST用doPost，等等）

自定义类的Servlet继承HttpServlet，只需要重写doGet、doPost等



