## GPIO
Hal中GPIO初始化可以同时配置输出和输入模式
```c
/**  
  * @brief  Initializes the GPIOx peripheral according to the specified parameters in the GPIO_Init.  * @param  GPIOx: where x can be (A..G depending on device used) to select the GPIO peripheral  * @param  GPIO_Init: pointer to a GPIO_InitTypeDef structure that contains  *         the configuration information for the specified GPIO peripheral.  * @retval None  */void HAL_GPIO_Init(GPIO_TypeDef  *GPIOx, GPIO_InitTypeDef *GPIO_Init)
```
