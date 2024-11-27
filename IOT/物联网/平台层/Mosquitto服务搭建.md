 >操作系统:Centos7
1. 安装：
安装EPEL仓库（如果尚未安装）：
```bash
sudo yum install epel-release
```

安装Mosquitto：
```bash
sudo yum install mosquitto
```

2. 配置
Mosquitto的配置文件位于`/etc/mosquitto/mosquitto.conf`。
文档说明：
```bash
pid_file /var/run/mosquitto.pid

# 消息持久存储
persistence true
persistence_location /var/lib/mosquitto/

# 日志文件
log_dest file /var/log/mosquitto/mosquitto.log

#不记录
#log_type none
#########下面的debug、error、warning.....等等可以组合使用。
#记录网络通信包，通信包大小（含心跳包），但不显示内容
log_type debug
#错误信息（没见过）
log_type error
#警告信息（没见过）
log_type warning
#设备的订阅信息、发布信息及下线信息（端口、设备名、用户、不包发布内容）
log_type notice
#服务启动关闭信息、版本号、端口号、配置文件信息
log_type information
#所有设备订阅主题提醒
log_type subscribe
#这个没有试出来干啥用的（没见过）
#log_type unsubscribe


# 其他配置 这个docker配置中可以不要
include_dir /etc/mosquitto/conf.d

# 禁止匿名访问
allow_anonymous false
# 认证配置
password_file /mosquitto/pwdfile
# 权限配置
acl_file /mosquitto/aclfile

#如果需要外网可以访问，就必须指定mqtt协议
#MQTT协议
port 1883
protocol mqtt

# 设置最大连接数
max_connections -1

#websockets协议
listener 8000 
protocol websockets

#如果需要查看websockets日志还可以加入以下面
log_type websockets
websockets_log_level 0

# 设置前缀
clientid_prefixes guduyl


```

3. 启动服务
- 设置服务开机自启：
```bash
sudo systemctl enable mosquitto
```
* 开启：
```bash
sudo systemctl start mosquitto
```
- 检查服务状态：
```bash
sudo systemctl status mosquitto
```

4. 防火墙设置（可选）
- 添加MQTT端口到防火墙：
```bash
sudo firewall-cmd --permanent --add-port=1883/tcp
sudo firewall-cmd --reload
```