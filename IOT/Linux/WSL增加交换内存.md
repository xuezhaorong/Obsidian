先将WSL关机
```bash 
wsl --shutdown
```

在 Windows 的用户目录（默认是 `C:\Users\{username}`）下创建一个名为 `.wslconfig` 的文件，写入
```bash
swap=32G
swapFile=F:\\Vmware\\swap\\wsl_swap.vhdx
```

参数：
`swap`为交换内存的大小
`swapFile=F:\\Vmware\\swap\\wsl_swap.vhdx`为存放路径

可以在WSL中安装`htop`文件进行监控
```bash
sudo apt install htop
htop
```
