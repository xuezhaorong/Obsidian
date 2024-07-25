>项目链接：[xuezhaorong/MyMqttClient (github.com)](https://github.com/xuezhaorong/MyMqttClient)
>第三方库链接：[hannesa2/paho.mqtt.android: Kotlin MQTT client for Android (github.com)](https://github.com/hannesa2/paho.mqtt.android)


## 导入第三方mqtt库
第三方库将旧版的mqtt库支持到了Android12+
```kotlin
implementation("com.github.hannesa2:paho.mqtt.android:4.2.3")
```

## 连接和断开连接

```kotlin
public fun connect(){ // 连接  
  
        // 实例化mqttClient  
        mqttClient = MqttAndroidClient(context,mqttServerURI, mqttClientID)  
  
        try {  
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
                    Log.d(TAG, "Receive message: ${message.toString()} from topic: $topic")  

  
                }  
  
                override fun deliveryComplete(token: IMqttDeliveryToken) {}  
            })  
        }catch (e:MqttException){  
            e.printStackTrace()  
        }  
  
        val mqttConnectOptions  = MqttConnectOptions()  
        // 是否选择匿名登录  
        if (mqttUsername != "" && mqttPassword!= ""){  
            mqttConnectOptions.userName = mqttUsername  
            mqttConnectOptions.password = mqttPassword.toCharArray()  
        }  
  
        // 设置cleansession  
//        mqttConnectOptions.isAutomaticReconnect = true  
//        mqttConnectOptions.isCleanSession = false  
        try {  
            mqttClient.connect(mqttConnectOptions, null, object : IMqttActionListener {  
                override fun onSuccess(asyncActionToken: IMqttToken?) {  
                    Log.d(TAG, "Connection success")  

                    connectFlag = true  
  
                    // 设置其他参数  
//                    val disconnectedBufferOptions = DisconnectedBufferOptions()  
//                    disconnectedBufferOptions.isBufferEnabled = true  
//                    disconnectedBufferOptions.bufferSize = 100  
//                    disconnectedBufferOptions.isPersistBuffer = false  
//                    disconnectedBufferOptions.isDeleteOldestMessages = false  
//                    mqttClient.setBufferOpts(disconnectedBufferOptions)  
                }  
  
                override fun onFailure(asyncActionToken: IMqttToken?, exception: Throwable?) {  
                    Log.d(TAG, "Connection failure")  
                    connectFlag = false  
                }  
            })  
        } catch (e: MqttException) {  
            e.printStackTrace()  
        }  
  
    }


public fun disconnect() { // 断开连接  
    try {  
        mqttClient.disconnect(null, object : IMqttActionListener {  
            override fun onSuccess(asyncActionToken: IMqttToken?) {  
                Log.d(TAG, "Disconnected")  

                connectFlag = false  
            }  
  
            override fun onFailure(asyncActionToken: IMqttToken?, exception: Throwable?) {  
                Log.d(TAG, "Failed to disconnect")  

                connectFlag = true  
            }  
        })  
    } catch (e: MqttException) {  
        e.printStackTrace()  
    }  
}
```

## 订阅和取消订阅
```kotlin
public fun subscribe(topic: String,qos: Int = 1) { //  订阅  
    try {  
        mqttClient.subscribe(topic, qos, null, object : IMqttActionListener {  
            override fun onSuccess(asyncActionToken: IMqttToken) {  
                Log.d(TAG, "subscribed $topic")  

            }  
  
            override fun onFailure(asyncActionToken: IMqttToken?, exception: Throwable?) {  
                Log.d(TAG, "Failed to subscribe $topic")  

            }  
        })  
  
  
    } catch (e: MqttException) {  
        e.printStackTrace()  
    }  
}  
  
public fun unsubscribe(topic: String) { // 取消订阅  
    try {  
        mqttClient.unsubscribe(topic, null, object : IMqttActionListener {  
            override fun onSuccess(asyncActionToken: IMqttToken?) {  
                Log.d(TAG, "Unsubscribed to $topic")  

            }  
  
            override fun onFailure(asyncActionToken: IMqttToken?, exception: Throwable?) {  
                Log.d(TAG, "Failed to unsubscribe $topic")  

            }  
        })  
    } catch (e: MqttException) {  
        e.printStackTrace()  
    }  
}
```

## 发布消息

```kotlin
public fun publish(topic: String, msg: String, qos: Int = 1, retained: Boolean = false) { // 发布消息  
    try {  
        val message = MqttMessage()  
        message.payload = msg.toByteArray()  
        message.qos = qos  
        message.isRetained = retained  
        mqttClient.publish(topic, message, null, object : IMqttActionListener {  
            override fun onSuccess(asyncActionToken: IMqttToken?) {  
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
```