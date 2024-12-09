Qt官方提供了基于MQTT的封装，需要通过源码进行编译。官方链接：[qt/qtmqtt: Qt Module to implement MQTT protocol version 3.1 and 3.1.1 http://mqtt.org/](https://github.com/qt/qtmqtt)

## 编译安装
```bash
cmake
make 
make install
```

## mqtt的使用
### mqtt的连接
```cpp
QMqttClient *m_client = new QMqttClient(); // 创建Mqtt对象
m_client->setHostname(HOSTNAME); // 设置mqtt服务器IP地址
m_client->setPort(PORT); // 设置mqtt服务器端口
if (m_client->state() == QMqttClient::Disconnected){
    m_client->connectToHost(); // 连接
}
```

### mqtt断开连接
```cpp
if (m_client->state() == QMqttClient::Connected){
	m_client->disconnectFromHost(); // 断开连接
}
```

通过`stateChanged`信号来判断连接和断开
```cpp
connect(m_client, &QMqttClient::stateChanged, [=](QMqttClient::ClientState state){
	if (state == QMqttClient::Connected){	
	  qDebug() << "---------MQTT CONNECT SUCCESS";
	  MqttState = true;
	}else{
	  MqttState = false;
	  qDebug() << "---------MQTT CONNECT FAILED";
} });
```

### mqtt发布消息
注意`topic`和`message`的类型
```cpp
void MqttModel::MqttPublish(const QString &topic, const QByteArray &message) const{
  m_client->publish(topic, message);
}
```

### mqtt订阅消息
```cpp
auto subscription = m_client->subscribe(topic);
if (!subscription){
	return false;
}
```

通过`messageReceived`信号获取消息
```cpp
connect(m_client, &QMqttClient::messageReceived, this, [this](const QByteArray &message, const QMqttTopicName &topic){ 

	const QString content =
	" Received Topic: " + topic.name() + " Message: " + message + '\n';
	qDebug() << content; 
});
```

### mqtt取消订阅
```cpp
m_client->unsubscribe(topic);
```