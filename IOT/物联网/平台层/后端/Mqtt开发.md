## 本地开发环境
本地开发需要本地的mqtt服务器保证编译的完成，选用`emqx`
### 配置emqx
官网链接：[免费试用 EMQX Cloud 或 EMQX Enterprise | 下载 EMQX](https://www.emqx.com/zh/try?tab=self-managed)
下载安装开源版：[Releases · emqx/emqx](https://github.com/emqx/emqx/releases?page=5)
注意：新版本没有更新window版本，选择旧版本
![image.png|825](https://cdn.jsdelivr.net/gh/xuezhaorong/Picgo//Source/fix-dir/picgo/picgo-clipboard-images/2024/12/09/13-28-31-79771f304e83af35aecea6f7e923870a-20241209132831-7c84c9.png)

解压后进入到`bin`目录下，打开终端，运行`.\emqx.cmd start`，即启动mqtt服务器

可以使用`MQTTX`软件进行测试
![image.png|550](https://cdn.jsdelivr.net/gh/xuezhaorong/Picgo//Source/fix-dir/picgo/picgo-clipboard-images/2024/12/09/13-31-25-f051ac77d985601032673ddbc6d3158e-20241209133125-45144e.png)

### 开发
开发模式选择的是生产者(mqtt发布消息)-消费者(mqtt订阅消息)分离的方式。

#### 导入依赖
```xml
<!--mqtt相关配置-->  
<dependency>  
    <groupId>org.springframework.integration</groupId>  
    <artifactId>spring-integration-mqtt</artifactId>  
    <version>5.3.2.RELEASE</version>  
</dependency>
```

#### 消费者
1. 在`application.yml`中写入mqtt配置信息
```yml
mqtt:  
  host: tcp://127.0.0.1:1883  
  userName:  
  passWord:  
  client_id:  
    consumer_id: mqtt_consumer  
    provider_id: mqtt_provider  
  topic: v1/#  
  timeout: 10  
  keepalive: 20
```

2. 新建`config.matt`包与`MqttConsumerConfig`配置类
```java
@Configuration  
public class MqttConsumerConfig {  
    @Value("${mqtt.host}")  
    public String host;  
  
    @Value("${mqtt.username}")  
    public String username;  
  
    @Value("${mqtt.password}")  
    public String password;  
  
    @Value("${mqtt.client_id.consumer_id}")  
    public String clientId;  
  
    @Value("${mqtt.topic}")  
    public String topic;  
  
    @Value("${mqtt.timeout}")  
    public int timeout;  
  
    @Value("${mqtt.keepalive}")  
    public int keepalive;  
  
}
```


3. 新建`callback.mqtt`包与`MqttConsumerCallBack`回调类
```java
@Component  
public class MqttConsumerCallBack implements MqttCallback {    
    /**  
     * 客户端断开连接的回调  
     */  
    @Override  
    public void connectionLost(Throwable throwable) {  
        System.out.println("与MQTT代理服务器断开连接！！！");  
    }  
  
    /**  
     * 消息到达的回调  
     */  
    @Override  
    public void messageArrived(String topic, MqttMessage message) throws Exception {  
        System.out.printf("接收消息主题 : %s%n",topic);  
        System.out.printf("接收消息Qos : %d%n",message.getQos());  
        System.out.printf("接收消息内容 : %s%n",new String(message.getPayload()));  
        System.out.printf("接收消息retained : %b%n",message.isRetained());  
    }  
    /**  
     * 消息发布成功的回调  
     */  
    @Override  
    public void deliveryComplete(IMqttDeliveryToken iMqttDeliveryToken) {  
        System.out.println("接收消息成功");  
    }  
}
```

在类中`messageArrived`方法用于接收消息。


4. 新建消费者服务类
`MqttConsumerService`为用于实现连接，断开连接，订阅主题，取消订阅的功能
```java
public interface MqttConsumerService {  
  
    public void connect() throws MqttException; // 连接  
  
    public void disconnect() throws MqttException; //断开连接  
  
    public void subscribe(String topic,int qos) throws MqttException; // 订阅主题  
  
    public void unsubscribe(String topic) throws MqttException ; // 取消订阅  
  
}
```

`MqttConsumerServiceImpl`各个接口功能的实现
```java
@Service  
public class MqttConsumerServiceImpl implements MqttConsumerService {  
    private static MqttClient client;  
  
    @Autowired  
    private MqttConsumerConfig mqttConsumerConfig;  
  
  
    @Autowired  
    private MqttConsumerCallBack mqttConsumerCallBack;  
  
    // 设置mqtt连接参数  
    private MqttConnectOptions setMqttConnectOptions(){  
        MqttConnectOptions options = new MqttConnectOptions();  
        options.setUserName(mqttConsumerConfig.username);  
        options.setPassword(mqttConsumerConfig.password.toCharArray());  
        options.setConnectionTimeout(mqttConsumerConfig.timeout);  
        options.setKeepAliveInterval(mqttConsumerConfig.keepalive);  
        options.setCleanSession(true);  
        options.setAutomaticReconnect(true);  
        return options;  
    }  
  
    /**  
     * 在bean初始化后连接到服务器  
     */  
    @PostConstruct  
    public void init() throws MqttException{  
        connect();  
    }  
  
    @Override  
    public void connect() throws MqttException {  
        if(client == null){  
            client = new MqttClient(mqttConsumerConfig.host,mqttConsumerConfig.clientId,new MemoryPersistence());  
            // 设置回调  
            client.setCallback(mqttConsumerCallBack);  
        }  
  
        MqttConnectOptions mqttConnectOptions = setMqttConnectOptions();  
  
        if (!client.isConnected()){  
            client.connect(mqttConnectOptions);  
        }else{  
            client.disconnect();  
            client.connect(mqttConnectOptions);  
        }  
  
        System.out.println("MQTT connect success");  
  
        // 自动订阅  
        subscribe(mqttConsumerConfig.topic,0);  
    }  
  
    @Override  
    public void disconnect() throws MqttException {  
        if (client != null){  
            if (client.isConnected()){  
                client.disconnect();  
            }  
        }  
    }  
  
    @Override  
    public void subscribe(String topic,int qos) throws MqttException {  
        if (client != null){  
            if (client.isConnected()){  
                client.subscribe(topic,qos);  
            }  
        }  
    }  
  
    @Override  
    public void unsubscribe(String topic) throws MqttException {  
        if (client != null){  
            if (client.isConnected()){  
                client.unsubscribe(topic);  
            }  
        }  
    }  
}
```

#### 生产者
1. 在`application.yml`中写入mqtt配置信息
```yml
mqtt:  
  host: tcp://127.0.0.1:1883  
  userName:  
  passWord:  
  client_id:  
    consumer_id: mqtt_consumer  
    provider_id: mqtt_provider  
  topic: v1/#  
  timeout: 10  
  keepalive: 20
```

2. 新建`config.matt`包与`MqttProviderConfig`配置类
```java
@Configuration  
public class MqttProviderConfig {  
    @Value("${mqtt.host}")  
    public String host;  
  
    @Value("${mqtt.username}")  
    public String username;  
  
    @Value("${mqtt.password}")  
    public String password;  
  
    @Value("${mqtt.client_id.provider_id}")  
    public String clientId;  
  
    @Value("${mqtt.topic}")  
    public String topic;  
  
    @Value("${mqtt.timeout}")  
    public int timeout;  
  
    @Value("${mqtt.keepalive}")  
    public int keepalive;  
  
}
```


3. 新建`callback.mqtt`包与`MqttProviderCallBack`回调类
```java
public class MqttProviderCallBack implements MqttCallback {  
    /**  
     * 客户端断开连接的回调  
     */  
    @Override  
    public void connectionLost(Throwable throwable) {  
        System.out.println("与MQTT代理服务器断开连接！！！");  
    }  
  
    /**  
     * 消息到达的回调  
     */  
    @Override  
    public void messageArrived(String topic, MqttMessage message) throws Exception {  
        System.out.printf("接收消息主题 : %s%n",topic);  
        System.out.printf("接收消息Qos : %d%n",message.getQos());  
        System.out.printf("接收消息内容 : %s%n",new String(message.getPayload()));  
        System.out.printf("接收消息retained : %b%n",message.isRetained());  
  
    }  
    /**  
     * 消息发布成功的回调  
     */  
    @Override  
    public void deliveryComplete(IMqttDeliveryToken iMqttDeliveryToken) {  
        System.out.println("接收消息成功");  
    }  
}
```
`deliveryComplete`用于确认mqtt服务器接收到了发布的消息


4. 新建生产者服务类
`MqttProviderService`为用于实现连接，断开连接，订阅主题，取消订阅的功能
```java
public interface MqttProviderService {  
    public void connect() throws MqttException; // 连接  
  
    public void disconnect() throws MqttException; //断开连接  
  
    public void publish(String message, String topic, int qos, boolean retained) throws MqttException;  
  
}
```

`MqttProviderServiceImpl`各个接口功能的实现
```java
@Service  
public class MqttProviderServiceImpl implements MqttProviderService {  
  
    private static MqttClient client;  
  
    @Autowired  
    private MqttProviderConfig mqttProviderConfig;  
  
    // 设置mqtt连接参数  
    private MqttConnectOptions setMqttConnectOptions(){  
        MqttConnectOptions options = new MqttConnectOptions();  
        options.setUserName(mqttProviderConfig.username);  
        options.setPassword(mqttProviderConfig.password.toCharArray());  
        options.setConnectionTimeout(mqttProviderConfig.timeout);  
        options.setKeepAliveInterval(mqttProviderConfig.keepalive);  
        options.setCleanSession(true);  
        options.setAutomaticReconnect(true);  
        return options;  
    }  
  
    @PostConstruct  
    public void init() throws MqttException{  
        connect();  
    }  
  
    @Override  
    public void connect() throws MqttException {  
        if(client == null){  
            client = new MqttClient(mqttProviderConfig.host,mqttProviderConfig.clientId,new MemoryPersistence());  
            // 设置回调  
            client.setCallback(new MqttProviderCallBack());  
        }  
  
        MqttConnectOptions mqttConnectOptions = setMqttConnectOptions();  
  
        if (!client.isConnected()){  
            client.connect(mqttConnectOptions);  
        }else{  
            client.disconnect();  
            client.connect(mqttConnectOptions);  
        }  
  
        System.out.println("MQTT connect success");  
  
  
    }  
  
    @Override  
    public void disconnect() throws MqttException {  
        if (client != null){  
            if (client.isConnected()){  
                client.disconnect();  
            }  
        }  
    }  
  
    @Override  
    public void publish(String message, String topic, int qos, boolean retained) throws MqttException {  
        MqttMessage mqttMessage = new MqttMessage();  
        mqttMessage.setPayload(message.getBytes());  
        mqttMessage.setQos(qos);  
        mqttMessage.setRetained(retained);  
  
        MqttTopic mqttTopic = client.getTopic(topic);  
  
        MqttDeliveryToken token;  
  
        synchronized (this){  
            token = mqttTopic.publish(mqttMessage);  
            token.waitForCompletion(1000L);  
        }  
  
    }  
}
```

#### mqtt Controller
新建mqtt Controller用于初始化消费者和生产者服务
```java
@Controller  
@RequestMapping("/mqtt")  
public class MqttController {  
    @Autowired  
    private MqttProviderService mqttProviderService; // 生产者业务  
  
    @Autowired  
    private MqttConsumerService mqttConsumerService; // 消费者业务  
}
```

### 测试
云端测试时，使用`Mosquitto`作为mqtt服务器，[[Mosquitto服务搭建]]
注意：需要配置Websocket协议
使用`MQTTX`进行测试
![image.png|550](https://cdn.jsdelivr.net/gh/xuezhaorong/Picgo//Source/fix-dir/picgo/picgo-clipboard-images/2024/12/09/13-50-57-4ef3a510f0e8bc8a74195890d812d120-20241209135057-f53501.png)


