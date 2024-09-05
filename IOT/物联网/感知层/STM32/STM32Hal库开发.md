## GPIO
Hal中GPIO初始化可以同时配置输出和输入模式
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

-第一个参数`Pin`为GPIO的端口
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

-第二个参数`Mode`为GPIO