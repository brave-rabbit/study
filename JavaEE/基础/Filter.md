# Filter：过滤器

### 

### 功能

1.用来拦截请求和响应

2.处理服务器与客户端之间的数据流

### 

### 实现

通过实现 Filter （javax.servlet.* ）接口

```java
@WebFilter("/filter")
public class MyFilter implements Filter {
    @Override
    public void doFilter(ServletRequest request, ServletResponse response, FilterChain chain) throws IOException, ServletException {
       //逻辑操作
       chain.doFilter(request,response);
    }
}
```

ServletRequest： 是HttpServletRequest/Response的父类，如果要用到**转发**或者**重定向**，需要向下强转

chain: Filter 具有"中断"功能，必须使用 **doFilter(request,response)**方法继续往下走，否则请求/响应⽆法向后传递，⼀直停留在过滤器中



### 配置

一个 Filter 可对应多个URL

一个 URL 可对于多个Filter（**执行Filter的顺序与映射的顺序有关**

​						1.XML文件中 从上往下---------------------------------------------XML配置

​						2.JAVA代码文件从上往下（按类名字母顺序排列）---------注解配置）

​						eg. 先执行MyFilter ，在执行OtherFilter

![1612185472491](C:\Users\86185\AppData\Roaming\Typora\typora-user-images\1612185472491.png)

1.web.XML配置(越往上的Filter越先执行)

```xml
<filter>
    <filter-name>myfilter</filter-name>
    <filter-class>com.example.filter.MyFilter</filter-class>
</filter>
<filter-mapping>
    <filter-name>myfilter</filter-name>
    
    <url-pattern>/filter</url-pattern>							//多个映射
    <url-pattern>/fil</url-pattern>
    
</filter-mapping>
```



2.基于注解的配置

```JAVA
@WebFilter({"/fil","/filter"})									//多个映射
```



### 生命周期

Tomcat启动时---------Tomcat关闭

1.无参构造函数：只调用一次，当Tomcat启动时调用（Filter一定要进行配置）

2.init：只调用一次，当Filter实例化对象创建完成后调用（紧接着无参构造函数之后）

3.doFilter：可调用多次，当访问 Filter 时调用

4.destroy：只调用一次，Tomcat关闭时调用



### 实际应用

1.同一处理中文乱码

2.屏蔽敏感词

3.控制资源的访问权限



​	











