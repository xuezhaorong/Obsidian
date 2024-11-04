## 编译前准备
* 下载编译工具
```bash
sudo apt install build-essential g++-aarch64-linux-gnu
```
### 安装**ffmpeg**
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

###  安装libjpeg-turbo
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
## 下载源码
> opencv github网址：[opencv/opencv: Open Source Computer Vision Library (github.com)](https://github.com/opencv/opencv)
可以选择对应版本
![image.png|800](https://cdn.jsdelivr.net/gh/xuezhaorong/Picgo//Source/fix-dir/https/cdn.jsdelivr.net/gh/xuezhaorong/Picgo/Source/fix-dir/picgo/picgo-clipboard-images/2024/08/03/2024/11/04/15-36-46-dd388a2782a7af3c58fd4ffcc93f90df-13-13-02-dd388a2782a7af3c58fd4ffcc93f90df-20240803131301-495b0d-5285ca.png)

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

![image.png|800](https://cdn.jsdelivr.net/gh/xuezhaorong/Picgo//Source/fix-dir/https/cdn.jsdelivr.net/gh/xuezhaorong/Picgo/Source/fix-dir/picgo/picgo-clipboard-images/2024/08/03/2024/11/04/15-36-46-3286f0b7bdeef5af91c925be5f5020d0-13-17-56-3286f0b7bdeef5af91c925be5f5020d0-20240803131756-c9057b-88cfe3.png)

## 编译opencv
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
