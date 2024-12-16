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
![image.png|1025](https://cdn.jsdelivr.net/gh/xuezhaorong/Picgo//Source/fix-dir/picgo/picgo-clipboard-images/2024/11/25/12-20-10-3184366e2a64d1c12ed251d69d4661f0-20241125122009-3c8d22.png)

 
 选择中断触发模式，参数分别为：
 * 上升沿触发外部中断
 * 下降沿触发外部中断
 * 双边沿触发外部中断
后面三个参数为外部事件的触发
 ![image.png|950](https://cdn.jsdelivr.net/gh/xuezhaorong/Picgo//Source/fix-dir/picgo/picgo-clipboard-images/2024/09/05/20-40-28-98e9e3380f4006443ef8515f189eee89-20240905204027-db5bbf.png)
配置为下降沿触发，引脚为上拉模式
![image.png|950](https://cdn.jsdelivr.net/gh/xuezhaorong/Picgo//Source/fix-dir/picgo/picgo-clipboard-images/2024/11/25/12-21-02-f5ecf2c8c51ced2f6c1b57647fe0dc41-20241125122101-f85953.png)


进入NVIC配置，将中断通道打勾使能中断，并配置抢占优先级和响应优先级
![image.png|1000](https://cdn.jsdelivr.net/gh/xuezhaorong/Picgo//Source/fix-dir/picgo/picgo-clipboard-images/2024/09/05/20-43-38-fd534b74699c33b59aefca781f5477fe-20240905204338-3cca9d.png)

在`stm32f1xx_it.c`文件下可以看到自动生成的中断服务函数
![image.png|550](https://cdn.jsdelivr.net/gh/xuezhaorong/Picgo//Source/fix-dir/picgo/picgo-clipboard-images/2024/11/25/12-21-32-d82e2866f25b6e19ed2e90f3b289c3f0-20241125122131-53c92a.png)

已经自动完成标志位的清除，进入回调函数 
![image.png|650](https://cdn.jsdelivr.net/gh/xuezhaorong/Picgo//Source/fix-dir/picgo/picgo-clipboard-images/2024/11/25/12-22-15-f0743979a14e0cd93cc45b4f7827d022-20241125122215-9b8e49.png)

需要在`main.c`中写中断回调函数
![image.png|925](https://cdn.jsdelivr.net/gh/xuezhaorong/Picgo//Source/fix-dir/picgo/picgo-clipboard-images/2024/11/25/12-22-52-483bdd6d90fe0585f2ec146201f459be-20241125122252-7f4436.png)

```c
void HAL_GPIO_EXTI_Callback(uint16_t GPIO_Pin) {  
  if (GPIO_Pin == KEY_CONNECT) {  
   if (HAL_GPIO_ReadPin(PORT_CONNECT, KEY_CONNECT) == GPIO_PIN_RESET)  
   {  

   }  
  }  
}
```

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
TIM_HandleTypeDef TIM_TimeBaseStructure;
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
HAL_NVIC_EnableIRQ(TIM6_DAC_IRQn);
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

#### 示例
```c
__HAL_RCC_TIM6_CLK_ENABLE();
TIM_HandleTypeDef TIM_TimeBaseStructure;
TIM_TimeBaseStructure.Instance = TIM6;
TIM_TimeBaseStructure.Init.Period = 5000-1;
TIM_TimeBaseStructure.Init.Prescaler = 8400-1;
HAL_TIM_Base_Init(&TIM_TimeBaseStructure);

//设置抢占优先级，子优先级
HAL_NVIC_SetPriority(TIM6_DAC_IRQn, 0, 3);
// 设置中断来源
HAL_NVIC_EnableIRQ(TIM6_DAC_IRQn);

HAL_TIM_Base_Start_IT(&TIM_TimeBaseStructure);

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

#### 输出比较单元配置
hal中输出比较单元初始化函数
```c
/**  
  * @brief  Initializes the TIM PWM  channels according to the specified  *         parameters in the TIM_OC_InitTypeDef.  
  * @param  htim TIM PWM handle  
  * @param  sConfig TIM PWM configuration structure  
  * @param  Channel TIM Channels to be configured  *          This parameter can be one of the following values:  
  *            @arg TIM_CHANNEL_1: TIM Channel 1 selected  
  *            @arg TIM_CHANNEL_2: TIM Channel 2 selected  
  *            @arg TIM_CHANNEL_3: TIM Channel 3 selected  
  *            @arg TIM_CHANNEL_4: TIM Channel 4 selected  
  * @retval HAL status  */
HAL_StatusTypeDef HAL_TIM_PWM_ConfigChannel(TIM_HandleTypeDef *htim, const TIM_OC_InitTypeDef *sConfig, uint32_t Channel)
```

第二个参数`TIM_OC_InitTypeDef`为输出比较单元的结构体

```c
typedef struct  
{  
  uint32_t OCMode;        /*!< Specifies the TIM mode.  
                               This parameter can be a value of @ref TIM_Output_Compare_and_PWM_modes */  
  uint32_t Pulse;         /*!< Specifies the pulse value to be loaded into the Capture Compare Register.  
                               This parameter can be a number between Min_Data = 0x0000 and Max_Data = 0xFFFF */  
  uint32_t OCPolarity;    /*!< Specifies the output polarity.  
                               This parameter can be a value of @ref TIM_Output_Compare_Polarity */  
  uint32_t OCNPolarity;   /*!< Specifies the complementary output polarity.  
                               This parameter can be a value of @ref TIM_Output_Compare_N_Polarity                               @note This parameter is valid only for timer instances supporting break feature. */  
  uint32_t OCFastMode;    /*!< Specifies the Fast mode state.  
                               This parameter can be a value of @ref TIM_Output_Fast_State                               @note This parameter is valid only in PWM1 and PWM2 mode. */  
  
  uint32_t OCIdleState;   /*!< Specifies the TIM Output Compare pin state during Idle state.  
                               This parameter can be a value of @ref TIM_Output_Compare_Idle_State                               @note This parameter is valid only for timer instances supporting break feature. */  
  uint32_t OCNIdleState;  /*!< Specifies the TIM Output Compare pin state during Idle state.  
                               This parameter can be a value of @ref TIM_Output_Compare_N_Idle_State                               @note This parameter is valid only for timer instances supporting break feature. */} TIM_OC_InitTypeDef;
```

主要参数有：
* `OCMode`：PWM模式
```c
#define TIM_OCMODE_TIMING                   0x00000000U                                              /*!< Frozen                                 */  
#define TIM_OCMODE_ACTIVE                   TIM_CCMR1_OC1M_0                                         /*!< Set channel to active level on match   */  
#define TIM_OCMODE_INACTIVE                 TIM_CCMR1_OC1M_1                                         /*!< Set channel to inactive level on match */  
#define TIM_OCMODE_TOGGLE                   (TIM_CCMR1_OC1M_1 | TIM_CCMR1_OC1M_0)                    /*!< Toggle                                 */  
#define TIM_OCMODE_PWM1                     (TIM_CCMR1_OC1M_2 | TIM_CCMR1_OC1M_1)                    /*!< PWM mode 1                             */  
#define TIM_OCMODE_PWM2                     (TIM_CCMR1_OC1M_2 | TIM_CCMR1_OC1M_1 | TIM_CCMR1_OC1M_0) /*!< PWM mode 2                             */  
#define TIM_OCMODE_FORCED_ACTIVE            (TIM_CCMR1_OC1M_2 | TIM_CCMR1_OC1M_0)                    /*!< Force active level                     */  
#define TIM_OCMODE_FORCED_INACTIVE          TIM_CCMR1_OC1M_2                                         /*!< Force inactive level
```
* `Pulse`：占空比
* `OCPolarity`：输出比较的极性

`第三个参数为输出比较的通道`


示例代码：
```c
TIM_OC_InitTypeDef TIM_OCInitStructure;
TIM_OCInitStructure.OCMode = TIM_OCMODE_PWM1;  
TIM_OCInitStructure.Pulse = 0;  
TIM_OCInitStructure.OCPolarity = TIM_OCPOLARITY_LOW;  
HAL_TIM_PWM_ConfigChannel(&TIM_TimeBaseStructure, &TIM_OCInitStructure, TIM_CHANNEL_1);
```

#### 启动输出比较

```c
HAL_TIM_PWM_Start(&TIM_TimeBaseStructure,TIM_CHANNEL_1);
```

#### 设置占空比
```c
__HAL_TIM_SetCompare(&TIM_TimeBaseStructure, TIM_CHANNEL_1, 0);
```

#### 示例
```c
// 复用引脚配置
__HAL_RCC_GPIOA_CLK_ENABLE();
GPIO_InitTypeDef GPIO_InitStruct;
GPIO_InitStruct.Pin = GPIO_PIN_0;  // 引脚
GPIO_InitStruct.Mode = GPIO_MODE_AF_PP;  
GPIO_InitStruct.Speed = GPIO_SPEED_FREQ_HIGH;  
HAL_GPIO_Init(GPIOA, &GPIO_InitStruct); // 端口

// 时基单元配置
__HAL_RCC_TIM2_CLK_ENABLE();
TIM_HandleTypeDef TIM_TimeBaseStructure;
TIM_TimeBaseStructure.Instance = TIM2;  // 定时器
TIM_TimeBaseStructure.Init.Prescaler = 71;  
TIM_TimeBaseStructure.Init.CounterMode = TIM_COUNTERMODE_UP;  
TIM_TimeBaseStructure.Init.Period = 499;  
TIM_TimeBaseStructure.Init.ClockDivision = TIM_CLOCKDIVISION_DIV1;  
TIM_TimeBaseStructure.Init.RepetitionCounter = 0;
TIM_TimeBaseStructure.Init.AutoReloadPreload = TIM_AUTORELOAD_PRELOAD_ENABLE;  
HAL_TIM_Base_Init(&TIM_TimeBaseStructure);

//  输出比较单元配置
TIM_OC_InitTypeDef TIM_OCInitStructure;
TIM_OCInitStructure.OCMode = TIM_OCMODE_PWM1;  
TIM_OCInitStructure.Pulse = 0;  
TIM_OCInitStructure.OCPolarity = TIM_OCPOLARITY_HIGH;  
HAL_TIM_PWM_ConfigChannel(&TIM_TimeBaseStructure, &TIM_OCInitStructure, TIM_CHANNEL_1); // 通道

// 开启PWM
HAL_TIM_PWM_Start(&TIM_TimeBaseStructure,TIM_CHANNEL_1); // 通道

// 设置占空比
__HAL_TIM_SetCompare(&TIM_TimeBaseStructure, TIM_CHANNEL_1, 0); // 通道
```

#### STM32CUBE操作
设置通用定时器的时钟源为内部时钟，开启相应的输出通道，对应的引脚自动设置为复用模式

![image.png|850](https://cdn.jsdelivr.net/gh/xuezhaorong/Picgo//Source/fix-dir/picgo/picgo-clipboard-images/2024/09/06/12-43-58-cf2563b2685378cc60f8ccd0160bad88-20240906124357-ae9bc0.png)

配置时基单元参数和输出比较单元参数
![image.png|925](https://cdn.jsdelivr.net/gh/xuezhaorong/Picgo//Source/fix-dir/picgo/picgo-clipboard-images/2024/09/06/12-53-03-9c602fa1c1cae428f88460fdd1369b8d-20240906125302-56a76e.png)

在`main.c`中开启PWM并设置占空比
```c
HAL_TIM_PWM_Start(&htim2,TIM_CHANNEL_1);

__HAL_TIM_SetCompare(&htim2, TIM_CHANNEL_1, 0);
```


### 输入捕获
#### GPIO引脚配置
```c
GPIO_InitTypeDef GPIO_InitStruct;
GPIO_InitStruct.Pin = GPIO_PIN_1;  
GPIO_InitStruct.Mode = GPIO_MODE_INPUT;  
GPIO_InitStruct.Pull = GPIO_PULLDOWN;  
HAL_GPIO_Init(GPIOA, &GPIO_InitStruct);
```

#### 时基单元配置
```c
TIM_HandleTypeDef TIM_TimeBaseStructure;
TIM_TimeBaseStructure.Instance = TIM2;  
TIM_TimeBaseStructure.Init.Prescaler = 71;  
TIM_TimeBaseStructure.Init.CounterMode = TIM_COUNTERMODE_UP;  
TIM_TimeBaseStructure.Init.Period = 65535;  
TIM_TimeBaseStructure.Init.ClockDivision = TIM_CLOCKDIVISION_DIV1;  
TIM_TimeBaseStructure.Init.AutoReloadPreload = TIM_AUTORELOAD_PRELOAD_ENABLE;  
HAL_TIM_Base_Init(&TIM_TimeBaseStructure);
```

#### 输入捕获单元配置
hal中输入捕获初始化的函数
```c
/**  
  * @brief  Initializes the TIM Input Capture Channels according to the specified  *         parameters in the TIM_IC_InitTypeDef.  
  * @param  htim TIM IC handle  
  * @param  sConfig TIM Input Capture configuration structure  
  * @param  Channel TIM Channel to configure  *          This parameter can be one of the following values:  
  *            @arg TIM_CHANNEL_1: TIM Channel 1 selected  
  *            @arg TIM_CHANNEL_2: TIM Channel 2 selected  
  *            @arg TIM_CHANNEL_3: TIM Channel 3 selected  
  *            @arg TIM_CHANNEL_4: TIM Channel 4 selected  * @retval HAL status  */
HAL_StatusTypeDef HAL_TIM_IC_ConfigChannel(TIM_HandleTypeDef *htim, const TIM_IC_InitTypeDef *sConfig, uint32_t Channel)
```

`TIM_IC_InitTypeDef`为输入捕获结构体
```c
typedef struct  
{  
  uint32_t  ICPolarity;  /*!< Specifies the active edge of the input signal.  
                              This parameter can be a value of @ref TIM_Input_Capture_Polarity */  
  uint32_t ICSelection;  /*!< Specifies the input.  
                              This parameter can be a value of @ref TIM_Input_Capture_Selection */  
  uint32_t ICPrescaler;  /*!< Specifies the Input Capture Prescaler.  
                              This parameter can be a value of @ref TIM_Input_Capture_Prescaler */  
  uint32_t ICFilter;     /*!< Specifies the input capture filter.  
                              This parameter can be a number between Min_Data = 0x0 and Max_Data = 0xF */} TIM_IC_InitTypeDef;
```
* 参数`ICPolarity`：触发极性
```c
#define  TIM_ICPOLARITY_RISING             TIM_INPUTCHANNELPOLARITY_RISING      /*!< Capture triggered by rising edge on timer input                  */  
#define  TIM_ICPOLARITY_FALLING            TIM_INPUTCHANNELPOLARITY_FALLING     /*!< Capture triggered by falling edge on timer input                 */  
#define  TIM_ICPOLARITY_BOTHEDGE           TIM_INPUTCHANNELPOLARITY_BOTHEDGE    /*!< Capture triggered by both rising and falling edges on timer input*/  
```

* 参数`ICSelection`：连通模式
```c
#define TIM_ICSELECTION_DIRECTTI           TIM_CCMR1_CC1S_0                     /*!< TIM Input 1, 2, 3 or 4 is selected to be connected to IC1, IC2, IC3 or IC4, respectively */  
#define TIM_ICSELECTION_INDIRECTTI         TIM_CCMR1_CC1S_1                     /*!< TIM Input 1, 2, 3 or 4 is selected to be connected to IC2, IC1, IC4 or IC3, respectively */  
#define TIM_ICSELECTION_TRC                TIM_CCMR1_CC1S                       /*!< TIM Input 1, 2, 3 or 4 is selected to be connected to TRC */  
```

* 参数`ICPrescaler`：分频模式
```c
#define TIM_ICPSC_DIV1                     0x00000000U                          /*!< Capture performed each time an edge is detected on the capture input */  
#define TIM_ICPSC_DIV2                     TIM_CCMR1_IC1PSC_0                   /*!< Capture performed once every 2 events                                */  
#define TIM_ICPSC_DIV4                     TIM_CCMR1_IC1PSC_1                   /*!< Capture performed once every 4 events                                */  
#define TIM_ICPSC_DIV8                     TIM_CCMR1_IC1PSC                     /*!< Capture performed once every 8 events
```

* 参数`ICFilter`：滤波器

示例代码：
```c
TIM_IC_InitTypeDef TIM_ICInitStructure;
TIM_ICInitStructure.ICPolarity = TIM_INPUTCHANNELPOLARITY_RISING;  
TIM_ICInitStructure.ICSelection = TIM_ICSELECTION_DIRECTTI;  
TIM_ICInitStructure.ICPrescaler = TIM_ICPSC_DIV1;  
TIM_ICInitStructure.ICFilter = 8;  
HAL_TIM_IC_ConfigChannel(&TIM_TimeBaseStructure, &TIM_ICInitStructure, TIM_CHANNEL_2);
```

#### 配置从模式
hal中配置从模式的函数
```c
/**  
  * @brief  Configures the TIM in Slave mode  
  * @param  htim TIM handle.  
  * @param  sSlaveConfig pointer to a TIM_SlaveConfigTypeDef structure that  *         contains the selected trigger (internal trigger input, filtered  *         timer input or external trigger input) and the Slave mode  *         (Disable, Reset, Gated, Trigger, External clock mode 1).  
  * @retval HAL status  */
HAL_StatusTypeDef HAL_TIM_SlaveConfigSynchro(TIM_HandleTypeDef *htim, const TIM_SlaveConfigTypeDef *sSlaveConfig)
```

`TIM_SlaveConfigTypeDef`为从模式的结构体
```c
typedef struct  
{  
  uint32_t  SlaveMode;         /*!< Slave mode selection  
                                    This parameter can be a value of @ref TIM_Slave_Mode */  uint32_t  InputTrigger;      /*!< Input Trigger source  
                                    This parameter can be a value of @ref TIM_Trigger_Selection */  uint32_t  TriggerPolarity;   /*!< Input Trigger polarity  
                                    This parameter can be a value of @ref TIM_Trigger_Polarity */  uint32_t  TriggerPrescaler;  /*!< Input trigger prescaler  
                                    This parameter can be a value of @ref TIM_Trigger_Prescaler */  uint32_t  TriggerFilter;     /*!< Input trigger filter  
                                    This parameter can be a number between Min_Data = 0x0 and Max_Data = 0xF  */  
} TIM_SlaveConfigTypeDef;
```

主要参数有`SlaveMode`和`InputTrigger`

* 参数`SlaveMode`：从模式选择
```c
#define TIM_SLAVEMODE_DISABLE                0x00000000U                                        /*!< Slave mode disabled           */  
#define TIM_SLAVEMODE_RESET                  TIM_SMCR_SMS_2                                     /*!< Reset Mode                    */  
#define TIM_SLAVEMODE_GATED                  (TIM_SMCR_SMS_2 | TIM_SMCR_SMS_0)                  /*!< Gated Mode                    */  
#define TIM_SLAVEMODE_TRIGGER                (TIM_SMCR_SMS_2 | TIM_SMCR_SMS_1)                  /*!< Trigger Mode                  */  
#define TIM_SLAVEMODE_EXTERNAL1              (TIM_SMCR_SMS_2 | TIM_SMCR_SMS_1 | TIM_SMCR_SMS_0) /*!< External Clock Mode 1         */
```

* 参数`InputTrigger`：从模式触发源
```c
#define TIM_TS_ITR0          0x00000000U                                                       /*!< Internal Trigger 0 (ITR0)              */  
#define TIM_TS_ITR1          TIM_SMCR_TS_0                                                     /*!< Internal Trigger 1 (ITR1)              */  
#define TIM_TS_ITR2          TIM_SMCR_TS_1                                                     /*!< Internal Trigger 2 (ITR2)              */  
#define TIM_TS_ITR3          (TIM_SMCR_TS_0 | TIM_SMCR_TS_1)                                   /*!< Internal Trigger 3 (ITR3)              */  
#define TIM_TS_TI1F_ED       TIM_SMCR_TS_2                                                     /*!< TI1 Edge Detector (TI1F_ED)            */  
#define TIM_TS_TI1FP1        (TIM_SMCR_TS_0 | TIM_SMCR_TS_2)                                   /*!< Filtered Timer Input 1 (TI1FP1)        */  
#define TIM_TS_TI2FP2        (TIM_SMCR_TS_1 | TIM_SMCR_TS_2)                                   /*!< Filtered Timer Input 2 (TI2FP2)        */  
#define TIM_TS_ETRF          (TIM_SMCR_TS_0 | TIM_SMCR_TS_1 | TIM_SMCR_TS_2)                   /*!< Filtered External Trigger input (ETRF) */  
#define TIM_TS_NONE          0x0000FFFFU                                                       /*!< No trigger selected                    */
```

示例代码：
```c
TIM_SlaveConfigTypeDef TIM_SlaveConfigStructure;
/* 选择从模式: 复位模式 */
TIM_SlaveConfigStructure.SlaveMode = TIM_SLAVEMODE_RESET;
/* 选择定时器输入触发: TI1FP1 */
TIM_SlaveConfigStructure.InputTrigger = TIM_TS_TI1FP1;
HAL_TIM_SlaveConfigSynchronization(&TIM_TimeBaseStructure,&TIM_SlaveConfigStructure);
```

#### 启动输入捕获
```c
HAL_TIM_IC_Start_IT(&TIM_TimeBaseStructure,TIM_CHANNEL_2); /*开启输入捕获*/
```

#### 获取输入捕获值
```c
HAL_TIM_ReadCapturedValue(&TIM_TimeBaseStructure,TIM_CHANNEL_2); //获取当前的捕获值
```


#### 示例
```c
__HAL_RCC_GPIOA_CLK_ENABLE();
GPIO_InitTypeDef GPIO_InitStruct;
GPIO_InitStruct.Pin = GPIO_PIN_1;  
GPIO_InitStruct.Mode = GPIO_MODE_INPUT;  
GPIO_InitStruct.Pull = GPIO_PULLDOWN;  
HAL_GPIO_Init(GPIOA, &GPIO_InitStruct);

__HAL_RCC_TIM2_CLK_ENABLE();
TIM_HandleTypeDef TIM_TimeBaseStructure;
TIM_TimeBaseStructure.Instance = TIM2;  
TIM_TimeBaseStructure.Init.Prescaler = 71;  
TIM_TimeBaseStructure.Init.CounterMode = TIM_COUNTERMODE_UP;  
TIM_TimeBaseStructure.Init.Period = 65535;  
TIM_TimeBaseStructure.Init.ClockDivision = TIM_CLOCKDIVISION_DIV1;  
TIM_TimeBaseStructure.Init.AutoReloadPreload = TIM_AUTORELOAD_PRELOAD_ENABLE;  
HAL_TIM_Base_Init(&TIM_TimeBaseStructure);

TIM_IC_InitTypeDef TIM_ICInitStructure;
TIM_ICInitStructure.ICPolarity = TIM_INPUTCHANNELPOLARITY_RISING;  
TIM_ICInitStructure.ICSelection = TIM_ICSELECTION_DIRECTTI;  
TIM_ICInitStructure.ICPrescaler = TIM_ICPSC_DIV1;  
TIM_ICInitStructure.ICFilter = 8;  
HAL_TIM_IC_ConfigChannel(&TIM_TimeBaseStructure, &TIM_ICInitStructure, TIM_CHANNEL_2);

TIM_SlaveConfigTypeDef TIM_SlaveConfigStructure;
/* 选择从模式: 复位模式 */
TIM_SlaveConfigStructure.SlaveMode = TIM_SLAVEMODE_RESET;
/* 选择定时器输入触发: TI1FP1 */
TIM_SlaveConfigStructure.InputTrigger = TIM_TS_TI1FP1;
HAL_TIM_SlaveConfigSynchronization(&TIM_TimeBaseStructure,&TIM_SlaveConfigStructure);

HAL_TIM_IC_Start_IT(&TIM_TimeBaseStructure,TIM_CHANNEL_2); /*开启输入捕获*/

HAL_TIM_ReadCapturedValue(&TIM_TimeBaseStructure,TIM_CHANNEL_2); //获取当前的捕获值
```

#### STM32CUBE操作
设置通用定时器的从模式，时钟源和输入捕获，对应的GPIO会自动配置
![image.png|900](https://cdn.jsdelivr.net/gh/xuezhaorong/Picgo//Source/fix-dir/picgo/picgo-clipboard-images/2024/09/06/16-23-58-87f22877c3ade2073d94394ed7ba2689-20240906162357-40b631.png)


配置时基单元的具体参数，从模式具体参数和输入捕获通道的具体参数
![image.png|975](https://cdn.jsdelivr.net/gh/xuezhaorong/Picgo//Source/fix-dir/picgo/picgo-clipboard-images/2024/09/06/16-24-52-5bbf6926d38ab9e5d84c8eb1e93d7585-20240906162452-651487.png)

在`main.c`中开启输入捕获并获取值
```c
HAL_TIM_IC_Start_IT(&htim2,TIM_CHANNEL_2); /*开启输入捕获*/
HAL_TIM_ReadCapturedValue(&htim2,TIM_CHANNEL_2); //获取当前的捕获值
```

## ADC数模转换
### GPIO引脚初始化
```c
__HAL_RCC_GPIOA_CLK_ENABLE();

GPIO_InitTypeDef GPIO_InitStructure;
// 配置 IO
GPIO_InitStructure.Pin = GPIO_PIN_7;
GPIO_InitStructure.Mode = GPIO_MODE_ANALOG; // 模拟输入模式
GPIO_InitStructure.Pull = GPIO_NOPULL ; //不上拉不下拉
HAL_GPIO_Init(RHEOSTAT_ADC_GPIO_PORT, &GPIO_InitStructure);
```

### ADC时钟配置
```c
RCC_PeriphCLKInitTypeDef ADC_CLKInit;
// 开启ADC时钟
ADC_CLKInit.PeriphClockSelection=RCC_PERIPHCLK_ADC;
//ADC外设时钟
ADC_CLKInit.AdcClockSelection=RCC_ADCPCLK2_DIV6;
//分频因子6时钟为72M/6=12MHz
HAL_RCCEx_PeriphCLKConfig(&ADC_CLKInit);
```

### 配置ADC工作模式
```c
__HAL_RCC_ADC1_CLK_ENABLE(); // 开启ADC时钟

ADC_HandleTypeDef ADC_Handle;
ADC_Handle.Instance = ADC1; 
ADC_Handle.Init.ScanConvMode = ADC_SCAN_DISABLE;  // 非扫描模式
ADC_Handle.Init.ContinuousConvMode = DISABLE;  // 非连续转换
ADC_Handle.Init.DiscontinuousConvMode = DISABLE;  // 禁止不连续采样
ADC_Handle.Init.ExternalTrigConv = ADC_SOFTWARE_START;  // 外部触发模式
ADC_Handle.Init.DataAlign = ADC_DATAALIGN_RIGHT;  // 右对齐
ADC_Handle.Init.NbrOfConversion = 0; // 不连续采样通道
HAL_ADC_Init(&ADC_Handle); // 初始化
```

### 常规通道配置
```c
ADC_ChannelConfTypeDef ADC_Config = {0};
ADC_Config.Channel = ADC_CHANNEL_7;  // ADC通道
ADC_Config.Rank = ADC_REGULAR_RANK_1;  
ADC_Config.SamplingTime = ADC_SAMPLETIME_1CYCLE_5; // 采样间隔
HAL_ADC_ConfigChannel(&ADC_Handle, &ADC_Config); // 初始化常规通道配置
```

### 校准函数
```c
HAL_ADCEx_Calibration_Start(&ADC_Handle); //AD校准
```

### 开始转换
```c
HAL_ADC_Start(&ADC_Handle);     //启动ADC转换
HAL_ADC_PollForConversion(&ADC_Handle, 50);   //等待转换完成，50为最大等待时间，单位为ms
 
 
if(HAL_IS_BIT_SET(HAL_ADC_GetState(&ADC_Handle), HAL_ADC_STATE_REG_EOC))
{
	ADC_Value = HAL_ADC_GetValue(&ADC_Handle);   //获取AD值
}
```

### 示例
* 初始化
```c
__HAL_RCC_GPIOA_CLK_ENABLE();

GPIO_InitTypeDef GPIO_InitStructure;
// 配置 IO
GPIO_InitStructure.Pin = GPIO_PIN_7;
GPIO_InitStructure.Mode = GPIO_MODE_ANALOG; // 模拟输入模式
GPIO_InitStructure.Pull = GPIO_NOPULL ; //不上拉不下拉
HAL_GPIO_Init(RHEOSTAT_ADC_GPIO_PORT, &GPIO_InitStructure);

// ADC时钟配置
RCC_PeriphCLKInitTypeDef ADC_CLKInit;
// 开启ADC时钟
ADC_CLKInit.PeriphClockSelection=RCC_PERIPHCLK_ADC;
//ADC外设时钟
ADC_CLKInit.AdcClockSelection=RCC_ADCPCLK2_DIV6;
//分频因子6时钟为72M/6=12MHz
HAL_RCCEx_PeriphCLKConfig(&ADC_CLKInit);


__HAL_RCC_ADC1_CLK_ENABLE(); // 开启ADC时钟

ADC_HandleTypeDef ADC_Handle;
ADC_Handle.Instance = ADC1; 
ADC_Handle.Init.ScanConvMode = ADC_SCAN_DISABLE;  // 非扫描模式
ADC_Handle.Init.ContinuousConvMode = DISABLE;  // 非连续转换
ADC_Handle.Init.DiscontinuousConvMode = DISABLE;  // 禁止不连续采样
ADC_Handle.Init.ExternalTrigConv = ADC_SOFTWARE_START;  // 外部触发模式
ADC_Handle.Init.DataAlign = ADC_DATAALIGN_RIGHT;  // 右对齐
ADC_Handle.Init.NbrOfConversion = 0; // 不连续采样通道
HAL_ADC_Init(&ADC_Handle); // 初始化

ADC_ChannelConfTypeDef ADC_Config = {0};
ADC_Config.Channel = ADC_CHANNEL_7;  // ADC通道
ADC_Config.Rank = ADC_REGULAR_RANK_1;  
ADC_Config.SamplingTime = ADC_SAMPLETIME_1CYCLE_5; // 采样间隔
HAL_ADC_ConfigChannel(&ADC_Handle, &ADC_Config); // 初始化常规通道配置

HAL_ADCEx_Calibration_Start(&ADC_Handle); //AD校准
```

* 获取数值
```c
HAL_ADC_Start(&ADC_Handle);     //启动ADC转换
HAL_ADC_PollForConversion(&ADC_Handle, 50);   //等待转换完成，50为最大等待时间，单位为ms
 
 
if(HAL_IS_BIT_SET(HAL_ADC_GetState(&ADC_Handle), HAL_ADC_STATE_REG_EOC))
{
	ADC_Value = HAL_ADC_GetValue(&ADC_Handle);   //获取AD值
}
```

### STM32CUBE操作

1. 在`Clock Configuration`中，将ADC通道的时钟分频设为6分频
![image.png|800](https://cdn.jsdelivr.net/gh/xuezhaorong/Picgo//Source/fix-dir/picgo/picgo-clipboard-images/2024/12/16/12-19-24-72ddd1fc2ce1643b8cff55610ad9a303-20241216121923-847513.png)

2. 在ADC1处开启ADC通道
![image.png|525](https://cdn.jsdelivr.net/gh/xuezhaorong/Picgo//Source/fix-dir/picgo/picgo-clipboard-images/2024/12/16/12-11-16-b81999520e983979fed6b8c459f0027d-20241216121115-07cce3.png)

3. 进行ADC的设置
![image.png|525](https://cdn.jsdelivr.net/gh/xuezhaorong/Picgo//Source/fix-dir/picgo/picgo-clipboard-images/2024/12/16/12-20-41-7877d490bd2381d0f42a9bc4445af19a-20241216122041-dceda2.png)
参数解释：
* `Data Alignment`：数据对齐方式
* `Scan Conversion Mode`：扫描模式
* `ContinuOUs Conversion Mode`：连续转换模式
* `Discontinuos Conversion Mode`：不连续采样模式

4. 规则组设置
![image.png|675](https://cdn.jsdelivr.net/gh/xuezhaorong/Picgo//Source/fix-dir/picgo/picgo-clipboard-images/2024/12/16/12-23-45-795a9ea205df6d97529f6139a7e0de16-20241216122344-c8d205.png)
参数解释：
* `Enable Regular Conversions`：允许规则组
* `Number Of Conversion`：规则组数量
* `External Trigger Conversion Source`：外部触发方式，这里选择软件触发 
* `Rank`：设置采样参数
![image.png|601](https://cdn.jsdelivr.net/gh/xuezhaorong/Picgo//Source/fix-dir/picgo/picgo-clipboard-images/2024/12/16/12-26-17-4d59f4934e8fdbfbd849c16e6d8e4d21-20241216122617-056819.png)

## 串口
### 开启时钟
```c
__HAL_USART1_CLK_ENABLE();
__HAL_GPIOA_CLK_ENABLE()
```
### 初始化GPIO
```c
GPIO_InitTypeDef  GPIO_InitStruct;
/* 配置Tx引脚为复用功能  */
GPIO_InitStruct.Pin = GPIO_PIN_9;
GPIO_InitStruct.Mode = GPIO_MODE_AF_PP;  //复用推挽输出
GPIO_InitStruct.Pull = GPIO_PULLUP;      //上拉
GPIO_InitStruct.Speed = GPIO_SPEED_FREQ_VERY_HIGH; //高速
GPIO_InitStruct.Alternate = GPIO_AF7_USART1; //复用为 USART1
HAL_GPIO_Init(GPIOA, &GPIO_InitStruct);
/* 配置Rx引脚为复用功能 */
GPIO_InitStruct.Pin = GPIO_PIN_10;
GPIO_InitStruct.Alternate = GPIO_AF7_USART1;
HAL_GPIO_Init(GPIOA, &GPIO_InitStruct);
```

### 初始化USART
```c
UART_HandleTypeDef UartHandle;
UartHandle.Instance        = USART1; //USART1句柄
UartHandle.Init.BaudRate   = 115200;//波特率
UartHandle.Init.WordLength = UART_WORDLENGTH_8B; //8位字长
UartHandle.Init.StopBits   = UART_STOPBITS_1;    //一个停止位
UartHandle.Init.Parity     = UART_PARITY_NONE;   //无奇偶校验
UartHandle.Init.HwFlowCtl  = UART_HWCONTROL_NONE;//无硬件流控
UartHandle.Init.Mode       = UART_MODE_TX_RX;   //收发模式

HAL_UART_Init(&UartHandle);
```

### 设置中断
```c
/*使能串口接收断 */
__HAL_UART_ENABLE_IT(&UartHandle,UART_IT_RXNE);
/*抢占优先级0，子优先级1*/
HAL_NVIC_SetPriority(USART1_IRQn ,0,1);
HAL_NVIC_EnableIRQ(USART1_IRQn ); /*使能USART1中断通道*/
```

### 发送功能
* 发送一个字节
```c
HAL_UART_Transmit( &UartHandle,(uint8_t *)(str + k) ,1,1000);
```

* 发送字符串
```c
 /*****************  发送字符串 **********************/
 void Usart_SendString(uint8_t *str)
 {
     unsigned int k=0;
     do {
         HAL_UART_Transmit( &UartHandle,(uint8_t *)(str + k) ,1,1000);
         k++;
     } while (*(str + k)!='\0');
 }
```

### 接收功能
```c
HAL_UART_Receive_IT(&huart1,(uint8_t *)&aRxBuffer,1); //中断接收一个字符，存储到aRxBuffer中

void HAL_UART_RxCpltCallback(UART_HandleTypeDef *huart){

	HAL_UART_Receive_IT(&huart1, (uint8_t *)&aRxBuffer, 1); //再开启接收中断
}

```

### STM32CUBE操作
![|1150](https://cdn.jsdelivr.net/gh/xuezhaorong/Picgo//Source/fix-dir/picgo/picgo-clipboard-images/2024/09/25/21-21-31-8d8ececbbbc69a7f59e08fab0698a3a6-20240925212130-10b6ce.png)

在USART中选择异步模式，设置参数，对应引脚自动配置。

![image.png|1150](https://cdn.jsdelivr.net/gh/xuezhaorong/Picgo//Source/fix-dir/picgo/picgo-clipboard-images/2024/09/25/21-23-24-f4594445fee06ce2b6b4939facaa192a-20240925212324-99ce1c.png)

使能NVIC。

```c
HAL_UART_Receive_IT(&huart1,(uint8_t *)&aRxBuffer,1); //中断接收一个字符，存储到aRxBuffer中

void HAL_UART_RxCpltCallback(UART_HandleTypeDef *huart){

	HAL_UART_Receive_IT(&huart1, (uint8_t *)&aRxBuffer, 1); //再开启接收中断
}

```