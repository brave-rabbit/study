# Maven

Maven是一个Java项目管理和构建工具，它可以定义项目结构、项目依赖，并使用统一的方式进行自动化构建，是Java项目不可缺少的工具。

### 核心概念

- 约定的目录结构
- POM
- 坐标
- 依赖管理
- 仓库管理
- 生命周期
- 插件和目标
- 继承
- 集合



##### 约定的目录结构

每一个 maven 项目都是一个文件夹（以 Hello 项目为例）

------

Hello/

​    ---/src						

​    --------/main					#放置用户主程序 java 代码和配置文件

​    ------------------/java			#用户程序各种包以及包中的 java 文件

​    ------------------/resources		#用户 java 程序中要使用的配置文件

​    --------/test					#放置测试代码和文件（可以没有）

​    ------------------/java			#测试程序的包以及包中 java 文件

​    ------------------/resources		#测试程序中所需要的配置文件

​    ---/pom.xml					# maven 的核心文件（必须要有）				

------

maven项目进行编译成功后会自动生成 target 目录（结果目录），java 程序编译后的 class	文件存放在 target 目录中



##### POM

Project Object Model（项目对象模型），pom.xml 是 maven 的核心，在里面进行各种声明	

基本信息（标签）：

- modelVersion：Maven模型的版本，对Maven2、Maven3来说，它只能是4.0.0
- groupId：组织 id，一是（公司域名的倒写），比如 com.baidu，二是（域名倒写+项目名）
- artifactId：项目名称，也是模块名称，对应 groupId 中的项目名
- version：项目的版本号(三位数，eg 1.0.2)，如果还在开发，通常在后面加上 -SNAPSHOT
- packaging：项目打包的类型，默认是 jar ，web 应用是 war（不写是 jar）
- properties：设置属性
- build：进行与构建相关的配置，比如配置各种插件，通过编译插件设置 jdk 版本
- target：告诉 maven 程序应该运行在 *** 的 jdk 上
- source：告诉 maven 我们写的程序是在 ***jdk 上编译的



##### 坐标

唯一值，在互联网中唯一标识一个项目

由 groupId、artifactId、version 三个标签组成



##### 依赖管理

dependencies 和 dependency：项目中要使用各种资源需要声明依赖，相当于 java中的 import

添加依赖后，maven 会在仓库中去寻找



##### 仓库管理

仓库：存放 jar 包

- maven 使用的插件（各种 jar 包）
- 我项目使用的 jar 包（第三方工具）

仓库的分类：

- 本机仓库：个人计算机上的文件夹，存放各种 jar 

- 远程仓库：

  ​		①中央仓库：最权威的，所有开发人员都能共享使用的仓库

  ​		②中央仓库的镜像：中央仓库的备份

  ​		③私服：在局域网中使用，一般是公司内部使用

仓库的使用不需要人为参与，maven 自动管理

使用顺序：本机仓库-->私服-->镜像-->中央仓库

**注意**

本机仓库：C:\Users\86185（每个人不同）\.m2\repository（默认）

修改本机仓库：

在 maven 安装目录/conf/setting.xml 文件中设置<localRepository>标签，设置之前先将 setting.xml 文件备份

```xml
//以（E:\a\1）为例，将斜杆换个方向，目录中不能有中文
<localRepository>E:/a/1</localRepository>
```



##### 生命周期

生命周期就是 maven 构建项目的过程，包括清理、编译、测试、报告、打包、安装、部署（详细见下"构建"）



##### 插件

maven 命令执行时，正真完成功能的是插件，插件就是一些 jar 文件，一些类

plugins 和 plugin 标签

##### 继承

##### 集合





# 使用方式

1.独立使用：使用 maven 的各种命令，完成代码的编译、测试、打包等

2.结合开发工具使用，一般在 IDEA 中使用 maven ，简单、快捷，不需要命令

# 作用

1.管理依赖：jar包的管理、下载、版本

2.构建项目：完成项目代码的编译、测试、打包、部署

2.管理 jar 包的依赖， a.jar 需要 b.jar ，maven 会自动下载 b.jar 

3.编译程序，测试代码，打包文件（形成 jar、war文件）

4.部署项目

# 构建

项目的构建

构建是面向过程的，就是一些步骤，完成项目代码的编译，测试，运行，打包，部署等等

maven 支持的构建包括有：

1.清理：把之前项目编译的东西删除掉，为新的编译代码做准备

2.编译：批量地把源代码编译成执行代码（.java----> .class）

3.测试：测试代码，验证功能是否正确

4.报告：生成测试结果文件，判断测试是否通过

5.打包：将项目中所有的 class 文件、配置文件以及资源放到一个压缩文件中（Java程序为 .jar ，web应用为 .war）

6.安装：将（5）生成的压缩文件安装到本机仓库

7.部署：将程序安装好，以便执行

# Maven常用命令

**！！！使用命令必须进入 pom.xml 文件所在目录！！！**

- mvn clean：删除 target 目录
- mvn compile：编译主程序，只能编译 main/java目录下的 java文件，会在当前目录生成 target 文件
- mvn test-compile：编译测试程序，只能编译 test/java目录下的 java文件，会在当前目录生成 target 文件
- mvn test：测试，会生成一个 surefire-reports 目录来保存测试结果
- mvn packge：打包主程序，生成 jar 包或者 war 包
- mvn install：安装主程序，将本工程保存到本地仓库
- mvn deplay：部署



















