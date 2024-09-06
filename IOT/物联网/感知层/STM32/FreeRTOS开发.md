## 任务
### 单任务创建
任务是一个死循环函数
```c
static void LED_Task(void *parameter){  
    while (1){  
        LED_ON;  
        vTaskDelay(500);  
  
        LED_OFF  
        vTaskDelay(500);  
  
    }  
}
```
任务中的延时函数必须使用FreeRTOS提供的延时函数`vTaskDelay()`，使用该阻塞函数会将任务挂起，调度器会切换到其他就绪的任务，实现多任务。

FreeRTOS在SRAM中定义一个堆供动态内存分配函数使用。
在