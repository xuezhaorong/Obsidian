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
                            .data(Result.success(deviceDataDTOList)) // 发送设备数据列表  
                            .build());  
                });  
    }  
}
```

参数解读：
* `just`：