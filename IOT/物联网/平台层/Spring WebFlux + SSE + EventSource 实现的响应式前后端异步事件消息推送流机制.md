## 介绍
服务器发送事件（SSE） 是一种从服务器向客户端推送数据的技术，属于 HTML5 的一部分。与传统的 HTTP 请求-响应模型不同，SSE 是单向的，服务器可以持续不断地向客户端发送数据，而客户端通过一次长连接持续接收这些更新。

相比 WebSocket，SSE 有以下特点：

* 单向通信：SSE 仅允许服务器向客户端推送数据，客户端无法向服务器发送数据。
* 基于 HTTP 协议：SSE 是建立在 HTTP 协议之上的，浏览器原生支持，不需要额外的协议处理。
* 自动重连：SSE 支持自动重连，当连接意外断开时，客户端会自动尝试重新连接服务器。

Spring Boot 3 提供了对响应式编程的全面支持，基于 Project Reactor 实现异步、非阻塞的流式数据处理。而响应式编程非常适合实现 SSE，因为它允许我们以非阻塞的方式持续推送数据，而不会阻塞服务器的资源。

Spring WebFlux 是 Spring Boot 3 中用于构建响应式应用的核心框架，它可以无缝集成 SSE，为我们提供简单高效的服务器推送功能。

传统的阻塞式编程在处理长连接（如 SSE）时可能会占用大量服务器资源。响应式编程通过非阻塞 I/O 操作，不仅可以高效处理长时间的连接，还能在有新数据时立即推送给客户端。响应式流（如 `Flux`）天然适合于这种流式数据推送场景。

## 后端SSE控制器的实现
* 引入依赖
```yml
<dependencies>
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-webflux</artifactId>
    </dependency>
</dependencies>
```

* 推送 SSE 事件流的`SSE Controller`
```java
@RestController  
@RequestMapping("/sse")  
public class SSEController {  
  
    @Autowired  
    private DeviceService deviceService;  
  
    @GetMapping("/stream")  
    public Flux<ServerSentEvent<Result<List<DeviceDataDTO>>>> flushDeviceData(Integer roomID, String deviceTypeID){  
  
        return Flux.interval(Duration.ofSeconds(10)) // 每 10 秒触发一次事件  
                .flatMap(sequence -> {  
                    // 查询数据库，获取设备数据列表  
                    List<DeviceDataDTO> deviceDataDTOList = deviceService.getDeviceData(roomID, deviceTypeID);  
  
                    // 返回 Flux，封装整个设备数据列表并发送给前端  
                    return Flux.just(ServerSentEvent.<Result<List<DeviceDataDTO>>>builder()  
                            .id(String.valueOf(sequence)) // 设置事件的唯一 ID  
                            .event("device-data-event") // 设置事件类型 
                            .data(Result.success(deviceDataDTOList)) // 发送设备数据列表  
                            .build());  
                });  
    }  
}
```

参数解读：
* `interval`：WebFlux 提供的一个方法，用于每隔一定时间`Duration.ofSeconds`（单位：秒)发布一个事件`event`
* `flatMap`：用来将每个事件(即seuqence指向的内容)映射为一个新的异步流。
* `sequence`：是 `Flux.interval()` 发出的数字，代表当前事件的序号。
* `just`：用于创建一个包含单一元素的流。
* `ServerSentEvent`：SSE 的核心对象，表示一个要发送到客户端的事件。每个事件都有一个 ID、类型、数据等属性。
* `data`：数据的内容，类型需要与`ServerSentEvent`内的参数一致。


工作流程：
1. 客户端发起 GET 请求 `/stream?..`。
2. 服务端收到请求后，每隔一段时间通过 `Flux.interval()` 触发一次事件。
3. 每次事件触发时，获取对应数据，并将其包装成 `ServerSentEvent`。
4. 事件通过 SSE 发送给客户端。
5. 客户端持续接收这些事件，并可以根据接收到的数据进行相应的处理。

## 前端EventSource的实现
`Pinia`的使用：[[Pinia]]，在`Pinia`的方法中使用`EventSource`实现`SSE`的连接和事件流的监听，并且订阅`Pinia`的`state`自动检测数据的变化。

`store`：
```js
import { defineStore } from "pinia";
import { getDeviceType } from "../api/device.js";

export const deviceStore = defineStore("device", {

  state: () => {

    return {

      count: 0,

      deviceDataList: [],

      eventSource: null,

    };

  },

  actions: {

    flushDeviceData(roomID, deviceTypeID) {
	// 当eventSource已经连接时，断开连接，避免重复调用
      if (this.eventSource) { 

        this.eventSource.close();

        console.log("EventSource closed before creating a new one.");

        this.eventSource = null;

      }
  

      // 创建一个新的 EventSource 实例，连接到后端的 SSE 流

      this.eventSource = new EventSource(

        `/sse/stream?roomID=${roomID}&deviceTypeID=${deviceTypeID}`

      );
  

      // 监听具体的事件

      eventSource.addEventListener("device-data-event", async (e) => {

        this.deviceDataList = JSON.parse(e.data).data;

  

        for (const item of this.deviceDataList) {

          const res = await getDeviceType(deviceTypeID, item.valueTypeID);

          // 为每个 item 添加 deviceType

          item.deviceType = res.data;

        }

        // 产生变化 可以调整来达到限流的效果

        this.count++;

        this.count = 0;

      });

      // 监听错误事件

      eventSource.onerror = function (error) {

        console.error("SSE connection error: ", error);

        eventSource.close(); // 关闭连接

      };

    },

  },

});
```

在`addEventListener`中需要根据后端的事件定义来填写，如果里面需要进行同步异步操作，需要`await`和`async`。

不仅可以在后端去设置`Duration.ofSeconds`的连接限流，也可以设置`this.count`更新逻辑，进行订阅的更新限流。

`EventSource`中的连接前缀默认以本地的地址，需要代理的设置中设置：
```js
"/sse": {

// 这里为 /sse 配置代理

target: "..., // 你的后端地址

changeOrigin: true, // 修改源

secure: false, // 忽略证书错误

},
```


在外部使用`Pinia`的`subscribe`订阅功能检测`deviceDataList`的变化：
```js
import { deviceStore } from "@/store/device.js";
const devicestore = deviceStore();

devicestore.$subscribe(async (mutation, state) => {

  deviceDataList.value = state.deviceDataList;

});
```

## fetchEventSource
`fetch-event-source`是微软开发的一个专门用于处理双向SSE数据流的npm库，提供一个遵循 WHATWG Fetch 标准的 API 来处理 SSE，不但允许我们可以和对应的url地址简历持久连接，并且允许我们在接收数据流信息的同时将我们想要发送的消息也通过相同的url进行发送。
### 安装
```bash
npm install --save @microsoft/fetch-event-source
```

### 使用案例
```js
import {defineStore} from 'pinia'  
import {getValue, postValue} from '@/api/soildApi.js'  
import {ref} from 'vue'  
import {fetchEventSource} from '@microsoft/fetch-event-source';  
  
  
export const useSoildStore = defineStore('soild', () => {  
    const value = ref({  
        'humidity': 0,  
        'water': 0,  
        'waterThread': 0  
    })  
  
    // 保存SSE控制器  
    let sseControllers_humidity = null;  
  
    // 获取湿度  
    async function getHumidity(fid) {  
        const res = await getValue('Humidity', fid);  
        if (res.code === 0) {  
            value.value.humidity = res.data.data;  
  
        }  
    }  
  
    // SSE  
    async function getHumidityWithSSE(fid){  
        // 先关闭现有的连接  
        if(sseControllers_humidity) {  
            sseControllers_humidity.abort();  
        }  
  
        sseControllers_humidity = new AbortController();  
  
        await fetchEventSource(`/sse/soild/getHumidity?fid=${fid}`,{  
            signal: sseControllers_humidity.signal,  
            async onopen(response) {  
                console.log('onopen',response);  
            },  
            onmessage(msg) {  
                const res = JSON.parse(msg.data);  
                if(res.code === 0){  
                    value.value.humidity = res.data.data;  
                }  
            },  
            onclose() {  
                console.log('onclose');  
            },  
            onerror(err) {  
                sseControllers_humidity.abort();  
                console.log('onerror', err);  
            }  
        });  
    }  
  
  
    // 获取水控  
    async function getWater(fieldClass) {  
        const res = await getValue('Water', fieldClass);  
        value.value.water = res.data;  
    }  
  
    // 获取湿度阈值  
    async function getWaterThread(fieldClass) {  
        const res = await getValue('WaterThread', fieldClass);  
        value.value.waterThread = res.data;  
    }  
  
    // 上传水控  
    async function Soild_postValue(valueClass, fid, newValue) {  
        const res = await postValue(valueClass, fid, newValue);  
        if (res.code === 0) {  
            console.log("修改控制器成功：", newValue);  
            if (valueClass === "Water") {  
                value.value.water = newValue;  
            }  
        }  
    }  
  
  
  
    return {  
        value,  
        getHumidity,  
        getWater,  
        getWaterThread,  
        Soild_postValue,  
        getHumidityWithSSE  
    };  
})
```
* 可以通过设置signal信号来控制数据流停止传输。