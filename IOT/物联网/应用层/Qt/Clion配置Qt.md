>模板链接：https://wwl.lanzn.com/iEtTA23xnchg

## 安装
*  下载并安装Qt Creator，下载链接：https://download.qt.io/archive/online_installers/ ，
*  旧版本离线安装包链接：[Index of /new_archive/qt](https://download.qt.io/new_archive/qt/)(5.15之前)
![image.png|412](https://cdn.jsdelivr.net/gh/xuezhaorong/Picgo//Source/fix-dir/picgo/picgo-clipboard-images/2024/07/14/21-44-29-29434fa041dedbce19306b57e76874b2-20240714214428-207ecd.png)
选择Qt版本，以及Mingw编译器
![image.png|635](https://cdn.jsdelivr.net/gh/xuezhaorong/Picgo//Source/fix-dir/picgo/picgo-clipboard-images/2024/07/20/17-53-03-5435fc01ca40be1de861396f19d02d06-20240720175302-ad8d9a.png)
勾选Tools中的对应版本的mingw编译器
![image.png|635](https://cdn.jsdelivr.net/gh/xuezhaorong/Picgo//Source/fix-dir/picgo/picgo-clipboard-images/2024/07/20/17-53-43-e0688618b2f93acda8030e53f3aa76cc-20240720175343-2803f2.png)
在线安装若没有旧版本，需要选择右边的Archive，再点击筛选，这时就会出现之前的Qt版本。
![image.png|700](https://cdn.jsdelivr.net/gh/xuezhaorong/Picgo//Source/fix-dir/picgo/picgo-clipboard-images/2024/07/21/13-13-59-e34515fd70441a20f0958d2c1a023c62-20240721131358-e60632.png)
必须勾选的项目：
Design Studio
![image.png|376](https://cdn.jsdelivr.net/gh/xuezhaorong/Picgo//Source/fix-dir/picgo/picgo-clipboard-images/2024/07/21/13-31-58-66dc57685fa0d0cd24865fd399ddba11-20240721133158-7bcd67.png)
MinGw 编译器
![image.png|376](https://cdn.jsdelivr.net/gh/xuezhaorong/Picgo//Source/fix-dir/picgo/picgo-clipboard-images/2024/07/21/13-32-41-44782659ce937d45852e166f2ed12e72-20240721133241-ae5508.png)

![image.png|377](https://cdn.jsdelivr.net/gh/xuezhaorong/Picgo//Source/fix-dir/picgo/picgo-clipboard-images/2024/07/21/13-33-21-3912408593fbed6cda82c82dbc66dee2-20240721133320-56d191.png)
 ### 加速
 如果下载过慢可以使用国内镜像进行下载，需要下载4.0以上的Qt在线安装软件
 ![image.png|875](https://cdn.jsdelivr.net/gh/xuezhaorong/Picgo//Source/fix-dir/picgo/picgo-clipboard-images/2024/07/21/17-44-27-21447ab4bef3be25a7a1deef5b0b2cbf-20240721174426-1c3f92.png)
首先需要清除缓存,然后退出安装器，在其目录下打开cmd命令窗口，输入命令：
```bash
.\qt-unified-windows-x64-4.6.1-online.exe --mirror https://mirrors.cloud.tencent.com/qt/
```
其中安装器的名字需要换成自己的安装器版本。

## 配置
1. 添加系统环境变量
![|450](https://cdn.jsdelivr.net/gh/xuezhaorong/Picgo//Source/fix-dir/picgo/picgo-clipboard-images/2024/07/08/14-29-25-54ea32c9466723c7369e07d5c5e5e7ed-20240708142924-ddde96.png)
3. 创建Qt项目，Cmake选择mingw编译器的根目录
![image.png|900](https://cdn.jsdelivr.net/gh/xuezhaorong/Picgo//Source/fix-dir/picgo/picgo-clipboard-images/2024/07/08/14-47-52-61858f56ebf189b16d98f35bad90b05e-20240708144752-c5c7c6.png)
4. 设置CLion工具链，在设置->构建、执行、部署->工具链选项中创建`MinGW` 工具链，注意工具链，构建工具，C 编译器，C++编译器的选择路径。
![image.png|925](https://cdn.jsdelivr.net/gh/xuezhaorong/Picgo//Source/fix-dir/picgo/picgo-clipboard-images/2024/07/08/14-50-12-14586e1badef4ddbd1c1d4bfae4d1687-20240708145011-02e3e1.png)
5. Cmake选择刚刚创建的工具链
![image.png|925](https://cdn.jsdelivr.net/gh/xuezhaorong/Picgo//Source/fix-dir/picgo/picgo-clipboard-images/2024/07/08/14-51-44-fd41370da7a306fc836b47aa3ba0ea8a-20240708145144-31bfb6.png)
6. 配置外部工具
![|925](https://cdn.jsdelivr.net/gh/xuezhaorong/Picgo//Source/fix-dir/picgo/picgo-clipboard-images/2024/07/08/17-21-10-fc5fa9ef6930b4373ccad4dd012f0f31-20240708172109-cdbe9c.png)
QtDesigned的配置
![image.png|925](https://cdn.jsdelivr.net/gh/xuezhaorong/Picgo//Source/fix-dir/picgo/picgo-clipboard-images/2024/07/08/17-23-34-ab160750c8ca2d3165ea679913d9309e-20240708172333-98eb48.png)
程序选择designer.exe，其他参数修改成上图相同
```
实参：$FileName$ 工作目录：$FileDir$
```

![image.png|925](https://cdn.jsdelivr.net/gh/xuezhaorong/Picgo//Source/fix-dir/picgo/picgo-clipboard-images/2024/07/08/17-22-40-501b60ac6c0c1f7b1a5106fcb791a3d8-20240708172240-77abb3.png)
QtUIC配置
![image.png|925](https://cdn.jsdelivr.net/gh/xuezhaorong/Picgo//Source/fix-dir/picgo/picgo-clipboard-images/2024/07/08/17-26-32-0bda44009abf8f7060bdd50b7aec9156-20240708172631-d70bc8.png)
程序选择uic.exe，其他参数改成上图相同
```
实参：$FileName$ -o ui_$FileNameWithoutExtension$.h 工作目录：$FileDir$
```
![image.png|925](https://cdn.jsdelivr.net/gh/xuezhaorong/Picgo//Source/fix-dir/picgo/picgo-clipboard-images/2024/07/08/17-27-05-0a94c8c527ac04138651b183c7626d58-20240708172704-c41c6e.png)
添加 Qt UI类之后 找到外部工具点击UIC进行编译 每次修改了`.ui`文件 都要在外部工具里面点击UIC进行编译
![image.png|925](https://cdn.jsdelivr.net/gh/xuezhaorong/Picgo//Source/fix-dir/picgo/picgo-clipboard-images/2024/07/08/17-29-22-639614bf7e3f4a87a29f7d6df2f9db99-20240708172922-b5a5cc.png)

## 问题解决
1. **对于使用Qt Designer时无法直接拖拽控件**
打开系统设置->文件代码模板->QtDesignner Form，将下列代码添加进去
```
#if( 'QMainWindow' == ${PARENT_CLASS} )
<widget class="QWidget" name="centralWidget"/>
#end
```
![image.png|925](https://cdn.jsdelivr.net/gh/xuezhaorong/Picgo//Source/fix-dir/picgo/picgo-clipboard-images/2024/07/08/19-25-42-6446bb23edaf823eb335733af8d06161-20240708192542-5087a4.png)
2. qDebug()不输出问题
打开运行->编辑配置，添加环境参数`QT_ASSUME_STDERR_HAS_CONSOLE=1`保存后重新运行
![image.png|925](https://cdn.jsdelivr.net/gh/xuezhaorong/Picgo//Source/fix-dir/picgo/picgo-clipboard-images/2024/07/08/19-32-07-6d7d3840fcc2ff3fbde045f49106b51c-20240708193207-987bf2.png)
![image.png|925](https://cdn.jsdelivr.net/gh/xuezhaorong/Picgo//Source/fix-dir/picgo/picgo-clipboard-images/2024/07/08/19-33-15-d5ebc0fcb49b1eda9e0085f21adb0ec1-20240708193314-1a8f17.png)
## 重构目录
 新增`src`目录存储`.cpp` `include`目录存储`.h`,`resource`目录存储资源,`form`目录存储`ui`文件
![image.png|925](https://cdn.jsdelivr.net/gh/xuezhaorong/Picgo//Source/fix-dir/picgo/picgo-clipboard-images/2024/07/08/22-41-42-9b290a7b96041a087fb7c5179987ca57-20240708224141-d9b66d.png)

新建QtUI类分别移动到上面的目录下
![image.png|1200](https://cdn.jsdelivr.net/gh/xuezhaorong/Picgo//Source/fix-dir/picgo/picgo-clipboard-images/2024/07/08/22-42-33-95f6223b5674dc97db36034cafb0c407-20240708224233-067c45.png)

更改图片上的内容,然后重新cmake
```cmake
set(CMAKE_AUTOUIC_SEARCH_PATHS ${PROJECT_SOURCE_DIR}/form)  
  

  
include_directories(${PROJECT_SOURCE_DIR}/include)  
  
FILE(GLOB_RECURSE RC_FILES ${PROJECT_SOURCE_DIR}/resource/*.rc)  
  
FILE(GLOB_RECURSE QRC_FILES ${PROJECT_SOURCE_DIR}/resource/*.qrc)  
  
FILE(GLOB_RECURSE HEADER_FILES ${PROJECT_SOURCE_DIR}/include/*.h)  
  
FILE(GLOB_RECURSE UI_FILES ${PROJECT_SOURCE_DIR}/form/*.ui)  
  
aux_source_directory(${PROJECT_SOURCE_DIR}/src SOURCE_FILES)

add_executable(Project main.cpp ${SOURCE_FILES} ${UI_FILES} ${RC_FILES} ${QRC_FILES} ${HEADER_FILES})
```
![image.png|1150](https://cdn.jsdelivr.net/gh/xuezhaorong/Picgo//Source/fix-dir/picgo/picgo-clipboard-images/2024/07/08/22-45-19-0a77d9f7afc59415c5ec1436e481518a-20240708224519-22ffcf.png)
![image.png|600](https://cdn.jsdelivr.net/gh/xuezhaorong/Picgo//Source/fix-dir/picgo/picgo-clipboard-images/2024/07/08/22-46-33-ff18fa7f40f629bbaa4888b2b2f0704f-20240708224633-2cc301.png)
4. 移植问题
为了方便移植到Qt Create后能够关联掉.ui文件，需要在代码模板中.cpp的.ui头文件导入改到.h中

![|900](https://cdn.jsdelivr.net/gh/xuezhaorong/Picgo//Source/fix-dir/https/cdn.jsdelivr.net/gh/xuezhaorong/Picgo/Source/fix-dir/picgo/picgo-clipboard-images/2024/07/14/2024/07/14/22-04-23-9f1633448b92f93ffb0be3893ef5ccde-22-03-44-9f1633448b92f93ffb0be3893ef5ccde-20240714220344-f1ba15-fad5d8.png)
删掉Qt Class中的`#[[#include]]# "${UI_HEADER_FILENAME}"`

![image.png|925](https://cdn.jsdelivr.net/gh/xuezhaorong/Picgo//Source/fix-dir/picgo/picgo-clipboard-images/2024/07/14/22-05-25-bfd8645a0038bd513d8515f8f634fd2a-20240714220525-b52dbe.png)
在Header中添加
![image.png|950](https://cdn.jsdelivr.net/gh/xuezhaorong/Picgo//Source/fix-dir/picgo/picgo-clipboard-images/2024/07/15/22-11-01-3f8c4fa8846c784aac892ab7e2fc4bcc-20240715221100-30b1f0.png)
更改Header的这处，将nullptr改成NULL，删去override;

## 添加外部库
![image.png|1175](https://cdn.jsdelivr.net/gh/xuezhaorong/Picgo//Source/fix-dir/picgo/picgo-clipboard-images/2024/07/15/22-36-18-412355e749ab35d08a486f2f750cad08-20240715223617-daa453.png)
在Cmake文件中的find_package和target_link_libraries中添加库，重新cmake，库的命名规则为首字母大写。

##  添加资源
![image.png|875](https://cdn.jsdelivr.net/gh/xuezhaorong/Picgo//Source/fix-dir/picgo/picgo-clipboard-images/2024/07/15/23-00-23-7e708bdd33a4ea7a1cc73fea4aaf86c8-20240715230022-ac64c2.png)
在resource中创建存放图片的目录和qrc文件
![image.png|1125](https://cdn.jsdelivr.net/gh/xuezhaorong/Picgo//Source/fix-dir/picgo/picgo-clipboard-images/2024/07/15/23-00-55-8a1662511d9f7a894578d1f581c54957-20240715230055-7f8008.png)
qrc中填写prefix虚拟路径，和file实际路径（以resource为根目录），代码中引用规则为`:虚拟文件夹名/文件路径`
```c++
palette.setBrush(QPalette::Background,QBrush(QPixmap(":/images/pic/menuUi.jpg")));
```

在Designer中添加资源时不会自动识别，需要手动添加一下
![image.png|875](https://cdn.jsdelivr.net/gh/xuezhaorong/Picgo//Source/fix-dir/picgo/picgo-clipboard-images/2024/07/26/16-14-31-6eec79db4a384d2155a9494d971b6272-20240726161430-534ac3.png)

## 移植
将Clion中的Qt项目移植到Qt Create中
1.  Qt Create新建项目
2.  复制Clion Qt中form文件夹下的.ui文件（.h文件不用复制），include文件夹下的.h文件，src文件夹下的.cpp文件到Qt Create的项目目录中
3. 在Qt Create中添加现存文件，把每个类的.cpp，.h，.ui文件添加进去，**并且将.h文件中的ui引入文件更改**，编译。
运行成功，移植完成