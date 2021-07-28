# Maven----IDEA

IDEA 自带 maven，但一般不使用，我们需要配置自己的 maven

# Maven 配置

#### 设置当前工程

1.进入 Settings

![1613617644328](C:\Users\86185\AppData\Roaming\Typora\typora-user-images\1613617644328.png)



2.进行相关配置：

①：设置Maven home path （maven 安装目录）

②：设置User settings file（settings.xml 文件）

③：设置Local repository（本机仓库）

![1613617693264](C:\Users\86185\AppData\Roaming\Typora\typora-user-images\1613617693264.png)



3.展开Maven，进入Runner

①：设置VM Options（输入 **-DarchetypeCatalog=internal**）

②：设置JRE

![1613617902418](C:\Users\86185\AppData\Roaming\Typora\typora-user-images\1613617902418.png)



#### 设置新建工程（在 ALL settings 中设置，其他的与上述一致）

![1613620361066](C:\Users\86185\AppData\Roaming\Typora\typora-user-images\1613620361066.png)





# Maven 使用

1.需要使用某个包时，先**添加依赖**（借助中央仓库）

2.系统回去本机仓库找，如果找到，刷新或者重启项目一下就可以；如果没找到，则会自动下载



**点击这个进行刷新**

### ![1613621384813](C:\Users\86185\AppData\Roaming\Typora\typora-user-images\1613621384813.png)



# 依赖范围

<scope></scope>

![1613633346891](C:\Users\86185\AppData\Roaming\Typora\typora-user-images\1613633346891.png)





