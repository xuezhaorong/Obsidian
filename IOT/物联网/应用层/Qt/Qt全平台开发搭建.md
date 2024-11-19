
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
sudo apt install xcb libwayland-dev libegl1-mesa-dev libgles2-mesa-dev wayland-protocols libxkbcommon-dev libx11-xcb1 libxcb1 libxcb-glx0 libxcb-keysyms1 libxcb-image0 libxcb-shm0 libxcb-icccm4 libxcb-sync1 libxcb-xfixes0 libxcb-shape0 libxcb-render-util0 libxkbcommon-x11-0 libegl1-mesa libxcb-xinerama0-dev 
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
sudo apt install xcb libwayland-dev libegl1-mesa-dev libgles2-mesa-dev wayland-protocols libxkbcommon-dev libx11-xcb1 libxcb1 libxcb-glx0 libxcb-keysyms1 libxcb-image0 libxcb-shm0 libxcb-icccm4 libxcb-sync1 libxcb-xfixes0 libxcb-shape0 libxcb-render-util0 libxkbcommon-x11-0 libegl1-mesa libxcb-xinerama0-dev
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
![image.png|575](https://cdn.jsdelivr.net/gh/xuezhaorong/Picgo//Source/fix-dir/picgo/picgo-clipboard-images/2024/11/19/16-57-54-d30b1013ea8373cd94d03a4b06a707cc-20241119165754-7a6a6f.png)

然后会跳出选择默认语言的界面，再选一次

添加中文字体
```bash
sudo apt install fontconfig
sudo vim /etc/fonts/local.conf

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