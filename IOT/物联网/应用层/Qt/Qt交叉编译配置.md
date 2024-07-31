## window-aarch64
[[交叉编译#交叉编译介绍]]选择：**gcc-arm-mingw-w64-i686-aarch64-none-linux-gnu**交叉编译工具
> 交叉编译工具包链接：https://www.123pan.com/s/zum7Vv-6PCXH.html
### Window配置
#### 环境配置
* 下载交叉编译工具包，并且设置环境变量
![image.png|600](https://cdn.jsdelivr.net/gh/xuezhaorong/Picgo//Source/fix-dir/picgo/picgo-clipboard-images/2024/07/25/08-46-56-d74d544e6a09dbe0a9457b1c4b3aacaf-20240725084655-f242ee.png)

* 安装并配置Qt（[[Clion配置Qt]]）进入到Qt目录下，使用加速打开**MaintenanceTool.exe**
```bash
.\MaintenanceTool.exe --mirror https://mirrors.cloud.tencent.com/qt/
```

* 下载Qt源码， 旧版需要点击**Archive**筛选，勾选**Sources**选项点击下一步，等待安装
![image.png|500](https://cdn.jsdelivr.net/gh/xuezhaorong/Picgo//Source/fix-dir/picgo/picgo-clipboard-images/2024/07/25/08-55-02-0e4d01bcf9a4614c33ff2fa92cc22c1b-20240725085502-4ce654.png)

*  安装cmake，链接：https://wwl.lanzn.com/iiIAd25h16if
#### 文件修改
* 下载完成后，在对应版本的目录下可以看到，进入Src源码文件进行修改
	![image.png|625](https://cdn.jsdelivr.net/gh/xuezhaorong/Picgo//Source/fix-dir/picgo/picgo-clipboard-images/2024/07/25/08-57-25-d17822f2ed8f8da0234fa73159a0feb4-20240725085725-34333c.png)

	1. 删除**qtquick3d**目录
	2. 对以下3个目录中的文件进行修改，添加头文件
	```c++
	#include <stdexcept> 
	#include <limits>
	```
	**文件路径：**
```bash
qtbase/src/corelib/global/qendian.h qtbase/src/corelib/text/qbytearraymatcher.h qtdeclarative/src/qmldebug/qqmlprofilerevent_p.h
```
3. 在 `qtbase/src/corelib/global/qglobal.h`的`__cplusplus`中添加`#include <limits>`
4. 在`D:\Qt\5.15.2\Src\qtbase\mkspecs\linux-aarch64-gnu-g++`下，修改`qmake.config`
	![image.png|700](https://cdn.jsdelivr.net/gh/xuezhaorong/Picgo//Source/fix-dir/picgo/picgo-clipboard-images/2024/07/25/09-40-51-120c8452270ee0d1fa7c9d0f72f256eb-20240725094050-86d160.png)
	复制并注释掉原来的工具选项，替换成本地的交叉编译工具包的选项
5. .  可选，向`qtlocation/src/3rdparty/mapbox-gl-native/src/mbgl/util/convert.cpp`中添加，`#include<stdint.h>`，在编译时会跳过这个库，所以可选

#### 编译交叉编译环境下的Qt库
* 在**Src**目录下新建**build**目录，打开Qt的mingw命令窗口，进入到**build**中
![image.png|429](https://cdn.jsdelivr.net/gh/xuezhaorong/Picgo//Source/fix-dir/picgo/picgo-clipboard-images/2024/07/25/09-10-19-9f6a193ae95b07790fb65ff7700a7889-20240725091017-9dbcb0.png)

![image.png|550](https://cdn.jsdelivr.net/gh/xuezhaorong/Picgo//Source/fix-dir/picgo/picgo-clipboard-images/2024/07/25/09-11-19-bc8a85cc64c46774225503c6ba0d2b39-20240725091118-08aef7.png)

* 输入指令开始配置编译，`D:\Qt\5.15.2\Src\`替换成Src的绝对路径，`D:\Program\Qt\arm_qt`为编译后的qt库的目录，要先创建一个
```bash
D:\Qt\5.15.2\Src\configure.bat -release -opensource -prefix D:\Program\Qt\arm_qt -nomake tests -nomake examples -no-opengl -skip qtvirtualkeyboard -skip qtwebengine -skip qt3d -skip qtquick3d -skip qttools -skip qtscript -skip qtlocation -xplatform linux-aarch64-gnu-g++
```
	配置选项说明（具体可以在命令行内输入“configure -help”命令查看详细说明）：
		1. -release: 编译release版本
		2.  -opensource: 表示开源许可
		3. -prefix: Qt安装路径
		4. -nomake: 表示不编译后面参数指定的模块
		5. -no-opengl: 表示不安装OpenGL
		6. -skip: 表示不安装的qt工具包
		7. -xplatform 表示使用源码路径qtbase\mkspecs\linux-aarch64-gnu-g++文件夹内的配置，编译时会自动去该路径下找到配置文件进行编译
	
* 等待配置完成，中途要选择y接受授权，没有**Error**可以开始编译，-j40可以根据电脑配置调整对应的线程数量
```bash
mingw32-make -j40
```

* 编译完成后，没有Error，可以进行安装
```bash
mingw32-make install
```
* 安装后，qt库在对应目录下
	![image.png|650](https://cdn.jsdelivr.net/gh/xuezhaorong/Picgo//Source/fix-dir/picgo/picgo-clipboard-images/2024/07/25/09-38-22-cd12c5631289564760cc737233530f9c-20240725093822-c59522.png)

**注意：** 
1. 在配置，编译和安装过程中的错误大部分都是由于本地环境下工具包版本，Src源文件库文件代码，以及配置过程中的选项导致，如果出现错误，可以根据build中的config日志文件中的提示进行修改。
2. 在配置时跳过了一些不常用的工具包和库来避免错误和减少编译时间，若需要用到可以根据需求进行修改。

 ### Clion
 > 模板链接：https://wwl.lanzn.com/iY8AH25gl9od
Clion同样以[[Clion配置Qt]]进行配置
* 需要修改的是，在创建项目时，不需要写入Qt前缀路径以便在交叉编译构建和桌面构建之间切换
	![image.png|575](https://cdn.jsdelivr.net/gh/xuezhaorong/Picgo//Source/fix-dir/picgo/picgo-clipboard-images/2024/07/25/10-02-39-995497df080d13d3e958e1c0c0c00eee-20240725100238-8ea4a2.png)
	将CmakeLists中的`CMAKE_PREFIX_PATH`删去
	![image.png|475](https://cdn.jsdelivr.net/gh/xuezhaorong/Picgo//Source/fix-dir/picgo/picgo-clipboard-images/2024/07/25/10-12-06-e4ff5527f560afca6b70dce09a808032-20240725101206-42feff.png)
	在Cmake中添加Cmake选项指定`CMAKE_PREFIX_PATH`
	![image.png|625](https://cdn.jsdelivr.net/gh/xuezhaorong/Picgo//Source/fix-dir/picgo/picgo-clipboard-images/2024/07/25/10-13-24-4b47cf3890f8a302348f823cacbec393-20240725101324-47466f.png)


* 在配置完之后，在根目录新建一个交叉编译工具链cmake文件，`toolchain-aarch64-linux-gnu.cmake`，在里面写入配置代码，`CMAKE_C_COMPILER`和`CMAKE_CXX_COMPILER`后面写入对应的本地交叉编译工具包的gcc和g++路径
```bash
set(CMAKE_SYSTEM_NAME Linux)  
set(CMAKE_SYSTEM_PROCESSOR aarch64)  
  
# specify the cross compiler  
set(CMAKE_C_COMPILER D:/Program/gcc-arm-mingw-w64-i686-aarch64-none-linux-gnu/bin/aarch64-none-linux-gnu-gcc.exe)  
set(CMAKE_CXX_COMPILER D:/Program/gcc-arm-mingw-w64-i686-aarch64-none-linux-gnu/bin/aarch64-none-linux-gnu-g++.exe)  
  
# specify the sysroot (optional, but recommended)  
# set(CMAKE_SYSROOT /path/to/sysroot)  
  
# additional flags for cross-compiling  
set(CMAKE_FIND_ROOT_PATH_MODE_PROGRAM NEVER)  
set(CMAKE_FIND_ROOT_PATH_MODE_LIBRARY ONLY)  
set(CMAKE_FIND_ROOT_PATH_MODE_INCLUDE ONLY)  
set(CMAKE_FIND_ROOT_PATH_MODE_PACKAGE ONLY)
```

* 设置中新建一个工具链，配置如图
![image.png|850](https://cdn.jsdelivr.net/gh/xuezhaorong/Picgo//Source/fix-dir/picgo/picgo-clipboard-images/2024/07/25/10-06-31-9c474a5a917eab2cc279146b7f6a0ebd-20240725100630-c320f1.png)
* 新建Cmake，工具链选择新建的工具链
	![image.png|600](https://cdn.jsdelivr.net/gh/xuezhaorong/Picgo//Source/fix-dir/picgo/picgo-clipboard-images/2024/07/25/10-07-30-690ec4398d6531454312650d7e1cb570-20240725100730-bb4d59.png)
	在Cmake选项中，指定`CMAKE_PREFIX_PATH`和工具链配置文件
```bash
cmake
-DCMAKE_PREFIX_PATH=D:/Program/Qt/arm_qt
-DCMAKE_TOOLCHAIN_FILE=./toolchain-aarch64-linux-gnu.cmake
```

此时出现两个Cmake配置：
![image.png|925](https://cdn.jsdelivr.net/gh/xuezhaorong/Picgo//Source/fix-dir/picgo/picgo-clipboard-images/2024/07/25/10-20-53-954d77e8c2a64907a55b08250ee1bcb6-20240725102053-076ee4.png)

重新cmake，编译，可在对应的cmake-build下看到可执行文件
![image.png|500](https://cdn.jsdelivr.net/gh/xuezhaorong/Picgo//Source/fix-dir/picgo/picgo-clipboard-images/2024/07/25/10-21-52-85f0b4d3084886bfafe06243f9b83d5d-20240725102152-1d3a2b.png)

## aarch64（树莓派4b为例）配置
根据[[安装qt5]]配置树莓派，window上的qt版本根据树莓派上的qt版本确定，可以有小版本的偏差
![image.png|515](https://cdn.jsdelivr.net/gh/xuezhaorong/Picgo//Source/fix-dir/picgo/picgo-clipboard-images/2024/07/25/10-24-33-4df61a73cd0d9dbe25892a550be99f88-20240725102432-0a10a4.png)

使用vnc传输aarch64可执行文件到树莓派上运行
![image.png|625](https://cdn.jsdelivr.net/gh/xuezhaorong/Picgo//Source/fix-dir/picgo/picgo-clipboard-images/2024/07/25/10-27-11-1fd8311e47a0f14b910530a533f2dfbf-20240725102711-3577e7.png)

需要为传入的可执行文件添加权限
![image.png|502](https://cdn.jsdelivr.net/gh/xuezhaorong/Picgo//Source/fix-dir/picgo/picgo-clipboard-images/2024/07/25/10-28-22-bf67a7a2673197bc1790494e31319d66-20240725102822-372761.png)

然后执行可执行文件
![image.png|557](https://cdn.jsdelivr.net/gh/xuezhaorong/Picgo//Source/fix-dir/picgo/picgo-clipboard-images/2024/07/25/10-29-01-21c859956a7fcfef7ce501dae8c89a96-20240725102900-186f67.png)

### 加入WiringPi库
WiringPi库链接：https://wwl.lanzn.com/im5pV25orqrc
下载WiringPi库，放到项目根目录下：
![image.png|950](https://cdn.jsdelivr.net/gh/xuezhaorong/Picgo//Source/fix-dir/picgo/picgo-clipboard-images/2024/07/27/14-42-25-decb0ad8322c975ecc898701b6717e90-20240727144224-b07864.png)
修改交叉工具包中gpio.h的代码以适配wiringPi库，进入路径:`D:\Program\gcc-arm-mingw-w64-i686-aarch64-none-linux-gnu\aarch64-none-linux-gnu\libc\usr\include\linux`，打开`gpio.h`文件，再65到68行加入三行代码。
```c
#define GPIOHANDLE_REQUEST_BIAS_PULL_UP (1UL << 5)
#define GPIOHANDLE_REQUEST_BIAS_PULL_DOWN (1UL << 6)
#define GPIOHANDLE_REQUEST_BIAS_DISABLE (1UL << 7)
```
![image.png|800](https://cdn.jsdelivr.net/gh/xuezhaorong/Picgo//Source/fix-dir/picgo/picgo-clipboard-images/2024/07/27/14-46-16-9b3fb9e817f0b8986d17ceb22e59d4ea-20240727144616-1390f0.png)

在项目中导入wiringPi库，打开`CmakeLists.txt`文件，加入代码导入库。
```cmake
set(WIRINGPI_PATHS ${PROJECT_SOURCE_DIR}/wiringPi)  
  
FILE(GLOB_RECURSE WIRINGPI_HEADER_FILES ${WIRINGPI_PATHS}/include/*.h)  
  
aux_source_directory(${WIRINGPI_PATHS}/src WIRINGPI_SOURCE_FILES)
```
同时在`include_directories`要引入
![image.png|1075](https://cdn.jsdelivr.net/gh/xuezhaorong/Picgo//Source/fix-dir/picgo/picgo-clipboard-images/2024/07/27/18-25-28-3c78bb2b054c53cede3a895176f2c187-20240727182527-687b29.png)



加入判断交叉编译时，加入库生成可执行文件的代码。
```cmake
# 检测是否交叉编译  
if(NOT CMAKE_HOST_SYSTEM_NAME STREQUAL CMAKE_SYSTEM_NAME)  
    message(STATUS "Cross compiling detected")  
    list(APPEND SOURCE_FILES ${WIRINGPI_SOURCE_FILES})  
    list(APPEND HEADER_FILES ${WIRINGPI_HEADER_FILES})  
else()  
    message(STATUS "Native compiling detected")  
endif()
```

![image.png|925](https://cdn.jsdelivr.net/gh/xuezhaorong/Picgo//Source/fix-dir/picgo/picgo-clipboard-images/2024/07/27/14-49-41-dda96ec2658dadfe283f4a90460ae7e8-20240727144940-0b0e52.png)

在交叉编译环境下需要显性导入crypt链接库，加入代码
```cmake
# 如果是在类 Unix 系统上编译，添加 crypt 库  
if(UNIX AND NOT APPLE)  
    target_link_libraries(Project crypt)  
endif()
```

![|1025](https://cdn.jsdelivr.net/gh/xuezhaorong/Picgo//Source/fix-dir/picgo/picgo-clipboard-images/2024/07/27/14-50-42-d6e321ba28a323cb365efcad7333767b-20240727145042-dbedd2.png)\

重新cmake，导入wiringPi库成功

### 使用交叉工具链编译opencv
1. 树莓派安装`aarch64-linux-gnu`交叉编译工具。
```bash
sudo apt-get install g++-aarch64-linux-gnu
```
2. 下载`opencv`源码
```bash
	git clone https://github.com/opencv/opencv.git
```
3.  进入opecv目录下，新建build目录
```bash
cd opencv
mkdir build && cd build
```
4. 配置编译，生成静态库
```bash
cmake -DCMAKE_TOOLCHAIN_FILE=../platforms/linux/aarch64-gnu.toolchain.cmake .. -DBUILD_SHARED_LIBS=OFF
```
5. 编译和安装
```bash
make -j2 && make install
```
6. 在`build/install` 下生成编译后的文件，移动到window下
![image.png|750](https://cdn.jsdelivr.net/gh/xuezhaorong/Picgo//Source/fix-dir/picgo/picgo-clipboard-images/2024/07/29/12-40-34-f7a71c6ae9d97cf808181d3a5e0c1e4e-20240729124033-8ded5d.png)
7. 查看`D:\Program\opencv_install\lib\cmake\opencv4`下的`OpenCVConfig.cmake`文档，在Clion中添加库依赖
```bash
set(OpenCV_DIR D:/Program/opencv_install)

find_package(  
        OpenCV REQUIRED  
)

target_link_libraries(Project  
        ${OpenCV_LIBS}  
)
```
