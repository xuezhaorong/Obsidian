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


## 外部中断
