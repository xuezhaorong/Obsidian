## 下载安装
1. 下载`Meven`构建工具，下载链接：https://maven.apache.org/download.cgi
![image.png|950](https://cdn.jsdelivr.net/gh/xuezhaorong/Picgo//Source/fix-dir/picgo/picgo-clipboard-images/2024/12/01/23-57-44-259ca0902482fc5474125b062a068ab6-20241201235743-1370c0.png)

2. 解压到指定目录然后添加到环境变量
**新增系统变量 `MAVEN_HOME`**,变量值为解压路径
![image.png|950](https://cdn.jsdelivr.net/gh/xuezhaorong/Picgo//Source/fix-dir/picgo/picgo-clipboard-images/2024/12/01/23-58-31-6ed45c6fb3676c03887dd0ed8dcb0ec1-20241201235830-2dd359.png)

3. **编辑变量Path，添加变量值`%MAVEN_HOME%\bin`**
![image.png|1000](https://cdn.jsdelivr.net/gh/xuezhaorong/Picgo//Source/fix-dir/picgo/picgo-clipboard-images/2024/12/01/23-59-49-6770b1b3cc059f2070a6c6880da29e97-20241201235948-314f20.png)

4. 验证安装是否成功
打开命令提示符窗口，输入`mvn -v`
![image.png|1000](https://cdn.jsdelivr.net/gh/xuezhaorong/Picgo//Source/fix-dir/picgo/picgo-clipboard-images/2024/12/02/00-00-26-a7418943f8de44f2880fea77d542cd1d-20241202000025-2d566a.png)

## 配置本地仓库
1. 新建`maven-repository`文件夹，用作`maven`的本地库
![image.png|1000](https://cdn.jsdelivr.net/gh/xuezhaorong/Picgo//Source/fix-dir/picgo/picgo-clipboard-images/2024/12/02/00-01-08-c1a1c44cd219452eaf1c443cdfa1bd4f-20241202000107-ac2aca.png)

2. 在路径`Maven`解压路径下找到`settings.xml`文件
![image.png|1000](https://cdn.jsdelivr.net/gh/xuezhaorong/Picgo//Source/fix-dir/picgo/picgo-clipboard-images/2024/12/02/00-01-48-8706d043a5bb8379e9a32095ab0a683c-20241202000148-6c18b0.png)

3. 打开settings.xml文件，找到节点localRepository，在注释外添加配置文件maven-repository路径
```bash
<localRepository>E:\Java\Maven\maven-repository</localRepository>
```
![image.png|850](https://cdn.jsdelivr.net/gh/xuezhaorong/Picgo//Source/fix-dir/picgo/picgo-clipboard-images/2024/12/02/00-02-29-ab6e9abb33703e37baaeb2383afd6891-20241202000228-302dd8.png)
localRepository节点用于配置本地仓库，本地仓库其实起到了一个缓存的作用，它的默认地址是 `C:\Users\用户名.m2`。当我们从maven中获取jar包的时候，maven首先会在本地仓库中查找，如果本地仓库有则返回；如果没有则从远程仓库中获取包，并在本地库中保存。此外，我们在maven项目中运行mvn install，项目将会自动打包并安装到本地仓库中。

## 配置镜像源
1. 在`settings.xml`配置文件中找到`mirrors`节点
![image.png|650](https://cdn.jsdelivr.net/gh/xuezhaorong/Picgo//Source/fix-dir/picgo/picgo-clipboard-images/2024/12/02/00-03-19-969f82388db5ba9e8d9c180d2998944d-20241202000318-efbc98.png)

添加mirror标签，配置内容如下
```bash
<mirror>
	<id>alimaven</id>
	<mirrorOf>central</mirrorOf>
	<name>aliyun maven</name>
	<url>http://maven.aliyun.com/nexus/content/repositories/central/</url>
</mirror>
```
![image.png|825](https://cdn.jsdelivr.net/gh/xuezhaorong/Picgo//Source/fix-dir/picgo/picgo-clipboard-images/2024/12/02/00-03-49-18a712a72349a32d72bc5cc8b6c5cfe7-20241202000349-d99e26.png)

## 配置系统JDK
1. 在`settings.xml`配置文件中找到profiles节点
添加如下配置，其中jdk版本要根据系统的实际版本，这里为`jdk-11`
```bash
<profile>
	  <id>jdk-11</id>
	  <activation>
		<activeByDefault>true</activeByDefault>
		<jdk>11</jdk>
	  </activation>

	  <properties>
		<maven.compiler.source>11</maven.compiler.source>
		<maven.compiler.target>11</maven.compiler.target>
		<maven.compiler.compilerVersion>11</maven.compiler.compilerVersion>
	  </properties>
</profile>

```

![image.png](https://cdn.jsdelivr.net/gh/xuezhaorong/Picgo//Source/fix-dir/picgo/picgo-clipboard-images/2024/12/04/00-53-55-f01ede09a3c30aff0fd3d8cd6fa496c6-20241204005354-5248c5.png)

2. 验证配置结果
打开命令提示符窗口，输入`mvn help:system`测试，配置成功则本地仓库（`E:\Tools\Maven\maven-repository`）中会出现一些文件

![image.png|1075](https://cdn.jsdelivr.net/gh/xuezhaorong/Picgo//Source/fix-dir/picgo/picgo-clipboard-images/2024/12/02/00-08-03-8d7f544a0736227ca6fc3bef1c206c22-20241202000803-7e1ee8.png)
