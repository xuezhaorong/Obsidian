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
set(LVGL_SRC_DIRPATH "")  
  
function(fetch_sublist dirpath output_list)  
    # 获取到所有内容（不含路径前缀）  
    file(GLOB ALL_ITEMS  ${dirpath}/*)  
    # 目录筛选  
    foreach(ITEM ${ALL_ITEMS})  
        if(IS_DIRECTORY ${ITEM})     # 判断是否为目录  
            list(APPEND _list ${ITEM})  
            set(${output_list} ${_list} PARENT_SCOPE)  
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
fetch_sublist(${LVGL_SRC_PATH} LVGL_SRC_DIRPATH)  
foreach (ITEM ${LVGL_SRC_DIRPATH})  
    include_directories(${ITEM})  
endforeach ()

file(GLOB_RECURSE LVGL_SOURCES "${LVGL_PATH}/*")

add_executable(${PROJECT_NAME}.elf ${SOURCES} ${LINKER_SCRIPT} ${LVGL_SOURCES})
```

重新cmake，构建

## 修改文件
打开`lv_port_disp_template.h`文件，配置输出
将`#if 0`改为`#if 1`
添加`lvgl`头文件和lcd驱动头文件

![image.png|900](https://cdn.jsdelivr.net/gh/xuezhaorong/Picgo//Source/fix-dir/picgo/picgo-clipboard-images/2024/09/04/13-04-38-9af08a3945ac11b93952a33d3e5ed922-20240904130437-beb4a2.png)
