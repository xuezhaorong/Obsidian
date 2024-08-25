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