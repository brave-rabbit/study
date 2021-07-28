# 核心

![1622274309406](C:\Users\86185\AppData\Roaming\Typora\typora-user-images\1622274309406.png)



# Web 开发

## 1、SpringMVC自动配置概览

Spring Boot provides auto-configuration for Spring MVC that **works well with most applications.(大多场景我们都无需自定义配置)**

The auto-configuration adds the following features on top of Spring’s defaults:

- Inclusion of `ContentNegotiatingViewResolver` and `BeanNameViewResolver` beans.

- - 内容协商视图解析器和BeanName视图解析器

- Support for serving static resources, including support for WebJars (covered [later in this document](https://docs.spring.io/spring-boot/docs/current/reference/html/spring-boot-features.html#boot-features-spring-mvc-static-content))).

- - 静态资源（包括webjars）

- Automatic registration of `Converter`, `GenericConverter`, and `Formatter` beans.

- - 自动注册 `Converter，GenericConverter，Formatter `

- Support for `HttpMessageConverters` (covered [later in this document](https://docs.spring.io/spring-boot/docs/current/reference/html/spring-boot-features.html#boot-features-spring-mvc-message-converters)).

- - 支持 `HttpMessageConverters` （后来我们配合内容协商理解原理）

- Automatic registration of `MessageCodesResolver` (covered [later in this document](https://docs.spring.io/spring-boot/docs/current/reference/html/spring-boot-features.html#boot-features-spring-message-codes)).

- - 自动注册 `MessageCodesResolver` （国际化用）

- Static `index.html` support.

- - 静态index.html 页支持

- Custom `Favicon` support (covered [later in this document](https://docs.spring.io/spring-boot/docs/current/reference/html/spring-boot-features.html#boot-features-spring-mvc-favicon)).

- - 自定义 `Favicon`  

- Automatic use of a `ConfigurableWebBindingInitializer` bean (covered [later in this document](https://docs.spring.io/spring-boot/docs/current/reference/html/spring-boot-features.html#boot-features-spring-mvc-web-binding-initializer)).

- - 自动使用 `ConfigurableWebBindingInitializer` ，（DataBinder负责将请求数据绑定到JavaBean上）

> If you want to keep those Spring Boot MVC customizations and make more [MVC customizations](https://docs.spring.io/spring/docs/5.2.9.RELEASE/spring-framework-reference/web.html#mvc) (interceptors, formatters, view controllers, and other features), you can add your own `@Configuration` class of type `WebMvcConfigurer` but **without** `@EnableWebMvc`.
>
> **不用@EnableWebMvc注解。使用** **@Configuration** **+** **WebMvcConfigurer** **自定义规则**

> If you want to provide custom instances of `RequestMappingHandlerMapping`, `RequestMappingHandlerAdapter`, or `ExceptionHandlerExceptionResolver`, and still keep the Spring Boot MVC customizations, you can declare a bean of type `WebMvcRegistrations` and use it to provide custom instances of those components.
>
> **声明** **WebMvcRegistrations** **改变默认底层组件**

> If you want to take complete control of Spring MVC, you can add your own `@Configuration` annotated with `@EnableWebMvc`, or alternatively add your own `@Configuration`-annotated `DelegatingWebMvcConfiguration` as described in the Javadoc of `@EnableWebMvc`.
>
> ## **使用** **@EnableWebMvc+@Configuration+DelegatingWebMvcConfiguration 全面接管SpringMVC**



## 2、简单功能分析

### 2.1、静态资源访问

#### 2.1.1、静态资源目录

**静态资源默认放在类路径下**： called `/static` (or `/public` or `/resources` or `/META-INF/resources`

访问 ： 当前项目根路径/ + 静态资源名 



原理： 静态映射/**。

请求进来，**先去找Controller看能不能处理**。不能处理的所有请求又都交给静态资源处理器。静态资源也找不到则响应404页面



改变默认的静态资源路径(**改变之后，默认的路径会失效**)

```yml
spring:
  resources:
    static-locations: [classpath:/haha/]
```



#### 2.1.2、静态资源访问前缀

默认无前缀

```yml
spring:
  mvc:
    static-path-pattern: /res/**
```

当前项目 + static-path-pattern + 静态资源名 = 静态资源文件夹下找



```yml
spring:
#  mvc:
#    static-path-pattern: /res/**

  resources:
    add-mappings: false   禁用所有静态资源规则
```



#### 2.1.3、webjar

自动映射 /[webjars](http://localhost:8080/webjars/jquery/3.5.1/jquery.js)/**

<https://www.webjars.org/>

```
        <dependency>
            <groupId>org.webjars</groupId>
            <artifactId>jquery</artifactId>
            <version>3.5.1</version>
        </dependency>
```

访问地址：[http://localhost:8080/webjars/**jquery/3.5.1/jquery.js**](http://localhost:8080/webjars/jquery/3.5.1/jquery.js)   后面地址要按照依赖里面的包路径



### 2.2、欢迎页支持

- 静态资源路径下  index.html

- - 可以配置静态资源路径
  - 但是不可以配置静态资源的访问前缀。否则导致 index.html不能被默认访问

```yml
spring:
#  mvc:
#    static-path-pattern: /res/**   这个会导致welcome page功能失效

  resources:
    static-locations: [classpath:/haha/]
```

- controller能处理/index





### 2.3、自定义 `Favicon`

favicon.ico 放在静态资源目录下即可。

```yml
spring:
#  mvc:
#    static-path-pattern: /res/**   这个会导致 Favicon 功能失效
```



## 3、请求参数处理



### 3.1、rest使用与原理

- @xxxMapping；
- Rest风格支持（*使用**HTTP**请求方式动词来表示对资源的操作*）

- - *以前：**/getUser*   *获取用户*     */deleteUser* *删除用户*    */editUser*  *修改用户*       */saveUser* *保存用户*
  - *现在： /user*    *GET-**获取用户*    *DELETE-**删除用户*     *PUT-**修改用户*      *POST-**保存用户*

- (表单提交只有get、put，如何使用其他类型呢？)

- - 核心Filter；HiddenHttpMethodFilter

- - - 用法： 表单method=post，隐藏域 _method=put
    - SpringBoot中手动开启

```yml
spring:
  mvc:
    hiddenmethod:
      filter:
        enabled: true   #开启页面表单的Rest功能
```

```jsp
<form method="post" action="/test">
    <input name="_method" value="put"/>
    <input type="submit" value="提交"/>
</form>
```

- 扩展：如何把_method 这个名字换成我们自己喜欢的。

```java
//自定义filter
    @Bean
    public HiddenHttpMethodFilter hiddenHttpMethodFilter(){
        HiddenHttpMethodFilter methodFilter = new HiddenHttpMethodFilter();
        methodFilter.setMethodParam("_m");
        return methodFilter;
    }
```

Rest原理（表单提交要使用REST的时候）

- 表单提交会带上**_method=PUT**
- **请求过来被**HiddenHttpMethodFilter拦截

- - 请求是否正常，并且是POST

- - - 获取到**_method**的值。
    - 兼容以下请求；**PUT**.**DELETE**.**PATCH**
    - **原生request（post），包装模式requesWrapper重写了getMethod方法，返回的是传入的值。**
    - **过滤器链放行的时候用wrapper。以后的方法调用getMethod是调用**requesWrapper的。



**Rest使用客户端工具，**

- 如PostMan直接发送Put、delete等方式请求，无需Filter。



### 3.2、请求映射原理

![1622279515751](C:\Users\86185\AppData\Roaming\Typora\typora-user-images\1622279515751.png)

springBoot 默认的5个HandlerMapping

其中：

- **RequestMappingHandlerMapping**：保存了所有 @RequestMapping 和 handler 的映射规则。

- SpringBoot自动配置欢迎页的 WelcomePageHandlerMapping 。访问 /能访问到index.html；

- 我们需要一些自定义的映射处理，我们也可以自己给容器中放**HandlerMapping**。自定义 **HandlerMapping**



## 4、普通参数与基本注解

### 4.1、注解

@PathVariable、@RequestHeader、@ModelAttribute、@RequestParam、@MatrixVariable、@CookieValue、@RequestBody

```java
@RequestMapping("/test/{id}/{name}")
    public Map test5(@PathVariable(value = "id",required = false) Integer id,
                     @PathVariable(value = "name",required = false) String usernmae,
                     @RequestHeader("accept") String accept,
                     @ModelAttribute("Person") Person person,//以“Person”为键值将对象存储在Model中，如果没有指定键值，以“person”(变量名)为键值存储在Model中
                     @RequestParam("car") String car,
                     @CookieValue("freePromorunningtmr") String _ga,
                     @CookieValue("freePromorunningtmr") Cookie cookie
                     ){

            Map map = new HashMap();
            map.put("id",id);
            map.put("name",usernmae);
            map.put("header",accept);
            map.put("person",person);
            map.put("car",car);
            map.put("_ga",_ga);
            System.out.println(cookie.getName()+"===>"+cookie.getValue());

            return map;
    }
```

```java
矩阵传参
//1、语法： 请求路径：/cars/sell;low=34;brand=byd,audi,yd
//2、SpringBoot默认是禁用了矩阵变量的功能
//     手动开启：原理。对于路径的处理。UrlPathHelper进行解析。
//              removeSemicolonContent（移除分号内容）支持矩阵变量的
//3、矩阵变量必须有url路径变量才能被解析
@GetMapping("/cars/{path}")
public Map carsSell(@MatrixVariable("low") Integer low,
                    @MatrixVariable("brand") List<String> brand,
                    @PathVariable("path") String path){//指有效路径，后面的带参路径不算
    Map<String,Object> map = new HashMap<>();

    map.put("low",low);
    map.put("brand",brand);
    map.put("path",path);
    return map;
}
```

```java
//开启矩阵变量传参
@Bean
public WebMvcConfigurer webMvcConfigurer() {
    return new WebMvcConfigurer() {
        @Override
        public void configurePathMatch(PathMatchConfigurer configurer) {
            UrlPathHelper urlPathHelper = new UrlPathHelper();
            // 不移除；后面的内容。矩阵变量功能就可以生效
            urlPathHelper.setRemoveSemicolonContent(false);
            configurer.setUrlPathHelper(urlPathHelper);
        }
    };
}
```



| 注解            | 描述                                                         |
| --------------- | ------------------------------------------------------------ |
| @PathVariable   | 将路径中的变量赋值给控制器的参数，路径变量采用 REST 风格	（ /test/{id}） |
| @RequestHeader  | 能够将请求头中的变量值映射到控制器的参数中                   |
| @ModelAttribute | 1、绑定请求参数到实体对象（**表单提供参数的优先，其次路径参数，参数名必须与属性名一致**）2、注解一个非请求处理方法（被 @ModelAttribute 注解的方法将在每次调用该控制器类的请求处理方法前被调用） |
| @RequestParam   | 将请求中的参数赋值给控制器的参数，（ /test?car=baoma ）      |
| @MatrixVariable | 矩阵变量传参，各个属性之间用分号分割开，/cars/sell;low=34;brand=byd,audi,yd |
| @CookieValue    | 用来获取Cookie中的值                                         |
| @RequestBody    | 用来接收 json 数据                                           |



### 4.2、Servlet API

WebRequest、ServletRequest、MultipartRequest、 HttpSession、javax.servlet.http.PushBuilder、Principal、InputStream、Reader、HttpMethod、Locale、TimeZone、ZoneId

由 **ServletRequestMethodArgumentResolver**  请求参数解析器进行解析



### 4.3、复杂参数

**Map**、**Model（map、model里面的数据会被放在request的请求域  request.setAttribute）、**Errors/BindingResult、**RedirectAttributes（ 重定向携带数据）**、**ServletResponse（response）**、SessionStatus、UriComponentsBuilder、ServletUriComponentsBuilder

```
Map<String,Object> map,  Model model, HttpServletRequest request 都是可以给request域中放数据，
request.getAttribute();	取数据
```





### 4.4、自定义对象参数

可以自动类型转换与格式化，可以级联封装

碰见特殊情况，我们可以在 WebDataBinder 里面放自己的 Converter

比如将传入的 String 转成 Pet 类接收

需求：输入宠物时，直接以 “名字，年龄” 的形式输入，就需要将 String 直接转成 Pet

```xml
<form name="person" method="post" action="/test/1/zw">
    <input name="_method" value="put"/>
    <input name="id" type="text"/>
    <input name="name" type="text"/>
<!--    <input name="pet.name"/>-->
<!--    <input name="pet.age"/>-->
   <input name="pet"/>
    <input type="submit" value="提交"/>
</form>
```

```java
//WebMvcConfigurer定制化SpringMVC的功能
    @Bean
    public WebMvcConfigurer webMvcConfigurer() {
        return new WebMvcConfigurer() {
            @Override
            public void configurePathMatch(PathMatchConfigurer configurer) {
                UrlPathHelper urlPathHelper = new UrlPathHelper();
                // 不移除；后面的内容。矩阵变量功能就可以生效
                urlPathHelper.setRemoveSemicolonContent(false);
                configurer.setUrlPathHelper(urlPathHelper);
            }

            //自定义 Converter
            @Override
            public void addFormatters(FormatterRegistry registry) {
                registry.addConverter(new Converter<String, Pet>() {
                    @Override

                    public Pet convert(String s) {
                        // 啊猫,3
                        if (!StringUtils.isEmpty(s)) {
                            Pet pet = new Pet();
                            String[] split = s.split(",");
                            pet.setName(split[0]);
                            pet.setAge(Integer.parseInt(split[1]));
                            return pet;
                        }
                        return null;
                    }
                });
            }
        };
    }
```







## 5、数据响应与内容协商

### 5.1、响应JSON

jackson.jar+@ResponseBody

（web场景自动引入了json场景）

**返回值解析器原理原理：**

- - - 1、内容协商（浏览器默认会以请求头的方式告诉服务器他能接受什么样的内容类型）
    - 2、服务器最终根据自己自身的能力，决定服务器能生产出什么样内容类型的数据，
    - 3、SpringMVC会挨个遍历所有容器底层的 HttpMessageConverter ，看谁能处理？

- - - - 1、得到MappingJackson2HttpMessageConverter可以将对象写为json
      - 2、利用MappingJackson2HttpMessageConverter将对象转为json再写出去。

**SpringMVC到底支持哪些返回值**：

```
ModelAndView
Model
View
ResponseEntity 
ResponseBodyEmitter
StreamingResponseBody
HttpEntity
HttpHeaders
Callable
DeferredResult
ListenableFuture
CompletionStage
WebAsyncTask
有 @ModelAttribute 且为对象类型的
@ResponseBody 注解的
```



### 5.2、响应XML

1、导入依赖

```xml
 <dependency>
            <groupId>com.fasterxml.jackson.dataformat</groupId>
            <artifactId>jackson-dataformat-xml</artifactId>
</dependency>
```

导入了jackson处理xml的包，xml的converter就会自动进来



2、测试返回 json 和xml

一般页面返回 xml 的 优先级高于返回 json

所以导入xml 依赖后网页会从返回 json 转为返回 xml

​		![1622388525348](C:\Users\86185\AppData\Roaming\Typora\typora-user-images\1622388525348.png)

![1622388424866](C:\Users\86185\AppData\Roaming\Typora\typora-user-images\1622388424866.png)



### 5.3、内容协商

根据客户端接收能力不同，返回不同媒体类型的数据

内容协商默认的协商策略为**基于请求头**，为了方便内容协商，我们可以开启**基于请求参数**的内容协商功能

```yml
spring:
  mvc:
    contentnegotiation:
      favor-parameter: true  #开启请求参数内容协商模式
```

Parameter策略：默认情况下传的参数名为 format，且值只有 xml 和 json 两个，如果想传其他的，需要我们自定义策略

Header策略：获取客户端（PostMan、浏览器）支持接收的内容类型（获取客户端Accept请求头字段）





### 5.4、自定义 MessageConverter（协商策略）

**实现多协议数据兼容 json、xml、x-guigu**

比如：一般输出 Person 是直接输出其属性，是分开的（json、xml）

需求：现在需要将 Person 的 id、name、birth 连着输出，以 x-guigu 的形式

1、@ResponseBody 响应数据出去 调用 **RequestResponseBodyMethodProcessor** 处理

2、Processor 处理方法返回值。通过 **MessageConverter** 处理

3、所有 **MessageConverter** 合起来可以支持各种媒体类型数据的操作（读、写）

4、内容协商找到最终的 **messageConverter**；



#### 5.4.1、实现

（1）自定义MessageConverter

```java
package com.dzx.boot.converter;

import com.dzx.boot.pojo.Person;
import org.springframework.http.HttpInputMessage;
import org.springframework.http.HttpOutputMessage;
import org.springframework.http.MediaType;
import org.springframework.http.converter.HttpMessageConverter;
import org.springframework.http.converter.HttpMessageNotReadableException;
import org.springframework.http.converter.HttpMessageNotWritableException;

import java.io.IOException;
import java.io.OutputStream;
import java.util.List;

public class MyMessageConverter implements HttpMessageConverter<Person> {
    @Override
    //如果这里设置为true，那就可以读@RequestBody的值
    public boolean canRead(Class<?> aClass, MediaType mediaType) {
        return false;
    }

    @Override
    public boolean canWrite(Class<?> aClass, MediaType mediaType) {
        return aClass.isAssignableFrom(Person.class);
    }

    /**
     * 服务器要统计所有MessageConverter都能写出哪些内容类型
     *
     * application/x-guigu
     * @return
     */
    @Override
    public List<MediaType> getSupportedMediaTypes() {
        return MediaType.parseMediaTypes("application/x-guigu");
    }

    @Override
    public Person read(Class<? extends Person> aClass, HttpInputMessage httpInputMessage) throws IOException, HttpMessageNotReadableException {
        return null;
    }

    @Override
    public void write(Person person, MediaType mediaType, HttpOutputMessage httpOutputMessage) throws IOException, HttpMessageNotWritableException {
        //自定义协议数据的写出
        String data =  person.getId()+";"+person.getName()+";"+person.getBirth();
        //写出去
        OutputStream body = httpOutputMessage.getBody();
        body.write(data.getBytes());
    }
}
```

(2)**在 WebMvcConfigurer 添加进行自己配置的** MessageConverter，以及添加自定义**内容协商管理器**

```java
//WebMvcConfigurer定制化SpringMVC的功能
@Bean
public WebMvcConfigurer webMvcConfigurer() {
    return new WebMvcConfigurer() {
      
        //添加自定义协商策略
        //如果使用的是configureMessageConverters，那么是覆盖了默认的所有messageconverter
        //这里用的是 extendMessageConverters
        @Override
        public void extendMessageConverters(List<HttpMessageConverter<?>> converters) {
            converters.add(new MyMessageConverter());
        }
		//自定义内容协商管理器
        @Override
        public void configureContentNegotiation(ContentNegotiationConfigurer configurer) {
            Map<String, MediaType> mediaTypes = new HashMap<>();
            mediaTypes.put("json", MediaType.APPLICATION_JSON);
            mediaTypes.put("xml",MediaType.APPLICATION_XML);
            mediaTypes.put("gg",MediaType.parseMediaType("application/x-guigu"));
            //指定支持解析哪些参数对应的哪些媒体类型
            ParameterContentNegotiationStrategy parameterStrategy = new ParameterContentNegotiationStrategy(mediaTypes);
            //修改Header策略所需参数的名字,默认是 format
            //parameterStrategy.setParameterName("ff");

            //添加漏掉的Header策略
            HeaderContentNegotiationStrategy headeStrategy = new HeaderContentNegotiationStrategy();

            configurer.strategies(Arrays.asList(parameterStrategy,headeStrategy));
          }  
        }
    };
}
```

  但是在这里要注意：如果我们自定义了参数策略，他的头策略反而会失效，在spingboot中，有可能我们添加的自定义的功能会覆盖默认很多功能，导致一些默认的功能失效,这里的解决方法是在**configureContentNegotiation中也添加自定义头策略**



（3）请求方法

```java
@RequestMapping("/t")
public Person test7(){
    Person person = new Person();
    person.setName("zhansan");
    person.setId(1);
    return person;
}
```



#### 5.4.2、结果

![1622392207407](C:\Users\86185\AppData\Roaming\Typora\typora-user-images\1622392207407.png)



![1622392188039](C:\Users\86185\AppData\Roaming\Typora\typora-user-images\1622392188039.png)

![1622392160968](C:\Users\86185\AppData\Roaming\Typora\typora-user-images\1622392160968.png)

 

#### 5.4.3、总结

有可能我们添加的自定义的功能会覆盖默认很多功能，导致一些默认的功能失效。

所以上面我们需要将被丢弃的Header策略添加回来





## 6、视图解析与模板引擎

![1622449177319](C:\Users\86185\AppData\Roaming\Typora\typora-user-images\1622449177319.png)

### 6.1、视图解析原理流程

1、目标方法处理的过程中，所有数据都会被放在 **ModelAndViewContainer 里面。包括数据和视图地址**

2、如果方法的参数是一个自定义类型对象（从请求参数中确定的），把他重新放在 **ModelAndViewContainer** 

3、任何目标方法执行完成以后都会返回 ModelAndView（数据和视图地址）

4、processDispatchResult  处理派发结果（页面改如何响应）

**视图解析：**

- - **返回值以 forward: 开始： new InternalResourceView(forwardUrl); -->  转发**request.getRequestDispatcher(path).forward(request, response);  
  - **返回值以** **redirect: 开始：** **new RedirectView() --》 render就是重定向** 
  - **返回值是普通字符串： new ThymeleafView（）--->** 



### 6.2、模板引擎-Thymeleaf

现代化、服务端Java模板引擎



### **基本语法：**

#### 6.2.1、表达式

| 表达式名字 | 语法       | 用途                               |
| ---------- | ---------- | ---------------------------------- |
| 变量取值   | ${...}     | 获取请求域、session域、对象等值    |
| 选择变量   | *{...}     | 获取上下文对象值                   |
| 消息       | #{...}     | 获取国际化等值                     |
| 链接       | @{...}     | 生成链接                           |
| 片段表达式 | ~{...}     | jsp:include 作用，引入公共页面片段 |
| 行类写法   | [[${...}]] | 没有标签时使用                     |



#### 6.2.2、字面量

文本值: **'one text'** **,** **'Another one!'** **,…**数字: **0** **,** **34** **,** **3.0** **,** **12.3** **,…**布尔值: **true** **,** **false**

空值: **null**

变量： one，two，.... 变量不能有空格

#### 6.2.3、文本操作

字符串拼接: **+**

变量替换: **|The name is ${name}|** 



#### 6.2.4、数学运算

运算符: + , - , * , / , %



#### 6.2.5、布尔运算

运算符:  **and** **,** **or**

一元运算: **!** **,** **not** 



#### 6.2.6、比较运算

比较: **>** **,** **<** **,** **>=** **,** **<=** **(** **gt** **,** **lt** **,** **ge** **,** **le** **)**等式: **==** **,** **!=** **(** **eq** **,** **ne** **)** 



#### 6.2.7、条件运算

If-then: **(if) ? (then)**

If-then-else: **(if) ? (then) : (else)**

Default: (value) **?: (defaultvalue)** 



#### 6.2.8、特殊操作

无操作： _



#### 6.2.9、设置属性值-th:attr

设置单个值

```html
<form action="subscribe.html" th:attr="action=@{/subscribe}">
  <fieldset>
    <input type="text" name="email" />
    <input type="submit" value="Subscribe!" th:attr="value=#{subscribe.submit}"/>
  </fieldset>
</form>
```

设置多个值

```html
<img src="../../images/gtvglogo.png"  th:attr="src=@{/images/gtvglogo.png},title=#{logo},alt=#{logo}" />
```



以上两个的代替写法 th:xxxx

```html
<input type="submit" value="Subscribe!" th:value="#{subscribe.submit}"/>
<form action="subscribe.html" th:action="@{/subscribe}">
```



所有h5兼容的标签写法

<https://www.thymeleaf.org/doc/tutorials/3.0/usingthymeleaf.html#setting-value-to-specific-attributes>



#### 6.2.10、迭代

```html
<tr th:each="prod : ${prods}">
        <td th:text="${prod.name}">Onions</td>
        <td th:text="${prod.price}">2.41</td>
        <td th:text="${prod.inStock}? #{true} : #{false}">yes</td>
</tr>
```

```html
iterStat为状态变量，其中可以使用count、index来计数
<tr th:each="prod,iterStat : ${prods}" th:class="${iterStat.odd}? 'odd'">
  <td th:text="${prod.name}">Onions</td>
  <td th:text="${prod.price}">2.41</td>
  <td th:text="${prod.inStock}? #{true} : #{false}">yes</td>
</tr>
```



#### 6.2.11、条件运算

```html
<a href="comments.html"
th:href="@{/product/comments(prodId=${prod.id})}"
th:if="${not #lists.isEmpty(prod.comments)}">view</a>
```

```html
<div th:switch="${user.role}">
  <p th:case="'admin'">User is an administrator</p>
  <p th:case="#{roles.manager}">User is a manager</p>
  <p th:case="*">User is some other thing</p>
</div>
```

 

#### 6.2.12、属性优先级

![image.png](https://cdn.nlark.com/yuque/0/2020/png/1354552/1605498132699-4fae6085-a207-456c-89fa-e571ff1663da.png?x-oss-process=image%2Fwatermark%2Ctype_d3F5LW1pY3JvaGVp%2Csize_14%2Ctext_YXRndWlndS5jb20g5bCa56GF6LC3%2Ccolor_FFFFFF%2Cshadow_50%2Ct_80%2Cg_se%2Cx_10%2Cy_10)

####  

### 6.3、thymeleaf使用

#### 6.3.1、引入Starter

```xml
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-thymeleaf</artifactId>
        </dependency>
```

#### 6.3.2、SpringBoot 自动配好的策略

- 所有thymeleaf的配置值都在 ThymeleafProperties
- 配置好了 **SpringTemplateEngine** 
- 配好了 **ThymeleafViewResolver** 
- 我们只需要直接开发页面（**将页面放在指定路径下 "classpath:/templates/"，后缀为 html**）
- **在网页头部添加标签** <html lang="en" xmlns:th="http://www.thymeleaf.org">

```html
    public static final String DEFAULT_PREFIX = "classpath:/templates/";

    public static final String DEFAULT_SUFFIX = ".html";  //xxx.html
```

#### 6.3.3、页面开发

```html
<!DOCTYPE html>
<html lang="en" xmlns:th="http://www.thymeleaf.org">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
</head>
<body>
<h1 th:text="${msg}">哈哈</h1>
<h2>
    <a href="www.atguigu.com" th:href="${link}">去百度</a>  <br/>
    <a href="www.atguigu.com" th:href="@{link}">去百度2</a>
</h2>
</body>
</html>
```





## 7、拦截器

![1622450196922](C:\Users\86185\AppData\Roaming\Typora\typora-user-images\1622450196922.png)

### 7.1、拦截器原理

1、根据当前请求，找到**HandlerExecutionChain【**可以处理请求的handler以及handler的所有 拦截器】

2、先来**顺序执行** 所有拦截器的 preHandle方法

- 1、如果当前拦截器prehandler返回为true。则执行下一个拦截器的preHandle
- 2、如果当前拦截器返回为false。直接    倒序执行所有**已经执行了**的拦截器的  afterCompletion；

**3、如果任何一个拦截器返回false。直接跳出不执行目标方法**

**4、所有拦截器都返回True。执行目标方法**

**5、倒序执行所有拦截器的postHandle方法。**

**6、前面的步骤有任何异常都会直接倒序触发** **已经执行了的**afterCompletion

7、页面成功渲染完成以后，也会倒序触发 afterCompletion



### 7.2、使用

- 编写一个拦截器实现HandlerInterceptor接口

- 将拦截器注册到容器中（实现WebMvcConfigurer的addInterceptors方法）
- 指定拦截规则【如果是拦截所有，静态资源也会被拦截】

```java
@Configuration
public class AdminWebConfig implements WebMvcConfigurer {

    @Override
    public void addInterceptors(InterceptorRegistry registry) {
        //添加拦截器并且设置拦截规则
        registry.addInterceptor(new LoginInterceptor())
                .addPathPatterns("/**")  //所有请求都被拦截包括静态资源             .excludePathPatterns("/","/login","/css/**","/fonts/**","/images/**","/js/**"); //放行的请求
    }
}
```



### 7.3、路径匹配

| Wildcard | Description             |      |
| -------- | ----------------------- | ---- |
| ?        | 匹配任何单字符          |      |
| *        | 匹配0或者任意数量的字符 |      |
| **       | 匹配0或者更多的目录     |      |







## 8、文件上传（下载）

### 上传

### 8.1、自动配置原理

**文件上传自动配置类-MultipartAutoConfiguration-**MultipartProperties

- 自动配置好了 **StandardServletMultipartResolver   【文件上传解析器】**
- **原理步骤**

- - **1、请求进来使用文件上传解析器判断（**isMultipart**）并封装（**resolveMultipart，**返回**MultipartHttpServletRequest**）文件上传请求**
  - **2、参数解析器来解析请求中的文件内容封装成MultipartFile**
  - **3、将request中文件信息封装为一个Map；**MultiValueMap<String, MultipartFile>

**FileCopyUtils**。实现文件流的拷贝



### 8.2、实现

- 表单：必须是post，且 enctype = **“multipart/form-data“**

  ```html
  <form method="post" action="/upload" enctype="multipart/form-data">
      <input type="file" name="file"><br>
      <input type="file" name="files" multiple><br>
      <input type="submit" value="提交">
  </form>
  ```

- 关键代码

```java
//文件上传
@RequestMapping("/upload")
public String upload(@RequestPart("file") MultipartFile headerImg,
                     @RequestPart("files") MultipartFile[] photos) throws IOException {
    //单文件
    if(!headerImg.isEmpty()){
        //保存到文件服务器，OSS服务器
        String originalFilename = headerImg.getOriginalFilename();
        headerImg.transferTo(new File("D:\\"+originalFilename));
    }

    //多文件
    if(photos.length > 0) {
        for (MultipartFile photo : photos) {
            if (!photo.isEmpty()) {
                String originalFilename = photo.getOriginalFilename();
                photo.transferTo(new File("D:\\" + originalFilename));
            }
        }
    }

    return "success";
}
```



### 下载

可以根据需要使用不同的流

```java
 /**
     * 文件下载
     */
    @RequestMapping("/dowload/{name}")
    public void dowload(@PathVariable("name") String fileName, HttpServletResponse resp, HttpServletRequest req) throws IOException {
        resp.setContentType("application/x-msdownload");
        //解决下载文件时,默认文件名出现"___"乱码现象
//        filename= new String(filename.getBytes("gbk"), "iso8859-1");

        //解决部分空格被+号替换了，但中文乱码
//        fileName = new String(fileName.getBytes("GBK"), "ISO-8859-1");
//        resp.setHeader("Content-disposition", "attachment;filename=" + fileName );

        //解决了中文乱码，但部分空格被+号替换了
        resp.setHeader("Content-Disposition", "attachment;filename=" + URLEncoder.encode(fileName, "GBK"));
        OutputStream outputStream = resp.getOutputStream();
        //资源路径
        String path = "D:\\work\\programe\\Moon\\src\\main\\resources\\static\\file\\" + fileName;
        InputStream inputStream = new FileInputStream(path);
        int temp = 0;
        while ((temp = inputStream.read()) != -1) {
            outputStream.write(temp);
        }
        inputStream.close();
        outputStream.close();
    }
```



## 9、异常处理

### 9.1、错误处理

默认规则：

- Spring Boot 会自动处理`/error` 请求
- 名字为的4xx.html，5xx.html的页面会被自动解析，但需要注意存放位置（自定义错误页面）

```
方式一：在resources/static/error/ 下定义
方式二：在templates/error/ 下定义（是模板引擎页面，需要加头标签）
注：templates/error/ 这个的优先级比较 resources/static/error/高
```

- error/404.html   error/5xx.html；**有精确的错误状态码页面就匹配精确**，没有就找 4xx.html；如果都没有就触发白页

如果失效的话，就在springboot配置文件中写

```yaml
server:
  error:
    # 此处可随便写一个路径，或者留空也行，只有不和自定义的error路径冲突，（会与index页面冲突）
    path: /error	
```





### 9.2、自定义异常处理

要完全替换默认行为，可以实现 `ErrorController `并注册该类型的Bean定义，或添加`ErrorAttributes类型的组件`以使用现有机制但替换其内容。

详细：https://blog.csdn.net/weixin_44349116/article/details/107114079





## 10、Web原生组件注入（Servlet、Filter、Listener）

往 Ioc 中加入新的 Web 原生组件

### 10.1 注入方式

- 注解+扫包
- 对应的 RegistrationBean 中添加新组件对象

#### 10.1.1使用注解

@ServletComponentScan(basePackages = **"com.atguigu.admin"**) :扫包识别Servlet组件，有默认规则（主程序所在包及其子包）

Servlet：@WebServlet(urlPatterns = **"/my"**)：效果：直接响应，**没有经过Spring的拦截器？**

Filter：@WebFilter(urlPatterns={**"/css/\*"**,**"/images/\*"**})

Listener：@WebListener



以 Servlet 为例子，Filter、Listener 类似

1. 在新的组件中添加注解（@WebServlet、@WebFilter、@WebListener）
2. 在主程序上添加包扫描（@ServletComponentScan）

```java
@WebServlet("/my")
public class MyServlet extends FrameworkServlet {
    @Override
    protected void doService(HttpServletRequest httpServletRequest, HttpServletResponse httpServletResponse) throws Exception {
        System.out.println("自制Servlet执行....");
    }
}
```

```java
@ServletComponentScan
@SpringBootApplication
public class MainApplication {

    public static void main(String[] args) {

        ConfigurableApplicationContext run = SpringApplication.run(MainApplication.class, args);
    }
}
```



扩展：容器中已经自动配置了 DispatchServlet （通过 **spring.mvc** 进行配置修改），默认映射的是 / 路径

![1622560572219](C:\Users\86185\AppData\Roaming\Typora\typora-user-images\1622560572219.png)

**多个Servlet都能处理到同一层路径时，采用精确优选原则**

 /my ：使用 MyServlet，被 Tomcat 处理，所以不会被 Spring 配置的拦截器拦截

 /my/1：使用 DispatcherServlet，只有由DispatcherServlet(Spring)处理的请求才会经过spring的拦截器





#### 10.1.2使用RegistrationBean

（Servlet、Filter、Listener）分别对应

`ServletRegistrationBean`, `FilterRegistrationBean`, and `ServletListenerRegistrationBean`

1. 得到新的组件
2. 用@Bean 创建对应的容器管理（ServletRegistrationBean、 FilterRegistrationBean 、ServletListenerRegistrationBean）
3. 使用容器管理添加新的组件

（以 Servlet 为例子）

```
public class MyServlet extends HttpServlet {
    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        System.out.println("成功");
    }

}
```

```java
@Configuration
public class MyRegistConfig {

    @Bean
    public ServletRegistrationBean myServlet(){
        MyServlet myServlet = new MyServlet();

        return new ServletRegistrationBean(myServlet,"/my","/my02");//路径
    }


    @Bean
    public FilterRegistrationBean myFilter(){

        MyFilter myFilter = new MyFilter();
//        return new FilterRegistrationBean(myFilter,myServlet());拦截路径继承Servlet的
        FilterRegistrationBean filterRegistrationBean = new FilterRegistrationBean(myFilter);
        filterRegistrationBean.setUrlPatterns(Arrays.asList("/my","/css/*"));
        return filterRegistrationBean;
    }

    @Bean
    public ServletListenerRegistrationBean myListener(){
        MySwervletContextListener mySwervletContextListener = new MySwervletContextListener();
        return new ServletListenerRegistrationBean(mySwervletContextListener);
    }
}
```





## 11、内置 Tomcat (Servlet容器)

### **11.1 原理**

- - SpringBoot应用启动发现当前是Web应用。web场景包-导入tomcat
  - web应用会创建一个web版的ioc容器 `ServletWebServerApplicationContext` 
  - `ServletWebServerApplicationContext`  启动的时候寻找 **ServletWebServerFactory**`（Servlet 的web服务器工厂---> Servlet 的web服务器）`  
  - SpringBoot底层默认有很多的WebServer工厂；`TomcatServletWebServerFactory`, `JettyServletWebServerFactory`, or `UndertowServletWebServerFactory`
  - `底层直接会有一个自动配置类。ServletWebServerFactoryAutoConfiguration`
  - `ServletWebServerFactoryAutoConfiguration导入了ServletWebServerFactoryConfiguration（配置类）`
  - `ServletWebServerFactoryConfiguration 配置类 根据动态判断系统中到底导入了那个Web服务器的包。（默认是web-starter导入tomcat包），容器中就有 TomcatServletWebServerFactory`
  - `TomcatServletWebServerFactory 创建出Tomcat服务器并启动；TomcatWebServer 的构造器拥有初始化方法initialize---this.tomcat.start();`
  - `内嵌服务器，就是手动把启动服务器的代码调用（tomcat核心jar包存在）`





### 11.2 定制Servlet容器

- 方式1：实现  **WebServerFactoryCu**stomizer<ConfigurableServletWebServerFactory> 接口



- 方式二：修改配置文件使用 **server.xxx**
- 方式三：直接自定义 **ConfigurableServletWebServerFactory** 

**xxxxx**Customizer：定制化器，可以改变xxxx的默认规则

```java
//方式一：
@Component
public class CustomizationBean implements WebServerFactoryCustomizer<ConfigurableServletWebServerFactory> {

    @Override
    public void customize(ConfigurableServletWebServerFactory server) {
        server.setPort(9000);
    }

}
```



## 12、（自定义）定制化原理

几种常见方式：

- 使用配置文件进行修改
- **xxxxxCustomizer**（用法见11.2）
- **编写自定义的配置类 @Configuration；+  **@Bean替换、增加容器中默认组件
- **Web应用编写一个配置类实现 WebMvcConfigurer 即可定制化web功能；**（重写方法）

【@EnableWebMvc + WebMvcConfigurer —— @Bean  可以全面接管SpringMVC，所有规则全部自己重新配置； 实现定制和扩展功能，**这也会导致所有默认功能失效**】





## 13、原理分析套路

**场景starter** - xxxxAutoConfiguration - 导入xxx组件 - 绑定xxxProperties -- **绑定配置文件项** 

我们只需要注重：

1. 导入场景
2. 根据需求编写配置文件

（有需要也可以通过定制化原理将自定义组件或则第三方组件添加到容器中）





