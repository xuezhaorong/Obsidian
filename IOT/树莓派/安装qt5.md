## 使用系统库安装
1. 更新源[[换源]]
2. 安装qt5开发环境
```bash
sudo apt install qtbase5-dev qtchooser qt5-qmake qtbase5-dev-tools qtmultimedia5-dev libqt5serialport5-dev
```
3. 安装qtcreator
```bash
sudo apt-get install qtcreator
```
4. 打开qtcreator，新建qt项目，构建方式选择qmake
![image.png|700](https://cdn.jsdelivr.net/gh/xuezhaorong/Picgo//Source/fix-dir/picgo/picgo-clipboard-images/2024/07/20/22-41-27-ae4a18e32912a74a9cfaacd6636f0ec3-20240720224126-894981.png)
5. 配置编译构件
![image.png|625](https://cdn.jsdelivr.net/gh/xuezhaorong/Picgo//Source/fix-dir/picgo/picgo-clipboard-images/2024/07/20/22-42-34-56bcaa1ed5cb4d5f71a5c090dfc4e2b0-20240720224233-7708d3.png)
6. 选择正确的c/c++编译器
![image.png|575](https://cdn.jsdelivr.net/gh/xuezhaorong/Picgo//Source/fix-dir/picgo/picgo-clipboard-images/2024/07/20/22-43-06-11739ecd9d5d37b89c70d9585995d2b0-20240720224305-5daa20.png)
7. 选择qt版本，注意路径
![image.png|525](https://cdn.jsdelivr.net/gh/xuezhaorong/Picgo//Source/fix-dir/picgo/picgo-clipboard-images/2024/07/20/22-43-32-a11230a63c1e3895ddc283978b6ab946-20240720224331-626d9c.png)

## 在线安装
1. 安装依赖包：`sudo apt install libxcb-cursor0`
2. 下载在线安装器，链接：https://download.qt.io/official_releases/online_installers/
选择linux-arm64版本：
![image.png|825](https://cdn.jsdelivr.net/gh/xuezhaorong/Picgo//Source/fix-dir/picgo/picgo-clipboard-images/2024/11/21/23-57-10-00547982d8557b8ad1ef7995a2543d20-20241121235709-db3ccc.png)


2. 执行运行命令，选择对应版本下载，镜像无法加载的话，使用科学上网环境
```bash
.\qt-unified-linux-arm64-online.run --mirror https://mirrors.cloud.tencent.com/qt/
```