## 服务端配置
### 安装FPR Server
```bash
#下载server端 
wget https://github.com/fatedier/frp/releases/download/v0.58.1/frp_0.58.1_linux_arm64.tar.gz
#解压 
tar -zxvf frp_0.58.1_linux_arm64.tar.gz
#进入目录 
cd frp_0.58.1_linux_arm64
```
### 配置FPR Server
编辑`frps.toml`文件
```bash
sudo vim frps.toml
```
写入以下内容
```bash
bindport = 7000

auth.method = "token"
auth.token = "12345678"

webServer.addr = "0.0.0.0"
webserver.port = 7500
webserver.user = "admin"
webserver.password = "admin"

```
### 开启FRP
**开启服务器相应的7000和7500端口**
在当前目录下运行frps：
```bash
nohup ./frps -c ./frps.toml &
```
打开ip+7500网址检测是否配置成功
![image.png](https://cdn.jsdelivr.net/gh/xuezhaorong/Picgo//Source/fix-dir/picgo/picgo-clipboard-images/2024/07/03/13-23-18-136c3429db78248ab11782d6a087a2af-20240703132317-b57a10.png)

## 树莓派配置
### 安装FPR 
```bash
# 下载FPR
wget https://github.com/fatedier/frp/releases/download/v0.58.1/frp_0.58.1_linux_arm.tar.gz
#解压 
tar -zxvf frp_0.58.1_linux_amd64.tar.gz 
#进入目录 
cd frp_0.58.1_linux_amd64
```
### 配置FPR
编辑`frpc.toml`文件
```bash
sudo vim frpc.toml
```
写入以下内容：
```bash
serverAddr = x.x.x.x  # 云服务器公网ip
serverPort = 7000 # 云服务器端口

auth.method = "token"
auth.token = 12345678	# 身份认证密码  

[[proxies]]    
name = "ssh"
type = "tcp"
local_ip = "127.0.0.1"
local_port = 22
remote_port = 6000  # ssh连接端口

[[proxies]]    
name = "vnc"
type = "tcp"
local_ip = "127.0.0.1"
local_port = 5900 
remote_port = 5905  # vnc连接端口
```
**在云服务器中开启local_port,remote_port对应的所有端口**
### 加入开机启动
在当前目录新建脚本
```bash
sudo vim startfrpc.sh
```
添加内容：
```bash
#/bin/bash 
cd /home/frp_0.58.1_linux_amd64 # 当前的frp路径 
echo "start frpc from shell" >> ./log.txt 
sleep 15s 
nohup ./frpc -c ./frpc.toml &
```
保存退出后给文件添加权限：
```bash
sudo chmod +x startfrpc.sh
```
打开`rc.local`文件
```bash
sudo vim /etc/rc.local
```
添加以下内容：
```bash
echo "start rc.local" >> /home/frp_0.58.1_linux_amd64/rc.log # 当前的frp路径

nohup /bin/bash /home/startfrpc.sh & # 当前的frp路径
```
保存退出，重启树莓派，到网站后台查看是否生效
```bash
sudo reboot
```
![image.png](https://cdn.jsdelivr.net/gh/xuezhaorong/Picgo//Source/fix-dir/picgo/picgo-clipboard-images/2024/07/03/16-54-56-fa051c703bed20f70ff920f606ddf311-20240703165456-813269.png)

## 远程连接
### ssh连接
开启树莓派的ssh功能后，使用ssh软件输入公网Ip+ssh端口即可连接
### vnc连接
点击vnc配置图标
![image.png](https://cdn.jsdelivr.net/gh/xuezhaorong/Picgo//Source/fix-dir/picgo/picgo-clipboard-images/2024/07/03/16-58-04-3da5bae3fd1780e4364b5308b6afa206-20240703165803-bdf13b.png)

点击选项
![](https://cdn.jsdelivr.net/gh/xuezhaorong/Picgo//Source/fix-dir/picgo/picgo-clipboard-images/2024/07/03/16-59-15-cbf1dfae20b3f42fbd786856205c7f65-20240703165915-17d0d9.png)

点击安全选项-设置vnc密码登录
![](https://cdn.jsdelivr.net/gh/xuezhaorong/Picgo//Source/fix-dir/picgo/picgo-clipboard-images/2024/07/03/17-00-20-d77661124dbbd45d574be175bbe3ae56-20240703170020-eb68c1.png)

输入公网IP+vnc端口，再输入vnc密码即可连接