## window-aarch64
[[编译#交叉编译介绍]]选择：**gcc-arm-mingw-w64-i686-aarch64-none-linux-gnu**交叉编译工具
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
qtbase/src/corelib/global/qendian.h 
qtbase/src/corelib/text/qbytearraymatcher.h 
qtdeclarative/src/qmldebug/qqmlprofilerevent_p.h
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

也可以直接为根目录添加所有文件权限
```bash
chmod -R 777 QtProject
```
### opencv交叉编译
> opencv github网址：[opencv/opencv: Open Source Computer Vision Library (github.com)](https://github.com/opencv/opencv)
可以选择对应版本
![image.png|800](https://cdn.jsdelivr.net/gh/xuezhaorong/Picgo//Source/fix-dir/picgo/picgo-clipboard-images/2024/08/03/13-13-02-dd388a2782a7af3c58fd4ffcc93f90df-20240803131301-495b0d.png)

#### 下载源码
克隆opencv项目
```bash
git clone https://github.com/opencv/opencv.git
```

切换到opencv目录中，创建并切换到build目录
```bash
cd opencv
mkdir build
cd build
```

![image.png|800](https://cdn.jsdelivr.net/gh/xuezhaorong/Picgo//Source/fix-dir/picgo/picgo-clipboard-images/2024/08/03/13-17-56-3286f0b7bdeef5af91c925be5f5020d0-20240803131756-c9057b.png)

#### 编译前准备
* 下载编译工具
```bash
sudo apt install build-essential g++-aarch64-linux-gnu
```
##### 安装**ffmpeg**
使用opencv相关读图/视频等相关功能，需安装ffmpeg
官网链接：http://www.ffmpeg.org/download.html
以3.4.8版本为例
```bash
wget http://www.ffmpeg.org/releases/ffmpeg-3.4.8.tar.gz
tar -zxvf ffmpeg-3.4.8.tar.gz
```
在编译ffmpeg之前需要安装**yasm**汇编编译器
在http://www.tortall.net/projects/yasm/releases下面找到适合自己平台的yasm版本。然后进行安装。
```bash
wget http://www.tortall.net/projects/yasm/releases/yasm-1.3.0.tar.gz
tar -zxvf yasm-1.3.0.tar.gz 
cd yasm-1.3.0
```
配置，编译，安装
```bash
./configure
make -j2 
make install
```
安装yasm后返回 ffmpeg文件夹下执行编译安装，--prefix为安装目录，需要提前创建
```bash
cd ffmpeg-3.4.8/
./configure --enable-shared --prefix=/usr/ffmpeg
```
编译，安装
```bash
make -j2
make install
```
查看版本
```bash
./ffmpeg -version
```
`libavdevice.so.57: cannot open shared object file: No such file or directory`报错解决方案：
1. 加载链接到系统库
切换到系统库目录，创建并编辑`ffmpeg.conf`链接配置文件
```bash
cd /etc/ld.so.conf.d 
sudo touch ffmpeg.conf
sudo vim ffmpeg.conf
```
添加ffmpeg的库文件路径，保存退出
```bash
/usr/ffmpeg/lib
```
执行链接配置
```bash
sudo ldconfig
```
配置环境路径
```bash
sudo vim /etc/profile
```
添加ffmpeg的pkgconfig路径
```bash
export PKG_CONFIG_PATH=$PKG_CONFIG_PATH:/usr/ffmpeg/lib/pkgconfig
```
source系统环境文件使其生效
```bash
source /etc/profile
```

2. 备用方案（第一个方法无效的情况下追加）
将`/usr/ffmpeg/lib/pkgconfig`目录下的pc文件复制到`/usr/local/lib/pkgconfig`目录下

#####  安装libjpeg-turbo
[opencv](https://so.csdn.net/so/search?q=opencv&spm=1001.2101.3001.7020)库中自带了 对JPEG的编解码，其内部实质上是基于第三方库libjpeg进行解码的。但是libjpeg本身的性能并不是很快，特别是在ARM平台下。
libjpeg-turbo是一个使用SIMD技术(MMX、SSE2、AVX2、NEON)进行加速的JPEG编码解码器，能够在基于x86、x86_64、arm的系统上使用。相对于标准版libjpeg能够提供2-6x的性能提升。
地址：https://github.com/libjpeg-turbo/libjpeg-turbo
克隆项目
```bash
git clone https://github.com/libjpeg-turbo/libjpeg-turbo.git
```
切换到项目创建build，配置cmake，CMAKE_INSTALL_PREFIX指定安装目录，自动创建可以不手动创建
```bash
cd libjpeg-turbo
mkdir build
cd build
cmake ../ -D CMAKE_INSTALL_PREFIX=./install ..
```
编译，安装
```bash
make -j2
make install
```

### 编译opencv
切换到opencv目录，新建并切换build目录
```bash
cd opencv
mkdir build
cd build
```
输入配置编译命令，注意`CMAKE_C_COMPILER`和`CMAKE_CXX_COMPILER`为编译工具，`JPEG_INCLUDE_DIR`和`JPEG_LIBRARY`为`ffmpeg`对应的路径，`CMAKE_INSTALL_PREFIX`为安装的路径，`BUILD_SHARED_LIBS`为是否生成动态库，ON为动态库，OFF为静态库。
```bash
cmake -D CMAKE_BUILD_TYPE=RELEASE -D CMAKE_C_COMPILER=aarch64-linux-gnu-gcc -D CMAKE_CXX_COMPILER=aarch64-linux-gnu-g++ -D BUILD_SHARED_LIBS=ON -D CMAKE_CXX_FLAGS=-fPIC -D CMAKE_C_FLAGS=-fPIC -D CMAKE_EXE_LINKER_FLAGS=-lpthread -D ENABLE_PIC=ON -D WITH_1394=OFF -D WITH_ARAVIS=OFF -D WITH_ARITH_DEC=ON -D WITH_ARITH_ENC=ON -D WITH_CLP=OFF -D WITH_CUBLAS=OFF -D WITH_CUDA=OFF -D WITH_CUFFT=OFF -D WITH_FFMPEG=ON -D WITH_GSTREAMER=ON -D WITH_GSTREAMER_0_10=OFF -D WITH_HALIDE=OFF -D WITH_HPX=OFF -D WITH_IMGCODEC_HDR=ON -D WITH_IMGCODEC_PXM=ON -D WITH_IMGCODEC_SUNRASTER=ON -D WITH_INF_ENGINE=OFF -D WITH_IPP=OFF -D WITH_ITT=OFF -D WITH_JASPER=ON -D WITH_JPEG=ON -D BUILD_JPEG=OFF -D JPEG_INCLUDE_DIR=/home/xuezhaorong/libjpeg-turbo/build/install/include -D JPEG_LIBRARY=/home/xuezhaorong/libjpeg-turbo/build/install/lib/libjpeg.a  -D WITH_LAPACK=ON -D WITH_LIBREALSENSE=OFF -D WITH_NVCUVID=OFF -D WITH_OPENCL=OFF -D WITH_OPENCLAMDBLAS=OFF -D WITH_OPENCLAMDFFT=OFF -D WITH_OPENCL_SVM=OFF -D WITH_OPENEXR=OFF -D WITH_OPENGL=OFF -D WITH_OPENMP=OFF -D WITH_OPENNNI=OFF -D WITH_OPENNNI2=OFF -D WITH_OPENVX=OFF -D WITH_PNG=OFF -D WITH_PROTOBUF=OFF -D WITH_PTHREADS_PF=ON -D WITH_PVAPI=OFF -D WITH_QT=OFF -D WITH_QUIRC=OFF  -D WITH_TBB=OFF -D WITH_TIFF=ON -D WITH_VULKAN=OFF -D WITH_WEBP=ON -D WITH_XIMEA=OFF -D CMAKE_INSTALL_PREFIX=/usr/opencv_arm  -D WITH_GTK=OFF -D OPENCV_EXTRA_MODULES_PATH=/home/xuezhaorong/opencv/opencv_contrib/modules ..
```
项目编译链接时无法链接到动态库的解决方法：
手动链接到系统库
```bash
cd /etc/ld.so.conf.d
sudo touch opencv.conf
sudo vim opencv.conf
```
添加lib路径 
```bash
/usr/opencv_arm/lib
```
执行链接配置
```bash
sudo ldconfig
```

#### 在qtcreator中加入opencv
在pro中导入头文件和库文件的路径
```bash
INCLUDEPATH += /usr/opencv_arm/include \
/usr/opencv_arm/include/opencv4 \
/usr/opencv_arm/include/opencv2 \

LIBS += /usr/opencv_arm/lib/libopencv_highgui.so \
/usr/opencv_arm/lib/libopencv_core.so \
/usr/opencv_arm/lib/libopencv_imgproc.so \
/usr/opencv_arm/lib/libopencv_imgcodecs.so \
/usr/opencv_arm/lib/libopencv_video.so \
/usr/opencv_arm/lib/libopencv_videoio.so \
/usr/opencv_arm/lib/libopencv_objdetect.so
```
![image.png](https://cdn.jsdelivr.net/gh/xuezhaorong/Picgo//Source/fix-dir/picgo/picgo-clipboard-images/2024/08/03/14-24-58-1348b584e9ca25a2cc4ee66134cac830-20240803142457-394956.png)

导入头文件
```cpp
#include <opencv2/highgui/highgui.hpp>
#include <opencv2/core/core.hpp>
#include <opencv2/imgproc/imgproc.hpp>
#include <opencv2/objdetect.hpp>
#include <opencv2/videoio.hpp>
#include <opencv2/opencv.hpp>
```


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
add_subdirectory(${PROJECT_SOURCE_DIR}/DiagramModel)
include_directories(${PROJECT_SOURCE_DIR}/DiagramModel/include)
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
