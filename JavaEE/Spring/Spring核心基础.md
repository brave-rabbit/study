# Spring 框架

Spring 是一个企业级开发框架，是软件设计层面的框架，可以将应用程序进行分层，开发者可以自主选择组件

优点：

1. 非侵入式设计
2. 方便解耦，简化开发
3. 支持AOP，对通用任务进行集中式处理，提高了程序的复用性
4. 方便程序的测试
5. 方便集成各种优秀框架，如 MyBatis、Struts2等
6. 降低 JavaEE API 的使用难度（Spring 提供了相关的封装）

两大核心机制：

- IoC （DI）
- AOP



## IoC

Inversion of Control  ，控制反转：当对象A需要调用对象B时，传统方式是由开发者主动使用 New 去创建对象B，但 IoC 模式下，由 IoC 容器创建对象B，开发者直接调用即可，控制权发生了反转

最常见方式：DI（Dependency Injection  依赖注入）【**将被依赖对象赋值给调用者的成员变量**】

相关 jar 包：**spring-context** 

**实现步骤**：

1.XML 中配置 bean（完成依赖注入）

2.利用 ApplicatonContext 对象获取 bean

```java
ApplicationContext applicationContext=new ClassPathXmlApplicationContext("applicationContext.xml");
Student student = (Student) applicationContext.getBean("stu");
```



### bean 的实例化

**有三种方式：** 

- 构造器实例化（最常用）
- 工厂方式实例化（静态工厂方式实例化、实例工厂方式实例化）详见后文的 “spring的工厂方法”

> 采用构造器实例化：

```XML
//XML 配置文件
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">

      <bean id="student" class="com.dzx.exmple.spring.Student">	//构造器实例化
          	<!-- 有参构造-->
         	<constructor-arg name="id" value="1"></constructor-arg>
            <constructor-arg name="name" value="dzx"></constructor-arg>
            <constructor-arg name="address" ref="address"></constructor-arg>
            <!--赋值-->
            <property name="id" value="1"></property>
            <property name="name" value="张三"></property>
            <property name="address" ref="address"></property>
      </bean>

      <bean id="address" class="com.dzx.exmple.spring.Address">
            <property name="content" value="湖南工业大学"></property>
      </bean>
</beans>
```

```java
//Student类
@Data
public class Student {
    private Integer id;
    private String name;
    private Address address;
}

//Address类
@Data
public class Address {
    private String content;

}

public class Test {
    public static void main(String[] args) {
        ApplicationContext applicationContext=new ClassPathXmlApplicationContext("applicationContext.xml");
        Student student = (Student) applicationContext.getBean("student");
        System.out.println(student);
    }
}
```



### 配置文件

- 通过 <bean> 标签来完成对象的管理

  id：对象名

  class：对象的模板类（**所有交给 IoC 容器来管理的类必须要有无参构造函数（使用构造器实例化的情况下），因为 Spring 底层是通过反射机制来创建对象，需要调用无参构造函数**）

- 对象成员变量通过 <property> 标签完成赋值 or 通过 <constructor-arg> 标签完成有参构造

  name：成员变量名

  value：成员变量值（只适用**基本数据类型**和 **String 数据类型**）

  ref：成员变量值，将 IoC 中的另一个 bean 赋给当前的成员变量（适用于非基本数据/String 类型）

  **【基本数据类型和String 用 value，其他类型用 ref】**




### 获取 bean

- 通过 id 获取，即 bean 标签中的 id
- 通过运行时类，即对象的类名+".class"，此方法只适用于只有**唯一**一个该数据类型的 bean，有多个的话会报错



### 给 bean 注入集合

以 list 为例，set、map于之类似

```XML
<property name="list" >
      <list>
            <value>"first"</value>				//基本数据类型和String 用 value，其他类型用 ref
            <value>"second"</value>
      </list>
</property>
```



### scope

Spring 管理的 bean 是根据 scope 来生成的，**表示 bean 的作用域**，**默认是 singleton**

- singleton：单例，在 IoC 中只有一个实例，通过 IoC 获取的 bean 是**唯一**的，即是同一个对象
- prototype：原型，每次通过 IoC 获取 bean 时，容器都会创建一个新的 bean，即是不同的对象

prototype 模式 ：**当获取 IoC 容器中的 bean 时，Spring 才去调用无参构造创建对应的 bean**

singleton 模式：**Spring 在加载配置文件的时候就会创建 bean，不管是否要获取 bean**



### Spring 继承

于 Java 中的继承不同，Spring 是**对象层面**的继承，即子 bean 可以继承父 bean 的属性值，通过属性 **parent** 实现

```XML
<bean id="student" class="com.dzx.exmple.spring.Student">
      <property name="id" value="1"></property>
      <property name="name" value="张三"></property>
      <property name="address" ref="address"></property>
</bean>

<bean id="address" class="com.dzx.exmple.spring.Address" scope="prototype" >
      <property name="content" value="湖南工业大学"></property>
</bean>

<bean id="student2" class="com.dzx.exmple.spring.Student" parent="student">
      <property name="name" value="zw"></property>			//可以进行覆盖
</bean>
```

可以是**不同类**之间的继承，但有前提：子类对象必须包含父类对象所有的属性，同时可以在此基础上添加其他属性



### Spring 依赖

正常情况下是根据 <bean> 在配置文件中的书写顺序来决定对象创建顺序，但是如果 bean 和 bean 之间存在依赖（通过属性 **depends-on** 实现），被依赖的 bean 先被创建，再创建依赖的 bean ，即 A 依赖 B，先创建 B 再创建A，

```xml
//student 依赖 address，所以先创建 address 再创建 studnet
<bean id="student" class="com.dzx.exmple.spring.Student" depends-on="address">
  	<property name="id" value="1"></property>
  	<property name="name" value="张三"></property>
</bean>

<bean id="address" class="com.dzx.exmple.spring.Address"></bean>
```



### Spring 的 p 命名空间

p 命名空间是对 IoC/DI 的简化操作，使用 p 命名空间可以更方便的完成 bean 的配置以及 bean 之间的依赖注入

```xml
<!--  不使用p命名空间    -->
<bean id="student" class="com.dzx.exmple.spring.Student">
      <property name="id" value="1"></property>
      <property name="name" value="张三"></property>
      <property name="address" ref="address"></property>
</bean>

<!--  使用p命名空间    -->
<bean id="student2" class="com.dzx.exmple.spring.Student" p:id="1" p:name="dzx" p:address-ref="address"></bean>


<bean id="address" class="com.dzx.exmple.spring.Address">
      <property name="content" value="湖南工业大学"></property>
</bean>
```

 两者效果是一样的，但 p 命名空间更加简化，注意要加入（	xmlns:p="http://www.springframework.org/schema/p"）



### Spring 的工厂方法

IoC 通过工厂模式创建 bean 有两种方式：

- 静态工厂方式（工厂类提供**静态**方法创建对象，只需要配置一个目标 bean）
- 实例工厂方式（工厂类提供**普通**方法创建对象，需要配置两个 bean，一个是工厂 bean，一个是目标 bean）



> 静态工厂方式实例化
>

创建一个工厂类（普通类），同时为该类创建一个**静态**方法，这个方法用来创建 bean，并且在配置文件中<bean>标签的 class 属性所指定的**不再是 bean 的实现类**，而是**静态工厂类**，同时还需要使用 factory-method 属性来指定静态工厂类中的方法名（创建 bean 的方法）

```java
//工厂类
public class StaticFactory {
	//使用自己的工厂创建Student实例
    public static Student getStudent(){
        return new Student();
    }
}
```

配置文件：

```xml
<bean id="stu" class="com.dzx.exmple.spring.StaticFactory" factory-method="getStudent"></bean>
```



> 实例工厂方式实例化

创建一个工厂类（普通类），同时为该类创建一个（**非静态**）方法，这个方法用来创建 bean，并且在配置文件中**先配置一个工厂bean**，然后再配置一个**目标 bean**，这个 bean **不需要** class 属性，而使用 **factory-bean 和 factory-method** 属性，它们的值分别对应工厂 bean 的 **id** 和 工厂类中创建 bean 的**方法名**

```java
//工厂类
public class Factory {
    public Student getStudent(){
        return new Student();
    }
}
```

配置文件：

```xml
<bean id="factory" class="com.dzx.exmple.spring.Factory"></bean>
<bean id="stu" factory-bean="factory" factory-method="getStudent">
      <property name="id" value="1"></property>
      <property name="name" value="张三"></property>
</bean>
```



### 手动装载

- 在 XML 中手动为 bean 配置<property> 、<constructor-arg>等标签，完成初始化

- 通过注解的方式

1）@Component

可以使用此注解描述 Spring 中的 Bean，但它是一个泛化的概念，仅仅表示一个组件（Bean），并且可以作用在任何层次。使用时只需将该注解标注在相应类上即可。

2）@Repository

用于将数据访问层（DAO层）的类标识为 Spring 中的 Bean，其功能与 @Component 相同。

3）@Service

通常作用在业务层（Service 层），用于将业务层的类标识为 Spring 中的 Bean，其功能与 @Component 相同。

4）@Controller

通常作用在控制层（如 Struts2 的 Action），用于将控制层的类标识为 Spring 中的 Bean，其功能与 @Component 相同。

5）@Autowired

用于对 Bean 的属性变量、属性的 Set 方法及构造函数进行标注，配合对应的注解处理器完成 Bean 的自动配置工作。默认按照 Bean 的类型进行装配。

6）@Resource

其作用与 Autowired 一样。其区别在于 @Autowired 默认按照 Bean 类型装配，而 @Resource 默认按照 Bean 实例名称进行装配。

@Resource 中有两个重要属性：name 和 type。

Spring 将 name 属性解析为 Bean 实例名称，type 属性解析为 Bean 实例类型。如果指定 name 属性，则按实例名称进行装配；如果指定 type 属性，则按 Bean 类型进行装配。如果都不指定，则先按 Bean 实例名称装配，如果不能匹配，则再按照 Bean 类型进行装配；如果都无法匹配，则抛出 NoSuchBeanDefinitionException 异常。

7）@Qualifier

与 @Autowired 注解配合使用，会将默认的按 Bean 类型装配修改为按 Bean 的实例名称装配，Bean 的实例名称由 @Qualifier 注解的参数指定。

XML 文件：只需要这一句

```XML
 <!--使用context命名空间，通知spring扫描指定目录，进行注解的解析-->
  <context:component-scan base-package="com.mengma.annotation"/>
```





### IoC 自动装载（Autowire）

通过 XML 文件的方式

IoC负责创建对象，DI 负责对象的依赖注入，通过配置 <property> 标签的 ref 属性完成，而**自动装载不需要手动配置<property>，IoC 会自动选择 bean 完成注入**，通过 **autowire** 属性实现

自动装载的两种方式：

- byName：通过属性**名**自动装载
- byType：通过属性的**数据类型**自动装载

**注意：被装载的对象需要有相对应的 setter 方法**

> byName

IoC 在配置文件中查找**与属性名相同**的 bean（id），查找成功后自动装载

```java
@Data//自动创建 set/get方法等
public class Student {
    private Integer id;
    private String name;
    private Address address;
}
```

```xml
<!-- id与属性名相同 -->
<bean id="address" class="com.dzx.exmple.spring.Address">
      <property name="content" value="湖南工业大学"></property>
</bean>

<!-- id与属性名不同 -->
<bean id="address2" class="com.dzx.exmple.spring.Address">
      <property name="content" value="湖南工业大学222222"></property>
</bean>

<!-- 自动装载 第一个bean -->
<bean id="stu" class="com.dzx.exmple.spring.Student" autowire="byName">
      <property name="id" value="1"></property>
      <property name="name" value="张三"></property>
</bean>
```



> byType

IoC 在配置文件中查找**与属性的数据类型一致**的 bean（class），查找到一个 bean 后自动装载，但是如果满足要求的bean 有**2个或多个**，系统会抛出异常

```XML
<!--class与属性数据类型一致 -->
<bean id="address2" class="com.dzx.exmple.spring.Address">
      <property name="content" value="湖南工业大学222222"></property>
</bean>
<bean id="stu" class="com.dzx.exmple.spring.Student" autowire="byType">
      <property name="id" value="1"></property>
      <property name="name" value="张三"></property>
</bean>
```



------



## AOP

Aspect Oriented Programming  面向切面编程：将不同方法中的**同一个位置**抽象成一个切面对象，对该切面对象进行编程

**(将非业务代码放进切面类中)**

优点：

1.降低模块之间的耦合度

2.非业务代码更加集中，便于管理

3.业务代码更加简介存粹

4.更好的代码复用



实现方式：

1. JDK 动态代理
2. CGLIB 代理
3. 基于代理类的AOP实现
4. AspectJ开发



### jdk 动态代理

创建**业务接口**、目标类、切面类、代理类（实现 InvocationHandler 接口，重写 invoke（）方法），将**初始**目标对象传给代理对象的方法，代理对象返回满足需求的**新的**目标对象，使用新的目标对象去执行业务

```java
//业务接口
public interface UserDao {
    public void addUser();
    public void deleteUser();
}

//目标类：完成业务工作（实现业务接口）
public class UserImpl implements UserDao{
    @Override
    public void addUser() {
        //非业务代码
        System.out.println("执行操作，添加用户");			//业务代码
        //非业务代码
    }

    @Override
    public void deleteUser() {
        //非业务代码
        System.out.println("执行操作，删除用户");			//业务代码
        //非业务代码
    }
}

//切面类：完成非业务工作
public class MyAspect {
    public void check(){
        System.out.println("权限检查...");
    }

    public void log(){
        System.out.println("日志记录...");
    }
}

//代理类：进行整合，将业务代码与非业务代码结合
public class JdkProxy implements InvocationHandler {
    //声明目标类业务接口
    private UserDao userDao;
    //创建代理对象
    public Object creatProxy(UserDao userDao){
        this.userDao=userDao;
        //类加载器
        ClassLoader classLoader=JdkProxy.class.getClassLoader();
        //被代理对象实现的所有业务接口
        Class[] clazz = userDao.getClass().getInterfaces();
        //使用代理类完成非业务工作，返回代理对象
        return Proxy.newProxyInstance(classLoader,clazz,this);
    }
    
    /*
    * 所有动态代理类的方法调用，都会交给 invoke()方法去处理
    * proxy 委托人（被代理对象）
    * method 将要被执行的方法信息（反射）
    * args 执行方法时需要的参数
    * */
    @Override
    public Object invoke(Object proxy, Method method, Object[] args) throws Throwable {
        //声明切面对象
        MyAspect myAspect = new MyAspect();
        //完成非业务工作
        myAspect.check();
        //完成业务工作,在目标类上调用方法，并传入参数
        Object obj = method.invoke(userDao,args);
        //完成非业务工作
        myAspect.log();
        return obj;
    }
}

//测试类
public class Test {
    public static void main(String[] args) {
        //创建代理对象
        JdkProxy jdkProxy = new JdkProxy();
        //创建目标对象（委托人）
        UserDao userDao = new UserImpl();
        //从代理类中获取增强后的目标对象（目标物）
        UserDao userDao1 = (UserDao)jdkProxy.creatProxy(userDao);
        //执行业务
        userDao1.addUser();
        userDao1.deleteUser();
    }
}
```

目标对象找到代理，告诉代理自己的需求（不想执行非业务代码），最终代理根据需求返回一个全新的 ”目标对象“（通过切面对象来完成非业务代码，目标对象只需要完成业务代码即可）

局限性：使用动态代理的对象（目标类）必须实现一个或多个接口



### CGlib  代理

对指定目标类生成一个子类，并对子类进行增强

不需要实现接口，可直接创建目标类、切面类、代理类（实现 MethodInterceptor 接口，重写 intercept（）方法），将**初始**目标对象传给代理对象的方法，代理对象返回满足需求的**新的**目标对象，使用新的目标对象去执行业务

```java
//目标类
public class UserDao {
    public void addUser(){
        System.out.println("执行业务，增加用户");
    }
    public void deleteUser(){
        System.out.println("执行业务，删除用户");
    }
}

//切面类：完成非业务工作
public class MyAspect {
    public void check(){
        System.out.println("权限检查...");
    }

    public void log(){
        System.out.println("日志记录...");
    }
}

//代理类
public class CglibProxy implements MethodInterceptor {
    //代理方法
    public Object creatProxy(Object target){
        //创建一个动态类对象
        Enhancer enhancer = new Enhancer();
        //确定目标类，设置其为父类，动态类为子类
        enhancer.setSuperclass(target.getClass());
        //添加回调函数
        enhancer.setCallback(this);
        //返回创建的代理类
        return enhancer.create();
    }
    /*
    * proxy        CGlib 根据指定父类生成的代理对象
    * method       拦截的方法
    * args         拦截方法的参数列表
    * methodProxy  方法的代理对象,用于执行父类的方法
    * */
    @Override
    public Object intercept(Object proxy, Method method, Object[] objects, MethodProxy methodProxy) throws Throwable {
        //创建切面对象
        MyAspect myAspect = new MyAspect();
        //完成非业务工作
        myAspect.check();
        //目标方法执行，完成业务工作
        Object obj = methodProxy.invokeSuper(proxy,objects);
        //完成非业务工作
        myAspect.log();
        return obj;
    }
}

//测试类
public class Test {
    public static void main(String[] args) {
        //创建代理对象
        CglibProxy cglibProxy = new CglibProxy();
        //创建初始目标对象
        UserDao userDao = new UserDao();
        //获取增强后的目标对象
        UserDao userDao1 = (UserDao) cglibProxy.creatProxy(userDao);
        userDao1.addUser();
        userDao1.deleteUser();
    }
}
```

Jdk 动态代理与 CGlib 代理的区别：

|                  | Cglib                                                        | JDK                                                          |
| ---------------- | ------------------------------------------------------------ | ------------------------------------------------------------ |
| 是否提供子类代理 | 是                                                           | 否                                                           |
| 是否提供接口代理 | 是（可强制）                                                 | 是                                                           |
| 区别             | 必须依赖于CGLib的类库，但是它需要类来实现任何接口代理的是指定的类生成一个子类，覆盖其中的方法 | 实现InvocationHandler  使用Proxy.newProxyInstance产生代理对象，被代理的对象必须要实现接口 |





### 基于代理类的AOP实现

在配置文件中创建目标对象，切面对象，再通过 **ProxyFactoryBean** 创建一个代理对象，并设置其属性，最后再获取增强后的 bean ，执行业务

```java
public interface UserDao {
    public void addUser();
    public void deleteUser();
}

//目标类：完成业务工作
public class UserImpl implements UserDao{
    @Override
    public void addUser() {
        System.out.println("执行操作，添加用户");
    }

    @Override
    public void deleteUser() {
        System.out.println("执行操作，删除用户");
    }
}
```

```java
package com.dzx.exmple.aopbase;

import org.aopalliance.intercept.MethodInterceptor;
import org.aopalliance.intercept.MethodInvocation;

//切面类
public class MyAspect implements MethodInterceptor {
    @Override
    public Object invoke(MethodInvocation methodInvocation) throws Throwable {
        check();
        //执行目标方法(业务方法)
        Object obj = methodInvocation.proceed();
        log();
        return obj;
    }

    public void check(){
        System.out.println("AOP权限检查...");
    }

    public void log(){
        System.out.println("AOP日志记录...");
    }
}
```

```java
package com.dzx.exmple.aopbase;

import com.dzx.exmple.aop_jdk.UserDao;
import org.springframework.context.ApplicationContext;
import org.springframework.context.support.ClassPathXmlApplicationContext;

public class Test {
    public static void main(String[] args) {
        ApplicationContext applicationContext = new ClassPathXmlApplicationContext("AOP.xml");
        //从容器中获取增强后的代理对象，并将其转为目标类
        UserDao userDao = (UserDao) applicationContext.getBean("userDaoProxy");
        userDao.addUser();
        userDao.deleteUser();
    }
}
```

配置文件：

```XML
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">

        <!--目标类-->
        <bean id="userDao" class="com.dzx.exmple.aop_jdk.UserImpl"/>
        <!--切面类-->
        <bean id="myAspect" class="com.dzx.exmple.aopbase.MyAspect"/>
        <!--使用Spring代理工厂定义一个名称为userDaoProxy的代理对象-->
        <bean id="userDaoProxy" class="org.springframework.aop.framework.ProxyFactoryBean">
            <!--指定代理对象实现的接口-->
            <property name="proxyInterfaces" value="com.dzx.exmple.aop_jdk.UserDao"/>
            <!--指定目标对象-->
            <property name="target" ref="userDao"/>
            <!--指定切面，植入环绕通知/可修改-->
            <property name="interceptorNames" value="myAspect"/>		//注意是 value
            <!--指定代理方式，true:使用cglib;false(默认):使用jdk动态代理-->
            <property name="proxyTargetClass" value="true"/>
        </bean>
</beans>
```

正常返回通知[After returning advice]：在连接点正常执行完成后执行，如果连接点抛出异常，则不会执行。 

异常返回通知[After throwing advice]：在连接点抛出异常后执行。 
返回通知[After (finally) advice]：在连接点执行完成后执行，不管是正常执行完成，还是抛出异常，都会执行返回通知中的内容。 

环绕通知[Around advice]：环绕通知围绕在连接点前后，比如一个方法调用的前后。这是最强大的通知类型，能在方法调用前后自定义一些操作。

Spring 的通知类型：

- 前置通知[Before advice]：在连接点前面执行，前置通知不会影响连接点的执行，除非此处抛出异常。
- 正常返回通知[After returning advice]：在连接点正常执行完成后执行，如果连接点抛出异常，则不会执行。 
- 异常返回通知[After throwing advice]：在连接点抛出异常后执行。
- 返回通知[After (finally) advice]：在连接点执行完成后执行，不管是正常执行完成，还是抛出异常，都会执行返回通知中的内容。  
- 环绕通知[Around advice]：环绕通知围绕在连接点前后，比如一个方法调用的前后。这是最强大的通知类型，能在方法调用前后自定义一些操作。



### AspectJ 开发（推荐）

由于 Spring 对 AOP 进行了封装，所以会自动根据目标类和切面类创建代理对象，因此重点偏向于对切面类的配置（切入时间、位置等等）我们只需要创建目标类和切面类（添加@Aspect），在切面类中编写方法实现所有的非业务代码

- 基于 XML 

通过配置文件配置目标类、切面类，以及利用 <aop: config> 等标签去完成切面配置  

- 基于注解（推荐）

```java
package com.dzx.exmple.spring;
public interface Cal {
    public int add(int x,int y);
    public int sub(int x,int y);
    public int mul(int x,int y);
    public int div(int x,int y);
}
```

```java
package com.dzx.exmple.spring;
import org.springframework.stereotype.Component;

@Component
public class CalImpl implements Cal{
    @Override
    public int add(int x, int y) {
        return x+y;
    }

    @Override
    public int sub(int x, int y) {
        return x-y;
    }

    @Override
    public int mul(int x, int y) {
        return x*y;
    }

    @Override
    public int div(int x, int y) {
        return x/y;
    }
}
```

```java
package com.dzx.exmple.spring;

import org.aspectj.lang.JoinPoint;
import org.aspectj.lang.annotation.After;
import org.aspectj.lang.annotation.AfterReturning;
import org.aspectj.lang.annotation.Aspect;
import org.aspectj.lang.annotation.Before;
import org.springframework.stereotype.Component;

import java.util.Arrays;

@Aspect
@Component
public class CalAspect {
    @Before(value = "execution(public int com.dzx.exmple.spring.CalImpl.*(..))")
    public void before(JoinPoint joinPoint){
        //获取方法名
        String name = joinPoint.getSignature().getName();
        //获取参数
        String args = Arrays.toString(joinPoint.getArgs());
        System.out.println(name+"方法的参数是："+args);
    }

    @After(value = "execution(public int com.dzx.exmple.spring.CalImpl.*(..))")
    public void after(JoinPoint joinPoint){
        String name = joinPoint.getSignature().getName();
        System.out.println(name+"方法执行完毕");
    }

    @AfterReturning(value = "execution(public int com.dzx.exmple.spring.CalImpl.*(..))",returning = "result")
    public void getResult(JoinPoint joinPoint,Object result){
        String name =joinPoint.getSignature().getName();
        System.out.println(name+"方法的结果是:"+result);
    }

}
```

```java
package com.dzx.exmple.test;

import com.dzx.exmple.spring.Cal;
import org.springframework.context.ApplicationContext;
import org.springframework.context.support.ClassPathXmlApplicationContext;


public class Test {
    public static void main(String[] args) {

          ApplicationContext applicationContext=new ClassPathXmlApplicationContext("applicationContext.xml");
          Cal cal =(Cal)applicationContext.getBean("cal");
          cal.add(1,1);
          cal.sub(2,2);
          cal.mul(3,3);
          cal.div(4,4);
    }
}
```

配置文件：

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:aop="http://www.springframework.org/schema/aop"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:p="http://www.springframework.org/schema/p"
       xmlns:context="http://www.springframework.org/schema/context"
       xsi:schemaLocation="http://www.springframework.org/schema/aop
http://www.springframework.org/schema/aop/spring-aop-4.3.xsd
 http://www.springframework.org/schema/beans
http://www.springframework.org/schema/beans/spring-beans-4.3.xsd
 http://www.springframework.org/schema/context
http://www.springframework.org/schema/context/spring-context-4.3.xsd
">
      <bean id="cal" class="com.dzx.exmple.spring.CalImpl"></bean>
      <!--自动扫描-->
      <context:component-scan base-package="com.dzx.exmple" ></context:component-scan>
      <!--使@Aspect注解生效，为目标类自动生成代理对象-->
      <aop:aspectj-autoproxy></aop:aspectj-autoproxy>
</beans>
```

@Aspect：表示该类是切面类

@Component：将该类的对象注入到 IoC 容器中，交给 IoC 管理

注解：

![1615458745238](C:\Users\86185\AppData\Roaming\Typora\typora-user-images\1615458745238.png)

Execution 切入点表达式

![1615458339738](C:\Users\86185\AppData\Roaming\Typora\typora-user-images\1615458339738.png)



### 总结

前3种方式都需要将目标类交给代理从而返回一个新的目标类（满足需求），最后一种方式则不需要将目标对象交给代理，而是切面对象在需要的时间点、位置插入进来执行非业务工作



