> 官网链接：https://docs.spring.io/spring-data/redis/reference/redis/getting-started.html
> 客户端工具`Another Redis Desktop Manager`链接：https://github.com/qishibo/AnotherRedisDesktopManager

## 安装
### Window安装
下载链接：https://github.com/tporadowski/redis/releases/tag/v5.0.14.1
![image.png](https://cdn.jsdelivr.net/gh/xuezhaorong/Picgo//Source/fix-dir/picgo/picgo-clipboard-images/2025/03/14/23-11-36-eaa59a980a5209d21fbd6dd68406f422-20250314231135-49370b.png)

下载后解压，cmd切换到解压目录后输入命令`.\redis-server.exe .\redis.windows.conf`后运行redis
![image.png](https://cdn.jsdelivr.net/gh/xuezhaorong/Picgo//Source/fix-dir/picgo/picgo-clipboard-images/2025/03/14/23-13-31-40e08f1fe7669f73cee60f3e49d8e02e-20250314231329-975f3f.png)
使用客户端工具连接
## 导入依赖
使用`Lettuce` 连接池
```xml
<!-- spring data redis -->  
<dependency>  
    <groupId>org.springframework.boot</groupId>  
    <artifactId>spring-boot-starter-data-redis</artifactId>  
</dependency>  
  
<dependency>  
    <groupId>org.apache.commons</groupId>  
    <artifactId>commons-pool2</artifactId>  
</dependency>
```

## yml配置
```yml
spring:
	data:  
	  redis:  
		host: 127.0.0.1  
		port: 6379  
		timeout: 1800000  
		lettuce:  
		  pool:  
			max-active: 20  # 连接池最大连接数（使用负值表示没有限制）  
			max-wait: -1  # 最大阻塞等待时间（负数表示没有限制）  
			max-idle: 5  # 连接池中最大空闲连接  
			min-idle: 0  # 连接池中最小空闲连接
```

## 配置类
在`config`下新建`RedisConfig`配置类
```java
@EnableCaching  // 开启缓存  
@Configuration  
public class RedisConfig {  
  
    @Bean  
    public LettuceConnectionFactory redisConnectionFactory() {  
        // 配置 Redis 服务器的连接信息  
        RedisStandaloneConfiguration redisStandaloneConfiguration = new RedisStandaloneConfiguration();  
        redisStandaloneConfiguration.setHostName("localhost");  
        redisStandaloneConfiguration.setPort(6379);  
        // redisStandaloneConfiguration.setPassword("password"); // 取消注释以设置密码  
  
        // 配置连接池  
        GenericObjectPoolConfig<Object> poolConfig = new GenericObjectPoolConfig<>();  
        poolConfig.setMaxTotal(10);       // 连接池中的最大连接数  
        poolConfig.setMaxIdle(5);         // 连接池中的最大空闲连接数  
        poolConfig.setMinIdle(1);         // 连接池中的最小空闲连接数  
  
        // 创建一个带有连接池配置的 Lettuce 客户端配置  
        LettucePoolingClientConfiguration lettucePoolingClientConfiguration =  
                LettucePoolingClientConfiguration.builder()  
                        .poolConfig(poolConfig)  
                        .build();  
  
        // 返回带有连接池配置的 Redis 连接工厂  
        return new LettuceConnectionFactory(redisStandaloneConfiguration, lettucePoolingClientConfiguration);  
    }  
  
    @Bean  
    public RedisTemplate<String, Object> redisTemplate() {  
        /*  
            1.创建 RedisTemplate: 这是 Spring 用于与 Redis 交互的核心类，简化了与 Redis 的交互。  
            2.设置连接工厂: 使用前面定义的 LettuceConnectionFactory。  
            3.设置序列化器: 设置键和值的序列化器，这里使用 StringRedisSerializer 来将键和值序列化为字符串。  
         */        RedisTemplate<String, Object> template = new RedisTemplate<>();  
        template.setConnectionFactory(redisConnectionFactory());  // 设置连接工厂  
        template.setKeySerializer(new StringRedisSerializer());  // 设置键的序列化器  
        template.setValueSerializer(new StringRedisSerializer()); // 设置值的序列化器  
        return template;  
    }  
}
```

## 使用
### String
```java
@Autowired  
RedisTemplate<String,Object> redisTemplate;  
  
@Test  
public void testKeyBoundOperations(){  
    // String 类型  
    BoundValueOperations<String, Object> username = redisTemplate.boundValueOps("username");  
    // 10秒过期  
    username.set("xue",10, TimeUnit.SECONDS);  
    // 设置过期  
	username.expire(10, TimeUnit.SECONDS);
}
```