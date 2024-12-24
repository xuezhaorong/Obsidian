
## 树莓派
使用的是2024最新的树莓派64位系统
### 准备
开启ssh[[开启ssh]]，并连接

换源[[换源]]

### 安装工具包
```bash 
sudo apt install cmake 
```

安装xcb支持
```bash
sudo apt install xcb libwayland-dev libegl1-mesa-dev libgles2-mesa-dev wayland-protocols libxkbcommon-dev libx11-xcb1 libxcb1 libxcb-glx0 libxcb-keysyms1 libxcb-image0 libxcb-shm0 libxcb-icccm4 libxcb-sync1 libxcb-xfixes0 libxcb-shape0 libxcb-render-util0 libxkbcommon-x11-0 libegl1-mesa libxcb-xinerama0-dev libx11-dev libx11-xcb-dev libxext-dev libxfixes-dev libxi-dev libxrender-dev libxcb1-dev libxcb-glx0-dev libxcb-keysyms1-dev libxcb-image0-dev libxcb-shm0-dev libxcb-icccm4-dev libxcb-sync0-dev libxcb-xfixes0-dev libxcb-shape0-dev libxcb-randr0-dev libxcb-render-util0-dev libxcb-xinerama0-dev libxkbcommon-dev libxkbcommon-x11-dev libdbus-1-dev libatspi2.0-dev
```
### 安装QT
[[安装qt5]]



## WSL
### 准备
使用的Debian12系统，安装配置过程[[WSL安装配置]]

然后进行换源：
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

### 安装工具包 
```bash
sudo apt install g++ git cmake sshfs pkg-config
```

安装xcb支持
```bash
sudo apt install xcb libwayland-dev libegl1-mesa-dev libgles2-mesa-dev wayland-protocols libxkbcommon-dev libx11-xcb1 libxcb1 libxcb-glx0 libxcb-keysyms1 libxcb-image0 libxcb-shm0 libxcb-icccm4 libxcb-sync1 libxcb-xfixes0 libxcb-shape0 libxcb-render-util0 libxkbcommon-x11-0 libegl1-mesa libxcb-xinerama0-dev libx11-dev libx11-xcb-dev libxext-dev libxfixes-dev libxi-dev libxrender-dev libxcb1-dev libxcb-glx0-dev libxcb-keysyms1-dev libxcb-image0-dev libxcb-shm0-dev libxcb-icccm4-dev libxcb-sync0-dev libxcb-xfixes0-dev libxcb-shape0-dev libxcb-randr0-dev libxcb-render-util0-dev libxcb-xinerama0-dev libxkbcommon-dev libxkbcommon-x11-dev libdbus-1-dev libatspi2.0-dev
```

### 安装QT
```bash
sudo apt install qtbase5-dev qtchooser qt5-qmake qtbase5-dev-tools qtmultimedia5-dev libqt5serialport5-dev
sudo apt-get install qtcreator
```

### 安装中文包
打开语言配置
```bash
sudo apt install locales
sudo dpkg-reconfigure locales
```

选择zh_CH.UTF-8 UTF-8，按空格选择
![image.png|575](https://cdn.jsdelivr.net/gh/xuezhaorong/Picgo//Source/fix-dir/https/cdn.jsdelivr.net/gh/xuezhaorong/Picgo/Source/fix-dir/picgo/picgo-clipboard-images/2024/11/19/2024/11/20/18-49-27-d30b1013ea8373cd94d03a4b06a707cc-16-57-54-d30b1013ea8373cd94d03a4b06a707cc-20241119165754-7a6a6f-f41b6c.png)

然后会跳出选择默认语言的界面，再选一次

添加中文字体
```bash
sudo apt install fontconfig
sudo nano /etc/fonts/local.conf

```

写入：
```bash
<?xml version="1.0"?>
<!DOCTYPE fontconfig SYSTEM "fonts.dtd">
<fontconfig>
    <dir>/mnt/c/Windows/Fonts</dir>
</fontconfig>
```

### 链接树莓派
查看树莓派ip
```bash
hostname -i
```

新建挂载目录
```bash
sudo mkdir /mnt/pi-rootfs
```

打开`~/.bashrc`，输入sshfs网络挂载命令和`PKGCONFIG`路径
```bash
sudo sshfs xuezhaorong@192.168.1.241:/ /mnt/pi-rootfs -o allow_other
sudo sshfs xuezhaorong@192.168.1.241:/ /mnt/pi-rootfs -o allow_other

PKG_CONFIG_PATH=$PKG_CONFIG_PATH:/mnt/pi-rootfs/usr/lib/aarch64-linux-gnu/pkgconfig:/mnt/pi-rootfs/usr/lib/pkgconfig
```

### 交叉编译Qt
根据[[Qt编译]]中选择对应Qt版本的源码，进行交叉编译
配置命令：
```bash
../configure -release -opensource -confirm-license -prefix /home/xuezhaorong/Software/Qt -hostprefix /home/xuezhaorong/Software/Qt -xplatform linux-aarch64-gnu-g++ -sysroot /mnt/pi-rootfs -verbose -nomake tests -nomake examples -opengl es2 -skip qtvirtualkeyboard -skip qt3d -skip qtquick3d -skip qttools -skip qtscript -skip qtlocation -skip qtwebengine
```

`-prefix`为目标平台树莓派的安装路径，`-hostprefix`为构建平台的安装路径

编译安装：
```bash
gamke -j4
gmake install
```

为Qt添加系统字体
```bash
sudo cp -r /mnt/c/Windows/Fonts/* /mnt/pi-rootfs/Software/Qt/lib/fonts/
```

### 创建项目
设置用户运行时目录的标准路径`XDG_RUNTIME_DIR`

```bash
sudo nano ~/.bashrc
```

写入命令：
```bash
export XDG_RUNTIME_DIR=/tmp/runtime-$(id -u) 
mkdir -p $XDG_RUNTIME_DIR 
chmod 700 $XDG_RUNTIME_DIR

```

运行`qtcreator`
```bash
qtcreator
```

创建一个项目，查看构建套件即可看到自动关联了安装的Qt版本。
![image.png|775](https://cdn.jsdelivr.net/gh/xuezhaorong/Picgo//Source/fix-dir/picgo/picgo-clipboard-images/2024/11/19/18-56-17-b754e876c9b0cbcb5700cac124f8ffda-20241119185616-5548f8.png)

新建Qt版本，选择交叉编译下的Qt版本
![image.png|1300](https://cdn.jsdelivr.net/gh/xuezhaorong/Picgo//Source/fix-dir/picgo/picgo-clipboard-images/2024/11/20/10-42-26-dfa480599271711924927bcdb3002bc7-20241120104225-feeddf.png)

添加编译套件，注意选择编译器和Qt版本

![image.png|1300](https://cdn.jsdelivr.net/gh/xuezhaorong/Picgo//Source/fix-dir/picgo/picgo-clipboard-images/2024/11/20/10-43-11-94fe5ea8087e7699c3e3a6db3855c359-20241120104310-d0bc03.png)

在项目中添加构建套件，构建目录可以选择挂载的路径，但是会影响编译速度
![image.png|1300](https://cdn.jsdelivr.net/gh/xuezhaorong/Picgo//Source/fix-dir/picgo/picgo-clipboard-images/2024/11/20/10-46-37-2cb72e6c66f2597b8990a9bdc8282307-20241120104636-78d642.png)

## 远程开发

首先需要保证树莓派和电脑在同一局域网下

### 远程连接配置
1. 进入`vscode`中下载`Remote - SSH`插件
![image.png|436](https://cdn.jsdelivr.net/gh/xuezhaorong/Picgo//Source/fix-dir/picgo/picgo-clipboard-images/2024/12/24/11-28-22-a246242fe2b6f3728d8335ab802efe53-20241224112822-7e66a1.png)

2. 点击远程资源管理器
![image.png|57](https://cdn.jsdelivr.net/gh/xuezhaorong/Picgo//Source/fix-dir/picgo/picgo-clipboard-images/2024/12/24/11-29-08-7ba618ebb08676473b666c938628564e-20241224112907-094a9f.png)

3. 新建连接，输入指令，格式为：`ssh username@ip`
![image.png|400](https://cdn.jsdelivr.net/gh/xuezhaorong/Picgo//Source/fix-dir/picgo/picgo-clipboard-images/2024/12/24/11-30-12-19aa289b78f92762d3e18aee1fed5cd9-20241224113012-bb72c4.png)
随意选择一个配置文件，进行保存配置信息，并按照指示输入密码

4. 免密码认证


### Qt插件配置
1. 连接到树莓派中，然后在本地和树莓派中安装`Qt Configure`插件
![image.png|525](https://cdn.jsdelivr.net/gh/xuezhaorong/Picgo//Source/fix-dir/picgo/picgo-clipboard-images/2024/12/24/11-33-14-b91761350d745e562a53a5ea0ac67d91-20241224113314-d9dae3.png)


2. 进行配置，点击设置
![|400](https://cdn.jsdelivr.net/gh/xuezhaorong/Picgo//Source/fix-dir/picgo/picgo-clipboard-images/2024/12/24/11-36-24-88f73059db0e7cb2932f5d9aa3bb4338-20241224113623-1a2a80.png)
在远程栏中修改`Qt Dir`和`Qt kit Dir`
![image.png|750](https://cdn.jsdelivr.net/gh/xuezhaorong/Picgo//Source/fix-dir/picgo/picgo-clipboard-images/2024/12/24/11-38-37-4f2f9cfc1b25b9c2af054d72ac9da943-20241224113836-89e434.png)

### 创建和运行项目
1.  在命令面板中选择`QtConfigure:New Project`来创建一个项目，输入项目名称，编译套件和路径后自动打开项目
![image.png](https://cdn.jsdelivr.net/gh/xuezhaorong/Picgo//Source/fix-dir/picgo/picgo-clipboard-images/2024/12/24/11-39-50-94800bf3f244c2ff9fea11e5edcbc99b-20241224113949-f0aeee.png)

2. 在`.vscode`下的`tasks.json`中添加一个编译而不运行的任务
```json
{

      "type": "shell",

      "label": "build-debug",

      "command": "cd build/debug && /usr/bin/make",

      "args": [],

      "options": {},

      "problemMatcher": [],

      "group": "build",

      "dependsOn": [

        "qmake build debug"

      ]

    },
```
![image.png|1050](https://cdn.jsdelivr.net/gh/xuezhaorong/Picgo//Source/fix-dir/picgo/picgo-clipboard-images/2024/12/24/11-43-46-0715e0ac72031791a0a8b4a14e344e41-20241224114345-ac90ab.png)

3. 快捷键`ctrl+shift+B`运行项目，点击`build-debug`任务进行编译，然后在树莓派该项目目录下的build中生成可执行文件
![image.png|825](https://cdn.jsdelivr.net/gh/xuezhaorong/Picgo//Source/fix-dir/picgo/picgo-clipboard-images/2024/12/24/11-45-07-e5102d816b95404ee14e42deac926f6f-20241224114507-f5b2d2.png)

## 运行
将编译的二进制可执行文件传到树莓派中

需要为传入的可执行文件添加权限
![image.png|652](https://cdn.jsdelivr.net/gh/xuezhaorong/Picgo//Source/fix-dir/picgo/picgo-clipboard-images/2024/07/25/10-28-22-bf67a7a2673197bc1790494e31319d66-20240725102822-372761.png)

然后执行可执行文件
![image.png|632](https://cdn.jsdelivr.net/gh/xuezhaorong/Picgo//Source/fix-dir/picgo/picgo-clipboard-images/2024/07/25/10-29-01-21c859956a7fcfef7ce501dae8c89a96-20240725102900-186f67.png)

也可以直接为根目录添加所有文件权限
```bash
chmod -R 777 QtProject
```

如果将编译目录设为挂载路径时，则自动给予权限
![image.png|900](https://cdn.jsdelivr.net/gh/xuezhaorong/Picgo//Source/fix-dir/picgo/picgo-clipboard-images/2024/11/20/15-35-06-e261ddd4550b0e5eaed32d17e8cac223-20241120153505-ee968a.png)

## 加入外部库
现在加入QWebEngine库，进行测试外部库的导入，下载链接：https://wwet.lanzn.com/i3MO42fmexeb
选择对应的架构解压文件，WSL为amd64，树莓派为arm64

![image.png|725](https://cdn.jsdelivr.net/gh/xuezhaorong/Picgo//Source/fix-dir/picgo/picgo-clipboard-images/2024/11/20/15-44-48-ddb8d4cbba5cb17765b243b32aed2559-20241120154448-72f7e1.png)

按顺序安装，注意：**Qt版本需要在5.13以上**
```bash
sudo dpkg -i libqt5webenginecore5_5.15.13+dfsg-1~deb12u1_arm64.deb
sudo dpkg -i libqt5webengine5_5.15.13+dfsg-1~deb12u1_arm64.deb
sudo dpkg -i libqt5webenginewidgets5_5.15.13+dfsg-1~deb12u1_arm64.deb
sudo dpkg -i qtwebengine5-dev_5.15.13+dfsg-1~deb12u1_arm64.deb
sudo dpkg -i qtwebengine5-dev-tools_5.15.13+dfsg-1~deb12u1_arm64.deb
```

在安装过程中出错则使用指令`sudo apt -f install` 修复依赖包关系，然后再次执行当前的安装指令。
因为安装时检测的是默认的Qt路径，所以需要将库文件移动到编译的Qt路径下，注意路径的替换
```bash
sudo cp -r /usr/include/aarch64-linux-gnu/qt5/QtWebEngine* /home/xuezhaorong/Software/Qt/include/
sudo cp -r /usr/include/aarch64-linux-gnu/qt5/QtPositioning* /home/xuezhaorong/Software/Qt/include/
sudo cp -r /usr/lib/aarch64-linux-gnu/libQt5WebEngine* /home/xuezhaorong/Software/Qt/lib/
sudo cp -r /usr/lib/aarch64-linux-gnu/libQt5Positioning* /home/xuezhaorong/Software/Qt/lib/
```


在qtcreator中导入，理论上，只用导入WSL或网络挂载的任意一个即可，移动库文件的操作也是一样，具体方法参照[[QCefView#QtCreator导入QCefView|库导入介绍]]，库和头文件路径选择具体的路径，需要导入`-lQt5WebEngine`，`-lQt5WebEngineCore`和`-lQt5WebEngineWidgets` 这三个库

```bash
unix:!macx: LIBS += -L$$PWD/../../../mnt/pi-rootfs/home/xuezhaorong/Software/Qt/lib/ -lQt5WebEngine

INCLUDEPATH += $$PWD/../../../mnt/pi-rootfs/home/xuezhaorong/Software/Qt/include/QtWebEngine
DEPENDPATH += $$PWD/../../../mnt/pi-rootfs/home/xuezhaorong/Software/Qt/include/QtWebEngine



unix:!macx: LIBS += -L$$PWD/../../../mnt/pi-rootfs/home/xuezhaorong/Software/Qt/lib/ -lQt5WebEngineCore

INCLUDEPATH += $$PWD/../../../mnt/pi-rootfs/home/xuezhaorong/Software/Qt/include/QtWebEngineCore
DEPENDPATH += $$PWD/../../../mnt/pi-rootfs/home/xuezhaorong/Software/Qt/include/QtWebEngineCore

unix:!macx: LIBS += -L$$PWD/../../../mnt/pi-rootfs/home/xuezhaorong/Software/Qt/lib/ -lQt5WebEngineWidgets

INCLUDEPATH += $$PWD/../../../mnt/pi-rootfs/home/xuezhaorong/Software/Qt/include/QtWebEngineWidgets
DEPENDPATH += $$PWD/../../../mnt/pi-rootfs/home/xuezhaorong/Software/Qt/include/QtWebEngineWidgets
```

导入库完毕