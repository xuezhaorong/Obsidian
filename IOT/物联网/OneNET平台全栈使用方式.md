## OneNET平台

### 产品管理
创建产品
![|925](https://cdn.jsdelivr.net/gh/xuezhaorong/Picgo//Source/fix-dir/picgo/picgo-clipboard-images/2025/02/25/17-46-13-f3e768387b2b17019ba4256bd1b9654a-20250225174612-4198c8.png)

 进入产品详情中创建数据流模板
![|1100](https://cdn.jsdelivr.net/gh/xuezhaorong/Picgo//Source/fix-dir/picgo/picgo-clipboard-images/2025/02/25/17-53-10-2ff9f88934ae3eee5c8844fcf67276af-20250225175309-df772c.png)

发布成品
![|1125](https://cdn.jsdelivr.net/gh/xuezhaorong/Picgo//Source/fix-dir/picgo/picgo-clipboard-images/2025/02/25/17-54-46-b2277a4011060301546fb6199986a4a1-20250225175446-13f679.png)

### 设备管理
进入设备管理，创建一个设备
![|1100](https://cdn.jsdelivr.net/gh/xuezhaorong/Picgo//Source/fix-dir/picgo/picgo-clipboard-images/2025/02/25/17-57-52-32225f43ebd5600c230730b9d36a96f4-20250225175751-9204f0.png)

记录设备名称，设备密钥和产品ID
![|1100](https://cdn.jsdelivr.net/gh/xuezhaorong/Picgo//Source/fix-dir/picgo/picgo-clipboard-images/2025/02/25/17-59-07-6631a8b64c6aa6215b76ebd957d7e9d7-20250225175906-a223b3.png)

生成token，token工具链接：[https://open.iot.10086.cn/college/video/onenet-portal/2024-04-19/17134946071850.exe](https://open.iot.10086.cn/college/video/onenet-portal/2024-04-19/17134946071850.exe)
![|500](https://cdn.jsdelivr.net/gh/xuezhaorong/Picgo//Source/fix-dir/picgo/picgo-clipboard-images/2025/02/25/18-03-10-922a6d4d9b7a08a147b1ca219a1d4fed-20250225180309-81618c.png)

* res格式：`products/{产品ID}/devices/{设备名称}`
- et：未来截止时间点的时间戳
- key：产品密钥
点击Generate生成token，记录下来

![image.png|1100](https://cdn.jsdelivr.net/gh/xuezhaorong/Picgo//Source/fix-dir/picgo/picgo-clipboard-images/2025/03/20/14-28-41-752096a8519cbc6f8ae59daadb5df623-20250320142841-717ee2.png)

### 项目管理
点击左侧的应用开发->项目管理
![image.png|271](https://cdn.jsdelivr.net/gh/xuezhaorong/Picgo//Source/fix-dir/picgo/picgo-clipboard-images/2025/03/30/15-44-47-1e533c7bebb4154ae6956457efd4a5ca-20250330154446-4101b1.png)

新建一个项目，然后进入项目管理中
![image.png|1000](https://cdn.jsdelivr.net/gh/xuezhaorong/Picgo//Source/fix-dir/picgo/picgo-clipboard-images/2025/03/30/15-44-01-f0d6d010c21ad580f166d9c8bdc38d30-20250330154400-9bf77f.png)

记录项目ID和项目KEY
![image.png|1000](https://cdn.jsdelivr.net/gh/xuezhaorong/Picgo//Source/fix-dir/picgo/picgo-clipboard-images/2025/03/30/15-54-00-6b5cb7400889f986a1f81234e67ec725-20250330155400-fea95e.png)

进入设备管理中的设备列表，添加创建的设备
![image.png|1000](https://cdn.jsdelivr.net/gh/xuezhaorong/Picgo//Source/fix-dir/picgo/picgo-clipboard-images/2025/03/30/15-56-23-9bd84d850614cdca117d2589084b2b38-20250330155623-d02811.png)

## BC260Y NBIOT模块
### 连接设备
连接串口工具，打开串口助手，输入指令，波特率为9600
注意：在MCU中发送时需要加入`\r\n`结尾

1. 退出低功耗模式：`AT+QSCLK=0`
![image.png|700](https://cdn.jsdelivr.net/gh/xuezhaorong/Picgo//Source/fix-dir/picgo/picgo-clipboard-images/2025/02/26/11-55-06-d97272e8214603e0f32cd94649286dc2-20250226115505-41fecb.png)


![image.png|800](https://cdn.jsdelivr.net/gh/xuezhaorong/Picgo//Source/fix-dir/picgo/picgo-clipboard-images/2025/02/26/11-55-24-832f6548da1c7d575ce5936de74de3b7-20250226115523-42408e.png)

2. 修改MQTT协议版本为3.1.1：`AT+QMTCFG="version",0,1`
![|600](https://cdn.jsdelivr.net/gh/xuezhaorong/Picgo//Source/fix-dir/picgo/picgo-clipboard-images/2025/02/25/18-45-08-f39b3b2340c5fa60fd86d119fb5ea7f5-20250225184507-ffdf7b.png)

![|625](https://cdn.jsdelivr.net/gh/xuezhaorong/Picgo//Source/fix-dir/picgo/picgo-clipboard-images/2025/02/25/18-48-39-62e7e877cdfffc8cd0bd7f9951c79e3c-20250225184838-15a8a3.png)

2. 打开MQTT客户端：`AT+QMTOPEN=0,"mqtts.heclouds.com",1883` 
![image.png|750](https://cdn.jsdelivr.net/gh/xuezhaorong/Picgo//Source/fix-dir/picgo/picgo-clipboard-images/2025/02/25/19-09-09-2e268201ee645aca5d570c1d01233992-20250225190907-d8a2b9.png)


![image.png|750](https://cdn.jsdelivr.net/gh/xuezhaorong/Picgo//Source/fix-dir/picgo/picgo-clipboard-images/2025/02/25/19-09-52-a655934c3abf469c3a55ce7df225ba0c-20250225190952-d27274.png)

3.  连接到设备：`AT+QMTCONN=0,"{设备名}","{产品ID}","{token}"`
![image.png|750](https://cdn.jsdelivr.net/gh/xuezhaorong/Picgo//Source/fix-dir/picgo/picgo-clipboard-images/2025/02/25/19-11-47-72a0e96d245e51317b860d9b4da4ebde-20250225191146-52309e.png)

![image.png|750](https://cdn.jsdelivr.net/gh/xuezhaorong/Picgo//Source/fix-dir/picgo/picgo-clipboard-images/2025/02/25/19-12-30-1997ff666dd8fd76df8a8858da515ab4-20250225191230-781b06.png)

### 上报数据
![image.png|750](https://cdn.jsdelivr.net/gh/xuezhaorong/Picgo//Source/fix-dir/picgo/picgo-clipboard-images/2025/02/25/19-13-25-02c2b8e6cc5c40d18baf7c0846980256-20250225191325-9159a5.png)

1. 订阅响应主题：`AT+QMTSUB=0,1,"$sys/{产品ID}/{设备名称}/dp/post/json/accepted",0
`
![image.png|1000](https://cdn.jsdelivr.net/gh/xuezhaorong/Picgo//Source/fix-dir/picgo/picgo-clipboard-images/2025/02/25/19-15-12-9ed57c75b5b61f52e4c3c11b63d224c9-20250225191512-4ad10a.png)

2. 发布上传主题：`AT+QMTPUB=0,0,0,0,"$sys/{产品ID}/{设备名称}/dp/post/json"`,待`>`出现后再发送消息。
`{"id":123,"dp":{"humidity":[{"v":30}]}}`(不需要加入\\r\\n)，需要发送`HEX`的`0x1A`(不需要加入\\r\\n)确认发送。
![image.png|800](https://cdn.jsdelivr.net/gh/xuezhaorong/Picgo//Source/fix-dir/picgo/picgo-clipboard-images/2025/03/20/14-07-18-f0f387911a9dae70ee9f68c63f30de6d-20250320140717-5e26a1.png)


![image.png|800](https://cdn.jsdelivr.net/gh/xuezhaorong/Picgo//Source/fix-dir/picgo/picgo-clipboard-images/2025/02/25/19-20-07-5c0947727b41509ce45fbd222d12a844-20250225192006-48ddf3.png)

### 下发命令

![image.png|800](https://cdn.jsdelivr.net/gh/xuezhaorong/Picgo//Source/fix-dir/picgo/picgo-clipboard-images/2025/03/30/16-02-40-5ac2763b31d4315430983161c9215976-20250330160239-c5dd46.png)

1. 订阅响应主题：`AT+QMTSUB=0,1,"$sys/{产品ID}/{设备名称}/cmd/#",0`
![image.png|800](https://cdn.jsdelivr.net/gh/xuezhaorong/Picgo//Source/fix-dir/picgo/picgo-clipboard-images/2025/02/25/19-15-12-9ed57c75b5b61f52e4c3c11b63d224c9-20250225191512-4ad10a.png)

2. 云平台模拟命令下发，在设备详情中点击命令下发，设置响应时间(最大30秒)，输入命令，点击发送
![image.png|900](https://cdn.jsdelivr.net/gh/xuezhaorong/Picgo//Source/fix-dir/picgo/picgo-clipboard-images/2025/03/30/16-06-47-f9c81f85669916e5f500754b123613f4-20250330160646-84ce98.png)

3. BC260Y接收到数据帧，格式为`+QMTRECV: 0,0,"$sys/{产品ID}/{设备名称}/cmd/request/{消息ID}","{命令}"`,需要保存获得的消息ID。

![image.png|847](https://cdn.jsdelivr.net/gh/xuezhaorong/Picgo//Source/fix-dir/picgo/picgo-clipboard-images/2025/03/30/16-44-53-76f8176b2a3a50e6797ce38cc9f5c1e4-20250330164453-01eac7.png)


![image.png|800](https://cdn.jsdelivr.net/gh/xuezhaorong/Picgo//Source/fix-dir/picgo/picgo-clipboard-images/2025/03/30/16-11-14-c29023343929f199b7f9aa73dd49d699-20250330161113-346ca1.png)

4. 将保存的消息ID填充到主题`$sys/{产品ID}/{设备名称}/cmd/response/{消息ID}`中，并发布一条任意消息，即可响应该命令，在响应时间的返回结果处可以看到响应的数据。
![image.png|800](https://cdn.jsdelivr.net/gh/xuezhaorong/Picgo//Source/fix-dir/picgo/picgo-clipboard-images/2025/03/30/16-48-36-d16b1a2cdcd8885cd1097dd9d5dd478f-20250330164835-b13457.png)

## 后端功能
### 服务端订阅
后端借助OneNET云平台的服务端订阅功能： https://open.iot.10086.cn/doc/v5/fuse/detail/1530 ,实现设备上报数据的转发和监听设备上下线等功能。
![image.png|1000](https://cdn.jsdelivr.net/gh/xuezhaorong/Picgo//Source/fix-dir/picgo/picgo-clipboard-images/2025/03/30/16-54-23-7b1b3d105f20e9704c0a30c89bbfb5db-20250330165423-062005.png)

需要在项目中的服务端订阅里设置 推送消息类型和添加订阅。
![image.png|300](https://cdn.jsdelivr.net/gh/xuezhaorong/Picgo//Source/fix-dir/picgo/picgo-clipboard-images/2025/03/30/17-07-05-873abc02f7f4dde056d82dc8d27d2304-20250330170704-b6dc0a.png)

![image.png|1000](https://cdn.jsdelivr.net/gh/xuezhaorong/Picgo//Source/fix-dir/picgo/picgo-clipboard-images/2025/03/30/17-05-26-9aee67e8c1f7d8dc2bb39493970e6fce-20250330170526-41e33d.png)


1. 添加工具包
```xml
<!-- ONENET -->  
<dependency>  
    <groupId>org.apache.pulsar</groupId>  
    <artifactId>pulsar-client</artifactId>  
</dependency>  
  
  
<dependency>  
    <groupId>com.alibaba</groupId>  
    <artifactId>fastjson</artifactId>  
    <version>2.0.32</version>  
</dependency>
```

2. 下载工具库，加载到项目中，链接：https://wwet.lanzn.com/iUIIc2s4rp8f
![image.png|464](https://cdn.jsdelivr.net/gh/xuezhaorong/Picgo//Source/fix-dir/picgo/picgo-clipboard-images/2025/03/30/16-58-21-fdeac2a8400abfd32b9617f75ed2e0bb-20250330165821-af637e.png)

3. 新建`IoTPulsarConsumerController`控制类，注意填写`iotAccessId`，`iotSecretKey`和`iotSubscriptionName`
```java
@RestController  
public class IoTPulsarConsumerController {  
    //TODO need to set iotAccessId 消费组ID  
    private static  String iotAccessId=IoTConfig.iotAccessId;  
    //TODO need to set iotSecretKey 消费组KEY  
    private static  String iotSecretKey=IoTConfig.iotSecretKey;  
  
    //TODO 订阅名称  
    private static  String iotSubscriptionName=IoTConfig.iotSubscriptionName;  
  
  
    @EventListener(ApplicationReadyEvent.class)  
    @Order(2)  
    public void IoTPulsarConsumer() throws Exception {  
        if (StringUtil.isNullOrEmpty(iotAccessId)) {  
            log.error("iotAccessId is null,please input iotAccessId");  
            System.exit(1);  
        }  
        if (StringUtil.isNullOrEmpty(iotSecretKey)) {  
            log.error("iotSecretKey is null,please input iotSecretKey");  
            System.exit(1);  
        }  
        if (StringUtil.isNullOrEmpty(iotSubscriptionName)) {  
            log.error("iotSubscriptionName is null,please input iotSubscriptionName");  
            System.exit(1);  
        }  
        //TODO 建议收到消息后将消息转到中间件后立即ACK。避免消息量过大导致消息过期  
        IoTConsumer iotConsumer = IoTConsumer.IOTConsumerBuilder.anIOTConsumer().brokerServerUrl(IoTConfig.brokerSSLServerUrl)  
                .iotAccessId(iotAccessId)  
                .iotSecretKey(iotSecretKey)  
                .subscriptionName(iotSubscriptionName)  
                .iotMessageListener(message -> {  
                    MessageId msgId = message.getMessageId();  
                    long publishTime = message.getPublishTime();  
                    String payload = new String(message.getData());  
                    IoTMessage iotMessage= JSONObject.parseObject(payload, IoTMessage.class);  
                    String originalMsg= AESBase64Utils.decrypt(iotMessage.getData(),iotSecretKey.substring(8,24));  
                    log.info("IOT consume message======>>>>>>> messageId={}, publishTime={},  payload={}",  
                            msgId, publishTime, payload);  
                    log.info(originalMsg);  
                // 自定义处理
            
                }).build();  
        iotConsumer.run();  
    }  
}
```

* 在返回的数据中，可以查看`msgType`节点来看是什么类型的消息推送，如`datapoint`则为数据上报，`deviceOnline`则为数据上线。

### 平台API下发命令
后端可以通过发送平台的API来实现命令的下发：https://open.iot.10086.cn/doc/v5/fuse/detail/1436，API接口为:`https://iot-api.heclouds.com/datapoint/synccmds`

新建一个`IoTPulsarProviderService`服务类，要设置`Authorization`安全授权，即产品的token，填充产品ID和设备名称以及超时，并使用`HttpEntity`构造消息主体，`POST`之后平台会向目标设备发送消息主体的MQTT消息。
```java
@Service  
public class IoTPulsarProviderServiceImpl implements IoTPulsarProviderService {  
  
  
    @Autowired  
    private RestTemplate restTemplate;  
  
    @Override  
    public Boolean postCommand(String product_id, String device_name, String timeout,String commandType, String value) {  
        HttpHeaders headers = new HttpHeaders();  
        headers.setContentType(MediaType.APPLICATION_JSON);  
        headers.set("Authorization", IoTConfig.tokenSoildProducts);  
        String url = IoTConfig.baseUrl + IoTConfig.postCommandUrl + "?product_id={product_id}&device_name={device_name}&timeout={timeout}";  
        Map<String, String> params = new HashMap<String, String>();  
        params.put("product_id", product_id);  
        params.put("device_name", device_name);  
        params.put("timeout", timeout);  
        Command<String> command = new Command<>(commandType,value);  
        HttpEntity<Command> entity = new HttpEntity<>(command, headers);  
        try {  
            ResponseEntity<String> responseEntity = restTemplate.postForEntity(url, entity, String.class, params);  
            // 获取响应体  
            String responseBody = responseEntity.getBody();  
            log.info("Return message: " + responseBody);  
            return true;  
        }catch (Exception e) {  
            log.error("Request failed: {}", e.getMessage());  
            return false;  
        }  
    }  
}
```

使用`RestTemplate`来实现http的发送，以下为`RestTemplate`的配置类。 
```java
@Configuration  
public class RestTemplateConfig {  
    @Bean  
    public RestTemplate restTemplate(ClientHttpRequestFactory factory) {  
        return new RestTemplate(factory);  
    }  
  
    @Bean  
    public ClientHttpRequestFactory simpleClientHttpRequestFactory() {  
        SimpleClientHttpRequestFactory factory = new SimpleClientHttpRequestFactory();  
        //超时设置  
        factory.setReadTimeout(30000);//ms  
        factory.setConnectTimeout(15000);//ms  
        return factory;  
    }  
}
```