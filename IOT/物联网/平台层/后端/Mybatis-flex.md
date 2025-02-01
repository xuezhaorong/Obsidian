## 介绍
官网链接：[MyBatis-Flex - MyBatis-Flex 官方网站](https://mybatis-flex.com/)
MyBatis-Flex 是一个优雅的 MyBatis 增强框架，它非常轻量、同时拥有极高的性能与灵活性。我们可以轻松的使用 Mybaits-Flex 链接任何数据库，其内置的 QueryWrapper^亮点 帮助我们极大的减少了 SQL 编写的工作的同时，减少出错的可能性。

总而言之，MyBatis-Flex 能够极大地提高我们的开发效率和开发体验，让我们有更多的时间专注于自己的事情。

*  特征[​](https://mybatis-flex.com/zh/intro/what-is-mybatisflex.html#%E7%89%B9%E5%BE%81)

**1、轻量**

> 除了 MyBatis，没有任何第三方依赖轻依赖、没有任何拦截器，其原理是通过 SqlProvider 的方式实现的轻实现。同时，在执行的过程中，没有任何的 Sql 解析（Parse）轻运行。 这带来了几个好处：1、极高的性能；2、极易对代码进行跟踪和调试； 3、更高的把控性。

**2、灵活**

> 支持 Entity 的增删改查、以及分页查询的同时，MyBatis-Flex 提供了 Db + Row^灵活 工具，可以无需实体类对数据库进行增删改查以及分页查询。 与此同时，MyBatis-Flex 内置的 QueryWrapper^灵活 可以轻易的帮助我们实现 **多表查询**、**链接查询**、**子查询** 等等常见的 SQL 场景。

**3、强大**

> 支持任意关系型数据库，还可以通过方言持续扩展，同时支持 **多（复合）主键**、**逻辑删除**、**乐观锁配置**、**数据脱敏**、**数据审计**、 **数据填充** 等等功能。

## 依赖引入
打开`pom.xml`，写入`Mybatis-flex`依赖，`HikariCP`线程池依赖，`mysql`数据库依赖
```xml
<!--mybatis-flex依赖相关-->  
<dependency>  
    <groupId>com.mybatis-flex</groupId>  
    <artifactId>mybatis-flex-spring-boot3-starter</artifactId>  
    <version>1.10.2</version>  
</dependency>  
<!--代码生成器-->  
<dependency>  
    <groupId>com.mybatis-flex</groupId>  
    <artifactId>mybatis-flex-codegen</artifactId>  
    <version>1.10.1</version>  
</dependency>  
<!--APT生成器-->  
<dependency>  
    <groupId>com.mybatis-flex</groupId>  
    <artifactId>mybatis-flex-processor</artifactId>  
    <version>1.10.2</version>  
    <scope>provided</scope>  
</dependency>  
<!--线程池-->  
<dependency>  
    <groupId>com.zaxxer</groupId>  
    <artifactId>HikariCP</artifactId>  
    <version>4.0.3</version>  
</dependency>  
<!--数据库驱动-->  
<dependency>  
    <groupId>com.mysql</groupId>  
    <artifactId>mysql-connector-j</artifactId>  
    <scope>runtime</scope>  
</dependency>
```

## yml文件配置
**url需要写到具体的数据库名**
```yml
# 开发环境  
spring:  
  datasource:  
    driver-class-name: com.mysql.cj.jdbc.Driver  
    url: jdbc:mysql://localhost:3306/test  
    username: root  
    password: 030619
```

## 代码生成器的使用
新建包名`codegen`，新建类`Codegen`，根据网页说明进行修改，运行。
官网配置说明：[MyBatis-Flex 代码生成器 - MyBatis-Flex 官方网站](https://mybatis-flex.com/zh/others/codegen.html)
```java  
package com.example.project.codegen;  
  
import com.mybatisflex.codegen.Generator;  
import com.mybatisflex.codegen.config.ColumnConfig;  
import com.mybatisflex.codegen.config.GlobalConfig;  
import com.zaxxer.hikari.HikariDataSource;  
  
public class Codegen {  
    public static void main(String[] args) {  
        //配置数据源  
        HikariDataSource dataSource = new HikariDataSource();  
        dataSource.setJdbcUrl("jdbc:mysql://localhost:3306/intelligent_agriculture");  
        dataSource.setUsername("root");  
        dataSource.setPassword("030619");  
  
        //创建配置内容，两种风格都可以。  
        GlobalConfig globalConfig = createGlobalConfigUseStyle1();  
        //GlobalConfig globalConfig = createGlobalConfigUseStyle2();  
  
        //通过 datasource 和 globalConfig 创建代码生成器  
        Generator generator = new Generator(dataSource, globalConfig);  
  
        //生成代码  
        generator.generate();  
    }  
  
    public static GlobalConfig createGlobalConfigUseStyle1() {  
        //创建配置内容  
        GlobalConfig globalConfig = new GlobalConfig();  
  
        //设置根包  
        globalConfig.setBasePackage("com.example.project");  
  
        //设置表前缀和只生成哪些表  
        globalConfig.setTablePrefix("tb_");  
        globalConfig.setGenerateTable("device_location");  
  
        //设置生成 entity 并启用 Lombok
        globalConfig.setEntityGenerateEnable(true);  
        globalConfig.setEntityWithLombok(true);  
        //设置项目的JDK版本，项目的JDK为14及以上时建议设置该项，小于14则可以不设置  
        globalConfig.setEntityJdkVersion(17);  
  
        //设置生成 mapper        
        globalConfig.setMapperGenerateEnable(true);  
  
        // 设置生成 service       
        globalConfig.setServiceGenerateEnable(true);  
  
        // 设置生成 serviceImpl        
        globalConfig.setServiceImplGenerateEnable(true);  
  
        // 设置生成 Controller        
        globalConfig.setControllerGenerateEnable(true);  
  
        //可以单独配置某个列  
//        ColumnConfig columnConfig = new ColumnConfig();  
//        columnConfig.setColumnName("tenant_id");  
//        columnConfig.setLarge(true);  
//        columnConfig.setVersion(true);  
//        globalConfig.setColumnConfig("tb_account", columnConfig);  
  
        return globalConfig;  
    }  
  
    public static GlobalConfig createGlobalConfigUseStyle2() {  
        //创建配置内容  
        GlobalConfig globalConfig = new GlobalConfig();  
  
        //设置根包  
        globalConfig.getPackageConfig()  
                .setBasePackage("com.test");  
  
        //设置表前缀和只生成哪些表，setGenerateTable 未配置时，生成所有表  
        globalConfig.getStrategyConfig()  
                .setTablePrefix("tb_")  
                .setGenerateTable("tb_device");  
  
        //设置生成 entity 并启用 Lombok        
        globalConfig.enableEntity()  
                .setWithLombok(true)  
                .setJdkVersion(17);  
  
        //设置生成 mapper        globalConfig.enableMapper();  
        //可以单独配置某个列  
        ColumnConfig columnConfig = new ColumnConfig();  
        columnConfig.setColumnName("tenant_id");  
        columnConfig.setLarge(true);  
        columnConfig.setVersion(true);  
        globalConfig.getStrategyConfig()  
                .setColumnConfig("tb_account", columnConfig);  
  
        return globalConfig;  
    }  
}
```

**注意修改的地方：** 选择对应样式的函数中，修改根包路径和表的设置
```java
//配置数据源  
HikariDataSource dataSource = new HikariDataSource();  
dataSource.setJdbcUrl("jdbc:mysql://localhost:3306/test");  
dataSource.setUsername("root");  
dataSource.setPassword("030619");  

//设置根包  
globalConfig.setBasePackage("com.project.test");  

//设置表前缀和只生成哪些表  
globalConfig.setTablePrefix("tb_");  
globalConfig.setGenerateTable("tb_user", "tb_room");  
```

## 使用
1. 先在主程序加入mapper扫描
```java
@SpringBootApplication  
@MapperScan("com.example.project.mapper")  
public class ProjectApplication {  
  
    public static void main(String[] args) {  
       SpringApplication.run(ProjectApplication.class, args);  
    }  
  
}
```