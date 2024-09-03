## STM32 简介

-   STM32 是 ST 公司基于 ARM Cortex-M 内核开发的 32 位微控制器，目前有 4 个系列：高性能，主流系列，超低功耗系列和无线系列。


![](https://cdn.jsdelivr.net/gh/xuezhaorong/Picgo//Source/fix-dir/STM32/%E5%BA%93%E5%87%BD%E6%95%B0%E5%BC%80%E5%8F%91/2024/05/12/19-35-24-e5615ab0a7d3e0c493694110c4ea78b5-STM%E7%BB%84%E6%88%90-93da53.png)



-   ARM 处理器内核有三大型号：A 系列 (Application)，主要用于手机领域，R 系列 (RealTime), 应用于实时性较高的任务，M 系列 (Microcontroller)，应用于单片机领域，


![](https://cdn.jsdelivr.net/gh/xuezhaorong/Picgo//Source/fix-dir/STM32/%E5%BA%93%E5%87%BD%E6%95%B0%E5%BC%80%E5%8F%91/2024/05/12/19-37-53-892ef021fda4810d60605ad26053d784-ARM%E5%86%85%E6%A0%B8%E5%9E%8B%E5%8F%B7-36584d.png)



-   外设资源，包括嵌套向量中断控制器和系统滴答定时器这两个内核外设和其他外设。



![](https://cdn.jsdelivr.net/gh/xuezhaorong/Picgo//Source/fix-dir/STM32/%E5%BA%93%E5%87%BD%E6%95%B0%E5%BC%80%E5%8F%91/2024/05/12/19-38-40-aec3b10bc10c21b51a37922522ed2091-%E5%A4%96%E8%AE%BE%E8%B5%84%E6%BA%90-b83f67.png)



 NVIC 是内核用于管理中断的设备，SysTick 主要用用于给操作系统提供定时服务，RCC 用于对系统的时钟进行配置，还可以使能各模块的时钟，GPIO 为通用的 IO 口，AFIO 为复用 IO 口，可以完成复用功能端口的重定义，还有中断端口的配置，EXTI 为外部中断，配置好外部中断后，当引脚有电平变化时，就可以触发中断，让 CPU 处理任务，TIM 是定时器，分为高级定时器，通用定时器，基本定时器三种类型，可以完成定时中断，测频率，生成 PWM 波形，配置成专用的编码器接口， ADC 模数转换器，内置了 12 位的 AD 转换器，可以直接读取 IO 口的模拟电压值，无需外接 AD 芯片，DMA 直接内存访问，可以帮助 CPU 完成搬运大量数据的繁杂任务，USART 同步异步串口通信，I2C，SPI 通信外设，可以用硬件输出时序波形，RTC 实时时钟，在 STM32 中完成年月日，时分秒的计时功能，可以外接备用电池，在掉电时也能正常运行，CRC 校验，可自动完成 CRC 校验，PWR 电源控制，可以让芯片进入睡眠模式等状态，BKP 备份寄存器，当系统掉电时，仍可由备用电池保持数据，IWDG，WWDG 当单片机因为电磁干扰死机或程序设计不合理出现死循环时，看门狗可以及时复位芯片，保证系统的稳定，DAC 数模转换器，可以在 IO 口直接输出模拟电压，SDIO 可以用来读取 SD 卡，FSMC，可变静态存储控制器，可以用于扩展内存，USB OTG 可以让 STM32 作为 USB 主机去读取其他 USB 设备。

-   产品功能和外设配置



![](https://cdn.jsdelivr.net/gh/xuezhaorong/Picgo//Source/fix-dir/STM32/%E5%BA%93%E5%87%BD%E6%95%B0%E5%BC%80%E5%8F%91/2024/05/12/19-40-12-f2b428967ab57293168e649d21133e33-STM32F103xx%E4%BA%A7%E5%93%81%E5%8A%9F%E8%83%BD%E5%92%8C%E5%A4%96%E8%AE%BE%E9%85%8D%E7%BD%AE-b29b85.png)



-   命名规则



![](https://cdn.jsdelivr.net/gh/xuezhaorong/Picgo//Source/fix-dir/STM32/%E5%BA%93%E5%87%BD%E6%95%B0%E5%BC%80%E5%8F%91/2024/05/12/19-41-11-b26a8f057a31efc04525967d9dfaba09-STM32%E5%91%BD%E5%90%8D%E8%A7%84%E5%88%99-134d70.png)



-   STM32 的最小系统电路



![](https://cdn.jsdelivr.net/gh/xuezhaorong/Picgo//Source/fix-dir/STM32/%E5%BA%93%E5%87%BD%E6%95%B0%E5%BC%80%E5%8F%91/2024/05/12/19-42-22-365b059ac9a70f83e996b8a55126199a-%E6%9C%80%E5%B0%8F%E5%B7%A5%E4%BD%9C%E7%94%B5%E8%B7%AF-c1ca7a.png)



## 单片机内部结构

### 系统构架

 在小容量、中容量和 大容量产品中，主系统由以下部分构成：

-   四个驱动单元： ─ Cortex™-M3 内核 DCode 总线 (D-bus)，和系统总线 (S-bus) ─ 通用 DMA1 和通用 DMA2
-   四个被动单元 ─ 内部 SRAM ─ 内部闪存存储器 ─ FSMC ─ AHB 到 APB 的桥 (AHB2APBx)，它连接所有的 APB 设备 25/754
-   这些都是通过一个多级的 AHB 总线构架相互连接的



![](https://cdn.jsdelivr.net/gh/xuezhaorong/Picgo//Source/fix-dir/STM32/%E5%BA%93%E5%87%BD%E6%95%B0%E5%BC%80%E5%8F%91/2024/05/12/19-44-17-46c7fa6534fe012519d87a3491bf046e-%E7%B3%BB%E7%BB%9F%E7%BB%93%E6%9E%84-8d423d.png)



-   ICode 总线 该总线将 Cortex™-M3 内核的指令总线与闪存指令接口相连接。指令预取在此总线上完成。
-   DCode 总线 该总线将 Cortex™-M3 内核的 DCode 总线与闪存存储器的数据接口相连接 (常量加载和调试访 问)。 系统总线 此总线连接 Cortex™-M3 内核的系统总线 (外设总线) 到总线矩阵，总线矩阵协调着内核和 DMA 间 的访问。
-   DMA 总线 此总线将 DMA 的 AHB 主控接口与总线矩阵相联，总线矩阵协调着 CPU 的 DCode 和 DMA 到 SRAM、闪存和外设的访问。
-   总线矩阵 总线矩阵协调内核系统总线和 DMA 主控总线之间的访问仲裁，仲裁利用轮换算法。在互联型产 品中，总线矩阵包含 5 个驱动部件 (CPU 的 DCode、系统总线、以太网 DMA、DMA1 总线和 DMA2 总线) 和 3 个从部件 (闪存存储器接口 (FLITF)、SRAM 和 AHB2APB 桥)。在其它产品中总线 矩阵包含 4 个驱动部件 (CPU 的 DCode、系统总线、DMA1 总线和 DMA2 总线) 和 4 个被动部件 (闪存 存储器接口 (FLITF)、SRAM、FSMC 和 AHB2APB 桥)。 AHB 外设通过总线矩阵与系统总线相连，允许 DMA 访问。
-   AHB/APB 桥 (APB) 两个 AHB/APB 桥在 AHB 和 2 个 APB 总线间提供同步连接。APB1 操作速度限于 36MHz，APB2 操 作于全速 (最高 72MHz)。 **在每一次复位以后，所有除 SRAM 和 FLITF 以外的外设都被关闭，在使用一个外设之前，必须设置寄存器 RCC_AHBENR 来打开该外设的时钟**。 注意： 当对 APB 寄存器进行 8 位或者 16 位访问时，该访问会被自动转换成 32 位的访问：桥会自动将 8 位 或者 32 位的数据扩展以配合 32 位的向量

 内核引出三条总线，分别是 ICode 指令总线，DCode 数据总线，System 系统总线，ICode 总线和 DCOde 总线主要用来链接 Flash 闪存，Flash 闪存存储编写的程序，ICode 指令总线用来加载程序指令，DCOde 数据总线用来加载数据，System 总线接到其他地方，AHB 先进高性能总线用于挂载最基本或性能比较高的外设，如复位和时钟控制，SDIO，两个桥接接到了 APB2 和 APB1 两个先进外设总线上，因为 AHB 和 APB 的总线协议，总线速度，还有数据传送格式的差异，所以中间需要加两个桥接，来完成数据的转换和缓存，DMA 帮助 CPU 搬运数据，通过 DMA 总线连接到总线矩阵上面，可以拥有和 CPU 一样的总线控制权，用于访问外设，当 DMA 搬运数据时，外设就会通过请求线发送 DMA 请求，然后 DMA 就会获得总线控制权，访问并转运数据。

### 复位和时钟控制 (RCC)

#### 时钟



![](https://cdn.jsdelivr.net/gh/xuezhaorong/Picgo//Source/fix-dir/STM32/%E5%BA%93%E5%87%BD%E6%95%B0%E5%BC%80%E5%8F%91/2024/05/12/19-51-16-9bf21223f7e4da68e27821534b2cf92a-%E6%97%B6%E9%92%9F%E6%A0%91-bf0dcc.png)



##### 外设时钟使能寄存器

 **当外设时钟没有启用时，软件不能读出外设寄存器的数值，返回的数值始终是 0x0。**

-   常用配置函数，AHB,APB2,APB1 外设桥的使能时钟开启



![](https://cdn.jsdelivr.net/gh/xuezhaorong/Picgo//Source/fix-dir/STM32/%E5%BA%93%E5%87%BD%E6%95%B0%E5%BC%80%E5%8F%91/2024/05/12/19-52-17-64296428ac273e3f1152a42c7a75507d-RCC%E5%B8%B8%E7%94%A8%E9%85%8D%E7%BD%AE-9eed98.png)



###### AHB 外设时钟使能寄存器 (RCC_AHBENR)



![](https://cdn.jsdelivr.net/gh/xuezhaorong/Picgo//Source/fix-dir/STM32/%E5%BA%93%E5%87%BD%E6%95%B0%E5%BC%80%E5%8F%91/2024/05/12/19-53-50-e20222d05f1a1dc4f27a6ef23a61f237-ABH%E5%A4%96%E8%AE%BE%E6%97%B6%E9%92%9F%E4%BD%BF%E8%83%BD%E5%AF%84%E5%AD%98%E5%99%A81-9ff77d.png)

![](https://cdn.jsdelivr.net/gh/xuezhaorong/Picgo//Source/fix-dir/STM32/%E5%BA%93%E5%87%BD%E6%95%B0%E5%BC%80%E5%8F%91/2024/05/12/19-54-15-11f459215fba7d862183fefddc0943e7-ABH%E5%A4%96%E8%AE%BE%E6%97%B6%E9%92%9F%E4%BD%BF%E8%83%BD%E5%AF%84%E5%AD%98%E5%99%A82-01bedc.png)



**相关库函数配置:**

包含外设
```
-   @arg RCC_AHBPeriph_DMA1
-   @arg RCC_AHBPeriph_DMA2
-   @arg RCC_AHBPeriph_SRAM
-   @arg RCC_AHBPeriph_FLITF
-   @arg RCC_AHBPeriph_CRC
-   @arg RCC_AHBPeriph_FSMC
-   @arg RCC_AHBPeriph_SDIO
```



```c
/**
  * @brief  Enables or disables the AHB peripheral clock.
  * @param  RCC_AHBPeriph: specifies the AHB peripheral to gates its clock.
  *   
  *   For @b STM32_Connectivity_line_devices, this parameter can be any combination
  *   of the following values:        
  *     @arg RCC_AHBPeriph_DMA1
  *     @arg RCC_AHBPeriph_DMA2
  *     @arg RCC_AHBPeriph_SRAM
  *     @arg RCC_AHBPeriph_FLITF
  *     @arg RCC_AHBPeriph_CRC
  *     @arg RCC_AHBPeriph_OTG_FS    
  *     @arg RCC_AHBPeriph_ETH_MAC   
  *     @arg RCC_AHBPeriph_ETH_MAC_Tx
  *     @arg RCC_AHBPeriph_ETH_MAC_Rx
  * 
  *   For @b other_STM32_devices, this parameter can be any combination of the 
  *   following values:        
  *     @arg RCC_AHBPeriph_DMA1
  *     @arg RCC_AHBPeriph_DMA2
  *     @arg RCC_AHBPeriph_SRAM
  *     @arg RCC_AHBPeriph_FLITF
  *     @arg RCC_AHBPeriph_CRC
  *     @arg RCC_AHBPeriph_FSMC
  *     @arg RCC_AHBPeriph_SDIO
  *   
  * @note SRAM and FLITF clock can be disabled only during sleep mode.
  * @param  NewState: new state of the specified peripheral clock.
  *   This parameter can be: ENABLE or DISABLE.
  * @retval None
  */
void RCC_AHBPeriphClockCmd(uint32_t RCC_AHBPeriph, FunctionalState NewState)

```

-   第二个参数为是否使能

```c
typedef enum {DISABLE = 0, ENABLE = !DISABLE} FunctionalState;
```

###### APB1 外设时钟使能寄存器 (RCC_APB1ENR)

![|775](https://cdn.jsdelivr.net/gh/xuezhaorong/Picgo//Source/fix-dir/STM32/%E5%BA%93%E5%87%BD%E6%95%B0%E5%BC%80%E5%8F%91/2024/05/12/19-55-40-a4243fe6af304867b77c7e8e127f8b20-APB1%E5%A4%96%E8%AE%BE%E6%97%B6%E9%92%9F%E4%BD%BF%E8%83%BD%E5%AF%84%E5%AD%98%E5%99%A81-f78966.png)

![|775](https://cdn.jsdelivr.net/gh/xuezhaorong/Picgo//Source/fix-dir/STM32/%E5%BA%93%E5%87%BD%E6%95%B0%E5%BC%80%E5%8F%91/2024/05/12/19-56-00-506307351d0bb0c243196ab7065e3463-APB1%E5%A4%96%E8%AE%BE%E6%97%B6%E9%92%9F%E4%BD%BF%E8%83%BD%E5%AF%84%E5%AD%98%E5%99%A82-18cbe3.png)

![|775](https://cdn.jsdelivr.net/gh/xuezhaorong/Picgo//Source/fix-dir/STM32/%E5%BA%93%E5%87%BD%E6%95%B0%E5%BC%80%E5%8F%91/2024/05/12/19-56-26-537188770d95593c9b87bf0b120cfdee-APB1%E5%A4%96%E8%AE%BE%E6%97%B6%E9%92%9F%E4%BD%BF%E8%83%BD%E5%AF%84%E5%AD%98%E5%99%A83-884adb.png)

![|825](https://cdn.jsdelivr.net/gh/xuezhaorong/Picgo//Source/fix-dir/STM32/%E5%BA%93%E5%87%BD%E6%95%B0%E5%BC%80%E5%8F%91/2024/05/12/19-56-54-a40b09691c975635bdfb9a8d6e2ff11d-APB1%E5%A4%96%E8%AE%BE%E6%97%B6%E9%92%9F%E4%BD%BF%E8%83%BD%E5%AF%84%E5%AD%98%E5%99%A84-961af3.png)



**相关库函数配置:**

-   函数原型，第一个参数为要开启的外设名，可在列表中寻找

```c
/**
  * @brief  Enables or disables the Low Speed APB (APB1) peripheral clock.
  * @param  RCC_APB1Periph: specifies the APB1 peripheral to gates its clock.
  *   This parameter can be any combination of the following values:
  *     @arg RCC_APB1Periph_TIM2, RCC_APB1Periph_TIM3, RCC_APB1Periph_TIM4,
  *          RCC_APB1Periph_TIM5, RCC_APB1Periph_TIM6, RCC_APB1Periph_TIM7,
  *          RCC_APB1Periph_WWDG, RCC_APB1Periph_SPI2, RCC_APB1Periph_SPI3,
  *          RCC_APB1Periph_USART2, RCC_APB1Periph_USART3, RCC_APB1Periph_USART4, 
  *          RCC_APB1Periph_USART5, RCC_APB1Periph_I2C1, RCC_APB1Periph_I2C2,
  *          RCC_APB1Periph_USB, RCC_APB1Periph_CAN1, RCC_APB1Periph_BKP,
  *          RCC_APB1Periph_PWR, RCC_APB1Periph_DAC, RCC_APB1Periph_CEC,
  *          RCC_APB1Periph_TIM12, RCC_APB1Periph_TIM13, RCC_APB1Periph_TIM14
  * @param  NewState: new state of the specified peripheral clock.
  *   This parameter can be: ENABLE or DISABLE.
  * @retval None
  */
void RCC_APB1PeriphClockCmd(uint32_t RCC_APB1Periph, FunctionalState NewState)

```

###### APB2 外设时钟使能寄存器 (RCC_APB2ENR)

![|850](https://cdn.jsdelivr.net/gh/xuezhaorong/Picgo//Source/fix-dir/STM32/%E5%BA%93%E5%87%BD%E6%95%B0%E5%BC%80%E5%8F%91/2024/05/12/19-59-05-8147424e2ce70ddb94a57c884bba229d-APB2%E5%A4%96%E8%AE%BE%E6%97%B6%E9%92%9F%E4%BD%BF%E8%83%BD%E5%AF%84%E5%AD%98%E5%99%A81-2430f1.png)



![|850](https://cdn.jsdelivr.net/gh/xuezhaorong/Picgo//Source/fix-dir/STM32/%E5%BA%93%E5%87%BD%E6%95%B0%E5%BC%80%E5%8F%91/2024/05/12/19-59-35-09e83a04553cec0296fcc4378f71a978-APB2%E5%A4%96%E8%AE%BE%E6%97%B6%E9%92%9F%E4%BD%BF%E8%83%BD%E5%AF%84%E5%AD%98%E5%99%A82-ec4763.png)

![|850](https://cdn.jsdelivr.net/gh/xuezhaorong/Picgo//Source/fix-dir/STM32/%E5%BA%93%E5%87%BD%E6%95%B0%E5%BC%80%E5%8F%91/2024/05/12/19-59-55-6880cd6465ae043b9e1b1e00672f3a8e-APB2%E5%A4%96%E8%AE%BE%E6%97%B6%E9%92%9F%E4%BD%BF%E8%83%BD%E5%AF%84%E5%AD%98%E5%99%A83-77dec7.png)



**相关库函数配置:**

-   函数原型，第一个参数为要开启的外设名，可在列表中寻找

```c
/**
  * @brief  Enables or disables the High Speed APB (APB2) peripheral clock.
  * @param  RCC_APB2Periph: specifies the APB2 peripheral to gates its clock.
  *   This parameter can be any combination of the following values:
  *     @arg RCC_APB2Periph_AFIO, RCC_APB2Periph_GPIOA, RCC_APB2Periph_GPIOB,
  *          RCC_APB2Periph_GPIOC, RCC_APB2Periph_GPIOD, RCC_APB2Periph_GPIOE,
  *          RCC_APB2Periph_GPIOF, RCC_APB2Periph_GPIOG, RCC_APB2Periph_ADC1,
  *          RCC_APB2Periph_ADC2, RCC_APB2Periph_TIM1, RCC_APB2Periph_SPI1,
  *          RCC_APB2Periph_TIM8, RCC_APB2Periph_USART1, RCC_APB2Periph_ADC3,
  *          RCC_APB2Periph_TIM15, RCC_APB2Periph_TIM16, RCC_APB2Periph_TIM17,
  *          RCC_APB2Periph_TIM9, RCC_APB2Periph_TIM10, RCC_APB2Periph_TIM11     
  * @param  NewState: new state of the specified peripheral clock.
  *   This parameter can be: ENABLE or DISABLE.
  * @retval None
  */
void RCC_APB2PeriphClockCmd(uint32_t RCC_APB2Periph, FunctionalState NewState)

```

-   示例配置代码

```c
RCC_APB2PeriphClockCmd(RCC_APB2Periph_GPIOA,ENABLE);  //rcc apb2外设使能时钟开启
```

## GPIO

### GPIO 的位结构



![](https://cdn.jsdelivr.net/gh/xuezhaorong/Picgo//Source/fix-dir/STM32/%E5%BA%93%E5%87%BD%E6%95%B0%E5%BC%80%E5%8F%91/2024/05/12/20-02-20-9b6af36ce1236b18c1351038626224b6-GPIO%E4%BD%8D%E7%BB%93%E6%9E%84-f17419.png)



 IO 引脚接入了两个保护二极管对输入电压进行限幅，上方保护二极管接 3.3V 电压，若输入电压大于 3.3V, 则上方二极管导通，电流直接流入 VDD 而不会流入内部电路，下方保护二极管接 0V 电压，若输入电压小于 0V, 则下方二极管导通，电流直接流入 VSS 而不会从内部电路汲取电流。若输入电压在 0~3.3V 之间，而两个二极管均不会导通，对电路不会造成影响。

 施密特触发器对电压进行整型，如果输入电压大于某阈值，输出会瞬间升为高电平，如果输入电压小于某阈值，输出电压会瞬间降为低电平，可以对输入电压进行整型将失真的模拟信号转为数字信号，有效避免了因信号波动造成的输出抖动现象。

### GPIO 通用输出输入模式

 GPIO 端口的每个位可以由软件分别配置成多种模式

![](https://cdn.jsdelivr.net/gh/xuezhaorong/Picgo//Source/fix-dir/STM32/%E5%BA%93%E5%87%BD%E6%95%B0%E5%BC%80%E5%8F%91/2024/05/12/20-02-40-27fa0e90c16aaa5823f35b13e65350ee-GPIO-1091f0.png)



#### 输入配置

当 I/O 端口配置为输入时：

-   输出缓冲器被禁止
-   施密特触发输入被激活
-   根据输入配置 (上拉，下拉或浮动) 的不同，弱上拉和下拉电阻被连接
-   出现在 I/O 脚上的数据在每个 APB2 时钟被采样到输入数据寄存器
-   对输入数据寄存器的读访问可得到 I/O 状态



![](https://cdn.jsdelivr.net/gh/xuezhaorong/Picgo//Source/fix-dir/STM32/%E5%BA%93%E5%87%BD%E6%95%B0%E5%BC%80%E5%8F%91/2024/05/12/20-03-08-9b6af36ce1236b18c1351038626224b6-GPIO%E4%BD%8D%E7%BB%93%E6%9E%84-fe8b1d.png)



 当上拉，下拉控制开关都不拉的时候，输入就会处于一种浮空的状态，引脚的输入电平极易受外界干扰而改变，导致输入数据不确定。若接入上拉电阻，当引脚悬空时，上拉电阻可以保证引脚的高电平，下拉同理。

#### 模拟输入配置

当 I/O 端口被配置为模拟输入配置时：

-   输出缓冲器被禁止
-   禁止施密特触发输入，实现了每个模拟 I/O 引脚上的零消耗。施密特触发输出值被强置 为’0’
-   弱上拉和下拉电阻被禁止
-   读取输入数据寄存器时数值为’0′

![](https://cdn.jsdelivr.net/gh/xuezhaorong/Picgo//Source/fix-dir/STM32/%E5%BA%93%E5%87%BD%E6%95%B0%E5%BC%80%E5%8F%91/2024/05/12/20-03-37-2dae07769f7e4c37e47f6fc207383496-GPIO%E6%A8%A1%E6%8B%9F%E8%BE%93%E5%85%A5-675600.png)



 模拟输入通常接在 ADC 上，ADC 需要接收模拟量，所以不需要施密特触发器进行电压整型将模拟量转为数字量。

#### 输出配置

当 I/O 端口被配置为输出时：

-   输出缓冲器被激活
-   施密特触发输入被激活
-   弱上拉和下拉电阻被禁止
-   出现在 I/O 脚上的数据在每个 APB2 时钟被采样到输入数据寄存器
-   在开漏模式时，对输入数据寄存器的读访问可得到 I/O 状态
-   在推挽式模式时，对输出数据寄存器的读访问得到最后一次写的值

![](https://cdn.jsdelivr.net/gh/xuezhaorong/Picgo//Source/fix-dir/STM32/%E5%BA%93%E5%87%BD%E6%95%B0%E5%BC%80%E5%8F%91/2024/05/12/20-05-05-8e537e9322fc7450f5d8412aa8206ac2-GPIO%E8%BE%93%E5%87%BA-fa89f7.png)



 开漏模式：输出寄存器上的’0’激活 N-MOS，而输出寄存器上的’1’将端口置于高阻状态 (PMOS 从不被激活), 只有低电平有驱动能力，高电平无驱动能力。 推挽模式：输出寄存器上的’0’激活 N-MOS，而输出寄存器上的’1’将激活 P-MOS, 高低电平均有较强的驱动能力。

#### 相关配置寄存器

-   常用库函数

![](https://cdn.jsdelivr.net/gh/xuezhaorong/Picgo//Source/fix-dir/STM32/%E5%BA%93%E5%87%BD%E6%95%B0%E5%BC%80%E5%8F%91/2024/05/12/20-05-34-5d279a9e8dd6a7207658fec489ce9ee3-GPIO%E5%B8%B8%E7%94%A8%E5%BA%93%E5%87%BD%E6%95%B0-dc311d.png)



##### 端口配置寄存器

 端口配置寄存器用于配置 GPIO 口的 8 个输出输入模式以及输出速度，限制了 GPIO 口电压的翻转速度

![](https://cdn.jsdelivr.net/gh/xuezhaorong/Picgo//Source/fix-dir/STM32/%E5%BA%93%E5%87%BD%E6%95%B0%E5%BC%80%E5%8F%91/2024/05/12/20-06-14-39a517f6cde6541fb56c5eaaef371913-%E7%AB%AF%E5%8F%A3%E9%85%8D%E7%BD%AE%E4%BD%8E%E5%AF%84%E5%AD%98%E5%99%A81-19b9dd.png)

![](https://cdn.jsdelivr.net/gh/xuezhaorong/Picgo//Source/fix-dir/STM32/%E5%BA%93%E5%87%BD%E6%95%B0%E5%BC%80%E5%8F%91/2024/05/12/20-06-37-cb2affce90f4ba16f7ce421ec66896cc-%E7%AB%AF%E5%8F%A3%E9%85%8D%E7%BD%AE%E4%BD%8E%E5%AF%84%E5%AD%98%E5%99%A82-90895f.png)

![](https://cdn.jsdelivr.net/gh/xuezhaorong/Picgo//Source/fix-dir/STM32/%E5%BA%93%E5%87%BD%E6%95%B0%E5%BC%80%E5%8F%91/2024/05/12/20-06-59-682765ed1239e9218e19d268811b2b25-%E7%AB%AF%E5%8F%A3%E9%85%8D%E7%BD%AE%E9%AB%98%E5%AF%84%E5%AD%98%E5%99%A81-c44f10.png)

![](https://cdn.jsdelivr.net/gh/xuezhaorong/Picgo//Source/fix-dir/STM32/%E5%BA%93%E5%87%BD%E6%95%B0%E5%BC%80%E5%8F%91/2024/05/12/20-07-29-2804d9aefaa383dcb4204d5d3b1978a6-%E7%AB%AF%E5%8F%A3%E9%85%8D%E7%BD%AE%E9%AB%98%E5%AF%84%E5%AD%98%E5%99%A82-61af93.png)



**相关库函数配置:**

-   **GPIO_Init** 函数原型:

```c
/**
  * @brief  Initializes the GPIOx peripheral according to the specified
  *         parameters in the GPIO_InitStruct.
  * @param  GPIOx: where x can be (A..G) to select the GPIO peripheral.
  * @param  GPIO_InitStruct: pointer to a GPIO_InitTypeDef structure that
  *         contains the configuration information for the specified GPIO peripheral.
  * @retval None
  */
void GPIO_Init(GPIO_TypeDef* GPIOx, GPIO_InitTypeDef* GPIO_InitStruct)

```

-   GPIOx 中的 x 可以选择 A 到 G 对应的 GPIO 类端口口

```c
#define GPIOA               ((GPIO_TypeDef *) GPIOA_BASE)
#define GPIOB               ((GPIO_TypeDef *) GPIOB_BASE)
#define GPIOC               ((GPIO_TypeDef *) GPIOC_BASE)
#define GPIOD               ((GPIO_TypeDef *) GPIOD_BASE)
#define GPIOE               ((GPIO_TypeDef *) GPIOE_BASE)
#define GPIOF               ((GPIO_TypeDef *) GPIOF_BASE)
#define GPIOG               ((GPIO_TypeDef *) GPIOG_BASE)
```

-   GPIO_Init 通过 GPIO_InitTypeDef 结构体进行初始化设置

```c
typedef struct
{
  uint16_t GPIO_Pin;             /*!< Specifies the GPIO pins to be configured.
                                      This parameter can be any value of @ref GPIO_pins_define */
 
  GPIOSpeed_TypeDef GPIO_Speed;  /*!< Specifies the speed for the selected pins.
                                      This parameter can be a value of @ref GPIOSpeed_TypeDef */
 
  GPIOMode_TypeDef GPIO_Mode;    /*!< Specifies the operating mode for the selected pins.
                                      This parameter can be a value of @ref GPIOMode_TypeDef */
}GPIO_InitTypeDef;
```

-   GPIO_Pin 用于设置 GPIO 每个类端口的具体端口，对应 0~15 口，或者全部选择

```c
/** @defgroup GPIO_pins_define 
  * @{
  */
 
#define GPIO_Pin_0                 ((uint16_t)0x0001)  /*!< Pin 0 selected */
#define GPIO_Pin_1                 ((uint16_t)0x0002)  /*!< Pin 1 selected */
#define GPIO_Pin_2                 ((uint16_t)0x0004)  /*!< Pin 2 selected */
#define GPIO_Pin_3                 ((uint16_t)0x0008)  /*!< Pin 3 selected */
#define GPIO_Pin_4                 ((uint16_t)0x0010)  /*!< Pin 4 selected */
#define GPIO_Pin_5                 ((uint16_t)0x0020)  /*!< Pin 5 selected */
#define GPIO_Pin_6                 ((uint16_t)0x0040)  /*!< Pin 6 selected */
#define GPIO_Pin_7                 ((uint16_t)0x0080)  /*!< Pin 7 selected */
#define GPIO_Pin_8                 ((uint16_t)0x0100)  /*!< Pin 8 selected */
#define GPIO_Pin_9                 ((uint16_t)0x0200)  /*!< Pin 9 selected */
#define GPIO_Pin_10                ((uint16_t)0x0400)  /*!< Pin 10 selected */
#define GPIO_Pin_11                ((uint16_t)0x0800)  /*!< Pin 11 selected */
#define GPIO_Pin_12                ((uint16_t)0x1000)  /*!< Pin 12 selected */
#define GPIO_Pin_13                ((uint16_t)0x2000)  /*!< Pin 13 selected */
#define GPIO_Pin_14                ((uint16_t)0x4000)  /*!< Pin 14 selected */
#define GPIO_Pin_15                ((uint16_t)0x8000)  /*!< Pin 15 selected */
#define GPIO_Pin_All               ((uint16_t)0xFFFF)  /*!< All pins selected */
```

-   GPIOSpeed_TypeDef 用于设置端口的速度，可以设置 10,2,50MHZ

```c
typedef enum
{ 
  GPIO_Speed_10MHz = 1,
  GPIO_Speed_2MHz, 
  GPIO_Speed_50MHz
}GPIOSpeed_TypeDef;
```

-   GPIOMode_TypeDef 用于设置端口的工作模式

```c
typedef enum
{ GPIO_Mode_AIN = 0x0, // 模拟输入
  GPIO_Mode_IN_FLOATING = 0x04, // 浮空输入
  GPIO_Mode_IPD = 0x28, // 下拉输入
  GPIO_Mode_IPU = 0x48, // 上拉输入
  GPIO_Mode_Out_OD = 0x14, // 开漏输出
  GPIO_Mode_Out_PP = 0x10, // 推挽输出
  GPIO_Mode_AF_OD = 0x1C, // 复用开漏输出
  GPIO_Mode_AF_PP = 0x18 // 复用推挽输出
}GPIOMode_TypeDef;
```

-   示例配置代码

```c
GPIO_InitTypeDef GPIO_InitStructure = { // GPIO_InitTypeDef的定义以及初始化
    .GPIO_Mode = GPIO_Mode_Out_PP, // 推挽输出
    .GPIO_Pin = GPIO_Pin_8, // A8引脚
    .GPIO_Speed = GPIO_Speed_50MHz //50MHZ速度
};
GPIO_Init(GPIOA, &GPIO_InitStructure); // 初始化GPIO
```

##### 端口输入数据寄存器

 可以读出输入数据寄存器的数值

![](https://cdn.jsdelivr.net/gh/xuezhaorong/Picgo//Source/fix-dir/STM32/%E5%BA%93%E5%87%BD%E6%95%B0%E5%BC%80%E5%8F%91/2024/05/12/20-07-54-0e79352149743fe933a223e0968cffd3-%E7%AB%AF%E5%8F%A3%E8%BE%93%E5%85%A5%E6%95%B0%E6%8D%AE%E5%AF%84%E5%AD%98%E5%99%A8-736e68.png)



**相关库函数配置:**

-   **GPIO_ReadInputDataBit** 函数可以读取输入寄存器某一个具体端口的值，**GPIO_ReadInputData** 函数只能读取整个类端口的值

```c
/**
  * @brief  Reads the specified input port pin.
  * @param  GPIOx: where x can be (A..G) to select the GPIO peripheral.
  * @param  GPIO_Pin:  specifies the port bit to read.
  *   This parameter can be GPIO_Pin_x where x can be (0..15).
  * @retval The input port pin value.
  */
uint8_t GPIO_ReadInputDataBit(GPIO_TypeDef* GPIOx, uint16_t GPIO_Pin)
/**
  * @brief  Reads the specified GPIO input data port.
  * @param  GPIOx: where x can be (A..G) to select the GPIO peripheral.
  * @retval GPIO input data port value.
  */
uint16_t GPIO_ReadInputData(GPIO_TypeDef* GPIOx)

```

##### 端口输出数据寄存器

 设置端口输出数据寄存器以输出数据

![](https://cdn.jsdelivr.net/gh/xuezhaorong/Picgo//Source/fix-dir/STM32/%E5%BA%93%E5%87%BD%E6%95%B0%E5%BC%80%E5%8F%91/2024/05/12/20-08-14-6410959cd62c4ba23a2dd7e5c6897a61-%E7%AB%AF%E5%8F%A3%E8%BE%93%E5%87%BA%E6%95%B0%E6%8D%AE%E5%AF%84%E5%AD%98%E5%99%A8-46beaa.png)



**相关库函数配置:**

-   **GPIO_ReadOutputDataBit** 函数可以读取输出寄存器某一个具体端口的值，**GPIO_ReadOutputData** 函数只能读取整个类端口的值

```c
/**
  * @brief  Reads the specified output data port bit.
  * @param  GPIOx: where x can be (A..G) to select the GPIO peripheral.
  * @param  GPIO_Pin:  specifies the port bit to read.
  *   This parameter can be GPIO_Pin_x where x can be (0..15).
  * @retval The output port pin value.
  */
uint8_t GPIO_ReadOutputDataBit(GPIO_TypeDef* GPIOx, uint16_t GPIO_Pin)
/**
  * @brief  Reads the specified GPIO output data port.
  * @param  GPIOx: where x can be (A..G) to select the GPIO peripheral.
  * @retval GPIO output data port value.
  */
uint16_t GPIO_ReadOutputData(GPIO_TypeDef* GPIOx)

```

-   **GPIO_WriteBit** 函数的第 3 个常数为对应的端口设置高低电平，**Bit_RESET** 为低电平，**Bit_SET** 为高电平

```c
/**
  * @brief  Sets or clears the selected data port bit.
  * @param  GPIOx: where x can be (A..G) to select the GPIO peripheral.
  * @param  GPIO_Pin: specifies the port bit to be written.
  *   This parameter can be one of GPIO_Pin_x where x can be (0..15).
  * @param  BitVal: specifies the value to be written to the selected bit.
  *   This parameter can be one of the BitAction enum values:
  *     @arg Bit_RESET: to clear the port pin
  *     @arg Bit_SET: to set the port pin
  * @retval None
  */
void GPIO_WriteBit(GPIO_TypeDef* GPIOx, uint16_t GPIO_Pin, BitAction BitVal)
/**
  * @brief  Writes data to the specified GPIO data port.
  * @param  GPIOx: where x can be (A..G) to select the GPIO peripheral.
  * @param  PortVal: specifies the value to be written to the port output data register.
  * @retval None
  */
void GPIO_Write(GPIO_TypeDef* GPIOx, uint16_t PortVal)

```

-   **BitAction** 是一个枚举值

```c
typedef enum
{ Bit_RESET = 0,
  Bit_SET
}BitAction;
```

##### 端口位清除寄存器

 给 GPIO 的对应口清 0

![](https://cdn.jsdelivr.net/gh/xuezhaorong/Picgo//Source/fix-dir/STM32/%E5%BA%93%E5%87%BD%E6%95%B0%E5%BC%80%E5%8F%91/2024/05/12/20-10-48-cc585f75b147c964d9e49e42943daee6-%E7%AB%AF%E5%8F%A3%E4%BD%8D%E6%B8%85%E9%99%A4%E5%AF%84%E5%AD%98%E5%99%A8-2fa55a.png)



##### 端口位设置清除寄存器

 高 16 位给 GPIO 对应口清 0, 低 16 位给 GPIO 对应口设置为 1

![img](https://cdn.jsdelivr.net/gh/xuezhaorong/Picgo//Source/%E7%AB%AF%E5%8F%A3%E4%BD%8D%E8%AE%BE%E7%BD%AE%E6%B8%85%E9%99%A4%E5%AF%84%E5%AD%98%E5%99%A8.png)



**相关库函数配置:**

-   设置端口

```c
/**
  * @brief  Sets the selected data port bits.
  * @param  GPIOx: where x can be (A..G) to select the GPIO peripheral.
  * @param  GPIO_Pin: specifies the port bits to be written.
  *   This parameter can be any combination of GPIO_Pin_x where x can be (0..15).
  * @retval None
  */
void GPIO_SetBits(GPIO_TypeDef* GPIOx, uint16_t GPIO_Pin)

```

-   清除端口

```c
/**
  * @brief  Clears the selected data port bits.
  * @param  GPIOx: where x can be (A..G) to select the GPIO peripheral.
  * @param  GPIO_Pin: specifies the port bits to be written.
  *   This parameter can be any combination of GPIO_Pin_x where x can be (0..15).
  * @retval None
  */
void GPIO_ResetBits(GPIO_TypeDef* GPIOx, uint16_t GPIO_Pin)

```

### GPIO 复用功能

#### 复用功能配置

当 I/O 端口被配置为复用功能时：

-   在开漏或推挽式配置中，输出缓冲器被打开
-   内置外设的信号驱动输出缓冲器 (复用功能输出)
-   施密特触发输入被激活
-   弱上拉和下拉电阻被禁止
-   在每个 APB2 时钟周期，出现在 I/O 脚上的数据被采样到输入数据寄存器
-   开漏模式时，读输入数据寄存器时可得到 I/O 口状态
-   在推挽模式时，读输出数据寄存器时可得到最后一次写的值

![](https://cdn.jsdelivr.net/gh/xuezhaorong/Picgo//Source/fix-dir/STM32/%E5%BA%93%E5%87%BD%E6%95%B0%E5%BC%80%E5%8F%91/2024/05/12/20-12-21-5da54cec70f1c6f235bc98f584cbcf6e-GPIO%E5%A4%8D%E7%94%A8%E8%BE%93%E5%87%BA-e9c9ec.png)

 在复用输出功能下，引脚的控制权转为片上外设，由片上外设控制。



## 中断系统



![](https://cdn.jsdelivr.net/gh/xuezhaorong/Picgo//Source/fix-dir/STM32/%E5%BA%93%E5%87%BD%E6%95%B0%E5%BC%80%E5%8F%91/2024/05/12/20-13-10-126145add6a9199efc461e19fc21d5da-%E4%B8%AD%E6%96%AD%E5%90%91%E9%87%8F%E8%A1%A81-e45b73.png)

![](https://cdn.jsdelivr.net/gh/xuezhaorong/Picgo//Source/fix-dir/STM32/%E5%BA%93%E5%87%BD%E6%95%B0%E5%BC%80%E5%8F%91/2024/05/12/20-13-28-5cc842a5c046147e0d90f48049170f7e-%E4%B8%AD%E6%96%AD%E5%90%91%E9%87%8F%E8%A1%A82-99e701.png)

![](https://cdn.jsdelivr.net/gh/xuezhaorong/Picgo//Source/fix-dir/STM32/%E5%BA%93%E5%87%BD%E6%95%B0%E5%BC%80%E5%8F%91/2024/05/12/20-14-00-c2fe6fec5e5b52834bf1bf116c9d1b44-%E4%B8%AD%E6%96%AD%E5%90%91%E9%87%8F%E8%A1%A83-32395b.png)

![](https://cdn.jsdelivr.net/gh/xuezhaorong/Picgo//Source/fix-dir/STM32/%E5%BA%93%E5%87%BD%E6%95%B0%E5%BC%80%E5%8F%91/2024/05/12/20-14-28-7f602086b61679d7b0c7da5bcd0c1dcd-%E4%B8%AD%E6%96%AD%E5%90%91%E9%87%8F%E8%A1%A84-3e1050.png)

### 外部中断

![](https://cdn.jsdelivr.net/gh/xuezhaorong/Picgo//Source/fix-dir/STM32/%E5%BA%93%E5%87%BD%E6%95%B0%E5%BC%80%E5%8F%91/2024/05/12/20-15-02-cd53087fad885a32854f218732ea5fe5-%E5%A4%96%E9%83%A8%E4%B8%AD%E6%96%AD%E5%9F%BA%E6%9C%AC%E7%BB%93%E6%9E%84-4dd5eb.png)



 外部的 3 个 GPIO 外设通过 AFIO 进行中断引脚选择配置，选择其中一个 Pin 引脚连接到后面的 EXTI 通道里，对于 GPIO 口所有的 Pin 不能同时触发中断。通过 AFIO 选择之后的 16 个通道，接到了 EXTI 边缘检测及控制电路上，同时外加 PVD 输出，RTC 闹钟，USB 唤醒，以太网唤醒 4 个口共同组成了 EXTI 的 20 个输入信号。输入的信号经过 EXTI 的边缘检测及控制处理之后分成两种输出，一种接到了 NVIC 中触发中断，其他 20 条接到其他外设上用来触发事件响应。

 对于互联型产品，外部中断 / 事件控制器由 20 个产生事件 / 中断请求的边沿检测器组成，对于其它 产品，则有 19 个能产生事件 / 中断请求的边沿检测器。每个输入线可以独立地配置输入类型 (脉冲 或挂起) 和对应的触发事件 (上升沿或下降沿或者双边沿都触发)。每个输入线都可以独立地被屏蔽。挂起寄存器保持着状态线的中断请求。

 使用 NVIC 统一管理中断，每个中断通道都拥有 16 个可编程的优先等级，可对优先级进行分组，进一步设置抢占优先级和响应优先级。

#### AFIO

 AFIO 主要用于引脚复用功能的选择和重定义，在 STM32 中，AFIO 主要完成两个任务：复用功能引脚重映射，中断引脚选择。

##### 复用功能引脚重映射

 此功能用于解决默认复用功能冲突的情况，将其中一个引脚功能重映射至重定义功能。

![](https://cdn.jsdelivr.net/gh/xuezhaorong/Picgo//Source/fix-dir/STM32/%E5%BA%93%E5%87%BD%E6%95%B0%E5%BC%80%E5%8F%91/2024/05/12/20-15-50-a9491705c4a2b5d51133aa9ca260bd77-STM32F103C8T6%E5%BC%95%E8%84%9A%E5%AE%9A%E4%B9%89-82120f.png)



##### 中断引脚选择



![](https://cdn.jsdelivr.net/gh/xuezhaorong/Picgo//Source/fix-dir/STM32/%E5%BA%93%E5%87%BD%E6%95%B0%E5%BC%80%E5%8F%91/2024/05/12/20-16-38-c22a095d00862b6f5dd71f2a6de0901f-%E5%A4%96%E9%83%A8%E4%B8%AD%E6%96%AD%E9%80%9A%E7%94%A8IO%E6%98%A0%E5%83%8F-6af5f1.png)



 通过一系列的数据选择器，将各个类的 Pin 口经过选择后映射成一个 Pin 口

**相关寄存器配置:**

-   各个引脚外部中断的引脚配置寄存器

![](https://cdn.jsdelivr.net/gh/xuezhaorong/Picgo//Source/fix-dir/STM32/%E5%BA%93%E5%87%BD%E6%95%B0%E5%BC%80%E5%8F%91/2024/05/12/20-17-26-d9c2ecea938f4c50d7d28c87fd403d7b-%E5%A4%96%E9%83%A8%E4%B8%AD%E6%96%AD%E9%85%8D%E7%BD%AE%E5%AF%84%E5%AD%98%E5%99%A8%201-998e87.png)

![](https://cdn.jsdelivr.net/gh/xuezhaorong/Picgo//Source/fix-dir/STM32/%E5%BA%93%E5%87%BD%E6%95%B0%E5%BC%80%E5%8F%91/2024/05/12/20-17-55-9dd2a853416831212b5d1344ed463840-%E5%A4%96%E9%83%A8%E4%B8%AD%E6%96%AD%E9%85%8D%E7%BD%AE%E5%AF%84%E5%AD%98%E5%99%A8%202-62b54a.png)

![](https://cdn.jsdelivr.net/gh/xuezhaorong/Picgo//Source/fix-dir/STM32/%E5%BA%93%E5%87%BD%E6%95%B0%E5%BC%80%E5%8F%91/2024/05/12/20-18-16-76a081b1b40db2792d8188bddc514789-%E5%A4%96%E9%83%A8%E4%B8%AD%E6%96%AD%E9%85%8D%E7%BD%AE%E5%AF%84%E5%AD%98%E5%99%A8%203-4d8aa3.png)

![](https://cdn.jsdelivr.net/gh/xuezhaorong/Picgo//Source/fix-dir/STM32/%E5%BA%93%E5%87%BD%E6%95%B0%E5%BC%80%E5%8F%91/2024/05/12/20-18-20-5c07ac600871a0bd07ec307af7e7b390-%E5%A4%96%E9%83%A8%E4%B8%AD%E6%96%AD%E9%85%8D%E7%BD%AE%E5%AF%84%E5%AD%98%E5%99%A8%204-600a35.png)



**相关库函数配置:**

-   外部中断引脚配置函数原型

```c
/**
  * @brief  Selects the GPIO pin used as EXTI Line.
  * @param  GPIO_PortSource: selects the GPIO port to be used as source for EXTI lines.
  *   This parameter can be GPIO_PortSourceGPIOx where x can be (A..G).
  * @param  GPIO_PinSource: specifies the EXTI line to be configured.
  *   This parameter can be GPIO_PinSourcex where x can be (0..15).
  * @retval None
  */
void GPIO_EXTILineConfig(uint8_t GPIO_PortSource, uint8_t GPIO_PinSource)

```

-   第一个参数为需要配置的类端口，第二个参数为具体的 Pin 端口

    

-   示例代码:

```c
GPIO_EXTILineConfig(GPIO_PortSourceGPIOA,GPIO_PinSource15);
```

#### EXTI

-   EXTI (ExternalInterrupt) 外部中断
-   EXTI 可以监测指定 GPIO 口的电平信号，当其指定的 GPIO 口产生电平变化时，EXTI 将立即向 NVIC 发出中断申请，经过 NVIC 裁决后即可中断 CPU 主程序，使 CPU 执行 CPU 对应的中断程序
-   支持的触发方式：上升沿 / 下降沿 / 双边沿 / 软件触发支持的 GPIO 口：所有 GPIO 口，但相同的 Pin 不能同时触发中断通道数：16 个 GPIO_Pin，外加 PVD 输出、RTC 闹钟、USB 唤醒、以太网唤醒
-   触发响应方式：中断响应 / 事件响应 NVIC
-   EXTI 的外设时钟是一直打开的，不需要手动打开

![](https://cdn.jsdelivr.net/gh/xuezhaorong/Picgo//Source/fix-dir/STM32/%E5%BA%93%E5%87%BD%E6%95%B0%E5%BC%80%E5%8F%91/2024/05/12/20-19-01-ceb8121db3a1dbb6c8a6dc065663b631-EXTI%E6%A1%86%E5%9B%BE-fc60c7.png)



 20 根输入线先进入边沿检测电路，通过上升沿寄存器和下降沿寄存器选择触发方式，通过或门加入软件触发的方式，之后进行中断响应或者事件响应，可通过屏蔽寄存器单独控制。

**相关寄存器:**

-   上升沿触发选择寄存器，用于设置对应线的上升沿触发方式

![](https://cdn.jsdelivr.net/gh/xuezhaorong/Picgo//Source/fix-dir/STM32/%E5%BA%93%E5%87%BD%E6%95%B0%E5%BC%80%E5%8F%91/2024/05/12/20-19-22-26cbb982683364444cef2cea7ec49319-%E4%B8%8A%E5%8D%87%E6%B2%BF%E8%A7%A6%E5%8F%91%E9%80%89%E6%8B%A9%E5%AF%84%E5%AD%98%E5%99%A8-38f815.png)

-   下降沿触发选择寄存器，用于设置对应线的下降沿触发方式

![](https://cdn.jsdelivr.net/gh/xuezhaorong/Picgo//Source/fix-dir/STM32/%E5%BA%93%E5%87%BD%E6%95%B0%E5%BC%80%E5%8F%91/2024/05/12/20-19-44-f3fd4073f8bbfa49397c258b1aea7bc5-%E4%B8%8B%E9%99%8D%E6%B2%BF%E8%A7%A6%E5%8F%91%E9%80%89%E6%8B%A9%E5%AF%84%E5%AD%98%E5%99%A8-50e4fe.png)

-   软件中断事件寄存器，设置对应线的软件中断触发方式

![](https://cdn.jsdelivr.net/gh/xuezhaorong/Picgo//Source/fix-dir/STM32/%E5%BA%93%E5%87%BD%E6%95%B0%E5%BC%80%E5%8F%91/2024/05/12/20-20-05-a3327a9dd0f8efaf8251ddbfcdff1244-%E8%BD%AF%E4%BB%B6%E4%B8%AD%E6%96%AD%E4%BA%8B%E4%BB%B6%E5%AF%84%E5%AD%98%E5%99%A8-985a97.png)

-   中断屏蔽寄存器，用于开启和关闭对应线的中断响应

![](https://cdn.jsdelivr.net/gh/xuezhaorong/Picgo//Source/fix-dir/STM32/%E5%BA%93%E5%87%BD%E6%95%B0%E5%BC%80%E5%8F%91/2024/05/12/20-21-10-c3a7f11af32491728e88627a5fe132f6-%E4%B8%AD%E6%96%AD%E5%B1%8F%E8%94%BD%E5%AF%84%E5%AD%98%E5%99%A8-a3a852.png)

-   挂起寄存器，设置对应线中断响应的标志位

![](https://cdn.jsdelivr.net/gh/xuezhaorong/Picgo//Source/fix-dir/STM32/%E5%BA%93%E5%87%BD%E6%95%B0%E5%BC%80%E5%8F%91/2024/05/12/20-21-32-d07e4b03701b8e58b92fa0ce15056128-%E6%8C%82%E8%B5%B7%E5%AF%84%E5%AD%98%E5%99%A8-01d5bd.png)

-   事件屏蔽寄存器，用于开启和关闭对应线的事件响应

![](https://cdn.jsdelivr.net/gh/xuezhaorong/Picgo//Source/fix-dir/STM32/%E5%BA%93%E5%87%BD%E6%95%B0%E5%BC%80%E5%8F%91/2024/05/12/20-22-37-870fb80ef2a6f3bbf6e1a139c571e233-%E4%BA%8B%E4%BB%B6%E5%B1%8F%E8%94%BD%E5%AF%84%E5%AD%98%E5%99%A8-1cba30.png)



**相关库函数:**

-   EXIT 的初始化，将 EXIT 功能集合在一起，包括选择对应的线路，触发方式和响应方式

```c
/**
  * @brief  Initializes the EXTI peripheral according to the specified
  *         parameters in the EXTI_InitStruct.
  * @param  EXTI_InitStruct: pointer to a EXTI_InitTypeDef structure
  *         that contains the configuration information for the EXTI peripheral.
  * @retval None
  */
void EXTI_Init(EXTI_InitTypeDef* EXTI_InitStruct)

```

-   **EXTI_InitTypeDef** 结构体说明

```c
typedef struct
{
  uint32_t EXTI_Line;               /*!< Specifies the EXTI lines to be enabled or disabled.
                                         This parameter can be any combination of @ref EXTI_Lines */
 
  EXTIMode_TypeDef EXTI_Mode;       /*!< Specifies the mode for the EXTI lines.
                                         This parameter can be a value of @ref EXTIMode_TypeDef */
 
  EXTITrigger_TypeDef EXTI_Trigger; /*!< Specifies the trigger signal active edge for the EXTI lines.
                                         This parameter can be a value of @ref EXTIMode_TypeDef */
 
  FunctionalState EXTI_LineCmd;     /*!< Specifies the new state of the selected EXTI lines.
                                         This parameter can be set either to ENABLE or DISABLE */ 
}EXTI_InitTypeDef;
```

-   **EXTI_Line** 用于选择对应的线路

```c
#define EXTI_Line0       ((uint32_t)0x00001)  /*!< External interrupt line 0 */
#define EXTI_Line1       ((uint32_t)0x00002)  /*!< External interrupt line 1 */
#define EXTI_Line2       ((uint32_t)0x00004)  /*!< External interrupt line 2 */
#define EXTI_Line3       ((uint32_t)0x00008)  /*!< External interrupt line 3 */
#define EXTI_Line4       ((uint32_t)0x00010)  /*!< External interrupt line 4 */
#define EXTI_Line5       ((uint32_t)0x00020)  /*!< External interrupt line 5 */
#define EXTI_Line6       ((uint32_t)0x00040)  /*!< External interrupt line 6 */
#define EXTI_Line7       ((uint32_t)0x00080)  /*!< External interrupt line 7 */
#define EXTI_Line8       ((uint32_t)0x00100)  /*!< External interrupt line 8 */
#define EXTI_Line9       ((uint32_t)0x00200)  /*!< External interrupt line 9 */
#define EXTI_Line10      ((uint32_t)0x00400)  /*!< External interrupt line 10 */
#define EXTI_Line11      ((uint32_t)0x00800)  /*!< External interrupt line 11 */
#define EXTI_Line12      ((uint32_t)0x01000)  /*!< External interrupt line 12 */
#define EXTI_Line13      ((uint32_t)0x02000)  /*!< External interrupt line 13 */
#define EXTI_Line14      ((uint32_t)0x04000)  /*!< External interrupt line 14 */
#define EXTI_Line15      ((uint32_t)0x08000)  /*!< External interrupt line 15 */
#define EXTI_Line16      ((uint32_t)0x10000)  /*!< External interrupt line 16 Connected to the PVD Output */
#define EXTI_Line17      ((uint32_t)0x20000)  /*!< External interrupt line 17 Connected to the RTC Alarm event */
#define EXTI_Line18      ((uint32_t)0x40000)  /*!< External interrupt line 18 Connected to the USB Device/USB OTG FS
                                                   Wakeup from suspend event */                                    
#define EXTI_Line19      ((uint32_t)0x80000)  /*!< External interrupt line 19 Connected to the Ethernet Wakeup event */
```

-   **EXTIMode_TypeDef** 用于设置响应方式，中断响应或者事件响应

```C
typedef enum
{
  EXTI_Mode_Interrupt = 0x00, // 中断响应
  EXTI_Mode_Event = 0x04 // 事件响应
}EXTIMode_TypeDef;
```

-   **EXTITrigger_TypeDef** 用于指定响应触发方式，上升沿触发，下降沿触发和双边触发

```c
typedef enum
{
  EXTI_Trigger_Rising = 0x08, // 上升沿触发
  EXTI_Trigger_Falling = 0x0C,  // 下降沿触发
  EXTI_Trigger_Rising_Falling = 0x10 // 双边触发
}EXTITrigger_TypeDef;
```

-   **FunctionalState** 指定对应线路的中断状态，开启中断或者关闭中断

```c
typedef enum {DISABLE = 0, ENABLE = !DISABLE} FunctionalState;
```

示例代码:

```C
// 配置EXTI
EXTI_InitTypeDef EXTIA15_InitStructure = {
    .EXTI_Line = EXTI_Line15, // 指定启用的线
    .EXTI_LineCmd = ENABLE, // 指定状态
    .EXTI_Mode = EXTI_Mode_Interrupt, // 中断响应
    .EXTI_Trigger = EXTI_Trigger_Falling// 指定边沿方式
};
 
EXTI_Init(&EXTIA15_InitStructure);
```

#### NVIC

 NVIC 属于内核外设，在 STM32 中，用来统一分配中断优先级和管理中断。

![](https://cdn.jsdelivr.net/gh/xuezhaorong/Picgo//Source/fix-dir/STM32/%E5%BA%93%E5%87%BD%E6%95%B0%E5%BC%80%E5%8F%91/2024/05/12/20-23-17-75d9c8904a945699190eadb1df9b14c1-NVIC%E5%9F%BA%E6%9C%AC%E7%BB%93%E6%9E%84-60718b.png)

-   NVIC 的中断优先级由优先级寄存器的 4 位 (0~15) 决定，这 4 位可以进行切分，分为高 n 位的抢占优先级和低 4-n 位的响应优先级
-   抢占先级高的可以中断嵌套，响应优先级高的可以优先排队，抢占优先级和响应优先级均相同的按中断号排队

![](https://cdn.jsdelivr.net/gh/xuezhaorong/Picgo//Source/fix-dir/STM32/%E5%BA%93%E5%87%BD%E6%95%B0%E5%BC%80%E5%8F%91/2024/05/12/20-23-37-90cfead90090694c74b38f6671241d05-NVIC%E4%BC%98%E5%85%88%E7%BA%A7%E5%88%86%E7%BB%84-360571.png)

由于 NVIC 属于内核外设，寄存器配置较为复杂，直接使用库函数进行配置

-   优先级分组配置
-   @arg NVIC_PriorityGroup_0: 0 bits for pre-emption priority 4 bits for subpriority
    -   @arg NVIC_PriorityGroup_1: 1 bits for pre-emption priority 3 bits for subpriority
    -   @arg NVIC_PriorityGroup_2: 2 bits for pre-emption priority 2 bits for subpriority
    -   @arg NVIC_PriorityGroup_3: 3 bits for pre-emption priority 1 bits for subpriority
    -   @arg NVIC_PriorityGroup_4: 4 bits for pre-emption priority 0 bits for subpriority
    -   对应上表的分组方式

```c
/**
  * @brief  Configures the priority grouping: pre-emption priority and subpriority.
  * @param  NVIC_PriorityGroup: specifies the priority grouping bits length. 
  *   This parameter can be one of the following values:
  *     @arg NVIC_PriorityGroup_0: 0 bits for pre-emption priority
  *                                4 bits for subpriority
  *     @arg NVIC_PriorityGroup_1: 1 bits for pre-emption priority
  *                                3 bits for subpriority
  *     @arg NVIC_PriorityGroup_2: 2 bits for pre-emption priority
  *                                2 bits for subpriority
  *     @arg NVIC_PriorityGroup_3: 3 bits for pre-emption priority
  *                                1 bits for subpriority
  *     @arg NVIC_PriorityGroup_4: 4 bits for pre-emption priority
  *                                0 bits for subpriority
  * @retval None
  */
void NVIC_PriorityGroupConfig(uint32_t NVIC_PriorityGroup)

```

-   NVIC 配置，选择线路，配置中断的开启和关闭，设置响应优先级和抢占优先级

```c
/**
  * @brief  Initializes the NVIC peripheral according to the specified
  *         parameters in the NVIC_InitStruct.
  * @param  NVIC_InitStruct: pointer to a NVIC_InitTypeDef structure that contains
  *         the configuration information for the specified NVIC peripheral.
  * @retval None
  */
void NVIC_Init(NVIC_InitTypeDef* NVIC_InitStruct)

```

-   **NVIC_InitTypeDef** 的具体说明

```c
typedef struct
{
  uint8_t NVIC_IRQChannel;                    /*!< Specifies the IRQ channel to be enabled or disabled.
                                                   This parameter can be a value of @ref IRQn_Type 
                                                   (For the complete STM32 Devices IRQ Channels list, please
                                                    refer to stm32f10x.h file) */
 
  uint8_t NVIC_IRQChannelPreemptionPriority;  /*!< Specifies the pre-emption priority for the IRQ channel
                                                   specified in NVIC_IRQChannel. This parameter can be a value
                                                   between 0 and 15 as described in the table @ref NVIC_Priority_Table */
 
  uint8_t NVIC_IRQChannelSubPriority;         /*!< Specifies the subpriority level for the IRQ channel specified
                                                   in NVIC_IRQChannel. This parameter can be a value
                                                   between 0 and 15 as described in the table @ref NVIC_Priority_Table */
 
  FunctionalState NVIC_IRQChannelCmd;         /*!< Specifies whether the IRQ channel defined in NVIC_IRQChannel
                                                   will be enabled or disabled. 
                                                   This parameter can be set either to ENABLE or DISABLE */   
} NVIC_InitTypeDef;
```

-   **NVIC_IRQChannel** 用于选择中断信道
-   STM32F10X_HD 信号的中断信道列表

```c
#ifdef STM32F10X_HD
  ADC1_2_IRQn                 = 18,     /*!< ADC1 and ADC2 global Interrupt                       */
  USB_HP_CAN1_TX_IRQn         = 19,     /*!< USB Device High Priority or CAN1 TX Interrupts       */
  USB_LP_CAN1_RX0_IRQn        = 20,     /*!< USB Device Low Priority or CAN1 RX0 Interrupts       */
  CAN1_RX1_IRQn               = 21,     /*!< CAN1 RX1 Interrupt                                   */
  CAN1_SCE_IRQn               = 22,     /*!< CAN1 SCE Interrupt                                   */
  EXTI9_5_IRQn                = 23,     /*!< External Line[9:5] Interrupts                        */
  TIM1_BRK_IRQn               = 24,     /*!< TIM1 Break Interrupt                                 */
  TIM1_UP_IRQn                = 25,     /*!< TIM1 Update Interrupt                                */
  TIM1_TRG_COM_IRQn           = 26,     /*!< TIM1 Trigger and Commutation Interrupt               */
  TIM1_CC_IRQn                = 27,     /*!< TIM1 Capture Compare Interrupt                       */
  TIM2_IRQn                   = 28,     /*!< TIM2 global Interrupt                                */
  TIM3_IRQn                   = 29,     /*!< TIM3 global Interrupt                                */
  TIM4_IRQn                   = 30,     /*!< TIM4 global Interrupt                                */
  I2C1_EV_IRQn                = 31,     /*!< I2C1 Event Interrupt                                 */
  I2C1_ER_IRQn                = 32,     /*!< I2C1 Error Interrupt                                 */
  I2C2_EV_IRQn                = 33,     /*!< I2C2 Event Interrupt                                 */
  I2C2_ER_IRQn                = 34,     /*!< I2C2 Error Interrupt                                 */
  SPI1_IRQn                   = 35,     /*!< SPI1 global Interrupt                                */
  SPI2_IRQn                   = 36,     /*!< SPI2 global Interrupt                                */
  USART1_IRQn                 = 37,     /*!< USART1 global Interrupt                              */
  USART2_IRQn                 = 38,     /*!< USART2 global Interrupt                              */
  USART3_IRQn                 = 39,     /*!< USART3 global Interrupt                              */
  EXTI15_10_IRQn              = 40,     /*!< External Line[15:10] Interrupts                      */
  RTCAlarm_IRQn               = 41,     /*!< RTC Alarm through EXTI Line Interrupt                */
  USBWakeUp_IRQn              = 42,     /*!< USB Device WakeUp from suspend through EXTI Line Interrupt */
  TIM8_BRK_IRQn               = 43,     /*!< TIM8 Break Interrupt                                 */
  TIM8_UP_IRQn                = 44,     /*!< TIM8 Update Interrupt                                */
  TIM8_TRG_COM_IRQn           = 45,     /*!< TIM8 Trigger and Commutation Interrupt               */
  TIM8_CC_IRQn                = 46,     /*!< TIM8 Capture Compare Interrupt                       */
  ADC3_IRQn                   = 47,     /*!< ADC3 global Interrupt                                */
  FSMC_IRQn                   = 48,     /*!< FSMC global Interrupt                                */
  SDIO_IRQn                   = 49,     /*!< SDIO global Interrupt                                */
  TIM5_IRQn                   = 50,     /*!< TIM5 global Interrupt                                */
  SPI3_IRQn                   = 51,     /*!< SPI3 global Interrupt                                */
  UART4_IRQn                  = 52,     /*!< UART4 global Interrupt                               */
  UART5_IRQn                  = 53,     /*!< UART5 global Interrupt                               */
  TIM6_IRQn                   = 54,     /*!< TIM6 global Interrupt                                */
  TIM7_IRQn                   = 55,     /*!< TIM7 global Interrupt                                */
  DMA2_Channel1_IRQn          = 56,     /*!< DMA2 Channel 1 global Interrupt                      */
  DMA2_Channel2_IRQn          = 57,     /*!< DMA2 Channel 2 global Interrupt                      */
  DMA2_Channel3_IRQn          = 58,     /*!< DMA2 Channel 3 global Interrupt                      */
  DMA2_Channel4_5_IRQn        = 59      /*!< DMA2 Channel 4 and Channel 5 global Interrupt        */
#endif /* STM32F10X_HD */  
```

-   **NVIC_IRQChannelPreemptionPriority** 设置信道的抢占优先级，**NVIC_IRQChannelSubPriority** 设置信道的响应优先级，具体的数值根据优先级分组
-   **NVIC_IRQChannelCmd** 设置信道是否使能

示例代码:

```c
NVIC_InitTypeDef NVICA15_InitStructure = {
    .NVIC_IRQChannel = EXTI15_10_IRQn,
    .NVIC_IRQChannelCmd = ENABLE,
    .NVIC_IRQChannelPreemptionPriority = 1,
    .NVIC_IRQChannelSubPriority = 1
};  
NVIC_Init(&NVICA15_InitStructure);
```

#### 中断函数

```c
__Vectors       DCD     __initial_sp               ; Top of Stack
                DCD     Reset_Handler              ; Reset Handler
                DCD     NMI_Handler                ; NMI Handler
                DCD     HardFault_Handler          ; Hard Fault Handler
                DCD     MemManage_Handler          ; MPU Fault Handler
                DCD     BusFault_Handler           ; Bus Fault Handler
                DCD     UsageFault_Handler         ; Usage Fault Handler
                DCD     0                          ; Reserved
                DCD     0                          ; Reserved
                DCD     0                          ; Reserved
                DCD     0                          ; Reserved
                DCD     SVC_Handler                ; SVCall Handler
                DCD     DebugMon_Handler           ; Debug Monitor Handler
                DCD     0                          ; Reserved
                DCD     PendSV_Handler             ; PendSV Handler
                DCD     SysTick_Handler            ; SysTick Handler
 
                ; External Interrupts
                DCD     WWDG_IRQHandler            ; Window Watchdog
                DCD     PVD_IRQHandler             ; PVD through EXTI Line detect
                DCD     TAMPER_IRQHandler          ; Tamper
                DCD     RTC_IRQHandler             ; RTC
                DCD     FLASH_IRQHandler           ; Flash
                DCD     RCC_IRQHandler             ; RCC
                DCD     EXTI0_IRQHandler           ; EXTI Line 0
                DCD     EXTI1_IRQHandler           ; EXTI Line 1
                DCD     EXTI2_IRQHandler           ; EXTI Line 2
                DCD     EXTI3_IRQHandler           ; EXTI Line 3
                DCD     EXTI4_IRQHandler           ; EXTI Line 4
                DCD     DMA1_Channel1_IRQHandler   ; DMA1 Channel 1
                DCD     DMA1_Channel2_IRQHandler   ; DMA1 Channel 2
                DCD     DMA1_Channel3_IRQHandler   ; DMA1 Channel 3
                DCD     DMA1_Channel4_IRQHandler   ; DMA1 Channel 4
                DCD     DMA1_Channel5_IRQHandler   ; DMA1 Channel 5
                DCD     DMA1_Channel6_IRQHandler   ; DMA1 Channel 6
                DCD     DMA1_Channel7_IRQHandler   ; DMA1 Channel 7
                DCD     ADC1_2_IRQHandler          ; ADC1 & ADC2
                DCD     USB_HP_CAN1_TX_IRQHandler  ; USB High Priority or CAN1 TX
                DCD     USB_LP_CAN1_RX0_IRQHandler ; USB Low  Priority or CAN1 RX0
                DCD     CAN1_RX1_IRQHandler        ; CAN1 RX1
                DCD     CAN1_SCE_IRQHandler        ; CAN1 SCE
                DCD     EXTI9_5_IRQHandler         ; EXTI Line 9..5
                DCD     TIM1_BRK_IRQHandler        ; TIM1 Break
                DCD     TIM1_UP_IRQHandler         ; TIM1 Update
                DCD     TIM1_TRG_COM_IRQHandler    ; TIM1 Trigger and Commutation
                DCD     TIM1_CC_IRQHandler         ; TIM1 Capture Compare
                DCD     TIM2_IRQHandler            ; TIM2
                DCD     TIM3_IRQHandler            ; TIM3
                DCD     TIM4_IRQHandler            ; TIM4
                DCD     I2C1_EV_IRQHandler         ; I2C1 Event
                DCD     I2C1_ER_IRQHandler         ; I2C1 Error
                DCD     I2C2_EV_IRQHandler         ; I2C2 Event
                DCD     I2C2_ER_IRQHandler         ; I2C2 Error
                DCD     SPI1_IRQHandler            ; SPI1
                DCD     SPI2_IRQHandler            ; SPI2
                DCD     USART1_IRQHandler          ; USART1
                DCD     USART2_IRQHandler          ; USART2
                DCD     USART3_IRQHandler          ; USART3
                DCD     EXTI15_10_IRQHandler       ; EXTI Line 15..10
                DCD     RTCAlarm_IRQHandler        ; RTC Alarm through EXTI Line
                DCD     USBWakeUp_IRQHandler       ; USB Wakeup from suspend
                DCD     TIM8_BRK_IRQHandler        ; TIM8 Break
                DCD     TIM8_UP_IRQHandler         ; TIM8 Update
                DCD     TIM8_TRG_COM_IRQHandler    ; TIM8 Trigger and Commutation
                DCD     TIM8_CC_IRQHandler         ; TIM8 Capture Compare
                DCD     ADC3_IRQHandler            ; ADC3
                DCD     FSMC_IRQHandler            ; FSMC
                DCD     SDIO_IRQHandler            ; SDIO
                DCD     TIM5_IRQHandler            ; TIM5
                DCD     SPI3_IRQHandler            ; SPI3
                DCD     UART4_IRQHandler           ; UART4
                DCD     UART5_IRQHandler           ; UART5
                DCD     TIM6_IRQHandler            ; TIM6
                DCD     TIM7_IRQHandler            ; TIM7
                DCD     DMA2_Channel1_IRQHandler   ; DMA2 Channel1
                DCD     DMA2_Channel2_IRQHandler   ; DMA2 Channel2
                DCD     DMA2_Channel3_IRQHandler   ; DMA2 Channel3
                DCD     DMA2_Channel4_5_IRQHandler ; DMA2 Channel4 & Channel5
```

通过查找中断函数名，写中断程序，**需要手动清除标志位**

```c
void EXTI15_10_IRQHandler(void){
    // 判断中断源
    if(EXTI_GetITStatus(EXTI_Line15) == SET){
        LED2_Turn();
        // 清楚标志位
        EXTI_ClearITPendingBit(EXTI_Line15);
    }
 
}
```

## 定时器

-   TIM (Timer) 定时器
-   定时器可以对输入的时钟进行计数，并在计数值达到设定值时触发中断
-   16 位计数器、预分频器、自动重装寄存器的时基单元，在 72MHZ 计数时钟下可以实现最大 59.65s 的定时
-   不仅具备基本的定时中断功能，而且还包含内外时钟源选择、输入捕获、输出比较、编码器接口、主从触发模式等多种功能
-   根据复杂度和应用场景分为了高级定时器、通用定时器、基本定时器三种类型

![](https://cdn.jsdelivr.net/gh/xuezhaorong/Picgo//Source/fix-dir/STM32/%E5%BA%93%E5%87%BD%E6%95%B0%E5%BC%80%E5%8F%91/2024/05/12/20-26-59-05f2f1e785a3cd9b4f341b42a562ae0d-%E5%AE%9A%E6%97%B6%E5%99%A8%E7%B1%BB%E5%9E%8B-6164f4.png)

### 基本定时器

-   16 位自动重装载累加计数器
-   16 位可编程 (可实时修改) 预分频器，用于对输入的时钟按系数为 1～65536 之间的任意数值 分频
-   触发 DAC 的同步电路
-   在更新事件 (计数器溢出) 时产生中断 / DMA 请求

![](https://cdn.jsdelivr.net/gh/xuezhaorong/Picgo//Source/fix-dir/STM32/%E5%BA%93%E5%87%BD%E6%95%B0%E5%BC%80%E5%8F%91/2024/05/12/20-27-41-78a7ccadfa2a848e2c075f31ff77094a-%E5%9F%BA%E6%9C%AC%E5%AE%9A%E6%97%B6%E5%99%A8-583e5b.png)



-   **时基单元**:

 这个可编程定时器的主要部分是一个带有自动重装载的 16 位累加计数器，计数器的时钟通过一 个预分频器得到。 软件可以读写**计数器、自动重装载寄存器和预分频寄存器**，即使计数器运行时也可以操作。

-   计数器寄存器 (TIMx_CNT)

 计数器计数自增，同时不断地与自动重装寄存器进行比较，置相等时，产生更新中断和更新事件。注意：实际的设置计数器使能信号 CNT_EN 相对于 CEN 滞后一个时钟周期。

![](https://cdn.jsdelivr.net/gh/xuezhaorong/Picgo//Source/fix-dir/STM32/%E5%BA%93%E5%87%BD%E6%95%B0%E5%BC%80%E5%8F%91/2024/05/12/20-29-12-2b153771e7176a6f1c97ff8d1428b9a7-%E8%AE%A1%E6%95%B0%E5%99%A8%E6%97%B6%E5%BA%8Fpng-87da3d.png)

-   预分频寄存器 (TIMx_PSC)

 预分频可以以系数介于 1 至 65536 之间的任意数值对计数器时钟分频。它是通过一个 16 位寄存器 (TIMx_PSC) 的计数实现分频。因为 TIMx_PSC 控制寄存器具有缓冲，可以在运行过程中改变它 的数值，**新的预分频数值将在下一个更新事件时起作用**。



![](https://cdn.jsdelivr.net/gh/xuezhaorong/Picgo//Source/fix-dir/STM32/%E5%BA%93%E5%87%BD%E6%95%B0%E5%BC%80%E5%8F%91/2024/05/12/20-28-08-a5cf35b61843beb9fc1a2ea34cd9c37a-%E9%A2%84%E5%88%86%E9%A2%91%E5%99%A8%E6%97%B6%E5%BA%8F%E5%9B%BE-54e10c.png)



-   自动重装载寄存器 (TIMx_ARR)

 自动重装载寄存器是预加载的，每次读写自动重装载寄存器时，实际上是通过读写预加载寄存器实现。写入预加载寄存器的内 容能够立即或在每次更新事件时，传送到它的**影子寄存器**。每当计数器达到溢出值时，硬件发出更新事件；软件也可以产生更新事件。



![](https://cdn.jsdelivr.net/gh/xuezhaorong/Picgo//Source/fix-dir/STM32/%E5%BA%93%E5%87%BD%E6%95%B0%E5%BC%80%E5%8F%91/2024/05/12/20-29-29-3e0f2b88495e19f419e1e7d75f42ef78-%E8%87%AA%E5%8A%A8%E9%87%8D%E8%A3%85%E5%99%A8%E6%97%B6%E5%BA%8F%E5%9B%BE-753784.png)

-   时钟源:

 计数器的时钟由内部时钟 (CK_INT) 提供。一旦置 CEN 位为’1’，内部时钟即向预分频器提供时钟。

![](https://cdn.jsdelivr.net/gh/xuezhaorong/Picgo//Source/fix-dir/STM32/%E5%BA%93%E5%87%BD%E6%95%B0%E5%BC%80%E5%8F%91/2024/05/12/20-30-37-56741f172ad0c06c0b801594e7d05993-%E5%86%85%E9%83%A8%E6%97%B6%E9%92%9F%E8%A7%A6%E5%8F%91%E4%B8%AD%E6%96%AD%E6%97%B6%E5%BA%8F-8dfc5f.png)

-   DAC 主模式:

 DAC 主模式将定时器的更新事件映射到触发输出 TRGO 的位置，TRGO 直接接到 DAC 的触发转换引脚上，定时器的更新不再通过中断来触发 DAC 转换，可用于在输入捕获功能中自动将计数器清 0

### 通用定时器

-   16 位**向上、向下、向上 / 向下自动装载计数器**
-   16 位可编程 (可以实时修改) 预分频器，计数器时钟频率的分频系数为 1～65536 之间的任意 数值
-   4 个独立通道： ─ 输入捕获 ─ 输出比较 ─ PWM 生成 (边缘或中间对齐模式) ─ 单脉冲模式输出
-   使用外部信号控制定时器和定时器互连的同步电路
-   如下事件发生时产生中断 / DMA： ─ 更新：计数器向上溢出 / 向下溢出，计数器初始化 (通过软件或者内部 / 外部触发) ─ 触发事件 (计数器启动、停止、初始化或者由内部 / 外部触发计数) ─ 输入捕获 ─ 输出比较
-   支持针对定位的增量 (正交) 编码器和霍尔传感器电路
-   **触发输入作为外部时钟或者按周期的电流管理**

 通用定时器是一个通过可编程预分频器驱动的 16 位自动装载计数器构成。 它适用于多种场合，包括测量输入信号的脉冲长度 (输入捕获) 或者产生输出波形 (输出比较和 PWM)。 使用定时器预分频器和 RCC 时钟控制器预分频器，脉冲长度和波形周期可以在几个微秒到几个 毫秒间调整。 每个定时器都是完全独立的，没有互相共享任何资源。它们可以一起同步操作。

![](https://cdn.jsdelivr.net/gh/xuezhaorong/Picgo//Source/fix-dir/STM32/%E5%BA%93%E5%87%BD%E6%95%B0%E5%BC%80%E5%8F%91/2024/05/12/20-30-59-35448908640a74518e2ebe27198d1ba5-%E9%80%9A%E7%94%A8%E5%AE%9A%E6%97%B6%E5%99%A8-5684ed.png)



-   **时钟源:**
-   计数器时钟可由下列时钟源提供：
-   内部时钟 (CK_INT)
-   外部时钟模式 1：由通过 TRGI 的 ETR, 其他定时器的 TRGO 输出，输入捕获的 CH1 引脚的边沿，CH1 引脚和 CH2 引脚提供时钟

 当 TIMx_SMCR 寄存器的 SMS=111 时，此模式被选中。计数器可以在选定输入端的每个上升沿 或下降沿计数。

![](https://cdn.jsdelivr.net/gh/xuezhaorong/Picgo//Source/fix-dir/STM32/%E5%BA%93%E5%87%BD%E6%95%B0%E5%BC%80%E5%8F%91/2024/05/12/20-31-23-a97ec9332cd24312e5203ceb7decefaf-%E5%A4%96%E9%83%A8%E8%BE%93%E5%85%A51%E6%A1%86%E5%9B%BE-c47868.png)

-   外部时钟模式 2：由 ETR 引脚提供时钟

 选定此模式的方法为：令 TIMx_SMCR 寄存器中的 ECE=1 计数器能够在外部触发 ETR 的每一个上升沿或下降沿计数。

![](https://cdn.jsdelivr.net/gh/xuezhaorong/Picgo//Source/fix-dir/STM32/%E5%BA%93%E5%87%BD%E6%95%B0%E5%BC%80%E5%8F%91/2024/05/12/20-31-38-68398688ba4cb4b06c18096645343444-%E5%A4%96%E9%83%A8%E8%BE%93%E5%85%A52%E6%A1%86%E5%9B%BE-c37d66.png)



-   内部触发输入 (ITRx)：使用一个定时器作为另一个定时器的预分频器，如可以配置一个定时 器 Timer1 而作为另一个定时器 Timer2 的预分频器。
-   编码器接口用于读取正交编码器的输出波形。
-   四个输出比较电路，用于输出 PWM 波形，驱动电机。
-   四个输入捕获电路，用于测量输入方波的频率。

### 定时中断

 输入的 72MHZ 基准频率进入到预分频器进行分频，计数器对预分配后的计数时钟进行计数，当计数到自动重装寄存器存储的目标值，产生中断信号，并清零寄存器，计数器自动开始下一次的计时。

![](https://cdn.jsdelivr.net/gh/xuezhaorong/Picgo//Source/fix-dir/STM32/%E5%BA%93%E5%87%BD%E6%95%B0%E5%BC%80%E5%8F%91/2024/05/12/20-32-00-545b7aa8d21ba46a7be4c92a5995e6f4-%E5%AE%9A%E6%97%B6%E4%B8%AD%E6%96%AD%E5%9F%BA%E6%9C%AC%E7%BB%93%E6%9E%84%E5%9B%BE-36fbc4.png)



1.  开启 RCC 时钟，开启定时器的基准时钟和整个外设的工作时钟。
2.  选择时钟源。
3.  配置时基单元。
4.  配置输出中断控制，允许更新中断输出到 NVIC。
5.  配置 NVIC, 在 NVIC 中打开定时器中断的通道，并分配一个优先级。
6.  运行控制，使能定时器。
7.  配置中断程序。

![](https://cdn.jsdelivr.net/gh/xuezhaorong/Picgo//Source/fix-dir/STM32/%E5%BA%93%E5%87%BD%E6%95%B0%E5%BC%80%E5%8F%91/2024/05/12/20-32-17-cfe732ea03328896ba9c268239fde459-%E5%AE%9A%E6%97%B6%E4%B8%AD%E6%96%AD%E9%85%8D%E7%BD%AE%E5%BA%93%E5%87%BD%E6%95%B0-b07606.png)

**库函数配置步骤:**

1.  开启时钟，APB1 外设时钟

```c
// 开始时钟使能
RCC_APB1PeriphClockCmd(RCC_APB1Periph_TIM2,ENABLE);
```

2.  选择内部时钟

```c
/**
  * @brief  Configures the TIMx internal Clock
  * @param  TIMx: where x can be  1, 2, 3, 4, 5, 8, 9, 12 or 15
  *         to select the TIM peripheral.
  * @retval None
  */
void TIM_InternalClockConfig(TIM_TypeDef* TIMx)

```

**TIM_TypeDef** 是定时器的编号，可以选择 TIM1~TIM12 或者 15

```c
// 使用内部时钟
TIM_InternalClockConfig(TIM2);
```

3.  配置时基单元

-   **注意：当时基单元初始化时，为保证影子寄存器能够立刻更新，会自动产生一次中断。**
-   可以调用 **TIM_ClearFlag(TIM2,TIM_FLAG_Update)** 或者 **TIM_ClearITPendingBit(TIM2,TIM_IT_Update)** 清除中断标志位

```c
/**
  * @brief  Initializes the TIMx Time Base Unit peripheral according to 
  *         the specified parameters in the TIM_TimeBaseInitStruct.
  * @param  TIMx: where x can be 1 to 17 to select the TIM peripheral.
  * @param  TIM_TimeBaseInitStruct: pointer to a TIM_TimeBaseInitTypeDef
  *         structure that contains the configuration information for the 
  *         specified TIM peripheral.
  * @retval None
  */
void TIM_TimeBaseInit(TIM_TypeDef* TIMx, TIM_TimeBaseInitTypeDef* TIM_TimeBaseInitStruct)

```

**TIM_TimeBaseInitTypeDef** 指定时钟分配 (处理滤波器的采样频率), 计数器模式，ARR 自动重装器的值，PSC 预分频器的值，重复计数器的值

```c
typedef struct
{
  uint16_t TIM_Prescaler;         /*!< Specifies the prescaler value used to divide the TIM clock.
                                       This parameter can be a number between 0x0000 and 0xFFFF 
                                       PSC预分频器的值*/
 
  uint16_t TIM_CounterMode;       /*!< Specifies the counter mode.
                                       This parameter can be a value of @ref TIM_Counter_Mode */
 
  uint16_t TIM_Period;            /*!< Specifies the period value to be loaded into the active
                                       Auto-Reload Register at the next update event.
                                       This parameter must be a number between 0x0000 and 0xFFFF. 
                                       指定ARR自动重装器的值*/ 
 
  uint16_t TIM_ClockDivision;     /*!< Specifies the clock division.
                                      This parameter can be a value of @ref TIM_Clock_Division_CKD */
 
  uint8_t TIM_RepetitionCounter;  /*!< Specifies the repetition counter value. Each time the RCR downcounter
                                       reaches zero, an update event is generated and counting restarts
                                       from the RCR value (N).
                                       This means in PWM mode that (N+1) corresponds to:
                                          - the number of PWM periods in edge-aligned mode
                                          - the number of half PWM period in center-aligned mode
                                       This parameter must be a number between 0x00 and 0xFF. 
                                       @note This parameter is valid only for TIM1 and TIM8. 
                                       重复计数器的值*/
} TIM_TimeBaseInitTypeDef;  
```

**TIM_CounterMode** 指定计数器模式

```c
/** @defgroup TIM_Counter_Mode 
  * @{
  */
 
#define TIM_CounterMode_Up                 ((uint16_t)0x0000) // 向上计数
#define TIM_CounterMode_Down               ((uint16_t)0x0010) // 向下计数
#define TIM_CounterMode_CenterAligned1     ((uint16_t)0x0020) // 中央队齐
#define TIM_CounterMode_CenterAligned2     ((uint16_t)0x0040)
#define TIM_CounterMode_CenterAligned3     ((uint16_t)0x0060)
```

**TIM_ClockDivision** 用于指定分频方式，有不分频，2 分频和 4 分频

```c
#define TIM_CKD_DIV1                       ((uint16_t)0x0000)
#define TIM_CKD_DIV2                       ((uint16_t)0x0100)
#define TIM_CKD_DIV4                       ((uint16_t)0x0200)
```

**TIM_Prescaler** 和 **TIM_Period** 用来配置预分频器和自动重装器的值
$$
Tout = ((arr+1) * (psc)+1) / Tclk
$$

根据上方的计数器溢出频率公式计算定时值

示例代码:

```c
TIM_TimeBaseInitTypeDef TIM_TimeBaseInitStructure = {
    .TIM_ClockDivision = TIM_CKD_DIV1,  
    .TIM_CounterMode = TIM_CounterMode_Up, // 向上计数
    .TIM_Period = 10000 - 1,
    .TIM_Prescaler = 7200 - 1,
    .TIM_RepetitionCounter = 0
};
// 配置时基单元
TIM_TimeBaseInit(TIM2,&TIM_TimeBaseInitStructure);
```

4.   配置输出中断控制

```c
/**
  * @brief  Enables or disables the specified TIM interrupts.
  * @param  TIMx: where x can be 1 to 17 to select the TIMx peripheral.
  * @param  TIM_IT: specifies the TIM interrupts sources to be enabled or disabled.
  *   This parameter can be any combination of the following values:
  *     @arg TIM_IT_Update: TIM update Interrupt source
  *     @arg TIM_IT_CC1: TIM Capture Compare 1 Interrupt source
  *     @arg TIM_IT_CC2: TIM Capture Compare 2 Interrupt source
  *     @arg TIM_IT_CC3: TIM Capture Compare 3 Interrupt source
  *     @arg TIM_IT_CC4: TIM Capture Compare 4 Interrupt source
  *     @arg TIM_IT_COM: TIM Commutation Interrupt source
  *     @arg TIM_IT_Trigger: TIM Trigger Interrupt source
  *     @arg TIM_IT_Break: TIM Break Interrupt source
  * @note 
  *   - TIM6 and TIM7 can only generate an update interrupt.
  *   - TIM9, TIM12 and TIM15 can have only TIM_IT_Update, TIM_IT_CC1,
  *      TIM_IT_CC2 or TIM_IT_Trigger. 
  *   - TIM10, TIM11, TIM13, TIM14, TIM16 and TIM17 can have TIM_IT_Update or TIM_IT_CC1.   
  *   - TIM_IT_Break is used only with TIM1, TIM8 and TIM15. 
  *   - TIM_IT_COM is used only with TIM1, TIM8, TIM15, TIM16 and TIM17.    
  * @param  NewState: new state of the TIM interrupts.
  *   This parameter can be: ENABLE or DISABLE.
  * @retval None
  */
void TIM_ITConfig(TIM_TypeDef* TIMx, uint16_t TIM_IT, FunctionalState NewState)

```

示例代码:

```c
// 更新中断
TIM_ITConfig(TIM2,TIM_IT_Update,ENABLE);
```

5.  配置 NVIC, 注意中断通道选择

```c
// 配置NVIC
NVIC_PriorityGroupConfig(NVIC_PriorityGroup_2);
 
NVIC_InitTypeDef NVIC_InitStructure = {
    .NVIC_IRQChannel = TIM2_IRQn,
    .NVIC_IRQChannelCmd = ENABLE,
    .NVIC_IRQChannelPreemptionPriority = 2,
    .NVIC_IRQChannelSubPriority = 1
 
};
 
NVIC_Init(&NVIC_InitStructure);
```

6.  启动定时器

```c
/**
  * @brief  Enables or disables the specified TIM peripheral.
  * @param  TIMx: where x can be 1 to 17 to select the TIMx peripheral.
  * @param  NewState: new state of the TIMx peripheral.
  *   This parameter can be: ENABLE or DISABLE.
  * @retval None
  */
void TIM_Cmd(TIM_TypeDef* TIMx, FunctionalState NewState)

```

示例代码:

```c
TIM_Cmd(TIM2,ENABLE);
```

写中断函数

```c
void TIM2_IRQHandler(void){
    if(TIM_GetITStatus(TIM2,TIM_IT_Update) == SET){
        LED1_Turn();
        // 清除中断标志位
        TIM_ClearITPendingBit(TIM2,TIM_IT_Update);
    }
}
```

### 外部时钟模式 2

 以 A0 口的复用 ETR 功能为例，需要额外配置 GPIO 口，并将时钟源改成外部时钟模式 2, 可以适当更改预分频器和自动重载器的值

-   外部时钟模式 2 的配置库函数

```c
/**
  * @brief  Configures the External clock Mode2
  * @param  TIMx: where x can be  1, 2, 3, 4, 5 or 8 to select the TIM peripheral.
  * @param  TIM_ExtTRGPrescaler: The external Trigger Prescaler.
  *   This parameter can be one of the following values:
  *     @arg TIM_ExtTRGPSC_OFF: ETRP Prescaler OFF.
  *     @arg TIM_ExtTRGPSC_DIV2: ETRP frequency divided by 2.
  *     @arg TIM_ExtTRGPSC_DIV4: ETRP frequency divided by 4.
  *     @arg TIM_ExtTRGPSC_DIV8: ETRP frequency divided by 8.
  * @param  TIM_ExtTRGPolarity: The external Trigger Polarity.
  *   This parameter can be one of the following values:
  *     @arg TIM_ExtTRGPolarity_Inverted: active low or falling edge active.
  *     @arg TIM_ExtTRGPolarity_NonInverted: active high or rising edge active.
  * @param  ExtTRGFilter: External Trigger Filter.
  *   This parameter must be a value between 0x00 and 0x0F
  * @retval None
  */
void TIM_ETRClockMode2Config(TIM_TypeDef* TIMx, uint16_t TIM_ExtTRGPrescaler, 
                             uint16_t TIM_ExtTRGPolarity, uint16_t ExtTRGFilter)

```

-   TIM_ExtTRGPrescaler

     

    表示是否需要预分配，可以选择不分配，2,4,8 分频

    -   @arg TIM_ExtTRGPSC_OFF: ETRP Prescaler OFF.
    -   @arg TIM_ExtTRGPSC_DIV2: ETRP frequency divided by 2.
    -   @arg TIM_ExtTRGPSC_DIV4: ETRP frequency divided by 4.
    -   @arg TIM_ExtTRGPSC_DIV8: ETRP frequency divided by 8.

-   TIM_ExtTRGPolarity

     

    表示触发方式，可以选择低电平下降沿触发或者高电平上升沿触发

    

-   @arg TIM_ExtTRGPolarity_Inverted: active low or falling edge active.

    -   @arg TIM_ExtTRGPolarity_NonInverted: active high or rising edge active.
    -   **ExtTRGFilter** 表示滤波器的采样频率，值范围为 0x00~0x0F

示例代码:

```c
void Timer_ETRCLock2Init(void){
 
    // 开始时钟使能
    RCC_APB1PeriphClockCmd(RCC_APB1Periph_TIM2,ENABLE);
    RCC_APB1PeriphClockCmd(RCC_APB2Periph_GPIOA,ENABLE);
 
    //A10引脚为ETR复用引脚
    GPIO_InitTypeDef GPIO_InitStructure = {
        .GPIO_Pin = GPIO_Pin_0,
        .GPIO_Mode = GPIO_Mode_IPU,
        .GPIO_Speed = GPIO_Speed_50MHz
    };
    GPIO_Init(GPIOA,&GPIO_InitStructure);
 
    // 使用外部时钟2
    TIM_ETRClockMode2Config(TIM2,TIM_ExtTRGPSC_OFF,TIM_ExtTRGPolarity_NonInverted,0x00);
 
    TIM_TimeBaseInitTypeDef TIM_TimeBaseInitStructure = {
        .TIM_ClockDivision = TIM_CKD_DIV1,  
        .TIM_CounterMode = TIM_CounterMode_Up, // 向上计数
        .TIM_Period = 10 - 1,
        .TIM_Prescaler = 1 - 1,
        .TIM_RepetitionCounter = 0
    };
    // 配置时基单元
    TIM_TimeBaseInit(TIM2,&TIM_TimeBaseInitStructure);
    TIM_ClearITPendingBit(TIM2,TIM_IT_Update);
    // 更新中断
    TIM_ITConfig(TIM2,TIM_IT_Update,ENABLE);
 
    // 配置NVIC
    NVIC_PriorityGroupConfig(NVIC_PriorityGroup_2);
 
    NVIC_InitTypeDef NVIC_InitStructure = {
        .NVIC_IRQChannel = TIM2_IRQn,
        .NVIC_IRQChannelCmd = ENABLE,
        .NVIC_IRQChannelPreemptionPriority = 2,
        .NVIC_IRQChannelSubPriority = 1
 
    };
 
    NVIC_Init(&NVIC_InitStructure);
 
    // 启动时钟
    TIM_Cmd(TIM2,ENABLE);
}
```

### 输出比较

-   OC (Output Compare) 输出比较
-   输出比较可以通过比较 CNT 与 CCR 寄存器值的关系，来对输出电平进行置 1, 置 0 或翻转的操作，用于输出一定频率和占空比的 PWM 波形
-   每个高级定时器和通用定时器都拥有 4 个输出比较波形
-   高级定时器的前 3 个通道额外拥有死区生成和互补输出的功能

![](https://cdn.jsdelivr.net/gh/xuezhaorong/Picgo//Source/fix-dir/STM32/%E5%BA%93%E5%87%BD%E6%95%B0%E5%BC%80%E5%8F%91/2024/05/12/20-32-44-7c49c84fdd3c6ce72a060ff14efec945-PWM-870f5f.png)

 当 CNT 大于等于 CCR1 时，给输出模式控制器传入信号，输出模式控制器根据控制模式改变输出 OC1ref (参考信号) 的高低电平，经过极性选择选择翻转或不翻转电平，通过输出选择电路在 OC1GPIO 引脚输出。

![](https://cdn.jsdelivr.net/gh/xuezhaorong/Picgo//Source/fix-dir/STM32/%E5%BA%93%E5%87%BD%E6%95%B0%E5%BC%80%E5%8F%91/2024/05/12/20-33-02-c93c3e73cba84c7ef5806c1fe57a789b-%E8%BE%93%E5%87%BA%E6%AF%94%E8%BE%83%E9%80%9A%E9%81%93-88269d.png)

 8 种控制模式

![](https://cdn.jsdelivr.net/gh/xuezhaorong/Picgo//Source/fix-dir/STM32/%E5%BA%93%E5%87%BD%E6%95%B0%E5%BC%80%E5%8F%91/2024/05/12/20-33-32-93317bb8a567a29b9005953fd32f00cf-%E8%BE%93%E5%87%BA%E6%AF%94%E8%BE%83%E6%A8%A1%E5%BC%8F-0b899a.png)

PWM 基本结构

 计数器 CNT 不断增加，当 CCR 小于等于 CNT 时，REF 置高电平，当 CCR 大于 CNT 时，REF 置低电平，在 CNT 由 0 到重装值的一个周期内，只需要调整 CCR 的值，就可以设置不同的占空比，当调整 PSC 和 ARR 的频率时，CNT 的频率改变，CCR 的频率也发生改变，这样就能输出不同频率，不同占空比的 PWM 信号。

![](https://cdn.jsdelivr.net/gh/xuezhaorong/Picgo//Source/fix-dir/STM32/%E5%BA%93%E5%87%BD%E6%95%B0%E5%BC%80%E5%8F%91/2024/05/12/20-33-47-b89bdec454e0e04dd1e7ab2def1dade5-PWM%E5%9F%BA%E6%9C%AC%E7%BB%93%E6%9E%84-150269.png)

-   通道引脚

| 通道 | 引脚 | 定义         |
| ---- | ---- | ------------ |
| 1    | P0   | TIM2_CH1_ETR |
| 2    | P1   | TIM2_CH2     |
| 3    | P2   | TIM2_CH3     |
| 4    | P3   | TIM2_CH4     |

**库函数配置:**

 在配置定时器之后，不需要配置中断部分

-   先根据引脚复用表选择对应输出比较通道引脚进行配置 (TIM_CH), 需要配置成复用推挽输出模式

```c
// RCC APB2外设时钟控制
RCC_APB2PeriphClockCmd(RCC_APB2Periph_GPIOA,ENABLE);
 
GPIO_InitTypeDef GPTOA_InitStructure = {
    .GPIO_Mode = GPIO_Mode_AF_PP, // 复用推挽输出
    .GPIO_Pin = GPIO_Pin_1, // A1
    .GPIO_Speed = GPIO_Speed_50MHz      
};
```

-   根据 PWM 公式进行自动重装器和预分配器的配置

![](https://cdn.jsdelivr.net/gh/xuezhaorong/Picgo//Source/fix-dir/STM32/%E5%BA%93%E5%87%BD%E6%95%B0%E5%BC%80%E5%8F%91/2024/05/12/20-34-04-4a315f9a39213bc918e8e4737d64b4b0-PWM%E5%8F%82%E6%95%B0%E8%AE%A1%E7%AE%97-638deb.png)

```c
// 使用内部时钟
TIM_InternalClockConfig(TIM2);
 
TIM_TimeBaseInitTypeDef TIM_TimeBaseInitStructure = {
    .TIM_ClockDivision = TIM_CKD_DIV1,  
    .TIM_CounterMode = TIM_CounterMode_Up, // 向上计数
    .TIM_Period = 20000 - 1,
    .TIM_Prescaler = 72 - 1,
    .TIM_RepetitionCounter = 0
};
// 配置时基单元
TIM_TimeBaseInit(TIM2,&TIM_TimeBaseInitStructure);
```

-   配置输出比较单元，注意四个通道不一样，以 1 通道为例

```c
/**
  * @brief  Initializes the TIMx Channel1 according to the specified
  *         parameters in the TIM_OCInitStruct.
  * @param  TIMx: where x can be  1 to 17 except 6 and 7 to select the TIM peripheral.
  * @param  TIM_OCInitStruct: pointer to a TIM_OCInitTypeDef structure
  *         that contains the configuration information for the specified TIM peripheral.
  * @retval None
  */
void TIM_OC1Init(TIM_TypeDef* TIMx, TIM_OCInitTypeDef* TIM_OCInitStruct)

```

**TIM_OCInitTypeDef** 结构体原型:

```c
typedef struct
{
  uint16_t TIM_OCMode;        /*!< Specifies the TIM mode.
                                   This parameter can be a value of @ref TIM_Output_Compare_and_PWM_modes */
 
  uint16_t TIM_OutputState;   /*!< Specifies the TIM Output Compare state.
                                   This parameter can be a value of @ref TIM_Output_Compare_state */
 
  uint16_t TIM_OutputNState;  /*!< Specifies the TIM complementary Output Compare state.
                                   This parameter can be a value of @ref TIM_Output_Compare_N_state
                                   @note This parameter is valid only for TIM1 and TIM8. */
 
  uint16_t TIM_Pulse;         /*!< Specifies the pulse value to be loaded into the Capture Compare Register. 
                                   This parameter can be a number between 0x0000 and 0xFFFF */
 
  uint16_t TIM_OCPolarity;    /*!< Specifies the output polarity.
                                   This parameter can be a value of @ref TIM_Output_Compare_Polarity */
 
  uint16_t TIM_OCNPolarity;   /*!< Specifies the complementary output polarity.
                                   This parameter can be a value of @ref TIM_Output_Compare_N_Polarity
                                   @note This parameter is valid only for TIM1 and TIM8. */
 
  uint16_t TIM_OCIdleState;   /*!< Specifies the TIM Output Compare pin state during Idle state.
                                   This parameter can be a value of @ref TIM_Output_Compare_Idle_State
                                   @note This parameter is valid only for TIM1 and TIM8. */
 
  uint16_t TIM_OCNIdleState;  /*!< Specifies the TIM Output Compare pin state during Idle state.
                                   This parameter can be a value of @ref TIM_Output_Compare_N_Idle_State
                                   @note This parameter is valid only for TIM1 and TIM8. */
} TIM_OCInitTypeDef;
```

在通用定时器下，只有 **TIM_OCMode,TIM_OCPolarity,TIM_OutputState ,TIM_Pulse** 四个参数有效，，，，

TIM_OCMode 用于设置输出比较模式

```C
#define TIM_OCMode_Timing                  ((uint16_t)0x0000)
#define TIM_OCMode_Active                  ((uint16_t)0x0010)
#define TIM_OCMode_Inactive                ((uint16_t)0x0020)
#define TIM_OCMode_Toggle                  ((uint16_t)0x0030)
#define TIM_OCMode_PWM1                    ((uint16_t)0x0060)
#define TIM_OCMode_PWM2                    ((uint16_t)0x0070)
```

TIM_OCPolarity 用于设置输出比较的极性

```c
#define TIM_OCPolarity_High                ((uint16_t)0x0000)
#define TIM_OCPolarity_Low                 ((uint16_t)0x0002)
```

TIM_OutputState 用于设置输出使能

```c
#define TIM_OutputState_Disable            ((uint16_t)0x0000)
#define TIM_OutputState_Enable             ((uint16_t)0x0001)
```

TIM_Pulse 用于设置 CCR 寄存器的值，范围在 0x0000 and 0xFFFF 之间

示例代码:

```C
// 配置输出比较单元
TIM_OCInitTypeDef TIM_OCInitStructure;
TIM_OCStructInit(&TIM_OCInitStructure); //   赋初始值
TIM_OCInitStructure.TIM_OCMode = TIM_OCMode_PWM1;
TIM_OCInitStructure.TIM_OCPolarity = TIM_OCPolarity_High;
TIM_OCInitStructure.TIM_OutputState = TIM_OutputState_Enable;
TIM_OCInitStructure.TIM_Pulse = 0; // CCR
TIM_OC1Init(TIM2,&TIM_OCInitStructure);
```

这里的 TIM_Pulse 设为 0, 方便调用 **TIM_SetCompare1** 函数进行设置

, 最好用 **TIM_OCStructInit** 函数初始化一下结构体，防止后续需要另外定时器时出现配置冲突

-   最后开启时钟

```c
// 启动定时器
TIM_Cmd(TIM2,ENABLE);
```

-   可以通过 **TIM_SetCompare1** 函数设置 CCR 的值

```c
void PWM_SetCompare1(uint16_t Compare){
    TIM_SetCompare1(TIM2,Compare);
}
```

-   通过 TIM_PrescalerConfigs 设置自动重装器的值

```c
void PWM_SetPrescaler(uint16_t Prescaler){
    TIM_PrescalerConfig(TIM2,Prescaler,TIM_PSCReloadMode_Immediate); // 立即重装1
}
```

### 主从触发模式

![](https://cdn.jsdelivr.net/gh/xuezhaorong/Picgo//Source/fix-dir/STM32/%E5%BA%93%E5%87%BD%E6%95%B0%E5%BC%80%E5%8F%91/2024/05/12/20-34-20-8d9911972140a67ccb63b42b30903b73-%E4%B8%BB%E4%BB%8E%E8%A7%A6%E5%8F%91%E6%A8%A1%E5%BC%8F-7b919b.png)

-   主模式将定时器内部信号映射到 TRGO 引脚，用于触发别的外设
-   从模式接收自身或其他外设的一些信号，，用于控制自身定时器的运行

### 输入捕获

-   IC (Input Capture) 输入捕获
-   输入捕获模式下，当通道输入引脚出现指定电平跳变时，当前 CNT 的值将被所存到 CCR 中，可用于测量 PWM 波形的频率，占空比，脉冲间隔，电平持续时间等参数
-   可配置为 PWMI 模式，同时测量频率和占空比
-   可配合主从触发模式，实现硬件全自动测量
-   选择从模式触发源
-   选择触发源之后的操作
-   开启定时器

**频率测量:**

![](https://cdn.jsdelivr.net/gh/xuezhaorong/Picgo//Source/fix-dir/STM32/%E5%BA%93%E5%87%BD%E6%95%B0%E5%BC%80%E5%8F%91/2024/05/12/20-35-52-499ad8e6d6db5451e2adb1af68f4fb81-%E6%B5%8B%E9%87%8F%E9%A2%91%E7%8E%87-09c88a.png)

 测量频率有两种方法: **测频法**和**测周法** , 测频法先规定一个闸门时间 T, 记录上升沿的次数，则一个时钟周期就是 T/N, 频率则为 N/T, 测周法用标准频率计算两个上升沿之间的次数，则一个周期为 N * 1/f, 频率为 f/N。

 测频法适合测量高频信号，在闸门时间内，计次数量越多，误差越小，测周法适合测量低频信号，低频信号周期较长，时钟信号计次就会更多，误差更小，中界频率则是测频法和测周法的 N 相同时得出的频率，当待测频率大于中界频率时用测频法，小于中界频率时用测周法。

**输入捕获内部结构:**

![](https://cdn.jsdelivr.net/gh/xuezhaorong/Picgo//Source/fix-dir/STM32/%E5%BA%93%E5%87%BD%E6%95%B0%E5%BC%80%E5%8F%91/2024/05/12/20-36-06-d32636e16bc6a5af3e5f54466dcdc6f9-%E8%BE%93%E5%85%A5%E6%8D%95%E8%8E%B7%E9%80%9A%E9%81%93-25dfe7.png)

 信号从引脚经过滤波器，滤波器以采样频率对输入信号进行采样，当连续 N 个值都为高电平，输出才为高电平，当连续 N 个值为低电平，输出才为低电平，若信号出现高频抖动，导致连续采样 N 个值不全都一样，则输出不会变化，这样可以达到滤波的效果，. 采样频率越低，采样个数 N 越大，滤波效果越好。滤波之后的信号通过边沿检测器捕获上升沿或下降沿，最终得到触发信号，通过数据选择器通过后续的捕获电路。还可以选择交叉连接，使 CH1 路的触发信号连接到 CH2, 连接到 CH2 的后续。后续的 CC1S 寄存器对触发信号进行选择，通过分频器，接着进行配置输出使能，就能使得 CNT 的值转运至 CCR 中去。其中 TI1FP1 信号可以同时触发从模式，自动完成 CNT 的清零。

**输入捕获基本模式测量频率:**

![](https://cdn.jsdelivr.net/gh/xuezhaorong/Picgo//Source/fix-dir/STM32/%E5%BA%93%E5%87%BD%E6%95%B0%E5%BC%80%E5%8F%91/2024/05/12/20-36-29-0089452a0dc9c544b270fed354ac3f51-%E8%BE%93%E5%85%A5%E6%8D%95%E8%8E%B7%E5%9F%BA%E6%9C%AC%E7%BB%93%E6%9E%84-6806ef.png)

 输入捕获基本模式不使用交叉连接的方式，只使用了一个通道，用于测量频率，时基单元 CNT 作为测周法的计次单元，GPIO 口输入的信号经过滤波器和边沿检测，选择 TI1FP1 为上升沿触发，选择直连通道，分频器不分频，这样当 TI1FP1 出现上升沿之后，CNT 的当前计数值转运到 CCR 里，同时触发源选择 TI1FP1 为触发信号，通过复位操作的从模式将 CNT 的值清 0。

**输入捕获 PWMI 模式:**

![](https://cdn.jsdelivr.net/gh/xuezhaorong/Picgo//Source/fix-dir/STM32/%E5%BA%93%E5%87%BD%E6%95%B0%E5%BC%80%E5%8F%91/2024/05/12/20-37-01-b6907157b1a8dfcf6a1b74b0a3353587-PWMI%E5%9F%BA%E6%9C%AC%E7%BB%93%E6%9E%84-8b1a4d.png)

 输入捕获的 PWMI 模式使用两个通道，可以同时测量周期和占空比，在基本模式下，配置 TI1FP2 为下降沿触发，通过交叉通道，触发通道 2 的捕获单元，这样 CCR2 的值为一个周期内高电平的持续时间。

**输入捕获基本模式配置:**

-   RCC 开启 GPIO 和 TIM 时钟

```c
// 开始时钟使能
RCC_APB1PeriphClockCmd(RCC_APB1Periph_TIM3,ENABLE);
// RCC APB2外设时钟控制
RCC_APB2PeriphClockCmd(RCC_APB2Periph_GPIOA,ENABLE);
```

-   GPIO 初始化

```c
GPIO_InitTypeDef GPTOA_InitStructure = {
    .GPIO_Mode = GPIO_Mode_IPU, // 上拉输入
    .GPIO_Pin = GPIO_Pin_6, // A6
    .GPIO_Speed = GPIO_Speed_50MHz      
};
 
 
// 初始化
GPIO_Init(GPIOA,&GPTOA_InitStructure);
 
```

-   配置时基单元，让 CNT 计数在内部时钟的驱动下直增运行

```c
// 使用内部时钟
TIM_InternalClockConfig(TIM3);
 
TIM_TimeBaseInitTypeDef TIM_TimeBaseInitStructure = {
    .TIM_ClockDivision = TIM_CKD_DIV1,  
    .TIM_CounterMode = TIM_CounterMode_Up, // 向上计数
    .TIM_Period = 65536 - 1,
    .TIM_Prescaler = 72 - 1,
    .TIM_RepetitionCounter = 0
};
// 配置时基单元
TIM_TimeBaseInit(TIM3,&TIM_TimeBaseInitStructure);
```

-   使用 **TIM_ICInit** 配置输入捕获单元

```c
/**
  * @brief  Initializes the TIM peripheral according to the specified
  *         parameters in the TIM_ICInitStruct.
  * @param  TIMx: where x can be  1 to 17 except 6 and 7 to select the TIM peripheral.
  * @param  TIM_ICInitStruct: pointer to a TIM_ICInitTypeDef structure
  *         that contains the configuration information for the specified TIM peripheral.
  * @retval None
  */
void TIM_ICInit(TIM_TypeDef* TIMx, TIM_ICInitTypeDef* TIM_ICInitStruct)

```

TIM_ICInitTypeDef 结构体:

```c
typedef struct
{
 
  uint16_t TIM_Channel;      /*!< Specifies the TIM channel.
                                  This parameter can be a value of @ref TIM_Channel */
 
  uint16_t TIM_ICPolarity;   /*!< Specifies the active edge of the input signal.
                                  This parameter can be a value of @ref TIM_Input_Capture_Polarity */
 
  uint16_t TIM_ICSelection;  /*!< Specifies the input.
                                  This parameter can be a value of @ref TIM_Input_Capture_Selection */
 
  uint16_t TIM_ICPrescaler;  /*!< Specifies the Input Capture Prescaler.
                                  This parameter can be a value of @ref TIM_Input_Capture_Prescaler */
 
  uint16_t TIM_ICFilter;     /*!< Specifies the input capture filter.
                                  This parameter can be a number between 0x0 and 0xF */
} TIM_ICInitTypeDef;
```

TIM_Channel 用于配置输入捕获通道:

```C
/** @defgroup TIM_Channel 
  * @{
  */
 
#define TIM_Channel_1                      ((uint16_t)0x0000)
#define TIM_Channel_2                      ((uint16_t)0x0004)
#define TIM_Channel_3                      ((uint16_t)0x0008)
#define TIM_Channel_4                      ((uint16_t)0x000C)
```

TIM_ICPolarity 用于配置极性选择:

```c
/** @defgroup TIM_Input_Capture_Polarity 
  * @{
  */
 
#define  TIM_ICPolarity_Rising             ((uint16_t)0x0000)
#define  TIM_ICPolarity_Falling            ((uint16_t)0x0002)
#define  TIM_ICPolarity_BothEdge           ((uint16_t)0x000A)
```

TIM_ICSelection 用于配置触发信号的连接方式，直连或者交叉连接

```c
/** @defgroup TIM_Input_Capture_Selection 
  * @{
  */
 
#define TIM_ICSelection_DirectTI           ((uint16_t)0x0001) /*!< TIM Input 1, 2, 3 or 4 is selected to be 
                                                                   connected to IC1, IC2, IC3 or IC4, respectively */
#define TIM_ICSelection_IndirectTI         ((uint16_t)0x0002) /*!< TIM Input 1, 2, 3 or 4 is selected to be
                                                                   connected to IC2, IC1, IC4 or IC3, respectively. */
#define TIM_ICSelection_TRC                ((uint16_t)0x0003) /*!< TIM Input 1, 2, 3 or 4 is selected to be connected to TRC. */
```

TIM_ICPrescaler 用于配置分配器:

```c
/** @defgroup TIM_Input_Capture_Prescaler 
  * @{
  */
 
#define TIM_ICPSC_DIV1                     ((uint16_t)0x0000) /*!< Capture performed each time an edge is detected on the capture input. */
#define TIM_ICPSC_DIV2                     ((uint16_t)0x0004) /*!< Capture performed once every 2 events. */
#define TIM_ICPSC_DIV4                     ((uint16_t)0x0008) /*!< Capture performed once every 4 events. */
#define TIM_ICPSC_DIV8                     ((uint16_t)0x000C) /*!< Capture performed once every 8 events. */
```

TIM_ICFilter 用于配置滤波器，范围 0x0 到 0xF

示例代码:

```C
// 配置输入捕获单元
TIM_ICInitTypeDef TIM_ICInitStructure;
TIM_ICInitStructure.TIM_Channel = TIM_Channel_1; // 通道
TIM_ICInitStructure.TIM_ICFilter = 0xF; // 滤波器
TIM_ICInitStructure.TIM_ICPolarity = TIM_ICPolarity_Rising; // 极性选择
TIM_ICInitStructure.TIM_ICPrescaler = TIM_ICPSC_DIV1; // 不分频
TIM_ICInitStructure.TIM_ICSelection = TIM_ICSelection_DirectTI; // 选择触发信号引脚,直连通道或者交叉通道
TIM_ICInit(TIM3,&TIM_ICInitStructure);
```

-   配置从模式的触发源:

```c
/**
  * @brief  Selects the Input Trigger source
  * @param  TIMx: where x can be  1, 2, 3, 4, 5, 8, 9, 12 or 15 to select the TIM peripheral.
  * @param  TIM_InputTriggerSource: The Input Trigger source.
  *   This parameter can be one of the following values:
  *     @arg TIM_TS_ITR0: Internal Trigger 0
  *     @arg TIM_TS_ITR1: Internal Trigger 1
  *     @arg TIM_TS_ITR2: Internal Trigger 2
  *     @arg TIM_TS_ITR3: Internal Trigger 3
  *     @arg TIM_TS_TI1F_ED: TI1 Edge Detector
  *     @arg TIM_TS_TI1FP1: Filtered Timer Input 1
  *     @arg TIM_TS_TI2FP2: Filtered Timer Input 2
  *     @arg TIM_TS_ETRF: External Trigger input
  * @retval None
  */
void TIM_SelectInputTrigger(TIM_TypeDef* TIMx, uint16_t TIM_InputTriggerSource)

```

TIM_InputTriggerSource 对应着 8 个触发源，这里选择 Internal Trigger 1

```c
TIM_SelectInputTrigger(TIM3,TIM_TS_TI1FP1);
```

-   配置从模式运行方式:

```c
/**
  * @brief  Selects the TIMx Slave Mode.
  * @param  TIMx: where x can be 1, 2, 3, 4, 5, 8, 9, 12 or 15 to select the TIM peripheral.
  * @param  TIM_SlaveMode: specifies the Timer Slave Mode.
  *   This parameter can be one of the following values:
  *     @arg TIM_SlaveMode_Reset: Rising edge of the selected trigger signal (TRGI) re-initializes
  *                               the counter and triggers an update of the registers.
  *     @arg TIM_SlaveMode_Gated:     The counter clock is enabled when the trigger signal (TRGI) is high.
  *     @arg TIM_SlaveMode_Trigger:   The counter starts at a rising edge of the trigger TRGI.
  *     @arg TIM_SlaveMode_External1: Rising edges of the selected trigger (TRGI) clock the counter.
  * @retval None
  */
void TIM_SelectSlaveMode(TIM_TypeDef* TIMx, uint16_t TIM_SlaveMode)

```

这里选择 Reset 模式将 CNT 清 0

```c
// reset
TIM_SelectSlaveMode(TIM3,TIM_SlaveMode_Reset);
```

-   最后开启定时器

```c
// 启动定时器
TIM_Cmd(TIM3,ENABLE);
```

-   可以通过 **TIM_GetCapture1** 函数获取 CCR 里的值

```c
uint32_t IC_GetFreq(void){
    return 1000000 / TIM_GetCapture1(TIM3) + 1;
 
}
```

**PWMI 模式的配置:**

只需将 **TIM_ICInit** 换成 **TIM_PWMIConfig** 即可

```C
TIM_PWMIConfig(TIM3,&TIM_ICInitStructure);
```

通过 **TIM_GetCapture2** 函数获取通道 2 的 CCR 即可计算占空比

### 编码器接口

-   Encoder Interface 编码器接口
-   编码器接口可接收增量 (正交) 编码器的信号，根据编码器旋转产生的正交信号脉冲，自动控制 CNT 自增或自减，从而指示编码器的位置，旋转方向和旋转速度
-   每个高级定时器和通用定时器都拥有 1 个编码器接口
-   两个输入引脚借用了输入捕获的通道 1 和通道 2

![](https://cdn.jsdelivr.net/gh/xuezhaorong/Picgo//Source/fix-dir/STM32/%E5%BA%93%E5%87%BD%E6%95%B0%E5%BC%80%E5%8F%91/2024/05/12/20-37-44-a041cfb148f031050e4ecf96462f0a4a-%E6%AD%A3%E4%BA%A4%E7%BC%96%E7%A0%81%E5%99%A8-8fede2.png)

 输入捕获前两个通道，通过 GPIO 口接入编码器 AB 相，然后通过滤波器和边沿检测极性选择，产生 TI1FP1 和 TI2FP2, 通向编码器接口，编码器接口通过预分配器控制 CNT 计数器的时钟，同时，编码器接口还根据编码器的旋转方向，控制 CNT 的计数方向，编码器正转时，CNT 自增，编码器反转时，CNT 自减，

![](https://cdn.jsdelivr.net/gh/xuezhaorong/Picgo//Source/fix-dir/STM32/%E5%BA%93%E5%87%BD%E6%95%B0%E5%BC%80%E5%8F%91/2024/05/12/20-38-05-892c1860f99bda7554711be05b1eafcf-%E7%BC%96%E7%A0%81%E5%99%A8%E5%9F%BA%E6%9C%AC%E7%BB%93%E6%9E%84-c88be2.png)

编码器接口工作模式

![](https://cdn.jsdelivr.net/gh/xuezhaorong/Picgo//Source/fix-dir/STM32/%E5%BA%93%E5%87%BD%E6%95%B0%E5%BC%80%E5%8F%91/2024/05/12/20-38-27-f91744e0c788f9cab32e989a1dfb7ba4-%E7%BC%96%E7%A0%81%E5%99%A8%E6%8E%A5%E5%8F%A3%E5%B7%A5%E4%BD%9C%E6%A8%A1%E5%BC%8F-3213f5.png)

-   库函数配置
-   配置 GPIO 口

```c
// RCC APB2外设时钟控制
RCC_APB2PeriphClockCmd(RCC_APB2Periph_GPIOA,ENABLE);
 
GPIO_InitTypeDef GPTOA_InitStructure = {
    .GPIO_Mode = GPIO_Mode_IPU, // 上拉输入
    .GPIO_Pin = GPIO_Pin_6 | GPIO_Pin_7, // A6 A7
    .GPIO_Speed = GPIO_Speed_50MHz      
};
// 初始化
GPIO_Init(GPIOA,&GPTOA_InitStructure);
```

-   配置时基单元和输入捕获单元

```c
// 开始时钟使能
RCC_APB1PeriphClockCmd(RCC_APB1Periph_TIM3,ENABLE);
TIM_TimeBaseInitTypeDef TIM_TimeBaseInitStructure = {
    .TIM_ClockDivision = TIM_CKD_DIV1,  
    .TIM_CounterMode = TIM_CounterMode_Up, // 向上计数
    .TIM_Period = 65536 - 1,
    .TIM_Prescaler = 1 - 1,
    .TIM_RepetitionCounter = 0
};
// 配置时基单元
TIM_TimeBaseInit(TIM3,&TIM_TimeBaseInitStructure);
 
 
// 配置输入捕获单元
TIM_ICInitTypeDef TIM_ICInitStructure;
TIM_ICStructInit(&TIM_ICInitStructure); //初始化结构体
TIM_ICInitStructure.TIM_Channel = TIM_Channel_1; // 通道1
TIM_ICInitStructure.TIM_ICFilter = 0xF; // 滤波器
//  TIM_ICInitStructure.TIM_ICPolarity = TIM_ICPolarity_Rising; // 极性选择
 
TIM_ICInit(TIM3,&TIM_ICInitStructure);
 
TIM_ICInitStructure.TIM_Channel = TIM_Channel_2; // 通道1
TIM_ICInitStructure.TIM_ICFilter = 0xF; // 滤波器
//  TIM_ICInitStructure.TIM_ICPolarity = TIM_ICPolarity_Rising; // 极性选择
 
TIM_ICInit(TIM3,&TIM_ICInitStructure);
```

-   配置编码器接口

函数原型

```
/**
  * @brief  Configures the TIMx Encoder Interface.
  * @param  TIMx: where x can be  1, 2, 3, 4, 5 or 8 to select the TIM peripheral.
  * @param  TIM_EncoderMode: specifies the TIMx Encoder Mode.
  *   This parameter can be one of the following values:
  *     @arg TIM_EncoderMode_TI1: Counter counts on TI1FP1 edge depending on TI2FP2 level.
  *     @arg TIM_EncoderMode_TI2: Counter counts on TI2FP2 edge depending on TI1FP1 level.
  *     @arg TIM_EncoderMode_TI12: Counter counts on both TI1FP1 and TI2FP2 edges depending
  *                                on the level of the other input.
  * @param  TIM_IC1Polarity: specifies the IC1 Polarity
  *   This parameter can be one of the following values:
  *     @arg TIM_ICPolarity_Falling: IC Falling edge.
  *     @arg TIM_ICPolarity_Rising: IC Rising edge.
  * @param  TIM_IC2Polarity: specifies the IC2 Polarity
  *   This parameter can be one of the following values:
  *     @arg TIM_ICPolarity_Falling: IC Falling edge.
  *     @arg TIM_ICPolarity_Rising: IC Rising edge.
  * @retval None
  */
void TIM_EncoderInterfaceConfig(TIM_TypeDef* TIMx, uint16_t TIM_EncoderMode,
                                uint16_t TIM_IC1Polarity, uint16_t TIM_IC2Polarity)
```

**TIM_EncoderMode** 为编码器的 3 种工作模式

TIM_IC1Polarity 和 TIM_IC2Polarity 可以选择电平是否翻转

```C
// 配置编码器接口
TIM_EncoderInterfaceConfig(TIM3,TIM_EncoderMode_TI12,TIM_ICPolarity_Rising,TIM_ICPolarity_Rising);
```

-   开启时钟

```c
TIM_Cmd(TIM3, ENABLE);          //使能TIM3，定时器开始运行
```

## ADC 数模转换

-   ADC（Analog-Digital Converter) 模拟 - 数字转换器
-   ADC 可以将引脚下连续变化的模拟电压转换为内存中存储的数字变量，建立模拟电路到数字电路的桥梁
-   12 位逐次逼近型 ADC，1us 转换时间
-   输入电压范围：0~3.3V, 转换结果范围：0 ~ 4095
-   18 个输入通道，可测量 16 个外部和 2 个内部信号源
-   规则组合注入组两个转换单元
-   模拟看门狗自动检测输入电压范围
-   STM32F103C8T6 ADC 资源：ADC1,ADC2,10 个外部输入通道

### 逐次逼近型 ADC

![](https://cdn.jsdelivr.net/gh/xuezhaorong/Picgo//Source/fix-dir/STM32/%E5%BA%93%E5%87%BD%E6%95%B0%E5%BC%80%E5%8F%91/2024/05/12/20-39-14-aa878f3207865a7cca0f892b68f3037b-%E9%80%90%E6%AC%A1%E9%80%BC%E8%BF%91%E5%9E%8BADC-6b3c17.png)

在转换之前发送开始转换信号，根据 CLK 时钟推进转换过程，左侧的 8 路输入通道 IN0 ~ IN7，通过通道选择开关合下面的地址锁存器选中一路，在比较器中与 DAC 输出的已知编码的电压进行大小判断，并且进行调整直到 DAC 输出的电压与外部通道输入的电压近似相等，SAR 通常使用二分法进行寻找调整，最快找到未知电压的编码，在量化编码前打开，采样开关，使用小电容存储外部电压，存储完成断开采样开关，在进行后续的 AD 转换，在量化编码期间，电压保持不变。之后，将 DAC 的输入数据通过缓冲器输出，并且发出转换结束信号。

![](https://cdn.jsdelivr.net/gh/xuezhaorong/Picgo//Source/fix-dir/STM32/%E5%BA%93%E5%87%BD%E6%95%B0%E5%BC%80%E5%8F%91/2024/05/12/20-39-34-a59413e815b6d927439211a9ea8e068a-STM32ADC%E7%BB%93%E6%9E%84%E5%9B%BE-b92ba5.png)

在 STM32 的 ADC 结构图中，包含 0~15 的外部输入通道合两个内部通道：内部温度传感器和 VREFINT (V Reference Internal) 内部参考电压，总共 18 个输入通道，在模拟多路开关中选择通道，然后输出到模数转换器，执行逐次比较的过程，转换结果存放在数据寄存器中。在转换的过程中，将输入通道分成两个组：规则通道组合注入通道组。其中规则组可以一次性最多选择 16 个通道，注入组最多可以选 4 个通道。规则组只有 1 个 16 位数据寄存器，可以配合 DMA 使用， 而注入组则有 4 个 16 位数据寄存器。触发 ADC 开始转换的信号有两种，软件触发和硬件触发，主要来自于定时器的各个通道和 TRGO 定时器主模式的输出。模拟看门狗可以设置数据高限和数据低限，当指定看门通道时，可以检测通道并产生模拟中断。

### ADC 基本结构

![](https://cdn.jsdelivr.net/gh/xuezhaorong/Picgo//Source/fix-dir/STM32/%E5%BA%93%E5%87%BD%E6%95%B0%E5%BC%80%E5%8F%91/2024/05/12/20-39-54-6035aef2f8620532af677d7d73172975-ADC%E5%9F%BA%E6%9C%AC%E7%BB%93%E6%9E%84-717288.png)

16 个 GPIO 口外加两个内部的通道进入 AD 转换器，AD 转换器中含有规则组和注入组，注入组最多可以选择 4 个通道，规则组可以选择 16 个通道，转换的结果可以存放在 AD 数据寄存器里，其中规则组只有 1 个数据寄存器，注入组有 4 个，触发控制提供了开始转换的信号，触发控制可以选择软件触发和硬件触发，硬件触发主要来自于定时器和外部中断的引脚，RCC 的 ADC 时钟 CLK 推动 ADC 的逐次比较。可以布置看门狗用于监测转换结果的范围，如果超出设定的阈值，就通过中断输出控制，向 NVIC 申请中断。另外规则组和注入组转换完成后会有个 EOC 信号，标志位或者通向 NVIC，最后还有开关控制给 ADC 上电。

![](https://cdn.jsdelivr.net/gh/xuezhaorong/Picgo//Source/fix-dir/STM32/%E5%BA%93%E5%87%BD%E6%95%B0%E5%BC%80%E5%8F%91/2024/05/12/20-40-26-6ac3ec0f4df9f1acb8ad582d0182d005-ADC%E8%BE%93%E5%85%A5%E9%80%9A%E9%81%93-9cb988.png)

### 转换模式

-   单次转换，非扫描模式

![](https://cdn.jsdelivr.net/gh/xuezhaorong/Picgo//Source/fix-dir/STM32/%E5%BA%93%E5%87%BD%E6%95%B0%E5%BC%80%E5%8F%91/2024/05/12/20-40-46-2986cc918f8c654550833559c2ad3739-ADC%E5%8D%95%E6%AC%A1%E8%BD%AC%E6%8D%A2%E9%9D%9E%E6%89%AB%E6%8F%8F%E6%A8%A1%E5%BC%8F-e489ae.png)

在每次转换时，需要触发开始信号。

-   连续转换，非扫描模式

![](https://cdn.jsdelivr.net/gh/xuezhaorong/Picgo//Source/fix-dir/STM32/%E5%BA%93%E5%87%BD%E6%95%B0%E5%BC%80%E5%8F%91/2024/05/12/20-41-02-e68b3966443bd4b9be6866772961d7a5-ADC%E8%BF%9E%E7%BB%AD%E8%BD%AC%E6%8D%A2%E9%9D%9E%E6%89%AB%E6%8F%8F%E6%A8%A1%E5%BC%8F-a5be78.png)

仅在工作前触发一次开始信号，ADC 转换会自动不断进行。

-   单次转换，扫描模式

![](https://cdn.jsdelivr.net/gh/xuezhaorong/Picgo//Source/fix-dir/STM32/%E5%BA%93%E5%87%BD%E6%95%B0%E5%BC%80%E5%8F%91/2024/05/12/20-41-18-91e798f48f41e8b3f331f90af8b891c1-ADC%E5%8D%95%E6%AC%A1%E8%BD%AC%E6%8D%A2%E6%89%AB%E6%8F%8F%E6%A8%A1%E5%BC%8F-bf2558.png)

多通道模式，会逐序列的转换。

-   连续转换，扫描模式

![](https://cdn.jsdelivr.net/gh/xuezhaorong/Picgo//Source/fix-dir/STM32/%E5%BA%93%E5%87%BD%E6%95%B0%E5%BC%80%E5%8F%91/2024/05/12/20-41-41-a5ddf22e071742c9cacfdd63a53ee832-ADC%E8%BF%9E%E7%BB%AD%E8%BD%AC%E6%8D%A2%E6%89%AB%E6%8F%8F%E6%A8%A1%E5%BC%8F-8f2c74.png)

### 触发控制

![](https://cdn.jsdelivr.net/gh/xuezhaorong/Picgo//Source/fix-dir/STM32/%E5%BA%93%E5%87%BD%E6%95%B0%E5%BC%80%E5%8F%91/2024/05/12/20-41-57-95d3ad29885a6ce23ff4e3dee3fd1d17-ADC%E8%A7%A6%E5%8F%91%E6%8E%A7%E5%88%B6-ad5feb.png)

### 数据对齐

![](https://cdn.jsdelivr.net/gh/xuezhaorong/Picgo//Source/fix-dir/STM32/%E5%BA%93%E5%87%BD%E6%95%B0%E5%BC%80%E5%8F%91/2024/05/12/20-42-47-22063790201fa319639d21329117b207-%E6%95%B0%E6%8D%AE%E9%98%9F%E9%BD%90-1f6f4e.png)

### 转换时间

-   AD 转换的步骤：采样，保持，量化，编码
-   STM32ADC 的总转换时间为：

$$
T_{CONV} = 采样时间 + 12.5 个 ADC 周期
$$

采样时间越大，可以避免毛刺信号的干扰并增加转换时间。

### 校准

-   ADC 有一个内置自校准模式。校准可大幅减小因内部电容器组的变化而造成的精准度误差。校准期间，在每个电容器上都会计算出一个误差修正码（数字值），这个码用于消除在随后的转换中每个电容器上产生的误差。
-   在每次上电后执行一次校准。
-   启动校准前，ADC 必须处于关电状态超过至少两个 ADCshi’zhon

### 配置过程

1.  开启 RCC 时钟，包括 ADC 和 GPIO 的时钟，配置 ADCCLK 的分频时钟

```c
// 初始化RCC使能
RCC_APB2PeriphClockCmd(RCC_APB2Periph_ADC1,ENABLE);
RCC_APB2PeriphClockCmd(RCC_APB2Periph_GPIOA,ENABLE);
// 配置ADCCLK
RCC_ADCCLKConfig(RCC_PCLK2_Div6);
```

RCC_ADCCLKConfigw 用来配置 ADC 的时钟，函数原型：

```c
/**
  * @brief  Configures the ADC clock (ADCCLK).
  * @param  RCC_PCLK2: defines the ADC clock divider. This clock is derived from 
  *   the APB2 clock (PCLK2).
  *   This parameter can be one of the following values:
  *     @arg RCC_PCLK2_Div2: ADC clock = PCLK2/2
  *     @arg RCC_PCLK2_Div4: ADC clock = PCLK2/4
  *     @arg RCC_PCLK2_Div6: ADC clock = PCLK2/6
  *     @arg RCC_PCLK2_Div8: ADC clock = PCLK2/8
  * @retval None
  */
void RCC_ADCCLKConfig(uint32_t RCC_PCLK2)

```

可选择 2,4,6,8 分频，最高频率为 14MHZ

1.  配置 GPIO 为模拟输入模式

```c
// 初始化GPIO为AIN模拟输入模式
GPIO_InitTypeDef GPIO_InitStructure;
GPIO_InitStructure.GPIO_Mode = GPIO_Mode_AIN;
GPIO_InitStructure.GPIO_Pin = GPIO_Pin_0;
GPIO_InitStructure.GPIO_Speed = GPIO_Speed_50MHz;
GPIO_Init(GPIOA,&GPIO_InitStructure);
```

将 GPIO 口设置为模拟输入模式，避免输入输出对 GPIO 口造成影响

1.  配置多路开关，将左边的通道接入到右边的规则组里

```c
ADC_RegularChannelConfig(ADC1,ADC_Channel_0,1,ADC_SampleTime_55Cycles5);
```

函数原型：

```c
/**
  * @brief  Configures for the selected ADC regular channel its corresponding
  *         rank in the sequencer and its sample time.
  * @param  ADCx: where x can be 1, 2 or 3 to select the ADC peripheral.
  * @param  ADC_Channel: the ADC channel to configure. 
  *   This parameter can be one of the following values:
  *     @arg ADC_Channel_0: ADC Channel0 selected
  *     @arg ADC_Channel_1: ADC Channel1 selected
  *     @arg ADC_Channel_2: ADC Channel2 selected
  *     @arg ADC_Channel_3: ADC Channel3 selected
  *     @arg ADC_Channel_4: ADC Channel4 selected
  *     @arg ADC_Channel_5: ADC Channel5 selected
  *     @arg ADC_Channel_6: ADC Channel6 selected
  *     @arg ADC_Channel_7: ADC Channel7 selected
  *     @arg ADC_Channel_8: ADC Channel8 selected
  *     @arg ADC_Channel_9: ADC Channel9 selected
  *     @arg ADC_Channel_10: ADC Channel10 selected
  *     @arg ADC_Channel_11: ADC Channel11 selected
  *     @arg ADC_Channel_12: ADC Channel12 selected
  *     @arg ADC_Channel_13: ADC Channel13 selected
  *     @arg ADC_Channel_14: ADC Channel14 selected
  *     @arg ADC_Channel_15: ADC Channel15 selected
  *     @arg ADC_Channel_16: ADC Channel16 selected
  *     @arg ADC_Channel_17: ADC Channel17 selected
  * @param  Rank: The rank in the regular group sequencer. This parameter must be between 1 to 16.
  * @param  ADC_SampleTime: The sample time value to be set for the selected channel. 
  *   This parameter can be one of the following values:
  *     @arg ADC_SampleTime_1Cycles5: Sample time equal to 1.5 cycles
  *     @arg ADC_SampleTime_7Cycles5: Sample time equal to 7.5 cycles
  *     @arg ADC_SampleTime_13Cycles5: Sample time equal to 13.5 cycles
  *     @arg ADC_SampleTime_28Cycles5: Sample time equal to 28.5 cycles 
  *     @arg ADC_SampleTime_41Cycles5: Sample time equal to 41.5 cycles 
  *     @arg ADC_SampleTime_55Cycles5: Sample time equal to 55.5 cycles 
  *     @arg ADC_SampleTime_71Cycles5: Sample time equal to 71.5 cycles 
  *     @arg ADC_SampleTime_239Cycles5: Sample time equal to 239.5 cycles   
  * @retval None
  */
void ADC_RegularChannelConfig(ADC_TypeDef* ADCx, uint8_t ADC_Channel, uint8_t Rank, uint8_t ADC_SampleTime)

```

第一个参数 **ADCx** 用于选择 ADC1，2，3，第二个参数 **ADC_Channel** 用于选择 ADC 通道 0~17，第三个参数 **Rank** 用于选择对应通道位于的序号，可选择 1 ~ 16，第四个参数 **ADC_SampleTime** 用于设置采样时间。

1.  配置 ADC 转换器

```c
// 初始化ADC
ADC_InitTypeDef ADC_InitStructure;
ADC_InitStructure.ADC_Mode = ADC_Mode_Independent; // 独立模式
ADC_InitStructure.ADC_DataAlign = ADC_DataAlign_Right; // 右对齐
ADC_InitStructure.ADC_ExternalTrigConv = ADC_ExternalTrigConv_None; // 触发控制的触发源 使用软件触发
ADC_InitStructure.ADC_ContinuousConvMode = DISABLE; // 单次转换
ADC_InitStructure.ADC_ScanConvMode = DISABLE; // 非扫描
ADC_InitStructure.ADC_NbrOfChannel = 1;
ADC_Init(ADC1,&ADC_InitStructure);
```

结构体原型：

```c
typedef struct
{
  uint32_t ADC_Mode;                      /*!< Configures the ADC to operate in independent or
                                               dual mode. 
                                               This parameter can be a value of @ref ADC_mode */
 
  FunctionalState ADC_ScanConvMode;       /*!< Specifies whether the conversion is performed in
                                               Scan (multichannels) or Single (one channel) mode.
                                               This parameter can be set to ENABLE or DISABLE */
 
  FunctionalState ADC_ContinuousConvMode; /*!< Specifies whether the conversion is performed in
                                               Continuous or Single mode.
                                               This parameter can be set to ENABLE or DISABLE. */
 
  uint32_t ADC_ExternalTrigConv;          /*!< Defines the external trigger used to start the analog
                                               to digital conversion of regular channels. This parameter
                                               can be a value of @ref ADC_external_trigger_sources_for_regular_channels_conversion */
 
  uint32_t ADC_DataAlign;                 /*!< Specifies whether the ADC data alignment is left or right.
                                               This parameter can be a value of @ref ADC_data_align */
 
  uint8_t ADC_NbrOfChannel;               /*!< Specifies the number of ADC channels that will be converted
                                               using the sequencer for regular channel group.
                                               This parameter must range from 1 to 16. */
}ADC_InitTypeDef;
```

-   **ADC_Mode** 可选择 ADC 的模式，常用第一个 **ADC_Mode_Independent** 单 ADC 模式

```c
/** @defgroup ADC_mode 
  * @{
  */
 
#define ADC_Mode_Independent                       ((uint32_t)0x00000000)
#define ADC_Mode_RegInjecSimult                    ((uint32_t)0x00010000)
#define ADC_Mode_RegSimult_AlterTrig               ((uint32_t)0x00020000)
#define ADC_Mode_InjecSimult_FastInterl            ((uint32_t)0x00030000)
#define ADC_Mode_InjecSimult_SlowInterl            ((uint32_t)0x00040000)
#define ADC_Mode_InjecSimult                       ((uint32_t)0x00050000)
#define ADC_Mode_RegSimult                         ((uint32_t)0x00060000)
#define ADC_Mode_FastInterl                        ((uint32_t)0x00070000)
#define ADC_Mode_SlowInterl                        ((uint32_t)0x00080000)
#define ADC_Mode_AlterTrig                         ((uint32_t)0x00090000)
```

-   **ADC_DataAlign** 用于设置数据队齐方式，可选择右对齐和左对齐，一般使用 **ADC_DataAlign_Right** 右对齐

```c
/** @defgroup ADC_data_align 
  * @{
  */
 
#define ADC_DataAlign_Right                        ((uint32_t)0x00000000)
#define ADC_DataAlign_Left                         ((uint32_t)0x00000800)
```

-   **ADC_ExternalTrigConv** 用于选择控制触发方式，使用 **ADC_ExternalTrigConv_None** 软件触发方式

```c
/** @defgroup ADC_external_trigger_sources_for_regular_channels_conversion 
  * @{
  */
 
#define ADC_ExternalTrigConv_T1_CC1                ((uint32_t)0x00000000) /*!< For ADC1 and ADC2 */
#define ADC_ExternalTrigConv_T1_CC2                ((uint32_t)0x00020000) /*!< For ADC1 and ADC2 */
#define ADC_ExternalTrigConv_T2_CC2                ((uint32_t)0x00060000) /*!< For ADC1 and ADC2 */
#define ADC_ExternalTrigConv_T3_TRGO               ((uint32_t)0x00080000) /*!< For ADC1 and ADC2 */
#define ADC_ExternalTrigConv_T4_CC4                ((uint32_t)0x000A0000) /*!< For ADC1 and ADC2 */
#define ADC_ExternalTrigConv_Ext_IT11_TIM8_TRGO    ((uint32_t)0x000C0000) /*!< For ADC1 and ADC2 */
 
#define ADC_ExternalTrigConv_T1_CC3                ((uint32_t)0x00040000) /*!< For ADC1, ADC2 and ADC3 */
#define ADC_ExternalTrigConv_None                  ((uint32_t)0x000E0000) /*!< For ADC1, ADC2 and ADC3 */
 
#define ADC_ExternalTrigConv_T3_CC1                ((uint32_t)0x00000000) /*!< For ADC3 only */
#define ADC_ExternalTrigConv_T2_CC3                ((uint32_t)0x00020000) /*!< For ADC3 only */
#define ADC_ExternalTrigConv_T8_CC1                ((uint32_t)0x00060000) /*!< For ADC3 only */
#define ADC_ExternalTrigConv_T8_TRGO               ((uint32_t)0x00080000) /*!< For ADC3 only */
#define ADC_ExternalTrigConv_T5_CC1                ((uint32_t)0x000A0000) /*!< For ADC3 only */
#define ADC_ExternalTrigConv_T5_CC3                ((uint32_t)0x000C0000) /*!< For ADC3 only */
```

-   **ADC_ContinuousConvMode** 参数可设置是否选择连续转换
-   **ADC_ScanConvMode** 参数可设置是否选择扫描模式
-   **ADC_NbrOfChannel** 参数设置在扫描模式下转换多少个序列，非扫描模式下无作用

1.  开关控制，开启 ADC

```c
// 开启ADC电源
ADC_Cmd(ADC1,ENABLE);
```

1.  对 ADC 进行校准

```c
// 校准
ADC_ResetCalibration(ADC1); // 复位校准
while(ADC_GetResetCalibrationStatus(ADC1)); // 等待复位校准完成 校准标志位由硬件自动清0
ADC_StartCalibration(ADC1); // 开始校准
while (ADC_GetCalibrationStatus(ADC1)); // 等待校准完成
```

1.  获取 ADC 转换值

```c
uint16_t AD_GetValue(void){
    ADC_SoftwareStartConvCmd(ADC1,ENABLE); // 触发AD转换
    while(!ADC_GetFlagStatus(ADC1,ADC_FLAG_EOC)); // 等待规则组转换完成
    return ADC_GetConversionValue(ADC1);
}
```

例子：

```c
void AD_Init(void){
    // 初始化RCC使能
    RCC_APB2PeriphClockCmd(RCC_APB2Periph_ADC1,ENABLE);
    RCC_APB2PeriphClockCmd(RCC_APB2Periph_GPIOA,ENABLE);
    // 配置ADCCLK
    RCC_ADCCLKConfig(RCC_PCLK2_Div6);
    // 初始化GPIO为AIN模拟输入模式
    GPIO_InitTypeDef GPIO_InitStructure;
    GPIO_InitStructure.GPIO_Mode = GPIO_Mode_AIN;
    GPIO_InitStructure.GPIO_Pin = GPIO_Pin_0;
    GPIO_InitStructure.GPIO_Speed = GPIO_Speed_50MHz;
    GPIO_Init(GPIOA,&GPIO_InitStructure);
    // 配置规则组
    ADC_RegularChannelConfig(ADC1,ADC_Channel_0,1,ADC_SampleTime_55Cycles5);
    // 初始化ADC
    ADC_InitTypeDef ADC_InitStructure;
    ADC_InitStructure.ADC_Mode = ADC_Mode_Independent; // 独立模式
    ADC_InitStructure.ADC_DataAlign = ADC_DataAlign_Right; // 右对齐
    ADC_InitStructure.ADC_ExternalTrigConv = ADC_ExternalTrigConv_None; // 触发控制的触发源 使用软件触发
    ADC_InitStructure.ADC_ContinuousConvMode = DISABLE; // 单次转换
    ADC_InitStructure.ADC_ScanConvMode = DISABLE; // 非扫描
    ADC_InitStructure.ADC_NbrOfChannel = 1;
    ADC_Init(ADC1,&ADC_InitStructure);
    // 开启ADC电源
    ADC_Cmd(ADC1,ENABLE);
    // 校准
    ADC_ResetCalibration(ADC1); // 复位校准
    while(ADC_GetResetCalibrationStatus(ADC1)); // 等待复位校准完成 校准标志位由硬件自动清0
    ADC_StartCalibration(ADC1); // 开始校准
    while (ADC_GetCalibrationStatus(ADC1)); // 等待校准完成
 
}
 
uint16_t AD_GetValue(void){
    ADC_SoftwareStartConvCmd(ADC1,ENABLE); // 触发AD转换
    while(!ADC_GetFlagStatus(ADC1,ADC_FLAG_EOC)); // 等待规则组转换完成
    return ADC_GetConversionValue(ADC1);
}
```

如果要使用连续模式，可以将 **ADC_ContinuousConvMode** 的值改为 **ENABLE**，将软件触发的代码放到初始化函数的最后，并且去掉等待规则组完成的 while 循环，直接返回 ADC 转换值，要读取时直接调用函数即可。

```c
void AD_Init(void){
    ...
    ADC_InitStructure.ADC_ContinuousConvMode = ENABLE   ; // 连续转换
    ...
    ADC_SoftwareStartConvCmd(ADC1,ENABLE); // 触发AD转换
}
 
uint16_t AD_GetValue(void){
    return ADC_GetConversionValue(ADC1);
}
```

## 串口

### USART 简介

-   USART(Univeral Synchronous/Asynchtonous Receiver/Transmitter)
-   USART 是 STM32 内部集成的硬件外设，可根据数据寄存器的一个直接数据自动生成数据帧时序，从 TX 引脚发送出去，也可以接收 RX 引脚的数据帧时序，拼接为一个字节数据，存放在数据寄存器里
-   自带波特率发生器，最高达 4.5Mbits/s
-   可配置数据位长度 (8/9)，停止位长度 (0.5/1/1.5/2)
-   可选校验位（无校验 / 奇校验 / 偶校验）
-   支持同步模式，硬件流控制，DMA，智能卡，IrDA，LIN

## 开发相关

### 启动配置

![](https://cdn.jsdelivr.net/gh/xuezhaorong/Picgo//Source/fix-dir/STM32/%E5%BA%93%E5%87%BD%E6%95%B0%E5%BC%80%E5%8F%91/2024/05/12/20-47-00-35f307eb2e1c4590ddfd19304ddcfe8d-%E5%90%AF%E5%8A%A8%E9%85%8D%E7%BD%AE-443f37.png)

 系统存储器存储 STM32 中的一段 BootLoader 程序，用于接收串口的数据，然后刷新到主闪存中。当 STM32 死锁时，需要切换到内置 SRAM 模式进行解锁。

### 工程结构

![](https://cdn.jsdelivr.net/gh/xuezhaorong/Picgo//Source/fix-dir/STM32/%E5%BA%93%E5%87%BD%E6%95%B0%E5%BC%80%E5%8F%91/2024/05/12/20-47-44-bc6ee65ad9904efcdbb72d8ed284c075-%E5%B7%A5%E7%A8%8B%E6%9E%B6%E6%9E%84-1c5e4c.png)

### StlinkV2 插针图

![](https://cdn.jsdelivr.net/gh/xuezhaorong/Picgo//Source/fix-dir/STM32/%E5%BA%93%E5%87%BD%E6%95%B0%E5%BC%80%E5%8F%91/2024/05/12/20-48-00-08060c87b23d4ff37fc85454a0e11a61-Stlink%E6%8F%92%E9%92%88-1981e4.png)