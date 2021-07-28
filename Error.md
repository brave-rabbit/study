# 常见错误以及解决方法



- **Class.forName("com.mysql.jdbc.Driver")**		

背景：java项目可以连接数据库，但是javaweb项目不可以

原因：jre有jar包，可以找到，但Tomcat找不到驱动，需要进行配置

解决方案：Tomcat配置mysql池，将jar包拷贝到 Web-INF/lib 目录下，并在工程中Libraries项添加 jar 包

![1612453313003](C:\Users\86185\AppData\Roaming\Typora\typora-user-images\1612453313003.png)

![1612453281547](C:\Users\86185\AppData\Roaming\Typora\typora-user-images\1612453281547.png)



- **A ResourcePool could not acquire a resource from its primary factory or source**

背景：连接数据库报错

原因：

1.驱动配置有误																								2.数据库连接地址有误																					3.密码或帐号有误																								4.数据库未启动或无权访问																								5.项目未引入对应的驱动jar包mysql-connector-java-5.1.6-bin.jar																				6.mysql root没有远程访问的权限，需要增加权限，增加权限的步骤如下：进入mysql数据库：grant all privileges on *.* to 'root'@'%' identified by 'root' with grant option;flush privileges;

解决方法：核对用户名、密码、URL、Driver等是否完全正确，并且检查数据库实例是否正确



- **No suitable driver**

背景：使用 c3p0 连接数据库，并采用 c3p0-config.xml 文件进行配置报错

原因：														解决方法：

1.JRE(运行环境)中没有 mysql 驱动								--再 Libraries 中添加相关驱动jar 包

2.lib文件目录下没有 mysql 驱动									--将 jar 包复制粘贴到 lib 文件目录下

3.URL错误													--检查URL、Drive格式

4.找不到 c3p0-config.xml 文件									--检查名字是否正确，并将其放在正确的位置

![1612795650391](C:\Users\86185\AppData\Roaming\Typora\typora-user-images\1612795650391.png)

![1612795455402](C:\Users\86185\AppData\Roaming\Typora\typora-user-images\1612795455402.png)

（一般情况是放在 src 目录下，使用IDEA必须将其放在 resources 文件下（WEB工程））



- ### java.lang.classnotfoundexception

  ![1613728525222](C:\Users\86185\AppData\Roaming\Typora\typora-user-images\1613728525222.png)

还要注意环境变量、jar 包依赖等因素







