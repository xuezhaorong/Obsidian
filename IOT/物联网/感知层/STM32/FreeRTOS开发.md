根据[[STM32标准库FreeRtos配置]]配置Clion项目
## 任务
### 任务创建
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
在`FreeRTOSConfig.h`设置堆的大小，堆的大熊啊不能超过内部`SRAM`的总大小
```c
#define configTOTAL_HEAP_SIZE      ( ( size_t ) ( 17 * 1024 ) )
```

任务创建时分配内存空间并创建任务控制块，任务创建函数会返回一个指针，用于指向任务控制块，要预先为任务栈定义一个任务控制块指针，也就是任务句柄。
```c
// 任务句柄  
static TaskHandle_t AppTaskCreate_Handle = NULL;  
static TaskHandle_t LED_Task_Handle = NULL;
```

创建任务：
```c
BaseType_t xTaskCreate( TaskFunction_t pxTaskCode,  
                   const char * const pcName,    /*lint !e971 Unqualified char types are allowed for strings and single characters only. */  
                   const configSTACK_DEPTH_TYPE usStackDepth,  
                   void * const pvParameters,  
                   UBaseType_t uxPriority,  
                   TaskHandle_t * const pxCreatedTask )
```

`xTaskCreate`函数参数说明：
* `pxTaskCode`：任务入口函数，即任务函数的名称
* `pcName`：任务名称，为字符串形式，最大长度由`FreeRTOSConfig.h`中定义的`configMAX_TASK_NAME_LEN`宏指定，多余部分会被自动截掉
* `usStackDepth`：任务栈大小，单位为字，在32位处理器下，一个字等于4字节
* `pvParameters`：任务入口形参，不用时配置为0或者NULL即可
* `uxPriority`：任务的优先级，数值越大优先级越高，范围由`FreeRTOSConfig.h`中的宏`configMAX_PRIORITIES`定义
* `pxCreatedTask`：任务控制块指针，在使用动态内存时，任务创建后会返回一个指向任务控制块的指针，该任务控制块是动态分配的一块内存

启动任务：
```c
if (xReturn == pdPASS)  
    vTaskStartScheduler(); // 开启调度器  
else  
    return -1;
```

当任务创建好后，任务处于就绪态，使用`vTaskStartScheduler`函数开启任务调度器，将任务由FreeRTOS管理

示例代码：
```c
#include  "stm32f10x.h"  
#include "FreeRTOS.h"  
#include "task.h"  
  
// 全局变量声明  
// 任务句柄  
static TaskHandle_t AppTaskCreate_Handle = NULL;  
static TaskHandle_t LED_Task_Handle = NULL;  
  
// 任务主体函数  
// 用于创建任务  
static void AppTaskCreate(void);  
static void LED_Task(void *parameter);  
  
  
int main(void){  
  
    LED_Init();  
  
    BaseType_t xReturn = pdPASS;  
  
    xReturn = xTaskCreate((TaskFunction_t) AppTaskCreate,  
                          (const char*)"AppTaskCreate",  
                          (uint16_t)512,  
                          (void* )NULL,  
                          (UBaseType_t)1,  
                          (TaskHandle_t *)&AppTaskCreate_Handle);  
  
    if (xReturn == pdPASS)  
        vTaskStartScheduler(); // 开启调度器  
    else  
        return -1;  
  
    while (1)  
    {  
        /* code */  
    }  
    return 0;  
}  
  
static void AppTaskCreate(void){  
  
    BaseType_t xReturn = pdPASS;  
  
    taskENTER_CRITICAL(); // 进入临界区  
  
    xReturn = xTaskCreate((TaskFunction_t) LED_Task,  
                                  (const char*)"LED_Task",  
                                  (uint16_t)512,  
                                  (void* )NULL,  
                                  (UBaseType_t)2,  
                                  (TaskHandle_t *)&LED_Task_Handle);  
  
//    if (pdPASS == xReturn) // 创建成功  
  
    vTaskDelete(AppTaskCreate_Handle); // 删除AppTaskCreate任务  
  
    taskEXIT_CRITICAL(); // 退出临界区  
}  
  
// 任务主体函数  
static void LED_Task(void *parameter){  
    while (1){  
        LED_ON;  
        vTaskDelay(500);  
  
        LED_OFF  
        vTaskDelay(500);  
  
    }  
}
```

