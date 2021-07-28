# 配置文件

**使用配置文件进行数据输入，在对应的类上需要配置前缀**

```java
@Component
@ConfigurationProperties(prefix = "car")
```



**方式**

- ## properties

同以前的properties用法，**优先级高于yaml**

- ## yaml

### 基本语法

- key: value；**kv之间有空格**
- 大小写敏感
- 使用缩进表示层级关系
- 缩进不允许使用tab，只允许空格（IDEA可以）
- 缩进的空格数不重要，只要相同层级的元素左对齐即可
- '#'表示注释
- 字符串无需加引号，如果要加，''与""表示字符串内容 会被 转义/不转义

(以控制台输出为例，单引号回原样输出 \n，而双引号会进行换行)



### 例子

- 字面量：单个的、不可再分的值。date、boolean、string、number、null

```
k: v
```

- 对象：键值对的集合。map、hash、set、object 

```
行内写法：  k: {k1:v1,k2:v2,k3:v3}
#或
k: 
  k1: v1
  k2: v2
  k3: v3
```

- 数组：一组按次序排列的值。array、list、queue

```
行内写法：  k: [v1,v2,v3]
#或者
k:
 - v1
 - v2
 - v3
```



### 设置配置提示

```xml
<dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-configuration-processor</artifactId>
            <optional>true</optional>
</dependency>


<plugin>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-maven-plugin</artifactId>
    <configuration>
        <excludes>
            <exclude>
                <groupId>org.springframework.boot</groupId>
                <artifactId>spring-boot-configuration-processor</artifactId>
            </exclude>
        </excludes>
    </configuration>
</plugin>
```

