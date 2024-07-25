
## 部署
官方部署链接：[http://www.ithingsboard.com/docs/user-guide/install/rhel/](http://www.ithingsboard.com/docs/user-guide/install/rhel/)
环境准备：**阿里云服务器2核2g（centos7）**
### 安装必要工具包
```shell
# Install wget
sudo yum install -y nano wget
# Add latest EPEL release for CentOS 7
sudo yum install -y https://dl.fedoraproject.org/pub/epel/epel-release-latest-7.noarch.rpm
```
### 更新软件源
下载阿里源
```shell
wget -O /etc/yum.repos.d/CentOS-Base.repo http://mirrors.aliyun.com/repo/Centos-7.repo
```
清理缓存
```shell
yum clean all
```
刷新缓存
```shell
yum makecache
```
更新最新源设置
```shell
yum update -y
```
### 安装JAVA11(OpenJDK)：
ThingsBoard服务运行在Java 11请按照以下说明安装OpenJDK 11
```shell
sudo yum install java-11-openjdk
```
使用以下命令设置默认版本是OpenJDK 11
```shell
sudo update-alternatives --config java
```
可以使用以下命令检查安装
```shell
java -version
```
命令输出结果
```shell
openjdk version "11.0.xx"
OpenJDK Runtime Environment (...)
OpenJDK 64-Bit Server VM (build ...)
```
### 安装服务
下载安装包，下载速度慢可以先在电脑上下载再传到服务器上
```shell
wget https://github.com/thingsboard/thingsboard/releases/download/v3.5.1/thingsboard-3.5.1.rpm
```
安装服务
```shell
sudo rpm -Uvh thingsboard-3.5.1.rpm
```
### 配置数据库
#### PostgreSQL安装
```shell
# Install the repository RPM (for CentOS 7):
sudo yum -y install https://download.postgresql.org/pub/repos/yum/reporpms/EL-7-x86_64/pgdg-redhat-repo-latest.noarch.rpm
# Install packages
sudo yum -y install epel-release yum-utils
sudo yum-config-manager --enable pgdg12
sudo yum install postgresql12-server postgresql12
# 初始化PostgreSQL数据库
sudo /usr/pgsql-12/bin/postgresql-12-setup initdb
# 启动 postgreSQL
sudo systemctl start postgresql-12
# 将 PostgreSQL 设置开机启动
sudo systemctl enable --now postgresql-12
```
创安装 PostgreSQL 后，创建一个新用户或为主用户设置密码。postgres安装完之后默认会创建 postgres账户，下列操作将设置 postgres的密码：输入到 \password之后会提示输入密码，设置密码即可。
```shell
sudo su - postgres
psql
\password
\q
```
然后，按”Ctrl+D”返回主用户控制台。配置密码后，编辑pg_hba.conf以对postgres用户使用MD5认证。
```shell
#编辑pg_hba.conf文件：
sudo vi /var/lib/pgsql/12/data/pg_hba.conf
#找到以下几行：
#IPv4 local connections:
host    all             all             127.0.0.1/32            ident
#替换ident为md5:
host    all             all             127.0.0.1/32            md5
```
重新启动PostgreSQL服务以初始化配置
```shell
sudo systemctl restart postgresql-12.service
```
#### PostgreSQL配置
连接到数据库并创建Thingsboard数据库
```shell
#连接到数据库并创建Thingsboard： 
# -h 指定本地socket目录
# -p 指定端口号
# -U 指定用户名
# -d 指定数据库名称
# -W 指定终端提示符下输入密码
psql -U postgres -d postgres -h 127.0.0.1 -W

#执行数据库创建数据库语句
CREATE DATABASE thingsboard;
\q
```
编辑配置文件
```shell
sudo vim /etc/thingsboard/conf/thingsboard.conf
```
将下面内容添加到配置文件中并**替换**“PUT_YOUR_POSTGRESQL_PASSWORD_HERE”为**postgres帐户密码**：
```shell
#将“PUT_YOUR_POSTGRESQL_PASSWORD_HERE”替换postgres用户真实密码
# DB Configuration 
export DATABASE_ENTITIES_TYPE=sql
export DATABASE_TS_TYPE=sql
export SPRING_JPA_DATABASE_PLATFORM=org.hibernate.dialect.PostgreSQLDialect
export SPRING_DRIVER_CLASS_NAME=org.postgresql.Driver
export SPRING_DATASOURCE_URL=jdbc:postgresql://localhost:5432/thingsboard
export SPRING_DATASOURCE_USERNAME=postgres
export SPRING_DATASOURCE_PASSWORD=PUT_YOUR_POSTGRESQL_PASSWORD_HERE
export SPRING_DATASOURCE_MAXIMUM_POOL_SIZE=5
# Specify partitioning size for timestamp key-value storage. Allowed values: DAYS, MONTHS, YEARS, INDEFINITE.
export SQL_POSTGRES_TS_KV_PARTITIONING=MONTHS
```
### 低性能配置（可选）
编辑配置文件
```shell
sudo nano /etc/thingsboard/conf/thingsboard.conf
```
将以下行添加到配置文件
```shell
# Update ThingsBoard memory usage and restrict it to 256MB in /etc/thingsboard/conf/thingsboard.conf
export JAVA_OPTS="$JAVA_OPTS -Xms256M -Xmx256M"
```
### 运行安装脚本
执行以下脚本安装ThingsBoard服务并初始化演示数据
```shell
# --loadDemo option will load demo data: users, devices, assets, rules, widgets.
sudo /usr/share/thingsboard/bin/install/install.sh --loadDemo
```
### 启动服务
**ThingsBoard UI 默认在8080端口上访问确保8080端口可以通过防火墙访问**
```shell
#ThingsBoard默认使用8080端口请执行以下命令确保正确打开：
sudo firewall-cmd --zone=public --add-port=8080/tcp --permanent
sudo firewall-cmd --reload
```
执行以下命令以启动ThingsBoard
```shell
sudo service thingsboard start
```
启动后使用以下链接打开Web UI（localhost为服务器IP）
```shell
http://localhost:8080/
```
### 登录账号

- 系统管理员（**System Administrator**）：` sysadmin@thingsboard.org / sysadmin`
- 租户管理员（**Tenant Administrator**）：` tenant@thingsboard.org / tenant`
- 客户（**Customer User**）：`customer@thingsboard.org / customer`

## 感知层通信
#### 遥测数据上报
 发布遥测数据到ThingsBoard服务端必须PUBLISH消息发送到主题：`v1/devices/me/telemetry`  
#### 属性上报
通过ThingsBoard服务端发布客户端属性必须PUBLISH消息到主题 ：`v1/devices/me/attributes`
#### 获取属性
在发送带有请求的PUBLISH消息之前客户端需要订阅主题：`v1/devices/me/attributes/response/+`
通过ThingsBoard服务端获取客户端属性或共享属性必须PUBLISH消息到主题：`v1/devices/me/attributes/request/$request_id`，其中$request_id**表示整数的请求标识符。
## 应用层通信
### 获得属性
通过服务端订阅共享属性必须SUBSCRIBE消息到主题：`v1/devices/me/attributes`
如果服务端组件（例如REST API或规则链）更改了共享属性时客户端就会收到对应属性值
### 下发指令
将RPC命令发送到服务端必须PUBLISH消息发送到下面主题：`v1/devices/me/rpc/request/$request_id`
**$request_id**表示请求的整型标识符
## 双向通信
双向通信指感知层向ThingsBoard平台上传遥测和属性数据，并订阅获得最新的属性，应用层向平台层订阅获得遥测和属性数据，并发送RPC指令控制属性值。
### 实体与关联设置
![](https://cdn.jsdelivr.net/gh/xuezhaorong/Picgo//Source/fix-dir/Download/photo/2024/06/26/18-55-15-e306b103ef38bd6dfde0f4d142133d2d-%E5%BE%AE%E4%BF%A1%E6%88%AA%E5%9B%BE_20240626185441-4f3f3a.png#id=aGqrr&originHeight=459&originWidth=2111&originalType=binary&ratio=1&rotation=0&showTitle=false&status=done&style=none)
包含两个实体：手机设备作为应用层，实验设备作为感知层，手机设备与实验设备为从Contains关联关系。
### 规则链设置
配置基本的感知层上传遥测数据和属性的节点
![](https://cdn.jsdelivr.net/gh/xuezhaorong/Picgo//Source/fix-dir/Download/photo/2024/06/26/19-01-54-3e2c30364cf1e07ae6d066bbe8d3e255-%E5%BE%AE%E4%BF%A1%E6%88%AA%E5%9B%BE_20240626190142-87095a.png#id=E0rV8&originHeight=326&originWidth=1206&originalType=binary&ratio=1&rotation=0&showTitle=false&status=done&style=none)
配置上传共享属性规则链
![](https://cdn.jsdelivr.net/gh/xuezhaorong/Picgo//Source/fix-dir/Download/photo/2024/06/26/19-05-51-9f6cc44e266141ca4acc88dfb9d310a1-%E5%BE%AE%E4%BF%A1%E6%88%AA%E5%9B%BE_20240626190307-b18599.png#id=GPdS1&originHeight=221&originWidth=1972&originalType=binary&ratio=1&rotation=0&showTitle=false&status=done&style=none)
配置转换发起者节点将属性上传者变换为手机设备
![](https://cdn.jsdelivr.net/gh/xuezhaorong/Picgo//Source/fix-dir/Download/photo/2024/06/26/19-07-17-fb7c5d2684811c7ff555407130287a63-%E5%BE%AE%E4%BF%A1%E6%88%AA%E5%9B%BE_20240626190708-826416.png#id=Edibb&originHeight=586&originWidth=1457&originalType=binary&ratio=1&rotation=0&showTitle=false&status=done&style=none)
转换节点中将data转换为合适的格式，并将metadata中的notifydevice属性设置为true，及时更新属性
```javascript
// 获取输入的 JSON 对象
// var input = msg;

// 获取 deviceMacId 作为键
var deviceMacId = input.deviceMacId;

// 创建目标 JSON 对象
var output = {};
output[deviceMacId] = {
     deviceMacId: input.deviceMacId,
     motor1: input.motor1,
     motor2: input.motor2,
     lighter: input.lighter,
     granted: input.granted
};

metadata.notifyDevice = true;
return {msg: msg, metadata: metadata, msgType: msgType};
```
配置RPC规则链，先将RPC Request的发起者转换为设备，经过RPC方法类型过滤，第二层转换将设备对象转为发起者即设备本身，然后保存属性，达到下发RPC改变属性的功能。
![](https://cdn.jsdelivr.net/gh/xuezhaorong/Picgo//Source/fix-dir/Download/photo/2024/06/26/20-05-38-e338e308940861e721aa6b0587b3ee51-%E5%BE%AE%E4%BF%A1%E6%88%AA%E5%9B%BE_20240626200526-1e7bb9.png#id=e8OeC&originHeight=472&originWidth=1966&originalType=binary&ratio=1&rotation=0&showTitle=false&status=done&style=none)
将发起者转换为设备
![](https://cdn.jsdelivr.net/gh/xuezhaorong/Picgo//Source/fix-dir/Download/photo/2024/06/26/20-07-26-e173f03a5b59a75f8b386aa670956588-%E5%BE%AE%E4%BF%A1%E6%88%AA%E5%9B%BE_20240626200720-11ea50.png#id=VxlOD&originHeight=677&originWidth=1472&originalType=binary&ratio=1&rotation=0&showTitle=false&status=done&style=none)
将设备对象转为发起者即设备本身。
```javascript
// 解析 params 字段中的 JSON 字符串
var params = JSON.parse(msg.params);

// 创建属性对象
var attributes = {
    lighter: parseFloat(params.lighter)
};

var metadata2 = {
    deviceName : metadata.originatorName,
    deviceType : metadata.originatorType,
    notifyDevice : true
};

// 返回属性对象
return {msg: attributes, metadata: metadata2, msgType: 'POST_ATTRIBUTES_REQUEST'};
```
连到共享属性的规则链中，更改共享属性，让应用端可以获得更改后的属性。
![](https://cdn.jsdelivr.net/gh/xuezhaorong/Picgo//Source/fix-dir/Download/photo/2024/06/26/20-08-38-f8e76c26623fb428a9abb5b619c82f29-%E5%BE%AE%E4%BF%A1%E6%88%AA%E5%9B%BE_20240626200829-bd9f53.png#id=Y53i1&originHeight=559&originWidth=1584&originalType=binary&ratio=1&rotation=0&showTitle=false&status=done&style=none)
### 感知层配置
使用esp8266的mqtt库进行通信
#### 遥测数据上报
```cpp
const char* topicAttributesPublish =  "v1/devices/me/attributes"; // 属性数据发布通道

// 上报数据
// 填充遥测数据
JsonDocument doc;
doc["temperature"] = dataArray[1];
doc["humidity"] = dataArray[2];
doc["light"] = dataArray[3];

// 序列化
serializeJson(doc, jsonBuffer, sizeof(jsonBuffer));
mqttPubMessage(topicTelemetryPublish, jsonBuffer); // 发布遥测数据

memset(jsonBuffer,0,sizeof(jsonBuffer));

// 填充属性数据
doc["deviceMacId"] = dataArray[0];
doc["granted"] = dataArray[7];

// 序列化
serializeJson(doc, jsonBuffer, sizeof(jsonBuffer));
mqttPubMessage(topicAttributesPublish, jsonBuffer); // 发布属性数据
```
#### 属性上报
```cpp
const char* topicAttributesPublish =  "v1/devices/me/attributes"; // 属性数据发布通道

// 上报数据
// 填充遥测数据
JsonDocument doc;
doc["temperature"] = dataArray[1];
doc["humidity"] = dataArray[2];
doc["light"] = dataArray[3];

// 序列化
serializeJson(doc, jsonBuffer, sizeof(jsonBuffer));
mqttPubMessage(topicTelemetryPublish, jsonBuffer); // 发布遥测数据

memset(jsonBuffer,0,sizeof(jsonBuffer));

// 填充属性数据
doc["deviceMacId"] = dataArray[0];
doc["granted"] = dataArray[7];

// 序列化
serializeJson(doc, jsonBuffer, sizeof(jsonBuffer));
mqttPubMessage(topicAttributesPublish, jsonBuffer); // 发布属性数据
```
#### 获取属性
```cpp
const char* topicGetAttributesPublish = "v1/devices/me/attributes/request/"; // 获取属性通道

void mqttGetAttributes(){
    static uint16_t requestId = 1;
    char requestIdStr[10];
    char finalTopic[100];
    snprintf(requestIdStr, sizeof(requestIdStr), "%u", requestId++);
    // 连接字符串
    // 复制原始主题字符串到最终缓冲区
    strncpy(finalTopic, topicGetAttributesPublish, sizeof(finalTopic) - 1);
    // 确保最终缓冲区以空字符结尾
    finalTopic[sizeof(finalTopic) - 1] = '0';
    // 连接整数字符串到最终缓冲区
    strncat(finalTopic, requestIdStr, sizeof(finalTopic) - strlen(finalTopic) - 1);
    // 订阅获取属性频道
    mqttScribeTopic(topicAttributesSubscribe);
    // 发送获取属性的请求
    String message = R"({"clientKeys":"motor1,motor2,lighter"})";
    mqttPubMessage(finalTopic,message);
    
}
```
### 应用层配置
#### 获得属性
先订阅获得共享属性的主题
```kotlin
// 订阅主题
mqttClientHelper.subscribe("v1/devices/me/attributes");
```
在回调函数中设置更新
```kotlin
mqttClient.setCallback(object : MqttCallbackExtended {
    override fun connectComplete(reconnect: Boolean, serverURI: String) {
        if (reconnect) {
            Log.d(TAG, "Reconnected: $serverURI.")

        } else {
            Log.d(TAG, "Connected: $serverURI.")
        }
    }

    override fun connectionLost(cause: Throwable?) {
        Log.d(TAG, "The Connection was lost.")
    }

    override fun messageArrived(topic: String, message: MqttMessage) {
        Log.d(TAG, "Receive message: $message from topic: $topic")


        // 解析JSON 数据
        val jsonData = String(message.payload)
        val gson = Gson()

        val device = gson.fromJson(jsonData,Device::class.java)
        if (device.deviceMacId!=""){
            Log.d(TAG,device.toString())

            // 更改数据库
            deviceViewModel.updateDeviceMessage(device)
        }

    }

    override fun deliveryComplete(token: IMqttDeliveryToken) {}
})
```
#### 下发指令
组装rpc指令发布到主题
```json
// rpc格式
{
    "method" : " ",
    "params" : {
        "" : ""
    }
}
```

```kotlin
// 发布消息
fun publish(topic: String, function:String,param:String,value:String, qos: Int = 1, retained: Boolean = false) {
    try {
        // 创建 JSON 对象并填充数据
        val json = JSONObject().apply {
            put("method", function)
            put("params","{\"${param}\":$value}")
        }
        val msg = json.toString()
        val message = MqttMessage()
        message.payload = msg.toByteArray()
        message.qos = qos
        message.isRetained = retained
        mqttClient.publish(topic, message, null, object : IMqttActionListener {
            override fun onSuccess(asyncActionTlooken: IMqttToken?) {
                Log.d(TAG, "$msg published to $topic")
            }

            override fun onFailure(asyncActionToken: IMqttToken?, exception: Throwable?) {
                Log.d(TAG, "Failed to publish $msg to $topic")

            }
        })
    } catch (e: MqttException) {
        e.printStackTrace()
    }
}


// 设置光照
fun mqttSetLighter(value: String){
    publish(
        topic = "v1/devices/me/rpc/request/1",
        function = "setLighter",
        param = "lighter",
        value = value
    )
}

// 设置马达1
fun mqttSetMotor1(value: String){
    publish(
        topic = "v1/devices/me/rpc/request/1",
        function = "setMotor1",
        param = "motor1",
        value = value
    )
}

// 设置马达2
fun mqttSetMotor2(value: String){
    publish(
        topic = "v1/devices/me/rpc/request/1",
        function = "setMotor2",
        param = "motor2",
        value = value
    )
}
```
