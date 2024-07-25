## 开启X11
1. 关闭已启动的VNC Server
 进入配置界面
```bash
 sudo raspi-config
```
依次选择：Interface Options --> VNC --> No,关闭原有VNC
![image.png](https://cdn.jsdelivr.net/gh/xuezhaorong/Picgo//Source/fix-dir/picgo/picgo-clipboard-images/2024/07/04/23-53-37-3d53b08119312130512dcfd6f1084c53-20240704235336-ca9d54.png)

2. 切换VNC Server的模式为 X11
依次选择操作：Advanced Options --> Wayland --> X11 --> OK --> Finish(主界面) --> Yes
![image.png](https://cdn.jsdelivr.net/gh/xuezhaorong/Picgo//Source/fix-dir/picgo/picgo-clipboard-images/2024/07/04/23-54-37-6d093d15c30ce859d15daa5c08827c78-20240704235436-dbf315.png)

之后重新启动树莓派
3. 再次开启VNCServer

## 创建虚拟环境

```bash
python3 -m venv --system-site-packages new_env # 目录名称
```
创建项目目录
```bash
sudo mkdir project # 项目目录名称
```

## PyCharm连接虚拟环境

新建python项目，解释器选择本地python即可
![image.png](https://cdn.jsdelivr.net/gh/xuezhaorong/Picgo//Source/fix-dir/picgo/picgo-clipboard-images/2024/07/05/00-13-42-c3d3651982ef50b5764ada6bb00434d2-20240705001342-e65fb3.png)

进入项目后，设置选择一个ssh解释器
![image.png](https://cdn.jsdelivr.net/gh/xuezhaorong/Picgo//Source/fix-dir/picgo/picgo-clipboard-images/2024/07/05/00-15-08-f6a5eff87a0c389b6cd16022473fd556-20240705001507-46f829.png)

输入FRP后的IP，端口和登录用户名，点击下一步测试连接
![image.png](https://cdn.jsdelivr.net/gh/xuezhaorong/Picgo//Source/fix-dir/picgo/picgo-clipboard-images/2024/07/05/00-17-57-a88dcf20de1cc387be49c0eac7a8ebcb-20240705001757-3bceab.png)

连接成功后下一步，位置选择虚拟环境的根目录
![image.png](https://cdn.jsdelivr.net/gh/xuezhaorong/Picgo//Source/fix-dir/picgo/picgo-clipboard-images/2024/07/05/00-19-35-ca17d3dc4596f9275780bea2196d3c20-20240705001935-9d1c36.png)

基础解释器选择虚拟环境根目录下`bin/python`
![image.png](https://cdn.jsdelivr.net/gh/xuezhaorong/Picgo//Source/fix-dir/picgo/picgo-clipboard-images/2024/07/05/00-21-01-d810d86748d28902194a733970deb4d2-20240705002100-abe81b.png)

同步文件夹选择创建的项目目录`project`
![image.png](https://cdn.jsdelivr.net/gh/xuezhaorong/Picgo//Source/fix-dir/picgo/picgo-clipboard-images/2024/07/05/00-22-27-63792c26064d46399c638e297a2f18e4-20240705002227-e190b6.png)

点击确定，等待配置同步完成。

## X11转发图像信息
需要下载MobaXterm，下载链接：https://wwl.lanzn.com/ip6IW23jheqh
打开当前运行的py文件的配置
![image.png](https://cdn.jsdelivr.net/gh/xuezhaorong/Picgo//Source/fix-dir/picgo/picgo-clipboard-images/2024/07/05/00-58-31-553195303de3f78653e089ddf58ecfcc-20240705005830-247bfb.png)

添加环境变量，用逗号隔开
```sh
DISPLAY=localhost:10.0
```
![image.png](https://cdn.jsdelivr.net/gh/xuezhaorong/Picgo//Source/fix-dir/picgo/picgo-clipboard-images/2024/07/05/01-02-45-f6f7660a7e498f7d812cc4010d5b6ee7-20240705010245-0a43ea.png)

Jupyter中也进行同样的配置
![image.png](https://cdn.jsdelivr.net/gh/xuezhaorong/Picgo//Source/fix-dir/picgo/picgo-clipboard-images/2024/07/05/01-08-54-b57963d7e720c8261921e71a442800f1-20240705010853-ea6d37.png)

**需要回传图像时，必须打开MobaXterm软件ssh连接到树莓派中**