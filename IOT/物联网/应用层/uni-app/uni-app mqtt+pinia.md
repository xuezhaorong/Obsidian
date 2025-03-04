## 安装导入
在项目目录下使用`npm install mqtt --save`安装

在`main.js`中导入mqtt包，并进行兼容性设置
```js
// 导入mqtt
import mqtt from 'mqtt/dist/mqtt.js';

// #ifndef MP
// mqtt兼容性设置
uni.connectSocket = (function(connectSocket){
  return function(options) {
  		console.log(options)
  		options.success = options.success || function() {}
  		return connectSocket.call(this, options)
  	}
})(uni.connectSocket)
// #endif
```


## 配置
新建`utils`目录和`mqtt_config.js`配置工具文件
注意：
* ***`MQTT_IP`连接的mqtt服务器要配置websocket协议**

```js
import mqtt from 'mqtt/dist/mqtt.js';


export const MQTT_IP = 'wx://ip:PORT/mqtt'; //mqtt地址端口
const MQTT_USERNAME = ''; //mqtt用户名
const MQTT_PASSWORD = ''; //密码
export let client = null;


export let  MQTT_OPTIONS = {
  connectTimeout: 5000,
  clientId: '',
  username: MQTT_USERNAME,
  password: MQTT_PASSWORD,
  clean: false
}

// mqtt 连接
export function mqtt_Connect(topic){
  // 填充uuid
  MQTT_OPTIONS.clientId = `mqtt_${Math.random().toString(16).slice(3)}`;
  client = mqtt.connect(MQTT_IP, MQTT_OPTIONS);
  //成功连接后触发的回调
  client.on('connect', () => {
    console.log('已经连接成功');
    mqtt_Subscribe(topic);
  });
  
  return client;
}

// mqtt 连接 + 订阅
// mqtt 订阅
export function mqtt_Subscribe(topic){
  client.subscribe([topic],()=>{
    console.log(`订阅了主题 ${topic}`)
  });
}
```

## 使用
结合`pinia`持久化工具使用[[uniap pinia配置]]
1. pinia持久化工具的设置
```js
import {defineStore} from 'pinia'

export const myStore = defineStore('mystore',{
  state: () => {
    return{
      count: 0,
      client: null, // mqtt客户端
      data: new Map() // 使用 Map 存储数据
    }
  },
  
  actions: {
    
    setCallback() {
      // 当客户端收到一个发布过来的消息时触发回调
      this.client.on('message', (topic, message, packet) => {
		  // 使用 Pinia store 更新 sensorData
		  this.data = message;
		  // 产生更新
		  this.count++;
		  this.count=0;
      });
    },
  }
  
})
```

2. 在主页面进行mqtt连接

```js

import {
mqtt_Connect,
} from '@/utils/mqtt_config.js';

import { myStore } from '@/store/myStore'; // 导入持久化工具
// 登录成功获取订阅链接
this.mqttSubscribeTopic = res.data;

// 实例持久化
const mystore = myStore();

// 连接 + 订阅 
const client = mqtt_Connect(this.mqttSubscribeTopic+'/#');
  
mystore.client = client;

mystore.setCallback(); // 设置回调函数
```

3.  在其他页面进行接收信息
借助`pinia`的数据检测功能来获取数据
```js
// 实例持久化
const mystore = myStore();

mystore.$subscribe(async (mutation, state) => {
	
	console.log(this.data);
});
```