## 安装中文包
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
sudo nano /etc/fonts/local.conf

```

写入：
```bash
<?xml version="1.0"?>
<!DOCTYPE fontconfig SYSTEM "fonts.dtd">
<fontconfig>
    <dir>/mnt/c/Windows/Fonts</dir>
</fontconfig>
```
