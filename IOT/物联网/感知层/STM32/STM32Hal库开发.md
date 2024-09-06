根据[[STM32Hal库配置]]配置Clion项目
## GPIO通用输入输出
#### 初始化
Hal中GPIO初始化函数
```c
/**  
  * @brief  Initializes the GPIOx peripheral according to the specified parameters in the GPIO_Init. 
  * @param  GPIOx: where x can be (A..G depending on device used) to select the GPIO peripheral 
  * @param  GPIO_Init: pointer to a GPIO_InitTypeDef structure that contains  *         the configuration information for the specified GPIO peripheral.  
  * @retval None  */
void HAL_GPIO_Init(GPIO_TypeDef  *GPIOx, GPIO_InitTypeDef *GPIO_Init)
```

结构体为Hal库的GPIO初始化结构体
```c
typedef struct  
{  
  uint32_t Pin;       /*!< Specifies the GPIO pins to be configured.  
                           This parameter can be any value of @ref GPIO_pins_define */  
  uint32_t Mode;      /*!< Specifies the operating mode for the selected pins.  
                           This parameter can be a value of @ref GPIO_mode_define */  
  uint32_t Pull;      /*!< Specifies the Pull-up or Pull-Down activation for the selected pins.  
                           This parameter can be a value of @ref GPIO_pull_define */  
  uint32_t Speed;     /*!< Specifies the speed for the selected pins.  
                           This parameter can be a value of @ref GPIO_speed_define */} GPIO_InitTypeDef;
```

-参数`Pin`为GPIO的端口
```c
#define GPIO_PIN_0                 ((uint16_t)0x0001)  /* Pin 0 selected    */  
#define GPIO_PIN_1                 ((uint16_t)0x0002)  /* Pin 1 selected    */  
#define GPIO_PIN_2                 ((uint16_t)0x0004)  /* Pin 2 selected    */  
#define GPIO_PIN_3                 ((uint16_t)0x0008)  /* Pin 3 selected    */  
#define GPIO_PIN_4                 ((uint16_t)0x0010)  /* Pin 4 selected    */  
#define GPIO_PIN_5                 ((uint16_t)0x0020)  /* Pin 5 selected    */  
#define GPIO_PIN_6                 ((uint16_t)0x0040)  /* Pin 6 selected    */  
#define GPIO_PIN_7                 ((uint16_t)0x0080)  /* Pin 7 selected    */  
#define GPIO_PIN_8                 ((uint16_t)0x0100)  /* Pin 8 selected    */  
#define GPIO_PIN_9                 ((uint16_t)0x0200)  /* Pin 9 selected    */  
#define GPIO_PIN_10                ((uint16_t)0x0400)  /* Pin 10 selected   */  
#define GPIO_PIN_11                ((uint16_t)0x0800)  /* Pin 11 selected   */  
#define GPIO_PIN_12                ((uint16_t)0x1000)  /* Pin 12 selected   */  
#define GPIO_PIN_13                ((uint16_t)0x2000)  /* Pin 13 selected   */  
#define GPIO_PIN_14                ((uint16_t)0x4000)  /* Pin 14 selected   */  
#define GPIO_PIN_15                ((uint16_t)0x8000)  /* Pin 15 selected   */  
#define GPIO_PIN_All               ((uint16_t)0xFFFF)  /* All pins selected */
```

-个参数`Mode`为GPIO的输入输出模式，其中输入模式需要配合参数`Pull`使用
```c
#define  GPIO_MODE_INPUT                        0x00000000u   /*!< Input Floating Mode                   */  
#define  GPIO_MODE_OUTPUT_PP                    0x00000001u   /*!< Output Push Pull Mode                 */  
#define  GPIO_MODE_OUTPUT_OD                    0x00000011u   /*!< Output Open Drain Mode                */  
#define  GPIO_MODE_AF_PP                        0x00000002u   /*!< Alternate Function Push Pull Mode     */  
#define  GPIO_MODE_AF_OD                        0x00000012u   /*!< Alternate Function Open Drain Mode    */  
#define  GPIO_MODE_AF_INPUT                     GPIO_MODE_INPUT          /*!< Alternate Function Input Mode         */
```

-参数`Pull`配置上拉输入或下拉输入
```c
#define  GPIO_NOPULL        0x00000000u   /*!< No Pull-up or Pull-down activation  */  
#define  GPIO_PULLUP        0x00000001u   /*!< Pull-up activation                  */  
#define  GPIO_PULLDOWN      0x00000002u   /*!< Pull-down activation                */
```

-参数`Speed`为输出的速度
```c
#define  GPIO_SPEED_FREQ_LOW              (GPIO_CRL_MODE0_1) /*!< Low speed */  
#define  GPIO_SPEED_FREQ_MEDIUM           (GPIO_CRL_MODE0_0) /*!< Medium speed */  
#define  GPIO_SPEED_FREQ_HIGH             (GPIO_CRL_MODE0)   /*!< High speed */
```

示例代码：

```c
// 开启外设时钟
__HAL_RCC_GPIOC_CLK_ENABLE();
// 初始化结构体
GPIO_InitTypeDef GPIO_InitStructure;  
GPIO_InitStructure.Pin = GPIO_PIN_13;  
GPIO_InitStructure.Mode = GPIO_MODE_OUTPUT_PP;  
GPIO_InitStructure.Speed = GPIO_SPEED_HIGH;  
HAL_GPIO_Init(GPIOC,&GPIO_InitStructure);
```


### 输出
关于输出的几个函数
1. **引脚写0或1**
```c
void HAL_GPIO_WritePin(GPIO_TypeDef* GPIOx, uint16_t GPIO_Pin, GPIO_PinState PinState);
```
参数`GPIO_PinState`为高低电平
```c
typedef enum  
{  
  GPIO_PIN_RESET = 0u,  
  GPIO_PIN_SET  
} GPIO_PinState;
```

2. **翻转引脚的电平状态**
```c
HAL_StatusTypeDef HAL_GPIO_LockPin(GPIO_TypeDef* GPIOx, uint16_t GPIO_Pin);
```

示例代码：
```c
HAL_GPIO_WritePin(GPIOC, GPIO_PIN_13,GPIO_PIN_RESET);
```

### 输入
**读取引脚的电平状态、函数返回值为0或1**
```c
GPIO_PinState HAL_GPIO_ReadPin(GPIO_TypeDef* GPIOx, uint16_t GPIO_Pin);
```

示例代码：
```c
if(HAL_GPIO_ReadPin(GPIOC,GPIO_PIN_13)==GPIO_PIN_RESET){

}
```

### STM32CUBE操作
点击灰色不占用功能的引脚，即可配置输入输出模式
![image.png|925](https://cdn.jsdelivr.net/gh/xuezhaorong/Picgo//Source/fix-dir/picgo/picgo-clipboard-images/2024/09/05/19-45-20-52186962035b5e2aafeff94b6ce816e7-20240905194519-118089.png)

点击左侧`System Core`的`GPIO`再点击相应的引脚，即可配置输入输出
![image.png|1075](https://cdn.jsdelivr.net/gh/xuezhaorong/Picgo//Source/fix-dir/picgo/picgo-clipboard-images/2024/09/05/19-51-15-12020394d9e5f351086930b9081bce04-20240905195115-9d7387.png)

![image.png|1075](https://cdn.jsdelivr.net/gh/xuezhaorong/Picgo//Source/fix-dir/picgo/picgo-clipboard-images/2024/09/05/19-54-27-2e567e982c596746c4dfbe4a63aee999-20240905195427-d1c91b.png)


## 外部中断
### 中断引脚选择与中断触发方式
Hal库中的AFIO与EXIT在GPIO初始化`Mode`时可以同时配置
```c
#define  GPIO_MODE_IT_RISING                    0x10110000u   /*!< External Interrupt Mode with Rising edge trigger detection          */  
#define  GPIO_MODE_IT_FALLING                   0x10210000u   /*!< External Interrupt Mode with Falling edge trigger detection         */  
#define  GPIO_MODE_IT_RISING_FALLING            0x10310000u   /*!< External Interrupt Mode with Rising/Falling edge trigger detection  */
```

### NVIC
由函数`HAL_NVIC_SetPriority`配置中断通道，抢占优先级和响应优先级

```c
/**  
  * @brief  Sets the priority of an interrupt.  
  * @param  IRQn: External interrupt number.  *         This parameter can be an enumerator of IRQn_Type enumeration  *         (For the complete STM32 Devices IRQ Channels list, please refer to the appropriate CMSIS device file (stm32f10xx.h))  
  * @param  PreemptPriority: The preemption priority for the IRQn channel.  *         This parameter can be a value between 0 and 15  *         A lower priority value indicates a higher priority  
  * @param  SubPriority: the subpriority level for the IRQ channel.  
  *         This parameter can be a value between 0 and 15  *         A lower priority value indicates a higher priority.  * @retval None  
  */void HAL_NVIC_SetPriority(IRQn_Type IRQn, uint32_t PreemptPriority, uint32_t SubPriority)
```

参数`IERQn_Type`为中断通道
```c
typedef enum  
{  
/******  Cortex-M3 Processor Exceptions Numbers ***************************************************/  
  NonMaskableInt_IRQn         = -14,    /*!< 2 Non Maskable Interrupt                             */  
  HardFault_IRQn              = -13,    /*!< 3 Cortex-M3 Hard Fault Interrupt                     */  
  MemoryManagement_IRQn       = -12,    /*!< 4 Cortex-M3 Memory Management Interrupt              */  
  BusFault_IRQn               = -11,    /*!< 5 Cortex-M3 Bus Fault Interrupt                      */  
  UsageFault_IRQn             = -10,    /*!< 6 Cortex-M3 Usage Fault Interrupt                    */  
  SVCall_IRQn                 = -5,     /*!< 11 Cortex-M3 SV Call Interrupt                       */  
  DebugMonitor_IRQn           = -4,     /*!< 12 Cortex-M3 Debug Monitor Interrupt                 */  
  PendSV_IRQn                 = -2,     /*!< 14 Cortex-M3 Pend SV Interrupt                       */  
  SysTick_IRQn                = -1,     /*!< 15 Cortex-M3 System Tick Interrupt                   */  
  
/******  STM32 specific Interrupt Numbers *********************************************************/  
  WWDG_IRQn                   = 0,      /*!< Window WatchDog Interrupt                            */  
  PVD_IRQn                    = 1,      /*!< PVD through EXTI Line detection Interrupt            */  
  TAMPER_IRQn                 = 2,      /*!< Tamper Interrupt                                     */  
  RTC_IRQn                    = 3,      /*!< RTC global Interrupt                                 */  
  FLASH_IRQn                  = 4,      /*!< FLASH global Interrupt                               */  
  RCC_IRQn                    = 5,      /*!< RCC global Interrupt                                 */  
  EXTI0_IRQn                  = 6,      /*!< EXTI Line0 Interrupt                                 */  
  EXTI1_IRQn                  = 7,      /*!< EXTI Line1 Interrupt                                 */  
  EXTI2_IRQn                  = 8,      /*!< EXTI Line2 Interrupt                                 */  
  EXTI3_IRQn                  = 9,      /*!< EXTI Line3 Interrupt                                 */  
  EXTI4_IRQn                  = 10,     /*!< EXTI Line4 Interrupt                                 */  
  DMA1_Channel1_IRQn          = 11,     /*!< DMA1 Channel 1 global Interrupt                      */  
  DMA1_Channel2_IRQn          = 12,     /*!< DMA1 Channel 2 global Interrupt                      */  
  DMA1_Channel3_IRQn          = 13,     /*!< DMA1 Channel 3 global Interrupt                      */  
  DMA1_Channel4_IRQn          = 14,     /*!< DMA1 Channel 4 global Interrupt                      */  
  DMA1_Channel5_IRQn          = 15,     /*!< DMA1 Channel 5 global Interrupt                      */  
  DMA1_Channel6_IRQn          = 16,     /*!< DMA1 Channel 6 global Interrupt                      */  
  DMA1_Channel7_IRQn          = 17,     /*!< DMA1 Channel 7 global Interrupt                      */  
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
  RTC_Alarm_IRQn              = 41,     /*!< RTC Alarm through EXTI Line Interrupt                */  
  USBWakeUp_IRQn              = 42,     /*!< USB Device WakeUp from suspend through EXTI Line Interrupt */  
} IRQn_Type;
```

参数`PreemptPriority`和参数`SubPriority`分别为抢占优先级和响应优先级。

通过函数`void HAL_NVIC_EnableIRQ(IRQn_Type IRQn)`使能中断

示例代码：
```c
GPIO_InitTypeDef GPIO_InitStructure;

     /*开启按键GPIO口的时钟*/
 __HAL_RCC_GPIOC_CLK_ENABLE;

 /* 选择按键1的引脚 */
 GPIO_InitStructure.Pin = GPIO_PIN_13;
 /* 设置引脚为输入模式 */
 GPIO_InitStructure.Mode = GPIO_MODE_IT_RISING;
 /* 设置引脚不上拉也不下拉 */
 GPIO_InitStructure.Pull = GPIO_NOPULL;
 /* 使用上面的结构体初始化按键 */
 HAL_GPIO_Init(GPIOC, &GPIO_InitStructure);
 /* 配置 EXTI 中断源 到key1 引脚、配置中断优先级*/
 HAL_NVIC_SetPriority(EXTI15_10_IRQn, 0, 0);
 /* 使能中断 */
 HAL_NVIC_EnableIRQ(EXTI15_10_IRQn);
```

### 中断服务函数
查询相应的中断服务函数名，重写
```c
 void EXTI15_10_IRQHandler(void)
 {
     //确保是否产生了EXTI Line中断
     if (__HAL_GPIO_EXTI_GET_IT(GPIO_PIN_13) != RESET) {
			// code
         //清除中断标志位
         __HAL_GPIO_EXTI_CLEAR_IT(GPIO_PIN_13);
     }
 }
```

### STM32CUBE操作
点击引脚中的EXIT中断配置
![image.png|875](https://cdn.jsdelivr.net/gh/xuezhaorong/Picgo//Source/fix-dir/picgo/picgo-clipboard-images/2024/09/05/20-38-57-70780b0c9fc4325acb8d3844e3a2dffa-20240905203856-1adfcf.png)
 
 选择中断触发模式，参数分别为：
 * 上升沿触发外部中断
 * 下降沿触发外部中断
 * 双边沿触发外部中断
后面三个参数为外部事件的触发
 ![image.png|950](https://cdn.jsdelivr.net/gh/xuezhaorong/Picgo//Source/fix-dir/picgo/picgo-clipboard-images/2024/09/05/20-40-28-98e9e3380f4006443ef8515f189eee89-20240905204027-db5bbf.png)
进入NVIC配置，将中断通道打勾使能中断，并配置抢占优先级和响应优先级
![image.png|1000](https://cdn.jsdelivr.net/gh/xuezhaorong/Picgo//Source/fix-dir/picgo/picgo-clipboard-images/2024/09/05/20-43-38-fd534b74699c33b59aefca781f5477fe-20240905204338-3cca9d.png)

在`stm32f1xx_it.c`文件下可以看到自动生成的中断服务函数
![image.png|1000](https://cdn.jsdelivr.net/gh/xuezhaorong/Picgo//Source/fix-dir/picgo/picgo-clipboard-images/2024/09/05/20-50-21-595165b67a796aff40448b918050445c-20240905205020-fea4fa.png)

## 定时器
### 定时中断
#### 时基单元配置
```c
/**  
  * @brief  Initializes the TIM Time base Unit according to the specified  *         parameters in the TIM_HandleTypeDef and initialize the associated handle.  
  * @note   Switching from Center Aligned counter mode to Edge counter mode (or reverse)  *         requires a timer reset to avoid unexpected direction  *         due to DIR bit readonly in center aligned mode.  *         Ex: call @ref HAL_TIM_Base_DeInit() before HAL_TIM_Base_Init()  
  * @param  htim TIM Base handle  
  * @retval HAL status  */
HAL_StatusTypeDef HAL_TIM_Base_Init(TIM_HandleTypeDef *htim)
```

`TIM_HandleTypeDef`结构体有两个重要的参数
1. `TIM_TypeDef`选择内部定时器
2. `TIM_Base_InitTypeDef`配置预分配值和自动重载值


示例代码：
```c
  
TIM_TimeBaseStructure.Instance = TIM6;

TIM_TimeBaseStructure.Init.Period = 5000-1;

TIM_TimeBaseStructure.Init.Prescaler = 8400-1;

HAL_TIM_Base_Init(&TIM_TimeBaseStructure);
```

#### 配置NVIC

```c
//设置抢占优先级，子优先级
HAL_NVIC_SetPriority(TIM6_DAC_IRQn, 0, 3);
// 设置中断来源
HAL_NVIC_EnableIRQ(TIM6_DAC_IRQn)
```

#### 开启定时器并更新中断

Hal中同时更新中断和开启定时器
```c
HAL_TIM_Base_Start_IT(&TIM_TimeBaseStructure);
```
若只想开启定时器可以使用函数
```c
HAL_TIM_Base_Start(&TIM_TimeBaseStructure);
```

#### 中断服务函数
Hal中的中断服务方式和中断回调函数分离，先进入中断服务函数中，然后触发回调函数
```c
 void  TIM6_DAC_IRQHandler(void)
 {
     HAL_TIM_IRQHandler(&TIM_TimeBaseStructure);
 }
 
 void HAL_TIM_PeriodElapsedCallback(TIM_HandleTypeDef *htim)
 {
     if (htim==(&TIM_TimeBaseStructure)) {
         
     }
 }
```

#### STM32CUBE操作
在`Timer`中选择定时器，勾选`Internal Clock`选择内部时钟
![image.png|800](https://cdn.jsdelivr.net/gh/xuezhaorong/Picgo//Source/fix-dir/picgo/picgo-clipboard-images/2024/09/05/22-00-08-d082222208e5a7ea0675f4492d021fa3-20240905220008-bcc6a4.png)

然后配置各种参数
![image.png|1050](https://cdn.jsdelivr.net/gh/xuezhaorong/Picgo//Source/fix-dir/picgo/picgo-clipboard-images/2024/09/05/22-03-24-86e597ee504bf9408e524dd51a91047c-20240905220323-571db7.png)

在`NVIC Setting`中设置NVIC
![image.png|750](https://cdn.jsdelivr.net/gh/xuezhaorong/Picgo//Source/fix-dir/picgo/picgo-clipboard-images/2024/09/05/22-04-15-3f24c3a04b093c69860181d1c8a55287-20240905220414-070e4b.png)

设置优先级
![image.png|875](https://cdn.jsdelivr.net/gh/xuezhaorong/Picgo//Source/fix-dir/picgo/picgo-clipboard-images/2024/09/05/22-04-48-877bd040efc5943b6478e7abc71fe741-20240905220448-6d40a6.png)

在`main.c`中写入开启中断和定时器
```c
HAL_TIM_Base_Start_IT(&htim3);
```

![image.png|1000](https://cdn.jsdelivr.net/gh/xuezhaorong/Picgo//Source/fix-dir/picgo/picgo-clipboard-images/2024/09/05/22-07-29-ede5097dfd7faceec45fbf723fa1ea29-20240905220728-3a3987.png)

加入中断回调函数
```c
void HAL_TIM_PeriodElapsedCallback(TIM_HandleTypeDef *htim)
{

    if (htim == (&htim3))
    {

    }
}
```

![image.png|1050](https://cdn.jsdelivr.net/gh/xuezhaorong/Picgo//Source/fix-dir/picgo/picgo-clipboard-images/2024/09/05/22-08-54-8b81bfeafe2cd54a93b213e72b57a4ab-20240905220853-053572.png)

### 输出比较
#### 复用引脚配置
```c
GPIO_InitTypeDef GPIO_InitStructure;
GPIO_InitStructure.Pin = GPIO_PIN_0;
GPIO_InitStructure.Mode = GPIO_MODE_AF_PP;
GPIO_InitStructure.Pull = GPIO_NOPULL;
GPIO_InitStructure.Speed = GPIO_SPEED_HIGH;
HAL_GPIO_Init(GPIOA, &GPIO_InitStructure);
```

#### 时基单元配置
```c
TIM_HandleTypeDef TIM_TimeBaseStructure;
TIM_TimeBaseStructure.Instance = TIM2;  
TIM_TimeBaseStructure.Init.Prescaler = 71;  
TIM_TimeBaseStructure.Init.CounterMode = TIM_COUNTERMODE_UP;  
TIM_TimeBaseStructure.Init.Period = 499;  
TIM_TimeBaseStructure.Init.ClockDivision = TIM_CLOCKDIVISION_DIV1;  
TIM_TimeBaseStructure.Init.AutoReloadPreload = TIM_AUTORELOAD_PRELOAD_ENABLE;  
HAL_TIM_Base_Init(&TIM_TimeBaseStructure);
```

