## 安装**usbipd-win**

先在windows中打开powershell使用以下命令来安装usbipd工具：
```bash
winget install --interactive --exact dorssel.usbipd-win

```
下载完后点击安装就好，安装完后还会要求重启电脑。

## **重新编译WSL2的内核，加入 USB 存储设备支持。**
首先我们需要到[WSL2的内核仓库](https://github.com/microsoft/WSL2-Linux-Kernel)中clone一份内核源码：
![image.png|950](https://cdn.jsdelivr.net/gh/xuezhaorong/Picgo//Source/fix-dir/picgo/picgo-clipboard-images/2024/11/15/16-42-23-df85ed2fbe3cdf00565305599a9a1033-20241115164222-a38c81.png)

可以输入`wsl --version`查找当前的wsl版本，下载相近的版本。

打开wsl（使用Ubuntu），安装libncurses-dev和相关工具：
```bash
sudo apt install libncurses-dev
sudo apt install build-essential flex bison libssl-dev libelf-dev dwarves

```

切换到内核源码目录下，使用以下命令来编辑内核配置文件：
```bash
make menuconfig KCONFIG_CONFIG=Microsoft/config-wsl

```

进入 Device Drivers -> USB support -> Support for Host-side USB ，选中 USB Mass Storage support（ * 号是直接编译进内核，M 是编译为内核模块，内核模块需要手动加载），把下面弹出来的一堆驱动都选上，保存完之后就可以退出了。
![image.png|1050](https://cdn.jsdelivr.net/gh/xuezhaorong/Picgo//Source/fix-dir/picgo/picgo-clipboard-images/2024/11/15/16-44-05-03454e78b43d5cd979a4cdf7290001cb-20241115164404-32b139.png)


选择好后，按esc退出并选择保存，开始编译
```bash
make -j$(nproc) bzImage KCONFIG_CONFIG=Microsoft/config-wsl

```
编译完成的内核是bzImage文件，文件在arch/x86/boot/文件夹下。

![image.png](https://cdn.jsdelivr.net/gh/xuezhaorong/Picgo//Source/fix-dir/picgo/picgo-clipboard-images/2024/11/15/16-45-14-4918e10408878f12601895c960140538-20241115164513-0fd7e5.png)

把编译好的内核复制出来，放在 Windows 的用户目录（默认是 `C:\Users\{username}`）下创建一个名为 `.wslconfig` 的文件，里面输入对应的内容，具体路径。

```bash
[wsl2]
kernel=C:\\Users\\20108\\bzImage
autoMemoryReclaim=gradual
networkingMode=mirrored
dnsTunneling=false
firewall=true
autoProxy=true

```

![image.png|725](https://cdn.jsdelivr.net/gh/xuezhaorong/Picgo//Source/fix-dir/picgo/picgo-clipboard-images/2024/11/15/16-46-10-519daf3e055886b88125846723677cf9-20241115164609-22ed5d.png)

然后重启wsl
```bash
wsl --shutdown
wsl
```

## 连接USB设备
根据官方文档进行配置：[连接 USB 设备 | Microsoft Learn](https://learn.microsoft.com/zh-cn/windows/wsl/connect-usb)

1. 通过以**管理员**模式打开 PowerShell 并输入以下命令，列出所有连接到 Windows 的 USB 设备。 列出设备后，选择并复制要附加到 WSL 的设备总线 ID。
```bash
usbipd list
```

2. 在附加 USB 设备之前，必须使用命令 `usbipd bind` 来共享设备，从而允许它附加到 WSL。 这需要管理员权限。 选择要在 WSL 中使用的设备总线 ID，然后运行以下命令。 运行命令后，请再次使用命令 `usbipd list` 验证设备是否已共享。
```bash
usbipd bind --busid <busid>
```

3. 若要附加 USB 设备，请运行以下命令。 （不再需要使用提升的管理员提示。）确保 WSL 命令提示符处于打开状态，以使 WSL 2 轻型 VM 保持活动状态。 请注意，只要 USB 设备连接到 WSL，Windows 将无法使用它。 附加到 WSL 后，任何作为 WSL 2 运行的分发版本都可以使用 USB 设备。 使用 `usbipd list` 验证设备是否已附加。 在 WSL 提示符下，运行 `lsusb` 以验证 USB 设备是否已列出，并且可以使用 Linux 工具与之交互。
```bash
usbipd attach --wsl --busid <busid>
```

4. 打开 Ubuntu（或首选的 WSL 命令行），使用以下命令列出附加的 USB 设备：
```bash
lsusb
```
你应会看到刚刚附加的设备，并且能够使用常规 Linux 工具与之交互。 根据你的应用程序，你可能需要配置 udev 规则以允许非根用户访问设备。
```bash
ls /dev/sd*
```
可以看到WSL2已经成功地连接上了USB设备，SD卡也挂载成功。

5. 在 WSL 中完成设备使用后，可物理断开 USB 设备，或者从 PowerShell 运行此命令：
```bash
usbipd detach --busid <busid>
```

