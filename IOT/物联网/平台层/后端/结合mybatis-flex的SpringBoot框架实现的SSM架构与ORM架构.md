## 介绍
### SSM架构
SSM框架是spring、spring MVC 、和mybatis框架的整合，是标准的MVC模式。标准的SSM框架有四层，分别是`dao`层（`mapper`），`service`层，`controller`层和`View`层。使用spring实现业务对象管理，使用spring MVC负责请求的转发和视图管理，mybatis作为数据对象的持久化引擎。

* 持久层：`dao`层（`mapper`）层：作用：
	主要是做数据持久层的工作，负责与数据库进行联络的一些任务都封装在此。
	1. Dao层首先设计的是接口，然后再Spring的配置文件中定义接口的实现类。
	2. 然后可以在模块中进行接口的调用来进行数据业务的处理。（不在关心接口的实现类是哪个类）
	3. 数据源的配置以及有关数据库连接的参数都在Spring的配置文件中进行配置。

* 业务层：`Service`层：
	作用：Service层主要负责业务模块的逻辑应用设计。
	1. 先设计接口然后再设计实类，然后再在Spring的配置文件中配置其实现的关联。（业务逻辑层的实现具体要调用到自己已经定义好的Dao的接口上）这样就可以在应用中调用Service接口来进行业务处理。
	2. 建立好Dao之后再建立service层，service层又要在controller层之下，因为既要调用Dao层的接口又要提供接口给controller层。每个模型都有一个service接口，每个接口分别封装各自的业务处理的方法。

* 表现层：Controller层（Handler层）：
	作用：负责具体的业务模块流程的控制。
	1. 配置也同样是在Spring的配置文件里面进行。
	2. 调用Service层提供的接口来控制业务流程。
	3. 业务流程的不同会有不同的控制器，在具体的开发中可以将我们的流程进行抽象的归纳，设计出可以重复利用的子单元流程模块。

![image.png|498](https://cdn.jsdelivr.net/gh/xuezhaorong/Picgo//Source/fix-dir/picgo/picgo-clipboard-images/2024/12/01/23-51-06-d5ce82f6ffa19a33fdff3510e60fc5c4-20241201235105-2b6ae5.png)


### ORM
ORM（Object-Relational Mapping） 表示对象关系映射。在面向对象的软件开发中，通过ORM，就可以把对象映射到关系型数据库中。只要有一套程序能够做到建立对象与数据库的关联，操作对象就可以直接操作数据库数据，就可以说这套程序实现了ORM对象关系映射

简单的说：ORM就是建立实体类和数据库表之间的关系，从而达到操作实体类就相当于操作数据库表的目的。

当实现一个应用程序时（不使用O/R Mapping），我们可能会写特别多数据访问层的代码，从数据库保存数据、修改数据、删除数据，而这些代码都是重复的。而使用ORM则会大大减少重复性代码。对象关系映射（Object Relational Mapping，简称ORM），主要实现程序对象到关系数据库数据的映射。

## 基础配置
### 本地开发环境搭建
1. 安装`IDEA`开发工具，官方链接：[jetbrains.com/idea/](https://www.jetbrains.com/idea/)
2. 配置Maven，[[Maven本地仓库配置]]
3. 安装Mysql，官网链接：[MySQL :: Download MySQL Community Server (Archived Versions)](https://downloads.mysql.com/archives/community/)
4. 安装数据库查看工具`Navicat`，官网链接：[Navicat Premium | 以单一的 GUI 同时连接不同类型的数据库](https://www.navicat.com.cn/products/navicat-premium)
5. 安装Api接口调试工具`Apifox`，官网链接：[Apifox - API 文档、调试、Mock、测试一体化协作平台。拥有接口文档管理、接口调试、Mock、自动化测试等功能，接口开发、测试、联调效率，提升 10 倍。最好用的接口文档管理工具，接口自动化测试工具。](https://apifox.com/)

### 创建项目
1. 打开`IDEA`新建SpringBoot项目，选择左侧的`Spring Initializr`，设置好名称，位置，组，软件包名称等，注意选择Maven构建类型和JDK版本。

![|925](https://cdn.jsdelivr.net/gh/xuezhaorong/Picgo//Source/fix-dir/picgo/picgo-clipboard-images/2024/12/02/00-19-28-5f4fc28ba1915a76f137890a76914ddd-20241202001927-8c2c5f.png)


2. 选择SPringBoot版本，~Lombok~注解工具，`Spring Web`框架工具和`MySQL Deiver`驱动库xxuan'zggou'jianggon'juxxaid的
![image.png|625](https://cdn.jsdelivr.net/gh/xuezhaorong/Picgo//Source/fix-dir/picgo/picgo-clipboard-images/2024/12/02/00-21-07-30b690cf38a0fb0550f3602bec204e9f-20241202002107-66185c.png)
![image.png|625](https://cdn.jsdelivr.net/gh/xuezhaorong/Picgo//Source/fix-dir/picgo/picgo-clipboard-images/2024/12/02/00-21-31-a79e8095deb3b0c069384b4feceaa02e-20241202002131-4c1a02.png)

3. 进入项目中，选择构建工具下的Maven，勾选重写，选择路径。

![image.png|850](https://cdn.jsdelivr.net/gh/xuezhaorong/Picgo//Source/fix-dir/picgo/picgo-clipboard-images/2024/12/02/00-23-27-9d2edf617691e979728f463a42070ccc-20241202002326-a07d93.png)

### 写入框架
### mybatis-flex持久层工具
1. 依赖导入

