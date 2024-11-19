
## 树莓派


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
sudo apt install g++ git cmake
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

