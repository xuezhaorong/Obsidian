Freerots9.0 链接：https://wwl.lanzn.com/iOzj21s0z7kh
主体文件链接：https://wwl.lanzn.com/izHz51s0bmkd
模板链接：https://wwl.lanzn.com/ioGQU224uvwh

1.   创建项目和打开 STM32CubeMX 与普通的流程[[STM32标准库配置]]一致
2.   在 STM32CubeMX 中设置 Freerots

![image](https://cdn.jsdelivr.net/gh/xuezhaorong/Picgo//Source/202405111837054.png)

3.   为 SYS 设置一个不常用的 TIM

![img](https://cdn.jsdelivr.net/gh/xuezhaorong/Picgo//Source/202405111845841.png)

4.   之后的设置和生成代码以及删除两个文件与普通的流程一致

5.   删除 CMSIS_RTOS_V2 文件夹

![image](https://cdn.jsdelivr.net/gh/xuezhaorong/Picgo//Source/314535738-ed417684-2b47-4a42-ac91-9dee4867cc84.png)

6.   同时删除 CMakeList 中对应的目录

![](https://cdn.jsdelivr.net/gh/xuezhaorong/Picgo//Source/314543738-ed62c4e7-21f4-4406-bfbc-74e40745540b.png)

7.   在 FreeRTOSv9.0.0\FreeRTOS\Demo\CORTEX_STM32F103_Keil 下的 FreeRTOSConfig 粘贴到 include 目录下

![image](https://cdn.jsdelivr.net/gh/xuezhaorong/Picgo//Source/202405111958380.png)

8.   在底部加入

```c
#define vPortSVCHandler SVC_Handler
#define xPortPendSVHandler PendSV_Handler
#define xPortSysTickHandler SysTick_Handler
```

![image](https://cdn.jsdelivr.net/gh/xuezhaorong/Picgo//Source/202405111900034.png)

9.   按照普通流程导入 5 个主体文件

![image](https://cdn.jsdelivr.net/gh/xuezhaorong/Picgo//Source/202405111901048.png)

10.   注释掉 stm32f10x_it.c 中的三个函数

![image](https://cdn.jsdelivr.net/gh/xuezhaorong/Picgo//Source/202405111901805.png)
