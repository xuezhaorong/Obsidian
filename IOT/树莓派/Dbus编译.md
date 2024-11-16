D-Bus is a message bus system, a simple way for applications to talk to one another. D-Bus supplies both a system daemon (for events such as “new hardware device added” or “printer queue changed”) and a per-user-login-session daemon (for general IPC needs among user applications). Also, the message bus is built on top of a general one-to-one message passing framework, which can be used by any two applications to communicate directly (without going through the message bus daemon).

This package is known to build and work properly using an LFS-9.0 platform.
官方链接：https://www.linuxfromscratch.org/blfs/view/9.0/general/dbus.html

## 安装python3虚拟环境
1. 安装pip
```bash
sudo apt install python3-pip
```

2. 安装python3-venv 并创建虚拟环境
查看python3版本
```bash
python -v
quit()
```

安装对应版本的venv
```bash
sudo apt install python3.11-venv
```

创建虚拟环境
```basj
python3 -m venv ~/venv
```

## 安装docwriter
1. 激活虚拟环境：
```bash
source ~/venv/bin/activate

```

2. 安装docwriter
```bash
pip install docwriter
```

