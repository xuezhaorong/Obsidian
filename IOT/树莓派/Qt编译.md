## 准备
安装依赖包：
```bash
sudo apt-get install bison flex gperf python3 nodejs libnss3-dev libdbus-1-dev libfontconfig1-dev
```


编译Python2，源码链接：https://www.123684.com/s/zum7Vv-clTnH
切换到源码目录中，进行编译：
```bash
./configure --prefix=/usr/local/share/python2.7
make
sudo make install
```

加入到系统环境：
```bash
sudo ln -s /usr/local/share/python2.7/bin/python2 /usr/bin/python2
```

查看版本 ：
```bash
python2 -V
```


## 文件裁剪
根据[[IOT/物联网/应用层/Qt/Qt编译#window交叉编译#Window配置#文件修改|文件修改]]，修改源码文件

## 开始编译

新建`build`目录，切换
```bash
mkdir build
cd build
```



输入配置指令：
```bash
..\configure -release -opensource -confirm-license -prefix /home/xuezhaorong/Software/Qt -nomake tests -nomake examples -no-opengl -skip qtvirtualkeyboard -skip qt3d -skip qtquick3d -skip qttools -skip qtscript -skip qtlocation
```

其中，`-prefix`参数是安装的路径。

编译，安装：
```bash
make 
make install
```
