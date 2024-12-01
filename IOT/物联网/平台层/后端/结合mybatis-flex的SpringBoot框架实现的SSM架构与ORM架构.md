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

## 本地回环测试

### 数据库初始化
1. 打开`cmd`输入`mysql -u root -p`，然后输入密码初始化mysql，使用`Navicat`连接， 输入初始化的用户名密码。

![image.png|900](https://cdn.jsdelivr.net/gh/xuezhaorong/Picgo//Source/fix-dir/picgo/picgo-clipboard-images/2024/12/02/00-42-02-cf3ac61696827234f741f40210c890a0-20241202004200-82ed58.png)

2. 创建数据库
![image.png|457](https://cdn.jsdelivr.net/gh/xuezhaorong/Picgo//Source/fix-dir/picgo/picgo-clipboard-images/2024/12/02/00-43-06-86c287851abb6a003328c0b4715aae25-20241202004306-1a3bcf.png)

3. 双击进入创建的数据库，使用查询创建数据表

```sql
CREATE TABLE IF NOT EXISTS `tb_user`
(
    `id`        INTEGER PRIMARY KEY auto_increment,
    `username` VARCHAR(100),
    `age`       INTEGER,
);
```

![image.png|825](https://cdn.jsdelivr.net/gh/xuezhaorong/Picgo//Source/fix-dir/picgo/picgo-clipboard-images/2024/12/02/00-44-32-8ee79c470850f221a2619ab208a2f980-20241202004432-f64c5c.png)

![image.png|825](https://cdn.jsdelivr.net/gh/xuezhaorong/Picgo//Source/fix-dir/picgo/picgo-clipboard-images/2024/12/02/00-48-55-2f6610dd03ac38a2edfe2d337e44f398-20241202004855-b7d377.png)


### mybatis-flex持久层工具的导入
依赖导入：[[Mybatis-flex#依赖引入]]，`yml`配置：[[Mybatis-flex#yml文件配置]]，将`property`文件改为`yml`
使用代码生成器：[[Mybatis-flex#代码生成器的使用]]，生成对应的`entity`，`mapper`,`service`，`serviceImpl`和`controler`

![image.png|350](https://cdn.jsdelivr.net/gh/xuezhaorong/Picgo//Source/fix-dir/picgo/picgo-clipboard-images/2024/12/02/01-00-20-ee4d2168dbb6eee867cb18601391d5e5-20241202010018-35e1d7.png)

**entity**
```java
@Data  
@NoArgsConstructor  
@AllArgsConstructor  
@Builder  
@Table("tb_room")  
public class Room implements Serializable {  
    @Id(keyType=KeyType.Generator, value= KeyGenerators.flexId)  
    @Column("ID")  
    private Integer ID;  
  
    @Column("roomID")  
    private Integer roomID;  
  
    @Column("macID")  
    private String macID;  
  
}
```

**mapper**
```java
@Mapper  
public interface RoomMapper extends BaseMapper<Room> {  
  
}
```

`service`
```java
public interface RoomService extends IService<Room> {  

}
```

`serviceImpl`
```java
@Service  
public class RoomServiceImpl extends ServiceImpl<RoomMapper, Room>  implements RoomService{  
  
}
```

`RoomController`
```java
@RestController  
public class RoomController {  
  
    /**  
     * 根据主键更新。  
     *  
     * @param room   
* @return {@code true} 更新成功，{@code false} 更新失败  
     */  
    @PutMapping("update")  
    public boolean update(@RequestBody Room room) {  
        return roomService.updateById(room);  
    }  
  
    /**  
     * 查询所有。  
     *  
     * @return 所有数据  
     */  
    @GetMapping("list")  
    public List<Room> list() {  
        return roomService.list();  
    }  
  
    /**  
     * 根据主键获取详细信息。  
     *  
     * @param id 主键  
     * @return 详情  
     */  
    @GetMapping("getInfo/{id}")  
    public Room getInfo(@PathVariable Integer id) {  
        return roomService.getById(id);  
    }  
  
    /**  
     * 分页查询。  
     *  
     * @param page 分页对象  
     * @return 分页对象  
     */  
    @GetMapping("page")  
    public Page<Room> page(Page<Room> page) {  
        return roomService.page(page);  
    }  
  
}
```

### 配置yml
默认为`8080`端口，如果占用了，可以选择改变端口
```java
server:  
  port: 8041	
```

### 实现功能
实现发送API：`/room/roomList` 查询所有房间列表
1. 编写`Controller`
```java
@RestController  
@RequestMapping("/room")  
public class RoomController {  
  
    @Autowired  
    private RoomService roomService;  
  
    // 获取房间信息  
    @GetMapping("/roomList")  
    public Result<List<Room>> getRoomList(){  
        List<Room> roomList = roomService.findAll();  
        if (roomList != null){  
            return Result.success(roomList);  
        }  
        return Result.error("查找房间信息失败");  
    }  
  
```

添加了顶层API：`/room`，使用`@Autowired`注入`roomService`，添加Get方法的请求函数

2. 编写`service`
在`RoomService`中添加功能接口
```java
public interface RoomService extends IService<Room> {  
  
    public List<Room> findAll();  
}
```

在`RoomServiceImpl`中实现接口
```java
@Service  
public class RoomServiceImpl extends ServiceImpl<RoomMapper, Room>  implements RoomService{  
  
    @Autowired  
    private RoomMapper roomMapper;  
  
    @Override  
    public List<Room> findAll() {  
  
        return roomMapper.selectAll();  
    }  
}
```

使用`@Autowired`注入`roomMapper`，调用`roomMapper`的`selectAll()`方法，`selectAll()`是`roomMapper`继承的`BaseMapper`类所自带的方法。

### API测试
1. 打开`Apifox`，新建测试环境，根据具体端口号设置
![image.png|950](https://cdn.jsdelivr.net/gh/xuezhaorong/Picgo//Source/fix-dir/picgo/picgo-clipboard-images/2024/12/02/01-23-28-e5c9f66efcd1f81ffb8934cf5ae4d5b5-20241202012327-bd3cf1.png)

![image.png|950](https://cdn.jsdelivr.net/gh/xuezhaorong/Picgo//Source/fix-dir/picgo/picgo-clipboard-images/2024/12/02/01-23-43-32ad860328ba11cd498425bfa7bf2d72-20241202012343-774a29.png)

2. 新建测试项目目录，新建一个接口，切换为`GET`方法，写入`API`路径
![image.png|1025](https://cdn.jsdelivr.net/gh/xuezhaorong/Picgo//Source/fix-dir/picgo/picgo-clipboard-images/2024/12/02/01-24-43-2b4131264af6c49094a566549409ad16-20241202012443-79a20d.png)

先运行SpringBoot后端程序，再点击运行-发送，查看接收的信息, 测试成功
![image.png|1050](https://cdn.jsdelivr.net/gh/xuezhaorong/Picgo//Source/fix-dir/picgo/picgo-clipboard-images/2024/12/02/01-26-47-570eca2d18040baeb98c1785b910e85f-20241202012646-2c5667.png)
