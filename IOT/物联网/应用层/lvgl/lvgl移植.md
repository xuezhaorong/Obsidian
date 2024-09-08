模板链接：
不带操作系统：https://wwet.lanzn.com/iqlp82990kab
带操作系统：https://wwet.lanzn.com/idaXW2990kri
下载lvgl源码，Github链接：https://github.com/lvgl/lvgl.git
以8.2版本为例
![image.png|925](https://cdn.jsdelivr.net/gh/xuezhaorong/Picgo//Source/fix-dir/picgo/picgo-clipboard-images/2024/09/04/12-35-28-03d24528baed7efe607fbfe0aeab63f9-20240904123527-2dee1d.png)

> Needs only 32kB RAM and 128 kB Flash, a frame buffer, and at least an 1/10 screen sized buffer for rendering.

MCU要求：至少32kB RAM 和 128 kB Flash，选用`stm32f411ceu6`，按照[[STM32Hal库配置]]配置项目

## 裁剪lgvl源码
在项目下创建目录`Middlewares`
![image.png|850](https://cdn.jsdelivr.net/gh/xuezhaorong/Picgo//Source/fix-dir/picgo/picgo-clipboard-images/2024/09/04/12-41-58-e14bfcfa9c5a769767ada91c6e60e74c-20240904124157-05f3ab.png)
目录内包含文件`".\Middlewares\LVGL\GUI\lvgl"`
在lvgl目录内存放lvgl源码，将lvgl源码的四个文件`lvgl_conf_template.h`，`lvgl.h`，`example`，`src`复制到该目录下
![image.png|800](https://cdn.jsdelivr.net/gh/xuezhaorong/Picgo//Source/fix-dir/picgo/picgo-clipboard-images/2024/09/04/12-44-05-9a2bbdb995484a0c85d0b5b041c4a948-20240904124405-6451d8.png)

注意，`example`只保留`porting`目录，其余删掉，然后把`lv_conf_template.h`修改为`lv_conf.h`
![image.png|825](https://cdn.jsdelivr.net/gh/xuezhaorong/Picgo//Source/fix-dir/picgo/picgo-clipboard-images/2024/09/04/12-51-51-686cdab34d308106b0620f44adb17a2a-20240904125151-93487e.png)

## CLion配置
用Clion打开项目，修改`CMakeList_template.txt`
* 设置LVGL变量，添加函数
![image.png|775](https://cdn.jsdelivr.net/gh/xuezhaorong/Picgo//Source/fix-dir/picgo/picgo-clipboard-images/2024/09/04/12-48-10-83a9fb5bd044c8a0f53e2729662f188b-20240904124810-15d7e3.png)

```bash
set(LVGL_PATH Middlewares/LVGL/GUI/lvgl)  
set(LVGL_SRC_PATH ${LVGL_PATH}/src)  

  
function(fetch_sublist dirpath output_list)  
    # 获取到所有内容（不含路径前缀）  
    file(GLOB ALL_ITEMS  ${dirpath}/*)  
    # 目录筛选  
    foreach(ITEM ${ALL_ITEMS})  
        if(IS_DIRECTORY ${ITEM})     # 判断是否为目录  
			include_directories(${ITEM})
	        fetch_sublist(${ITEM} _list)  
        endif()  
    endforeach()  
endfunction()
```

* 引入头文件和源文件，并加入到最终构建中
![image.png|900](https://cdn.jsdelivr.net/gh/xuezhaorong/Picgo//Source/fix-dir/picgo/picgo-clipboard-images/2024/09/04/12-49-37-c8268894b0ed197ccb881d1582e634fb-20240904124936-56fc0d.png)

```bash
include_directories(${LVGL_PATH} ${LVGL_PATH}/examples/porting ${LVGL_SRC_PATH})  
  
# 子文件  
fetch_sublist(${LVGL_SRC_PATH})  


file(GLOB_RECURSE LVGL_SOURCES "${LVGL_PATH}/*")

add_executable(${PROJECT_NAME}.elf ${SOURCES} ${LINKER_SCRIPT} ${LVGL_SOURCES})
```

重新cmake，构建

## 修改文件
打开`lvgl_conf.h`文件，将`#if 0`改为`#if 1`
![image.png|1000](https://cdn.jsdelivr.net/gh/xuezhaorong/Picgo//Source/fix-dir/picgo/picgo-clipboard-images/2024/09/04/13-23-42-50d4cc853adda2b227e59752e61e789d-20240904132341-e4e5ca.png)


打开`lv_port_disp_template.h`文件，配置输出
将`#if 0`改为`#if 1`
添加`lvgl`头文件和lcd驱动头文件

![image.png|1000](https://cdn.jsdelivr.net/gh/xuezhaorong/Picgo//Source/fix-dir/picgo/picgo-clipboard-images/2024/09/04/13-04-38-9af08a3945ac11b93952a33d3e5ed922-20240904130437-beb4a2.png)

打开``lv_port_disp_template.c`文件
同样是将`#if 0`改为`#if 1`，并加入宽度和高度的宏定义
```cpp
#define MY_DISP_HOR_RES LCD_W  
#define MY_DISP_VER_RES LCD_H
```

![image.png|800](https://cdn.jsdelivr.net/gh/xuezhaorong/Picgo//Source/fix-dir/picgo/picgo-clipboard-images/2024/09/04/13-12-40-d1e8baea83e5539b056a0e59c4a0969b-20240904131239-11787b.png)

到`lv_port_disp_init`函数中，配置图形缓存模式，注释掉2，3模式

![|900](https://cdn.jsdelivr.net/gh/xuezhaorong/Picgo//Source/fix-dir/picgo/picgo-clipboard-images/2024/09/04/13-14-11-7043c891af105340338e51522936e355-20240904131410-152dea.png)

设置宽度和高度
![image.png|925](https://cdn.jsdelivr.net/gh/xuezhaorong/Picgo//Source/fix-dir/picgo/picgo-clipboard-images/2024/09/04/13-16-41-1ff69e3f034ed0e95f403292da4793f2-20240904131640-aa2519.png)


在`disp_init`函数中加入LCD的初始化函数

![image.png|850](https://cdn.jsdelivr.net/gh/xuezhaorong/Picgo//Source/fix-dir/picgo/picgo-clipboard-images/2024/09/04/13-15-09-a15126bc825e0376bd489af3aec2196f-20240904131509-30567b.png)

在`disp_flush`函数中设置打点输出，注释掉原来的内容，加入LCD打点输出的函数

![image.png|900](https://cdn.jsdelivr.net/gh/xuezhaorong/Picgo//Source/fix-dir/picgo/picgo-clipboard-images/2024/09/04/13-17-32-bbb9ad86229c5f7127936de42b3da4d2-20240904131731-18cb6c.png)

## 引入lvgl
在`main.c`中引入头文件
```cpp
#include "lvgl.h"
#include "lv_port_disp_template.h"
```

![|1000](https://cdn.jsdelivr.net/gh/xuezhaorong/Picgo//Source/fix-dir/https/cdn.jsdelivr.net/gh/xuezhaorong/Picgo/Source/fix-dir/picgo/picgo-clipboard-images/2024/09/04/2024/09/04/13-25-08-cbd5737785f138cf2e6c714bcb611767-13-24-37-cbd5737785f138cf2e6c714bcb611767-20240904132436-680eb8-6f83bd.png)

配置1ms的定时器
![image.png|900](https://cdn.jsdelivr.net/gh/xuezhaorong/Picgo//Source/fix-dir/picgo/picgo-clipboard-images/2024/09/04/15-43-34-93879a8f95a5616383699c9874d5b87f-20240904154333-b9a8f0.png)
启动定时中断，在中断回调函数中加入lvgl心跳配置，**注意：抢占优先级要为最高**
```cpp
void HAL_TIM_PeriodElapsedCallback(TIM_HandleTypeDef *htim) {  
    if (htim == (&htim9)) {  
        lv_tick_inc(1);                 /*lvgl的1ms心跳*/  
    }  
}
```

加入lvgl和输出的 初始化代码
![image.png|850](https://cdn.jsdelivr.net/gh/xuezhaorong/Picgo//Source/fix-dir/picgo/picgo-clipboard-images/2024/09/04/15-45-25-a6764e96d804a6b763431fe70259c42a-20240904154525-845221.png)

```cpp
lv_init();  
lv_port_disp_init();
```

在`while`循环中添加
```cpp
lv_timer_handler();  
HAL_Delay(10);
```

## 加入FreeRTOS
根据[[STM32Hal库配置]]在上面的基础上配置FreeRTOS，并取消定时器
![image.png|1075](https://cdn.jsdelivr.net/gh/xuezhaorong/Picgo//Source/fix-dir/picgo/picgo-clipboard-images/2024/09/05/16-33-33-30ab69e319af398e16c831f1b6d8797a-20240905163332-d51e96.png)

修改`CMakeLists_template.txt`，加入FreeRTOS进项目中
```bash
set(FREERTOS_PATH Middlewares/Third_Party/FreeRTOS/Source)

include_directories(${FREERTOS_SOURCES})

fetch_sublist(${FREERTOS_PATH})

file(GLOB_RECURSE FREERTOS_SOURCES "${FREERTOS_PATH}/*")

add_executable($${PROJECT_NAME}.elf $${SOURCES} $${LINKER_SCRIPT} ${LVGL_SOURCES} ${FREERTOS_SOURCES})

```

删除lvgl初始化代码和循环中的代码，放在在任务中执行
![image.png|950](https://cdn.jsdelivr.net/gh/xuezhaorong/Picgo//Source/fix-dir/picgo/picgo-clipboard-images/2024/09/05/16-37-10-9e24b6b655fede62cfa357fe977622de-20240905163710-f52343.png)
![image.png|950](https://cdn.jsdelivr.net/gh/xuezhaorong/Picgo//Source/fix-dir/picgo/picgo-clipboard-images/2024/09/05/16-37-19-3af8726f50f7c5b127dae7e1ec3ea4a0-20240905163719-048905.png)

打开`lv_conf.h`文件，找到`LV_TICK_CUSTOM`，将其改为1，`LV_TICK_CUSTOM_INCLUDE`改为`"cmsis_os.h"`，`LV_TICK_CUSTOM_SYS_TIME_EXPR`改为`xTaskGetTickCount()`

![image.png|1100](https://cdn.jsdelivr.net/gh/xuezhaorong/Picgo//Source/fix-dir/picgo/picgo-clipboard-images/2024/09/05/16-38-15-ff84d9dc92a3b3d838007d1d6e3f1850-20240905163815-3c4990.png)

 ## 内存不够解决方案
 1. 修改`lvgl.conf`，减少分配给lvgl管理的内存
```c
#define LV_MEM_SIZE (20U * 1024U)          /*[bytes]*/
```

![image.png|1050](https://cdn.jsdelivr.net/gh/xuezhaorong/Picgo//Source/fix-dir/picgo/picgo-clipboard-images/2024/09/07/18-51-00-9c9059bfe0528e997c19385fa48ffabe-20240907185059-a895a1.png)

2. 配置`lv_port_disp_template.c`，适当减少图形缓存区的大小，同时需要兼顾运行效果
```c
static lv_color_t buf_1[MY_DISP_HOR_RES * 10];                          /*A buffer for 10 rows*/
```

![image.png|1100](https://cdn.jsdelivr.net/gh/xuezhaorong/Picgo//Source/fix-dir/picgo/picgo-clipboard-images/2024/09/07/18-53-38-0ab6a8a1a8b38f3e5ac3a28fbf861fec-20240907185337-47b2f8.png)

3. 适当减少分配给FreeRTOS的内存

![image.png|1100](https://cdn.jsdelivr.net/gh/xuezhaorong/Picgo//Source/fix-dir/picgo/picgo-clipboard-images/2024/09/07/18-58-08-0582c22b953f1d68bddd509716559e8e-20240907185807-aca4f7.png)

## LVGL模拟器安装
1. 下载lvgl8.3版本，链接：[lvgl/lvgl at release/v8.3 (github.com)](https://github.com/lvgl/lvgl/tree/release/v8.3)，下载好的文件夹中**lv_drives**和**lvgl**是空文件夹
![image.png|1075](https://cdn.jsdelivr.net/gh/xuezhaorong/Picgo//Source/fix-dir/picgo/picgo-clipboard-images/2024/09/08/12-28-43-01d1cd25a485a4c025b431c7e02546bf-20240908122842-52d122.png)

2. 下载[https://github.com/lvgl/lvgl](https://github.com/lvgl/lvgl "https://github.com/lvgl/lvgl")中的所有文件，下载好后复制到**lvgl**文件夹中（注意版本为8.3）

![image.png|1075](https://cdn.jsdelivr.net/gh/xuezhaorong/Picgo//Source/fix-dir/picgo/picgo-clipboard-images/2024/09/08/12-29-50-63261c2df9ae6b6a3c22ecf02adc9a45-20240908122950-d43e60.png)

下载[https://github.com/lvgl/lv_drivers](https://github.com/lvgl/lv_drivers "https://github.com/lvgl/lv_drivers")中的所有文件，下载好后复制到**lv_drivers**文件夹中

![image.png|1075](https://cdn.jsdelivr.net/gh/xuezhaorong/Picgo//Source/fix-dir/picgo/picgo-clipboard-images/2024/09/08/12-30-34-1dc554053a2893a6c8c935da530da7f6-20240908123033-4e8fa0.png)

3. 下载Mingw，链接：[SourceForge](http://sourceforge.net/projects/mingw-w64/files/mingw-w64/mingw-w64-release/ "SourceForge").
![image.png|950](https://cdn.jsdelivr.net/gh/xuezhaorong/Picgo//Source/fix-dir/picgo/picgo-clipboard-images/2024/09/08/12-31-55-7745ad9768bf46b5096fbef2c9b3258b-20240908123154-0dacbd.png)

注意要添加到系统环境变量中
![image.png|625](https://cdn.jsdelivr.net/gh/xuezhaorong/Picgo//Source/fix-dir/picgo/picgo-clipboard-images/2024/09/08/12-37-33-005757768579510b57c47e03d3b0fc16-20240908123733-086f67.png)

4. 下载SDL，链接：https://github.com/libsdl-org/SDL/releases/download/release-2.0.22/SDL2-devel-2.0.22-mingw.zip，进入到`x86_64_w64-mingw32`目录下
![image.png|1025](https://cdn.jsdelivr.net/gh/xuezhaorong/Picgo//Source/fix-dir/picgo/picgo-clipboard-images/2024/09/08/12-41-46-8591f1e2e4cf589f56851b42d62731d1-20240908124145-7246e8.png)

将`x86_64_w64-mingw32/include/SDL2`复制到mingw/x86_64_w64-mingw32/include`文件夹中
![image.png|950](https://cdn.jsdelivr.net/gh/xuezhaorong/Picgo//Source/fix-dir/picgo/picgo-clipboard-images/2024/09/08/12-44-44-130e4ab128d1e569d2b593b7fb8c19c1-20240908124443-3bd721.png)

将`x86_64_w64-mingw32/lib`所有的文件复制到`mingw/x86_64_w64-mingw32/lib`中
![image.png|950](https://cdn.jsdelivr.net/gh/xuezhaorong/Picgo//Source/fix-dir/picgo/picgo-clipboard-images/2024/09/08/12-47-32-89cde6a338a0ec411028ceee5c6b8e97-20240908124732-531770.png)

5. 