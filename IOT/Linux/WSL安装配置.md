## 安装WSL
**启用适用于 Linux 的 Windows 子系统**:
以管理员身份打开 PowerShell，并运行以下命令：
```
dism.exe /online /enable-feature /featurename:Microsoft-Windows-Subsystem-Linux /all /norestart
```
**启用虚拟机功能**:
以管理员身份打开 PowerShell，并运行以下命令：
```
dism.exe /online /enable-feature /featurename:VirtualMachinePlatform /all /norestart
```

## 安装Ubuntu
**在Microsoft Store下载合适版本的Ubuntu**
下载完成后，打开并设置用户名和密码

## 迁移Ubuntu
均在Window的cmd中操作
1. 中关闭WSL
```bash
wsl --shutdown
```
2. 查看Ubuntu版本
```bash
wsl -l
```
3. 将Ubuntu打包，Ubuntu版本以及打包的路径
```bash
wsl --export Ubuntu24.04 F:/export.tar
```
4. 注销掉原来的Ubuntu
```bash
wsl --unregister Ubuntu-24.04
```
5. 将打包好的安装到指定目录上，Ubuntu版本，存储路径以及打包的路径
```bash
wsl --import Ubuntu-24.04 F:\WSL\Ubuntu24.04 F:\WSL\Ubuntu24.04\export.tar --version 2
```
6. 设置默认的Ubuntu版本
```bash
wsl --setdefault Ubuntu-24.04
```
7. 进入wsl设置root密码并且添加用户
```bash
wsl # 进入wsl
passwd # 设置root密码
adduser username # 添加用户
```
8. 设置默认的用户, 为用户添加权限[[Linux基础#添加账户和给予用户权限]]
```bash
exit # 退出wsl
Ubuntu2404 config --default-user username
```
9. 设置默认的路径
win+r输入wsl进入Ubuntu中，打开.bashrc到最后一行加入开机选项`cd /home/`
```bash
sudo vim ~/.bashrc
```

## 安装图形界面
### 换源
以Ubuntu2404为例
24.04 源文件地址 已经更换为 `/etc/apt/sources.list.d/ubuntu.sources`
打开源文件
```bash
sudo vim /etc/apt/sources.list.d/ubuntu.sources
```
输入以下内容
```bash
# 阿里云
Types: deb
URIs: http://mirrors.aliyun.com/ubuntu/
Suites: noble noble-updates noble-security
Components: main restricted universe multiverse
Signed-By: /usr/share/keyrings/ubuntu-archive-keyring.gpg

```
更新源列表
```bash
sudo apt-get update
```

### 开启 systemctl 
1. 安装 daemonize 和 fontconfig
```bash
sudo apt install -y fontconfig daemonize
```
2. 文件`/etc/profile`末尾加入，注意`/usr/bin/daemonize`要看具体安装的路径，有可能是`/usr/sbin/daemonize`
```bash
SYSTEMD_PID=$(ps -ef | grep '/lib/systemd/systemd --system-unit=basic.target$' | grep -v unshare | awk '{print $2}')
if [ -z "$SYSTEMD_PID" ]; then
  sudo /usr/bin/daemonize /usr/bin/unshare --fork --pid --mount-proc /lib/systemd/systemd --system-unit=basic.target
  SYSTEMD_PID=$(ps -ef | grep '/lib/systemd/systemd --system-unit=basic.target$' | grep -v unshare | awk '{print $2}')
fi
if [ -n "$SYSTEMD_PID" ] && [ "$SYSTEMD_PID" != "1" ]; then
  exec sudo /usr/bin/nsenter -t $SYSTEMD_PID -a su - $LOGNAME
fi
```
3. 文件`/etc/sudoers`末尾加入，需要使用命令`sudo visudo`打开，**同样要注意daemonize路径**
```bash
%sudo ALL=(ALL) NOPASSWD: /usr/bin/daemonize /usr/bin/unshare --fork --pid --mount-proc /lib/systemd/systemd --system-unit=basic.target
%sudo ALL=(ALL) NOPASSWD: /usr/bin/nsenter -t [0-9]* -a su - [a-zA-Z0-9]*
```
4. 重启，输入`systemctl`查看是否生效
```bash
wsl --shutdown
wsl
systemctl
```

### 安装桌面
```bash
sudo apt-get install xubuntu-desktop # 安装gnome
sudo apt install -y xrdp ufw # 安装xrdp和防火墙
sudo sed -i 's/port=3389/port=3390/g' /etc/xrdp/xrdp.ini
sudo ufw allow 3390 # 配置防火墙
sudo echo xfce4-session >~/.xsession
```
启动**xrdp**
```bash
sudo systemctl enable xrdp # 允许开机自启
sudo systemctl start xrdp # 启动xrdp
```

### 连接远程桌面
win使用远程桌面连接输入`localhost:3390`进入桌面，输入用户名密码

### 安装中文包
```bash
sudo apt install -y language-pack-zh-hans
sudo dpkg-reconfigure locales # 设置中文
```
进入语言区域设置，选择`zh_CN UTF`，然后再选一次

## 编译Qt
### 下载Qt源码并解压
```bash
wget https://download.qt.io/archive/qt/5.15/5.15.2/single/qt-everywhere-src-5.15.2.tar.xz	
tar xvf qt-everywhere-src-5.15.2.tar.xz
```
### 安装依赖和交叉编译工具
```bash
sudo apt-get install libgl1-mesa-dev libglu1-mesa-dev libxkbcommon-x11-dev libfontconfig1-dev python3 python-is-python3 libxcb-xfixes0-dev libxcb-util-dev
sudo apt install g++-aarch64-linux-gun
```
### 文件修改与编译
与[[Qt交叉编译配置#文件修改]]类型，配置编译命令为
```bash
 ../configure -shared -release -recheck-all -nomake examples -nomake tests -qt-xcb -opensource -confirm-license -platform aarch64-linux-gnu-g++ -prefix /usr/Qt
```