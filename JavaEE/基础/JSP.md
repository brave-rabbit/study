# JSP

JSP本质上是一个Servlet，JSP主要负责于用户进行交互，将最终的**界面**呈现给用户，是HTML+CSS+JS+JAVA的混合文件。

当服务器接收到 .JSP 的请求时，将该请求交给JSP引擎处理，**最终被翻译成Servlet（java）文件**，再由Web容器调用Servlet完成响应。

单纯从开发的角度来看，就是**在HTML中嵌入JAVA程序**。

具体嵌入的方式有三种：

1. JSP脚本：执行JAVA逻辑代码

   ```JSP
   <%
       JAVA语句
   %>
   ```

2. JSP声明：定义JAVA方法

   ```JSP
   <%!
      定义方法
   %>
   ```

3. JSP表达式：把JAVA对象直接输出到HTML页面中

   ```JSP
   <%=JAVA变量%>
   ```



## JSP九个内置对象

1. request：表示一次请求，**HttpServletRequest**定义
2. response：表示一次响应，**HttpServletResponse**定义
3. pageContext：页面上下文，获取页面信息，**PageContext**定义
4. session：表示一次会话，保存用户信息，**HttpSession**定义
5. application：表示当前Web应用，全局对象，保存用户共享信息，**ServletContext**定义
6. config：当前JSP对应的Servlet的ServletConfig对象，获取Servlet信息，**ServletConfig**定义
7. out：向浏览器输出数据，**JspWrite**定义
8. page：当前JSP对应的Servlet对象，**Servlet**定义
9. exception：表示JSP页面发生异常，**Exception**定义



### 1.request

服务请求


1. String getParameter(String key)：获取客户端传来的参数值**[客户端----->服务器】**
2. void setAttribute（String key，Object value)：通过键值对的形式保存数据
3. Object getAttribute（String key）：通过key取出value**【服务器内部】**
4. RequestDispatcher getRequestDispatcher(String path) 返回⼀个 RequestDispatcher 对象，该对象的 **forward** ⽅法⽤于**请求转发**
5. String[ ] getParameterValues(String key)：返回客户端传来的多个**同名**参数值
6. void setCharacterEncoding(String charset) //设置请求的编码



### 2.response

服务响应

1.void sendRedirect(String location)：重定向，实现页面之间的跳转

2.resp.setContentType("text/html;charset=UTF-8") ：设置响应编码



**请求转发 getRequestDispatcher 和重定向 sendRedirect 的区别：**

(转发是将同一个请求传给下一个页面，重定向是创建一个**新的请求**传给下一个页面，之前的请求结束生命周期)

- 转发：同一个请求在服务器之间传递，地址栏不变，也叫服务器跳转


- 重定向：由客户端发送一个新的请求来访问跳转后的目标资源，地址栏改变，也叫客户端跳转


【如果两个页面之间传值，必须使用转发，不能使用重定向】




### 3.用户会话（ssesion、cookie）

服务器无法识别每一次HTTP请求的出处（不知道来自哪个终端），所以需要一种技术来让服务器知道请求来自哪，这就是会话技术。

**会话**：客户端与服务端之间发生的一系列连续的请求和响应的过程，即打开浏览器到关闭浏览器的过程。

会话状态：指客户端和服务器在会话过程中产生的信息状态，借助于会话状态，服务器能把属于同一会话的一系列请求和响应关联起来。

实现会话的两种方式：

- **ssesion**
- **cookie**



#### session

同一个会话的sessionID是相同的

常用方法;

1. String getId() ：获取 sessionID

------

 session被销毁的两种方式：

- 关闭会话

-   **超时**（在一定的时效内未与服务器进行交互）

   【刷新一次页面（算一次交互）会重置失效时间（从0开始计时）】

------

2. void setMaxInactiveInterval(int interval)： 设置 session 的失效时间，单位为秒 （0表示无限期）
3. int getMaxInactiveInterval() ：获取当前 session 的失效时间 
4. void invalidate() ：设置 session ⽴即失效
5. void setAttribute(String key,Object value) ：通过键值对的形式来存储数据（重复的key进行覆盖）
6. Object getAttribute(String key) ：通过键获取对应的数据
7. void removeAttribute(String key)：通过键值删除对应数据



#### cookie

Cookie 是服务端在 HTTP 响应中附带传给浏览器的⼀个⼩⽂本⽂件，⼀旦浏览器保存了某个 Cookie， 

在之后的请求和响应过程中，会**将此 Cookie 来回传递**，这样就可以通过 Cookie 这个载体完成客户端 

和服务端的数据交互

- 创建cookie

```java
Cookie cookie=new Cookie(username,password);
res.addCookie(cookie);
```

**对cookie进行操作后，必须调用addCookie**

- 读取cookie

```
<%
    Cookie[] cookies=request.getCookies();
    for (Cookie cookie:cookies) {
        对cookie进行操作
    }
%>
```

常用方法：

1. void setMaxAge(int age)： 设置 Cookie 的有效时间，单位为秒

2. int getMaxAge() ：获取 Cookie 的有效时间（默认为-1：表示关闭浏览器就销毁）

3. String getName() ：获取 Cookie 的 name 

4. String getValue() ：获取 Cookie 的 value


**session与cookie的区别：**

session：保存在服务器

​		保存的数据是 Object

​		随着会话结束而销毁

​		保存重要的信息

​		网页刷新一次失效时间重置

​		生命周期 ( 服务端：只要Web应用关闭就销毁；客户端：只要浏览器关闭就销毁)	



cookie：  保存在客户端（浏览器）

​	        保存的数据是 String

​	      	可以长期保存，与会话无关

​	      	保存不重要信息

​	      	网页刷新一次有效时间不重置

​		生命周期（服务端：无关；客户端：默认是只要关闭浏览器就销毁，我们通过 setMaxAge⽅法设置有效期，⼀旦设置了有效期，则不随浏览器的关闭⽽销毁，⽽是由设置的时间来决定）



## 内置对象作用域

page作用域：内置对象是pageContext						只在当前页面有效

request作用域：内置对象是request							在同一个请求有效

session作用域：内置对象是session							在同一个会话有效

application作用域：内置对象是application					对整个WEB应用有效（服务器关闭则失效）

查找优先级page>request>session>application		         	作用范围：page<request<session<application				



## EL表达式（只存在于非JAVA区）

Expression Language 表达式语⾔，替代 JSP ⻚⾯中数据访问时的复杂编码，可以⾮常便捷地取出**域对** 

**象（pageContext、request、session、application）**中保存的数据，前提是⼀定要先 setAttribute， 

EL 就相当于在简化 getAttribute



方式：

- **${ 变量名 }**				

1.变量名=参数名

2.变量名= setAttribute 对应的 key值（**如果是对象，对象的属性必须设置get方法，且则以变量名引用该对象）**



本质：（1）将Key值首字母大写	  	

​	   （2）设置 get...... 方法

eg:

```jsp
${name}												${user.id} or ${user["id"]}
(1)将name变成Name
(2)设置getName方法并调用得到name
```



查找：

1.默认查找顺序

pageContext>request>session>application	（查到就直接返回,没有返回null）

2.按指定作用域进行查找

查找pageContext：${pageScope.变量名}

查找request：${requestScope.变量名}

查找session：${sessionScope.变量名}

查找application：${applicationScope.变量名}



逻辑运算: 

&&、||、！、>、<、>=、<=、==、empty（长度为0也算空）

eg：${X运算符Y}



## JSTL（标签库） 

JSP Standard Tag Library JSP 标准标签库，JSP 为开发者提供的⼀系列的标签，使⽤这些标签可以完成 

⼀些逻辑处理，⽐如循环遍历集合，让代码更加简洁，不再出现 JSP 脚本穿插的情况。 

实际开发中 EL 和 JSTL 结合起来使⽤，JSTL 侧重于逻辑处理，EL 负责展示数据。



使用步骤

1.创建lib文件夹，将jar包（jstl.jar、standard.jar）复制进去-----------------------存放位置：web/WEB-INF目录下

2.在 Libraries 中添加上述2个包

3.在JSP首页开始的地方导入JSTL标签库	

4.在需要的地方使用



优点：

1.提供了统一的标签；

2.可以用于编写各种动态功能



- **核心标签库常用标签**

```JSP
<%@ taglib prefix="c" uri="http://java.sun.com/jsp/jstl/core" %>
```

​	

1.set：向域对象中添加数据(变量直接添加，对象则需要先创建对象在操作【类似于修改】)

```jsp
<c:set var="name" value="dzx" scope="request"></c:set>
<%											
      User user = new User(1,"dzx");
      request.setAttribute("admin",user);
%>
<c:set target="${admin}" property="name" value="zw"></c:set>  //target:返回key对应的对象并以key引用 
														
${name}<br>
${admin.name}<br>
${admin.id}
```

**scope默认是 page**

------

var:	  

如果“value”为null，“var”指定的属性将被删除！

如果“var”指定的属性不存在，则会创建一个属性，但仅当“value”不为null时才会创建新属性。  



target:

如果“target”是一个Map，“property”指定的是该Map的一个键；如果“target”是一个对象，“property”指定的是该对象的一个成员字段。

如果“target”表达式为null，容器会抛出一个异常。

如果“target”表达式不是一个Map或对象，容器会抛出一个异常。

如果“target”表达式是一个对象，但是这个对象没有与“property”匹配的成员字段，容器会抛出一个异常。  

------

2.out：输出域对象中的数据

```jsp
<c:set var="name" value="dzx" scope="request"></c:set>
        <%
            User user = new User(1,"dzx");
            request.setAttribute("admin",user);
        %>
<c:set target="${admin}" property="name" value="zw"></c:set>

<c:out value="${name}" default="未定义1"></c:out><br>
<c:out value="${admin.name}" default="未定义2"></c:out><br>
<c:out value="${user}" default="未定义3"></c:out>
```

如果未找到数据，则返回 default



3.remove：删除域对象中的数据

```jsp
<c:remove var="name"></c:remove>
<c:remove var="admin"></c:remove>
```

不管是对象还变量，var 对应的都是 key，不需要用EL

默认（scope=null）删除全部域对象数据



4.catch：捕获异常

```jsp
<c:catch var="error">
<%
   int a=10/0;
%>
</c:catch>
${error}
```



5.条件标签：if / choose

```jsp
<c:set var="x" value="1"></c:set>
<c:set var="y" value="2"></c:set>

<c:if test="${x>y}">false</c:if>

<c:choose>
    <c:when test="${x<y}">ok</c:when>
    <c:otherwise>no</c:otherwise>
</c:choose>
```

test : 逻辑运算表达式（EL）

if：没有else

choose：有else



6.迭代标签：forEach（循环）

```JSP
<%
    List<String> list=new ArrayList<>();
    list.add("a");
    list.add("b");
    list.add("c");
    list.add("d");
    list.add("e");
    request.setAttribute("list",list);
%>

//将str存进域对象的数据池中，可以用EL表达式来读取数据
<c:forEach items="${list}" var="str" begin="0" varStatus="sta" step="1">
    ${sta.count}-${str}<br>
</c:forEach>
```

varStatus:跟序号有关		eg:count----从1开始，index----从0开始等等

items：接收数据

var：临时展示数据（结果）

begin/end：开始/结束

step：跨度



- **格式化标签库常用标签**

```jsp
<%@ taglib prefix="fmt" uri="http://java.sun.com/jsp/jstl/fmt" %>
```

```JSP
<%
    request.setAttribute("time",new Date());
%>

//格式化日期
<fmt:formatDate value="${time}" pattern="yyyy-MM-dd HH:mm:ss">
</fmt:formatDate>											

//设置精度
<fmt:formatNumber value="1234.5678" maxIntegerDigits="2" maxFractionDigits="1">
</fmt:formatNumber>				  
（从小数点往两边走）
type：按照什么类型格式化
pattern：自定义格式化样式
currencyCode:ISO-4721货币代码，只适用于按照货币格式化的数字
currencySymbol： 货币符号,如￥,只适用于按照货币格式化的数字
groupingUsed： 是否包含分隔符
maxIntegerDigits： 整数部分最多显示多少位
mixIntegerDigits： 整数部分最少显示多少位
maxFractionDigits： 小数部分最多显示多位位
minFractionDigits： 小数部分最少显示多位位
```



- **函数标签库常用标签**

```jsp
<%@ taglib prefix="fun" uri="http://java.sun.com/jsp/jstl/functions" %>
```

```jsp
<%
    request.setAttribute("info","abcde");
%>
${fun:contains(info,"g")}<br>							 //是否包含
${fun:startsWith(info, "a")}<br>					 	 //是否是第一个开始
${fun:endsWith(info, e)}<br>					    	 //是否是最后一个
${fun:indexOf(info, "b")}<br>							 //查找（返回下标）
${fun:replace(info, "c","i")}<br>						 //替换（返回整个字符串）
${fun:substring(info,0 ,2 )}<br>						 //截取（半闭半开）
${fun:split(info, "b")[0]}-${fun:split(info, "b")[1]}	   //拆分（返回数组）
```







