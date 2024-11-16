## 保存自动格式化代码
ArtisiticStyle工具官网链接：https://astyle.sourceforge.net/
1. 加入`Beautidier`插件 
![image.png|800](https://cdn.jsdelivr.net/gh/xuezhaorong/Picgo//Source/fix-dir/picgo/picgo-clipboard-images/2024/10/31/12-42-54-de52ad44f2382544adc182b72701b356-20241031124253-afc49f.png)
设置后重启Qt

2. 下载ArtisiticStyle工具
![image.png|975](https://cdn.jsdelivr.net/gh/xuezhaorong/Picgo//Source/fix-dir/picgo/picgo-clipboard-images/2024/10/31/12-43-39-40f0dd11096c986fd20f5d78bb9c09f9-20241031124338-c47d18.png)

3. 编译安装
```cmake
mkdir  as-gcc-exe
cd  as-gcc-exe
cmake  ../
make
make install 
```
注意：如果是Window操作系统直接下载可执行包:https://www.123684.com/s/zum7Vv-c86XH
4. 配置美化器
![image.png|825](https://cdn.jsdelivr.net/gh/xuezhaorong/Picgo//Source/fix-dir/picgo/picgo-clipboard-images/2024/10/31/12-45-39-943d092201c4851f05811dbe9bd0574e-20241031124538-b73a5d.png)

![image.png|850](https://cdn.jsdelivr.net/gh/xuezhaorong/Picgo//Source/fix-dir/picgo/picgo-clipboard-images/2024/10/31/12-46-53-44f86c2b96a3fd3140cba14fff2509dc-20241031124652-9a5c87.png)

添加样式
```
style=allman             # 设置 大括号 风格
indent=spaces=4          # 每个缩进使用 4 个空格
attach-closing-while     # 将“do-while”语句的结束“while”附加到结束大括号
indent-preproc-block     # 缩进预处理
indent-preproc-define    # 缩进以反斜杠结尾的多行预处理器定义
indent-col1-comments     # 从第一列开始缩进 C++ 注释
min-conditional-indent=1 # 设置在由多行构建标题时添加的最小缩进量
break-blocks             # 在标题块周围填充空行（例如“if”、“for”、“while”
pad-oper                 # 在运算符周围插入空格填充
pad-comma                # 在逗号后插入空格填充
```

注意：Window操作系统需要选择`astyle`的路径
![image.png|1000](https://cdn.jsdelivr.net/gh/xuezhaorong/Picgo//Source/fix-dir/picgo/picgo-clipboard-images/2024/11/05/09-49-57-845ce3e253ba0641d536aa73117e374b-20241105094956-a925a9.png)

## Qt分模块编译

* Clion
以DiagramModel模块为例
1. 新建`DiagramModel`目录，在里面新建`include`,`src`目录和`CMakeLists`文件，并移入`.cpp`和`.h`
![image.png|490](https://cdn.jsdelivr.net/gh/xuezhaorong/Picgo//Source/fix-dir/picgo/picgo-clipboard-images/2024/10/30/13-55-44-0e5d87f6210f664ab036a3283f62d91f-20241030135543-2c2943.png)

2. 编写`CMakeLists`文件
```cmake
cmake_minimum_required(VERSION 3.26)  
project(DiagramModel)  
  
set(CMAKE_CXX_STANDARD 17)  
set(CMAKE_AUTOMOC ON)  
set(CMAKE_AUTORCC ON)  
set(CMAKE_AUTOUIC ON)  
  
add_library(diagramModel STATIC ${SOURCE_FILES} )  
  
  
find_package(Qt5 COMPONENTS  
        Core  
        Gui  
        Widgets  
        Sql  
        REQUIRED)  
        
# 对外部的头文件
target_include_directories(diagramModel  
        PUBLIC  
        ${CMAKE_CURRENT_SOURCE_DIR}/include  
)  
  
# 源文件  
file(GLOB_RECURSE HEADER_FILES ${CMAKE_CURRENT_SOURCE_DIR}/include/*.h)  
file(GLOB_RECURSE SOURCE_FILES ${CMAKE_CURRENT_SOURCE_DIR}/src/*.cpp)  
  
# 添加源文件  
target_sources(diagramModel PRIVATE ${SOURCE_FILES} ${HEADER_FILES})  
  
  
target_link_libraries(diagramModel  
        Qt5::Core  
        Qt5::Gui  
        Qt5::Widgets  
        Qt5::Sql  
)
```

3. 编写顶层`CMakeLists`
添加模块库的路径
```cmake
include_directories(${PROJECT_SOURCE_DIR}/DiagramModel/include)
add_subdirectory(${PROJECT_SOURCE_DIR}/DiagramModel)

```

如果要添加多个模块，需要先导入头文件路径，再假如库路径
如：
```cmake
include_directories(${PROJECT_SOURCE_DIR}/DataBaseModel/include)  
include_directories(${PROJECT_SOURCE_DIR}/DiagramModel/include)  
  
  
add_subdirectory(${PROJECT_SOURCE_DIR}/DataBaseModel)  
add_subdirectory(${PROJECT_SOURCE_DIR}/DiagramModel)
```

链接模块
```cmake
target_link_libraries(Project  
        Qt5::Core  
        Qt5::Gui  
        Qt5::Widgets  
        Qt5::Sql  
        diagramModel  
)
```

* QtCreate
以DataBaseModel为例
1. 在项目目录下新建`DataBaseModel`目录，新建`DataBaseModel.pri`文件
![image.png|675](https://cdn.jsdelivr.net/gh/xuezhaorong/Picgo//Source/fix-dir/picgo/picgo-clipboard-images/2024/10/30/14-30-06-c1c603c296c25d5159860764a0f44eda-20241030143005-e4008b.png)

2. 在顶层`.pro`文件添加
![image.png|400](https://cdn.jsdelivr.net/gh/xuezhaorong/Picgo//Source/fix-dir/picgo/picgo-clipboard-images/2024/10/30/14-31-59-48310fc91b61ae85cae8e831ada9e3aa-20241030143158-785e2f.png)

```cpp
INCLUDEPATH += $$PWD/DataBaseModel
include($$PWD/DataBaseModel/DataBaseModel.pri)
```

3. 编译后，出现在项目文件栏中，可以添加类等文件
![image.png|500](https://cdn.jsdelivr.net/gh/xuezhaorong/Picgo//Source/fix-dir/picgo/picgo-clipboard-images/2024/10/30/14-33-14-dca3cb4504d8490507314c7e4def564b-20241030143313-c9e125.png)
