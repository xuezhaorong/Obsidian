> 介绍如何在window和树莓派中编译适合树莓派的qt源码
## window交叉编译
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

官网源码下载地址：[Index of /archive/qt](https://download.qt.io/archive/qt/)

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
cd```
3. 在 `qtbase/src/corelib/global/qglobal.h`的`__cplusplus`中添加`#include <limits>`
4. 在`D:\Qt\5.15.2\Src\qtbase\mkspecs\linux-aarch64-gnu-g++`下，修改`qmake.config`
	![image.png|700](https://cdn.jsdelivr.net/gh/xuezhaorong/Picgo//Source/fix-dir/picgo/picgo-clipboard-images/2024/07/25/09-40-51-120c8452270ee0d1fa7c9d0f72f256eb-20240725094050-86d160.png)
	复制并注释掉原来的工具选项，替换成本地的交叉编译工具包的选项
5. .  可选，向`qtlocation/src/3rdparty/mapbox-gl-native/src/mbgl/util/convert.cpp`中添加，`#include<stdint.h>`，在编译时会跳过这个库，所以可选

#### 开始编译
* 在**Src**目录下新建**build**目录，打开Qt的mingw命令窗口，进入到**build**中
![image.png|429](https://cdn.jsdelivr.net/gh/xuezhaorong/Picgo//Source/fix-dir/picgo/picgo-clipboard-images/2024/07/25/09-10-19-9f6a193ae95b07790fb65ff7700a7889-20240725091017-9dbcb0.png)

![image.png|550](https://cdn.jsdelivr.net/gh/xuezhaorong/Picgo//Source/fix-dir/picgo/picgo-clipboard-images/2024/07/25/09-11-19-bc8a85cc64c46774225503c6ba0d2b39-20240725091118-08aef7.png)

* 输入指令开始配置编译，`D:\Qt\5.15.2\Src\`替换成Src的绝对路径，`D:\Program\Qt\arm_qt`为编译后的qt库的目录，要先创建一个
```bash
..\configure.bat -release -opensource -confirm-license -prefix D:\Program\Qt\arm_qt -nomake tests -nomake examples -no-opengl -skip qtvirtualkeyboard -skip qt3d -skip qtquick3d -skip qttools -skip qtscript -skip qtlocation -xplatform linux-aarch64-gnu-g++
```
	配置选项说明（具体可以在命令行内输入“configure -help”命令查看详细说明）：
		1. -release: 编译release版本
		2.  -opensource: 表示开源许可
		3. -prefix: Qt安装路径
		4. -nomake: 表示不编译后面参数指定的模块
		5. -no-opengl: 表示不安装OpenGL
		6. -skip: 表示不安装的qt工具包
		7. -xplatform 表示使用源码路径qtbase\mkspecs\linux-aarch64-gnu-g++文件夹内的配置，编译时会自动去该路径下找到配置文件进行编译
	
* 等待配置完成，中途要选择y接受授权，没有**Error**可以开始编译，-j4可以根据电脑配置调整对应的线程数量
```bash
mingw32-make -j4
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

 ### Clion配置
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

## WSL交叉编译

在树莓派中编译带QWebEnign的qt版本不够资源，需要在WSL中进行编译
使用的是Debian12
对Debian12进行换源，清华园镜像链接：[debian | 镜像站使用帮助 | 清华大学开源软件镜像站 | Tsinghua Open Source Mirror](https://mirrors.tuna.tsinghua.edu.cn/help/debian/)
先安装https支持：
```bash
sudo apt update
sudo apt install apt-transport-https ca-certificates
```
使用传统格式换源
```bash
sudo nano /etc/apt/sources.list
```
注意版本：
```bash
# 默认注释了源码镜像以提高 apt update 速度，如有需要可自行取消注释
deb https://mirrors.tuna.tsinghua.edu.cn/debian/ bookworm main contrib non-free non-free-firmware
# deb-src https://mirrors.tuna.tsinghua.edu.cn/debian/ bookworm main contrib non-free non-free-firmware

deb https://mirrors.tuna.tsinghua.edu.cn/debian/ bookworm-updates main contrib non-free non-free-firmware
# deb-src https://mirrors.tuna.tsinghua.edu.cn/debian/ bookworm-updates main contrib non-free non-free-firmware

deb https://mirrors.tuna.tsinghua.edu.cn/debian/ bookworm-backports main contrib non-free non-free-firmware
# deb-src https://mirrors.tuna.tsinghua.edu.cn/debian/ bookworm-backports main contrib non-free non-free-firmware

# 以下安全更新软件源包含了官方源与镜像站配置，如有需要可自行修改注释切换
deb https://security.debian.org/debian-security bookworm-security main contrib non-free non-free-firmware
# deb-src https://security.debian.org/debian-security bookworm-security main contrib non-free non-free-firmware
```

```bash
sudo apt update
sudo apt upgrade
```
### 准备

安装编译工具：
```bash
sudo apt install g++-aarch64-linux-gnu g++ cmake gperf bison flex nodejs pkg-config zlib1g zlib1g-dev python3
``` 

编译Python2，源码链接：https://www.123684.com/s/zum7Vv-clTnH
切换到源码目录中，进行编译：
```bash
./configure --prefix=/usr/local/share/python2.7 --with-zlib --enable-optimizations 
make
sudo make install
```

加入到系统环境：
```bash
sudo ln -s /usr/local/share/python2.7/bin/python2 /usr/bin/python2
```

查看版本 ：
```bash
python2 --version
```



### 挂载树莓派sysroot
下载sshfs
```bash
sudo apt install sshfs
```

挂载树莓派，必须在同一局域网下，注意替换用户名和ip地址
```bash
sudo mkidr /mnt/pi-rootfs
sudo sshfs xuezhaorong@192.168.1.241:/ /mnt/pi-rootfs -o allow_other
```

![image.png|775](https://cdn.jsdelivr.net/gh/xuezhaorong/Picgo//Source/fix-dir/picgo/picgo-clipboard-images/2024/11/17/20-20-09-e4e036e989a730bb99e115a7e56e4eac-20241117202007-489451.png)
输入yes确认连接，然后输入树莓派的密码

```bash
ls /mnt/pi-rootfs/
```
![image.png](https://cdn.jsdelivr.net/gh/xuezhaorong/Picgo//Source/fix-dir/picgo/picgo-clipboard-images/2024/11/17/20-20-59-af02eb86f93c77c723173434291cc52a-20241117202059-6fe567.png)
可以看到对应的目录

### 树莓派设置
安装依赖包
按照[[Qt编译#aarch64（树莓派4b为例）编译#准备|树莓派依赖包安装]]

### 开始编译
```bash
../configure -release -opensource -confirm-license -prefix /home/xuezhaorong/Software/Qt -hostprefix /home/xuezhaorong/Software/Qt -xplatform linux-aarch64-gnu-g++ -sysroot /mnt/pi-rootfs -verbose -nomake tests -nomake examples -no-opengl -skip qtvirtualkeyboard -skip qt3d -skip qtquick3d -skip qttools -skip qtscript -skip qtlocation
```

## aarch64（树莓派4b为例）编译

### 准备
安装依赖包：
```bash
sudo apt-get install bison flex gperf python3 nodejs libnss3-dev libdbus-1-dev libfontconfig1-dev libgles2-mesa libgles2-mesa-dev xorg-dev  libxcb1 libxcb-util1 libx11-xcb1 libxkbcommon-x11-0 libxcb-keysyms1 libxcb-image0 libxcb-randr0 libxcb-render-util0 libxcb-shape0 libxcb-shm0 libxcb-sync1 libxcb-xfixes0 libxcb-icccm4 libxcb-xinerama0 libxcb-cursor0

```


编译Python2，源码链接：https://www.123684.com/s/zum7Vv-clTnH
切换到源码目录中，进行编译：
```bash
./configure --prefix=/usr/local/share/python2.7
make
sudo make install
```

加入到系统环境：
```bash
sudo ln -s /usr/local/share/python2.7/bin/python2 /usr/bin/python2
```

查看版本 ：
```bash
python2 -V
```


### 文件修改
根据[[IOT/物联网/应用层/Qt/Qt编译#window交叉编译#Window配置#文件修改|文件修改]]，修改源码文件

### 开始编译

新建`build`目录，切换
```bash
mkdir build
cd build
```



输入配置指令：
```bash
../configure -release -opensource -confirm-license -prefix /home/xuezhaorong/Software/Qt -nomake tests -nomake examples -no-opengl -skip qtvirtualkeyboard -skip qt3d -skip qtquick3d -skip qttools -skip qtscript -skip qtlocation -skip qtwayland -skip qtwebengine
```
编译QtwebEnign可能会导致资源不足而崩溃，可以选择跳过
其中，`-prefix`参数是安装的路径。

编译，安装：
```bash
make 
make install
```






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


在qtcreator中加入opencv
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
![image.png|975](https://cdn.jsdelivr.net/gh/xuezhaorong/Picgo//Source/fix-dir/picgo/picgo-clipboard-images/2024/08/03/14-24-58-1348b584e9ca25a2cc4ee66134cac830-20240803142457-394956.png)

导入头文件
```cpp
#include <opencv2/highgui/highgui.hpp>
#include <opencv2/core/core.hpp>
#include <opencv2/imgproc/imgproc.hpp>
#include <opencv2/objdetect.hpp>
#include <opencv2/videoio.hpp>
#include <opencv2/opencv.hpp>
```

