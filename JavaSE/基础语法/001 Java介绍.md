# Java

- 跨平台
- 面向对象
- 分布式计算

# Java运行机制

- 编写 java 程序
- 编译 java 文件
- JVM 读取字节码文件运行程序

# Java三大体系

- Java SE(J2SE)
- Java ME(J2ME)
- Java EE(J2EE)

# JDK、JRE和JVM之间的关系



- **JDK（java development kit）**
  JDK是Java开发工具包，是整个Java的核心，包括了Java运行环境JRE、Java工具和Java基础类库。
- **JRE（ java runtime environment）**
  JRE是java的运行环境，包含JVM标准实现及Java核心类库。
- **JVM（java virtual machine）**
  JVM是java虚拟机，是整个java实现跨平台的最核心的部分，能够运行以Java语言写作的软件程序。



JDK = JRE + 开发工具

JRE = JVM + 类库

![1610100975293](C:\Users\86185\AppData\Roaming\Typora\typora-user-images\1610100975293.png)



# JDK安装目录结构解析

![1610101040076](C:\Users\86185\AppData\Roaming\Typora\typora-user-images\1610101040076.png)

# Java程序的开发过程为：

- 我们利用 JDK （调用 Java API）编写出 Java 源代码，存储于 `.java` 文件中
- JDK 中的编译器 javac 将 Java 源代码编译成 Java 字节码，存储于 `.class` 文件中（十六进制）
- JRE （JVM+类库）加载、验证、执行 Java 字节码；其中，JVM 将字节码解析为机器码（二进制）并映射到 CPU 指令集或 OS 的系统调用

![1613726072108](C:\Users\86185\AppData\Roaming\Typora\typora-user-images\1613726072108.png)

![1610101977963](C:\Users\86185\AppData\Roaming\Typora\typora-user-images\1610101977963.png)

# 注意

- 在 DOS 中使用 **javac 文件名(类名).java** 进行编译，再使用 **java 文件名(类名)** 运行字节码文件（**不用加后缀了**）

![1613730251642](C:\Users\86185\AppData\Roaming\Typora\typora-user-images\1613730251642.png)

- 字节码文件（.class）是以**十六进制**进行存储的

- 查看字节码文件中的 JVM 指令

  需要将字节码中的 JVM 指令转换成我们能够看懂的命令

  ```DOS
  javap -c 字节码文件名(不用加后缀)
  ```



